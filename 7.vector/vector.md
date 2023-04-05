# vector

[TOC]

## vector的介绍及使用

### vector的介绍

1. vector是表示可变大小数组的序列容器。
2. 就像数组一样，vector也采用连续存储空间来存储元素。也就是意味着可以采用下标对vector的元素进行访问，和数组一样高效。但是又不像数组，它的大小是可以动态改变的，而且它的大小会被容器自动处理。
3. 本质讲，vector使用动态分配数组来存储它的元素。当新元素插入的时候，为了增加存储空间，这个数组需要被重新分配大小。其做法是分配一个新的数组，然后将全部元素移到这个数组。就时间而言，这是一个相对代价高的任务，因为每当一个新的元素加入到容器的时候，vector并不会每次都重新分配大小。
4. vector分配空间策略：vector会分配一些额外的空间以适应可能的增长，因为存储空间比实际需要的存储空间更大。不同的库采用不同的策略权衡空间的使用和重新分配。但是无论如何，重新分配都应该是对数增长的间隔大小，以至于在末尾插入一个元素的时候是在常数时间的复杂度完成的。
5. 因此，vector占用了更多的存储空间，为了获得管理存储空间的能力，并且以一种有效的方式动态增长。
6. 与其它动态序列容器相比(deques,lists and forward_lists)，vector在访问元素的时候更加高效，在末尾添加和删除元素相对高效。对于其它不在末尾的删除和插入操作，效率更低。比起lists和forward_lists统一的迭代器和引用更好。

### vector的使用

#### vector的定义

|(constructor)构造函数声明|接口声明|
|-|-|
|vector()(重点)|无参构造|
|vector(size_type n,const value_type& val=value_type())|构造并初始化n个val|
|vector(const vector& x)(重点)|拷贝构造|
|vector(InputIterator first,InputIterator last)|使用迭代器进行初始化构造|

```C++{.line-numbers}
//constructing vectors
#include<iostream>
#include<vector>
int main()
{
    //constructors used in the same order as described above;
    std::vector<int> first;//empty vector of ints
    std::vector<int> second(4,100);//four ints with value 100
    std::vector<int>third(second.begin(),second.end());//iterating through second
    std::vector<int>fourth(third);//a copy of third
    //the iterator constructor can also be used to construct from arrays;
    int myints[]={16,2,77,29};
    std::vector<int>fifth(myints,myints+sizeof(myints)/sizeof(int));
    std::cout<<"The contents of fifth are:";
    for(std::vector<int>::iterator it=fifth.begin();it!=fifth.end();++it)
    {
        std::cout<<' '<<*it;
    }
    std::cout<<'\n';
    return 0;
}
```

#### vector iterator的使用

|iterator的使用|接口说明|
|-|-|
|begin+end(重点)|获取第一个数据位置的iterator/const_iterator，获取最后一个数据的下一个位置的iterator/const_iterator|
|rbegin+rend|获取最后一个数据位置的reverse_iterator，获取第一个数据前一个位置的reverse_iterator|

```C++{.line-numbers}
#include<iostream>
#include<vector>
using namespace std;
int main()
{   vector<int> s;//无参构造
    vector<int> s1(4,100);//构造并初始化4个100
    //vector<int> s2(s1.begin(),s1.end());//使用迭代器进行初始化构造
    //vector<int> s3(s1);//拷贝构造
    //cinst对象使用const迭代器进行遍历打印
    s1.push_back(1);
    s1.push_back(2);
    s1.push_back(3);
    s1.push_back(4);
    //使用迭代器进行遍历打印
    vector<int>::iterator it=s1.begin();//普通正向迭代器，可读可写
    while(it!=s1.end())
    {
        cout<<*it<<" ";
        ++it;
    }
    cout<<endl;
    vector<int>::const_iterator cit=s1.begin();//const正向迭代器，只读
    while(cit!=s1.end())
    {
        //*cit=1;错误
        cout<<*cit<<" ";
        ++cit;
    }
    //反向迭代器进行遍历打印
    vector<int>::reverse_iterator rit=s1.begin();//reverse逆置
    while(rit!=s1.rend())
    {
        cout<<*rit<<" ";
        ++rit;
    }
    cout<<endl;
    //范围for进行遍历打印->被编译器替换成迭代器方式遍历
    for(auto e:v)
    {
        cout<<e<<" ";
    }
    cout<<endl;
    return 0;
}
```

