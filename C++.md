# 指针



## 1、一个指针在32位系统里占4字节，在64位系统里占8字节。

 

 

## 2、空指针：指针变量指向内存中编号为0的空间(列：int * p= NULL;)

 



## 3、野指针：指针变量指向非法的内存空间。



 空指针和野指针都不是我们自己申请的空间，因此不要访问。

 

## 4、Const修饰指针三种情况: (const : 常量; * : 指针)



 **如果一个变量被const修饰，那么它的值就不能再被改变**

 

 

- const  int * p=….;-----常量指针，是指**指针指向的内容是常量**。指针的指向可以更改，但是指针指向的值不能修改。

 

 

- int * const p=….;-----指针常量，是指**指针本身是个常量**，**不能在指向其他的地址**。指针的指向不可以更改，指针指向的值可以更改。

 

 

- const int * const  p=…;-----修饰指针（修饰常量）,指针的指向和指针的值都不可以更改。



 

# 指针和函数



地址传递可以改变实参的值(利用指针指向实参的地址)，值传递不会。

如：

​     void swap(int *p1, int *p2)

{

​             int temp = * p1;

​            * p1 = * p2;

​            * p2 = temp;

}

 

​     int main()

{

​       int a=10;

​       int b=20;

swap(&a,&b);

}

此时，主函数里的实参 a 和 b 就会交换数值。

### 

 

 

# 结构体

##        1. struct student s1;

​        如：

​             s1.name="...";

​             s1.age=...;

​             s1.score=....;



##        2.struct student s2={....};

​         如：

​             struct student s2={"李四",19,80};



##        3.在定义结构体时顺便创建结构体变量

.         如：

​              struct student

{

string name;

int age;

int score;

}s3;

s3.name="王五";

s3.age=20;

s3.score=60;







## 结构体指针



### 1.创建学生结构变量



​        struct student 

{

string name;

int age;

int score;

};

student s = {"张三",18,100};



### 2.通过指针指向结构体变量



student * p =&s;



### 3.通过指针访问结构体变量中的数据



cout  << "姓名:" << p->name << "年龄:" << p->age << "分数:" << p-> score <<  endl;



通过**结构体指针**访问结构体中的属性，需要利用"->".





# 分区



## 代码区：



存放函数体的二进制代码，由操作系统进行管理的



特点：**共享** ，**只读**



## 全局区：



存放全局变量和静态变量以及常量，程序结束后数据由操作系统释放



### 全局变量：写在函数外的变量，都是全局变量（如在main函数上面的变量）



### 静态变量：在普通变量前面加static，属于静态变量



### 常量：



##### 字符串常量：如：“Hello,world”



##### const修饰的常量:



###### const修饰的全局变量



###### const修饰的局部变量



**global:全局；local:局部**



![](C:\Users\GSL\Desktop\QQ截图20230801194426.png)



## 栈区：



由编译器自动分配释放，存放函数的参数值，局部变量等

### 局部变量：写在函数内的变量，都是局部变量（包括main函数）



注意：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放



 



## 堆区：



由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收





# new用法



## new基本语法



int * p = new int (10);             ==**new返回是该数据类型的指针**==

return p; 



如果想释放堆区的数据，利用关键字delete

如：delete p;



## 在堆区用new开辟数组



int * arr = new int[10]；   ==10 代表数组里有10个元素==



释放数组：

如：delete[] arr;   释放数组的时候，要加[]才可以









# 引用



## 语法：数据类型 &别名 = 原名

如：int a=10;



​        int &b = a;



​        b=20;            ==此时a是20==



## 引用必须要初始化



如：  int &b; ==是错的==



**引用一旦初始化后，就不可以更改引用了**



 

## 引用做函数参数



引用可以让形参修饰实参，改变实参的值



如：

void myswap(int &a, int &b)

{

int temp =  a;

a = b;

b = temp;

}



int main()

