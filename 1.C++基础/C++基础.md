# C++基础

C语言是面向过程的，关注的是过程，分析出求解问题的步骤，通过函数调用逐步解决问题。
C++是基于面向对象的，关注的是对象，将一件事拆分成不同的对象，靠对象之间的交互完成。

## C++关键字(C++98)

C++总计63个关键字，C语言总计32个关键字。

## 命名空间

定义命名空间，需要使用到namespace关键字，后面跟命名空间的名字，然后接一对{}即可，{}中即为命名空间的成员。

```C++{.line-numbers}
//普通的命名空间
namespace N1//N1为命名空间的名称
{
//命名空间中，既可以定义变量，也可以定义函数
int a;
int ADD(int left,int right)
{
    return left+right;
}
}
//命名空间可以嵌套
namespace N2
{
    namespace N3
    {
        int a;
    }
}
//同一个工程中允许存在多个相同名称的命名空间
//编译器最后会合成到一个命名空间
```

## 命名空间的使用

* 命名空间名称加作用域限定符

```C++{.line-numbers}
#include<iostream>
namespace N
{
    int a=1;
}
int main()
{
    std::cout<<N::a<<endl;
    return 0;
}
```

* 使用using将命名空间中的成员引入

```C++{.line-numbers}
#include<iostream>
namespace N
{
    int a=1;
}
using N::a;
using std::cout;
int main()
{
    cout<<a;
    return 0;
}
```

* 使用using namespace 命名空间名称引入

```C++{.line-numbers}
#include<iostream>
namespace N
{
    int a=1;
}
using namespace N;
using namespace std; 
int main()
{
    cout<<a;
    return 0;
}
```

C++库中所有内容都是放到**std**命名空间中的。

## C++输入和输出

```C++{.line-numbers}
#include<iostream>
int main()
{
    using namespace std;
    cout<<"Hello World!";
    cout<<endl;
    cout<<"This is C++!"<<endl;
    return 0;
}
```

 1. **""** 引起来的部分是要打印的消息，在C++中，用双引号括起来的一系列字符叫做字符串，因为它是由若干字符组合而成的。 
 2. **<<** 符号表示该语句将把这个字符串发送给cout，该符号指出了信息流动的路径。
 3. **cout**是一个预定义对象，知道如何显示字符串、数字、和单个字符等。
 从概念上看，输出是一个流，即从程序中流出的一系列字符。cout对象表示这种流，其属性是在**iostream**文件中定义的。cout的对象属性包括一个插入运算符<<，它可以将其右侧的信息插入到流中。
 4. 控制符**endl**，endl是一个特殊的C++符号，表示一个重要的概念：重起一行。在输出流中插入endl将使屏幕光标移到下一行开头。诸如endl等对于cout来说有特殊含义的特殊符号被称为控制符(manipulator)。和cout一样，endl也是在头文件iostream中定义的，且位于名称空间std中。
 5. 换行符 **\n** ，C++还提供了另一种在输出中指示换行的旧式方法：C语言符\n。

```C++{.line-numbers}
#include<iostream>
int main()
{
    using namespace std;
    cout<<"Hello World!\n";
    return 0;
}
```

### cout

cout也可以打印变量。

```C++{.line-numbers}
#include<iostream>
using namespace std;
int main()
{
    int a=25;
    cout<<a<<endl;
    printf("%d\n",a);
    return 0;
}
```

程序没有打印"a"，而是打印存储在变量a中的整数值，即25。实际上，这将两个操作合而为一了。首先，cout将a替换为当前值25；然后，把值转换为合适的输出字符。
与老式C语言的区别在于cout的聪明程度。在C语言中，要打印字符串"25"和整数25，可以使用C语言的多功能输出函数printf()。撇开printf()的复杂性不说，必须用特殊代码(%s和%d)来指出是要打印字符串还是整数。如果让printf()打印字符串，但又错误地提供了一个整数，由于printf()不够精密，因此根本发现不了错误。它将继续处理，显示一堆乱码。
cout的智能行为源自C++的面向对象特性。实际上，C++插入运算符<<将根据其和后面的数据类型相应地调整其行为，这是一个运算符重载的例子。

### cin

cin可以读取键盘输入。

```C++{.line-numbers}
#include<iostream>
using namespace std;
int main()
{
    int a;
    cin>>a;
    return 0;
}
```

信息从cin流向a。显然，对这一过程有更为正式的描述。就像C++将输出看作是流出程序的字符流一样，它也将输入看作是流入程序的字符流。iostream文件将cin定义为一个表示这种流的对象。输出时，>>运算符从输入流中抽取字符。通常，需要在运算符右侧提供一个变量，以接收抽取的信息(符号<<与>>被选择用来指示信息流的方向)。
与cout一样，cin也是一个智能对象。它可以将通过键盘输入的一系列字符转换为接收信息的变量能够接收的形式。

