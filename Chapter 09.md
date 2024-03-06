# Chapter 09

<h1>Review Questions</h1>

## 9-1
实际参数和形式参数的区别是什么？

> 形式参数是定义在被调函数中的变量。实际参数是出现在函数调用中的值，该值被赋给形式参数。可以把实际参数视为在函数调用时初始化形式参数的值。

## 9-2
根据下面各函数的描述，分别编写它们的ANSI C函数头。注意，只需写出函数头，不用写函数体。
```c
a.donut()接受一个int类型的参数，打印若干（参数指定数目）个0
b.gear()接受两个int类型的参数，返回int类型的值
c.guess()不接受参数，返回一个int类型的值
d.stuff_it()接受一个double类型的值和double类型变量的地址，把第1个值储存在指定位置
```

```c
a.void donut(int n)
b.int gear(int t1, int t2)
c.int guess(void)
d.void stuff_it(double d, double *pd)
```

## 9-3
根据下面各函数的描述，分别编写它们的ANSI C函数头。注意，只需写出函数头，不用写函数体。
```c
a.n_to_char()接受一个int类型的参数，返回一个char类型的值
b.digit()接受一个double类型的参数和一个int类型的参数，返回一个int类型的值
c.which()接受两个可储存double类型变量的地址，返回一个double类型
的地址
d.random()不接受参数，返回一个int类型的值
```

```c
a.char n_to_char(int n)
b.int digits(double x, int n)
c.double * which(double * p1, double * p2)
d.int random(void)
```

## 9-4
设计一个函数，返回两整数之和。
```c
int sum(int a, int b)
{
	return a + b;
}
```

## 9-5
如果把复习题4改成返回两个double类型的值之和，应如何修改函数？
```c
double sum(double a, double b)
{
	return a + b;
}
```

## 9-6
设计一个名为alter()的函数，接受两个int类型的变量x和y，把它们的值分别改成两个变量之和以及两变量之差。
```c
void alter(int * pa, int * pb)
{
	int temp;
	temp = *pa + *pb;
	*pb = *pa - *pb;
	*pa = temp;
}
```

## 9-7
下面的函数定义是否正确？
```c
void salami(num)
{
	int num, count;
	for (count = 1; count <= num; num++)
		printf(" O salami mio!\n");
}
```
```c
不正确。num应声明在salami()函数的参数列表中，而不是声明在函数体中。另外，把count++改成num++。
```

## 9-8
编写一个函数，返回3个整数参数中的最大值。
```c
int largest(int a, int b, int c)
{
	int max = a;
	if (b > max)
		max = b;
	if (c > max)
		max = c;
	return max;
}
```

## 9-9
给定下面的输出：
```c
Please choose one of the following:
1) copy files         2) move files
3) remove files       4) quit
Enter the number of your choice:
```
a.编写一个函数，显示一份有4个选项的菜单，提示用户进行选择（输出如上所示）。

b.编写一个函数，接受两个int类型的参数分别表示上限和下限。该函数从用户的输入中读取整数。如果整数超出规定上下限，函数再次打印菜单（使用a部分的函数）提示用户输入，然后获取一个新值。如果用户输入的整数在规定范围内，该函数则把该整数返回主调函数。如果用户输入一个非整数字符，该函数应返回4。

c.使用本题a和b部分的函数编写一个最小型的程序。最小型的意思是，该程序不需要实现菜单中各选项的功能，只需显示这些选项并获取有效的响应即可。

