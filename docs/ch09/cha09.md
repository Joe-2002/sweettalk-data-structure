[TOC]

## 大话数据结构—第九章：排序

### 9.1开场白

​		这个就不多赘述了，因为觉得书中将的非常的生动有趣，如果说我个人再来编写一个的话，我觉得就像我们上学的时候的排队，为了队伍的没美观我们会，老师会按照身高将我们进行排序，这其实也是个很生动的排序的例子，然后我们的生活中有很多这样的例子，大家可以自行去发现。

### 9.2排序的基本概念与分类

​		排序的严格意义:

​		**假设含有n个记录的序列为{r~1~ ,r~2~ …,r~n~ }，其相应的关键字分别为{k~1~ ,k~2~ …，k~3~ }，需确定1,2,…,n的一种排列p~1~,p~2~…,p~n~，使其相应的关键字满足${k}_{p_{1}}$≤${k}_{p_{2}}$,≤…≤${k}_{p_{n}}$.非递减（或非递增）关系，即使得序列成为一个按关键字有序的序列{${r}_{p_{1}}$, ${r}_{p_{2}}$,…，${r}_{p_{n}}$}，这样的操作就称为排序。**

​		读起来其实有点拗口，也有点难以理解，那就用几个例子来打个比方，排序是我们生活中经常会面对的问题。同学们做操时会按照从矮到高排列;老师查看上课出勤情况时,会按学生学号顺序点名;逛某宝时，习惯性的升序价格来看商品（不是）。

​		注意我们在排序问题中,通常将数据元素称为记录。显然我们输入的是一个记录集合，输出的也是一个记录集合，所以说，可以将排序看成是线性表的—种操作。 

​		排序的依据是关键字之间的大小关系，那么，对同一个记录集合,针对不同的关键字进行排序,可以得到不同的序列。

#### 9.2.1排序的稳定性

​		也正是由于排序不仅是针对主关键字，那么对于次关键字，因为待排序的记录序列中可能存在两个或两个以上的关键字相等的记录。排序结果可能会存在不唯—的情况，我们给出了稳定与不稳定排序的定义。

​		 假设k~i~＝k~j~（1≤i≤n,1≤j≤n,i≠j）,且在排序前的序列中r~i~领先于r~j~（即i≤j）。如果排序后r~i~仍领先于r~j~,则称所用的排序方法是稳定的;反之,若可能使得排序后的序列中r~j~ 领先于r~i~，则称所用的排序方法时不稳定的。

#### 9.2.2内排序与外排序

​		根据在排序过程中待排序的记录是否全部被放置在内存中，排序分为内排序和外排序。

​		***内排序***是在排序整个过程中,待排序的所有记录全部被放置在内存中。***外排序***是由于排序的记录个数太多,不能同时放置在内存中，整个排序过程需要在内存之间多次交换数据才能进行。

​		**而本书主要讲述的是内排序的方法**

​		对于内排序来说，排序算法的性能主要受三个方面的影晌:

![image-20220921001858358](scr\image-20220921001858358.png)

##### 		1.时间性能

​		排序是数据处理中经常执行的一种操作，往往属于系统的核心部分，因此排序算法的时间开销是衡量其好坏的最重要的标志。。在内排序中，主要进行两种操作:比较和移动。而高效率的内排序算法应该是具有尽可能少的关键字比较次数和尽可能少的记录移动次数。

##### 		2.辅助空间

​		评价排序算法的另—个主要标准是执行算法所需要的辅助存储空间。辅助存储空间是除了存放待排序所占用的存储空间之外，执行算法所需要的其他存储空间。

##### 		3.算法的复杂性

​		注意这里指的是算法本身的复杂度，而不是指算法的时间复杂度。显然算法过于复杂也会影晌排序的性能。

​		本章一共要讲解7种排序的算法，按照算法的复杂度分为两大类，冒泡排序、简单选择排序和直接插入排序属于简单算法，而希尔排序、堆排序、归并排序、决速排序属于改进算法。

**Python数据结构实现各种排序算法(含算法介绍与稳定性,复杂度汇总表)**