## 缺省参数

概念：缺省参数是声明或定义函数时为函数参数指定一个默认值。在调用该函数时，如果没有指定实参则采用该默认值，否则使用指定的实参。

```C++{.line-numbers}
#include<iostream>
using namespace std;
void TestFunc(int a=0)
{
    cout<<a<<endl;
}
int main()
{
    TestFunc();
    TestFunc(10);
    return 0;
}
```
>0  
>10  

### 缺省参数的分类

* 全缺省参数

```C++{.line-numbers}
void TestFunc(int a=0,int b=1,int c=2)
{
    cout<<a<<endl;
    cout<<b<<endl;
    cout<<c<<endl;
} 
```

* 半缺省参数

```C++{.line-numbers}
void TestFunc(int a,int b=1,int c=2)
{
    cout<<a<<endl;
    cout<<b<<endl;
    cout<<c<<endl;
}
```

**注意**

1. 半缺省参数必须从右往左依次来给出，不能间隔。
2. 缺省参数不能在函数声明和定义中同时出现，以声明为主。
3. 缺省值必须是常量或者全局变量。
4. C语言不支持(编译器不支持)。

## 函数重载

函数重载是函数的一种特殊情况，C++允许在同一作用域中声明几个功能类似的同名函数，这些同名函数的形参列表 **(参数个数 或 参数类型 或 顺序)** 必须不同，常用来处理实现功能类似数据类型不同的问题。

```C++{.line-numbers}
int ADD(int a,int b)
{
    return a+b;
}
double ADD(double a,double b)
{
    return a+b;
}
long ADD(long a,long b)
{
    return a+b;
}
int main()
{
    ADD(1,2);
    ADD(3.0,4.5);
    ADD(1L,2L);
    return 0;
}
```

只是函数的返回值不同是不能构成函数重载。
函数重载会根据函数参数的不同自动识别。

>#### C++如何实现函数重载？  
>
>C++的编译过程  
>1. 预处理->头文件展开、宏替换、条件编译、去注释  
>list.i test.i  
>2. 编译->检查语法、生成汇编代码  
>list.s test.s  
>3. 汇编->转成二进制的机器码  
>list.o test.o  
>4. 链接->将两个目标文件链接在一起  
>
>C语言不支持重载，因为无法区分同名函数，因此无法正常链接。  
>而C++通过函数修饰规则来区分，只要函数参数不同，修饰出来的名字就不一样，因此支持重载。  
>同时也能理解，为什么函数重载要求函数不同，而与返回值无关。  

## extern"C"

有时候在C++工程中可能需要将某些函数按照C的风格来编译，在函数前加extern"C"，意思是告诉该编译器将该函数按照C语言规则来编译。

## 引用

### 引用概念

引用不是新定义一个变量，而是给已存在的变量取了一个别名，编译器不会为引用变量开辟内存空间，它和它引用的变量**共用同一块内存空间**。

>类型&引用变量名(对象名)=引用实体；  

```C++{.line-numbers}
int a=0;
int& ra=a;
```

**注意** 

引用类型必须和引用实体是同种类型的。

### 引用特性

1. 引用在定义时必须初始化。
2. 一个变量可以有多个引用。
3. 引用一旦引用一个实体，再不能引用其它实体。

### 常引用

引用取别名时，变量访问权限可以缩小，不能放大。
常引用时可以隐式类型转换。

```C++{.line-numbers}
int a=10;
const double& ra=a;
//ra引用a时，a会先转换成一个double类型的临时变量
//这个临时变量具有常性
```

权限的缩小与放大规则适用于引用和指针。

```C++{.line-numbers}
//const修饰的变量相当于只读
//非const修饰的正常变量相当于只读+只写
//非const可以引用非const
int a=0;
int& ra=a;
//权限缩小，const可以引用非const
int b=0;
const int& rb=b;
//const可以引用const
const int c=0;
const int& rc=c;
//权限放大，非const不可以引用const
const int d=0;
int& rd=d;//错误
```

### 引用使用场景

1. 引用做参数

```C++{.line-numbers}
void swap(int& left,int& right)
{
    int temp;
    temp=left;
    left=right;
    right=temp;
}
int main()
{
    int a=1;
    int b=2;
    swap(a,b);
    //相当于left引用a，right引用b
    //直接交换a、b的值
    return 0;
}
```

