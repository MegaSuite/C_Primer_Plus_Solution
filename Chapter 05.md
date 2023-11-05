# Chapter 05

<h1>Review Questions</h1>

## 5-1
假设所有变量的类型都是`int`，下列各项变量的值是多少：

	a.x = (2 + 3) * 6;
	b.x = (12 + 6)/2*3;
	c.y = x = (2 + 3)/4;
	d.y = 3 + 2*(x = 7/2);

> a.30
> 
> b.27。不是(12+6)/(2*3)得3。
> 
> c.x = 1，y = 1（整数除法）。
> 
> d.x = 3（整数除法），y = 9。

## 5-2
假设所有变量的类型都是int，下列各项变量的值是多少：

	a.x = (int)3.8 + 3.3;
	b.x = (2 + 3) * 10.5;
	c.x = 3 / 5 * 22.0;
	d.x = 22.0 * 3 / 5;

> a.6（由3 + 3.3截断而来）
> 
> b.52(由5 * 10.5截断而来)
> 
> c.0（0 * 22.0的结果）
> 
> d.13（66.0 / 5 = 13.2，然后把结果赋给int类型变量）

## 5-3
对下列各表达式求值：

	a.30.0 / 4.0 * 5.0;
	b.30.0 / (4.0 * 5.0);
	c.30 / 4 * 5;
	d.30 * 5 / 4;
	e.30 / 4.0 * 5;
	f.30 / 4 * 5.0;

> a.37.5（7.5 * 5.0的结果）
> 
> b.1.5（30.0 / 20.0的结果）
> 
> c.35（7 * 5的结果，整形运算）
> 
> d.37（150 / 4的结果）
> 
> e.37.5（7.5 * 5的结果）
> 
> f.35.0（7 * 5.0的结果，先整形后浮点）

## 5-4
请找出下面的程序中的错误。
```c
int main(void)
{
	int i = 1,
	float n;
	printf("Watch out! Here come a bunch of fractions!\n");
	while (i < 30)
	n = 1/i;
	printf(" %f", n);
	printf("That's all, folks!\n");
	return;
}
```
	第0行：应增加一行`#include <stdio.h>`。
	第3行：末尾用分号，而不是逗号。
	第6行：`while`语句创建了一个无限循环。因为i的值始终为1，所以它总是小于30。应该是while(i++ < 30)。
	第6～8行：这样的缩进布局不能使第7行和第8行组成一个代码块。由于没有用花括号括起来， while循环只包括第7行，所以要添加花括号。
	第7行：因为1和i都是整数，所以当i为1时，除法的结果是1；当i为更大的数时，除法结果为0。用n  =  1.0/i，i在除法运算之前会被转换为浮点数，这样就能得到非零值。
	第8行：在格式化字符串中没有换行符（\n），这导致数字被打印成一行。
	第10行：应该是return 0;
```c
#include <stdio.h>
int main(void)
{
	int i = 1;
	float n;
	printf("Watch out! Here come a bunch of fractions!\n");
	while (i++ < 30)
	{
		n = 1.0/i;
		printf(" %f\n", n);
	}
	printf("That's all, folks!\n");
	return 0;
}
```
## 5-5
这是程序清单`5.9`的另一个版本。从表面上看，该程序只使用了一条`scanf()`语句，比程序清单`5.9`简单。请找出不如原版之处。
```c
#include <stdio.h>
#define S_TO_M 60
int main(void)
{
	int sec, min, left;
	printf("This program converts seconds to minutes and ");
	printf("seconds.\n");
	printf("Just enter the number of seconds.\n");
	printf("Enter 0 to end the program.\n");
	while (sec > 0) 
	{
		scanf("%d", &sec);
		min = sec/S_TO_M;
		left = sec % S_TO_M;
		printf("%d sec is %d min, %d sec. \n", sec, min, left);
		printf("Next input?\n");
	}
	printf("Bye!\n");
	return 0;
}
```
这个版本最大的问题是测试条件（`sec`是否大于`0`？）和`scanf()`语句获取`sec`变量的值之间的关系。具体地说，第一次测试时，程序尚未获得`sec`的值，用来与0作比较的是正好在`sec`变量内存位置上的一个垃圾值。一个比较笨拙的方法是初始化`sec`（如，初始化为`1`）。这样就可通过第一次测试。当最后输入`0`结束程序时，在循环结束之前不会检查`sec`，所以`0`也被打印了出来。因此，更好的方法是在`while`测试之前使用`scanf()`语句。`while`循环第一轮迭代使用的是`scanf()`在循环外面获取的值。因此，在`while`循环的末尾还要使用一次`scanf()`语句。这是处理类似问题的常用方法。
```c
scanf("%d", &sec);
while ( sec > 0 ) 
{
	min = sec/S_TO_M;
	left = sec % S_TO_M;
	printf("%d sec is %d min, %d sec.\n", sec, min, left);
	printf("Next input?\n");
	scanf("%d", &sec);
}
```

