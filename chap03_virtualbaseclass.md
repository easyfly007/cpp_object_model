** virtual base class **

我们很少用到 virtual base class，也就是虚拟的基类。
有什么用呢？

举个例子，我们有基类A，B和C继承于A，而D多重继承于B和C。
如果A 中有成员_a, 那么在一个实例化的D d 中就会有两份_a。

如果我们的B 和C是通过virtual 继承于A的，那么在d 中只会保留一份_a。
```c++
#include <iostream>
using namespace std;
class A{};
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

来分析一下，
为什么一个空的a 还是有1 byte 的大小呢？

- 一个类，哪怕没有任何的成员，也占有一个char 的大小，你去看一下 A a1， A a2，他们的地址，是不一样的。
这个char 大小的内存是用来区分不同的实例的。
当然如果这个类有了某个成员，那么这个char 也就没有了，不需要了。

- 对于b，如果是virtual 继承于一个基类，会有一个额外的负担，这个负担，或者是要有一个指向virtual base class 的指针，或者一个指向相关的表格的指针（不同的对象模型有不同的处理）

- alignment 的padding 处理，也就是，为了实现效率的一些考虑，会对内存进行补全，使得实际的存储空间为某个数值（1，2，4，8，16， 等等）的整数倍。



再看下面一个例子：

```c++
#include <iostream>
using namespace std;

class A{
public:
    char _a;
};
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

我跑出来的结果是：1，16，16，24，1
如果_a 是int 类型，那么是4，16，16，24，4


分析一下：
a 的大小是1，很好理解，一个char
b 的大小是16，由一个来自于基类a的1，加上virtual base 的一个指针什么，8，加在一起就是9，padding 到16
c 同 b
对于 d，大小是 一个 b 和c 的大小相加，9+9 = 18， 减去 重复的a，得到17，padding 到24
对于 e，大小就是和a 相同了


```c++
#include <iostream>
using namespace std;

const int arrsize = 4；
class A{
public:
    char[arrsize] _a;
};
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
这里是多少？ 4， 16， 16， 24， 4
如果arrsize 是8，那么，8，16，16，24，8



如果B 和C 对于A 的继承不是virtual的呢？如下

```c++
#include <iostream>
using namespace std;

const int arrsize = 8；
class A{
public:
    char[arrsize] _a;
};
class B: public A{};
class C: public A{};
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
答案是 8，8，8，16，8