2. 引用做返回值
函数返回分为传值返回与传引用返回。
传值返回会返回一个临时变量，额外分配一块内存空间。
传引用返回会引用这个返回值，不会额外分配内存空间。
传值返回会拷贝返回值，传引用返回不会拷贝返回值。
临时变量具有常性，所以非const修饰的变量无法引用函数传值返回的临时变量。

```C++{.line-numbers}
int Count1()
{
    static int n=0;
    n++;
    return n;
}
int& Count2()
{
    static int n=0;
    n++;
    return 0;
}
int mian()
{
    int& r1=Count1();//错误
    const int& r2=Count1;
    //传值返回会返回一个临时变量
    //这个临时变量具有常性
    int& r3=Count2();
    //传引用返回会直接返回n的引用
    //此时n不具有常性
    return 0;
}
```

如果返回变量是一个局部变量时，引用返回是不安全的。

```C++{.line-numbers}
#include<iostream>
int& ADD(int a,int b)
{
    int c=a+b;
    return c;
}
int main()
{
    int& ret=ADD(1,2);
    return 0;
}
```

原函数运行结束时，所开辟的临时空间销毁，临时变量也随之销毁，而ret仍引用该空间。
可以用static来修饰这个局部变量。
一个函数要使用引用返回，返回变量出了这个函数的作用域还存在，就可以使用引用返回，否则就不安全。
函数使用引用返回可以少创建一个临时变量，提高效率，引用传参同理。

```C++{.line-numbers}
#include<iostream>
#include<time.h>
using namespace std;
struct A
{
    int a[10000];//40000byte
};
A a;
A TestFunc1()//传值返回
{
    return a；
}
A& TestFunc2()//传引用返回
{
    return a;
}
void main()
{
    size_t begin1=clock();
    for(size_t i=0;i<1000000;i++)
        TestFunc1();
    size_t end1=clock();
    size_t begin2=clock();
    for(size_t i=0;i<1000000;i++)
        TestFunc2();
    size_t end2=clock();
    cout<<"TestFunc1 time:"<<end1-end2<<endl;
    cout<<"TestFunc2 time:"<<end1-end2<<endl;
}
```

>TestFunc1 time:1504  
>TestFunc2 time:3  

### 引用和指针的区别

在**语法概念**上，引用就是一个别名，没有独立空间，和其引用实体共用同一块空间。
在**底层实现**上，引用实际是有空间的，因为引用是按照指针方式实现的。
1. 引用概念上定义一个变量的别名，指针存储一个变量地址。
2. 引用在定义时必须初始化，指针没有要求。
3. 引用在初始化时引用一个实体后，就不能再引用其它实体，而指针可以在任何时候指向任何一个同类型实体。
4. 没有NULL引用，但有NULL指针。
5. 在sizeof中含义不同：引用结果为引用类型的大小，但指针始终是地址空间所占字节个数。
6. 引用自加即引用的实体增加1，指针自加即指针向后偏移一个类型的大小。
7. 有多级指针，但没有多级引用。
8. 访问实体方式不同，指针需要显示解引用，引用编译器自己处理。
9. 引用比指针使用起来相对更安全。

## 内联函数

概念：以inline修饰的函数叫做内联函数，编译时C++编译器会在调用内联函数的地方展开，没有函数压栈的开销，内联函数会提升程序运行的效率。
函数在调用时需建立栈帧，影响效率，C语言中可以使用宏函数解决，C++可以使用内联函数解决。
如果在函数前增加inline关键字将其改成内联函数，在编译期间编译器会用函数体替换函数的调用。
查看方式：
1. 在release模式下，查看编译器生成的汇编代码中是否存在"call 函数"。
2. 在debug模式下，需要对编译器进行设置，否则不会展开(因为debug模式下，编译器默认不会对代码进行优化)。

### 内联函数特性

1. inline是一种以空间换时间的做法，省去调用函数的额外开销。所以代码很长或者有循环/递归的函数不适合用作内联函数。
2. inline对于编译器而言只是一个建议，编译器会自动优化，如果定义为inline的函数体内有循环/递归等，编译器优化时会忽略掉内联。
3. inline不建议声明和定义分离，分离会导致链接错误。因为inline被展开，就没有函数地址，链接就会找不到。
4. 内联函数本质是宏。

### 宏的优缺点

**优点**

1. 增加代码的复用性。
2. 提高性能。

**缺点**

1. 不方便调试宏(因为预编译阶段进行了替换)。
2. 导致代码可读性差，可维护性差，容易误用。
3. 没有类型安全的检查。

## auto关键字

### auto简介

