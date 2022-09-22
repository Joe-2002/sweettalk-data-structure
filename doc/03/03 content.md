# 第三章：线性表

## 一、线性表的定义

1.规范定义：线性表（List)：零个或多个数据元素的有限序列。

2.定义通俗理解

- 组成成员：有限个数的元素，可以是0个，可以是多个

- 组成结构：有一定顺序的队列

- 如下图形象的理解

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209201947085.png)

星座就是一个线性结构，有以下特性

- 除了头部和尾部，其他成员左右，**只且只有一个成员紧邻**。在线性表中，位于当前元素左边的称为直接前驱元素，同理，右边的称为直接后驱元素

- 元素的个数就是线性表的长度，星座是12个，所以星座线性表的长度是12。当一个成员都没有的时候，线性表的长度为0，称为**空表**

## 二、线性表的抽象数据类型

1.在复杂的线性表中，组成线性表的元素有多个组成部分，这些部分叫做数据项

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209201957781.png)

学号是有序的序列，代表不同的学生，姓名和性别这些，就是组成学生的特征属性，也就是数据项

2.线性表的抽象数据类型

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209201959384.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209201959162.png)

- 基本用法

```c
/*将所有的在线性表Lb中但不在La中的数据元素插入到La中*/
void unionL(SqList *La,SqList Lb)
{
	int La_len,Lb_len,i;
	ElemType e;                        /*声明与La和Lb相同的数据元素e*/
	La_len=ListLength(*La);            /*求线性表的长度 */
	Lb_len=ListLength(Lb);
	for (i=1;i<=Lb_len;i++)
	{
		GetElem(Lb,i,&e);              /*取Lb中第i个数据元素赋给e*/
		if (!LocateElem(*La,e))        /*La中不存在和e相同数据元素*/
			ListInsert(La,++La_len,e); /*插入*/
	}
}
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202024178.png)





## 三、线性表的物理存储方式

线性表有两种存储结构：顺序存储结构，链式存储结构



### 1.顺序存储结构



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202028083.png)



我们用一维的数组来实现顺序存储结构，即把第一个数据元素存储到下标0的位置，接着把其他元素存在相邻的位置，这样每个位置的元素都有对应的下标，从而能快速的查询。

```c
#define MAXSIZE 20          /* 存储空间初始分配量 */
typedef int ElemType;       /* ElemType类型根据实际情况而定，这里为int */
typedef struct
{
	ElemType data[MAXSIZE]; /* 数组，存储数据元素 */
	int length;             /* 线性表当前长度 */
}SqList;
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202034282.png)

数组的长度相当于提前预定好的空间，元素长度为线性表的实际长度，即入驻了多少个元素。线性表的长度为元素个数，是要小于等于数组长度。

#### 1.1 地址

**地址：因为是以一维数组来做空间，数组都是带有下标的，所以每个存储元素都有自己的编号，这个编号就叫做地址。**

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202038921.png)



有一个元素的地址，就可以计算其他元素的地址

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202041910.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202041709.png)



#### 1.2元素的查找

```c
#define OK 1
#define ERROR 0
/* Status是函数的类型,其值是函数结果状态代码，如OK等 */
typedef int Status;         

/* 初始条件：顺序线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：用e返回L中第i个数据元素的值，注意i是指位置，第1个位置的数组是从0开始 */
Status GetElem(SqList L,int i,ElemType *e)
{
	if(L.length==0 || i<1 || i>L.length)
		return ERROR;
	*e=L.data[i-1];

	return OK;
}
```

#### 1.3 元素的插入操作

插入实现过程

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202047900.png)

