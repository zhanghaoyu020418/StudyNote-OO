# C++面向对象学习笔记

## 构造函数对类对象的初始化
前言类中的私有的数据成员可以用成员函数初始化，但是太麻烦，所以引出了构造函数的初始化,这是一类特殊的成员函数，系统会自动调用构造函数

//回顾一下成员函数来初始化数据成员
```cpp
class Time
{
public:

	void set_time();
	void show_time();
private:
	int hour;
	int minute;
	int sec;
};

void Time::set_time()
{
	cin >> hour >> minute >> sec;
}

void Time::show_time()
{
	cout << hour << ':' << minute << ':' << sec << endl;
}

int main()
{
	Time t1;
	t1.set_time();
	t1.show_time();
	return 0;
}
```


***构造函数实现数据成员的初始化***

==**1.可以直接在类内直接初始化**==
```cpp
public:
	Time()
	{
		hour = 18;
		minute = 18;
		sec = 18;
	}
```
==**2.在类内声明，在类外定义**==
```cpp
public:
	Time();
/*------*/	
Time::Time()
{
	hour = 18;
	minute = 18;
	sec = 18;
}

```
## 带参数的构造函数
**注释：为什么要带参数的构造函数呢？用上面可知构造函数会自动调用，所以后果就是所用的对象都会有一样的初值，而带参数的构造函数就可以让不同的对象会有不同的初值**

```cpp
class Book
{
public:
	Book(int, float, const char*, string);//带参数的构造函数,可以自定义每个对象的数据成员
	void show_book();
private:
	int id;
	float price;
	char book_name[20];
	string author_name;
};

Book::Book(int i, float p, const char* n1, string n2)//用输入的数据初始化每个成员数据
{
	id = i;
	price = p;
	strcpy(book_name, n1);
	author_name = n2;
}

void Book::show_book()
{
	cout << "id :" << id << endl;
	cout << "price :" << price << endl;
	cout << "book_name :" << book_name << endl;
	cout << "author_name :" << author_name << endl;
}

int main()
{
	Book b1(1, 55.5, "老人与海", "海明威");//可以用，类名+对象名(初始化的数据)
	b1.show_book();
	return 0;
}
```
## 用参数初始化表对数据成员初始化

虽然构造函数可以让每个人对象的数据成员不同，但是还是要写一个成员函数来初始化成员数据，有没有一种让对象简便的初始化方法同时还可以让每个对象的成员数据不同呢？参数初始化表就可以做到这一点

```cpp
Book(int i, float p, const char* n1, string n2) :id(i), price(p), author_name(n2)//参数的初始化列表
{
	strcpy(book_name, n1);
}
```

```cpp
Box(int h, int w, int len):height(h),width(w),length(len) {};
```

==**注意：**==数组不能在参数的初始化列表中初始化，而要在函数体中({}中)初始化(strcpy),**而且strcpy需要const char[]赋值到char[]中**

## 构造函数的重载
重载函数的就是同名函数但是函数的参数不同

```cpp
class Book
{
public:
	Book(); //不给出实参的构造函数是默认构造函数
	Book(int i, float p, const char* bn) :id(i), price(p)
	{
         bn = new char[strlen(bn) + 1];
		strcpy(book_name, bn);
	}//没有给出string的重载函数
	Book(int i, float p) :id(i), price(p) {}//没有给出string和char的构造函数
	Book(int i) :id(i) {}//没有给出string,char,float的构造函数
	void show_book();//因为对象的成员数据是私有的，所以必须写一个函数展示成员的数据成员
private:
	int id;
	float price;
	char book_name[20];
	string author_name;
};

Book::Book()
{
	id = 1;
	price = 66.6;
	author_name = "haha";
}

void Book::show_book()
{
	cout << id << endl;
	cout << price << endl;
	cout << book_name << endl;
	cout << author_name << endl;
}

int main()
{
	Book b1;
	Book b2(1, 55.5, "zhy");
	Book b3(2, 66.6);
	Book b4(3);
	return 0;
}
```
==注意：==

**1.无参数的构造函数属于默认的构造函数，如果没有定义构造函数则系统会自动增加一个函数体为空的默认构造函数，所以每个类中==有且只有一个默认构造函数==
		2.==一般的构造函数初始化时Book b1(...),但是选用默认无参构造函数是直接写成Book b1;而不是Book b1();==
	    3.虽然每个类中可以有多个构造函数，但是每次只会执行一个构造函数去初始化成员数据、**

## 使用默认参数的构造函数
在购构造函数中可以通过实参传递的方式给对象赋值，也可以用默认参数在构造函数中直接复制，这样就不用每次都给对象符合同样的值了
```cpp
public:
	Box(int h = 10, int w = 10, int len = 10) :height(h), width(w), length(len) {}
```
这样用了带有默认参数的构造函数，初始化对象的时候，如果只是`Box b1`这时b1的灿哥参数都为10。
**构造函数的的作用其实相当于好几个重载函数在一起，这样子即没有参数赋值也不会报错，并且在初始化有相同参数的对象的时候十分方便**

