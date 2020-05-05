1. MRO
Method Resolution Order,即方法解析顺序，是用来处理Python中的二义性问题的算法。
2. 二义性
Python因为支持多继承，而多继承的编程语言往往存在二义性问题。
3. 二义性问题
    魔鬼三角继承：有两个基类A和B，A和B中都定义了方法f(),C类继承了A类和B类，那么调用C类的f()时出现不确定情况。
    恐怖菱形继承：有一个基类A，定义了方法f()，B类和C类继承了A类（的f()方法），D类继承了B和C类，那么出现一个问题，D不知道应该继承B的f()方法还是C的f()方法。
4. 深度遍历优先算法
指代一种搜索算法，其算法核心是尽可能先对纵深方向进行搜索，当一条分支搜索到叶子结点后，终止，从左往右找到下一条分支，继续开始纵深方向的搜索，直至搜索到最后一条分支的叶子节点，停止搜索。
5. 广度遍历优先算法
指代一种搜索算法，其算法核心是针对树形结构的每一层，从根结点出发，按照一层一层的顺序，每层从左至右的顺序搜索，当搜索到某一层最后一个非叶子结点，结束该层搜索，进入下一层，继续搜索，直至搜索到最后一个叶子节点，停止搜索。
6. 经典类
一种没有继承的类，所有的类型都是type类型，如果经典类作为父类，子类调用父类构造函数会报错。一种没有继承的类，所有的类型都是type类型，如果经典类作为父类，子类调用父类构造函数会报错。
7. 新式类
每一个类都继承自一个基类，默认继承自object，子类可以调用基类的构造函数，所有类都有一个公共的祖先类object。每一个类都继承自一个基类，默认继承自object，子类可以调用基类的构造函数，所有类都有一个公共的祖先类object。

```
单继承示例代码
class A():
    def __init__(self):
        self.name = "A.name"
    def fun(self):
        print("A:fun")
    
class B(A):
    def __init__(self):
        self.name = "B.name"
        A.__init__(self)
        super(B,self).__init__()
    def fun(self):
        print("B:fun")
        A.fun(self)
        super(B,self).fun()
        print("B:fun")
        
```

```
多继承示例代码
class A(object):
	def __init__(self):
		print('进入a')
		print('离开a')

class B(object):
	def __init__(self):
		print('进入b')
		print('离开b')
		
class C(A):
	def __init__(self):
		print('进入c')
		super(C,self).__init__()
		print('离开c')
		
class D(A):
	def __init__(self):
		print('进入d')
		super(D,self).__init__()
		print('离开d')
		
class E(B,C):
	def __init__(self):
		print('进入e')
		B.__init__(self)
		C.__init__(self)
		print('离开e')

class F(E,D):
	def __init__(self):
		print('进入f')
		E.__init__(self)
		D.__init__(self)
		print('离开f')

f = F()
print(F.__mro__)

```


