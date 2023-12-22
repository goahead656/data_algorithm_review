# 数据结构复习

### 前置基础知识

电子计算机的早期用途：

- 早期：数值计算
- 后期：处理逐渐扩大到非数值计算领域，能处理多种复杂的具有一定结构关系的数据

其中人机问题和文件结构主要涉及了树的知识。其中染色问题是图的相关知识，这里只提出相关知识，后面会对相关知识进行详细介绍

那么如何才能设计出合适的数据结构和算法呢。首先要考虑对相关的各种信息如何表示和存储

数据结构课程的研究内容为研究**非数值计算的程序设计问题**中计算机的**操作对象以及他们之间的关系和操作**。

数据结构的两个层次：

- 逻辑结构：数据元素之间的抽象化的相互关系，与数据的存储无关，独立于计算机，他是从具体问题抽象出来的数学模型
  - 划分方法一
    - **线性结构**：**线性表、栈、队列**、串（一般来说很少用）
    - **非线性结构**：**树，图**。其中树是比较常用的数据结构，我们常用的文件系统就是基于树这种数据结构，但其实树是图的一种特殊形式，即树是一种无环（没有循环或回路）的连通图（即任意两个顶点之间都存在路径）
  - 划分方法二
    - **集合**
    - **线性结构**
    - **树形结构**
    - **图形结构**
- 存储结构（物理结构：实实在在的文件系统，可以看见的）：数据元素及其关系在计算机存储器中的存储方式
  - **顺序存储结构**：借助元素在存储器中的相对位置来表示数据元素间的逻辑关系。简而言之：***主要是连续的数据存储地址***
  - **链式存储结构**：借助指示元素存储地址的指针表示数据元素间的逻辑关系。简而言之：***主要是通过指针对数据元素进行存储***

数据结构的基础操作包括增删改查，也就是常说的crud，还有一个对于考试来说很重要的排序算法，几乎是实验课的必考内容

## 数据类型

定义：在一种程序设计语言中，变量所具有的数据种类

对于课上的C语言：涉及到基本数据类型有**`char`,`int`,`float`,`double`,`void`,`string`**，构造数据类型主要包括：数组，结构体，共用体，文件。

抽象数据类型：

- 由用户自定义，用以表示应用问题的数据模型
- 由基本的数据类型组成，并包括一组的相关的操作

抽象数据类型可以通过固有的数据类型（整型，浮点型）来表示和实现。或者也可以使用C/C++语言中独有的结构体来定义

接下来来看下结构体的用法：

```c++
struct Person
{
	# 定义了一些属性
    private:
        int age, height;
        double money;
        string books[100];
	#定义了一些方法
    public:
        string name;
		#没有返回值的方法
        void say()
        {
            cout << "I'm " << name << endl;
        }
		#set和接下来的get方法，属于面向对象的内容啦，不考的话不需要了解
        int set_age(int a)
        {
            age = a;
        }
		#因为是int，所以需要设置返回值，返回的数值也是int类型的
        int get_age()
        {
            return age;
        }

        void add_money(double x)
        {
            money += x;
        }
} person_a, person_b, persons[100];
#最后面的是对于对象的实例化
```

其中对于内存的动态分配与释放，内存的分配一般是使用`new`关键字，内存的释放使用`delete`关键字。

return表示函数的结束，一般来说主方法中最后都是需要`return 0`。在循环中一般使用的是`break`关键字来结束

一些扩展函数

- min和max：用于查找最大最小值

## 算法与算法分析

定义：一个有穷的指令集，这些指令为解决某一特定任务规定了一个运算序列

特性

- 输入
- 输出
- **确定性**
- **有穷性**
- **可行性**

对算法的评价包括以下方面

- 正确性
- 可读性
- 健壮性
- 高效性（主要涉及时空复杂度）

时空复杂度：

- 时间复杂度：一般直接看程序的执行次数就可以，找到最关键的语句，统计执行次数，内外循环如果没有明确标明的话，直接把按照循环n次处理，具体的话，可以看你们的PPT，你们老师写的还是挺详细的（好吧，我承认是我偷懒啦，突然有点事情，所以这部分省略了些），时间复杂度只需要关注执行次数最多的那个就可以，执行次数少的可以略作忽略。
- 空间复杂度：一般来说，直接数组开的大小，在数据操作的过程中会不会重新申请新的空间以及申请了多少空间。

## 开始入门数据结构

对于不是计算机专业的同学来说，数据结构考核一般不会很难，而且数据结构一般都是从C/C+++的基础知识演变过来，接下来开始介绍数据结构

### 运算符的优先级

注意：

1. 运算的产生只能是在一个变量上。例如`int x=5`其中x就是变量，可以对x进行操作
2. 其中前置运算是先运算，后引用。后置运算是先引用，后运算。

### 循环

举例：求解s=1+2+3+....+100,请写出s的答案

以下是使用while-do的代码，while-do有可能一次也不执行

```C++
#include <iostream>

using namespace std;

int main()
{
    int s=0;
    int i=0;
    while(i<=100){
        s=s+i;
        i=i+1;
    }
    printf("%d",s);
    
    return 0;
}
```

接下来是do while的程序，其中do while是先执行后判断，肯定会执行一次

```C++
#include <iostream>

using namespace std;

int main()
{
    int s=0;
    int i=1;
    do{
        s=s+i;
        i=i+1;
    }while(i<=100);
    printf("%d",s);
    
    return 0;
}
```

无限循环

```C++
#include <iostream>

using namespace std;

int main()
{
    while(true){
        # 内容部分循环执行
    }
}
```



![image-20231218195154414](D:\study\数据结构\image-20231218195154414.png)



```C++
#include <iostream>

using namespace std;

int main()
{
    int i,j,k,c;
    for(i=1;i<=9;i++){
        for(j=0;j<=9;j++){
            if(i!=j){
                k=i*1000+i*100+j*10+j;
                	for(c=31;c*c<k;c++){
                        if(c*c==k) printf("%d",k);
                    }
            }
        }
    }
    return 0;
}
```

### 字符串的比较

strcmp的使用，strcmp使用主要是用来比较字符串。[strcmp(str1,str2)](https://cplusplus.com/reference/cstring/strcmp/),点击链接即可访问该网址，找到函数的用法。

### 数组

求一维数组和二维数组的极值，直接循环一次来找到所有位置中的最大值，可以将初始值设置为数组开头的第一个数值，然后遍历求解最大值

与数组紧密相关的就是排序算法，如何考试是机试的话，可以直接只用以下代码进行排序

```C++
#include <iostream>
#导入额外的库函数
#include <algorithm>

using namespace std;

int main()
{
    int arr[5];
    sort(arr,arr+5);
    #接下来就可以直接输入数组啦
    
    return 0;
}
```

进一步了解排序算法，我会单独再写一个文件

### 指针

指针的用法颇为复杂，一般考试也不会对指针进行过多的考察
