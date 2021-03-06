## C语言学习笔记-day4

### 指针

​		指针是c语言的灵魂，我也是第一次学习，希望和大家共同探索，笔记如有错误，烦请大家指正。

​		指针是一种新数据类型，它的值本质是另一个变量的地址，也就是内存位置的地址，在使用之前，也要对指针变量进行声明。

​		为了充分理解指针的工作原理，我们需要理解计算机是怎么工作的。

​		程序是被保存在硬盘当中的， 运行程序，加载到内存，运行着的程序加进程内存。定义变量，就会分配，内存是沟通硬盘和CPU之间桥梁。

内存，例如：char ch，ch这个变量会分配1字节的大小，int a ， a这个变量会分配4字节的大小。

​		对于内存来说：

  1.  内存，以1个字节为单位分配内存，一个字节对应一个地址；

  2. 每个字节的内存都有标号，这个标号就是地址，同时也是指针；

     地址需要储存，32位编译器用32位（4字节）存储地址

     ​						   64位编译器用64位（8字节）存储地址

     存储地址们同样必须要声明它，在定义时，我们必须要在指针名字之前增加一个*：

     ```c
     int    *ip;    //一个整型的指针 
     double *dp;    // 一个 double 型的指针 
     float  *fp;    // 一个浮点型的指针    
     char   *ch;    // 一个字符型的指针 
     ```

     ***

     ### 指针和指针变量

     1--内存区每一个字节都有一个编号，这就是“地址”；（重点理解）

     2--指针的实质就是内存“地址”，地址就是指针，指针就是地址；

     3--我们在叙述的时候会将指针变量简称为指针，实际上它们并不一样：（较为抽象 建议反复阅读 深刻体会）

     **指针**：内存单元的编号

     **指针变量**：指针变量是存放地址的变量

     ***

     ### 指针的应用

     举个例子：

     ```c
     #include<stdio.h>
     int main()
     {
     	 //1.指针也是一种数据类型
          //p是一个变量，p的类型是int *
     	 int *p; 
         //2. 指针指向谁，就把谁的地址赋给指针（重点）
         int a = 10;
         p = &a;// p保存了a的地址 &加个变量的意思就是取这个变量的地址哦 
         printf("%p,%p\n",p,&a);//C语言中%p用来输出指针类型自身的值。 也就是说 %p用来输出地址。以16进制的方式打印地址
         
         return 0;
     
     }
     ```

     这里的运行结果是打印p和a的地址，在我机器上的运行结果：

     ```C
     0060FEF8,0060FEF8
     ```

     直接操作指针变量本身是没有意义的，一定要给指针一个归宿，需要操作*p,操作指针所指向的内存

     例如：

     ```c
     int a = 10;
     p = &a;
     *p = 100;
     printf("%d , %d",*p,a);
     ```

     这里输出结果

     ```
     100，100
     ```

     指针使用的本质有一个图示，但是言语无法表述，奉上一个四分钟的视频，清晰明白，传送门：

     https://www.bilibili.com/video/BV1w7411q7AA?p=172

     ***

     ### 指针变量和指针变量指向的内存

     我们在定义一个指针的时候，要加一个*，这个 有两层的内涵或者称之为奥义：

     奥义1.定义变量时，* 代表的是类型，它是指针类型int *

     奥义2.在使用变量时，* 代表操作指针所指向的内存

     例如：

     ```c
     #include<stdio.h>
     
     int main()
     {
         int a = 10; //定义一个变量 名字为a 存储的值为10
         int *p = &a;// 根据奥义1 这个定义了一个int* 型指针p 它这里被赋予的值是a的地址
         *p = 111;//根据奥义2 在*使用的时候p是a的地址，即*p = a 这一步将a的值赋成 111
         int *q;//根据奥义1 这个定义了一个int* 型指针q
         q = p;//将p的值给到q 也就是说现在q的值也是a的地址
         *q = 222;//根据奥义2 在*使用的时候q是a的地址 即*q = a 这一步a的值成为222
         printf("&a = %p, p = %p, q = %p\n",&a, p ,q);
         printf("a = %d, *p = %d,  *q = %d\n",a, *p,*q);
         return 0;
     }
     ```

     这道题的输出结果是：

     ```c
     &a = 0060FEF4, p = 0060FEF4, q = 0060FEF4
     a = 222, *p = 222,  *q = 222
     ```

     这道题有点抽象，建议反复阅读，深刻理解

     ***

     ### 野指针

     定义：这个指针变量保存了一个没有意义（非法）的地址

     在开始举例之前我们必须要明白的是： 只有定义后的变量，此变量的地址才是合法地址

     例如：

     ```c
     #include<stdio.h>
     int main()
     {
     	int *p;
         p = 0x1234;
         printf("p = %d\n",p)//到这一步都是没有问题的
         *p = 100;//可能系统中存在一个地址为0x1234内存，因为它是未知or系统没有授权内存 系统不会允许你擅自来修改
         
     }
     ```

     这个程序无法编译

     ​		这个程序告诉我们野指针是保存没有意义的指针变量，然而有趣的是操作野指针本身是不会有任何问题的，操作野指针所指向的内存才会产生段错误。

     **段错误**：段错误一般是当你访问了未申请的内存或非法的内存时产生的。主要还是程序的内存管理有问题。

***

### 空指针

空指针可以在一定程度上阻止野指针产生

空指针就是给指针变量赋值成为NULL ，而NULL就是数字零 这时通过宏定义定义过的啦

```c
#include<stdio.h>
int main()
{
    int *p = NULL;//这行代码相当于是 int *; p = NULL;
    int a = 11;
    p =&a;
    
    if(p!=NULL)//这就是空指针意义 可以进行判定 在不为空的情况下操作 
    {
        *p = 100;
    }
    return 0;
}
```

***

### 多级指针

​		如何定义一个何时类型的变量保存另一个变量的地址是个问题，在普通指针的基础上，我们可以在需要保存变量地址的类型基础上加一个*

例如：

```c
#include<stdio.h>
int main()
{	
    int a = 10;
    int *p = &a;
    int **q = &p;
    int ***t = &q;
    int ****m =&t;
    
    *m; -> t
    **m; ->q
    ***m; ->p
    *t; -> a
    **t;
    ***t;
    return 0;
}
```

 这个例子告诉我们：

1.我们遇到多少*都不要怕，微笑着面对它，找好这一个指针对应地址一层层寻找就可以

2.指针也是一个变量，是变量就可以赋值

3.指针指向谁，就把谁的地址赋给指针

4.*p操作是指针所指向内存