## 5-6
下面的程序将打印出什么内容？
```c
#include <stdio.h>
#define FORMAT "%s! C is cool!\n"
int main(void)
{
	int num = 10;
	printf(FORMAT,FORMAT);
	printf("%d\n", ++num);
	printf("%d\n", num++);
	printf("%d\n", num--);
	printf("%d\n", num);
	return 0;
}
```
```c
%s! C is cool!
! C is cool!
11
11
12
11
```
	第1个`printf()`语句与下面的语句相同：
	`printf("%s! C is cool!\n","%s! C is cool!\n");`
	第2个`printf()`语句首先把`num`递增为`11`，然后打印该值。
	第3个`printf()`语句打印`num`的值（值为11）。
	第4个`printf()`语句打印`n`当前的值（仍为12），然后将其递减为`11`。
	最后一个`printf()`语句打印`num`的当前值（值为11）。

## 5-7
下面的程序将打印出什么内容？
```c
#include <stdio.h>
int main(void)
{
	char c1, c2;
	int diff;
	float num;
	c1 = 'S';
	c2 = 'O';
	diff = c1 - c2;
	num = diff;
	printf("%c%c%c:%d %3.2f\n", c1, c2, c1, diff, num);
	return 0;
}
```
`SOS:4 4.00`

> 表达式`c1 -c2`的值和`'S' - '0'`的值相同（其对应的`ASCII`值是`83 - 79`）。

## 5=8
下面的程序将打印出什么内容？
```c
#include <stdio.h>
#define TEN 10
int main(void)
{
	int n = 0;
	while (n++ < TEN)
	printf("%5d", n);
	printf("\n");
	return 0;
}
```
> 把`1～10`打印在一行，每个数字占`5`列宽度，然后开始新的一行：
> 
> `1 2 3 4 5 6 7 8 9 10`

## 5-9
修改上一个程序，使其可以打印字母`a～g`。
```c
#include <stdio.h>
int main(void)
{
	char ch = 'a';
	while (ch <= 'g')
	printf("%5c", ch++);
	printf("\n");
	return 0;
}
```

## 5-10
假设下面是完整程序中的一部分，它们分别打印什么？
```c
a.
int x = 0;
while (++x < 3)
printf("%4d", x);
b.
int x = 100;
while (x++ < 103)
	printf("%4d\n",x);
	printf("%4d\n",x);
c.
char ch = 's';
while (ch < 'w')
{
	printf("%c", ch);
	ch++;
}
printf("%c\n",ch);
```

```c
a.
1 2
//注意，先递增x的值再比较。光标仍留在同一行。

b.
101
102
103
104
/*
注意，这次x先比较后递增。在示例a和b中，x都是在先递增后打印。
另外还要注意，虽然第2个`printf()`语句缩进了，但是这并不意味着它是while循环的一部分。
因此，在while循环结束后，才会调用一次该printf()语句。
*/

c.
stuvw
//该例中，在第1次调用printf()语句后才会递增ch
```

## 5-11
下面的程序会打印出什么？
```c
#define MESG "COMPUTER BYTES DOG"
#include <stdio.h>
int main(void)
{
	int n = 0;
	while ( n < 5 )
	printf("%s\n", MESG);
	n++;
	printf("That's all.\n");
	return 0;
}
```
	`while`循环没有用花括号把两个缩进的语句括起来，只有`printf()`是循环的一部分，
	所以该程序一直重复打印消息`COMPUTER BYTES DOG`，直到强行关闭程序为止。

## 5-12
分别编写一条语句，完成下列各任务（或者说，使其具有以下副作用）：

	a.将变量x的值增加10
	b.将变量x的值增加1
	c.将a与b之和的两倍赋给c
	d.将a与b的两倍之和赋给c

```c
a.x = x + 10;
//or x += 10;
b.x++; 
//or ++x; 
//or x = x + 1;
c.c = 2 * (a + b);
d.c = a + 2* b;
```

## 5-13
分别编写一条语句，完成下列各任务：

	a.将变量x的值减少1
	b.将n除以k的余数赋给m
	c.q除以b减去a，并将结果赋给p
	d.a与b之和除以c与d的乘积，并将结果赋给x
