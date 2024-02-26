# Chapter 07

<h1>Review Questions</h1>

## 7-1
判断下列表达式是true还是false。
```c
a 100 > 3 && 'a'>'c'
b 100 > 3 || 'a'>'c'
c !(100>3)
```

> b是true，其他都是false。

## 7-2
根据下列描述的条件，分别构造一个表达式：
```c
a  number等于或大于90，但是小于100
b  h不是字符q或k
c  number在1～9之间（包括1和9），但不是5
d  number不在1～9之间
```

> a.number >= 90 && number < 100
> 
> b.ch != 'q' && ch != 'k'
> 
>c.(number >= 1 && number <= 9) && number != 5
>
>d.number < 1 || number > 9

## 7-3
下面的程序关系表达式过于复杂，而且还有些错误，请简化并改正。
```c
#include <stdio.h>
int main(void)                  /* 1 */
{                        /* 2 */
	int weight, height; /* weight以磅为单位，height以英寸为单位 */
											/* 4 */
	scanf("%d , weight, height);          /* 5 */
	if (weight < 100 && height > 64)        /* 6 */
		if (height >= 72)             /* 7 */
			printf("You are very tall for your weight.\n");
		else if (height < 72 &&> 64)        /* 9 */
			printf("You are tall for your weight.\n");/* 10 */
	else if (weight > 300 && !(weight <= 300)  /* 11 */
		&& height < 48)            /* 12 */
		if (!(height >= 48))            /* 13 */
			printf(" You are quite short for your weight.\n");
	else                     /* 15 */
		printf("Your weight is ideal.\n");      /* 16 */
													/* 17 */
	return 0;
}
```

```c
- 第5行：应该是 scanf("%d  %d",  &weight,  &height); 这一行前面应该有提示用户输入的语句。
- 第9行：测试条件中要表达的意思是(height < 72 && height > 64)。根据前面第7行中的测试条件，能到第9行的height一定小于72，所以，只需要用表达式(height > 64)即可。但是，第6行中已经包含了height > 64这个条件，所以这里完全不必再判断，else if 应改成else。
- 第11行：条件冗余。第2个表达式（weight不小于或不等于300）和第1个表达式含义相同。只需用一个简单的表达式(weight > 300)即可。但是，问题不止于此。第  11  行是一个错误的if，这行的else  if与第6行的if匹配。但是，根据if的“最接近规则”，该else  if应该与第9行的else  if匹配。因此，在weight小于100且小于或等于64时到达第11行，此情况难以成立。
- 第7行～第10行：应该用花括号括起来。这样第11行就确定与第6行匹配。但是，如果把第9行的else if替换成简单的else，就不需要使用花括号。
- 第13行：应简化成if (height > 48)。实际上，完全可以省略这一行。因为第12行已经测试过该条件。
下面是修改后的版本：
#include <stdio.h>
int main(void)
{
	int weight, height; /* weight in lbs, height in inches */
	printf("Enter your weight in pounds and ");
	printf("your height in inches.\n");
	scanf("%d %d", &weight, &height);
	if (weight < 100 && height > 64)
	{
		if (height >= 72)
			printf("You are very tall for your weight.\n");
		else
			printf("You are tall for your weight.\n");
	}
	else if (weight > 300 && height < 48)
		printf(" You are quite short for your weight.\n");
	else
		printf("Your weight is ideal.\n");
	return 0;
}
```	

## 7-4
下列表达式的值是多少？
```c
a.5 > 2
b.3 + 4 > 2 && 3 < 2
c.x >= y || y > x
d.d = 5 + ( 6 > 2 )
e.'X' > 'T' ? 10 : 5
f.x > y ? y > x : x > y
```

> a.1。5确实大于2，表达式为真，即是1。
>
> b.0。3比2大，表达式为假，即是0。
> 
> c.1。如果第1个表达式为假，则第2个表达式为真，反之亦然。所以，只要一个表达式为真，整个表达式的结果即为真。
>
> d.6。因为6 > 2为真，所以(6 > 2)的值为1。
>
> e.10。因为测试条件为真。
>
>f.0。如果x > y为真，表达式的值就是y > x，这种情况下它为假或0。如果x > y为假，那么表达式的值就是x > y，这种情况下为假。

