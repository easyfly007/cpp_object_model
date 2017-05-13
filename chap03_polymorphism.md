继承关系的转换，要看背后实际的内存中分布

比如我们有如下的继承关系，还是多重继承的呢（先不考虑多态）

```C++
class Point2d{
public:
    // ... 
protected:
    float _x, _y;
};

class Point3d{
public:
    //
protected:
    float _z;
};

class Vertex{
public:
    // 
protected:
    Vertex *next;
};

class Vertex3d:public Point3d, public Vertex{
public:
  
  ```C++  // 
protected:
    float mumble;
};

```
```

那么这个继承关系是这样的：

```C++
Point2d    Vertex
   |         |
Point3d      | 
   |         |
   V e r t e x  
```

由于Vertex3d的多重继承中，Point3d是写在左边的，Vertex是写在右边的，那么

对于如下的转换：
```C++
/*
---------------
|             | |   | |- Point2
|   MEMORY    | |   |--- Point3
|             | | |----- Vertex  
|             | |------- Vertex3d
--------------- 
*/
Vertex3d v3d;
Vertex   *pv;
Point2d  *p2d;
Point3d  *p3d;

p   MEMORYv& d = &v3d;
p3d = &v3d;
```

pv = &v3d 需要做一个转换，把 pv 转到真正的 Vertex 那部分内容开始的内存位置中去。
而 p2d, p3d 都不需要进行内存位置的计算，因为他们都是从整个 Vertex3d 的起始位置开始的。

如果我们 进行 pv = pv3d 的转换，那么是不是 pv 应该指向 pv3d location + sizeof(Point3d) ?

不一定，极端情况下 pv3d = 0，就不对。所以要有一个额外的判断，如果pv3d = 0， 那么pv 也等于0，否则才等于 pv3d 再偏移 Point3d 的大小

而 如果是作引用，就不用这个判断，因为 0 无法被引用。


------

以上的例子比较简单，如果中间有虚拟继承，就很麻烦了。
虚拟继承的例子有：

```C++
/*
          i o s
          |   |    
    istream   ostream
         |     |
     i o s t r e a m
*/

class ios{};
class istream : public virtual ios {};
class ostream : public virtual ios {};
class iostream : public istream, public ostream {};

```

如何保证在 iostream 中通过 istream 继承来的 iso 和通过 ostream 继承来的 iso 是同一份？