```c
//下面是一个最小的程序，showmenu()和getchoice()函数分别是a和b的答案。
#include <stdio.h>

void showmenu(void);
int getchoice(int, int);

int main()
{
	int res;
	showmenu();

	while ((res = getchoice(1, 4)) != 4)
	{
		printf("I like choice %d.\n", res);
		showmenu();
	}
	printf("Bye!\n");
	return 0;
}


void showmenu(void)
{
	printf("Please choose one of the following:\n");
	printf("1) copy files     2) move files\n");
	printf("3) remove files    4) quit\n");
	printf("Enter the number of your choice:\n");
}


int getchoice(int low, int high)
{
	int ans;
	int good;
	good = scanf("%d", &ans);
	
	while (good == 1 && (ans < low || ans > high))
	{
		printf("%d is not a valid choice; try again\n", ans);
		showmenu();
		scanf("%d", &ans);
	}

	if (good != 1)
	{
		printf("Non-numeric input.");
		ans = 4;
	}

	return ans;
}
```


<h1>Programming Exercises</h1>

## 9-1 

Devise a function called min(x,y) that returns the smaller of two double values. Test the function with a simple driver.
***
```c
#include <stdio.h>

double min(double, double);

int main(void)
{
	double x, y;

	printf("Enter two doubles (non-double input to quit): ");
	while (scanf("%lf %lf", &x, &y) == 2)
	{
		printf("The minimum of %.3f and %.3f is %.3f.\n", x, y, min(x,y));
		printf("Enter two doubles (non-double input to quit): ");
	}

	return 0;
}

double min(double x, double y)
{
	return x < y ? x : y;
    // return the minimum of x and y
}
```

## 9-2 
Devise a function chline(ch,i,j) that prints the requested character in columns i through j. Test it in a simple driver.
***
```c
#include <stdio.h>

void chline(char, unsigned int, unsigned int);

int main(void)
{
	char ch;
	unsigned int i, j;

	printf("Enter a character and two integers: ");
	while (scanf("%c %u %u", &ch, &i, &j) == 3)
	{
		chline(ch, i, j);
		printf("\n");

		while (getchar() != '\n') continue; // clear input
		
		printf("Enter a character and two integers: ");
	}

	return 0;
}

void chline(char ch, unsigned int i, unsigned int j)
{
	unsigned int col;
	for (col = 1; col < i; col++)
	{
		putchar(' ');
	}

	for (; col <= j; col++)
	{
		putchar(ch);
	}
}
```

## 9-3
Write a function that takes three arguments: a character and two integers.The character is to be printed. The first integer specifies the number of times that the character is to be printed on a line, and the second integer specifies the number of lines that are to be printed. Write a program that makes use of this function.
***
```c
#include <stdio.h>

void printgrid(char ch, unsigned int cols, unsigned int rows);

int main(void)
{
	char ch;
	unsigned int rows, cols;

	printf("Enter a character, number of rows and number of columns: ");
	while (scanf("%c %u %u", &ch, &rows, &cols) == 3)
	{
		printgrid(ch, cols, rows);
		printf("Enter a character, number of rows and number of columns: ");
	}

	return 0;
}

void printgrid(char ch, unsigned int cols, unsigned int rows)
{
	// prints given character in a block sized rows x cols
	for (unsigned int i = 0; i < rows; i++)
	{
		for (unsigned int j = 0; j < cols; j++)
		{
			putchar(ch);
		}
		putchar('\n');
	}
}
```

## 9-4
The harmonic mean of two numbers is obtained by taking the inverses of the two numbers, averaging them, and taking the inverse of the result. Write a function that takes two double arguments and returns the harmonic mean of the two numbers.
***