{

int a = 10;

int b = 20;

myswap(a,b);

}





## 引用做函数返回值



**不要返回局部变量的引用**



### 函数的调用可以作为左值



int & test()



{



static int a = 10;          ==**静态变量**。存在全局区，在**程序结束后系统释放**==



return a;



}



int main()

{

int &ref = test()



cout << "ref = " << ref << endl;    ==此时ref是10==



test() = 1000;               ==如果**函数的返回值是引用**，这个函数调用**可以作为左值**（等号左边）==



cout << "ref = " << ref << endl;  此时ref是1000

}





## 引用的本质



引用在C++中其实是一个**指针常量** (指向不可更改，值可以更改)



## 常量引用



主要用来**修饰形参**，防止误操作



const int & ref = 10;



相当于：



int temp = 10;

const int & ref = temp;



ref的值**不可修改**





# 函数高级



## 函数默认参数



### 如果某个位置参数有默认值，那么从这个位置以后，从左往右，必须要有默认值



如：

void func(int a = 10,int b = 20,int c = 30);

a 有默认值，后面的 b 和 c 都要有默认值



### 如果函数声明有默认值，函数实现时候就不能有默认值



如：

int func(int a = 10,int b = 20);   ==这是函数的声明部分==



int func(int a,int b)   ==这是函数的实现部分==

{

......;

}



==函数**两个部分只能有一个有默认值**，哪怕两个部分默认值都一样==





## 函数的占位参数



void func(int a,int )     ==后面变量名为空，给这个参数占位==  

{

...;

}



int main()

{

func(10,10);     ==参数类型要对应得上，值可以正常传递==

...;

}



还可以有默认参数，如：

 void func(int a,int =10);





## 函数重载



### 概念：函数名可以相同，提高复用性



### 函数重载满足条件：



- 同一个作用域下



- 函数名称相同



- 函数参数 ==**类型不同**== 或者 **==个数不同==** 或者 ==**顺序不同**==



**注意**： 函数的返回值不可以作为函数重载条件



如：int func(int a,int b) 和 void func(int a,int b) 这两个函数的**==返回值不同==**，不能作为重载的条件







void func()

{

cout << "func的调用" << endl;

}



void func(int a )

{

cout << "func (int a ) 的调用" << endl;

}



这种情况下，两个函数作用域，名字都相同，第一个函数没有参数，第二个函数有一个 int型 参数，运行func()，结果是第一个函数；运行func(10)，结果是第二个函数







# 类和对象



C++面向对象的三大特性：==封装==、==继承==、==多态==



C++认为万事万物皆为对象：人可以是对象，有年龄，身高，体重......；车可以是对象，有轮胎，方向盘，车灯......

具有**相同性质的对象**，我们称之为==类==，人属于人类，车属于车类



## 封装



###  封装的意义一



- 将属性和行为作为一个整体，表现生活中的事物



- 将属性和行为加以权限控制





#### 在设计类时，属性和行为写在一起，表现事物



==**语法**==：class 类名{ 访问权限:  属性 / 行为 }；



如：



class circle                          ==求一个圆的周长==

{

public:



​     int  r;                                 ==圆的半径==



​     double calculate()

​     {

​           return 2 * PI * r;       ==返回圆的周长==

​     }

};





int main()

{

circle c1;      ==创建一个叫c1的圆（对象）==



c1.r = 10;     ==给圆的半径赋值为10==



cout << "圆的周长为:" << c1.calculate() << endl;



}



###  封装意义二



#### 将属性和行为放在不同的权限下，加以控制



##### 三种访问权限



- public               ==公共权限==



成员 类内可以访问，类外也可以访问



- protected        ==保护权限==



成员 类内可以访问，类外不可以访问(儿子可以访问父亲中的保护内容)



- private             ==私有权限== 



成员 类内可以访问，类外不可以访问(儿子不可以访问父亲的私有内容)

 

如：