![1615346443621](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\1615346443621.png)


==注意==
**1.有默认参数的函数必须在类中声明就显示出默认参数**

2.
==在类中也可以省去参数名==
```cpp
Box(int = 10, int = 10, int = 10);

Box::Box(int h, int w, int len)
{
	height = h;
	width = w;
	length = len;
}
```

**3.默认参数函数也算是默认构造函数，但是注意一个类中只能有一个默认构造函数，所以有了带默认参数函数就不能1.无参数的函数2.重载函数，否咋系统无法识别，就会报错，所以一般重载函数和默认参数函数只会出现一个**

## 析构函数

**析构函数和构造函数是一对，一个自动给对象初始化，一个自动给对象清理数据，所以结构和构造函数相似，如果不写析构函数，系统会自动生成一个仅仅回收对象内存的析构函数**

==**构造函数作用就是在回收对象所占内存之前将对象中的数据清空**==

形式如下：

`~类名(){}`
参数列表中你可以让函数输出你要在对象回收之前所作的操作

```cpp
~Box() {}
```

## 构造函数和析构函数的调用顺序
函数调用满足栈的条件先进后出，就是先构造的后析构，后构造的先析构

**1.同种类型的对象满足先进后出**

**2.不同种类型的对象调用的顺序也不一样**

![](D:\github仓库\StudyNote-OO\QQ图片20210310115205.png)

举例如下：

```cpp
class Box
{
public:
	Box(int h = 10) :height(h) { cout << "调用啦 " << height << endl; }
	~Box() { cout << "回收啦" << height << endl; }
private:
	int height;
};

Box b1(1);//全局变量，从main执行之前开始调用构造到main执行结束析构
int main()
{
	static Box b2(2);//static定义的局部变量，从开始定义调用构造到main函数中结束析构
	Box b3(3);//局部变量，从定义位置调用构造到main结束析构

	{
		Box b4(4);//从进入函数构造到出了作用域析构
	}

	Box* pb = new Box(5);//new分配的动态内存,从new位置调用构造到delete调用析构
	delete pb;

	return 0;
}
```


## 对象数组
数组嘛，就是存放对象的数组，数组中的元素就是一个个对象
### 对象数组的创建很简单: 
`Box box[20]` 
`类名 对象名[num]`
### 主要学会对象的初始化


**1.对象只有一个参数的时候可以直接**
```cpp
Box(int h):height(h) {}

/*-----------*/
Box box[3] = {1, 2, 3};
```
**2.有默认参数构造函数**
```cpp
Box(int h = 10, int w = 10, int l = 10) :height(h), width(w), length(l) {}
```
**这其实给每个对象的第一个参数赋值，这样其实有不太好，因为会有人理解为给第一个对象赋值3个参数，所以一般不要这么写。**

**3.分别给每一个对象初始化，这种方式最好，最常见**

```cpp
Box box[3] = {
	Box(1, 1, 1),
	Box(2, 2, 2),
	Box(3, 3, 3)
};
```
==在函数体中分别再给对象初始化==


## 对象指针

### 指向对象的指针
`类名 *对象指针名`
用法和结构体指针类似

### 指向对象成员的指针
`成员的数据类型 *指针变量名`
用法和普通的指针类似

### 指向对象函数的指针
这个和一般的函数指针有所不同，因为是类中的函数所以要加一个类名作为限定

```cpp
数据类型名 (类名::指针变量名)(参数列表) = & 类名::函数//不能写成对象.成员函数
```

举个栗子：

```cpp
class Box
{
public:
	Box() :height(0), width(0), length(0) {}
	void show_box();
	int height;
	int width;
	int length;
};

void Box::show_box()
{
	cout << height << ' ' << width << ' ' << length << endl;
}

int main()
{
	//指向对象的指针
	Box b1;
	Box *pb = &b1;
	pb->show_box();
	//指向对象成员的指针
	int *pb_member = &b1.height;
	cout << *pb_member << endl;
	//指向对象的成员函数的指针
	void(Box::*pbr)() = &Box::show_box;
	(b1.*pbr)();

	return 0;
}
```


## this指针

一个类的成员函数是共有的，但是对象却可以有很多创建的对象，那当怎么分清各个对象的数据成员的呢？其实有一个this指针指向每一个对象，当调用成员的时候其实隐式的将this指针也传了过去
```cpp
Box b1(1, 2, 3);

void Box()
{
	cout << height << ' ' << width << ' ' << length << endl;
	cout << this->height << ' ' << this->width << ' ' << this->length << endl;
	cout << b1.height << ' ' << b1.width << ' ' << b1.length << endl;
}
```
**以上3种形式是一样的效果，只是系统会给优化成this指针版本**