| 排序方法 | 平均情况         | 最好情况  | 最坏情况 | 辅助空间      | 稳定性 |
| :------- | ---------------- | --------- | -------- | ------------- | ------ |
| 冒泡排序 | O(n^2^)          | O(n)      | O(n^2^)  | O(1)          | 稳定   |
| 选择排序 | O(n^2^)          | O(n^2^)   | O(n^2^)  | O(1)          | 不稳定 |
| 插入排序 | O(n^2^)          | O(n)      | O(n^2^)  | O(1)          | 稳定   |
| 希尔排序 | O(nlogn)~O(n^2^) | O(n^1.3^) | O(n^2^)  | O(1)          | 不稳定 |
| 堆排序   | O(nlogn)         | O(nlogn)  | O(nlogn) | O(1)          | 不稳定 |
| 归并排序 | O(nlogn)         | O(nlogn)  | O(nlogn) | O(n)          | 稳定   |
| 快速排序 | O(nlogn)         | O(nlogn)  | O(n^2^)  | O(nlogn)~O(n) | 不稳定 |

#### 9.2.3 python排序用到的结构与函数

​		python与c不同实现排序使用list即可，不需要用python再去设计一个顺序表结构，也能够很好的实现排序算法的讲解，所以我们下面就不会用顺序表来实现排序，但我们也提供一下python实现顺序表的代码。

```python
class SqList:

    def __init__(self, capacity):
        self.capacity = capacity
        self.data = [None] * self.capacity
        self.size = 0

    def resize(self, newcapacity):
        assert newcapacity >= 0
        if newcapacity >= self.capacity:
            self.data = self.data + [None] * (newcapacity - self.capacity)
        else:
            self.data = self.data[:newcapacity]
        self.capacity = newcapacity

    def fromlist(self, a):
        for i in range(len(a)):
            if self.size == self.capacity:
                self.resize(self.size * 2)
            self.data[self.size] = a[i]
            self.size += 1

    def add(self, e):
        if self.size == self.capacity:
            self.resize(self.size * 2)
        self.data[self.size] = e
        self.size += 1

    def insert(self, i, e):
        assert 0 <= i <= self.size
        if self.size == self.capacity:
            self.resize(self.size * 2)
        for j in range(self.size, i, -1):
            self.data[j] = self.data[j - 1]
        self.data[i] = e
        self.size += 1

    def delete(self, i):
        assert 0 <= i < self.size
        for j in range(i, self.size - 1):
            self.data[j] = self.data[j + 1]
        self.size -= 1
        if self.size < self.capacity / 2:
            self.resize(self.capacity / 2)

    def __getitem__(self, i):
        assert 0 <= i < self.size
        return self.data[i]

    def __setitem__(self, i, x):
        assert 0 <= i < self.size
        self.data[i] = x

    def getno(self, e):
        for i in range(self.size):
            if self.data[i] == e:
                return i
        return -1

    def getsize(self):
        return self.size

    def display(self):
        print(*self.data[:self.size])

```

​		由于排序最常用到的操作是数组的两个元素的交换,我们将它写成函数,在之后的讲解中会大量用到。

```python
def swap(i,j):
    return j,i
```



### 9.3冒泡排序

​		**冒泡排序（Bubble Sort）是一种交换排序，它的基本思想是:两两比较相邻记录的关键字,如果反序则交换，直到没有反序的记录为止。**

​		下面的动图是实现冒泡流程。

​		网图侵权删

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210611113132421.gif#pic_center)



#### 9.3.1 python实现算法

```python
array_test = [8, 3, 5, 1, 10, 4, 2, 6, 7, 9]
#  冒泡排序
#  从后向前依次对比 每一轮都把最小的放在最前面
# 一共需要进行n-1次
def swap(i,j):
    return j,i
 
def BubbleSort(array):
    n = len(array)
    for i in range(0, n-1):
        j = n-1
        while j > i:
            if array[j] < array[j-1]:
                array[j],array[j-1] = swap(array[j],array[j-1])
            j = j - 1
    return array
 
print(BubbleSort(array_test))
```

优化通过增加一个标记变量flag来实现：

```python
array_test = [9, 1, 5, 8, 3, 7, 4, 6, 2]
#  冒泡排序
#  从后向前依次对比 每一轮都把最小的放在最前面
# 一共需要进行n-1次
def swap(i,j):
    return j,i
 
def BubbleSort(array):
    n = len(array)
    for i in range(0, n-1):
        flag = False
        j = n-1
        while j > i:
            if array[j] < array[j-1]:
                array[j],array[j-1] = swap(array[j],array[j-1])
                flag = True
            j = j - 1
         if not flag:
            break
    return array
 
print(BubbleSort(array_test))
```

​		代码改动的关键就是在j变量的循环中,增加了对flag是否为TRUE的判断。经过这 样的改进,冒泡排序在性能上就有了一些提升,可以避免已经有序的情况下的无意义循环判断。

#### 9.3.2复杂度分析

