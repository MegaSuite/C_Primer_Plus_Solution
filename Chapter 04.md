# Chapter 04

<h1>Review Questions</h1>

## 4-1
再次运行程序清单`4.1`但是在要求输入名时，请输入名和姓（根据英文书写习惯，名和姓中间有一个空格,看看会发生什么情况？为什么？

> 程序不能正常运行。
> 
> 第1个`scanf()`语句只读取用户输入的名，而用户输入的姓仍留在输入缓冲区中。
> 
> 下一条`scanf()`语句在输入缓冲区查找重量时，从上次读入结束的地方开始读取。这样就把留在缓冲区的姓作为体重来读取，导致`scanf()`读取失败。
> 
> 另一方面，如果在要求输入姓名时输入`Lasha  144`，那么程序会把`144`作为用户的体重（虽然用户是在程序提示输入体重之前输入了144）。

## 4-2
假设下列示例都是完整程序中的一部分，它们打印的结果分别是什么？
```c
	a.printf("He sold the painting for $%2.2f.\n", 2.345e2);

	b.printf("%c%c%c\n", 'H', 105, '\41');

	c.#define Q "His Hamlet was funny without being vulgar."
	  printf("%s\nhas %d characters.\n", Q, strlen(Q));

	d.printf("Is %2.2e the same as %2.2f?\n", 1201.0, 1201.0);
```

> a.He sold the painting for $234.50.
> 
> b.Hi!（注意，第1个字符是字符常量；第2个字符由十进制整数转换而来；第3个字符是八进制字符常量的ASCII表示）
> 
> c.His Hamlet was funny without being vulgar.
> 
> has 42 characters.
> 
> d.Is 1.20e+003 the same as 1201.00?
> 

## 4-3
在第2题的c中，要输出包含双引号的字符串Q，应如何修改？

