# 线性表

定义：若结构是非空有限集，则有且仅有一个开始节点和一个终端节点，并且所有节点都最多只有一个直接前趋和一个直接后继。

线性表的表达式：[$a_1$,$a_2$,$a_3$,..............,$a_n$]

线性表的特点：

- 只有一个首节点和尾节点
- 除去首尾节点外，其他节点只有一个前驱和直接后继

线性结构反映了节点间的逻辑是一对一的线性结构，线性结构通常包括线性表（其中**链表**也属于线性表），堆栈，队列，字符串，数组等等。

## 顺序存储

最经典的表达是数组，数组存储的地址空间是连续的，连续索引的空间是在一起的。

```C++
#数组创建，初始化
int arr[100];

#数据增加
arr[cur_index]=new_data;
  
#将index位置的数据删除
arr[index]=0;
#如果index正好是数组的最后一个位置,执行以下代码可以直接完成数据的删除
index--;

#更改index位置的数据
arr[index]=change_data;
    
#查询index位置的数据
int a=arr[index];
```

也可以使用指针构造顺序结构，像链表那样,使用一个结构体就可以实现

```C++
#include <iostream>
#include <string>

using namespace std;

typedef struct ArrayList_{
    int *arr;
    int size;
    int MaxSize;
}ArrayList;

int InitSqList(ArrayList &L){
    L.arr = new int[20];
    L.size = 0;
    L.MaxSize = 20;
    return 0;
}

// 数据的插入操作
int InsertSqList(ArrayList &L, int i, int e){
    if(i<1 || i>L.size+1){
        return -1;
    }
    if(L.size==L.MaxSize){
        return -1;
    }
    for(int j=L.size;j>=i;j--){
        L.arr[j]=L.arr[j-1];
    }
    L.arr[i-1]=e;
    L.size++;

    return 0;
}

int DeleteSqList(ArrayList &L, int i){
    if(i<1 || i>L.size){
        return -1;
    }
    // 从目标位置向后移动挪动元素
    for(int j=i;j<L.size;j++){
        L.arr[j-1]=L.arr[j];
    }
    L.size--;
    return 0;
}

int LocateSqList(ArrayList &L, int e){
    // 遍历查找元素的位置
    for(int i=0;i<L.size;i++){
        if(L.arr[i]==e){
            return i+1;
        }
    }
    return -1;
}

void ForeachSqList(ArrayList &L){
    for(int i=0;i<L.size;i++){
        cout<<L.arr[i]<<" ";
    }
    cout<<endl;
}

// 工具类，使用这些工具类可以更加方便的使用顺序表
int GetSqList(ArrayList L, int i){
    if(i<1 || i>L.size){
        return -1;
    }
    return L.arr[i-1];
}

int LengthSqList(ArrayList L){
    return L.size;
}

int EmptySqList(ArrayList L){
    if(L.size=0){
        return 1;
    }
    return 0;
}

int FullSqList(ArrayList L){
    if(L.size==L.MaxSize){
        return 1;
    }
    return 0;
}

int DestorySqList(ArrayList &L){
    delete []L.arr;
    L.arr = NULL;
    L.size=0;
    L.MaxSize=0;
    return 0;
}

int main(){
    // 实例化的是一个对象，而不是一个指针，这个对象包含了指针和其中数组的大小
    ArrayList L;
    InitSqList(L);
    // InsertSqList(*L,0,1);
    InsertSqList(L,1,2);
    InsertSqList(L,2,3);
    InsertSqList(L,3,4);
    // int a=GetSqList(L,1);
    // cout<<a<<endl;

    DeleteSqList(L,2);

    // for(int i=0;i<L->size;i++){
    //     cout<<L->arr[i]<<" ";
    // }
    ForeachSqList(L);


    return 0;
}

```



## 链式存储

链式存储就必须要使用到指针了，在链式存储中，数据之间的存储是不连续的.

在链表中设置头节点的好处:

1. 便于首元节点的处理
   - 首元节点的地址保存在头节点的指针域中,所以在链表的第一个位置上的操作而和其他位置上的一致,无须进行特殊处理.
2. 便于空表和非空表的统一处理
   - 无论链表是否为空,头指针都是指向头节点的非空指针,因此空表和非空表的处理也就统一了.

链表的优缺点:

- 优点:
  - 数据元素的个数可以自由扩充
  - 插入,删除等操作不必移动数据,只需修改链接指针,修改效率较高
- 缺点
  - 存储密度小
  - 存取效率不高,必须采取顺序存取,即存取数据元素时,只能按链表的顺序进行访问

链式存储的代码实现:

```C++
#include <iostream>
#include <cmath>

using namespace std;

/***
 * 注：链表的操作必须使用一个临时指针，否则会导致链表的断裂（单向链表和双向链表均相同）
 * 临时指针可以增加链表的便捷性，不会影响链表的结构，但是如果一旦改动链表的头节点，那将是灾难性的
 * 
*/

// 定义节点
typedef struct Node_{
    int data;
    struct Node_ *next;

    Node_(int val){
        data=val;
        next=NULL;
    }
} Node;


// 查询链表中指定位置的数值
int SelectLinkedList(Node *L,int i){
    // 临时指针
    Node *p=L;
    int j=0;

    while(p && j<i){
        p=p->next;
        j++;
    }
    if(!p || j>i){
        return -1;
    }
    return p->data;
}

// 增加一个节点
int InsertLinkedList(Node *L,int i,int val){
    Node *p=L;
    int j=0;

    while(p && j<i-1){
        p=p->next;
        j++;
    }
    if(!p || j>i-1){
        return -1;
    }

    Node *s=new Node(val);
    s->next=p->next;
    p->next=s;
    return 0;
}

// 删除一个节点
int DeleteLinkedList(Node *L,int i){
    Node *p=L;
    int j=0;

    while(p && j<i-1){
        p=p->next;
        j++;
    }
    if(!p || j>i-1){
        return -1;
    }
    p->next=p->next->next;
    return 0;
}

// 改变链表中的数值
int UpdateLinkedList(Node *L,int i,int val){
    Node *p=L;
    int j=0;

    while(p && j<i){
        p=p->next;
        j++;
    }
    if(!p || j>i){
        return -1;
    }

    p->data=val;

    return 0;
}

// Foreach遍历链表
int ForeachLinkedList(Node *L){
    Node *p=L;
    while(p){
        p=p->next;
        cout<<p->data<<endl;   
    }
    return 0;
}



int main()
{
    Node *L=new Node(-1);
    InsertLinkedList(L,1,1);
    InsertLinkedList(L,2,2);
    InsertLinkedList(L,3,3);
    InsertLinkedList(L,4,4);
    UpdateLinkedList(L,2,5);
    DeleteLinkedList(L,3);
    ForeachLinkedList(L);


    return 0;
}
```

## 顺序表和链表的区别

![image-20231230195733068](D:\study\数据结构\data_algorithm_review\imgs\image-20231230195733068.png)

## 思考题

请使用链表求解两个一元多次方程的求和:
$$
A_{17}(x)=7+3x+9x^8+5x^17\\B_8(x)=8x+22x^7-9x^8
$$
下面给出解决问题的思路:

设置节点Node结构体

```C++
typedef struct Node_{
    double arg;  #存储系数
    int expo;    #存储指数
    Node_ *next;
} Node;
```

思路:(注意:全程使用临时指针,不要私自动头节点)

1. 将方程转换为两个有序链表
2. 对两个有序链表进行排序
3. 指数相同的进行加和处理,不同的按照指数的大小顺序进行排序
4. 最后将结果输出