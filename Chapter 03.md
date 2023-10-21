# Chapter 03

<h1>Review Questions</h1>

## 3-1
指出下面各种数据使用的合适数据类型（有些可使用多种数据类型）：

	a.East Simpleton的人口
	b.DVD影碟的价格
	c.本章出现次数最多的字母
	d.本章出现次数最多的字母次数

> a.int类型，也可以是short类型或unsigned  short类型。人口数是一个整数。
> 
> b.float类型，价格通常不是一个整数（也可以使用double类型，但实际上不需要那么高的精度）。
> 
> c.char类型。
> 
> d.int类型，也可以是unsigned类型。


## 3-2
在什么情况下要用long类型的变量代替int类型的变量？
> 原因之一：在系统中要表示的数超过了int可表示的范围，这时要使用long类型。
> 
> 原因之二：如果要处理更大的值，那么使用一种在所有系统上都保证至少是 32 位的类型，可提高程序的可移植性。
>

## 3-3
使用哪些可移植的数据类型可以获得32位有符号整数？选择的理由是什么？
> 如果要正好获得32位的整数，可以使用int32_t类型。
> 
> 要获得可储存至少32位整数的最小类型，可以使用int_least32_t类型。
> 
> 如果要为32位整数提供最快的计算速度，可以选择int_fast32_t类型（假设你的系统已定义了上述类型）。
> 
## 3-4
指出下列常量的类型和含义（如果有的话）：

	a.'\b'
	b.1066
	c.99.44
	d.0XAA
	e.2.0e30

> a.char类型常量（但是储存为int类型）
> 
> b.int类型常量
> 
> c.double类型常量
> 
> d.unsigned int类型常量，十六进制格式
> 
> e.double类型常量


## 3-5
`Dottie Cawm`编写了一个程序，请找出程序中的错误。
```c
include <stdio.h>
main
(
	float g; h;
	float tax, rate;
	g = e21;
	tax = rate*g;
)
```
	第1行：应该是#include <stdio.h>
	第2行：应该是int main(void)
	第3行：把(改为｛
	第4行：g和h之间的;改成,
	第5行：没问题
	第6行：没问题
	第7行：虽然这数字比较大，但在e前面应至少有一个数字，如1e21或1.0e21都可以。
	第8行：没问题，至少没有语法问题。
	第9行：把)改成}
	除此之外，还缺少一些内容。首先，没有给rate变量赋值；其次未使用h变量；而且程序不会报告计算结果。虽然这些错误不会影响程序的运行（编译器可能给出变量未被使用的警告），但是它们确实与程序设计的初衷不符合。另外，在该程序的末尾应该有一个return语句。

下面是一个正确的版本，仅供参考：
```c
#include <stdio.h>
int main(void)
{
	float g, h;
	float tax, rate;
	rate = 0.08;
	g = 1.0e5;
	tax = rate*g;
	h = g + tax;
	printf("You owe $%f plus $%f in taxes for a total of $%f.\n", g, tax, h);
	return 0;
}
```

