### 内存分区简介

- 操作系统对内存的操作实际上是在虚拟内存上完成的，cpu读取数据是对虚拟内存，虚拟地址通过映射到主存的物理地址，来读取数据。
- 虚拟内存是计算机系统内存管理的一种技术。它使得应用程序认为它拥有连续可用的内存（一个连续完整的地址空间），而实际上，它通常是被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换。
- 虚拟地址和物理地址间有一个页表用来记录虚拟地址和物理地址的映射关系
- 映射的概念 : 假设两个非空集合X、Y，存在一个法则f，使得对X中的每个元素x,按法则f，在Y中有唯一确定的元素y与之对应，那么称f为从X到Y的映射,映射它的本质就是一种对应关系。
- http://t.zoukankan.com/PIRATE-JFZHOU-p-8186352.html

### **整型家族**

整型家族包括字符、短整型、整型和长整型，它们都分为有符号（signed）和无符号

| 类型               | 大小  | 取值范围                                   |
| ------------------ | ----- | :----------------------------------------- |
| char               | 1byte | -128 ~ 127                                 |
| unsigned char      | 1byte | 0 ~ 255                                    |
| short              | 2byte | -32768 ~ 32767                             |
| unsigned short     | 2byte | 0 ~ 65535                                  |
| int                | 4byte | -2147483648 ~ 2147483647                   |
| unsigned int       | 4byte | 0 ~ 4294967295                             |
| long               | 8byte | -9223372036854775808 ~ 9223372036854775807 |
| unsigned long      | 8byte | 0 ~ 18446744073709551615                   |
| long long          | 8byte | -9223372036854775808 ~ 9223372036854775807 |
| unsigned long long | 8byte | 0 ~ 18446744073709551615                   |

![image-20221022121443505](C:\Users\学习\AppData\Roaming\Typora\typora-user-images\image-20221022121443505.png)

![image-20221022123122484](C:\Users\学习\AppData\Roaming\Typora\typora-user-images\image-20221022123122484.png)

![image-20221022123106341](C:\Users\学习\AppData\Roaming\Typora\typora-user-images\image-20221022123106341.png)

### C/C++内存分配

![v2-fb232cab9f85cef53ab4eed7bcd11049_1440w](C:\Users\学习\Nutstore\1\我的坚果云\c_asset\v2-fb232cab9f85cef53ab4eed7bcd11049_1440w.webp)

c++内存分布分为以下几个部分：

**栈区（stack）**：由编译器自动分配与释放，存放为运行时函数分配的局部变量、函数参数、返回数据、返回地址等。
**堆区（heap）**：一般由程序员自动分配，比如malloc/free、new/delete。其分配类似于链表。
**全局区（静态区static）**：存放全局变量、静态数据(静态全局和静态局部)、常量。程序结束后由系统释放。全局区分为已初始化全局区（data）和未初始化全局区（bss）。
**常量区（文字常量区）**：存放常量字符串，程序结束后有系统释放。
**代码区**：存放函数体（类成员函数和全局区）的二进制代码。

### **栈和堆得区别**

- **分配机制不同**：栈是由编译器需要时自动分配，不需要时自动清除的变量存储区。通常存放局部变量、函数参数等 ；。堆是由程序员自己进行申请和释放，一般是new/delete活着malloc/free;
- **空间大小不同**：系统使用链表进行存储空闲内存地址，所以堆是不连续的内存区域，堆的大小只受限于计算机系统中有效的虚拟内存；栈的空间是一块连续的空间，也是操作系统事先预定好的，在linux下可以使用 ulimit -a 可以查看栈空间大小，一般为10240，也就是10M；
- **碎片问题**：在堆上我们频繁的会使用new/delete操作，会造成内存碎片；但是对于栈来说，由于是先进后出，是一一对应的，所以不会有内存碎片的产生。
- **内存地址：**对于内存地址，堆是向上生长；而栈是由高地址向低地址生长；

### **栈为什么是从高地址往低地址分配内存的**

首先，我们来看下栈的特性是先进后出，栈中存储的是局部变量和参数，并且栈的内存分配是连续的；比如我们给一个数组分配内存：一般情况下我们数组的首地址即是起始地址，数组的元素的地址是从低到高，而栈是先进后出，操作系统分配内存在入栈的时候，把高地址放在栈底，最后低地址就在栈顶，所以对应的栈顶就是低地址，也符合数组的首地址就是起始地址的说法。反过来来，如果栈分配内存地址也是按照从低到高，只能拿到栈顶的高地址数据，从而无法知道数组的起始地址。

### 高地址和低地址、高字节与低字节、大小端模式的转换、存储顺序

### **一、高地址和低地址**

![20180923182938492](C:\Users\学习\Nutstore\1\我的坚果云\c_asset\20180923182938492.png)

**二、高字节低字节**

如int a=16777220，化为十六进制是0x01 00 00 04，则04属于低字节，01属于高字节。

### **三、大小端模式**

（1）如果a在内存中的存放顺序为下图（即低字节存放在高地址），则为大端模式

![20180923182938492](C:\Users\学习\Nutstore\1\我的坚果云\c_asset\20180923182938492.png)

（2）如果a在内存中的存放顺序为下图（即低字节存放在低地址），则为小端模式

![20180923182938492](C:\Users\学习\Nutstore\1\我的坚果云\c_asset\20180923182938492.png)

（3）如何互换（通过移位操作再或）

![20180923182938492](C:\Users\学习\Nutstore\1\我的坚果云\c_asset\20180923182938492.png)