## 7-5
下面的程序将打印什么？
```c
#include <stdio.h>
int main(void)
{
	int num;
	for (num = 1; num <= 11; num++)
	{
		if (num % 3 == 0)
			putchar('$');
		else
			putchar('*');
			putchar('#');
		putchar('%');
	}
	putchar('\n');
	return 0;
}
```

```c
该程序打印以下内容：
*#%*#%$#%*#%*#%$#%*#%*#%$#%*#%*#%
无论怎样缩排，每次循环都会打印#，因为缩排并不能让putchar('#');成为if else复合语句的一部分。
```

## 7-6
下面的程序将打印什么？
```c
#include <stdio.h>
int main(void)
{
	int i = 0;
	while (i < 3) 
	{
		switch (i++) 
		{
			case 0: printf("fat ");
			case 1: printf("hat ");
			case 2: printf("cat ");
			default: printf("Oh no!");
		}
		putchar('\n');
	}
	return 0;
}
```
```c
程序打印以下内容：
fat hat cat Oh no!
hat cat Oh no!
cat Oh no!
```

## 7-7
下面的程序有哪些错误？
```c
#include <stdio.h>
int main(void)
{
	char ch;
	int lc = 0; /*统计小写字母
	int uc = 0; /* 统计大写字母
	int oc = 0; /* 统计其他字母
	while ((ch = getchar()) != '#')
	{
		if ('a' <= ch >= 'z')
			lc++;
		else if (!(ch < 'A') || !(ch > 'Z')
			uc++;
		oc++;
	}
	printf(%d lowercase, %d uppercase, %d other, lc, uc, oc);
	return 0;
}
```


第5行～第7行的注释要以*/结尾，或者把注释开头的/*换成//。表达式'a' <= ch >= 'z'应替换成ch >= 'a' && ch <= 'z'。或者，包含  ctype.h  并使用  islower()，这种方法更简单，而且可移植性更高。

虽然从 C 的语法方面看，'a' <= ch >= 'z'是有效的表达式，但是它的含义不明。因为关系运算符从左往右结合，该表达式被解释成('a' <= ch) >= 'z'。圆括号中的表达式的值不是1就是0（真或假），然后判断该值是否大于或等于'z'的数值码。1和0都不满足测试条件，所以整个表达式恒为0（假）。


在第2个测试表达式中，应该把||改成&&。另外，虽然!(ch<  'A')是有效的表达式，而且含义也正确，但是用ch  >=  'A'更简单。这一行的'z'后面应该有两个圆括号。更简单的方法是使用isuupper()。在oc++;前面应该加一行else。否则，每输入一个字符， uc 都会递增 1。另外，在 printf()语句中的格式化字符串应该用双引号括起来。

下面是修改后的版本：
```c
#include <stdio.h>
#include <ctype.h>
int main(void)
{
	char ch;
	int lc = 0; /*统计小写字母*/
	int uc = 0; /*统计大写字母*/
	int oc = 0; /*统计其他字母*/
	while ((ch = getchar()) != '#')
	{
		if (islower(ch))
			lc++;
		else if (isupper(ch))
			uc++;
		else
			oc++;
	}
	printf("%d lowercase, %d uppercase, %d other", lc, uc, oc);
	return 0;
}
```


## 7-8
下面的程序将打印什么？
```c
/* retire.c */
#include <stdio.h>
int main(void)
{
	int age = 20;
	while (age++ <= 65)
	{
		if ((age % 20) == 0) /* age是否能被20整除？ */
			printf("You are %d.Here is a raise.\n", age);
		if (age = 65)
			printf("You are %d.Here is your gold watch.\n", age);
	}
	return 0;
}
```
```c
该程序将不停重复打印下面一行：
You are 65.Here is your gold watch.
问题出在：if (age = 65)
这行代码把age设置为65，使得每次迭代的测试条件都为真。
```

## 7-9
给定下面的输入时，以下程序将打印什么？
```
q
c
h
b
```
```c
#include <stdio.h>
int main(void)
{
	char ch;
	while ((ch = getchar()) != '#')
	{
		if (ch == '\n')
			continue;
		printf("Step 1\n");
		if (ch == 'c')
			continue;
		else if (ch == 'b')
			break;
		else if (ch == 'h')
			goto laststep;
		printf("Step 2\n");
	laststep: printf("Step 3\n");
	}
	printf("Done\n");
	return 0;
}
```

```c
下面是根据给定输入的运行结果：
> q
Step 1
Step 2
Step 3
> c
Step 1
> h
Step 1
Step 3
> b
Step 1
Done
注意，b和#都可以结束循环。但是输入b会使得程序打印step 1，而输入#则不会。
```

## 7-10
重写复习题9，但这次不能使用continue和goto语句
```c
#include <stdio.h>
int main(void)
{
	char ch;
	while ((ch = getchar()) != '#')
	{
		if (ch != '\n')
		{
			printf("Step 1\n");
			if (ch == 'b')
				break;
			else if (ch != 'c')
			{
				if (ch != 'h')
					printf("Step 2\n");
				printf("Step 3\n");
			}
		}
	}
	printf("Done\n");
	return 0;
}
```

<h1>Programming Exercises</h1>

## 7-1
Write a program that reads input until encountering the `#` character and then reports the number of spaces read, the number of newline characters read,and the number of all other characters read.
***
```c
#include <stdio.h>
#define STOP '#'

int main(void)
{
	char ch;
	unsigned int spaces = 0, newlines = 0, other= 0;
	printf("Enter input (%c to stop):\n", STOP);
	while((ch = getchar()) != STOP)
	{
		if (ch == ' ')
			spaces++;
		else if (ch == '\n')
			newlines++;
		else
			other++;
	}
	printf("\n");
	printf("Character Count:\n");
	printf("----------------\n");
	printf("Spaces: %u\n"
		   "Newlines: %u\n"
		   "Other: %u\n", spaces, newlines, other);

	return 0;
}
```