​		当最好的情况，就是要排序的表本身就是有序的,那么我们比较次数，根据最后改进的代码，可以推断出就是n-1次的比较，没有数据交换，时间复杂度为O(n)。当最坏的情况’即待排序表是逆序的情况,此时需要比较 $\sum_{i=2}^{n}(i-1)=1+2+3+\cdots +(n-1)=\frac{n(n-1)}{2}$次,并作等数量级的记录移动。因此，总的时间复杂度为$O(n^2)$。



### 9.4简单选择排序

​		**简单选择排序法（Simple Selection Sort）就是通过*n-i*次关键字间的比较，从*n-i+1*个记录中选出关键字最小的记录，并和第i（*1≤i≤n*）个记录交换。**

​		下面的动图是实现选择排序的流程。

​		网图侵权删

![img](https://www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)

#### 9.4.1 python实现算法：

```python
array_test = [8, 3, 5, 1, 10, 4, 2, 6, 7, 9]
#  选择排序
#  从左向右 每次选择最小的值，放到未排序序列的首部
#  一共需要n-1次
def swap(i,j):
    return j,i

def SelectSort(array):
    n = len(array)
    for i in range(0,n-1):
        min_pos = i  # 记录最小元素位置
        for j in range(i+1,n):
            if array[j]<array[min_pos]:
                array[j],array[min_pos] = swap(array[j],array[min_pos])
    return array
 
print(SelectSort(array_test))
```

#### 9.4.2 简单选择排序复杂度分析

​		从简单选择排序的过程来看，它最大的特点就是交换移动数据次数相当少，这样也就节约了相应的时间。

​		分析它的时间复杂度发现，无论最好最差的情况，其比较次数都是一样的多，第i趟排序需要进行n-i次关键字的比较，因而需要比较 $\sum_{i=1}^{n-1}(i-1)=1+2+3+\cdots +(n-1)=\frac{n(n-1)}{2}$次。而对干交换次数而言′当最好的时候,交换为0次，最差的时候，也就初始降序时，交换次数为n-1次,基于最终的排序时间是比较与交换的次数总和，因此，总的时间复杂度依然为$O(n^2)$。

​	应该说,尽管与冒泡排序同为$O(n^2)$，但简单选择排序的性能上还是要略优于冒泡排序。

#### 9.4.3 简单选择排序习题

##### [洛谷：1104生日](https://www.luogu.com.cn/problem/P1104)

###### 题目描述

cjf 君想调查学校 OI 组每个同学的生日，并按照年龄从大到小的顺序排序。但 cjf 君最近作业很多，没有时间，所以请你帮她排序。

###### 输入格式

输入共有 $2$ 行，

第 $1$ 行为 OI 组总人数 $n$；

第 $2$ 行至第 $n+1$ 行分别是每人的姓名 $s$、出生年 $y$、月 $m$、日 $d$。

###### 输出格式

输出共有 $n$ 行，

即 $n$ 个生日从大到小同学的姓名。（如果有两个同学生日相同，输入靠后的同学先输出）

###### 样例 #1

样例输入 #1

```
3
Yangchu 1992 4 23
Qiujingya 1993 10 13
Luowen 1991 8 1
```

样例输出 #1

```
Luowen
Yangchu
Qiujingya
```

###### 提示

数据保证，$1<n<100$，$1\leq |s|<20$。保证年月日实际存在，且年份 $\in [1960,2020]$。

### 9.5直接插入排序

​		直接插入排序（Straight Insertion Sort）的基本操作是将一个记录插入到已经排好序的有序表中，从而得到一个新的、记录数增1的有序表。

​		下面的动图是实现直接插入排序的流程。

​		网图侵权删

![img](https://www.runoob.com/wp-content/uploads/2019/03/insertionSort.gif) 

#### 9.5.1 python实现算法：

```python
array_test = [8,3,5,1,10,4,2,6,7,9]
#  插入排序
#  将数组分为“已排序好”“未排序”两部分
#  每次循环依次从“未排序”中拿出一个 插入到“已排序好”的合适位置
def InsertSort(array):
    for i in range(1,len(array)):  # 默认0号位置已经排好
        if array[i] < array[i-1]:  # 如果比已排序好的列表最大的元素还大 就不需要比较
            temp = array[i]
            # 思路：将temp放到合适位置 并将temp后的元素向后移
            j = i-1
            k = i
            # 找到temp的合适位置
            while array[j] > temp and j >= 0:
                j = j - 1
            # 注意此时j指向的是待插入元素的前一个位置
            # 所以不能移动array[j] 从后到前移动到array[j+1]
            while k > j+1:
                array[k] = array[k-1]
                k = k - 1
            # 把temp放进来
            array[j+1] = temp
    return array
print(InsertSort(array_test))
```

​		**优化**——折半插入排序（在寻找插入位置时，通过改用折半查找以减少比较次数，提升效率）：

```python
def InsertSort(array):
    for i in range(1,len(array)):  # 默认0号位置已经排好
        if array[i] < array[i-1]:  # 如果比已排序好的列表最大的元素还大 就不需要比较
            temp = array[i]
            # 思路：将temp放到合适位置 并将temp后的元素向后移
            high = i-1
            low = 0
            k = i
            # 找到temp的合适位置
            while low <= high:
                mid = (low+high)/2
                if array[mid] > temp:
                    high = mid - 1
                else:
                    low = mid + 1
            # 注意此时j多减了1 已经是temp之前的位置
            # 把合适位置后的所有元素向后移位时 j要加一
            while k > high+1:
                array[k] = array[k-1]
                k = k - 1
            # 把temp放进来
            array[high+1] = temp
    return array
```

​		在代码中，我们需要注意的是二分查找进行了一点小改动，取消了array[mid] == temp的情况，这是因为即时二者相等，出于排序算法稳定性的考虑，我们希望“原本位置靠后的元素”仍可以排在“值相等、但原本靠前的元素”之后。

​		折半插入排序优化了比较时间复杂度（O(nlogn))，但由于其移动的时间复杂度仍为$O(n^2)$，故其总的时间复杂度为$O(n^2)$。

#### 9.5.2 直接插入排序复杂度分析

​        当最好的情况,也就是要排序的表本身就是有序的，因此没有移动的记录，时间复杂度为O(n)。

当最坏的情况,即待排序表是逆序的情况，此时需要比较 $\sum_{i=2}^{n}i=1+2+3+\cdots +(n-1)=\frac{(n+1)(n-1)}{2}$次, 而记录的移动次数也达到最大值$\sum_{i=2}^{n}(i+1)=\frac{(n+4)(n-1)}{2}$次。

​       如果排序记录是随机的，那么根据概率相同的原则，平均比较和移动次数约为$\frac{n^2}{4}$次。因此,我们得出直接插入排序法的时间复杂度为。$O（n^2）$从这里也看出’同样的$O（n^2）$ 时间复杂度’直接插入排序法比冒泡和简单选择排序的性能要好一些。

#### 9.5.3 直接插入排序习题

##### [[CSP-J 2021] 插入排序](https://www.luogu.com.cn/problem/P7910)

###### 题目描述

​		插入排序是一种非常常见且简单的排序算法。小 Z 是一名大一的新生，今天 H 老师刚刚在上课的时候讲了插入排序算法。

​		假设比较两个元素的时间为 $\mathcal O(1)$，则插入排序可以以 $\mathcal O(n^2)$ 的时间复杂度完成长度为 $n$ 的数组的排序。不妨假设这 $n$ 个数字分别存储在 $a_1, a_2, \ldots, a_n$ 之中，则如下伪代码给出了插入排序算法的一种最简单的实现方式：

​		为了帮助小 Z 更好的理解插入排序，小 Z 的老师 H 老师留下了这么一道家庭作业：

​		H 老师给了一个长度为 $n$ 的数组 $a$，数组下标从 $1$ 开始，并且数组中的所有元素均为非负整数。小 Z 需要支持在数组 $a$ 上的 $Q$ 次操作，操作共两种，参数分别如下：

​		$1~x~v$：这是第一种操作，会将 $a$ 的第 $x$ 个元素，也就是 $a_x$ 的值，修改为 $v$。保证 $1 \le x \le n$，$1 \le v \le 10^9$。**注意这种操作会改变数组的元素，修改得到的数组会被保留，也会影响后续的操作**。

​		$2~x$：这是第二种操作，假设 H 老师对 $a$ 数组进行排序，你需要告诉 H 老师原来 $a$ 的第 $x$ 个元素，也就是 $a_x$，在排序后的新数组所处的位置。保证 $1 \le x \le n$。**注意这种操作不会改变数组的元素，排序后的数组不会被保留，也不会影响后续的操作**。

​		H 老师不喜欢过多的修改，所以他保证类型 $1$ 的操作次数不超过 $5000$。

​		小 Z 没有学过计算机竞赛，因此小 Z 并不会做这道题。他找到了你来帮助他解决这个问题。

###### 输入格式

第一行，包含两个正整数 $n, Q$，表示数组长度和操作次数。

第二行，包含 $n$ 个空格分隔的非负整数，其中第 $i$ 个非负整数表示 $a_i$。

接下来 $Q$ 行，每行 $2 \sim 3$ 个正整数，表示一次操作，操作格式见【**题目描述**】。

###### 输出格式

对于每一次类型为 $2$ 的询问，输出一行一个正整数表示答案。

##### 样例 #1

###### 样例输入 #1

```
3 4
3 2 1
2 3
1 3 2
2 2
2 3
```

###### 样例输出 #1

```
1
1
2
```

##### 提示

###### **【样例解释 #1】**

在修改操作之前，假设 H 老师进行了一次插入排序，则原序列的三个元素在排序结束后所处的位置分别是 $3, 2, 1$。

在修改操作之后，假设 H 老师进行了一次插入排序，则原序列的三个元素在排序结束后所处的位置分别是 $3, 1, 2$。

注意虽然此时 $a_2 = a_3$，但是我们**不能将其视为相同的元素**。

###### **【样例 #2】**

见附件中的 `sort/sort2.in` 与 `sort/sort2.ans`。

该测试点数据范围同测试点 $1 \sim 2$。

###### **【样例 #3】**

见附件中的 `sort/sort3.in` 与 `sort/sort3.ans`。

该测试点数据范围同测试点 $3 \sim 7$。

###### **【样例 #4】**

见附件中的 `sort/sort4.in` 与 `sort/sort4.ans`。

该测试点数据范围同测试点 $12 \sim 14$。

###### **【数据范围】**

对于所有测试数据，满足 $1 \le n \le 8000$，$1 \le Q \le 2 \times {10}^5$，$1 \le x \le n$，$1 \le v,a_i \le 10^9$。

对于所有测试数据，保证在所有 $Q$ 次操作中，至多有 $5000$ 次操作属于类型一。

各测试点的附加限制及分值如下表所示。

|    测试点    | $n \le$ |     $Q \le$     |            特殊性质             |
| :----------: | :-----: | :-------------: | :-----------------------------: |
|  $1 \sim 4$  |  $10$   |      $10$       |               无                |
|  $5 \sim 9$  |  $300$  |      $300$      |               无                |
| $10 \sim 13$ | $1500$  |     $1500$      |               无                |
| $14 \sim 16$ | $8000$  |     $8000$      | 保证所有输入的 $a_i,v$ 互不相同 |
| $17 \sim 19$ | $8000$  |     $8000$      |               无                |
| $20 \sim 22$ | $8000$  | $2 \times 10^5$ | 保证所有输入的 $a_i,v$ 互不相同 |
| $23 \sim 25$ | $8000$  | $2 \times 10^5$ |               无                |

##### 附件下载

[sort.zip](https://www.luogu.com.cn/fe/api/problem/downloadAttachment/1m7wo6sc)19.25KB



### 9.6希尔排序

​       希尔排序，也称递减增量排序算法，是插入排序的一种更高效的改进版本。但希尔排序是非稳定排序算法。

​       希尔排序的基本思想是：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录"基本有序"时，再对全体记录进行依次直接插入排序。

下面的动图是实现直接希尔排序的流程。

网图侵权删

![img](https://www.runoob.com/wp-content/uploads/2019/03/Sorting_shellsort_anim.gif)



#### 9.6.1 python实现算法

```python
array_test = [8, 3, 5, 1, 10, 4, 2, 6, 7, 9]
#  希尔排序
#  希尔排序是基于“插入排序对有序表/基本有序表效率高”产生的
#  先追求部分有序 再追求全局有序
#  排序增量为d的子表[i,i+d,i+2d...] 不断缩小增量d
def ShellSort(array):
    d = len(array)//2
    while d >= 1:
        for i in range(d, len(array)):
            if array[i] < array[i-d]:   # 则需要将array[i]插入有序子表
                # 插入排序
                temp = array[i]
                j = i - 1
                k = i
                while array[j] > temp and j >= 0:
                    j = j - 1
                while k > j + 1:
                    array[k] = array[k - 1]
                    k = k - 1
                array[j + 1] = temp
        d = d//2  # 步长折半
    return array
 
print(ShellSort(array_test))
```

#### 9.6.2 希尔排序练习题

##### [力扣：912. 排序数组](https://leetcode.cn/problems/sort-an-array/)

###### 题目描述

给你一个整数数组 `nums`，请你将该数组升序排列。

###### **示例 1：**

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

###### **示例 2：**

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

###### **提示：**

- `1 <= nums.length <= 5 * 104`
- `-5 * 104 <= nums[i] <= 5 * 104`

### 9.7堆排序

  	**堆是具有下列性质的完全二叉树:每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆;或者每个结点的值都小于或等于具左右孩子结点的值,称为小顶堆。**

​      堆排序就是利用堆（假设利用大顶堆）进行排序的方法。它的基本思想是，将待排序的序列构造成一个大顶堆。此时，整个序列的最大值就是堆顶的根结点。将它移走 （其实就将其与堆数组的末尾元素交换，此时末尾元素就是最大值）,然后将剩余的 *n-1*个序列重新构造成一个堆，这样就会得到*n*个元素中的次大值。如此反复执行，便能得到一个有序序列了。

​		下面的动图是实现直接堆排序的流程。

​		网图侵权删

![img](https://www.runoob.com/wp-content/uploads/2019/03/heapSort.gif)

#### 9.7.1 python实现算法：

```python
array_test = [8, 3, 5, 1, 10, 4, 2, 6, 7, 9]
#  堆排序
#  建立大根堆
#  将堆顶元素和待排序序列中的最后一个交换
#  将剩余的[0,n-i]个元素调整为大根堆
def swap(i, j):
    return j, i
 
# 将k为根结点的树 调整为大根堆
def HeadAjust(array, k, len):
    temp = array[k]  # 保存要判断的结点 直到下降到满足大根堆的位置
    i = 2*k  # 左孩子
    while i < len:  # 存在左孩子
        if i+1 < len and array[i] < array[i+1]:  # 存在右孩子
            # 比较左右孩子的大小
            i = i + 1
    # 若以该结点为根节点的树不是最大堆，以上操作已经找到了应该成为根节点的孩子
        if temp >= array[i]:  # 如果已经满足了最大堆
            break
        else:
            array[k] = array[i]  # 与较大子树的值交换
            # 小元素逐层下坠
            k = i  # 此时还不能直接完成交换 仍要继续向下比较
        i = i * 2
    array[k] = temp
 
# 建立大根堆
def BuildMaxHeap(array,len):
    # 只需要处理非叶结点
    k = len // 2  # 一颗完全二叉树的叶子结点数为 n/2向上取整
    while k >= 0:
        HeadAjust(array, k, len)
        k = k - 1  # 自底向上调整
 
# 堆排序
def HeapSort(array):
    n = len(array)
    # 建立大根堆
    BuildMaxHeap(array, n)
    # 指向待排序数组的最后位置
    i = n-1
    while i > 0:
        array[i], array[0] = swap(array[i], array[0])
        # 将根节点为0的调整为大根堆 即选出了数组中最大的元素 放到array[i]的位置
        HeadAjust(array, 0, i-1)
        # 每次待排序的长度都减一
        i = i - 1
    return array
 
print(HeapSort(array_test))
```

#### 9.7.2 堆排序复杂度分析

​		堆排序的运行时间主要消耗在初始构建堆和在重建堆时的反复筛选上。	

​		在构建堆的过程中，因为我们是完全二叉树从最下层最右边的非终端结点开始构建,将它与其孩子进行比较,若有必要进行互换，对于每个非终端结点来说，其实最多进行两次比较和互换操作，因此整个构建堆的时间复杂度为*O(n)*。

​         在正式排序时,第i次取堆顶记录重建堆需要用$O(\log i)$的时间（完全二叉树的某个结 点到根结点的距离为［$\log_2i$]+1）,并且需要取n-1次堆顶记录，因此，重建堆的时间复杂度为$O(n\log n)$。

​		空间复杂度上，它只有一个用来交换的暂存单元。不过由于记录的 比较与交换是跳跃式进行的,因此堆排序也是—种**不稳定**的排序方法。 

​		另外,由于初始构建堆所需的比较次数较多，因此,它并**不适合待排序序列个数较少**的情况。

#### 9.7.3堆排序习题

##### [力扣:前k个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

###### 题目描述

​		给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

###### 示例 1:

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

###### 示例 2:

```
输入: nums = [1], k = 1
输出: [1]
```



### 9.8归并排序

​       归并排序（Merging Sort）就是利用归并的思想实现的排序方法。它的原理是假设初始序列含有n个记录，则可以看成是n个有序的子序列，每个子序列的长度为1，然后两两归并，得到[n/2]（[x]表示不小于x的最小整数）个长度为2或1的有序子序列;再两两归并，……，如此重复，直至得到一个长度为n的有序序列为止，这种排序方法称为2路归并排序。

下面的动图是实现归并排序的流程。

网图侵权删

![img](https://www.runoob.com/wp-content/uploads/2019/03/mergeSort.gif)

#### 9.8.1 python实现算法：

```python
array_test = [8, 3, 5, 1, 10, 4, 2, 6, 7, 9]
#  归并排序
 
#  合并两个有序数组的操作 Merge
def Merge(array,low,mid,high):
    # 必须要分配空间 若直接temp = [] 则temp[k]会报错
    temp = [0] * (high+1)
    # 将原数组复制到temp中
    for k in range(low, high+1):
        temp[k] = array[k]
    i = low
    j = mid+1
    k = i
    # 合并过程
    while i <= mid and j <= high:
        if temp[i] <= temp[j]:
            array[k] = temp[i]
            i = i + 1
        else:
            array[k] = temp[j]
            j = j + 1
        k = k + 1
    # 处理没有合并完的子序列
    while i <= mid:
        array[k] = temp[i]
        k = k + 1
        i = i + 1
    while j <= high:
        array[k] = temp[j]
        k = k + 1
        j = j + 1
 
def MergeSort(array,low,high):
    if low < high:
        mid = (low+high)//2
        # 归并排序左半边
        MergeSort(array, low, mid)
        # 归并排序右半边
        MergeSort(array, mid+1, high)
        # 合并
        Merge(array, low, mid, high)
    return array
print(MergeSort(array_test, 0, len(array_test)-1))
 
```

#### 9.8.2 归并排序复杂度分析

​		归并排序需要将待排序序列中的所有记录扫描一遍，因此耗费O(n)时间,而由完全二叉树的深度可知,整个归并排 序需要进行[$\log_2n$]次,因此’总的时间复杂度为$O(n\log n)$,而且这是归并排序算法中最好、最坏、平均的时间性能。 

 		由于归并排序在归并过程中需要与原始记录序列同样数量的存储空间存放归并结果 以及递归时深度为$\log_2n$的栈空间，因此空间复杂度为$O(n+\log n)$。

​		另外，对代码进行仔细研究，发现Merge函数中有if temp[i] <= temp[j]语句,这就说明它 需要两两比较，不存在跳跃,因此归并排序是—种稳定的排序算法。

​		也就是说，归并排序是—种比较占用内存,但却效率高且稳定的算法。

#### 9.8.3 非递归实现归并排序

```python
def merge(array, low, mid, high):
    left = array[low: mid]
    right = array[mid: high]
    k = 0 
    j = 0
    result = []
    while k < len(left) and j < len(right):
        if left[k] <= right[j]:
            result.append(left[k])
            k += 1
        else:
            result.append(right[j])
            j += 1
    result += left[k:]
    result += right[j:]
    array[low: high] = result

def MergeSort(array):
    i = 1 # i是步长
    while i < len(array):
        low = 0
        while low < len(array):
            mid = low + i #mid前后均为有序
            high = min(low+2*i,len(array))
            if mid < high: 
                merge(array, low, mid, high)
            low += 2*i
         i*= 2

```

​		非递归版本不需要额外的空间。直接在原数组上进行切割合并。

#### 9.8.4归并排序习题

##### [洛谷：P1908逆序对](https://www.luogu.org/problem/P1908)

###### 题目描述

​		猫猫 TOM 和小老鼠 JERRY 最近又较量上了，但是毕竟都是成年人，他们已经不喜欢再玩那种你追我赶的游戏，现在他们喜欢玩统计。

​		最近，TOM 老猫查阅到一个人类称之为“逆序对”的东西，这东西是这样定义的：对于给定的一段正整数序列，逆序对就是序列中 $a_i>a_j$ 且 $i<j$ 的有序对。知道这概念后，他们就比赛谁先算出给定的一段正整数序列中逆序对的数目。注意序列中可能有重复数字。

**Update:数据已加强。**

###### 输入格式

第一行，一个数 $n$，表示序列中有 $n$个数。

第二行 $n$ 个数，表示给定的序列。序列中每个数字不超过 $10^9$。

###### 输出格式

输出序列中逆序对的数目。

###### 样例 #1

###### 样例输入 #1

```
6
5 4 2 6 3 1
```

###### 样例输出 #1

```
11
```

###### 提示

对于 $25\%$ 的数据，$n \leq 2500$

对于 $50\%$ 的数据，$n \leq 4 \times 10^4$。

对于所有数据，$n \leq 5 \times 10^5$

请使用较快的输入输出

应该不会 $O(n^2)$ 过 50 万吧 by chen_zhe



### 9.9快速排序

​		快速排序（Quick Sort）的基本思想是:通过一趟排序将待排记录分割成独立的两部分，其中一部分记录的关键字均比另—部分记录的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序的目的。

#### ![img](https://www.runoob.com/wp-content/uploads/2019/03/quickSort.gif)

#### 9.9.1 python实现算法：

```python
array_test = [8, 3, 5, 1, 10, 4, 2, 6, 7, 9]
#  快速排序
#  每次选择一个元素作为基准 将表“划分”为左右两个部分
#  递归的对子表选择基准 进行划分
def QuickSort(array,low,high):
    if low < high:  # 递归终止条件
        # low == high 的时候即为划分后为两个元素的情况
        pivotpos = Partition(array,low,high)
        QuickSort(array,low,pivotpos-1)
        QuickSort(array, pivotpos+1,high)
    return array
 
def Partition(array,low,high):
    privot = array[low]  # 第一个元素作为privot
    # 因为已经用privot保存了第一个元素的值 所以可以将low指针指向的位置视为空
    while low < high:
        # 此时，可以将low指针指向的位置视为空
        # 先移动high指针
        while low < high and array[high] >= privot:
            high = high - 1
        array[low] = array[high]  # 比privot小的放左边
        # 此时，可以将high指针指向的位置视为空
        while low < high and array[low] <= privot:
            low = low + 1
        array[high] = array[low]  # 比privot大的放右边
    array[low] = privot  # 此时low == high 将privot元素放置于此
    return low  # 返回存放privot的位置
 
print(QuickSort(array_test,0,len(array_test)-1))
```

​		快速排序递归调用的层数就是二叉树的高度。

9.9.2  快速排序复杂度分析

![image-20220921204035900](C:\Users\33879\AppData\Roaming\Typora\typora-user-images\image-20220921204035900.png)

#### 9.9.3快速排序优化方法

##### 1.优化选取枢轴

##### 2.优化不必要的交换

##### 3.优化小数组时的排序方案

##### 4.优化递归操作

#### 9.9.4 快速排序习题

##### [P1177 [模板]快速排序 - 洛谷]([P1177 [模板]快速排序 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1177))

###### 题目描述

​		利用快速排序算法将读入的 $N$ 个数从小到大排序后输出。

​		快速排序是信息学竞赛的必备算法之一。对于快速排序不是很了解的同学可以自行上网查询相关资料，掌握后独立完成。（C++ 选手请不要试图使用 `STL`，虽然你可以使用 `sort` 一遍过，但是你并没有掌握快速排序算法的精髓。）

###### 输入格式

​		第 $1$ 行为一个正整数 $N$，第 $2$ 行包含 $N$ 个空格隔开的正整数 $a_i$，为你需要进行排序的数，数据保证了 $A_i$ 不超过 $10^9$。

###### 输出格式

​		将给定的 $N$ 个数从小到大输出，数之间空格隔开，行末换行且无空格。

###### 样例 #1

###### 样例输入 #1

```
5
4 2 4 5 1
```

###### 样例输出 #1

```
1 2 4 4 5
```

###### 提示

对于 $20\%$ 的数据，有 $N\leq 10^3$；

对于 $100\%$ 的数据，有 $N\leq 10^5$。



### 9.10总结

​		由下图和下表我们可以看出各个排序算法的优势和劣势。



![image-20220921204255334](C:\Users\33879\AppData\Roaming\Typora\typora-user-images\image-20220921204255334.png)

| 排序方法 | 平均情况         | 最好情况  | 最坏情况 | 辅助空间      | 稳定性 |
| :------- | ---------------- | --------- | -------- | ------------- | ------ |
| 冒泡排序 | O(n^2^)          | O(n)      | O(n^2^)  | O(1)          | 稳定   |
| 选择排序 | O(n^2^)          | O(n^2^)   | O(n^2^)  | O(1)          | 不稳定 |
| 插入排序 | O(n^2^)          | O(n)      | O(n^2^)  | O(1)          | 稳定   |
| 希尔排序 | O(nlogn)~O(n^2^) | O(n^1.3^) | O(n^2^)  | O(1)          | 不稳定 |
| 堆排序   | O(nlogn)         | O(nlogn)  | O(nlogn) | O(1)          | 不稳定 |
| 归并排序 | O(nlogn)         | O(nlogn)  | O(nlogn) | O(n)          | 稳定   |
| 快速排序 | O(nlogn)         | O(nlogn)  | O(n^2^)  | O(nlogn)~O(n) | 不稳定 |