#### vector空间增长问题

|容量空间|接口说明|
|-|-|
|size|获取数据个数|
|capacity|获取容量大小|
|empty|判断是否为空|
|resize(重点)|改变vector的size|
|reserve(重点)|改变vector放入capacity|

* capacity的代码在vs和g++下分别运行会发现，vs下capacity是按1.5倍增长的，g++是按2倍增长的。这个问题经常会考察，不要固化地认为，顺序表增容都是2倍，具体增长多少是根据具体的需求定义的。vs是PJ版本STL，g++是SGI版本STL。
* reserve只负责开辟空间，如果确定知道需要用多少空间，reserve可以缓解vector增容的代价缺陷问题。
* resize在开辟空间的同时还会进行初始化，影响size。

```C++{.line-numbers}
//vector::capacity
#include<iostream>
#include<vector>
int main()
{
    size_t sz;
    std::vector<int> foo;
    sz=foo.capacity();
    std::cout<<"making foo grow:\n";
    for(int i=0;i<100;++i)
    {
        foo.push_back(i);
        if(sz!=foo.capacity())
        {
            sz=foo.capacity();
            std::cout<<"capacity changed:"<<sz<<'\n';
        }
    }
}
```

> vs运行结果:  
> making foo grow:  
> capacity changed:1  
> capacity changed:2  
> capacity changed:3  
> capacity changed:4  
> capacity changed:6  
> capacity changed:9  
> capacity changed:13  
> capacity changed:19  
> capacity changed:28  
> capacity changed:42  
> capacity changed:63  
> capacity changed:94  
> capacity changed:141  
>  
> g++运行结果:  
> making foo grow:  
> capacity changed:1  
> capacity changed:2  
> capacity changed:4  
> capacity changed:8  
> capacity changed:16  
> capacity changed:32  
> capacity changed:64  
> capacity changed:128  

增容相同空间，vs增容次数更多，效率更低，空间浪费小，g++增容次数更少，效率更高，空间浪费大。

```C++{.line-numbers}
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int main()
{
    vector<int> s;
    s.reserve(10);//把s的空间扩至10
    s.resize(10);//把s的有效位扩至10，空位用0代替，若空间不够，会先自动扩容
    s.resize(20,5)//把s的有效位扩至20，空位用5代替
    //at()函数
    vector<int> v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);
    v[4]=5;//错误，程序中断运行
    v.at(4)=5;//错误，at函数会检测是否访问越界
    //at函数与[]的区别是，at函数会检测访问是否越界
    //[]效率更高
    v.insert(v.begin(),0);
    v.erase(v.begin());
    //find函数
    //find函数存在于算法中，其头文件为algorithm
    //使用find函数查找3所在位置的iterator
    vector<int>::iterator pos1=find(v.begin(),v.end(),3);
    //在pos位置之前插入44
    v.insert(pos,44);
    //找到3的位置，删除之前插入的44
    vector<int>::iterator pos2=find(v.begin(),v.end(),3);
    if(pos2!=v.end())//若未找到，会返回end()
    {
        v.erase(pos2-1);
    }
    return 0;
}
```

#### vector迭代器失效问题

```C++{.line-numbers}
#include<iostream>
#include<vector>
using namespace std;
int main()
{
    vector<int> v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);
    //使用find查找3所在位置的iterator
    vector<int>::iterator pos=find(v.begin(),v.end(),3);
    //在pos位置之前插入44
    v.insert(pos,44);
    //删除插入的44
    v.erase(pos-1);
}

