** virtual base class **

我们很少用到 virtual base class，也就是虚拟的基类。
有什么用呢？

举个例子，我们有基类A，B和C继承于A，而D多重继承于B和C。
如果A 中有成员_a, 那么在一个实例化的D d 中就会有两份_a。

如果我们的B 和C是通过virtual 继承于A的，那么在d 中只会保留一份_a。
```c++
#include <iostream>
using namespace std;

class B: public virtual A{};
class C: public virtual A{};
class D: public B,C{};
class E: public A{};
int main()
{
    A a;
    B b;
    C c;
    D d;
    E e;
    cout << " class A size is: " << sizeof(a) << endl;
    cout << " class B size is: " << sizeof(b) << endl;
    cout << " class C size is: " << sizeof(c) << endl;
    cout << " class D size is: " << sizeof(d) << endl;
    cout << " class E size is: " << sizeof(e) << endl;
    
    return 0;
}
```

这里 abcde 的大小是多少呢？
我在ubuntu 上g++ 上面变编译出来的结果是1，8，8，16，1
