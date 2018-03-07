---
layout: post
title:  "Apex Testing Best Practice Guide"
date:   2017-03-11 13:14:30 +0800
categories: jekyll update
---

    This article is writed by my collegue "Brian" and "Thomas"

## Test Class Naming Conventions

 Salesforce has no definable directory structure so its helpful to keep classes grouped together in an alphabetical listing under the *classes* folder. To keep tests next to their subjects the word "Test" should be appended to the end of the class name that is being tested:
 * **OpportunityLineItemsTest** (for the "OpportunityLineItems" class)

## Avoid the *@isTest(seeAllData=true)* annotation!

Using this annotation greatly slows down test times as well as making the tests dependent on existing data in the organization/salesforce instance.  By default test classes/methods do not have access to pre-existing data in the organization, such as standard objects, custom objects, and custom settings data.  This is good practice and forces you to create your test data in *isolation*.  The @isTest(seeAllData=true) annotation overrides this "safety" feature and should only be used in very unique cases when you cannot create your own test data.

## Follow the Test Pyramid principles

![test pyramide](/images/testpyramide_with_dimensions_english.jpg "Test Pyramid")

### Unit Testing

Salesforce "unit" tests are not true unit tests and are more at the integration/service level as they test units in aggregation.  To allow for true unit testing in Apex you must organize your code in layers (see more info here **NEED TO ADD LINK TO GENERAL STYLE GUIDE**).  Here is where the bulk of tests should reside and happy path, negative path, and edge case testing should occur.

Using dependency injection techniques allows you to mock the dependencies between layers in your tests.  A common example is mocking the database query results in your Selector layer when unit testing the Domain layer.  Proper use of dependency injection and mocking allows for tests to focus on the unit they are concerned with and also allows them to ignore other parts of the codebase.  

