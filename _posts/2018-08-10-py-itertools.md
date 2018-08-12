---
layout: post
title:  "Python内建库-itertools"
date:   2018-08-10 01:00:00 +0800
categories: Python
---
itertools 是python中一个内置的库，可以用来构造一些复杂的迭代器，方便对数据结构进行操作。下面就简单介绍下期中包含的函数。

> 下面的函数签名中如果有[arg]，则代表该arg为可选值

## 首先是三个无限循环迭代器

1. `count(start,[step])`，该返回无限长度的等间隔序列，在使用时一定要有一个中断机制避免死循环：

```
from itertools import *
for i in count(0,5):
     print(i)
     if i>20:
         break
         
0
5
10
15
20
25
```

2.  `cycle(iterable)`, 对给定的迭代器进行无限循环，如果迭代器内容较多，该操作可能会消耗大量内存。
```
count =0
for i in cycle([1,'A',2,'B']):
    count+=1
    print(i)
    if (count>10):
        break
        
1
A
2
B
1
A
2
B
1
A
2
```

3. `repeat(object[, times])`, 如果未指定times，则一直返回object；否则返回`times`次。常和`zip`或`map`一起使用来，用来构成不变的部分。

```
list(map(pow, range(10), repeat(2)))

[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

## 有限迭代器

1. `accumulate(iterable[, func])`,返回一个与输入等长的迭代器，如果不提供func，则进行累加操作，否则用func代替加法操作

```
data = [3, 4, 6, 2, 1, 9, 0, 7, 5, 8]
list(accumulate(data, operator.mul))

[3, 12, 72, 144, 144, 1296, 0, 0, 0, 0]

list(accumulate(data, max)) 

[3, 4, 6, 6, 6, 9, 9, 9, 9, 9]

```

2. `chain(*iterables)`, `from_iterable(iterables)`：这两个方法功能相同，都是用来将多个迭代器连接起来。

```
for i in chain('asd', [1,2,3]):
    print(i)
    
a
s
d
1
2
3
```

3. `combinations(iterable, r)`： 返回一个包含输入的各种长度为`r`的组合的迭代器。通过下面例子能更好的解释：
```
list(combinations([1,2,3,4],2))
[(1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]

list(combinations([1,2,3,4],3))
[(1, 2, 3), (1, 2, 4), (1, 3, 4), (2, 3, 4)]
```

4. `compress(data, selectors)`: 和方法名体现的不太一致，方法更像一个filter,通过检查selector序列中对应值是否为真来返回data中对应的值。当data和selector长度不同时，以较短的为准。

```
for i in compress(list(range(100)),[0,0,1,1,0,2,1,0,10]):
    print(i)
    
2
3
5
6
8
```

5. `dropwhile(predicate, iterable)`,该函数使用`predicate`函数判断iteratble中的对象是否为真，并将为真的对象删除，当找到第一个不为真的对象后则将其后（包含该对象本身）所有元素返回。

```
list(dropwhile(bool,[0,2,3,4,5]))
[0, 2, 3, 4, 5]
list(dropwhile(bool,[0,0,3,4,5]))
[0, 0, 3, 4, 5]
list(dropwhile(bool,[1,2,3,0,0,3,4,5]))
[0, 0, 3, 4, 5]
```

6. `filterfalse(predicate, iterable)`, 根据`predicate` 的结果将测试值为False的对象丢弃，和`dropwhile`不同，`filterfalse`会对每个元素进行测试。

```
list(filterfalse(greater_than_five, [6, 7, 8, 9, 1, 2, 3, 10]))
[6, 8, 2, 10]
```

7. `groupby(iterable, key=None)`, 将`iterable`按照key来分组，分组的方式是每当key值变化就产生一个新的分组，这就要求输入的`iterable`必须是排过序的迭代器。如果没有提供`key`方法，则将对象本身作为key。

```
vehicles = [('Ford', 'Taurus'), ('Dodge', 'Durango'),
            ('Chevrolet', 'Cobalt'), ('Ford', 'F150'),
            ('Dodge', 'Charger'), ('Ford', 'GT')]
 
sorted_vehicles = sorted(vehicles)
 
for key, group in groupby(sorted_vehicles, lambda make: make[0]):
    for make, model in group:
        print('{model} is made by {make}'.format(model=model,
                                                 make=make))
    print ("**** END OF GROUP ***")

Cobalt is made by Chevrolet
**** END OF GROUP ***
Charger is made by Dodge
Durango is made by Dodge
**** END OF GROUP ***
F150 is made by Ford
GT is made by Ford
Taurus is made by Ford
**** END OF GROUP ***
```

8. `islice(iterable, start, stop[, step])`, 对迭代器进行切片操作，`step`如果不指定，则默认为1.

```
# islice 是的参数规则比较复杂，又不支持kwargs，下面是一些使用示例
# islice('ABCDEFG', 2) --> A B
# islice('ABCDEFG', 2, 4) --> C D
# islice('ABCDEFG', 2, None) --> C D E F G
# islice('ABCDEFG', 0, None, 2) --> A C E G
for i in islice('123456', 4):
    print(i)  
1
2
3
4
```

9. `starmap(function, iterable)`, 和python的`map`类似，但是`iterable`中每个元素都包含function所需的所有参数,下面的例子分别说明map和starmap的用法

```
list(map(pow,[1,2,3],repeat(3,3)))
[1, 8, 27]

list(starmap(pow, [(1,3),(2,3),(3,3)]))
[1, 8, 27]
```

10. `takewhile(predicate, iterable)`, 和前面的`dropwhile`类似，只是取舍逻辑想反

11. `tee(iterable, n=2)` 将输入`iterable`复制（并不是简单地复制）成多个`iterable`。不过需要注意的是，当iterable通过tee方法进行复制后，原iterable不应再进行任何操作，否则会产生意想不到的效果：

```
i = iter([1,2,3])
i1,i2 = tee(i,2)
list(i1)
[1, 2, 3]

list(i2)
[1, 2, 3]
```

12. `zip_longest(*iterables, fillvalue=None)`, 顾名思义，按较长的iterable来组合两个迭代器，空值用`fillvalue`代替，其默认值为None：
```
for a,b in zip([1,2,3,4],['a','b','c']):
    print (a,b)
    
1 a
2 b
3 c

for a,b in zip_longest([1,2,3,4],['a','b','c']):
    print (a,b)
    
1 a
2 b
3 c
4 None
```

13. `product(*iterables, repeat=1)`, 从输入的每个iterables中选取一个进行“组合”（即不关注顺序）, 其`repeat`参数代表将前面的iterable重复多少次，默认为1.

```
for i in product([1,2], repeat=2):
    print(i)
    
(1, 1)
(1, 2)
(2, 1)
(2, 2)

for i in product([1,2],[1,2]]):
    print(i)

(1, 1)
(1, 2)
(2, 1)
(2, 2)
```

14. `permutations(iterable, r=None)`，从iterable中选取`r`个元素进行排列（关注顺序），`r`默认为`iterable`的长度.

```
for item in permutations(['a','b','c'],2):     
    print(''.join(item))
ab
ac
ba
bc
ca
cb
```

15. `combinations(iterable, r)`, 和`permutations`方法相似，只是这里编程了组合（不关注元素顺序）：
```
for item in combinations(['a','b','c'],2):     
    print(''.join(item))
ab
ac
bc
```

16. `combinations_with_replacement(iterable, r)`, 与上面`combinations`方法相似，只是这里可以可以重复选取同一个元素：

```
for item in combinations_with_replacement(['a','b','c'],2):     
    print(''.join(item))
aa
ab
ac
bb
bc
cc
```