class person

{

public:

string m_name;          ==姓名==



protected:

string m_car;               ==汽车==



private:

int m_password;         ==密码==



public:

void func()

{

m_name = "张三";

m_car = "车";

m_password = "123";         ==在类内可以访问protected 和 private 内容,对其进行初始化以及更改==

}

}；



**整个 class person 大括号里面都叫类内**



int main()

{

person p1;                              ==创建一个对象p1==

p1.m_name = "李四";            ==public 内容，类外可以更改==

p1.m_car = "汽车";                 ==此步错误，protected 内容，类外不可访问==

p1.m_password = "456"；   ==此步错误，private 内容，类外不可访问==

}







### struct 和 class 的区别



#### struct 默认权限是 公共 public



#### class 默认权限是 私有 private





### 成员属性设置为私有



#### 优点一：将所有成员属性设置为私有，可以自己控制读写权限



#### 优点二：对于写权限，可以检测数据的有效性



 在类内，可以在 public 中用函数关联 private 或 protected 中的信息，这样就可以间接地改变 private 或 protected 中的信息，还可以控制信息的可读或可写





## 对象的初始化和清理



###  构造函数和析构函数



**如果人不提供构造和析构，编译器会提供。编译器提供的构造函数和析构函数是空实现。**



构造函数：主要作用于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，不需要手动调用



析构函数：主要作用于对象**销毁前**系统自动调用，执行一些清理工作



#### 构造函数语法：类名(){}



- 构造函数，没有返回值也不写void



- **函数名称与类名相同**



- 构造函数可以有参数，因此**可以发生重载**



- 程序在调用对象时会自动调用构造函数里的内容，不用手动调用，而且只会调用一次





#### 析构函数语法：~类名(){}



- 析构 函数，没有返回值也不写void



- 函数名称与类名相同，在名称前加上符号~



- 析构函数不可以有参数，因此**不可以发生重载**



- 程序在调用对象时会自动调用析构函数里的内容，不用手动调用，而且只会调用一次



 ==切记：构造函数和析构函数是在 类 ( class )里的==



每调用一次构造函数，其对应的析构函数就会随之调用，调用几次构造函数就会自动调用几次析构函数，二者相互对应



- 如果连续调用同一个构造函数，结果会是一次构造函数，一次析构函数，来回往复：ab,ab,ab......( a：构造函数；b：析构函数)



- 如果连续调用不同的构造函数，结果会是各个析构函数按就近原则运行如同多个连续的 if 和 else ：ab,ba,ab,ba......( a：构造函数；b：析构函数)





### 构造函数的分类及调用



#### 两种分类方式：

##### 按参数分为：**有参构造** 和 **无参构造**(默认构造)

##### 按类型分为：**普通构造** 和 **拷贝构造**



**拷贝构造函数：**

class person

{

public:

person()

{

age = 18;

}

person ( const person &p )              ==括号内为 const + 构造函数名 + 引用==

{

age = p.age;                                     ==将传入的函数身上的所有属性，拷贝到当前函数身上==

}

int age;

};





#### 三种调用方式：

括号法

显示法

隐式转换法



##### 括号法：

void test()

{

person p1;          ==默认构造函数（无参构造函数）的调用==

person p2(10);   ==有参构造函数的调用==

person p3(p2);   ==拷贝函数的调用==

}



调用**默认构造函数**时，**不要加()**，要不然会被认为是一个函数的声明



##### 显示法：

person p1;

person p2 = person(10);     ==调用有参构造==

person p3 = person(p2);     ==调用拷贝构造==



person(10);    ==匿名对象，特点：执行完之后，立即释放(即使main函数未执行完)，即一次性物体==



不要利用拷贝构造函数，初始化匿名对象

如：

person(p3);               ==此步错误，p3是拷贝函数，放在括号内即初始化匿名对象==

编译器会认为  person (p3) === person (p3);