```c
a.x--; 
//or --x; 
//or x = x - 1;
b.m = n % k;
c.p = q / (b - a);
d.x = (a + b) / (c * d);
```


<h1>Programming Exercises</h1>

## 5-1
Write a program that converts time in minutes to time in hours and minutes. Use `#define` or `const` to create a symbolic constant for 60. Use a while loop to allow the user to enter values repeatedly and terminate the loop if a value for the time of 0 or less is entered. 
***
```c
#include <stdio.h>

const int MINUTES_PER_HOUR = 60;

int main(void)
{
	int minutes;

	printf("Enter an amount of time in minutes: "); // get first input
	scanf("%d", &minutes);
		
	while (minutes > 0)
	{
		printf("%d minute(s) is %d hour(s) and %d minute(s).\n",
			   minutes,
			   minutes / MINUTES_PER_HOUR, // hours
			   minutes % MINUTES_PER_HOUR); // minutes

		printf("Enter an amount of time in minutes: "); // get new input
		scanf("%d", &minutes);
	}

	return 0;
}
```

## 5-2
Write a program that asks for an integer and then prints all the integers from (and including) that value up to (and including) a value larger by 10. (That is, if the input is 5, the output runs from 5 to 15.) Be sure to separate each output value by a space or tab or newline.

---
```c
#include <stdio.h>

int main(void)
{
	int input;
	int i = 0;

	printf("Enter an integer: ");
	scanf("%d", &input);
	while (i <= 10)
	{
		printf("%d\n", input + i);
		i++;
	}

	return 0;
}
```


## 3-5
Write a program that asks the user to enter the number of days and then converts that value to weeks and days .For example, it would convert 18 days to 2 weeks, 4 days. Display results in the following format:

    18 days are 2 weeks, 4 days.

Use a while loop to allow the user to repeatedly enter day values; terminate the loop when the user enters a nonpositive value, such as 0 or -20.

---
```c
#include <stdio.h>

const int DAYS_PER_WEEK = 7;

int main(void)
{
	int days;

	printf("Enter a number of days (or enter 0 to quit): ");
	scanf("%d", &days);
	while (days > 0)
	{
		printf("%d days are %d weeks, %d days.\n", 
				days, 
				days / DAYS_PER_WEEK,
				days % DAYS_PER_WEEK);

		printf("Enter a number of days (or enter 0 to quit): ");
		scanf("%d", &days);
	}

	return 0;
}
```

## 5-4
Write a program that asks the user to enter a height in centimeters and then displays the height in centimeters and in feet and inches. Fractional centimeters and inches should be allowed, and the program should allow the user to continue entering heights until a nonpositive value is entered. A sample run should look like this:

    Enter a height in centimeters: 182
    182.0 cm = 5 feet, 11.7 inches
    Enter a height in centimeters (<=0 to quit): 168.7 
    168.0 cm = 5 feet, 6.4 inches
    Enter a height in centimeters (<=0 to quit): 0
    bye

---
```c
#include <stdio.h>

const float CM_PER_IN = 2.54;
const int IN_PER_FT = 12;

int main(void)
{
	float height_cm, height_in, inches;
	int feet;

	printf("Enter a height in centimeters: ");
	scanf("%f", &height_cm);

	while (height_cm > 0)
	{
		height_in = height_cm / CM_PER_IN; // convert height to inches
		feet = (int) height_in / IN_PER_FT; // get number of feet in height
		inches = height_in - feet * IN_PER_FT; // get remaining inches

		printf("%.1f cm = %d feet, %.1f inches\n",
		       height_cm, feet, inches);

		printf("Enter a height in centimeters (<= 0 to quit): ");
		scanf("%f", &height_cm);
	}

	printf("bye\n");
	return 0;
}
```

## 5-5
Change the program addemup.c (Listing 5.13), which found the sum of the first 20 integers. (If you prefer, you can think of addemup.c as a program that calculates how much money you get in 20 days if you receive $1 the first day, $2 the second day, $3 the third day, and so on.) Modify the program so that you can tell it interactively how far the calculation should proceed. That is, replace the 20 with a variable that is read in.

---
```c
#include <stdio.h>

int main(void)
{
	int count, sum, max_count;
	sum = 0;
	count = 1;

	printf("How many integers would you like to sum? ");
	scanf("%d", &max_count);
	while (count <= max_count)
	{
		sum = sum + count;
		count++;
	}
	printf("sum = %d\n", sum);

	return 0;
}
```