## 7-2
Write a program that reads input until encountering `#`. Have the program print each input character and its `ASCII `decimal code. Print eight character-code pairs per line. 
Suggestion: Use a character count and the modulus operator `(%)` to print a newline character for every eight cycles of the loop.
***
```c
#include <stdio.h>
#define STOP '#'

#define SPACE ' '
#define TAB '\t'
#define NEWLINE '\n'
#define BACKSPACE '\b'

int main(void)
{
	unsigned int count = 0;
	char ch;
	printf("ASCII Character Codes\n");
	printf("Enter input (%c to stop):\n", STOP);
	while ((ch = getchar()) != STOP)
	{
		switch (ch)
		{
			case SPACE :
					printf("' ': %3d ", ch);
					break;
			case TAB :
					printf("'\\t': %3d ", ch);
					break;
			case NEWLINE :
					printf("'\\n': %3d ", ch);
					break;
			case BACKSPACE :
					printf("'\\b': %3d ", ch);
					break;
			default:
					printf(" %c : %3d ", ch, ch);
		}
		count++;
		if (count % 8 == 0)
			printf("\n");
	}
	printf("\n");
	return 0;
}
```

## 7-3
Write a program that reads integers until 0 is entered. After input terminates, the program should report the total number of even integers (excluding the 0) entered, the average value of the even integers, the total number of odd integers entered, and the average value of the odd integers.
***
```c
#include <stdio.h>
#include <ctype.h>

#define STOP 0

int main(void)
{
	int even_count = 0, even_sum = 0, odd_count = 0, odd_sum = 0;
	float even_avg, odd_avg;
	int input;

	printf("Enter integers (0 to stop):\n");
	while(scanf("%d", &input) == 1 && input != STOP)
	{
		if (input % 2 == 0)
		{
			even_count++;
			even_sum += input;
		}
		else
		{
			odd_count++;
			odd_sum += input;
		}
	}

	even_avg = even_sum / (float) even_count;
	odd_avg = odd_sum / (float) odd_count;

	printf("Number of even integers: %d\n", even_count);
	printf("Average value of even integers: %.2f\n", even_avg);
	printf("Number of odd integers: %d\n", odd_count);
	printf("Average value of odd integers: %.2f\n", odd_avg);

	return 0;
}
```

