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
    // 
protected:
    float mumble;
};

```

那么这个继承关系是这样的：

Point2d    Vertex
   |         |
Point3d      | 
   |         |
   V e r t e x  

由于Vertex3d的多重继承中，Point3d是写在左边的，Vertex是写在右边的，那么

对于如下的转换：
```C++
/*
---------------
|             | |   | |- Point2
|             | |   |--- Point3
|             | | |----- Vertex  
|             | |------- Vertex3d
--------------- 
*/
Vertex3d v3d;
Vertex   *pv;
Point2d  *p2d;
Point3d  *p3d;

pv = &v3d;
p2d = &v3d;
p3d = &v3d;
```

```