> 在这条语句中使用`\`"：`printf("\"%s\"\nhas %d characters.\n", Q, strlen(Q));`
> 

## 4-4
找出下面程序中的错误。
```c
define B booboo
define X 10
main(int)
{
	int age;
	char name;
	printf("Please enter your first name.");
	scanf("%s", name);
	printf("All right, %c, what's your age?\n", name);
	scanf("%f", age);
	xp = age + X;
	printf("That's a %s! You must be at least %d.\n", B, xp);
	rerun 0;
}
```

> 下面是修改后的程序：
```c
#include <stdio.h>   /* 别忘了要包含合适的头文件 */
#define B "booboo"   /* 添加#、双引号 */
#define X 10     /* 添加# */
int main(void)    /* 不是main(int) */
{
	int age;
	int xp;      /* 声明所有的变量 */
	char name[40];   /* 把name声明为数组 */
	printf("Please enter your first name.\n"); /* 添加\n，提高可读性 */
	scanf("%s", name);
	printf("All right, %s, what's your age?\n", name); /* %s用于打印字符串*/
	scanf("%d", &age); /* 把%f改成%d，把age改成&age */
	xp = age + X;
	printf("That's a %s! You must be at least %d.\n", B, xp);
	return 0; /* 不是rerun */
}
```
## 4-5
假设一个程序的开头是这样：
```c
#define BOOK "War and Peace"
int main(void)
{
float cost =12.99;
float percent = 80.0;
```
请构造一个使用`BOOK`、`cost`和`percent`的`printf()`语句，打印以下内容：

	This copy of "War and Peace" sells for $12.99.
	That is 80% of list.

> 记住，要打印`%`必须用`%%`：
```c
printf("This copy of \"%s\" sells for $%0.2f.\n", BOOK, cost);
printf("That is %0.0f%% of list.\n", percent);
```

## 4-6
打印下列各项内容要分别使用什么转换说明？

	a.一个字段宽度与位数相同的十进制整数
	b.一个形如8A、字段宽度为4的十六进制整数
	c.一个形如232.346、字段宽度为10的浮点数
	d.一个形如2.33e+002、字段宽度为12的浮点数
	e.一个字段宽度为30、左对齐的字符串
>
	a.%d
	b.%4X
	c.%10.3f
	d.%12.2e
	e.%-30s

## 4-7
打印下面各项内容要分别使用什么转换说明？

	a.字段宽度为15的unsigned long类型的整数
	b.一个形如0x8a、字段宽度为4的十六进制整数
	c.一个形如2.33E+02、字段宽度为12、左对齐的浮点数
	d.一个形如+232.346、字段宽度为10的浮点数
	e.一个字段宽度为8的字符串的前8个字符
>
	a.%15lu
	b.%#4x
	c.%-12.2E 
	d.%+10.3f
	e.%8.8s

## 4-8
打印下面各项内容要分别使用什么转换说明？

	a.一个字段宽度为6、最少有4位数字的十进制整数
	b.一个在参数列表中给定字段宽度的八进制整数
	c.一个字段宽度为2的字符
	d.一个形如+3.13、字段宽度等于数字中字符数的浮点数
	e.一个字段宽度为7、左对齐字符串中的前5个字符
>
	a.%6.4d
	b.%*o
	c.%2c
	d.%+0.2f
	e.%-7.5s

## 4-9
分别写出读取下列各输入行的`scanf()`语句，并声明语句中用到变量和数组。

	a.101
	b.22.32 8.34E−09
	c.linguini
	d.catch 22
	e.catch 22 （但是跳过catch）

```c
a.int dalmations;
  scanf("%d", &dalmations);

b.float kgs, share;
  scanf("%f%f", &kgs, &share);
（注意：对于本题的输入，可以使用转换字符`e`、`f`和`g`。另外，除了`%c`之外，在`%`和转换字符之间加空格不会影响最终的结果）

c.char pasta[20];
  scanf("%s", pasta);

d.char action[20];
  int value;
  scanf("%s %d", action, &value);

e.int value;
  scanf("%*s %d", &value);
```

## 4-10
什么是空白？
>空白包括空格、制表符和换行符。C语言使用空白分隔记号。`scanf()`使用空白分隔连续的输入项。

## 4-11
下面的语句有什么问题？如何修正？

`printf("The double type is %z bytes..\n", sizeof(double));`

> `%z`中的 `z` 是修饰符，不是转换字符，所以要在修饰符后面加上一个它修饰的转换字符。可以使用`%zd`打印十进制数，或用不同的说明符打印不同进制的数，例如，`%zx`打印十六进制的数。

## 4-12
假设要在程序中用圆括号代替花括号，以下方法是否可行？
```c
#define ( {
#define ) }
```


> 可以分别把`(`和`)`替换成`{`和`}`。但是预处理器无法区分哪些圆括号应替换成花括号，哪些圆括号不能替换成花括号。因此，
```c
#define ( {
#define ) }
int main(void)
(
	printf("Hello, O Great One!\n");
)
```
将变成：
```c
int main{void}
{
	printf{"Hello, O Great One!\n"};
}
```

<h1>Programming Exercises</h1>

## 4-1
Write a program that asks for your first name, your last name, and then prints the names in the format last, first.
***
```c
#include <stdio.h>

int main(void)
{
	char first_name[20];
	char last_name[20];

	printf("Enter your first and last name (e.g.: John Doe): ");
	scanf("%s %s", first_name, last_name);
	printf("%s, %s\n", last_name, first_name);

	return 0;
}
```
## 4-2
Write a program that requests your first name and does the following with it:
a. Prints it enclosed in double quotation marks
b. Prints it in a field 20 characters wide, with the whole field in quotes and the name at the right end of the field
c. Prints it at the left end of a field 20 characters wide, with the whole field enclosed in quotes
d. Prints it in a field three characters wider than the name
***
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
	char name[20];
	int name_length;

	printf("Enter your first name: ");
	scanf("%s", name);
	name_length = strlen(name);
	printf("\"%s\"\n", name); // a. enclosed in double quotes
	printf("\"%20s\"\n", name); // b. double quotes, 20 char wide, right-justified
	printf("\"%-20s\"\n", name); // c. double quotes, 20 char wide, left-justified
	printf("\"%*s\"\n", name_length + 3, name); // d. double quotes, 3 char wider than name

	return 0;
}
```

## 4-3
Write a program that reads in a floating-point number and prints it first in
decimal-point notation and then in exponential notation. Have the output use
the following formats (the number of digits shown in the exponent may be
different for your system):

a. The input is 21.3 or 2.1e+001.
b. The input is +21.290 or 2.129E+001.
***
```c
#include <stdio.h>

int main(void)
{
	float num;

	printf("Enter a number: ");
	scanf("%f", &num);
	printf("The input is %.1f or %.1e\n", num, num);
	printf("The input is %+.3f or %.3E\n", num, num);

	return 0;
}
```

## 4-4
Write a program that requests your height in inches and your name, and then displays the information in the following form:

	Dabney, you are 6.208 feet tall

Use type float, and use / for division. If you prefer, request the height in centimeters and display it in meters. 
***
```c
#include <stdio.h>

int main(void)
{
	const float INCHES_PER_FEET = 12;
	float height;
	char name[40];

	printf("What is your name?: ");
	scanf("%s", name);
	printf("What is your height in inches?: ");
	scanf("%f", &height);
	printf("%s, you are %.3f feet tall.\n", name, height / INCHES_PER_FEET);

	return 0;
}
```

## 4-5
Write a program that requests the download speed in megabits per second (Mbs) and the size of a file in megabytes (MB). The program should calculate the download time for the file. Note that in this context one byte is eight bits. Use type float, and use / for division. The program should report all three values (download speed, file size, and download time) showing two digits to the right of the decimal point, as in the following:

	At 18.12 megabits per second, a file of 2.20 megabytes downloads in 0.97 seconds. 
***
```c
#include <stdio.h>

int main(void)
{
	const float BITS_PER_BYTE = 8;
	float download_speed_Mps;
	float file_size_MB;

	printf("Enter the download speed (in megabits/second): ");
	scanf("%f", &download_speed_Mps);
	printf("Enter the file size (in megabytes): ");
	scanf("%f", &file_size_MB);
	printf("At %.2f megabits per second, a file of %.2f megabytes"
		   " downloads in %.2f seconds.\n", download_speed_Mps, file_size_MB,
		   file_size_MB * BITS_PER_BYTE / download_speed_Mps);

	return 0;
}
```

## 4-6
Write a program that requests the user’s first name and then the user’s last
name. Have it print the entered names on one line and the number of letters in
each name on the following line. Align each letter count with the end of the
corresponding name, as in the following:

    Melissa Honeybee 
          7        8

Next, have it print the same information, but with the counts aligned with the
beginning of each name.

    Melissa Honeybee 
    7       8
***
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
	char first_name[20];
	char last_name[20];

	printf("Enter your first and last name: ");
	scanf("%s %s", first_name, last_name);
	printf("\n");
	printf("%s %s\n", first_name, last_name);
	printf("%*lu %*lu\n", // right justified
		   (int) strlen(first_name), strlen(first_name),
		   (int) strlen(last_name), strlen(last_name));
	printf("\n");
	printf("%s %s\n", first_name, last_name);
	printf("%-*lu %-*lu\n", // left justified
		   (int) strlen(first_name), strlen(first_name),
		   (int) strlen(last_name), strlen(last_name));
	printf("\n");

	return 0;
}
```
## 4-7
Write a program that sets a type double variable to 1.0/3.0 and a type float variable to 1.0/3.0. Display each result three times—once showing four digits to the right of the decimal, once showing 12 digits to the right of the decimal, and once showing 16 digits to the right of the decimal. Also have the program include float.h and display the values of FLT_DIG and DBL_DIG. Are the displayed values of 1.0/3.0 consistent with these values?
***
```c
#include <stdio.h>
#include <float.h>