#### Using @testSetup annotation
Test isolation is also important.  Tests should not be aware of each other or share any state as this can lead to false positives.  This can be achieved using the **@testSetup** annotation for setting up and tearing down dependancies and variables that would otherwise be used several times.  More info can be found [here](https://www.salesforce.com/us/developer/docs/apexcode/Content/apex_testing_testsetup_using.htm)

```java
@testSetup static void setup() {
    // Create common test accounts
    List<Account> testAccts = new List<Account>();
    for(Integer i=0;i<2;i++) {
        testAccts.add(new Account(Name = 'TestAcct'+i));
    }
    insert testAccts;
}
```

### Integration/Service Testing
Salesforce's "unit" testing is actually more of an integration/service level test as it relies on test data inserted into the database and often the test runs over many units of code.  In an ideal testing scenario these tests should be generally a happy path test to make sure the units works in aggregation.  There is good documentation on Salesforce "unit" tests [here](http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_testing_unit_tests.htm).

### End-to-end/Functional UI Testing
This is the level where browser testing takes place.  Typically some automated tool like Selenium is used here.  This is more to cover end-to-end user journeys and because these tests are expensive to maintain and are long running it is best to try to limit the number of tests at the top of the pyramid.

## Using assertions

It is very important to use assert statements in your tests.  Salesforce requires 75% overall code coverage but does not take into account the use of assert statements.  So it is possible to have a unit test without an assert and Salesforce will consider the code tested/covered.  **A test without assert statements is NOT a test!**

Salesforce provides a few different assertion methods.  There is an optional parameter that allows for an explanation of the test and why it failed.  This parameter should be used in all asserts.

```java
System.assertNotEquals(originalId, newId, 'Original ID and new ID should differ');
```

## Mocking with ApexMocks Framework

ApexMocks is a [Mockito](http://mockito.org/)-like framework that assists in using mocks within your unit tests.

### Using dependency injection to make classes unit testable

Apex has a special **@testVisible** annotation which allows for dependency injection

```java
public with sharing class Opportunities implements IOpportunities {
    // Use interface to define selector dependency so we can either pass in real or mock implementation
    private IOpportunitiesSelector oppsSelector;

    // Constructor used for real instance of Opportunities domain
    public Opportunities() {
        this.oppsSelector = new OpportunitiesSelector();
    }

    // Test constructor that can only be used by Apex unit tests
    @testVisible private Opportunities(IOpportunitiesSelector oppsSelector) {
        this.oppsSelector = oppsSelector;
    }

...

}
```

### Defining Mocks

#### ApexMocks generator
ApexMocks has a [mock generator](https://code4cloud.wordpress.com/2014/06/27/new-improved-apex-mocks-generator/) that will produce your Mock classes for you.  This helps simplify the process of defining the mock class.  There are a few different components needed to generate the mock class as discussed below.

##### Example interfaces.properties file
Interfaces are specified via a properties file using key-value type syntax &lt;interface name&gt;=&lt;mock class name&gt;
```
IOpportunitiesSelectorInterface=MockOpportunitiesSelector
```

##### Example Interface file
All classes that you want to mock require an interface that can be used as a blueprint for generating the mock class
```java
public interface IOpportunitiesSelector {
    Map<Id, OpportunityLineItem> queryOpps(Set<Id> opportunityIds);
}
```

##### Example generator command to be run from Mac Terminal
```
$ java -jar apex-mocks-generator-3.1.2.jar src/classes interfaces.properties Mocks src/classes
```
Generator Parameters needed:
1. sourcePath – where your apex source files reside
  * “src/classes”
2. interfaces – the properties file containing the interfaces you want to mock
  * “interfaces.properties”
3. mocksClassName – the class name for the class which will contain all your mock classes
  * “Mocks”
4. targetPath – where you want the generated mocks class to reside
  * "src/classes"

##### Example generated Mocks.cls file
An example of the generator output.  The MockOpportunitiesSelector class would be used in a unit test to mock an OpportunitiesSelector  class dependency in the Opportunities domain class (the class that is being tested).  Using the mock we can define behavior that we want to test for and not concern ourselves with the real implementation.
```java
@isTest
public class Mocks
{
    public class MockOpportunitiesSelector implements IOpportunityLineItemsSelector
    {
        private fflib_ApexMocks mocks;

        public MockOpportunitiesSelector(fflib_ApexMocks mocks)
        {
            this.mocks = mocks;
        }

        public Map<Id, Opportunity> queryOpps(Set<Id> opportunityIds)
        {
            return (Map<Id, Opportunity>) mocks.mockNonVoidMethod(this, 'queryOpps', new List<Object> {opportunityIds});
        }
    }
}
```

### Instantiating Mocks
Continuing the example of unit testing the Opportunities domain class we would create a mock instance as follows

```java
@isTest
private class OpportunitiesTest {
    @isTest static void testOpportunitiesMethod() {
        // create mocks instance using ApexMocks framework
        fflib_ApexMocks mocks = new fflib_ApexMocks();
        // use the generic interface to instantiate selector mock
        IOpportunitiesSelector mockOpportunitiesSelector = new Mocks.MockOpportunitiesSelector(mocks);

        ...
    }
}
```

### Stubbing with ApexMocks
ApexMocks uses stubbing to define behavior in our mocks which allows for proper separation of concerns in tests.

```java
@isTest static void testOpportunitiesMethod() {
    /...

    // ... test setup above ...

    mocks.startStubbing();
    mocks.when(mockOpportunitiesSelector.queryOpps(oppIds)).thenReturn(testOppsMap);
    mocks.stopStubbing();

    // inject our stub into the Opportunities constructor
    Opportunities oppsDomain = new Opportunities(mockOpportunitiesSelector);

    // Call Domain method that is being tested which will execute behavior defined in our mock rather than using a real instance
    Map<Id, Opportunities> returnedOppLineItemsMap = oppsDomain.getOppsByOppIds(oppIds);

    /...
}
```

#### ApexMocks Resources
 * [ApexMocks library](https://github.com/financialforcedev/fflib-apex-mocks) on github

 * [Brief write-up](https://code4cloud.wordpress.com/2014/05/06/apexmocks-framework-tutorial/) by ApexMocks author

 * Decent [YouTube presentation](https://www.youtube.com/watch?v=HBulci-5xIs) on ApexMocks
