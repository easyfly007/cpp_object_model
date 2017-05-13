###**C++ 的对象模型** 

本文内容来自于《深度探索C++对象模型》一书第一章

每一个实例化的C++ 类对象，它的对象模型是这样的：

 1. static 的数据类成员，放置于每一个实例对象中
 2. static 的数据成员，放置于属于类的范围中，所有实例对象共享一份
 3. static 和非static 的成员函数（除了virtual 函数之外），放置于属于类的范围中，所有实例对象共享一份
 4. 对于virtual 成员函数




    1. 每个class 产生出一堆指向所有virtual 成员函数的指针，放置于一个表格中，这个表格叫做vietual table （vtb）
比如，有两个 function a 和 function b，都是virtual 的，那么产生一个有两个元素的表格
    2. 在每个class 的实例中，有一个特殊的指针，这个指针指向对应的 virtual table，称之为 vptr。
vptr 的设定和重置，都有每一个class 的construct，destructor，copy assignment 等自动完成。
而每一个class 的type_info object （用来支持 runtime type identification， RTTI）也是通过virtual table 被指出来。

举个例子，比如A 是基类，B 继承了A，C继承了B
A里面有virtual function function1， virtual function2，virtual function3
B 里面重定义了function function1，virtual function2
C 里面重定义了function function1
这里的function1，function2，function3 都是public 的。

 - 对于Class A，它有一个virtual table， 里面是virtual function 的指针，指向
    - A：function1
    - A：function2
    - A：function3
 - 对于Class B，它有一个vietual table，里面是virtual function
   的指针，指向
    - B：function1
    - B：function2
    - A：function3
 - 对于class C，它有一个virtual table，里面是virtual functiton 的指针，指向
   - C：function1
   - B：function2
   - A：function3

如果我们实例化了一个C c1，那么c1的vptr 指向的是Class C 的vtb
如果我们实例化了一个B b1，那么b1的vptr 指向的是Class B 的vtb
如果我们实例化了一个A b1，那么a1的vptr 指向的是Class A 的vtb

在这当中，如果我们用一个A 的指针指向了一个 C 的实例，那么背后绑定的vptr是不变的，还是Class C 的vtb