int main(void)
{
	double db_one_third = 1.0 / 3.0;
	float ft_one_third = 1.0 / 3.0;

	printf("Float                Double              \n");
	printf("-------------------- --------------------\n");
	printf("%-20.4f %-20.4f\n", ft_one_third, db_one_third); // show 4 digits
	printf("%-20.12f %-20.12f\n", ft_one_third, db_one_third); // show 12 digits
	printf("%-20.16f %-20.16f\n", ft_one_third, db_one_third);
	printf("\n");
	printf("FLT_DIG: %d\n", FLT_DIG);
	printf("DBL_DIG: %d\n", DBL_DIG);

	/* results: both float and double are accurate to at least the amount of sig
	figs specified by FLT_DIG and DBL_DIG */

	return 0;
}
```
## 4-8
Write a program that asks the user to enter the number of miles traveled and the number of gallons of gasoline consumed. It should then calculate and display the miles-per-gallon value, showing one place to the right of the decimal. Next, using the fact that one gallon is about 3.785 liters and one mile is about 1.609 kilometers, it should convert the mile- per-gallon value to a liters-per-100-km value, the usual European way of expressing fuel consumption, and display the result, showing one place to the right of the decimal.Note that the U. S. scheme measures the distance traveled per amount of fuel (higher is better), whereas the European scheme measures the amount of fuel per distance (lower is better). Use symbolic constants (using const or #define) for the two conversion factors.
***
```c
#include <stdio.h>

int main(void)
{
	const float KM_PER_MILE = 1.609;
	const float LT_PER_GALLON = 3.785;
	float miles_travelled, gallons_gas_consumed;
	float miles_per_gallon, liters_per_100km;

	printf("Enter your distance travelled in miles: ");
	scanf("%f", &miles_travelled);
	printf("Enter the amount of gas consumed in gallons: ");
	scanf("%f", &gallons_gas_consumed);

	// calculate miles per gallon and liters per km
	miles_per_gallon = miles_travelled / gallons_gas_consumed;
	liters_per_100km = 100. / miles_per_gallon * LT_PER_GALLON / KM_PER_MILE;

	printf("Miles per gallon: %.1f\n", miles_per_gallon);
	printf("Liters per 100 kilometers: %.1f\n", liters_per_100km);

	return 0;
}
```
