先看个例子：
```c++
#include <iostream>
using namespace std;

class BasePoint{
public:
    int _b;	
};

class Point : public Basepoint {
public:
    int _v;	
    static int _s;
};

int main()
{
    Point a;
    Point *pa = &a;
    
    a._v = 1;
    pa->_v = 2;
    
    a._s = 1;
    pa->_s = 2;

    Point::_s = 3;

    a._b = 1;
    pa->_b = 2;    

    count << &Point::_s << endl;
    return 0;
}
```

请问以上的数据访问都有啥区别？
- 对于static 的成员的访问，通过 指针或者通过实例来访问，都是一样的，因为他们的存储空间都在实例之外
- 访问 static 成员的地址，它应该在实例的地址范围之外
- 对于非static 成员的访问，如果是通过一个virtual base class 继承来的，那么通过 指针进行访问要比通过实例访问多绕一层。
- 对于非static 成员，除了上面一种情况之外，不管是来自于基类的，或者是通过指针访问，或者通过实例访问的，效率都一样，都是通过 对指针进行一个偏移量来访问的。


