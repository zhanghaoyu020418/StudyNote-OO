![image-20210528155501798](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528155501798.png)



# 从面向过程到面向对象之类的引入

> 阅读完这个版块你可以了解以下内容
>
> 1）面向过程和面向对象的区别
>
> 2）类的定义和使用
>
> 3）类的大小计算
>
> 4）this指针的存放位置和this是否可以为空





## 对面向过程和面向对象的认识

> 面向过程注重的是过程，也就是分析问题的步骤，靠的是变量和函数调用，其中变量和函数是分离开来的
>
> 面向对象注重的是对象，将一件事情分成了不同的对象，靠的是对象之间的交互，其中对象中结合了数据成员和函数（有点像离散数学中的[代数系统]([代数系统_百度百科 (baidu.com)](https://baike.baidu.com/item/代数系统/345758?fr=aladdin))）

**面向过程的语言最经典的就是C语言，面向对象的有很多高级语言像C++,Java,go….**简单的来说对象就是将面向过程中的变量和函数整合到一个体系当中，那就是对象。另外面向对象有四大特点：**抽象，封装，继承，多态**。而对象的抽象就是类，类也可以实例化（定义）出对象，所以就要先谈谈类的定义。

## 类的引入

在C++中，定义类可以用两个关键字`class`和`struct`，这两个都可以定义一个类，但有所区别（在下文我会汇总问题统一回答。）但是一般习惯上都是用`class`定义一个类

### 类的定义

```cpp
class className
{
   // 类体：由成员函数和成员变量组成
 
}; // 一定要注意后面的分号
```

**class**为**定义类的**关键字，**ClassName**为类的名字，**{}**中为类的主体，注意**类定义结束时后面分号**。

**定义类的两种方式：**

1）**声明和定义都放在类体中**，需要注意：成员函数如果**在类中定义**，编译器可能会将其当成**[内联函数]([(23条消息) C++基础（namespace）（引用）（缺省参数）（函数重载）（内联函数）_zhybiancheng的博客-CSDN博客](https://blog.csdn.net/zhybiancheng/article/details/117082247))**处

理。

![image-20210528163643471](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528163643471.png)

2）**声明放在.h的头文件中，**定义放在.cpp文件中，分文件编写。

![image-20210528163856398](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528163856398.png)



### 类的访问限定符

![image-20210528164339172](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528164339172.png)



**【访问限定符说明】**

1. public修饰的成员在类外可以**直接被访问**

2. protected和private修饰的成员**在类外不能直接被访问**（但在类中可以被访问）

3. 访问权限**作用域从该访问限定符出现的位置开始直到下一个访问限定符出现时为止**

4. class的默认访问权限为private

	```cpp
	class A
	{
		void fun4();// 默认私有，类外不可调用
	
	public:
		void fun1();// public限定，类外可以调用
	
	protected:
		void fun2();// protected限定，类外不可调用
	
	private:
		void fun3();// private限定，类外不可调用
	
	};
	```

了解了访问限定符就可以回答上面遗留的问题了

> class和struct定义类有什么区别？
>
> 答：唯一的区别就是默认的访问权限不同，class默认访问权限是public，struct的默认访问权限是private.

注意：为了保护成员数据和给用户使用接口，所以一般把数据成员写在`private`，给外界调用的成员函数写在`public`下

### 类的作用域

**类定义了一个新的作用域**，类的所有成员都在类的作用域中**。**在类体外定义成员，需要使用`::`作用域解析符

指明成员属于哪个类域。

![image-20210528165509299](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528165509299.png)

刚刚在说分文件编写的时候，类中的成员函数在类内声明，在类外定义，但是叫`show`的函数可能有很多，怎么知道这个函数是Date类的呢？就是通过`Date::`类的作用域解析符来判别的。



### 封装的意义

类是对象的抽象，一个类其实也把数据和函数都封装起来的，封装有以下的几点好处

1）可以隐藏内部的细节，对外提供公共的访问接口。

2）提高安全性，防止数据成员被修改



## 类的实例化

类的大体框架了解过后，就可以回到对象上了，前文也讲过其实**类就是对象的抽象，对象就是类的实例化**，

```cpp
class Date
{
public:
	// 打印日期（inline函数）
	void show();
private:
	int _year; // 年
	int _month;// 月
	int _day;  // 日
};
```



类是一个抽象化的东西，就像上面这个类一样，它只说明了可以定义一个日期，但是日期还是得具体问题具体分析，所以你可以定义一个今天的日期`xxxx年xx月xx日`的日期，还可以定义一个明天的日期`xxxx年xx月xx日`，这些被具体化出来的日期就是一个个对象。



## 类大小的计算

```cpp
class A
{
public:
	void fun1();
    void fun2();
private:
    int a;
    char b;
};
```



#### 对象的存储方式

在一个类中有两部分组成，一部分是数据成员，一部分是成员函数，这两个部分都是在类中声明的，所以他们的存储方式就决定了一个类的大小。但是无非就只有两种存储方式。

1）**数据成员和成员函数都存放在类中，每一实例化出的对象中都包含这两个部分。**

但是可以想一想，每个对象的数据成员肯定是以对象而异的，但是所用的对象调用的函数不都是一样的嘛。所有就出现了第二种存储方式，也就是真真的存储方式

2）**对象只保存成员变量，成员函数保留在公共的代码段中**

![image-20210528172045933](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528172045933.png)





**总结：计算类的大小的时候可以只算成员变量的大小之和，成员函数就可以不用管了**



> 思考：要是类中没有数据成员变量，那类的大小是多少呢？

```cpp
class A
{
    // 空..
};
```



有人认为是0字节，但是这个类已经被定义出来了，所以不可能是0字节，答案**是这个类比较特殊所以编译器给了空类一个字节来唯一标识这个类.**

==`sizeof(A) == 0`==



计算类的大小要注意的几点：

**1）成员函数不在计算的范围之内**

**2）空类的大小为1**

**3）在注意到上面两种情况小，计算普通的一个类的大小和计算结构体一样，需要满足**[内存对齐规则]([(23条消息) 结构体内存对齐规则_zhybiancheng的博客-CSDN博客](https://blog.csdn.net/zhybiancheng/article/details/117369441))



### this指针

上文已经了解过了，成员函数是放在公共代码段中的，而数据成员是每个对象都独有的，但是有一个问题，当对象调用成员函数的时候，函数是怎么识别每一个对象的呢？

```cpp
int main()
{
    
	Date d1;
	Date d2;
	d1.show();// d1是怎么调用show()函数的？
	d2.show();// d2是怎么调用show()函数的？

	return 0;
}
```

成员函数是通过一个名为this的指针的额外的隐式参数来访问调用的那个对象。

**this指针的特点：**

1. this指针的类型：类的类型* const（例如日期类中this就是Date* const 类型）

2. 只能在“成员函数”的内部使用

3. **this**指针本质上其实是一个成员函数的形参，是对象调用成员函数时，将对象地址作为实参传递给this

形参。所以**对象中不存储this指针**。

4. **this**指针是成员函数第一个隐含的指针形参，一般情况由编译器通过**ecx**寄存器自动传递

```cpp
int main()
{
    
	Date d1;
	Date d2;
	d1.show();// 相当于d1.show(&d1);
	d2.show();// 相当于d2.show(&d2);
	// 但是this在实际中，是不能显示的写出来的
	return 0;
}
```

```cpp
inline void Date::show(Date* this)
{
    cout << _year << "-" << _month << "-" << _day << endl;
}
```



**注意**

1）**this指针就是隐式的指针，在调用的时候不可以显式的写出来。**

2）**this存放的地方根据编译器不同可能会不同，可能是存放在栈上，但是VS上this存放在寄存器中传递**

![image-20210528180502678](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528180502678.png)



**最后通过一道题目来更深刻的了解this指针**

```cpp
class Date
{
public:
	void PrintDate()
	{
		cout << _year << "-" << _month << "-" << _day << endl;
	}

	void Show()
	{
		cout << "日期显示" << endl;
	}
private:
	int _year;
	int _month;
	int _day;
};

int main()
{
	Date* p = nullptr;
    // // 可以调用以下这两个函数吗？
	p->PrintDate();
	p->Show();
}
```

请思考过后在看答案

> 答案：
>
> p->PrintDate();不可以调用
>
> p->Show();可以调用

有些同学肯定觉得两个函数都不能调用，因为p是空指针，所以用`->`解引用就会报错，但是真的是p指针在调用吗？p指针就是指向Date类的一个指针,但是他指向的对象有点特殊是一个空，所以这个对象的地址就是`nullptr`，相当于`this`是`nullptr`。在成员函数调用的时候是将对象的地址传给函数了，函数在接收地址后，执行对象想要对数据的操作，在调用`p->PrintDate()`的时候，函数进行了访问`_year_`这个成员数据，再写的清楚一点就是`this->_year`，但是this是nullptr，这时就是空指针访问了，所以程序就崩溃了。在调用`p->Show()`,尽管`this`还是`nullptr`,但是函数的内部就是打印了一句话而已，并没有对`this`进行任何的操作，所以程序就不会崩溃。



> 回答问题：
>
> 1. this指针存在哪里？
>
> 2. this指针可以为空吗？

1.this指针存在哪里？

其实编译器在生成程序时加入了获取对象首地址的相关代码。并把获取的首地址存放在了寄存器ECX中也就是成员函数的其它参数正常都是存放在栈中。而this指针参数则是存放在寄存器中。

2.this指针可以为空吗？

可以为空，当我们在调用函数的时候，如果函数内部并不需要使用到this,也就是不需要通过this指向当前对象并对其进行操作时才可以为空，如果调用的函数需要指向当前对象，并进行操作，则会发生错误（空指针引用）。





# 从面向过程到面向对象之熟悉类的组成



## 类中的6个默认构造函数

![image-20210528201242323](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528201242323.png)

在一个类中（即使是空类）会有自动生成6个默认的默认构造函数。

**默认构造成员函数：在我们不在类中自己主动声明和定义的时候，类中就会自动生成的函数**

**默认构造函数的意义：在C语言中，如果我们要写一个Date的结构体或者Stack这样的数据结构，是不是一定会写初始化结构的函数，销毁结构的函数…..既然都要写，所以在C++中就默认类中必须要有初始化对象的函数，销毁对象的函数还有可以复制对象的函数，可以复制给另一个对象的函数，这分别对应着构造函数，析构函数，拷贝构造函数，拷贝赋值运算符重载。**

所以这四个函数是非常重要的4个默认成员函数，就下来会一个一个介绍每一个函数。

因为这四个函数对于带指针的对象和不带指针的对象是不一样的（后面会解释），所以这四个函数会通过演示Complex类和String类的方式来解释这四个函数。



## 构造函数



既然每个对象在类的实例化中都要初始化，所以构造函数就应运而生了，它就是专门给对象初始化的函数，注意不要被构造函数这个名字骗了，其实并不是给对象分配空间构造出对象的空间，而是给对象中的数据初始化。

**其特征如下：**

1. 函数名与类名相同。

2. 无返回值。

3. 对象实例化时编译器**自动调用**对应的构造函数。

4. 构造函数可以重载。





### Complex类（不带指针的类）：

```cpp
class Complex
{
public:
    // 函数名和类名相同
    // 无返回值
	Complex()
	{
		_real = 0;
		_image = 0;
	}
	// 函数重载
	Complex(int real, int image)
	{
		_real = real;
		_image = image;
	}
private:
	int _real; // 实部
	int _image;// 虚部
};

int main()
{
	// 调用无参数的构造函数
	Complex c1;
	// 调用有参数的构造函数
	Complex c2(1, 2);
	// 不会返回一个对象，编译器会把它当做一个函数的声明
	// 以Complex对象为返回值的一个叫c3的函数的函数声明
	Complex c3();
	return 0;
}
```



**要特别注意一下上面的第三个栗子`Complex c3()`这样是不会创建对象的。**



要是我们不写构造函数其实也是可以创建对象的

```cpp
class Complex
{
public:
    /*
    // 如果不写，编译器就会调用自己默认生成的那个构造函数
	Complex(int real, int image)
	{
		_real = real;
		_image = image;
	}
	*/
private:
	int _real; // 实部
	int _image;// 虚部
};

int main ()
{
    Complex c;
    return 0;
}
```

如果不写，编译器就会调用自己默认生成的那个构造函数，但是会有一些不愉快的事情发生

![image-20210528205549008](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528205549008.png)

我们发现_real  和  _image并没有初始化，还是随机值，是的，编译器是不会帮你初始化的，只是不会报错而已，那构造函数还有神马用呀，其实还是有用的向下面这个例子

```cpp
class A
{
public:
	A()
	{
		_a = 10;
	}
private:
	int _a;
};


class Complex
{
public:
    /*
    没写构造函数
    */
private:
	A t;
};

int main()
{
	Complex c;
	return 0;
}
```





![image-20210528210205219](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528210205219.png)

a竟然初始化了，但是Complex也没有写构造函数。这就是语法规则，在类中如果数据成员是内置类型（基本数据类型）就默认不处理，但是对于自定义类型（类，结构体，联合体）的成员就调用对象的构造函数。

**总结：如果类中含有内置类型的数据成员就必须要自己显式的写构造函数，这样才能保证数据初始化，如果类中只含有自定义类型的数据成员就可写可不写构造函数因为系统都会调用自定义的构造函数。（只要有一个内置类型的数据成员就必须写构造函数）**

### 写构造函数的技巧

其实写构造函数搭配[缺省函数]([(24条消息) C++基础（namespace）（引用）（缺省参数）（函数重载）（内联函数）_zhybiancheng的博客-CSDN博客](https://blog.csdn.net/zhybiancheng/article/details/117082247))是最棒的,因为缺省函数可以相当与好多函数重载，这样就可以一劳永逸了

```cpp
class Complex
{
public:
	Complex(int real = 0, int image = 0)
	{
		_real = real;
		_image = image;
	}
private:
	int _real; // 实部
	int _image;// 虚部
};

int main()
{
    Complex c1;// 0, 0
    Complex c2(1);// 1, 0
    Complex c3(1, 2);// 1, 2
    return 0;
}
```

这样就可以方便很多了

**但是还有两点要注意的是**

**1）如果构造函数在类内声明，在类外实现，默认的参数就必须只能写在声明中，在实现的函数中就不能再写默认的参数了**

```cpp
class Complex
{
public:
	Complex(int real = 0, int image = 0);
private:
	int _real; // 实部
	int _image;// 虚部
};

Complex::Complex(int real = 0, int image = 0)// 错误
{
	_real = real;
	_image = image;
}

Complex::Complex(int real, int image)// 正确
{
	_real = real;
	_image = image;
}

```



**2）默认的构造函数只能有一个，缺省的函数是默认的构造函数，不带参数的函数也是默认的构造函数，所以这两者不能同时存在，否则函数不知道要调用哪个函数就会报错**

```cpp
class Complex
{
public:
	Complex()// 不用传参数
	{
		_real = 0;
		_image = 0;
	}

	Complex(int real = 0, int image = 0)// 不用传参数
	{
		_real = real;
		_image = image;
	}
private:
	int _real; // 实部
	int _image;// 虚部
};


int main ()
{
    Complex c;// 调用哪一个函数呢？
    return 0;
}
```



### 初始化列表

在C++语法中初始化成员数据的方法除了上面在**构造函数体内初始化**之外还有一种方法叫做：**初始化列表初始化**

**初始化列表：以一个冒号开始，接着是一个以逗号分隔的数据成员列表，每个"成员变量"后面跟一个放在括**

**号中的初始值或表达式。**

```cpp
class Complex
{
public:
    //                      冒号  成员数据(形参),成员数据(形参) ... {}
	Complex(int real, int image):_real(real), _image(image) {}
private:
	int _real; // 实部
	int _image;// 虚部
};


int main ()
{
    Complex c;// 调用哪一个函数呢？
    return 0;
}
```

**在这里要解决两个问题：**

**1）初始化列表初始化和构造函数体内初始化的区别？**

**2）初始化列表的使用注意细节和好处？**



1.第一个问题两个中初始化方式的不同点。其实在严格意义上讲只有在初始化列表中初始化才叫做初始化，而函数体内的初始化叫做成员数据的赋值。举个栗子：

```cpp
// 方法1
int i = 0;
// 方法2
int i;
i = 0;
```

在上面这个栗子中，两中方法正好对应着两种初始化方式。方法一就对应这初始化列表初始化，而方法二对应着函数体内初始化。也就是说在进入构造函数之前就会进入初始化列表，如果在初始化列表中初始化，就相当于**在定义的时候初始化**（方法1），一旦进入了函数体内部，就意味着**已经对变量声明，在函数体内者在进行赋值操作。** 再简单来说就是**函数体内对变量先声明再赋值，初始化列表中对变量在定义的时候初始化。**



2.第二个就要谈谈初始化列表的使用细节。

1）在初始化列表中只能对一个成员变量**初始化一次。**

```cpp
class Complex
{
public:
	Complex(int real, int image):_real(real), _image(image),_real(1) {}
private:
	int _real; // 实部
	int _image;// 虚部
};
```

像这样对_real初始化了两次就不可以。

2）有些数据成员可用可不用初始化列表初始化的，但是有些数据成员是必须要用初始化列表初始化的。

如果数据成员是内置类型像Complex的两个数据成员都是int类型的，就可以不用初始化列表初始化。因为初始化列表的作用就是让变量在定义的时候初始化，但是C++中内置类型的变量在声明的时候虽然不会分配空间但是会给变量一个随机值（或者默认值），如Complex中如果不给int一个初值，它自己就会有一个随机值，函数体内再赋值给变量。

```cpp
int i;
i = 0;
```

内置类型就像这样初始化了，但是有一些必须要在定义的时候初始化的成员就一定要用初始化列表初始化。

1）const成员变量

2）引用成员变量

3）自定义类型成员并且没有默认的构造函数

```cpp
const int& a;// 错误
int& b;// 错误

int t = 10;
const int& a = t;// 正确
int& b = t;// 正确
```

const成员只有在初始化的时候才可以给变量数值，引用不能空引用。所以const成员变量和引用成员因为必须在初始化的时候定义所以必须要在初始化列表中初始化。



如果自定义类型的成员数据有默认构造函数（1.无参构造函数2.全缺省构造函数3.系统默认生成的构造函数）可以不用初始化列表。

```cpp
class A
{
public:
    // 全缺省的构造函数
    A(): _a(0) {}
private:
    int _a;
}

class B
{
public:
    B(int b) : _b(b) {}
private:
    int _b;
    A _a;
}
```

这样是可以的，因为在声明的时候`A _a`就默认已经在调用构造函数了，但是如果是自定义类型没有默认的构造函数就必须要用初始化列表。

```cpp
class A
{
public:
    // 需要参数的构造函数
    A(int a): _a(a) {}
private:
    int _a;
}

class B
{
public:
    B(int b) : _b(b) {}// 错误
private:
    int _b;
    A _a;
}
```

👆这错误的写法，因为在`A _a`声明的时候，这是能能调用的。这就像如果A类的构造函数需要一个参数你可以直接`A _a`吗？不可以吧，这时必须要用`A _a(2)`括号里面必须要有一个参数，才可以传参。



**总结：在成员数据中有const成员或者引用成员或者没有默认构造函数的自定义成员时，必须要用初始化列表初始化。而且如果可以的话，初始化变量的时候都用初始化列表的方式，这样可以效率更高也不容易出错。**

还有最后一个小的注意细节：就是成员变量在初始化列表中的初始化顺序。

注意： **成员变量**在类中**声明次序**就是其在初始化列表中的**初始化顺序**，与其在初始化列表中的**先后次序无关**

```cpp
class A {
public:
 A(int a)
 :_a1(a)
 ,_a2(_a1)
 {}
 
 void Print() {
 cout<<_a1<<" "<<_a2<<endl;
 }
private:
 int _a2;
 int _a1; }
int main() {
 A aa(1);
 aa.Print();
}
```

上面这个程序的会出现什么？答案是输出`1和随机值`因为在成员变量声明的时候是_a1先声明 _1再声明，所以在初始化列表中是 _a2先初始化，所以a2是随机值。这是初始化列表中的初始化顺序的一个小细节



### String类（带指针的类）

String类和Complex类还是不一样的，在String类中需要带一个指针方便动态的开辟空间。

String类中一定要写构造函数，因为要给动态开辟一段空间，所以在构造函数中一定要写开辟空间，如果数据成员中还有其他的成员数据还要给内置类型的数据初始化，自定义类型的数据成员还是会自动调用自己的构造函数

```cpp
class String
{
	String(const char* str = 0)
	{
		if (str)
		{
			_data = new char[strlen(str) + 1];// 开辟空间留下一个位置给'\0'
			strcpy(_data, str);// 将内容拷贝到_data中
		}
		else// 如果str为空字符串就默认是'\0'
		{
			_data = new char[1];
			*_data = '\0';
		}
	}
private:
	char* _data;
};
```

总结：

![image-20210528214858408](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528214858408.png)



## 析构函数

每个类创建对象后，使用完之后都要对象中的内容清空，所以析构函数就出现了

析构函数：与构造函数功能相反，析构函数不是完成对象的销毁，局部对象销毁工作是由编译器完成的。而

**对象在销毁时会自动调用析构函数，完成类的一些资源清理工作**。



析构函数**特征**如下：

1. 析构函数名是在类名前加上字符 ~。

2. 无参数无返回值。

3. 一个类有且只有一个析构函数。若未显式定义，系统会自动生成默认的析构函数。

4. 对象生命周期结束时，C++编译系统系统自动调用析构函数。



### Complex类（不带指针的类）

```cpp
class Complex
{
public:

	Complex(int real = 0, int image = 0)
	{
		_real = real;
		_image = image;
	}
	// ~ + 类名
	~Complex()// 析构函数
	{
		// 中间什么都不用写
	}
private:
	int _real; // 实部
	int _image;// 虚部
};
```

### String类（带指针的类）

```cpp
class String
{
	String(const char* str = 0)
	{
		if (str)
		{
			_data = new char[strlen(str) + 1];// 留下一个位置给'\0'
			strcpy(_data, str);
		}
		else// 如果str为空字符串就默认是'\0'
		{
			_data = new char[1];
			*_data = '\0';
		}
	}

	~String()// 析构函数记得要释放内存
	{
		delete[]_data;
	}
private:
	char* _data;
};
```





**在Complex类的析构函数中什么都不用写，是不是又感觉析构函数也没啥用,可是在String类中析构函数要释放刚刚动态分配的内存，而且和构造函数类似如果类中有自定义类型的数据，他就会自动的调用自己的析构函数**

```cpp
class String
{
public:
	String(const char* str = 0)
	{
		if (str)
		{
			_data = new char[strlen(str) + 1];// 留下一个位置给'\0'
			strcpy(_data, str);
		}
		else// 如果str为空字符串就默认是'\0'
		{
			_data = new char[1];
			*_data = '\0';
		}
	}

	~String()// 析构函数记得要释放内存
	{
		delete[]_data;
	}
private:
	char* _data;
};

class A
{
private:
	String _str;// 自定义数据默认调用自己的析构函数
	int _a;
};

int main()
{
	A a;
	return 0;
}
```



![image-20210528221125165](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210528221125165.png)



## 拷贝构造函数

在对象的构建中，我们经常会对一个对象进行复制拷贝，需要一个和某个对象一模一样的对象，所以在类中默认就会生成的函数中就包括了拷贝构造函数。

看名字就知道拷贝构造函数也是一种构造函数，也就是说他的作用是给一个对象初始化。

**注意的两个点：**

1. 拷贝构造函数**是构造函数的一个重载形式**。

2. 拷贝构造函数的**参数只有一个**且**必须使用引用传参**，使用**传值方式会引发无穷递归调用**。

### Complex类（不带指针的类的拷贝）

```cpp
class Complex
{
public:
    // 缺省的构造函数
    Complex(int real = 0, int image = 0)
	{
		_real = real;
		_image = image;
	}
	// 传递的参数是对象c的引用
	Complex(const Complex& obj)
	{
		_real = c._real;
		_image = c._image;
	}

private:
	int _real; // 实部
	int _image;// 虚部
};
```

**重点：一定要用`const + 对象 + 引用`来传值,如果用普通的传参拷贝就会引发递归死循环，因为如果是普通的传参，其实传进函数中的对象obj是函数外调用者对象的一个临时拷贝，这是就会又引发obj的拷贝构造，在拷贝构造的参数传进来之前又引发拷贝构造，这样下去就是一个死循环。**

![image-20210529231005511](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210529231005511.png)

**所以为了解决这个问题，那传参数的时候，就不让参数再调用构造函数，而是用引用给传进来的对象起一个“别名”就可以啦。而且因为是出入一个引用，为了保护类外调用的被引用的对象的安全，所以要加一个`const`保护一下，防止对象会被人修改。**

### 拷贝构造函数的使用

两种方式都是一样的

```cpp
Complex c1;
Complex c2(c1);// 第一种
Complex c3 = c1;// 第二种
```

以上两中方式都是可以的。

### 浅拷贝（值拷贝）

 **若未显示定义，系统生成默认的拷贝构造函数。** 默认的拷贝构造函数对象按内存存储按字节序完成拷

贝，这种拷贝我们叫做浅拷贝，或者值拷贝。

**也就是说如果在没有指针的类中，其实默认生成的构造函数就和我们显式写出来的拷贝构造函数是一样的。**都可以达到一样的效果。



### String类（带指针的拷贝构造）

如果是在类中有指针的类中，还是用系统生成的默认拷贝构造函数，浅拷贝是完不成我们的期望的。

```cpp
class String
{
	String(const char* str = 0)
	{
		if (str)
		{
			_data = new char[strlen(str) + 1];// 留下一个位置给'\0'
			strcpy(_data, str);
		}
		else// 如果str为空字符串就默认是'\0'
		{
			_data = new char[1];
			*_data = '\0';
		}
	}

	~String()// 析构函数记得要释放内存
	{
		delete[]_data;
	}
private:
	char* _data;
};

int main()
{
    String obj1("hello");
    String obj2(obj1);
    return 0;
}
```

![image-20210529232654071](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210529232654071.png)

**浅拷贝使得带指针的对象只是将指针复制过去了而已，所以只能得到一个对象obj2的指针和对象obj1的指针所指向的对象是同一个对象，而不是将指针所指向的内容一起复制，而且最严重的是因为两个指针同时指向一块空间，所以在调用析构函数的时候，两个对象会对同一块空间释放两次内存，这就是常说的“浅拷贝问题”。**

解决方法其实很简单：进行深拷贝。**即将指针指向的内容也复制一份，然后让obj2的指针指向被复制的区域即可。**

```cpp
inline
String(const String& str)
{
    // 开辟和str中_data一样的空间大小，+1是为了放结尾的'\0'
    _data = new char[strlen(str._data) + 1];
    // 复制内容
    strcpy(_data, str._data);
}
```

**总结：**

**1）在不带指针的类中（如Complex类），在调用拷贝构造时候值拷贝（浅拷贝）就可以满足复制一个对象的效果了。所以在这种类的内部可以不用显式的写出构造函数，可以直接用类中默认生成的构造拷贝。**

**2）在带指针的类中（如String类），在复制另一个对象的时候需要的”深拷贝“，所以必须要手动的开辟另一块空间并将原来的内容复制到这块空间中，否则程序就会因为在同一块空间析构两次二崩溃。**

## 拷贝赋值运算符重载



### 运算符重载

运算符重载就是赋予了运算符的意义。在默认的系统中，内置类型是有运算符可以直接比较的（+，-，*，/…），但是一个同种的对象中怎么比较呢？当然你可以写一个函数来实现你的目的。如果你想将两个Complex对象相加，你可以这样：

```cpp
Complex& Add(Complex obj1, Complex obj2)
{
    // 注意一个小语法：类名(/*数据*/)创建一个临时对象
    // 也是说这个对象的生命期就在这5行上，到第6行的时候就自动销毁了
    Complex(obj1._real + obj2._real, obj1._img + obj2._img);
}
```

但是这样不够直观，如果可以这样`obj1 + obj2`，不是更只直观嘛。所以**C++为增加代码的可读性**，就增加了运算符重载。

**函数名字为：operator后面接需要重载的运算符符号。**

函数原型：**返回值类型** **operator操作符(参数列表)**

**注意：**

**1）运算符重载不能创造一个新的元素符（如@…)**

**2）内置类型的操作符不可以改变原来操作符的意义。**

**3）作为成员函数重载运算符的时候，有一个默认形参this，而且限定为第一个函数形参**

**4）.* （点，星号）、:: （冒号冒号）、sizeof 、?:（问号冒号） 、.（点） 注意以上5个运算符不能重载**



拿Complex的==好举个栗子：

```cpp
class Complex
{
public:

	// 函数重载
	Complex(int real = 0, int image = 0)
	{
		_real = real;
		_image = image;
	}
private:
	int _real; // 实部
	int _image;// 虚部
};
// 错误
bool operator== (const Complex& c1, const Complex& c2)
{
	return c1._real == c2._real && c1._image == c2._image;
}
```

这样将函数写在类外不加任何东西是错误的写法，因为在类外的对象根本就不能直接访问自己的私有成员，所以这样就会报错。但是在后面的内容中我会介绍一种特殊的函数`friend`友元函数，这种函数可以打破类的封装，那样就可以访问私有成员了。或者还可以将运算符重载写在函数中也是可以的，这样还可以少写一个形参（在类中默认的参数this）

```cpp
class Complex
{
public:

	// 函数重载
	Complex(int real = 0, int image = 0)
	{
		_real = real;
		_image = image;
	}
 //返回值  operator运算符()
	bool operator== (const Complex& c)// bool operator== (const Complex* this, const Complex& c)
	{
		return _real == c._real && _image == c._image;
	}
private:
	int _real; // 实部
	int _image;// 虚部
};

int main()
{
	Complex c1(1, 2);
	Complex c2(1, 2);
	// 因为==的优先级比较低，所以要加括号
    // c1就是调用函数的对象
	cout << (c1 == c2) << endl;
    cout << c1.operator(c2) << endl;// 和上面一样，但是为了可读性一般写成上面的样子
	return 0;
}
```

前置和后置的区别：多加一个占位符int

函数默认是前置（+ +或- -）要用int占位符区分

### 再谈拷贝复制运算符重载



在运算符中有一种特殊的运算符—赋值号（=），这个运算符可以将一个对象的值赋值给另一个对象。

**复制和赋值的区别：复制是在一个对象还不存在的时候，用一个对象初始化另一个对象，但是赋值是两个对象都已经存在了，将一个对象的内容赋给另一个对象**

```cpp
Complex c2(1,2);
// 复制
Complex c1 = c2;// c1在初始化创建
// 赋值
Complex c3;//c3已经创建出来了
c3 = c2;
```



赋值运算符要注意四点：

1. 参数类型

2. 返回值

3. 检测是否自己给自己赋值

4. 返回*this

5. 一个类如果没有显式定义赋值运算符重载，编译器也会生成一个，完成对象按字节序的值拷贝。

#### Complex类（不带指针的类的赋值）

```cpp
class Complex
{
public:

	// 函数重载
	Complex(int real = 0, int image = 0)
	{
		_real = real;
		_image = image;
	}

	Complex& operator= (const Complex& c)
	{
		// 如果this和对象c是同一个对象，就直接返回
		if (this == &c)
			return *this;

		_real = c._real;
		_image = c._image;
		return *this;
	}
private:
	int _real; // 实部
	int _image;// 虚部
};

int main()
{
    Complex c1;
    Complex c2;
    c1 = c1;// 自己赋值给自己
    c1 = c2;
    return 0;
}
```

**注意：**

**1）在进入赋值函数的时候，要先判断一下是不是赋值给自己，如果赋值给自己就没必要在进行重复的操作，而且在带指针的类中是必须要加这个判断的（后面讲解）。**

**2）如果不自己写一个拷贝复制运算符重载函数，其实在不带指针的类中是没有区别的，因为默认的拷贝复制运算符重载函数也是这样写的。**



#### String类（带指针的类的赋值）

但是在带指针的类中的拷贝复制运算符重载实际上也是一个”浅拷贝“，所以又是将指针拷贝了一份，而指针指向的内容却没有，只是两个指针指向了同一块空间。

![image-20210530220936736](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210530220936736.png)

这时候只能是深拷贝来解决这个问题了，但是对于初学者来说可能有些复杂，所以只是贴一个代码，以后我会再写一个深拷贝的讲解

```cpp

class String
{
	String(const char* str = 0)
	{
		if (str)
		{
			_data = new char[strlen(str) + 1];// 开辟空间留下一个位置给'\0'
			strcpy(_data, str);// 将内容拷贝到_data中
		}
		else// 如果str为空字符串就默认是'\0'
		{
			_data = new char[1];
			*_data = '\0';
		}
	}

	String& operator=(const String& str)
	{
		if (this == &str)
			return *this;

		// 删掉原来的空间
		delete[] _data;
		// 开辟一个和str一样的大小空间
		_data = new char[strlen(str._data) + 1];
		strcpy(_data, str._data);
		return *this;
	}

private:
	char* _data;
};
```

总结：

![image-20210530215017569](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210530215017569.png)

**在带指针的类中一定要自己写一个拷贝复制运算符重载函数，以避免浅拷贝的问题。**





# 从面向过程到面向对象之类的其他特殊成员



## 内存管理（new和delete）

array new 用delete释放而不是delete[]释放的问题：

1）对于内置类型来说，编译时不会报错，也不会存在内存泄漏，但是不建议这么使用。

2）但是对于自定义类型来说，如果用new 类型[]初始化，但使用delete来释放不仅会倒置内存泄漏，更可怕的是程序会直接崩溃掉







## const成员





## 友元函数





## 静态成员



静态成员只能在类外初始化

只能在该cpp所在的编译模块中使用该变量（static限制了变量的作用域）

static函数唯一能够访问的就是static变量或者其他static函数



## 日期类的实现

```cpp
#pragma once
#include <iostream>

using namespace std;

class Date
{
public:
	// 获取某年某月的天数
	int GetMonthDay(int year, int month) const
	{
		// 用静态成员修饰一只要用的数组，这样可以减少一点开销
		static int monthday[13] = { 0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
		int day = monthday[month];
		// 判断闰年，然后修改天数
		if (month == 2 && ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)))
			day += 1;
		return day;
	}
	// 全缺省的构造函数
	Date(int year = 1900, int month = 1, int day = 1)
	{


		// 判断日期的合法性
		if (year < 0 || month < 1 || month > 12
			|| day < 1 || day > GetMonthDay(year, month))
			cout << "日期不合法" << endl;




		_year = year;
		_month = month;
		_day = day;
	}
	// 拷贝构造函数
	// d2(d1)
	Date(const Date& d)
	{
		_year = d._year;
		_month = d._month;
		_day = d._day;
	}
	// 赋值运算符重载
	// d2 = d3 -> d2.operator=(&d2, d3)
	Date& operator=(const Date& d)
	{


		// 如果this和d是一个对象，自己给自己赋值要特判
		if (this == &d)
			return *this;




		_year = d._year;
		_month = d._month;
		_day = d._day;
		return *this;
	}
	// 析构函数
	~Date()  {}
	// 日期+=天数
	Date& operator+=(int day)
	{
		// 如果天数为负数，就调用-=
		if (day < 0)
		{
			*this -= -day;
		}
		else
		{
			_day += day;// 将天数先加到日期中
			// 如果日期的天数不符合当月的天数就让_month++
			while (_day > GetMonthDay(_year, _month))
			{
				_day -= GetMonthDay(_year, _month);
				_month++;
				// 判断月份是否超过12月，否则将_year++
				if (_month == 13)
				{
					_year++;
					_month = 1;
				}
			}
		}
		return *this;
	}
	// 日期+天数
	Date operator+(int day)
	{
		// +重载是将一个新的对象返回，+=是对this直接操作
		Date tmp(*this);
		// 用上面已经写好的+=运算符
		tmp += day;
		return tmp;
	}
	// 日期-天数
	Date operator-(int day)
	{
		Date tmp(*this);
		tmp -= day;
		return tmp;
	}
	// 日期-=天数
	Date& operator-=(int day)
	{
		// 如果天数是负数，就调用+=
		if (day < 0)
		{
			*this += -day;
		}
		else
		{
			_day -= day;
			while (_day < 0)
			{
				// 因为是要加上个月的天数，所以month--
				// 然后判断天数在当月的合法性
				_month--;
				if (_month == 0)
				{
					_month = 12;
					_year--;
				}
				_day += GetMonthDay(_year, _month);
			}
		}

		return *this;
	}
	// 前置++
	Date& operator++()
	{
		*this += 1;
		return *this;
	}
	// 后置++
	Date operator++(int) 
	{
		// 保留一份*this
		Date tmp(*this);
		*this += 1;
		return tmp;
	}
	// 后置--
	Date operator--(int) 
	{
		// 保留一份*this
		Date tmp(*this);
		*this -= 1;
		return tmp;
	}
	// 前置--
	Date& operator--()
	{
		*this -= 1;
		return *this;
	}

	// 比较运算符只要写出2个
	// 就可以复用出其他所有的比较运算符

	// >运算符重载
	bool operator>(const Date& d) const 
	{
		// 如果年不相等，就返回年份更大的
		if (_year != d._year) return _year > d._year;
		// 如果年份相等但是月份不相等，就返回月份更大的
		if (_month != d._month) return _month > d._month;
		// 如果年月都相等，就返回天数更大的
		return _day > d._day;
	}
	// ==运算符重载
	bool operator==(const Date& d) const
	{
		return (_year == d._year && _month == d._month
			&& _day == d._day);
	}
	// >=运算符重载
	bool operator >= (const Date& d) const
	{
		return (*this > d || *this == d);
	}
	// <运算符重载
	bool operator < (const Date& d) const
	{
		return !(*this >= d);
	}
	// <=运算符重载
	bool operator <= (const Date& d) const
	{
		return !(*this > d);
	}
	// !=运算符重载
	bool operator != (const Date& d) const
	{
		return !(*this == d);
	}
	// 日期-日期 返回天数
	int operator-(const Date& d)
	{
		int day = 0;
		Date maxDate = d;
		Date minDate = *this;
		if (*this > d)
		{
			maxDate = *this;
			minDate = d;
		}
		while (minDate != maxDate)
		{
			minDate += 1;
			day++;
		}
		return day;
	}
	friend ostream& operator<<(ostream& out, const Date& d);
	// 输入的时候，不能用const修饰，因为会改变对象d的值
	friend istream& operator>>(istream& in, Date& d);
	
private:
	int _year;
	int _month;
	int _day;
};

ostream& operator<<(ostream& out, const Date& d)
{
	out << d._year << "-" << d._month << "-" << d._day;
	return out;
}

istream& operator>>(istream& in, Date& d)
{
	in >> d._year >> d._month >> d._day;
	return in;
}
```