```c

/* 初始条件：顺序线性表L已存在,1≤i≤ListLength(L)， */
/* 操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1 */
Status ListInsert(SqList *L,int i,ElemType e)
{ 
	int k;
	if (L->length==MAXSIZE)  			/* 顺序线性表已经满 */
		return ERROR;
	if (i<1 || i>L->length+1)			/* 当i比第一位置小或者比最后一位置后一位置还要大时 */
		return ERROR;				

	if (i<=L->length)        			/* 若插入数据位置不在表尾 */
	{
		for(k=L->length-1;k>=i-1;k--)  	/* 将要插入位置后的元素向后移一位 */
			L->data[k+1]=L->data[k];
	}
	L->data[i-1]=e;          			/* 将新元素插入 */
	L->length++;

	return OK;
}
```



#### 1.4 元素的删除操作

删除实现过程

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202048311.png)



```c

/* 初始条件：顺序线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：删除L的第i个数据元素，并用e返回其值，L的长度减1 */
Status ListDelete(SqList *L,int i,ElemType *e) 
{ 
	int k;
	if (L->length==0)               /* 线性表为空 */
		return ERROR;
	if (i<1 || i>L->length)         /* 删除位置不正确 */
		return ERROR;
	*e=L->data[i-1];
	if (i<L->length)                /* 如果删除不是最后位置 */
	{
		for(k=i;k<L->length;k++)	/* 将删除位置后继元素前移 */
			L->data[k-1]=L->data[k];
	}
	L->length--;
	return OK;
}
```



#### 1.5顺序存储结构的优缺点



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202051694.png)







### 2.链式存储结构



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202052749.png)



顺序结构的缺点在于插入和删除，需要移动大量的元素，耗费时间。

为了解决这个整体移动的情况，我们用一组任意的存储单元来存储数据元素，这些存储位置可以是连续的，也可以是不连续的，但是我们要保证元素之间的直接前驱元素和直接后继元素能够有关联且唯一，即存储对应的地址。



#### 2.1组成部分

数据域：存储数据元素信息的域叫做数据域

指针域：存储直接后继元素位置的域叫做指针域

结点：数据域+指针域组合



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202059030.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202100042.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202102222.png)



#### 2.2注意事项

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202102903.png)

#### 2.3链式结构代码解释

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202104523.png)



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202105429.png)



```c
/* 线性表的单链表存储结构 */
typedef struct Node
{
	ElemType data;
	struct Node *next;
}Node;
typedef struct Node *LinkList; /* 定义LinkList */

```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202105128.png)





## 四、各种各样的链表



### 1.单链表

#### 1.1单链表的读取

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202110468.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202110726.png)           

```c
/* 初始条件：链式线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：用e返回L中第i个数据元素的值 */
Status GetElem(LinkList L,int i,ElemType *e)
{
	int j;
	LinkList p;		/* 声明一结点p */
	p = L->next;		/* 让p指向链表L的第一个结点 */
	j = 1;		/*  j为计数器 */
	while (p && j<i)  /* p不为空或者计数器j还没有等于i时，循环继续 */
	{   
		p = p->next;  /* 让p指向下一个结点 */
		++j;
	}
	if ( !p || j>i ) 
		return ERROR;  /*  第i个元素不存在 */
	*e = p->data;   /*  取第i个元素的数据 */
	return OK;
}


```



#### 1.2单链表的插入

