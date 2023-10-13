# Chapter 02

> **Review Questions**

## 2-1
C语言的基本模块是什么？

> 函数

## 2-2
什么是语法错误？写出一个英语例子和C语言例子。

> 语法错误违反了组成语句或程序的规则。
> 这是一个有语法错误的英文例子：Me  speak  English  good.。
> 这是一个有语法错误的C语言例子：printf"Where are the parentheses?";。

## 2-3
什么是语义错误？写出一个英语例子和C语言例子。

> 语义错误是指含义错误。这是一个有语义错误的英文例子：This sentence isexcellent Czech.> 这是一个有语义错误的C语言例子： thrice_n =3 + n;

## 2-4
Indiana Sloth编写了下面的程序，并征求你的意见。请帮助他评定。
```c
    include studio.h
    int main{void} /* 该程序打印一年有多少周 /*
    (
        int s
        s := 56;
        print(There are s weeks in a year.);
    return 0;
```
> 第1行：以一个`#`开始；`studio.h`应改成`stdio.h`；然后用一对尖括号把`stdio.h`括起来。
> 第2行：把`{}`改成`()`；注释末尾把`/*`改成`*/`。
> 第3行：把(改成｛
> 第4行：`int s`末尾加上一个分号。
> 第5行没问题。
> 第6行：赋值用`=`，而不是用`:=`（这说明Indiana Sloth了解`Pascal`）。另外，用于赋值的值56也不对，一年有52周，不是56周。
> 第7行应该是：`printf("There are %d weeks in a year.\n", s)`;
> 第9行：原程序中没有第9行，应该在该行加上一个右花括号`｝`。
> 修改后的程序如下：
```c
#include <stdio.h>
int main(void) /* this prints the number of weeks in a year */
{
    int s;
    s = 52;
    printf("There are %d weeks in a year.\n", s);
    return 0;
}
```

## 2-5
假设下面的4个例子都是完整程序中的一部分，它们都输出什么结果？

    a.printf("Baa Baa Black Sheep.");
      printf("Have you any wool?\n");
    b.printf("Begone!\nO creature of lard!\n");
    c.printf("What?\nNo/nfish?\n");
    d.int num;
      num = 2;
      printf("%d + %d = %d", num, num, num + num);

> a.Baa Baa Black Sheep.Have you any wool?（注意，Sheep.和Have之间没有空格）
> b.Begone!
> O creature of lard!
> c.What?
> No/nfish?（注意斜杠/和反斜杠\的效果不同，/只是一个普通的字符，原样打印）
> d.2 + 2 = 4（注意，每个`%d`与列表中的值相对应。还要注意，`+`的意思是加法，可以在`printf()`语句内部计算）
> 
## 2-6
在`main`、`int`、`function`、`char`、`=`中，哪些是C语言的关键字？

> 关键字是`int`和`char`（`main`是一个函数名；`function`是函数的意思；`=`是一个运算符）。

## 2-7
如何以下面的格式输出变量words和lines的值（这里，3020和350代表两个变量的值）？
> There were 3020 words and 350 lines.

> `printf("There were %d words and %d lines.\n", words, lines);`

## 2-8
考虑下面的程序：
```c
#include <stdio.h>
int main(void)
{
    int a, b;
    a = 5;
    b = 2; /* 第7行 */
    b = a; /* 第8行 */
    a = b; /* 第9行 */
    printf("%d %d\n", b, a);
    return 0;
}
```
请问，在执行完第7、第8、第9行后，程序的状态分别是什么？

> 执行完第7行后，a是5，b是2。执行完第8行后，a和b都是5。执行完第9行后，a和b仍然是5（注意，a不会是2，因为在执行`a=b`;时，b的值已经被改为5）。

## 2-9
考虑下面的程序：
```c
#include <stdio.h>
int main(void)
{
    int x, y;
    x = 10;
    y = 5;   /* 第7行 */
    y = x + y; /*第8行*/
    x = x*y;  /*第9行*/
    printf("%d %d\n", x, y);
    return 0;
}
```
请问，在执行完第7、第8、第9行后，程序的状态分别是什么？

> 执行完第7行后，x是10，b是5。执行完第8行后，x是10，y是15。执行完第9行后，x是150，y是15。