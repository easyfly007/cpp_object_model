**struct 和class**

如果你要在C++ 的代码中利用 C 的struct，
你可以 用一个class 继承于一个struct，但是更好的方法是 用组合而不是继承，也就是一个Class 当中有一个struct 的数据成员。
而对于struct 的用途，比较好的是用它来对数据进行封装，但是这种封装所带来的内存空间布局的一致性，只有在 被class 用于组合的时候才存在，如果是继承，那么继承的class 不一定有这种数据布局的一致性。

举个例子，一个class 中的每一块 public 的数据，或者private 的数据，或者protected的数据，在这些数据块的内部，他们是一定的次序排列的，但是块与块之间，并不能保证一致的先后顺序。


比如一个 C 中的小技巧，当然这很不值得提倡，
struc A 的最后一个数据成员是一个char[1]，
我们如果强制 malloc 获取一块大小为sizeof(struct A) + strlen(str) + 1 的空间并且转换成一个 struct A　的指针，
那么最后一个成员其实带有的空间大小是char[strlen(str)+1] 了。

但是如果另外有一个class　继承了这个struct A，就不能保证该数据成员是在class的最后一个。

--------


除了　指针可以进行动态绑定之外，引用也是可以的。
比如
```c++
class B:public A{};
B b;
A &a = b;
a.dosomething();
```
这里a.dosomething() 就会调用B的dosomething()