```c

s->next = p->next;    /* 将p的后继结点赋值给s的后继  */
p->next = s;          /* 将s赋值给p的后继 */
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202112975.png)



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202114823.png)

<img src="https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202114690.png" title="" alt="" width="650">

```c
/* 初始条件：链式线性表L已存在,1≤i≤ListLength(L)， */
/* 操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1 */
Status ListInsert(LinkList *L,int i,ElemType e)
{ 
	int j;
	LinkList p,s;
	p = *L;   
	j = 1;
	while (p && j < i)     				/* 寻找第i个结点 */
	{
		p = p->next;
		++j;
	} 
	if (!p || j > i) 
		return ERROR;   				/* 第i个元素不存在 */

	s = (LinkList)malloc(sizeof(Node)); /* 生成新结点(C语言标准函数) */
	s->data = e;  
	s->next = p->next;    				/* 将p的后继结点赋值给s的后继 */
	p->next = s;          				/* 将s赋值给p的后继 */
	return OK;
}
```



#### 1.3单链表的删除



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202116402.png)



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202117669.png)

```c
/* 初始条件：链式线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：删除L的第i个数据元素，并用e返回其值，L的长度减1 */
Status ListDelete(LinkList *L,int i,ElemType *e) 
{ 
	int j;
	LinkList p,q;
	p = *L;
	j = 1;
	while (p->next && j < i)	/* 遍历寻找第i个元素 */
	{
		p = p->next;
		++j;
	}
	if (!(p->next) || j > i) 
		return ERROR;           /* 第i个元素不存在 */
	q = p->next;
	p->next = q->next;			/* 将q的后继赋值给p的后继 */
	*e = q->data;               /* 将q结点中的数据给e */
	free(q);                    /* 让系统回收此结点，释放内存 */
	return OK;
}
```



#### 1.4单链表的整表创建

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202125844.png)

```c
/*  随机产生n个元素的值，建立带表头结点的单链线性表L（头插法） */
void CreateListHead(LinkList *L, int n) 
{
	LinkList p;
	int i;
	srand(time(0));                         /* 初始化随机数种子 */
	*L = (LinkList)malloc(sizeof(Node));
	(*L)->next = NULL;                      /* 先建立一个带头结点的单链表 */
	for (i=0; i<n; i++) 
	{
		p = (LinkList)malloc(sizeof(Node)); /* 生成新结点 */
		p->data = rand()%100+1;             /* 随机生成100以内的数字 */
		p->next = (*L)->next;    
		(*L)->next = p;						/* 插入到表头 */
	}
}
```



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202126944.png)



```c
/*  随机产生n个元素的值，建立带表头结点的单链线性表L（尾插法） */
void CreateListTail(LinkList *L, int n) 
{
	LinkList p,r;
	int i;
	srand(time(0));                     	/* 初始化随机数种子 */
	*L = (LinkList)malloc(sizeof(Node)); 	/* L为整个线性表 */
	r=*L;                                	/* r为指向尾部的结点 */
	for (i=0; i<n; i++) 
	{
		p = (Node *)malloc(sizeof(Node)); 	/* 生成新结点 */
		p->data = rand()%100+1;           	/* 随机生成100以内的数字 */
		r->next=p;                        	/* 将表尾终端结点的指针指向新结点 */
		r = p;                            	/* 将当前的新结点定义为表尾终端结点 */
	}
	r->next = NULL;                       	/* 表示当前链表结束 */
}
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202127849.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202128606.png)



#### 1.5单链表的整表删除

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202128591.png)



```c
/* 初始条件：链式线性表L已存在。操作结果：将L重置为空表 */
Status ClearList(LinkList *L)
{ 
	LinkList p,q;
	p=(*L)->next;           /*  p指向第一个结点 */
	while(p)                /*  没到表尾 */
	{
		q=p->next;
		free(p);
		p=q;
	}
	(*L)->next=NULL;        /* 头结点指针域为空 */
	return OK;
}
```



#### 1.6单链表与顺序存储结构的对比

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202130496.png)



- 若线性表需要频繁查找，很少进行插入和删除操作，宜采用顺序存储结构

- 若线性表中的元素个数变化比较大或者根本不知道有多大时，最好用单链表结构



### 2.静态链表

#### 2.1定义

对于没有指针的高级语言，用数组来代替指针描述单链表。数组的元素都是由两个数据域组成，data和cur，cur就相当于指针，我们叫做游标。就把这种用数组描述的链表叫做**静态链表**，或者**游标实现法**



```c
#define MAXSIZE 1000 	/* 存储空间初始分配量 */

/* 线性表的静态链表存储结构 */
typedef struct 
{
	ElemType data;
	int cur;  			/* 游标(Cursor) ，为0时表示无指向 */
} Component,StaticLinkList[MAXSIZE];
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202137931.png)

```c
/* 将一维数组space中各分量链成一个备用链表，space[0].cur为头指针，"0"表示空指针 */
Status InitList(StaticLinkList space) 
{
	int i;
	for (i=0; i<MAXSIZE-1; i++)  
		space[i].cur = i+1;
	space[MAXSIZE-1].cur = 0; 	/* 目前静态链表为空，最后一个元素的cur为0 */
	return OK;
}
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202138003.png)