## 7-4
Using if else statements, write a program that reads input up to `#`, replaces each period with an exclamation mark, replaces each exclamation mark initially present with two exclamation marks, and reports at the end the number of substitutions it has made.
***
```c
#include <stdio.h>

#define STOP '#'

int main(void)
{
	char ch;

	printf("Enter input (%c to exit):\n", STOP);
	while ((ch = getchar()) != STOP)
	{
		if (ch == '.')
			printf("!");
		else if (ch == '!')
			printf("!!");
		else
			printf("%c", ch);
	}

	return 0;
}
```

## 7-5
Redo exercise 4 using a switch.
***
```c
#include <stdio.h>

#define STOP '#'

int main(void)
{
	char ch;

	printf("Enter input (%c to exit):\n", STOP);
	while ((ch = getchar()) != STOP)
	{
		switch (ch)
		{
			case '.' :
				printf("!");
				break;
			case '!' :
				printf("!!");
				break;
			default :
				printf("%c", ch);
		}
	}

	return 0;
}
```

## 7-6
Write a program that reads input up to `#`and reports the number of times that the sequence `ei` occurs.
***
```c
#include <stdio.h>
#include <stdbool.h>
#include <ctype.h>

#define STOP '#'

int main(void)
{
	char ch;
	unsigned int ei_count = 0;
	bool e_flag = false;

	printf("This program reads input and counts the number of times the\n"
		   "sequence 'ei' occurs (case insensitive).\n");
	printf("Enter input (%c to stop):\n", STOP);

	while ((ch = getchar()) != STOP)
	{
		ch = tolower(ch);
		if (ch == 'e')
			e_flag = true;
		else if (ch == 'i')
		{
			if (e_flag)
				ei_count++;
			e_flag = false;
		}
		else
			e_flag = false;

	}

	printf("The sequence 'ei' occurs %u times.\n", ei_count);

	return 0;
}
```

## 7-7
Write a program that requests the hours worked in a week and then prints the gross pay, the taxes, and the net pay. Assume the following:

    a. Basic pay rate = $10.00/hr
    b. Overtime (in excess of 40 hours) = time and a half
    c. Tax rate: #15% of the first $300
	20% of the next $150 25% of the rest

Use `#define` constants, and don’t worry if the example does not conform to current tax law.
***
```c
#include <stdio.h>

#define BASIC_RATE 10.0
#define OVERTIME_HOURS 40.0
#define OVERTIME_MULTIPLIER 1.5
#define TAX_RATE_1 0.15
#define TAX_BRACKET_1 300.0
#define TAX_RATE_2 0.20
#define TAX_BRACKET_2 450.0
#define TAX_RATE_3 0.25

float calculate_gross_pay(float hours);
float calulate_taxes(float gross_pay);

int main(void)
{
	float hours, gross_pay, taxes;

	printf("Enter number of hours worked in a week: ");

	if (scanf("%f", &hours) == 1)
	{
		gross_pay = calculate_gross_pay(hours);
		taxes = calulate_taxes(gross_pay);

		printf("For %.1f hours of work you make $%.2f and pay $%.2f in taxes.\n",
			   hours, gross_pay, taxes);
	}
	else
		printf("Invalid input...terminating.\n");

	return 0;
}

float calculate_gross_pay(float hours)
{
	if (hours > OVERTIME_HOURS)
		return OVERTIME_HOURS * BASIC_RATE + (hours - OVERTIME_HOURS) * BASIC_RATE * OVERTIME_MULTIPLIER;
	else
		return hours * BASIC_RATE;
}

float calulate_taxes(float gross_pay)
{
	if (gross_pay > TAX_BRACKET_2)
		return TAX_RATE_3 * (gross_pay - TAX_BRACKET_2) + TAX_RATE_2 * (TAX_BRACKET_2 - TAX_BRACKET_1) + TAX_RATE_1 * TAX_BRACKET_1;
	else if (gross_pay > TAX_BRACKET_1)
		return TAX_RATE_2 * (gross_pay - TAX_BRACKET_1) + TAX_RATE_1 * TAX_BRACKET_1;
	else
		return TAX_RATE_1 * gross_pay;
}
```

