**递归**

- 递归是否是循环逻辑 (circular logic)?
  + 递归调用反复进行到基准情形出现  base case
- 1. base case  不需要递归就能求解
  2. 不断推进 making progress 需要递归的解 总是朝着产生base case的方向推进
  3. 设计法则 design rule 假设所有的递归调用都能运行
  4. 合成效益法则 compound interest rule 在求解一个问题的同一个实例时，
     切勿在不同的递归调用中做重复的工作

**定义**

- 如果存在正常数 c和n0使得当N>=n0时T(N)<=cf(N), 则记为T(N)=O(f(N))

- 如果存在正常数c和n0使得当N>=n0时T(N)>=cg(N), 则记为T(N)=Ω(g(N))

- T(N)=Θ(h(N))当且仅当T(N)=O(h(N))且T(N)=Ω(h(N))

- 如果T(N)=O(p(N))且T(N)!=Θ(p(N)), 则T(N)=o(p(N))

  O(f(N))	Ω(g(N))	Θ(h(N))	o(p(N))

  *chapter 2.1*

相对增长率( relative rate of growth )

**法则**

+ 如果T1(N) = O(f(N)) 且 T2(N) = O(g(N)), 那么
  + T1(N) + T2(N) = max( O(f(N)), O(g(N)) )
  + T1(N) * T2(N) = O( f(N) * g(N) )
+ 如果T(N)是一个 k 次多项式，则T(N) = Θ(N^k)
+ 对任意常数 k, log^kN = O(N)。它告诉我们对数增长的非常缓慢

洛必达法则 L'Hopital' s rule

**导数**

导数与积分 是一对互逆的操作

**一般法则**

---

sizeof(type)
> 该操作符的结果类型为size_t
> 它在头文件中定义为: typedef unsigned int size_t;
> 该类型保证能容纳实现所建立的最大对象的字节大小.

**栈**
```C
/* 栈ADT链表实现的类型声明 */
#ifndef _Stack_h

struct Node;
typedef struct Node *PtrToNode;
typedef PtrToNode Stack;

int IsEmpty(Stack S);
Stack CreateStack( void );
void DisposeStack( Stack S);
void MakeEmpty( Stack S );
void Push( ElementType X, Stack S);
ElementType Top( Stack S );
void Pop( Stack S );

#endif /*_Stack_h*/

/* Place in implementation file */ 
/* Stack implementation is a linked list with a header*/ 
struck Node
{
    ElementType Element;
    PrtToNode Next;
}

/*测试是否空栈*/
int
IsEmpty( Stack S )
{
    return S->Next == Null;
}

/* 创建空栈 */
Stack
CreateStack( void )
{
    Stack S;

    S = malloc(sizeof( struct Node ));
    if ( S == NULL)
        FatalError("Out of space!!");
    S->Next == NULL;
    MakeEmpty( S );
    return S;
}

void
MakeEmpty( Stack S )
{
    if ( S == NULL )
        Error("Must use CreateStack first");
    else
        while(!IsEmpty(S))
            pop(S);
}

/* Push */
void
Push( ElementType X, Stack S )
{
    PtrToNode TmpCell;

    TmpCell = malloc(sizeof(struct Node));
    if( TmpCell == NULL )
        FatalError("Out of space");
    else
    {
        TmpCell->Element = X;
        TmpCell->Next = S->Next;
        S->Next = TmpCell;
    }
}

/* Top */
ElementType
Top( Stack S )
{
    if( !IsEmpty( S ))
        return S->Next->Element;
    Error("Empty stack");
    return 0; /*Return value used to avoid warning */
}

/* Pop */
void
Pop( Stack S )
{
    PtrToNode FirstCell;

    if( IsEmpty(S))
        Error("Empty stack");
    else
    {
        FirstCell = S->Next;
        S->Next = S->Next->Next;
        free( FirstCell);
    }
}

```

**数组实现 栈**
每一个栈有一个*TopOfStack*,对于空栈它是*-1*(这就是栈的初始化)
法则：程序的任何部分都没有存取 被每个栈蕴含的数组或 栈顶(TopOfStack)变量的可能
```C
/* 用数组实现栈 */
```

栈的应用：
    平衡符号
    后缀表达式
        抛弃优先级法则，只进行栈操作
    中缀到后缀的转换
        定义优先级 ()优先级最高 其次* 最低为 +-
        遇到操作符当当前操作符比TopOfStack优先级大时，压入栈，否则弹出
        例外：当遇到最高 ）时将栈中'('到')'操作符全部弹出
        
    函数调用