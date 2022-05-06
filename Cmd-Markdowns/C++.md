# C++   
## 变量声明与定义
---
变量只能被定义一次，但是可以被多次声明。
变量的定义必须出现在且只能出现在一个文件中，而其他用到该变量的文件必须对其进行声明，绝对不能重复定义。

对所有指针进行初始化，在可能情况下，尽量定义完对象后，再定义指针。

*  引用一旦定义，无法再绑定到其他的对象，之后使用的这个引用都将访问 其最初绑定的那个对象。
*  指针和其他任何变量一样（只要不是引用），对指针赋值就是令他存放一个新地址，从而指向一个新对象。

> 赋值永远改变的是等号左侧的对象
> 两个类型相同的合法指针，可使用（==）相等操作符或者（！=）操作符来进行比较，返回比较结果为布尔类型。

## const 定义一种变量，其值不能改变

``` 
#可以用const类型的对象去初始化另外一个对象，ci还是一个整型数，此时与其是否为一个常量无关。
int i = 42;
const int ci = i;
int j = ci;
```
## 指针、引用与const
所谓的指向常量的引用或者指针，其实是指针或者引用“自以为是”，他们觉得自己指向了常量，自觉的不去改变所指对象的值，但是其所指对象值是可以修改的。
```C++
//通过改变引用对象的值，实现对指向常量的引用进行修改
    int i = 100;
    int &r1 = i;
    const int &r2 = i;
    r1 = 4;
    cout<<"r1 : "<<r1<<endl;
    cout<<"r2 : "<<r2<<endl;
    
  >>r1 : 4
  >>r2 : 4
```

```c++
    const double pi = 3.14;
    const double *cptr = &pi ;
    cout<<"*cptr : "<<*cptr<<cptr<<endl;
    double dval = 3.44;
    cptr = &dval;

    cout<<"pi : "<<pi<<&pi<<endl;
    cout<<"*cptr : "<<*cptr<<cptr<<endl;
    
    >>*cptr : 3.140x7fff17bfd310
    >>pi : 3.140x7fff17bfd310
    >>*cptr : 3.440x7fff17bfd318
```

> 一般来说，如果认定变量一定就是一个常量表达式，那就把他声明成 `constespr`

## 头文件
头文件通常包含那些只能被定义一次的实体，如类，const和constexpr变量等。头文件也经常用到其他头文件的功能。

> 头文件一旦改变，相关的源文件必须重新编译以获取更新过的声明。

## string 的操作

    os<<s                将s写到流os中，返回os
    is>>s                从is中读取字符串赋给s，字符串以空白分割，返回is
    getline（is,s)       从is中读取一行赋给s,返回is
    
## 字符串字面值与string 是不同的类型
```C++
    string s1 = "hello",s2= "world";
    string s3 = s1+ "," +s2;
    string s4 = s1  +",";
    // 两个运行对象都不是string,两个都是字面值，不能直接相加。
    // string s5 = "hello"+ ",";
    string s6 = s1 +" ," +"world";
    // 不能把字面值直接相加
    //  string s7 = "hello" +","+s2;
    string s7 = "hello ," +s2+ ",";
```

## 标准库类型

### 列表初始量还是元素数量//实现函数阶乘

    int fact(int val){
    	
    }

初始化的真实含义依赖于传递初始值时用的是花括号`{}`还是圆括号`（）`。
```
 vector<int> v1(10);                //V1有10个元素，每个元素的值都为0
 vector<int> v2{10};                //v2有1个元素，该元素的值为10
 
 vector<int> v3(10,1);              //v3有10个元素，每个元素的值都为1
 vector<int> v4{10,1};              //v4有2个元素，值分别为10,1
``` 

### 列表初始化一个含有string对象的vector对象
```
    vector<string> v5{"hello"};          //初始化列表，v5有一个元素
    vector<string> v6("hello");          // 错误，不能用字符串字面值构建vector对象
    vector<string> v7{10};               //v7有10个默认初始化的元素
    vector<string> v8{10,"hello"};       //v8有10个值为“hello"的元素
    //实现函数阶乘
    K
```
## 迭代器
```
vector<int>::iterator it;                   //it能读写vector<int>中的元素
string::iterator it2;                        //it2能读写string对象中的字符
vector<int>::const_iterator it3;            //it3只能读元素不能写元素
string::const_iterator it4;                 //it4只能读字符，不能写字符
```

> begin 和 end 分别返回指示容器的第一个元素和最后一个元素的下一个位置

## 结合解引用和成员访问操作
### 理解数组的引用最好的办法是从数组的名字开始按照由内向外的顺序阅读。
解引用迭代器可获得迭代器所指的对象，如果该类型恰好是类，有可能希望进一步访问他的成员。
```
int (*Parray)[10] = &arr;           //Parray指向一个含有10个整数的数组
int (&arrayRef)[10] = arr;          //arrRef引用一个含有10数组
```
> Parray是一个指针，它指向一个int型数组，数组包含10个元素
> `(&arrRef)`表示`arrRef`是一个引用，它引用的对象是一个大小为10的数组，数组中元素的类型是int。
> 
> `int *(&array)[10] = ptrs`;  //array是数组的引用，该数组含有10个指针
>按照由内向外的顺序阅读上述语句，首先array是一个引用，由右边可知，array引用的对象是一个大小为10的数组，观察左边，数组元素类型是指向int的指针，因此，array就是一个含有10个int型指针的u数组的引用。 

## 数组
### 指针和数组

> 数组还有一个特性
> >很多用到数组的di地方，编译器都会自动将其替换为一个指向数组首元素的指针：

    string *p2 = nums;                       //等价与p2 = &nums[0]