## 7-8
Modify assumption a. in exercise 7 so that the program presents a menu of pay rates from which to choose. Use a switch to select the pay rate. The beginning of a run should look something like this:
```
*****************************************************************
Enter the number corresponding to the desired pay rate or action:
1) $8.75/hr 							2) $9.33/hr
3) $10.00/hr 						4) $11.20/hr
5) quit 
*****************************************************************
```
If choices 1 through 4 are selected, the program should request the hours worked. The program should recycle until 5 is entered. If something other than choices 1 through 5 is entered, the program should remind the user what the proper choices are and then recycle. Use `#defined `constants for the various earning rates and tax rates.
***
```c
#include <stdio.h>
#include <stdbool.h>

#define RATE_1 8.75
#define RATE_2 9.33
#define RATE_3 10.00
#define RATE_4 11.20

#define OVERTIME_HOURS 40.0
#define OVERTIME_MULTIPLIER 1.5
#define TAX_RATE_1 0.15
#define TAX_BRACKET_1 300.0
#define TAX_RATE_2 0.20
#define TAX_BRACKET_2 450.0
#define TAX_RATE_3 0.25

void flush_input_buffer(void);
float calculate_gross_pay(float hours, float rate);
float calulate_taxes(float gross_pay);

int main(void)
{
	bool exit_flag = false;
	int rate_option;
	float rate, hours, gross_pay, taxes;

	while (1) // main program loop
	{

		// print usage instructions
		printf("*****************************************************************\n");
		printf("Enter the number corresponding to the desired pay rate or action:\n");
		printf("1) $%.2f/hr 				2) $%.2f/hr\n", RATE_1, RATE_2);
		printf("3) $%.2f/hr 				4) $%.2f/hr\n", RATE_3, RATE_4);
		printf("5) quit \n");
		printf("*****************************************************************\n");

		scanf("%d", &rate_option);
		switch (rate_option)
		{
			case (1) : 	
				rate = RATE_1;
				break;
			case (2) : 	
				rate = RATE_2;
				break;
			case (3) :
				rate = RATE_3;
				break;
			case (4) :
				rate = RATE_4;
				break;
			case (5) :
				exit_flag = true;
				break;
			default : // invalid input
				flush_input_buffer();
				printf("Please enter an integer between 1 and 5.\n\n");
				continue; // repeat main program loop
		}

		if (exit_flag)
			break; // exit program

		printf("Enter number of hours worked in a week: ");
		while (scanf("%f", &hours) != 1 || hours <= 0)
		{
			flush_input_buffer();
			printf("Please enter a positive number. \n");
			printf("Enter number of hours worked in a week: ");
		}

		gross_pay = calculate_gross_pay(hours, rate);
		taxes = calulate_taxes(gross_pay);

		printf("For %.1f hours of work at $%.2f/hr, you make $%.2f and pay"
			   " $%.2f in taxes.\n", hours, rate, gross_pay, taxes);
		printf("\n");

	}

	printf("Bye.\n");

	return 0;
}

void flush_input_buffer(void)
{
	while (getchar() != '\n')
		;
}

float calculate_gross_pay(float hours, float rate)
{
	if (hours > OVERTIME_HOURS)
		return OVERTIME_HOURS * rate + (hours - OVERTIME_HOURS) * rate * OVERTIME_MULTIPLIER;
	else
		return hours * rate;
}

float calulate_taxes(float gross_pay)
{
	if (gross_pay > TAX_BRACKET_2)
		return TAX_RATE_3 * (gross_pay - TAX_BRACKET_2) + TAX_RATE_2 * (TAX_BRACKET_2 - TAX_BRACKET_1) + TAX_RATE_1 * TAX_BRACKET_1;
	else if (gross_pay > TAX_BRACKET_1)
		return TAX_RATE_2 * (gross_pay - TAX_BRACKET_1) + TAX_RATE_1 * TAX_BRACKET_1;
	else
		return TAX_RATE_1 * gross_pay;
}
```

