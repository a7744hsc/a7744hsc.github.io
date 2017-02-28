Lighting guidance 总结
========================

Lighting 是salesforce平台力推的新一代前端开发语言，基于开源的aura框架。

1. lightning 框架的内置组件主要有三个前缀， aura，ui，force
2. component 可以使用aura:id关键字设置局部id 例如 <ui:button aura:id="button1" label = "xx">  可以再js中通过cmp.find(id)得到对应组件
3. 每一个component 都有一个global id， 这个id在不同生命周期中可变，可以通过cmp.getGclobalId() 获得
4. 可以用aura.attribute 给component 设置变量， 变量可以使用<!v.vriableName>在component中获取，或使用cmp.get("v.variableName")在js中获取
5. component可以进行组合，在调用component时可以对其设置变量的值 <c:example varableinexample = "xxx">
6. 








Communicating with Events
-------------
1. lightning framework 使用事件驱动编程，你可以编写事件处理器来处理发生的事件
2. 事件在js controller中被触发，事件可以包含参数，参数可以在触发事件时设置
3. event使用aura:event定义，是一个.evt类型的文件。
4. event可以用两种类型，component和application。
5. componet event从component中触发，component event 可以被自身和他上级的的component处理。
6. application event 遵循传统的发布-订阅机制。它从component中触发，任何提供了对应 event handler的component都会受到通知。
7. controller方法的三个参数 
    * cmp: controller 对应的component
    * even: trigger 事件的event
    * helper: component对应的helper
8. raw js in componen will not work because lightning will ignore it.
9. cmp.get cmp.set 可以读写attributes  attributes 的名字为 v.attributesName
10. use <aura:registerEvent > 注册当前component可能trigger 一个event。 use <aura:handler > 注册event的handler。 他们的name属性需要一致
11. 