```c
#include <stdio.h>

double harmonic_mean(double, double);

int main(void)
{
	double x, y;

	printf("Harmonic means\n");
	printf("Enter two numbers: ");
	while (scanf("%lf %lf", &x, &y) == 2)
	{
		printf("%f\n", harmonic_mean(x, y));

		printf("Enter two numbers: ");
	}

	return 0;
}

double harmonic_mean(double x, double y)
{
	return 2 / (1 / x + 1 / y);
}
```
## 9-5
Write and test a function called larger_of() that replaces the contents of two double variables with the maximum of the two values. For example,larger_of(x,y) would reset both x and y to the larger of the two.
***
```c
#include <stdio.h>

void larger_of(double * x, double * y);

int main(void)
{
	double x, y;

	printf("Test larger_of() function\n");
	printf("=========================\n");
	printf("Enter two numbers x and y: ");
	while(scanf("%lf %lf", &x, &y) == 2)
	{
		printf("Before calling larger_of():\n");
		printf("x = %f, y = %f\n", x, y);

		larger_of(&x, &y);

		printf("After calling larger_of():\n");
		printf("x = %f, y = %f\n", x, y);	

		printf("Enter two numbers x and y: ");
	}

	return 0;
}

void larger_of(double * x, double * y)
{
	// replace contents of 
	if (*x > *y) *y = *x;
	else *x = *y;
}
```
## 9-6
Write and test a function that takes the addresses of three double variables as arguments and that moves the value of the smallest variable into the first variable, the middle value to the second variable, and the largest value into the third variable.
***
```c
#include <stdio.h>

void sort_variables(double *x, double *y, double *z);

int main(void)
{
	double x, y, z;

	printf("Test sort_variables():\n");
	printf("Enter three numbers x, y and z:\n");
	while(scanf("%lf %lf %lf", &x, &y, &z) == 3)
	{
		putchar('\n');
		printf("Before calling sort_variables:\n");
		printf("x = %f, y = %f, z = %f\n", x, y, z);

		sort_variables(&x, &y, &z);

		putchar('\n');
		printf("After calling sort_variables:\n");
		printf("x = %f, y = %f, z = %f\n", x, y, z);

		putchar('\n');

		printf("Enter three numbers x, y and z:\n");
	}

	return 0;
}

void sort_variables(double *x, double *y, double *z)
{
	double tmp;

	if (*x > *y)
	{
		// switch x and y
		tmp = *y;
		*y = *x;
		*x = tmp;
	}

	if (*y > *z)
	{
		// switch y and z
		tmp = *z;
		*z = *y;
		*y = tmp;

		if (*x > *y)
		{
			// switch x and y
			tmp = *y;
			*y = *x;
			*x = tmp;
		}
	}
}

```
## 9-7
Write a program that reads characters from the standard input to end-of-file. For each character, have the program report whether it is a letter. If it is a letter, also report its numerical location in the alphabet. For example, c and C would both be letter 3.Incorporate a function that takes a character as an argument and returns the numerical location if the character is a letter and that returns –1 otherwise.