#### 2.2静态链表的插入

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202139645.png)

```c
/* 若备用空间链表非空，则返回分配的结点下标，否则返回0 */
int Malloc_SSL(StaticLinkList space) 
{ 
	int i = space[0].cur;           		/* 当前数组第一个元素的cur存的值 */
											/* 就是要返回的第一个备用空闲的下标 */
	if (space[0]. cur)         
		space[0]. cur = space[i].cur;       /* 由于要拿出一个分量来使用了， */
											/* 所以我们就得把它的下一个 */
											/* 分量用来做备用 */
	return i;
}
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202141851.png)



```c
#缺代码


```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202143636.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202144843.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202144637.png)



#### 2.3静态链表的删除

```c
/*  删除在L中第i个数据元素   */
Status ListDelete(StaticLinkList L, int i)   
{ 
	int j, k;   
	if (i < 1 || i > ListLength(L))   
		return ERROR;   
	k = MAXSIZE - 1;   
	for (j = 1; j <= i - 1; j++)   
		k = L[k].cur;   
	j = L[k].cur;   
	L[k].cur = L[j].cur;   
	Free_SSL(L, j);   
	return OK;   
} 
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202149337.png)

```c
/*  将下标为k的空闲结点回收到备用链表 */
void Free_SSL(StaticLinkList space, int k) 
{  
	space[k].cur = space[0].cur;    /* 把第一个元素的cur值赋给要删除的分量cur */
	space[0].cur = k;               /* 把要删除的分量下标赋值给第一个元素的cur */
}
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202147307.png)

```c
/* 初始条件：静态链表L已存在。操作结果：返回L中数据元素个数 */
int ListLength(StaticLinkList L)
{
	int j=0;
	int i=L[MAXSIZE-1].cur;
	while(i)
	{
		i=L[i].cur;
		j++;
	}
	return j;
}
```



#### 2.4静态链表的优缺点

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202150585.png)

### 3.循环链表

循环链表简图如下

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202151058.png)

- 空表循环



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202152698.png)

- 非空链表循环

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202152347.png)

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202154724.png)

```c
p=rearA->next;   			    /* 保存A表的头结点，即① */
rearA->next=rearB->next->next;	/* 将本是指向B表的第一个结点（不是头结点）*/
                 				/* 赋值给reaA->next，即② */
q=rearB->next;
rearB->next=p;				   	/* 将原A表的头结点赋值给rearB->next，即③ */
free（q）;					   	/* 释放q */

```





### 4.双向链表



![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202156548.png)

```c
/*线性表的双向链表存储结构*/
typedef struct DulNode
{
		ElemType data;
		struct DuLNode *prior;    	/*直接前驱指针*/
		struct DuLNode *next;		/*直接后继指针*/
} DulNode, *DuLinkList;
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202157529.png)

```c
p->next->prior = p = p->prior->next
```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202158449.png)

```c
s - >prior = p;   			/*把p赋值给s的前驱，如图中①*/
s -> next = p -> next;		/*把p->next赋值给s的后继，如图中②*/
p -> next -> prior = s;		/*把s赋值给p->next的前驱，如图中③*/
p -> next = s;				/*把s赋值给p的后继，如图中④*/

```

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202159522.png)

```c
p->prior->next=p->next;   	/*把p->next赋值给p->prior的后继，如图中①*/
p->next->prior=p->prior;	/*把p->prior赋值给p->next的前驱，如图中②*/
free（p）;					/*释放结点*/

```

### 5.线性表总结

![](https://raw.githubusercontent.com/shishenshashen/block_picture/master/imgs/202209202202189.png)