## 7-9
Write a program that accepts a positive integer as input and then displays all the prime numbers smaller than or equal to that number.
***
```c
#include <stdio.h>
#include <stdbool.h>

void flush_input_buffer(void);

int main(void)
{
	bool prime_flag;
	int limit;
	printf("Primes: this program prints all primes less than or equal to any positive integer.\n");
	printf("Enter a positive integer: \n");
	while (scanf("%d", &limit) != 1 || limit < 1)
	{
		flush_input_buffer();
		printf("Enter a positive integer: \n");
	}

	for (int i = 2; i <= limit; i++)
	{
		prime_flag = true;
		for (int j = 2; j < i; j++) // for all j less than i ...
		{
			if (i % j == 0) // if i is divisible by j ...
			{
				prime_flag = false; // then i is not prime
				break; // break out of inner loop
			}
		}
		if (prime_flag)
			printf("%d is prime.\n", i);
	}

	return 0;
}

void flush_input_buffer(void)
{
	while (getchar() != '\n')
		;
}
```

## 7-10
The 1988 United States Federal Tax Schedule was the simplest in recent times. It had four categories, and each category had two rates. Here is a summary (dollar amounts are taxable income):
```
Category					Tax
Single						15% of first $17,850 plus 28% of excess
Head of Household			15% of first $23,900 plus 28% of excess
Married, Joint				15% of first $29,750 plus 28% of excess
Married, Separate			15% of first $14,875 plus 28% of excess
```   
For example, a single wage earner with a taxable income of `$20,000` owes`0.15 × $17,850 + 0.28 × ($20,000−$17,850)`. Write a program that lets the user specify the tax category and the taxable income and that then calculates the tax. Use a loop so that the user can enter several tax cases.
***
```c
#include <stdio.h>

#define SINGLE 1
#define HEAD_OF_HOUSEHOLD 2
#define MARRIED_JOINT 3
#define MARRIED_SEPARATE 4
#define EXIT 5

#define RATE_1 0.15f
#define RATE_2 0.28f

void flush_input_buffer(void);

int main(void)
{
	int category;
	float income, bracket, taxes;

	printf("US 1988 Tax Calculator\n");

	while(1)
	{
		printf("1) Single  2) Head of Household  3) Married, Joint  4) Married Separate\n");
		printf("Enter your tax category (1-4) or 5 to quit: ");
		scanf("%d", &category);

		switch (category)
		{
			case (SINGLE) :	
					bracket = 17850.0;
					break;
			case (HEAD_OF_HOUSEHOLD) :
					bracket = 23900.0;
					break;
			case (MARRIED_JOINT) :
					bracket = 29750.0;
					break;
			case (MARRIED_SEPARATE) :
					bracket = 14875.0;
					break;
			case (EXIT) : 
					printf("Bye.\n");
					return 0; // Exit Program
			default :
					flush_input_buffer();
					printf("Invalid input: please enter an integer between 1 and 5.\n");
					continue;
		}
		printf("Enter your income: ");
		while (scanf("%f", &income) != 1 || income < 0)
		{
			flush_input_buffer();
			printf("Invalid input: please enter a positive number.\n");
			printf("Enter your income: ");
		}

		if (income > bracket)
			taxes = RATE_2 * (income - bracket) + RATE_1 * bracket;
		else
			taxes = RATE_1 * income;

		printf("You will owe $%.2f in taxes.\n\n", taxes);
	}
}

void flush_input_buffer(void)
{
	while (getchar() != '\n')
		;
}
```
## 7-11
`The ABC Mail Order Grocery` sells artichokes for `$2.05` per pound, beets for `$1.15` per pound, and carrots for `$1.09` per pound. It gives a `5%` discount for orders of `$100` or more prior to adding shipping costs. It charges `$6.50` shipping and handling for any order of `5 `pounds or under, `$14.00` shipping and handling for orders over `5` pounds and under `20` pounds, and `$14.00` plus `$0.50` per pound for shipments of `20` pounds or more. Write a program that uses a switch statement in a loop such that a response of `a` lets the user enter the pounds of artichokes desired, `b` the pounds of beets, `c` the pounds of carrots, and `q` allows the user to exit the ordering process. The program should keep track of cumulative totals. That is, if the user enters `4` pounds of beets and later enters `5` pounds of beets, the program should use report` 9` pounds of beets. The program then should compute the total charges, the discount, if any, the shipping charges, and the grand total. The program then should display all the purchase information: the cost per pound, the pounds ordered, and the cost for that order for each vegetable, the total cost of the order, the discount (if there is one), the shipping charge, and the grand total of all the charges.
***
```c
#include <stdio.h>
#include <stdbool.h>

#define ARTICHOKE_PRICE_PER_LB 2.05
#define BEET_PRICE_PER_LB 1.15
#define CARROT_PRICE_PER_LB 1.09

#define SHIPPING_5LB 6.50
#define SHIPPING_20LB 14.00
#define SHIPPING_OVER_20LB_RATE 0.5

#define DISCOUNT_RATE 0.05

void flush_input_buffer(void);
float calculate_shipping(float weight);

int main(void)
{
	float artichoke_weight = 0, beet_weight = 0, carrot_weight = 0, total_weight;
	float artichoke_price, beet_price, carrot_price, subtotal, discount, shipping, total;
	bool discount_flag;
	float weight;
	char option;

	printf("ABC Mail Order Grocery\n");
	while(1)
	{
		printf("What would you like to order?\n");
		printf("a) artichokes  b) beets  c) carrots  q) quit\n");
		option = getchar();
		switch (option)
		{
			case ('q') : 
					printf("Bye.\n");
					return 0; // exit program

			case ('a') : // artichokes
					printf("How many pounds of artichokes would you like to add? ");
					if (scanf("%f", &weight) == 1)
						artichoke_weight += weight;
					else
					{
						flush_input_buffer();
						printf("Invalid input. Try again.\n");
						continue; // repeat main program loop
					}
					break;

			case ('b') : // beets
					printf("How many pounds of beets would you like to add? ");
					if (scanf("%f", &weight) == 1)
						beet_weight += weight;
					else
					{
						flush_input_buffer();
						printf("Invalid input. Try again.\n");
						continue; // repeat main program loop
					}
					break;

			case ('c') : // carrots
					printf("How many pounds of carrots would you like to add? ");
					if (scanf("%f", &weight) == 1)
						carrot_weight += weight;
					else
					{
						flush_input_buffer();
						printf("Invalid input. Try again.\n");
						continue; // repeat main program loop
					}
					break;

			default :
					printf("Invalid input. Try again.\n");
					continue; // repeat main program loop
		}

		// calculate subtotal
		artichoke_price = artichoke_weight * ARTICHOKE_PRICE_PER_LB;
		beet_price = beet_weight * BEET_PRICE_PER_LB;
		carrot_price = carrot_weight * CARROT_PRICE_PER_LB;
		subtotal = artichoke_price + beet_price + carrot_price;

		// calculate discount
		if (subtotal >= 100)
		{
			discount_flag = true;
			discount = DISCOUNT_RATE * subtotal;
		}
		else
			discount_flag = false;

		// calculate shipping
		total_weight = artichoke_weight + beet_weight + carrot_weight;
		shipping = calculate_shipping(total_weight);

		// grand total
		total = subtotal + shipping - (discount_flag ? discount : 0.0);

		printf("\n");
		printf("Your order summary:\n\n");
		printf("Artichokes: %.2flbs @ $%.2f/lb: $%.2f\n",
			   artichoke_weight, ARTICHOKE_PRICE_PER_LB, artichoke_price);
		printf("Beets: %.2flbs @ $%.2f/lb: $%.2f\n",
			   beet_weight, BEET_PRICE_PER_LB, beet_price);
		printf("Carrots: %.2flbs @ $%.2f/lb: $%.2f\n",
			   carrot_weight, CARROT_PRICE_PER_LB, carrot_price);
		printf("\n");
		printf("Subtotal: $%.2f\n", subtotal);
		if (discount_flag)
			printf("%.0f%% discount: $%.2f\n", DISCOUNT_RATE * 100, discount);
		printf("Shipping charges: $%.2f\n", shipping);
		printf("Grand total: $%.2f\n", total);
		printf("\n");

		flush_input_buffer();
	}
}

void flush_input_buffer(void)
{
	while (getchar() != '\n')
		;
}

float calculate_shipping(float weight)
{
	if (weight < 5.0)
		return SHIPPING_5LB;
	else if (weight < 20.0)
		return SHIPPING_20LB;
	else
		return SHIPPING_20LB + SHIPPING_OVER_20LB_RATE * (weight - 20.0);
}
```