在早期C/C++中auto的含义是使用auto修饰的变量，是具有自动存储器的局部变量，但遗憾的是一直没有人去使用它。
C++11中，标准委员会赋予了auto全新的含义即：auto不再是一个存储类型指示符，而是作为一个新的类型指示符来指示编译器，auto声明的变量必须由编译器在编译时期推导而得。使用auto来进行优化，可以简化代码的写法。

```C++{.line-numbers}
#include<iostream>
using namespace std;
int main()
{
    int a=10;
    auto b=a;//b的类型是根据a的类型推导出来的
    auto c="a";
    cout<<typeid(a).name()<<endl;
    cout<<typeid(b).name()<<endl;
    cout<<typeid(c).name()<<endl;
    return 0;
}
```

>int  
>int  
>char const * __ptr64  

```C++{.line-numbers}
auto a=1,b=2;
auto c=3,d=4.0;//错误
//该行代码会报错，因为c、d推导出的类型不同
```

### auto不能推导的场景

1. auto不能作为函数的参数类型。
2. auto不能直接用来声明数组。
3. 为了避免与C++98中的auto发生混淆，C++11只保留了auto作为类型指示符的用法。

## 基于范围for循环(C++11)

对于一个有范围的集合而言，由程序员来说明循环的范围是多余的，有时候还会容易犯错误。因此C++11中引入了基于范围for的循环。for循环的参数列表由冒号":"分为两部分：第一部分是范围内用于迭代的变量；第二部分则表示被迭代的范围。
与普通循环类似，可以用continue结束本次循环，也可以用break来跳出整个循环。

```C++{.line-numbers}
#include<iostream>
using namespace std;
int main()
{
    int array[]={1,2,3,4,5};
    //C语言方法
    for(int i=0;i<sizeof(array)/sizeof(int);i++)
        array[i]*=2;
    for(int i=0;i<sizeof(array)/sizeof(int);i++)
        printf("%d",array[i]);
    printf("\n");
    //C++方法
    for(auto e:array)
        e*=2;
    for(auto e:array)
        cout<<e;
    cout<<endl;
    return 0;
}
```

>246810  
>246810  

使用C++方式的范围for循环并未改变原数组，原因是范围for循环时，会将数组中的数据依次付给变量e，因此实际改变的是变量e，而非数组。
若想改变数组，可以将e的类型改为引用。

```C++{.line-numbers}
#include<iostream>
using namespace std;
int main()
{
    int array[]={1,2,3,4,5};
    //C语言方法
    for(int i=0;i<sizeof(array)/sizeof(int);i++)
        array[i]*=2;
    for(int i=0;i<sizeof(array)/sizeof(int);i++)
        printf("%d",array[i]);
    printf("\n");
    //C++方法
    for(auto& e:array)
        e*=2;
    for(auto e:array)
        cout<<e;
    cout<<endl;
    return 0;
}
```

>246810  
>48121620  

### 范围for的使用条件

1. 范围for循环迭代的范围必须是确定的。
   对于数组而言，就是数组中第一个元素到最后一个元素的范围；对于类而言，应该提供begin和end的方法，begin和end就是for循环迭代的范围。
2. 迭代的对象要实现++和==的操作。

## 指针空值nullptr

### C++98中的指针空值

在良好的C/C++编程习惯中，声明一个变量时最好给该变量一个合适的初始值，否则可能会出现不可预料的错误，比如未初始化的指针。如果指针没有合法的指向，我们基本都是按照如下方式对其进行初始化。

```C++{.line-numbers}
int* p1=NULL;
int* p2=0;
```

>#define NULL ((void*)0)  

NULL实际是一个宏，NULL可能被定义为字面常量0，或者被定义为无类型指针(void*)的常量。无论采取何种定义，在使用空值的指针时，都不可避免会遇到一些麻烦。

```C++{.line-numbers}
#include<iostream>
using namespace std;
void Func(int n)
{
    cout<<"整型"<<endl;
}
void Func(int*p)
{
    cout<<"整型指针"<<endl;
}
int main()
{
    Func(0);
    Func(NULL);
    Func(nullptr);
    return 0;
}
```

>整型  
>整型  
>整型指针  

程序本意是想通过NULL调用指针版本的Func(int*)函数，但是由于NULL被定义成0，因此与程序的初衷相悖。
在C++98中，字面常量0既可以是一个整型数字，也可以是无类型的指针(void*)常量，但是编译器默认情况下将其看成是一个整型常量，如果要将其按照指针方式来使用，必须对其进行强制类型转换(void*)0。

**注意**

1. 在使用nullptr表示指针空值时，不需要包含头文件，因为nullptr是C++11作为新关键字引入的。
2. 在C++11中，sizeof(nullptr)与sizeof((void*)0)所占的字节数相同。

为提高代码的健壮性，表示指针空值时建议最好使用nullptr。