### **四、存放顺序**

数据在内存中存放的原则

- **一个整数类型内部 :**  低地址存储低位，高地址存储高位。比如int a=1，则存储情况为0000（高地址） 0000 0000 0001（低地址）

- **若干个局部变量（在栈中存储的）: ** 先定义的高地址，后定义的低地址。

- **类、结构体或数组的元素 : **先定义的低地址，后定义的高地址。

### **五、测试说明**

- **整数类型内部：**低地址存储低位，高地址存储高位。

```c++
#include<iostream>
using namespace std;
 
union U
{
	char str[2];
    short int num;
};
 
int main() 
{
	U u;
	u.str[0] = 10;//存放在低地址，0000 1010
	u.str[1] = 1;//存放在高地址， 0000 0001
	cout << u.num << endl;//组合的时候，整数类型内部低地址存储低位，高地址存储高位，因此是0000 0001 0000 1010 = 266
	system("PAUSE");
	return 0;
}
```

- 若干个局部变量（在栈中存储的）：先定义的高地址，后定义的低地址。

  类、结构体、[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)中的元素：先定义的低地址，后定义的高地址

  ```cpp
  class Test {
  public:
  	int m;
  	int n;
  };
  int main() 
  {
  	int a;
  	char b;
  	int c[10];
  	Test t;
  	cout << (size_t)&a << endl;//结果1
  	cout << (size_t)&b << endl;//结果2
  	cout << (size_t)&c << endl;//结果3
  	cout << (size_t)&t << endl;//结果4
  	cout << (size_t)&t.m << endl;//结果5
  	cout << (size_t)&t.n << endl;//结果6
  	system("PAUSE");
  	return 0;
  }
  ```

![20180923193608345](C:\Users\学习\Nutstore\1\我的坚果云\c_asset\20180923193608345.png)

结果1>结果2>结果3>结果4=结果5<结果6

分析：

结果1>结果2>结果3>结果4，是因为a、b、c、t都是局部变量，在栈上存储，栈是从高地址到低地址，因此地址逐渐减小。

结果5<结果6，是因为结构体内部，先定义的地址小，后定义的地址大，这与类内的成员，数组总的元素，都是类似的。      

分析它们的数值差，可以发现字节对齐问题，数组名占用4字节等问题。

总的来说，具体的地址，需要考虑“栈的高地址到低地址”、“字节对齐”、“数组”这样的特殊情况等等。

### 结构体的内存分配机制

- 编译器在给结构体开辟空间时，首先找到结构体中最宽的基本数据类型，然后寻找内存地址能是该基本数据类型的整倍的位置，作为结构体的首地址。
- 为结构体的一个成员开辟空间之前，编译器首先检查预开辟空间的首地址相对于结构体首地址的偏移是否是本成员类型宽度的整数倍，若是，则存放本成员，反之，则在本成员和上一个成员之间填充一定的字节，以达到整数倍的要求，也就是将预开辟空间的首地址后移几个字节。
- 结构体的总大小为结构体中最宽基本数据成员的整数倍。如有需要，编译器将会在结构体的添加填充字符。
- 结构体中的数据成员按照所占空间从大到小的顺序来排列的话，这样空间利用率就会得以提高。

- 对四条进行修正。在组织数据结构的数据成员的时候，可以将相同类型的成员放在一起**，**这样就减少了编译器为了对齐而添加的填充字符。

### 位段

位段，C语言允许在一个结构体中以位为单位来指定其成员所占内存长度，这种以位为单位的成员称为“位段”或称“位域”( bit field) 。利用位段能够用较少的位数存储数据。信息的存取一般以字节为单位。实际上，有时存储一个信息不必用一个或多个字节，例如，“真”或“假”用0或1表示，只需1位即可。在计算机用于过程控制、参数检测或数据通信领域时，控制信息往往只占一个字节中的一个或几个二进制位，常常在一个字节中放几个信息。

**以下程序则展示了一个位段的声明：**

```c
struct CHAR
{
    unsigned int ch   : 8;    //8位
    unsigned int font : 6;    //6位
    unsigned int size : 18;   //18位
};
struct CHAR ch1;
```

**以下程序展示了一个结构体的声明：**

```c
struct CHAR2
{
    unsigned char ch;    //8位
    unsigned char font;  //8位
    unsigned int  size;  //32位
};
struct CHAR2 ch2;
```

第一个声明取自一段文本格式化程序，应用了位段声明。它可以处理256个不同的字符（8位），64种不同字体（6位），以及最多262,144个单位的长度（18位）。这样，在ch1这个字段对象中，一共才占据了32位的空间。而第二个程序利用结构体进行声明，可以看出，处理相同的数据，CHAR2类型占用了48位空间，如果考虑边界对齐并把要求最严格的int类型最先声明进行优化，那么CHAR2类型则要占据64位的空间。

●不允许位段跨越一个字的边界，如果一个字余下的空间不能容纳一个位段，则这个位段从相邻的下一个字的边界开始存放。由此在上一个字中留下未用的空位，称为空穴。（16位系统一个字是2个字节，32位系统一个字是4个字节，64位系统是8个字节。）

●位段可以没有名字，无名位段表示该空间不用。

●位段只能作为结构体成员，不能作共用体的成员。

●位段没有地址，对位段不能进行取地址的“&”运算。

●位段的长度不能大于机器字长度，也不能定义位段数组。

●位段可以用整型格式符输出。

●位段可以在数值表达式中引用，它会被系统自动转换成int型。