## 5-6
Now modify the program of Programming Exercise 5 so that it computes the sum of the squares of the integers. (If you prefer, how much money you receive if you get $1 the first day, $4 the second day, $9 the third day, and so on. This looks like a much better deal!) C doesn’t have a squaring function, but you can use the fact that the square of n is `n * n`.

---
```c
#include <stdio.h>

int main(void)
{
	int sum, count, max_count;
	sum = 0;
	count = 1;

	printf("How many squares would you like to sum? ");
	scanf("%d", &max_count);
	while (count <= max_count)
	{
		sum = sum + count * count;
		count++;
	}
	printf("The sum of the first %d squares is: %d\n", max_count, sum);

	return 0;
}
```

## 5-7
Write a program that requests a type double number and prints the value of the number cubed. Use a function of your own design to cube the value and print it. The `main()` program should pass the entered value to this function.

---
```c
#include <stdio.h>

double cubed(double n); // prototype declaration for cubed

int main(void)
{
	double input;
	printf("Enter a number to cube: ");
	scanf("%lf", &input);

	printf("%.3f cubed is %.3f\n", input, cubed(input));

	return 0;
}

double cubed(double n)
{
	return n * n * n;
}
```

## 5-8
Write a program that displays the results of applying the modulus operation. The user should first enter an integer to be used as the second operand, which will then remain unchanged. Then the user enters the numbers for which the modulus will be computed, terminating the process by entering 0 or less. A sample run should look like this:

    > This program computes moduli.
    > Enter an integer to serve as the second operand: 256
    > Now enter the first operand: 438
    > 438 % 256 is 182
    > Enter next number for first operand (<= 0 to quit): 1234567
    > 1234567 % 256 is 135
    > Enter next number for first operand (<= 0 to quit): 0
    > Done

---
```c
#include <stdio.h>

int main(void)
{
	int first, second;
	printf("This program computes moduli.\n");
	printf("Enter an integer to serve as the second operand: ");
	scanf("%d", &second);
	printf("Now enter the first operand: ");
	scanf("%d", &first);
	while (first > 0)
	{
		printf("%d %% %d is %d\n", first, second, first % second); //print results

		printf("Enter next number for first operand (<= 0 to quit): ");
		scanf("%d", &first); // get new input
	}

	return 0;
}
```

## 5-9
Write a program that requests the user to enter a Fahrenheit temperature. The program should read the temperature as a type double number and pass it as an argument to a user-supplied function called `Temperatures()`. 

This function should calculate the Celsius equivalent and the Kelvin equivalent and display all three temperatures with a precision of two places to the right of the decimal. It should identify each value with the temperature scale it represents. 

Here is the formula for converting Fahrenheit to Celsius:

`Celsius = 5.0 / 9.0 * (Fahrenheit - 32.0)` 

The Kelvin scale, commonly used in science, is a scale in which 0 represents absolute zero, the lower limit to possible temperatures. 

Here is the formula for converting Celsius to Kelvin: 

`Kelvin = Celsius + 275.16` 

The `Temperatures()` function should use const to create symbolic representations of the three constants that appear in the conversions. 

The `main()` function should use a loop to allow the user to enter temperatures repeatedly, stopping when a q or other nonnumeric value is entered. 

Use the fact that `scanf()` returns the number of items read, so it will `return 1` if it reads a number, but it won’t return 1 if the user enters q. 

The `== `operator tests for equality, so you can use it to compare the return value of `scanf()` with 1.

---
```c
#include <stdio.h>

void Temperatures(double fahr); 
// prototype declaration of Temperatures

int main(void)
{
	double fahr;
	printf("This program converts fahrenheit to celsius and kelvin.\n");
	printf("Enter a temperature in degrees fahrenheit (q to quit): ");
	while (scanf("%lf", &fahr) == 1) 
	// continue executing loop if user enters valid number
	{
		Temperatures(fahr); 
		// convert fahr to celsius and kelvin

		// prompt for new input
		printf("Enter a temperature in degrees fahrenheit (q to quit): ");
	}

	printf("bye\n");
}

void Temperatures(double fahr)
{
	const double FAHR_TO_CEL_SCALE = 5.0 / 9.0;
	const double FAHR_TO_CEL_OFFSET = -32.0;
	const double CEL_TO_KEL_OFFSET = 273.16;

	double celsius = (fahr + FAHR_TO_CEL_OFFSET) * FAHR_TO_CEL_SCALE;
	double kelvin = celsius + CEL_TO_KEL_OFFSET;

	printf("%.2f degrees fahrenheit is %.2f degrees celsius or %.2f degrees kelvin.\n",
			fahr, 
			celsius, 
			kelvin);
}
```