***
```c
#include <stdio.h>
#include <ctype.h>

int letter_location(char ch);

int main(void)
{
	char ch;
	int location;

	while ((ch = getchar()) != EOF)
	{
		if ((location = letter_location(ch)) == -1)
			printf("%c is not a letter\n", ch);
		else
			printf("%c is a letter: location = %d\n", ch, location);
	} 

	return 0;
}

int letter_location(char ch)
{
	if (isalpha(ch))
	{
		ch = tolower(ch);
		return (ch - 'a' + 1);
	}
	else
		return -1;
}
```
## 9-8
Chapter 6, “C Control Statements: Looping,” (Listing 6.20) shows a power()function that returned the result of raising a type double number to a positive integer value. Improve the function so that it correctly handles negative powers. Also, build into the function that 0 to any power other than 0 is 0 and that any number to the 0 power is 1. (It should report that 0 to the 0 is undefined, then say it’s using a value of 1.) Use a loop. Test the function in a program.
***
```c
#include <stdio.h>
#include <stdlib.h> // prototype for abs() 

double power(double base, int exponent);

int main(void)
{
	double base, output;
	int exponent;

	printf("Test power() function:\n");
	printf("Enter a :double: base and :int: exponent: ");
	while (scanf("%lf %d", &base, &exponent) == 2)
	{
		output = power(base, exponent);

		printf("%f ^ %d = %f \n", base, exponent, output);

		printf("Enter a :double: base and :int: exponent: ");
	}

	return 0;
}

double power(double base, int exponent)
{
	double power = base;

	if (base == 0)
	{
		if (exponent == 0)
		{
			printf("Warning: 0 ^ 0 is undefined. Using 1.\n");
			return 1.0;
		}
		else
			return 0;
	}

	for (int i = 1; i < abs(exponent); i++)
	{
		power *= base;
	}

	if (exponent < 0)
		power = 1 / power;

	return power;
}
```
## 9-9
Redo Programming Exercise 8, but this time use a recursive function.
***
```c
#include <stdio.h>
#include <stdlib.h> // prototype for abs() 

double power(double base, int exponent);

int main(void)
{
	double base, output;
	int exponent;

	printf("Test power() function:\n");
	printf("Enter a :double: base and :int: exponent: ");
	while (scanf("%lf %d", &base, &exponent) == 2)
	{
		output = power(base, exponent);

		printf("%f ^ %d = %f \n", base, exponent, output);

		printf("Enter a :double: base and :int: exponent: ");
	}

	return 0;
}

double power(double base, int exponent)
{
	double dbl_power;

	// handle powers of zero
	if (base == 0)
	{
		if (exponent == 0)
		{
			printf("Warning: 0 ^ 0 is undefined. Using 1.\n");
			return 1.0;
		}
		else
			return 0;
	}

	if (exponent == 0) return 1; // stop recursion

	dbl_power = base * power(base, abs(exponent) - 1); // recursion step

	// if exponent is negative, take reciprocal
	if (exponent < 0) dbl_power = 1 / dbl_power;

	return dbl_power;
}
```
## 9-10
Generalize the to_binary() function of Listing 9.8 to a to_base_n() function that takes a second argument in the range 2–10. It then should print the number that is its first argument to the number base given by the second argument. For example, to_ base_n(129,8) would display 201, the base-8 equivalent of 129. Test the function in a complete program.
***
```c
#include <stdio.h>
#include <stdlib.h>

void to_base_n(int integer, int base);

int main(void) {
	int integer, base;

	printf("Test to_base_n() function\n");
	printf("Enter an integer in base 10 and a base to convert to: ");
	while (scanf("%d %d", &integer, &base) == 2)
	{
		to_base_n(integer, base);
		putchar('\n');

		printf("Enter an integer in base 10 and a base to convert to: ");
	}

	return 0;
}

void to_base_n(int integer, int base)
{
	// handle invalid bases
	if (base < 2 || 10 < base)
	{
		printf("Error: base must be between 2 and 10.");
		return;
	}

	// stop recursion
	if (integer == 0) return;

	// handle negative integers
	if (integer < 0)
	{
		putchar('-');
		integer = abs(integer);
	}

	to_base_n(integer / base, base);
	printf("%d", integer % base);
	return;

}
```
## 9-11
Write and test a Fibonacci() function that uses a loop instead of recursion to calculate Fibonacci numbers.
***
```c
#include <stdio.h>	

long Fibonacci(long n);

int main(void) {
	long n;

	printf("Test Fibonacci() function\n");
	printf("Enter an integer n: ");

	while (scanf("%ld", &n) == 1)
	{
		printf("Fibonacci #%ld = %ld\n", n, Fibonacci(n));

		printf("Enter an integer n: ");
	}

	return 0;
}

long Fibonacci(long n)
{
	// return nth Fibonacci number

	// handle invalid arguments
	if (n < 1)
	{
		printf("Error: n must be a positive integer.\n");
		return -1;
	}

	long fib_n1 = 1, fib_n2 = 1, fib_n;

	// n equals 1 or 2
	if (n == 1) return 1;
	if (n == 2) return 1;

	// n greater than or equal to 3
	for (long i = 3; i <= n; i++)
	{
		// update old values
		fib_n1 = fib_n2;
		fib_n2 = fib_n;
		fib_n = fib_n1 + fib_n2;
	}

	return fib_n;
}
```
