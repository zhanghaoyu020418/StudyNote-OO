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
		2.一般的构造函数初始化时Book b1(...),但是选用默认无参构造函数是直接写成Book b1;而不是Book b1();
	    3.虽然每个类中可以有多个构造函数，但是每次只会执行一个构造函数去初始化成员数据、**

