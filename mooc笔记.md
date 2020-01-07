## 第一章

##### 数据结构

​		存储、组织数据的方式.

##### 算法 

​		一个有限指令集，接受输入，产生输出，有限步骤后终止，每一条指令目的明确、计算机计算范围内.

​		空间复杂度S(n)	占用存储单元的长度.

​		时间复杂度T(n)	耗费时间的长度.
$$
T(N)=O(f(n)),存在C>0,n_0>0,使得n \geq n_0时,T(n) \leq C·f(n)
$$

$$
T(N)=\Omega (g(n)),存在C>0,n_0>0,使得n \geq n_0时,T(n) \geq C·g(n)
$$

$$
T(N)=\theta (h(n)),同时有T(N)=O(f(n)) 和 T(N)=\Omega (g(n))
$$

$$
T_1(n)+T_2(n)=max(O(f_1(n)),O(f_2(n)))
$$

$$
T_1(n)*T_2(n)=O(f_1(n)*f_2(n))
$$

$$
若T(n)是关于n的k阶多项式，T(n)=\theta(n^k)
$$

for循环的T(n)=循环次数循环体代码复杂度

if-else结构的复杂度取决于-----if的条件判断复杂度,两个分支部分的复杂度,取三者最大

最大子列和问题:遍历T(n)=O(N^3) ->O(N^2)(前一基础上再取和) -> O(NlogN)分而治之 

​							->O(N)在线处理

```c
#include<stdio.h>
#include<time.h>
#define MAXN 100

clock_t		start, stop;
double		duration;

void printN(int N)
{	/*int i;
	for (i = 1; i <= N; i++) {
		printf("%d\n", i);
	}*/
	if (N) {
		printN(N - 1);
		printf("%d\n",N);
    }
}
int main()
{	start = clock();
	int N=20;
	//scanf_s("%d",&N);
	for ( int i = 0; i <= MAXN; i++)
	{
		printN(N);
	}
	stop = clock();
	duration = ((double)(stop - start)) / CLK_TCK/MAXN;
	printf("ticke1 = %f\n", (double)(stop - start));
	printf("duration1 = %6.2e\n", duration);
	//system("pause");
	return 0;
}
```

## 第二章

### 2.1 线性表及其实现

​		多项式的表示

一元多项式：
$$
f(x)=a_0+a_1x+···+a_n-_1x^n-^1+a_nx^n
$$
主要运算：多项式相加、相减、相乘等

方法1：顺序存储结构直接表示

方法2：顺序存储结构表示非零项

方法3：链表结构存储非零项

<img src="mooc笔记.assets/image-20200107152545530.png" alt="image-20200107152545530" style="zoom:50%;" />

线性表：同类型数据元素构成有序序列的线性结构

​			长度，空表，表头、表尾

线性表的抽象数据类型描述

<img src="mooc笔记.assets/image-20200107152820368.png" alt="image-20200107152820368" style="zoom:50%;" />

#### 线性表的顺序存储实现(利用数组的连续存储空间顺序存放线性表的各元素)

```c
typedef struct{
    ElenmentType Data[MAXSIZE];
	int Last;
}List;
List L,*Ptrl
```

访问下标为i的元素:L.Data[i]或Ptrl->Data[i]

线性表的长度:L.Last+1或Ptrl ->Last+1

##### 主要操作的实现

###### 1.初始化

```c
List *MakeEmpty()
{	List *PtrL
	Ptrl = (List *)malloc(sizeof(List));
 	Ptrl ->Last=-1;
 	return Ptrl;
}
```

###### 2.查找

```c
int Find(ElementType X,List *Ptrl)
{	int i=0;
	while(i<=Ptrl -> Last && Ptrl -> Data[i]!=X )
	i++;
	if(i>Ptrl -> Last)	return -1;
	else return i;
}
```

###### 3.插入(第i个位置上插入一个值为X的新元素)

```c
void insert(ElementType X,int i,List *Ptrl)
{	int j;
 	if(Ptrl -> Last == MAXSIZE-1){
        printf("表满");
        return;
    }
 	if(i<1||i>Ptrl -> Last+2){
        printf("位置不合法");
        return;
    }
 	for(j=Ptrl -> Last;j>=i-1;j--)
        Ptrl -> Data[j+1] = Ptrl -> Data[j];
 	Ptrl -> Data[i-1]=X;
 	Ptrl -> Last++;
 	return;
}
```

