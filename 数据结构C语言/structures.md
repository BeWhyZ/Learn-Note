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

**伸展树(slapy tree)**
具有最坏情形运行时间O(N)但保证对任意M次连续操作最多花费O(M*logN)运行时间的查找树数据结构确实可以满意了，因为不存在坏的操作序列

基本思想： 当一个节点被访问的时候，他就要经过一些列AVL树的旋转被放到根节点上。当节点过深时，要求重新构造因具有平衡这棵树的作用。

展开操作（slapying）:
    如果X的父节点是树根，只需要旋转X与树根，否则X一定有父节点P与祖父节点G
    之字形：X是P的右节点或者左节点时，P是G的左儿子或者右儿子
        操作：进行一次像AVL树样的双旋转
    一字形：X与P都是G的左儿子或者右儿子
        操作：将左边的树变成右边的树，或者将右边的树变成左边

每个操作绝不会落后O(logN)这个时间：我们总是遵守这个时间，即使偶尔有不良操作。

删除操作：

1. 访问要被删除的节点。使被删除节点变为根。
2. 删除该节点，剩下两颗子树TL 与 TR
3. 找到TL中最大的元素，将其推到根处。(由于TL树有一个没有右儿子的根)
4. TR作为TL树该根的 右儿子。

**树的遍历**
```
/* 按照顺序列出所有关键字 */
void 
PrintTree(SearchTree T)
{
    if(T != NULL)
    {
        PrintTree(T->left);
        PrintElement(T->element);
        PrintTree(T->Right);
    }
}
```

中序遍历：首先遍历左子树，然后是当前节点，最后遍历右子树。总运行时间为：O(N)。  
后序遍历：在一个节点处的工作使它诸多儿子节点完成后才进行的。总运行时间为：O(N)  
先序遍历：在诸多儿子节点被处理前，先处理当前节点。  
层序遍历(level-order traversal)：所有深度为D的节点要在深度为D+1的节点之前进行处理。（不是递归实现，用到队列，而不是递归所默示的栈）  

**B-树**  

这种查找树不是二叉树。  
阶为M的B-树是一颗具有一下结构特性的树:

+ 树的根或者是一片树叶，或者其儿子数在2和M之间。
+ 除根外，所有非树叶节点的儿子数在[M/2]和M之间。
+ 所有的树叶都在相同的深度上。
+ 所有的数据都存储在树叶上。

4阶B-树 2-3-4树。  
3阶B-数 2-3树。    其中节点内有两个信息 例如：(18:-)， - 表示该节点只有两个儿子。  
B-树的深度最多是 `O(log_[M/2](N))`
最坏运行时间为：`O(log_M(N)) = O((M/logM)logN)` 
经验指出：从运行时间考虑，M的最好(合法的)  选择是M=3或M=4,他指出当M再增大时插入和删除的时间就会增加。  

**散列**  
散列表抽象数据类型(hash table abstract data type)  
散列表的实现叫做散列(hashing)。散列是一种用于以常数平均时间执行插入、删除和查找的技术。 

Horner法则计算多项式：  
```C
    h_k = k_1 + 27k_2 + 27^2k_3 
        = ((k_3) * 27 + k_2) * 27 + k_1  
```

当一个元素插入时另一个元素以及存在(散列值相同),那么就产生一个冲突。解决冲突的方法(最简单的两个)：分离链接法，开放定址法。  

**分离链接法(separate chaining)**  
其做法是将散列到同一个值的所有元素保留到一个表中。  