## 3-6
写出下列常量在声明中使用的数据类型和在printf()中对应的转换说明：
![answer-6](https://github.com/MegaSuite/C_Primer_Plus_Solution/blob/main/Resources/answer-6.png?raw=true)

## 3-7
写出下列常量在声明中使用的数据类型和在printf()中对应的转换说明（假设int为16位）：
![answer-7](https://github.com/MegaSuite/C_Primer_Plus_Solution/blob/main/Resources/answer-7.png?raw=true)
> !!倒数第二个转换说明应为`%#x`
## 3-8
假设程序的开头有下列声明：

	int imate = 2;
	long shot = 53456;
	char grade = 'A';
	float log = 2.71828;

把下面`printf()`语句中的转换字符补充完整：
```c
printf("The odds against the %__ were %__ to 1.\n", imate, shot);
printf("A score of %__ is not an %__ grade.\n", log, grade);
```
> `printf("The odds against the %d were %ld to 1.\n", imate, shot);`
> 
> `printf("A score of %f is not an %c grade.\n", log, grade);`
> 

## 3-9
假设ch是char类型的变量。分别使用转义序列、十进制值、八进制字符常量和十六进制字符常量把回车字符赋给ch（假设使用ASCII编码值）。

> `ch = '\r';`
> 
> `ch = 13;`
> 
> `ch = '\015'`
> 
> `ch = '\xd'`

## 3-10
修正下面的程序（在C中，/表示除以）。
```c
void main(int) / this program is perfect /
{
	cows, legs integer;
	printf("How many cow legs did you count?\n);
	scanf("%c", legs);
	cows = legs / 4;
	printf("That implies there are %f cows.\n", cows)
}
```

	最前面缺少一行（第0行）：#include <stdio.h>
	第1行：使用/*和*/把注释括起来，或者在注释前面使用//。
	第3行：int cows, legs;
	第4行：count？\n");
	第5行：把%c改为%d，把legs改为&legs。
	第7行：把%f改为%d。
	另外，在程序末尾还要加上return语句。

下面是修改后的版本：
```c
#include <stdio.h>
int main(void) /* this program is perfect */
{
	int cows, legs;
	printf("How many cow legs did you count?\n");
	scanf("%d", &legs);
	cows = legs / 4;
	printf("That implies there are %d cows.\n", cows);
	return 0;
}
```
## 3-11
指出下列转义序列的含义：

	a.\n
	b.\\
	c.\"
	d.\t

> a.换行字符
> 
> b.反斜杠字符
> 
> c.双引号字符
> 
> d.制表字符
> 
<h1>Programming Exercises</h1>

## 3-1
Find out what your system does with integer overflow, floating-point overflow,and floating point underflow by using the experimental approach; that is,write programs that have these problems.
***
```c
#include <stdio.h>
#include <limits.h>
#include <float.h>

int main(void)
{
	int int_overflow;
	int MAX_INTEGER = INT_MAX;
	float flt_overflow, flt_underflow;
	float MIN_FLOAT = FLT_MIN;
	float MAX_FLOAT = FLT_MAX;
	
	// artificially create over/underflow
	int_overflow = INT_MAX + 1;
	flt_overflow = FLT_MAX * 2.;
	flt_underflow = FLT_MIN / 2.;
	
	// print results
	printf("Max integer: %d \tMax integer + 1: %d\n", INT_MAX, int_overflow);
	printf("Max float: %f \tMax float * 2: %f\n", FLT_MAX, flt_overflow);
	printf("Min float: %f \tMin float / 2: %f\n", FLT_MIN, flt_underflow);

	return 0;
}
```
## 3-2
Write a program that asks you to enter an ASCII code value, such as 66, and then prints the character having that ASCII code.
***
```c
#include <stdio.h>
int main(void) 
{
	int ascii_code;
	printf("Enter an ASCII code: ");
	scanf("%d", &ascii_code);
	printf("Character for ASCII code %d: %c\n", ascii_code, ascii_code);

	return 0;
}
```
## 3-3
Write a program that sounds an alert and then prints the following text: Startled by the sudden sound, Sally shouted,"By the Great Pumpkin, what was that!"
***
```c
#include <stdio.h>
int main(void)

{
	printf("\a"); // sound alert
	printf("Startled by the sudden sound, Sally shouted, \"By the Great Pumpkin, what was that!\"\n");

	return 0;
}
```
## 3-4
Write a program that reads in a floating-point number and prints it first in decimal-point notation,then in exponential notation, and then, if your system supports it, p notation. Have the output use the following format (the actual number of digits displayed for the exponent depends on the system):

Enter a floating-point value: 64.25
fixed-point notation: 64.250000
exponential notation: 6.425000e+01
p notation: 0x1.01p+6 
***
```c
#include <stdio.h>
int main(void) 
{
	float flt_input;

	printf("Enter a floating-point value: ");
	scanf("%f", &flt_input);
	printf("Fixed-point notation: %f\n", flt_input);
	printf("Exponential notation: %e\n", flt_input);
	printf("P notation: %a\n", flt_input);

	return 0;
}
```
## 3-5
There are approximately 3.156 × 10^7 seconds in a year. Write a program that requests your age in years and then displays the equivalent number of seconds.
***
```c
#include <stdio.h>

int main(void)
{
	unsigned int SECONDS_PER_YEAR = 31560000;
	unsigned int age;

	printf("What is your age (in years)?: ");
	scanf("%u", &age);
	printf("You are %u seconds old!\n", SECONDS_PER_YEAR * age);

	return 0;
}
```
## 3-6
The mass of a single molecule of water is about 3.0×10^-23 grams. A quart of water is about 950 grams. Write a program that requests an amount of water, in quarts, and displays the number of water molecules in that amount.
***
```c
#include <stdio.h>

int main(void)
{
	float H20_MASS = 3.0e-23;
	float GRAMS_H20_PER_QUART = 950.;
	float quarts;

	printf("Enter an amount of water (in quarts): ");
	scanf("%f", &quarts);
	printf("There are %f molecules in %f quarts of water.\n", quarts * GRAMS_H20_PER_QUART / H20_MASS, quarts);

	return 0;
}

```
## 3-7
There are 2.54 centimeters to the inch. Write a program that asks you to enter your height in inches and then displays your height in centimeters. Or, if you prefer, ask for the height in centimeters and convert that to inches.
***
```c
#include <stdio.h>

int main(void)
{
	float CM_PER_INCH = 2.54;
	float height;

	printf("Enter your height (in inches): ");
	scanf("%f", &height);
	printf("You are %f centimeters tall.\n", height * CM_PER_INCH);

	return 0;
}
```
## 3-8
In the U.S. system of volume measurements, a pint is 2 cups, a cup is 8 ounces, an ounce is 2 tablespoons, and a tablespoon is 3 teaspoons. Write a program that requests a volume in cups and that displays the equivalent volumes in pints, ounces, tablespoons, and teaspoons. Why does a floating-point type make more sense for this application than an integer type? 
***
```c
#include <stdio.h>

int main(void)
{

	/* If the number of cups is not an even whole number, then the number of
		pints will not be a whole number. */
	float PINTS_PER_CUP = .5;
	float OUNCES_PER_CUP = 8;
	float TBS_PER_CUP = 2 * OUNCES_PER_CUP; // tablespoons/ounce * ounces/cup
	float TSP_PER_CUP = 3 * TBS_PER_CUP; // teaspoons/tablespoon * tablespoons/ounce * ounces/cup
	float cups;

	printf("Enter an amount in cups:");
	scanf("%f", &cups);
	printf("%f cups is equivalent to:\n", cups);
	printf("%f pints\n", cups * PINTS_PER_CUP);
	printf("%f ounces\n", cups * OUNCES_PER_CUP);
	printf("%f tablespoons\n", cups * TBS_PER_CUP);
	printf("%f teaspoons\n", cups * TSP_PER_CUP);

	return 0;
}
```