###### 4.删除(删除表的第i个位置上的元素)

```c
void Delete(int i,List *Ptrl)
{	int j;
 	if(i<1 || i>Ptrl -> Last+1){
        printf("不存在第%d"个元素,i);
        return;
    for(j=i;j<=Ptrl->Last;j++)
        Ptrl -> Data[j-1] = Ptrl -> Data[j];
    Ptrl -> Last--;
    return;
}
```

#### 线性表的链式存储实现

##### 主要操作的实现

###### 1.求表长

```c
int Length ( List *PtrL )
{	List *p = PtrL; 		/* p指向表的第一个结点*/
	int j = 0;
	while ( p ) {
		p = p->Next;
		j++; 				/* 当前p指向的是第 j 个结点*/
	}
	return j;
}
```

###### 2.查找

按序号查找

```c
List *FindKth( int K, List *PtrL )
{ 	List *p = PtrL;
	int i = 1;
	while (p !=NULL && i < K ){
		p = p->Next;
		i++;
	}
	if ( i == K ) return p;
			/* 找到第K个，返回指针 */
	else return NULL;
			/* 否则返回空 */
}
```

按值查找

```c
List *Find( ElementType X, List
*PtrL )
{
	List *p = PtrL;
	while ( p!=NULL && p->Data != X )
		p = p->Next;
	return p;
}
```

###### 3.插入

```c
List *Insert( ElementType X, int i, List *PtrL )
{ 	List *p, *s;
	if ( i == 1 ) { 						/* 新结点插入在表头 */
		s = (List *)malloc(sizeof(List)); 	/*申请、填装结点*/
        s->Data = X;
        s->Next = PtrL;
		return s; 							/*返回新表头指针*/
	}
    p = FindKth( i-1, PtrL ); 				/* 查找第i-1个结点 */
    if ( p == NULL ) { 						/* 第i-1个不存在，不能插入 */
        printf(＂参数i错＂ );
        return NULL;
    }else {
        s = (List *)malloc(sizeof(List)); 	/*申请、填装结点*/
        s->Data = X;
        s->Next = p->Next;				/*新结点插入在第i-1个结点的后面*/
        p->Next = s;
        return PtrL;
}
```

###### 4.删除

```
List *Delete( int i, List *PtrL )
{	List *p, *s;
    if ( i == 1 ) { 			/* 若要删除的是表的第一个结点 */
        s = PtrL; 				/*s指向第1个结点*/
        if (PtrL!=NULL) PtrL = PtrL->Next; /*从链表中删除*/
        else return NULL;
        free(s); 				/*释放被删除结点 */
        return PtrL;
    }
    p = FindKth( i-1, PtrL ); 	/*查找第i-1个结点*/
    if ( p == NULL ) {
    	printf(“第%d个结点不存在”, i-1); return NULL;
    } else if ( p->Next == NULL ){
    	printf(“第%d个结点不存在”, i); return NULL;
    } else {
        s = p->Next; 			/*s指向第i个结点*/
        p->Next = s->Next; 		/*从链表中删除*/
        free(s); 				/*释放被删除结点 */
        return PtrL;
}
```

#### 广义表(Generalized List)  

​	1.广义表是线性表的推广
​	2.对于线性表而言， n个元素都是基本的单元素；
​	3.广义表中，这些元素不仅可以是单元素也可以是另一个广义表。  

```
typedef struct GNode{
	int Tag; 			/*标志域： 0表示结点是单元素， 1表示结点是广义表 */
    union { 		/* 子表指针域Sublist与单元素数据域Data复用，即共用存储空间 */
        ElementType Data;
        struct GNode *SubList;
    } URegion;
    struct GNode *Next; /* 指向后继结点 */
} GList;
```

#### 多重链表

多重链表： 链表中的节点可能同时隶属于多个链
		多重链表中结点的指针域会有多个，如前面例子包含了Next和SubList两个指针域；
		但包含两个指针域的链表并不一定是多重链表，比如在双向链表不是多重链表。
多重链表有广泛的用途：基本上如树、图这样相对复杂的数据结构都可以采用多重链表方式实现存储。  

