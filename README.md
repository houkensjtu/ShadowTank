# ShadowTank
幻影坦克的研究

## C0语言入门
C0语言是CMU根据教育需求特别设计的一门语言，其语法特征类似于C语言，但进行大幅度的精简并加入了提高编程安全性的contract功能。具体的语言说明请移步[C0 Tutorial](http://c0.typesafety.net/tutorial/Basic-Building-Blocks.html)。这里仅仅列举一些最基础的语言特征以供参考。

### 1. 变量类型
```C
int n;                // 整数。int默认是4 Byte或者32 bit
char c;               // 字符
bool b;               // 布尔值
string str = "Hello"; // 字符串。使用前需要 #use <string>

int[] arr = alloc_array(int , 10); // 数组。必须用alloc_array来分布内存；arr是一个指针

int* p = alloc(int);  // 指针变量

struct Point{         // 结构体
  int x, y;
};

struct Point *p1 = alloc(struct Point); // 与数组类似，struct必须用alloc分配内存，返回一个指针
p1->x = 3;
p1->y = 4; // 访问结构体成员

```

### 2. 函数
函数的声明与定义基本与C语言保持一致，不同的只是有了contract功能。@requires用于确认执行前条件，@ensures用来确认结果正确。比如假设我们自己写
了一个函数来实现幂次方的功能，其输入是底数x和指数y，由于算法简单只能对应指数大于等于0的请况。为了避免用户错误输入负数，在C0中可以这样写：

```C
int power(int x, int y)
//@requires y >= 0;
//@ensures \result = x^y;
{
...
}
```
在编写函数时需要时时考虑函数的先决条件和正确性，CMU的CS15-122课程推荐你在编写时总是先写好precondition和postcondition，然后再去考虑实现。
同时，为了辅助过程式编程的正确性检验，C0还提供了//@loop_invariant这个指令用来抽象循环体的性质。关于contract和loop_invariant以及过程式编程
的正确性检验方法，推荐阅读CS15-122的课件以及这里的文章。

### 3.数组与结构体的内存分配
在C0中声明一个数组一定要通过alloc_array这个方法来分配内存：

```C
int[] arr = alloc_array(int , 10);
//@requires n >= 0;
//@ensures \length(\result) == n;
```
此时arr就是存在于local函数栈中的一个指针，而实际的数组数据则被分配在另外的heap内存之内。访问数组成员方法是通过index访问：
```C
for (int i = 0; i < 10; i++){
  arr[i] = i;
 }
```
注意访问数组成员时必须保证index在合法范围之内，即//@requires i >=0 && i < \length(arr);。如果是在自己编写的函数中
访问某个数组，那就需要添加这个条件来确保访问安全。

类似地，结构体也需要通过alloc函数分配到heap内存内（而不能直接存在于local函数栈）。

## RGB色彩与幻影坦克原理

## C0数据结构

## 最终实现