### C++ 中没有多维数组，通常所说的多维数组其实是数组的数组。

    int ia[3][4];               //大小为3的数组，每个元素是含有4个整数的数组

## 递增递减运算符

> `++i`首先将运算对象加1，将改变后的对象作为求值结果
> `i++`也会将运算对象加1，求值结果是运算对象改变之前那个值的副本

    int i = 0 , j;
    j = ++i;        //j = 1,i = 1
    int i1 = 0 ,j1;
    j1 = i1 ++；      //j1 = 0,i1 = 1

## 成员访问运算符
点运算符和箭头运算符都可以用于访问成员，两者之间的关系为：
`ptr->men`等价于`(*ptr).men`

## 函数

    int arr[10];                             //arr是一个含有10个整数的数组
    int *p1[10];                             //p1是一个含有10个指针的数组
    int (*P2)[10];                           //p2是一个指针，指向一个含有十个整数的数组
    
 ## `::`与`->`
 `::`是作用域操作符,表示引用的变量限定在该作用域内
 `->`是箭头操作符,其目的是为了简化输入,以及增强程序的可读性
 `->`的功能相当于解引用操作符`*`和成员调用操作符`.`的组合体
 `->`和`.`的实现的功能是一样的,都是访问类的成员变量或成员函数普通变量操作,`->`只能用与指针变量操作
 如:若a为一指向对象的指针,a->f(s)就表示调用a所指的对象中的成员函数f(s)
 
例如:

    class C
    {
    static int a;
    }

访问a就可以使用`C::a`来访问,表明这个变量a具有类C的作用域.它在该类内可见

    class A{
    int data;
    char key;
    }
    class *p;
    p=&A;
    //则A.data和p->data等价
    
##virtual关键字

virtual是定义在C++中虚函数的关键字
1. virtual关键字的作用
C++中的函数调用默认不适用动态绑定,触发动态绑定,必须满足两个条件:第一,指定为虚函数;第二,通过基类类型的引用或指针调用.由此可见,virtual主要功能是实现动态绑定
2. virtual关键字的使用情况
virtual可用来定义类函数和应用到虚继承
友元函数 构造函数 static静态函数 不能用virtual关键字修饰
普通成员函数和析构函数可以用virtual关键字进行修饰
3. virtual关键字效果

```C++
class GrandFather //祖父类
{
public:
GrandFather(){}  //构造函数
virtual void fun()   //虚函数声明定义
{
cout<<"GrandFather call function !"<<endl;
}
};

class Father:public GrandFather  //父类,公有继承祖父类
{
public:
Father(){}  //构造函数
void fun()   //fun函数声明定义
{
public:
Father(){}   //构造函数
void fun()    //fun函数声明定义
{
cout<<"Father call function!"<<endl;
}
};
class Son:public Father  //子类,公有继承父类
{
public:
Son(){}   //构造函数 
void fun()   //fun函数声明定义
{
cout<<"Son call function!"<<endl;
}
};

void print(GrandFather* father}   //输出函数,祖父类形参
{
father->fun();   //调用fun函数
}

int_tmain(int argc,_TCHAR* argv[])
{
Father* pfather = mew Son;   //建立一个父类的指针让它指向子类
pfather->fun();
GrandFther *pgfather = new Father;
print(pgfather);    //祖父类指针变量
return 0;
}
```
4.virtual的集成性:
只要基函数定义了virtual,继承类的该函数也就具有了vortual属性,即GrandFather, Father,Son同时定义了vitual void fun() 与vortual void fun效果是一样的.

## vitual关键字的用途：

1、vitual基类

在多重继承中，从派生类到基类存在多条路线时（多个继承脉络或者途径），一个这种派生类的对象实例化将包含多个基类对象，浪费资源且调用混乱的现象出现。

因此引入了vitual baseclass，来在运行阶段克服多个基类对象的产生。这个vitual是在运行阶段保证对象唯一性的。

2.vitual函数

虚函数的出现，是由于基类的指针可以执行派生类，因此引出了不便，引入vitual函数，来告诉编译器，出现这种情况时，在运行时动态链接进行处理。

3.vitual在纯虚函数中使用

纯虚函数完全是为了继承体系的完整，也是集成vitual函数的作用而产生的。代表了编译器阶段对象的绑定，将调用选择转移到运行时动态绑定。

综上：vitual关键的引入，可以理解为阻止编译阶段的静态绑定，将绑定（虚函数）和约束工作（虚基类）转移到运行时动态处理。


## C++中的:和::
### 3处使用::
1.使用命名空间的时候:
```C++
std::cout<<"abc"<<std::endl;
```
2.调用静态变量:

```C++
class BB
{
public:
protected:
private:
int a;
int b;
static int c;     //静态成员变量
};
//静态函数中不能使用普通成员变量 普通成员函数 
int BB::c 10;
```

3.C++实现的时候


### : 使用

1.访问修饰符private, protected,public修饰变量
private:string ageB;
2.给const和类中的对象赋值调用构造函数

```C++
class A{
private:
int a;
public:
A(int _a){
a = _a;
}
};
/**
* 构造函数初始化列表,B中类A作为成员变量,必须初始化A类
* 构造函数调用顺序: 首先调用Ａ类的构造函数，然后调用Ｂ类的构造函数
**/
class B{
private:
A a;
int b ;
const int c;
public:
B(int _b,int m):a(m),C(0)  // const变量补习这样初始化
{
b = _b;
}
};


void main(){
B b1(3,4);
system("pause");
}

```

3.在继承中使用:

```C++
class Parent:public Object{}
```