##### 隐式转换法

person p4 = 10;         ==相当于写了   person p4 = person (10); 有参构造==

person p5 = p4;         ==拷贝构造==





### 拷贝构造函数调用时机



class person

{

public:

int m_age;

person(int a)

{

m_age = a;

...;

}

~person()

{

...;

}

person(const person &p)

{

m_age = p.m_age;

...;

}

};



#### 1.使用一个已经创建完毕的对象来初始化一个新对象

  void test()

{

person p1(20);

person p2(p1);                  ==此时拷贝构造函数p2的 age 为 p1 的20==

}

#### 2.值传递的方式给函数参数传值

void dowork(person p)

{

​                       ==dowork函数内什么也没有==

}



void test1()

{

person (p);         ==调用一次默认无参构造函数==

dowork(p);         ==此时会将实参 p 通过值传递传递给 dowork形参 p ,**中间会调用拷贝构造函数**==

}



**简而言之，值传递会调用拷贝构造函数**



#### 3.以值方式返回局部对象

person dowork2()

{

person p1;       ==创建局部对象==

return p1;         ==**return会拷贝一个新的 p1 ，中间会调用拷贝函数**==

}



void test2()

{

person p = dowork2();         

}





### 构造函数调用规则

默认情况下，C++编译器至少给一个类添加3个函数

1. 默认无参构造函数
2. 默认析构函数
3. 默认拷贝构造函数，对属性进行拷贝



#### 如果用户定义有参构造函数，C++不会提供无参构造函数，但是会提供默认拷贝构造

 

#### 如果用户定义拷贝构造函数，C++不会提供其他构造函数





### 深拷贝和浅拷贝



#### 浅拷贝：简单的复制拷贝操作



上面所有的拷贝都是浅拷贝



#### 深拷贝：在堆区重新申请空间，进行拷贝操作



person()

{

public:

person(int age,int height)

{

m_age = age;

m_height = new int(height);       ==用new创建堆区数据，用指针接收==

}

~person()                ==析构代码可以将堆区开辟的数据释放==

{

if (m_height != NULL)

{

delete m_height;

m_height = NULL;

}

...;

}

int *m_height;        ==用身高指针来接受数据==

int m_age;

}



void test()

{

person p1(18,160)

{

cout << "p1年龄为：" << p1.m_age << "身高为：" << *p1.height << endl;

}

person p2(p1);

cout << "p2年龄为：" << p2.m_age << "身高为：" << *p2.height << endl;

}

在test函数里，编译器自带的拷贝会使指针height指向的**堆区new的数据被重复释放两次**，因此程序会崩溃



浅拷贝带来的问题就是**堆区的内存重复释放**





#### 深拷贝：在堆区重新申请空间，进行拷贝操作

让两个指针指向地址不一样（另起炉灶）

person(const person &p)

{

...;

m_age = p.m_age;

//m_height = p.m_height;           ==这一步错误，会让两个指针指向同一个地址，进而在析构函数里重复释放导致程序崩溃==

m_height = new int (*p.m_height);          ==让每个height指针都分别指向新建立的一个堆区的地址，地址上的值就是要拷贝的值，从而实现每个指针指向的地址不一样，二者互不干扰，不会使同一块内存重复释放==

}



**总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来问题**




### 初始化列表



作用：提供初始化列表语法，用来初始化属性



**语法：构造函数():属性1(值1)，属性2(值2)...{}**



如：



class person

{

==传统方式初始化:==

person(int a , int b, int c)

{

m_a = a;

m_b = b;

m_c = c;

}



==初始化列表初始化属性:==

person(int a,int b,int c) : m_a(a), m_b(b), m_c(c){}    ==更方便简捷赋值，不占地==



private:

int m_a;

int m_b;

int m_c;

};



void test()

{



person p(10,20,30);



cout << "m_a = " << a << "m_b = " << b << "m_c = " << c << endl;



}


