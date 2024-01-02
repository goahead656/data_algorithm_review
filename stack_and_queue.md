# 栈和队列

栈和队列都是操作受限的线性表，最主要的区别就是运算规则不同

## 栈

栈的特性***先进后出***，用个简单的比方来说，向弹夹里压子弹的过程就是进栈的过程，子弹被射出来就是出栈的过程。最先被压入的子弹最晚被发射出来，最晚被压入的子弹，最先被发射出来。

定义：栈是受限的线性表，只能在线性表的一段进行操作（CRUD）。

存储结构：顺序栈(***数组***)和链栈(***头插法***)均可，但是顺序栈更为常见

![image-20240102195543652](D:\study\数据结构\data_algorithm_review\imgs\image-20240102195543652.png)

空栈的要求：base==top

顺序栈的代码实现:

```C++
#include <iostream>

using namespace std;

// 构造栈
typedef struct Stack_{
    int *data;
    int top;
    int size;
} Stack;

// 初始化栈
Stack *InitStack(int size){
    Stack *s=new Stack;
    s->data=new int[size];
    s->top=-1;
    s->size=size;
    return s;
}

// 判断栈是否为空
bool IsEmpty(Stack *s){
    return s->top==-1;
}

// 判断栈是否已满
bool IsFull(Stack *s){
    return s->top==s->size-1;
}

// 入栈
int Push(Stack *s,int val){
    if(IsFull(s)){
        return -1;
    }
    s->data[++s->top]=val;
    return 0;
}

// 出栈
int Pop(Stack *s){
    if(IsEmpty(s)){
        return -1;
    }
    return s->data[s->top--];
}

// 获取栈顶元素
int Top(Stack *s){
    if(IsEmpty(s)){
        return -1;
    }
    return s->data[s->top];
}

// 销毁栈
void DestroyStack(Stack *s){
    delete[] s->data;
    delete s;
}


int main()
{
    Stack *s=InitStack(10);
    Push(s,1);
    Push(s,2);
    Push(s,3);
    cout<<Top(s)<<endl;
    cout<<Pop(s)<<endl;
    cout<<Top(s)<<endl;
    // DestroyStack(s);

    return 0;
}
```

### 递归

定义：若一个对象部分包含他自己，或用他自己给自己定义，则称这个对象是递归的；若一个过程直接地或间接的调用自己，则称这个过程是递归的过程。

所有的递归问题本质上都是树的问题，并且递归算法一般都可以转换为非递归写法**迭代**。

递归使用的三种条件：

1. 递归定义的数学问题
2. 具有递归结构的数据结构
3. 可递归求解的问题

举个栗子：

```C++
long Fact(int n){
    if(n==1) return 1;
    else return n*Fact(n-1);
}
```

递归求解问题的办法：

1. 分治法：将复杂的问题拆分为简单的几个问题，并且这几个问题的解法相似，注***一定要在求解问题的时候画出问题的树结构***
   - 必须要有一个明确的递归出口
   - ![image-20240102204536849](D:\study\数据结构\data_algorithm_review\imgs\image-20240102204536849.png)

递归算法的效率分析：

- 空间效率：与递归树的深度成正比
- 时间效率：与递归树的节点数成正比

递归算法的优缺点

- 优点：结构清晰，程序易读
- 缺点：每次调用都要生成工作记录，保存装填信息，入栈和出栈操作时间开销大



## 队列

队列是一种先进后出的线性表，类似与水管一样的结构。

定义：只能在表的一端（队尾）进行插入，在另一端（队头）进行删除运算的线性表

队列的代码实现：

```	C++
#include <iostream>

using namespace std;

// 构造队列
typedef struct Queue_{
    int *data;
    int head;
    int tail;
    int size;
} Queue;

// 初始化队列
Queue *InitQueue(int size){
    Queue *q=new Queue;
    q->data=new int[size];
    q->head=0;
    q->tail=0;
    q->size=size;
    return q;
}

// 判断队列是否为空
bool IsEmpty(Queue *q){
    return q->head==q->tail;
}

// 判断队列是否已满
bool IsFull(Queue *q){
    return (q->tail+1)%q->size==q->head;
}

// 入队
int EnQueue(Queue *q,int val){
    if(IsFull(q)){
        return -1;
    }
    q->data[q->tail]=val;
    q->tail=(q->tail+1)%q->size;
    return 0;
}

// 出队
int DeQueue(Queue *q){
    if(IsEmpty(q)){
        return -1;
    }
    int val=q->data[q->head];
    q->head=(q->head+1)%q->size;
    return val;
}

// 获取队头元素
int Front(Queue *q){
    if(IsEmpty(q)){
        return -1;
    }
    return q->data[q->head];
}

// 获取队尾元素
int Rear(Queue *q){
    if(IsEmpty(q)){
        return -1;
    }
    return q->data[q->tail-1];
}

// 销毁队列
void DestroyQueue(Queue *q){
    delete[] q->data;
    delete q;
}

int main()
{
    Queue *q=InitQueue(10);
    EnQueue(q,1);
    EnQueue(q,2);
    EnQueue(q,3);
    cout<<DeQueue(q)<<endl;
    cout<<DeQueue(q)<<endl;
    cout<<DeQueue(q)<<endl;
    // DestroyQueue(q);

    return 0;
}
```

