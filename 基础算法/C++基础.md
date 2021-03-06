- nullptr是c++11中的关键字，表示空指针。书中的例子经常看到判断函数的参数是否为空指针。
- 引用和指针做参数的区别？功能差不多，但是**优先用引用**。**引用本身只是一个别名，本身不占内存，但是指针首先是变量，本身是要占内存的**。引用作为形参也更方便寻址。



## 指针

- 函数要求返回一个新建的指针，如何新建？

  手动的申请内存: c++是`new`, c是`malloc`。 这样申请的内存属于堆，不会自动释放。具体为
  
  ```c++
  int* p = new int;
  TreeNode* pNode = new TreeNode(1); // 和构造函数对应
  ```
  
  

## 内存分配和初始化

二维数组如果全局变量，初始值全部是0。如果不是全局变量，初始值是随机的，需要手动初始化。

`memset(next,0,sizeof(next))`  以next为起始地址，把之后的`sizeof(next)`个字节初始化为0。**只能初始化为0或-1。**

字符串初始化 `string s(7, 'a') // 7个a`

字符串vector初始化 `vector<string> vec(2, string(3,'a'))` 

新建长度为n的bool数组 `bool* arr = new bool[n]`

## 常用函数

`sort()`  参数是两个指针，默认升序。

**sort自定义cmp：** 1） 返回的是bool，排序之后的元素能满足true, 最好加上static（表示只能在本文件中调用，leetcode上必须加上这个）； 2）传入的是const引用，第一个参数表示排序后位置在前面的元素（**注意写cmp函数时，当两个元素完全相同，认为返回false，否则会报错。**）

`swap()`   

`sprintf( char *buffer, const char *format [, argument] … )` 打印数据到一个字符串变量。

`void qsort(void*base,size_t num,size_t width,int(__cdecl*compare)(const void*,const void*))`  base需要排序的目标数组开始地址， num目标数组元素个数，width每个元素长度， compare比较函数。compare函数的参数类型是空指针，需要自己转换类型。排序后的数组，能够满足使compare函数返回true。

`strcat()`  拼接两个字符串

`strcpy()` 拷贝字符串

`strcmp()` 相当于两个字符串做减法，相等返回0，str1小于str2，返回负数。 str1大于str2，  返回正数。

`vector.end() - 1` 才是vector末尾元素的指针

`stoi()` 将string类型字符串转为int, `to_string()` 将数字转字符串

`atoi()` 将char*类型字符串转为int

## 字符串处理

ascii码为0的字符是'\0'

`string s` 添加字符 `s.append(c, 3)` 把字符串c的前3个字符添加到s末尾， `s.append(c)` 把字符串c添加到s末尾。

`s.substr(int begin, int N)` 从下标begin开始， 拷贝N个元素。

ASCII码为0的字符，是空字符，不是可显示字符。统计256个字符的数量时，直接对char的值（0-255）当成int统计，不需要减字符。

## STL相关

二维动态数组初始化 `    vector< vector<int> > dp(ncols,vector<int>(nrows, 0));`

size()的时间复杂度是O(1)还是O(n) 取决于STL的版本，和编译器有关，比如vc6中的size是O(1)， gcc中的size是O(n)。

复制一个容器来初始化`vector<int> v(v_old)`

**vector基本操作**

`v.insert(v.begin()+pos, element)` 将pos处插入一个element的拷贝

`vector<int> tmp(v.begin(), v.begin()+2)`  拷贝子数组

**哈希表基本操作:**

1) map和unordered_map的区别，unordered_map中的key不是排序的，如果想获得排序后的数据，或打印具有一定顺序的元素，用map; 如果只是想记录数据，用unordered_map. map查找和插入的时间复杂度是O(logn), unordered_map查找和插入的平均时间复杂度是O(1), 最坏是O(n)

2) 基本操作:  

```c++
unordered_map<int, int> hash;  // 初始化
auto iter = hash.find(1)// 查找key, 如果存在则返回对应的迭代器，不存在则返回hash.end(). 和其他容器一样end()是没有元素的。
int tmp = hash[1];  // 访问
hash.insert( pair<int, int> (1, 2) ); // 插入
hash.insert( {1, 2} ); //插入
hash.erase(); // 删除
iter->first; //迭代器的key
iter->second; // 迭代器的value
```

unordered_map的key可以是字符串，但不能是整数数组。value可以是任何数据结构。

**set基本操作**

和map的操作类似。但是访问set的最后一个元素时，不能用`*(mSet.end()-1)` ，会报错。 **用`*mSet.rbegin()` 是访问最后一个元素，表示反向迭代器的第一个元素。** 

**array基本操作**

`array<int, N>` 开一个长度为N的数组。目前没看到和常规的数组有什么区别。

`array<int, N> arr[100]`  可以开一个长度为100*N的二维数组, `arr[0]` 可以直接赋值给`arr[1]`, 如果普通的数组是不行的。

**链表list**

list可以看作一个双端队列，头尾插入的时间复杂度是O(1),  **删除一个已知节点的时间复杂度也是O(1)**。

从尾节点循环创建链表：

```c++
// 新建节点后，节点的next指向上一个创建的节点。
ListNode* node = nullptr;
for(int i=0;i<nums.size();i++) {
	ListNode* old = node;
    node = new ListNode( num[i] );
    node->next = old;
}
```



**双向队列**

```c++
deque<int> dq;
dq.push_back();
dq.pop_back();
dq.push_front();
dq.pop_front();
```

**优先队列**

```c++
// 一般当作堆来使用，避免手写，但是需要额外空间。
// priority_queue<Type, Container, Functional>
// Type表示数据类型， Container表示容器，一般就是vector<>， Functional表示排序方式

//升序队列
priority_queue <int,vector<int>,greater<int> > q;
//降序队列
priority_queue <int,vector<int>,less<int> >q;
// less, greater是c++自带的仿函数
int x = q.top() // 队头元素
q.push(x) // 插入元素到队尾，并排序
q.pop() // 弹出对头元素
    
```



## 常用宏

```c++
INT_MAX, INT_MIN // int的最大值, 最小值
LONG_MAX, LONG_MIN // long long 的最大值，最小值, 64位
```

看变量占几个字节用`sizeof()`, long 和long long 都是8位。



## algorithm的函数

`inplace_merge()`  合并**一个数组的**两个有序区间, 

```
void inplace_merge (BidirectionalIterator first, BidirectionalIterator middle,
                      BidirectionalIterator last);
```

两个区间都是用左闭右开表示。

`merge()` 合并两个数组的指定区间, 

 ```c++
// result的空间需要手动分配 
OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                        InputIterator2 first2, InputIterator2 last2,
                        OutputIterator result);
 ```





`reverse(vec.begin(), vec.end())` 翻转数组

`reverse_copy(vec.begin(), vec.end())` 翻转并拷贝

**二分查找**

`lower_bound()` 是最常用的，一定要记住，返回第一个**大于等于val**的下标。在有序数组中查找val，是可能有多个取值为val的元素，这些元素组成一个连续区间，`lower_bound` 就是指这个区间的下界，`upper_bound` 指的就是整个区间的上界。

```c++
// 查找数组中是否存在val
bool binary_search (ForwardIterator first, ForwardIterator last,
                      const T& val);
// 查找数组中第一个大于等于val的元素的下标
ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last,
                               const T& val);
// 查找第一个 大于 val的元素下标
ForwardIterator upper_bound (ForwardIterator first, ForwardIterator last,
                               const T& val);
```



## 运算优先级

位运算优先级非常低。保险起见，都加括号。



## 模板的使用

调用：

如 `swap<int> (a, b)`