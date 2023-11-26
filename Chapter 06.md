# Chapter 06

<h1>Review Questions</h1>

## 6-1
写出执行完下列各行后`quack`的值是多少。后5行中使用的是第1行生成的`quack`的值。
```c
int quack = 2;
quack += 5;
quack *= 10;
quack -= 6;
quack /= 8;
quack %= 3;
```

> 2，7，70，64，8，2。

## 6-2
假设value是int类型，下面循环的输出是什么？
```c
for (value = 36; value > 0; value /= 2)
	printf("%3d", value);
```
> 该循环的输出是：36 18 9 4 2 1

如果value是double类型，会出现什么问题？
> 如果value是double类型，即使value小于1，循环的测试条件仍然为真。
循环将一直执行，直到浮点数下溢生成0为止。另外，value是double类型
时，%3d转换说明也不正确。

## 6-3
用代码表示以下测试条件：

	a. x大于5
	b. scanf()读取一个名为x的double类型值且失败
	c. X的值等于5
```c
a.x > 5
b.scanf("%lf",&x) != 1
c.x == 5
```

## 6-4
用代码表示以下测试条件：

	a. 成功读入一个整数
	b. 不等于
	c. 大于或等于
```c
a.scanf("%d", &x) == 1
b.x != 5
c.x >= 20
```

## 6-5
下面的程序有点问题，请找出问题所在。
```c
#include <stdio.h>
int main(void)
{                  /* 第3行 */
	int i, j, list(10);       /* 第4行 */

	for (i = 1, i <= 10, i++)    /* 第6行 */
	{                /* 第7行 */
		list[i] = 2*i + 3;      /* 第8行 */
		for (j = 1, j > = i, j++)  /* 第9行 */
			printf(" %d", list[j]); /* 第10行 */
		printf("\n");        /* 第11行 */
}                  /* 第12行 */
```

	第4行：应该是list[10]。
	第6行：逗号改为分号。i的范围应该是0～9，不是1～10。
	第9行：逗号改为分号。>=改成<=，否则，当i等于1时，该循环将成为无限循环。
	第10行：在第11行之后少了一个右花括号。该右花括号与第7行的左花括号配对，形成一个for循环块。然后在这个右花括号与最后一个右花括号之间，少了一行return 0;。

> 下面是一个正确的版本：
```c
#include <stdio.h>
int main(void)
{                /* 第3行 */
	int i, j, list[10];      /* 第4行 */
	for (i = 0; i < 10; i++)    /* 第6行 */
	{              /* 第7行 */
		list[i] = 2*i + 3;     /* 第8行 */
		for (j = 1, j <= i, j++)  /* 第9行 */
			printf(" %d", list[j]); /* 第10行 */
		printf("\n");       /* 第11行 */
	}
	return 0;
}
```

## 6-6
编写一个程序打印下面的图案，要求使用嵌套循环：

	$$$$$$$$
	$$$$$$$$
	$$$$$$$$
	$$$$$$$$

```c
#include <stdio.h>
int main(void)
{
	int col, row;
	for (row = 1; row <= 4; row++)
	{
		for (col = 1; col <= 8; col++)
			printf("$");
		printf("\n");
	}
	return 0;
}
```

## 6-7
下面的程序各打印什么内容？
```c
//a
#include <stdio.h>
int main(void)
{
	int i = 0;

	while (++i < 4)
		printf("Hi! ");
	
	do
		printf("Bye! ");
	while (i++ < 8);
	
	return 0;
}

//b
#include <stdio.h>
int main(void)
{
	int i;
	char ch;

	for (i = 0, ch = 'A'; i < 4; i++, ch += 2 * i)
		printf("%c", ch);

	return 0;
}
```

	a.Hi! Hi! Hi! Bye! Bye! Bye! Bye! Bye!
	b.ACGM（因为代码中把int类型值与char类型值相加，编译器可能警告会损失有效数字）

## 6-8
假设用户输入的是`Go west, young man!`，下面各程序的输出是什么？（在ASCII码中，!紧跟在空格字符后面）
```c
//a
#include <stdio.h>
int main(void)
{
	char ch;

	scanf("%c", &ch);
	while (ch != 'g')
	{
		printf("%c", ch);
		scanf("%c", &ch);
	}
	return 0;
}

//b
#include <stdio.h>
int main(void)
{
	char ch;
	scanf("%c", &ch);
	while (ch != 'g')
	{
		printf("%c", ++ch);
		scanf("%c", &ch);
	}
	return 0;
}

//c
#include <stdio.h>
int main(void)
{
	char ch;
	do {
		scanf("%c", &ch);
		printf("%c", ch);
	} while (ch != 'g');
	return 0;
}

//d
#include <stdio.h>
int main(void)
{
	char ch;
	scanf("%c", &ch);
	for (ch = '$'; ch != 'g'; scanf("%c", &ch))
		printf("%c", ch);
	return 0;
}
```

```c
a.Go west, youn
b.Hp!xftu-!zpvo
c.Go west, young
d.$o west, youn
```

## 6-9
下面的程序打印什么内容？
```c
#include <stdio.h>
int main(void)
{
	int n, m;
	n = 30;

	while (++n <= 33)
		printf("%d|", n);

	n = 30;
	do
		printf("%d|", n);
	while (++n <= 33);

	printf("\n***\n");

	for (n = 1; n*n < 200; n += 4)
		printf("%d\n", n);

	printf("\n***\n");

	for (n = 2, m = 6; n < m; n *= 2, m += 2)
		printf("%d %d\n", n, m);
	
	printf("\n***\n");

	for (n = 5; n > 0; n--)
	{
		for (m = 0; m <= n; m++)
			printf("=");
		printf("\n");
	}
	return 0;
}
```

```c
31|32|33|30|31|32|33|
***
1
5
9
13
***
2 6
4 8
8 10
***
======
=====
====
===
==
```

## 6-10
考虑下面的声明：
```c
double mint[10];
```

	a.数组名是什么？
	b.该数组有多少个元素？
	c.每个元素可以储存什么类型的值？
	d.下面的哪一个scanf()的用法正确？
		i.scanf("%lf", mint[2])
		ii.scanf("%lf", &mint[2])
		iii.scanf("%lf", &mint)
>
	a.mint
	b.10个元素
	c.double 类型的值
	d.第ii行正确，mint[2]是double类型的值，&mint[2]是它在内存中的位置。

## 6-11
Noah先生喜欢以2计数，所以编写了下面的程序，创建了一个储存`2、4、6、8`等数字的数组。这个程序是否有错误之处？如果有，请指出。
```c
#include <stdio.h>
#define SIZE 8
int main(void)
{
	int by_twos[SIZE];
	int index;
	for (index = 1; index <= SIZE; index++)
		by_twos[index] = 2 * index;
	for (index = 1; index <= SIZE; index++)
		printf("%d ", by_twos);
	printf("\n");
	return 0;
}
```
因为第1个元素的索引是0，所以循环的范围应该是`0～SIZE-1`，而不是`1～SIZE`。但是，如果只是这样更改会导致赋给第1个元素的值是0，不是2。所以，应重写这个循环：
```c
for (index = 0; index < SIZE; index++)
	by_twos[index] = 2 * (index + 1);
```
与此类似，第2个循环的范围也要更改。另外，应该在数组名后面使用数组索引：
```c
for( index = 0; index < SIZE; index++)
	printf("%d ", by_twos[index]);
```
> 错误的循环条件会成为程序的定时炸弹。程序可能开始运行良好，
> 但是由于数据被放在错误的位置，可能在某一时刻导致程序不能正常工作。

## 6-12
假设要编写一个返回long类型值的函数，函数定义中应包含什么？

> 该函数应声明为返回类型为long，并包含一个返回long类型值的return语句。

## 6-13
定义一个函数，接受一个int类型的参数，并以long类型返回参数的平方值。

> 把num的类型强制转换成long类型，确保计算使用long类型而不是int类型。
> 
> 在int为16位的系统中，两个int类型值的乘积在返回之前会被截断为一个int类型的值，这可能会丢失数据。
```c
long square(int num)
{
	return ((long) num) * num;
}
```

## 6-14
下面的程序打印什么内容？
```c
#include <stdio.h>
int main(void)
{
	int k;
	for (k = 1, printf("%d: Hi!\n", k); printf("k = %d\n", k),
		k*k < 26; k += 2, printf("Now k is %d\n", k))
			printf("k is %d in the loop\n", k);
	return 0;
}
```

```c
1: Hi!
k = 1
k is 1 in the loop
Now k is 3
k = 3
k is 3 in the loop
Now k is 5
k = 5
k is 5 in the loop
Now k is 7
k = 7
```

<h1>Programming Exercises</h1>

## 6-1
Write a program that creates an array with 26 elements and stores the 26 lowercase letters in it. Also have it show the array contents.
---
```c
#include <stdio.h>
#define ALPHABET_LENGTH 26

int main(void)
{
	char alphabet_lowercase[ALPHABET_LENGTH];
	char letter;
	int i;

	// initialize array
	for (letter = 'a'; letter - 'a' < ALPHABET_LENGTH; letter++)
	{
		alphabet_lowercase[letter - 'a'] = letter; // store letter in array
	}

	printf("The lowercase letters of the alphabet are:\n");
	// print each item in array
	for (i = 0; i < ALPHABET_LENGTH; i++)
	{
		printf("%c ", alphabet_lowercase[i]);
	}
	printf("\n");

	return 0;
}
```

## 6-2
Use nested loops to produce the following pattern:

    $
    $$
    $$$
    $$$$
    $$$$$

---
```c
#include <stdio.h>

int main(void)
{
	for (int i = 1; i < 6; i++)
	{
		for (int j = 1; j <= i; j++)
		{
			printf("$");
		}
		printf("\n");
	}

	return 0;
}
```

## 6-3
Use nested loops to produce the following pattern:

    F
    FE
    FED
    FEDC
    FEDCB
    FEDCBA

---
```c
#include <stdio.h>

int main(void)
{
	for (int i = 1; i < 7; i++)
	{
		for (char c = 'F'; 'F' - c < i; c--)
		{
			printf("%c", c);
		}
		printf("\n");
	}

	return 0;
}
```

## 6-4
Use nested loops to produce the following pattern:

    A
    BC
    DEF
    GHIJ
    KLMNO
    PQRSTU

---
```c
#include <stdio.h>

int main(void)
{
	char c = 'A';

	for (int i = 1; i < 7; i++)
	{
		for(int j = 1; j <= i; j++)
		{
			printf("%c", c++); // print and THEN increment c
		}
		printf("\n");
	}

	return 0;
}
```

## 6-5
Have a program request the user to enter an uppercase letter. Use nested loops to produce a pyramid pattern like this:

        A 
       ABA
      ABCBA
     ABCDCBA
    ABCDEDCBA

The pattern should extend to the character entered. For example, the preceding pattern would result from an input value of E.

---
```c
#include <stdio.h>

void print_spaces(unsigned int n);

int main(void)
{
	char uppercase_letter;
	char c1, c2;

	do // get uppercase letter from user
	{
		printf("Enter an uppercase letter: ");
		scanf(" %c", &uppercase_letter);
	} while (uppercase_letter < 'A' || 'Z' < uppercase_letter);

	for(c1 = 'A'; c1 <= uppercase_letter; c1++)
	{
		// print opening spaces
		print_spaces(uppercase_letter - c1);

		// print letters
		// ascending
		for (c2 = 'A'; c2 < c1; c2++)
		{
			printf("%c", c2);

		}
		// descending
		for (; 'A' <= c2; c2--)
		{
			printf("%c", c2);
		}

		// print closing spaces
		print_spaces(uppercase_letter - c1);
		printf("\n");
	}

	return 0;
}

void print_spaces(unsigned int n)
{
	for (int i = 0; i < n; i++)
	{
		printf(" ");
	}
}
```

## 6-6
Write a program that prints a table with each line giving an integer, its square, and its cube. Ask the user to input the lower and upper limits for the table. Use a for loop.

---
```c
#include <stdio.h>

int main(void)
{
	long upper = -1, lower=0;
	int reads;

	printf("This program prints a table of integers with their "
		   "squares and cubes.\n");
	do
	{	
		printf("Enter lower and upper integer limits (in that order): ");
		reads = scanf("%ld%ld", &lower, &upper);
		if (reads != 2)
		{
			while (getchar() != '\n') ; // if read fails, clear input buffer
		}
	} while (lower > upper); // if lower is greater than upper, get new input

	printf("\n");
	// table header
	printf(" Integer       | Square        | Cube          \n");
	printf("---------------|---------------|---------------\n");
	for (long int i = lower; i <= upper; i++)
	{
		printf(" %-14ld| %-14ld| %-14ld\n", i, i * i, i * i * i);		
	}
	printf("\n");

	return 0;
}
```
## 6-7
Write a program that reads a single word into a character array and then prints the word backward. Hint: Use `strlen() `(Chapter 4) to compute the index of the last character in the array.

---
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
	char word[30];

	printf("Enter a string: ");
	scanf("%s", word);
	for (int i = strlen(word) - 1; i >= 0; i--)
	{
		printf("%c", word[i]);
	}
	printf("\n");

	return 0;
}
```

## 6-8
Write a program that requests two floating-point numbers and prints the value of their difference divided by their product. Have the program loop through pairs of input values until the user enters nonnumeric input.

---
```c
#include <stdio.h>

int main(void)
{
	float num1, num2;
	int reads;
	
	printf("Enter two floating-point numbers: ");
	while (scanf(" %f %f", &num1, &num2) == 2)
	{
		printf("(%.3f - %.3f)/(%.3f * %.3f) = %.3f\n", num1, num2, num1, num2,
		       (num1 - num2)/(num1 * num2));
		printf("Enter two floating-point numbers (enter non-numeric to quit): ");
	}

	return 0;
}
```

## 6-9
Modify exercise 8 so that it uses a function to return the value of the calculation.

---
```c
#include <stdio.h>

float calculate(float n1, float n2);

int main(void)
{
	float num1, num2;
	int reads;
	
	printf("Enter two floating-point numbers: ");
	while (scanf(" %f %f", &num1, &num2) == 2)
	{
		printf("(%.3f - %.3f)/(%.3f * %.3f) = %.3f\n", num1, num2, num1, num2,
		       calculate(num1, num2));
		printf("Enter two floating-point numbers (enter non-numeric to quit): ");
	}

	return 0;
}

float calculate(float n1, float n2)
{
	return (n1 - n2) / (n1 * n2);
}
```
## 6-10
Write a program that requests lower and upper integer limits, calculates the sum of all the integer squares from the square of the lower limit to the square of the upper limit, and displays the answer. The program should then continue to prompt for limits and display answers until the user enters an upper limit that is equal to or less than the lower limit. A sample run should look something like this:

    Enter lower and upper integer limits: 5 9
    The sums of the squares from 25 to 81 is 255
    Enter next set of limits: 3 25
    The sums of the squares from 9 to 625 is 5520
    Enter next set of limits: 5 5
    Done

---
```c
#include <stdio.h>

int sum_of_squares(int lower, int upper);

int main(void)
{
	int upper, lower, reads;

	printf("Enter lower and upper integer limits: ");
	while(reads = scanf("%d%d", &lower, &upper), reads == 2 && lower < upper)
	{
		printf("The sums of the squares from %d to %d is %d\n",
			   lower * lower, upper * upper, sum_of_squares(lower, upper));
		printf("Enter next set of limits: ");
	}
	printf("Done\n");

	return 0;
}

int sum_of_squares(int lower, int upper) // calculate sum of squares from lower to upper
{
	int sum = 0;

	for (int i = lower; i <= upper; i++)
	{
		sum += i * i;
	}

	return sum;
}
```

## 6-11
Write a program that reads eight integers into an array and then prints them in reverse order.

---
```c
#include <stdio.h>

int main(void)
{
	int int_array[8];
	int i; // array index

	printf("Enter 8 integers:\n");
	for (i = 0; i < 8; i++) // read ints into array
	{
		scanf("%d", &int_array[i]);
	}
	for (i--; i >= 0; i--) // decrement i to 7 to initialize loop
	{
		printf("%d", int_array[i]);
	}
	printf("\n");

	return 0;
}
```

## 6-12
Consider these two infinite series:

    1.0 + 1.0/2.0 + 1.0/3.0 + 1.0/4.0 + ...
    1.0 - 1.0/2.0 + 1.0/3.0 - 1.0/4.0 + ...

Write a program that evaluates running totals of these two series up to some limit of number of terms. Hint: –1 times itself an odd number of times is –1, and –1 times itself an even number of times is 1. Have the user enter the limit interactively; let a zero or negative value terminate input. Look at the running totals after 100 terms, 1000 terms, 10,000 terms. Does either series appear to be converging to some value?

---
```c
#include <stdio.h>

int main(void)
{
	long int limit;
	float sign = 1.0f;
	float series1 = 0, series2 = 0;

	printf("Enter a number of terms to sum: ");
	scanf("%ld", &limit);

	for (long int i = 1; i <= limit; i++)
	{
		series1 += 1.0f/i;
		series2 += (1.0f/i) * sign;
		sign = -sign; // toggle sign
	}

	printf("The %ldth partial sum for series 1 is: %.5f\n", limit, series1);
	printf("The %ldth partial sum for series 2 is: %.5f\n", limit, series2);

	// Answer: Series 1 has no limit. Series 2 appears to be bounded above

	return 0;
}
```

## 6-13
Write a program that creates an eight-element array of ints and sets the elements to the first eight powers of 2 and then prints the values. Use a for loop to set the values, and, for variety, use a do while loop to display the values.

---
```c
#include <stdio.h>

int main(void)
{
	int powers_of_2[8];
	int power = 1;
	int i;

	for (int i = 0; i < 8; i++)
	{
		power *= 2;
		powers_of_2[i] = power;
	}
	printf("Powers of 2:\n");
	i = 0;
	do {
		printf("%d ", powers_of_2[i]);
		i++;
	} while (i < 8);
	printf("\n");

	return 0;
}
```

## 6-14
Write a program that creates two eight-element arrays of doubles and uses  loop to let the user enter values for the eight elements of the first array. Have the program set the elements of the second array to the cumulative totals of the elements of the first array. For example, the fourth element of the second array should equal the sum of the first four elements of the first array, and the fifth element of the second array should equal the sum of the first five elements of the first array. (It’s possible to do this with nested loops, but by using the fact that the fifth element of the second array equals the fourth element of the second array plus the fifth element of the first array, you can avoid nesting and just use a single loop for this task.) Finally, use loops to display the contents of the two arrays, with the first array displayed on one line and with each element of the second array displayed below the corresponding element of the first array.

---
```c
#include <stdio.h>

int main(void)
{
	int int_array[8], cumulative_sum[8];
	int sum = 0;

	printf("Enter 8 integers:\n");
	for (int i = 0; i < 8; i++)
	{
		scanf("%d", &int_array[i]);
		sum += int_array[i];
		cumulative_sum[i] = sum;
	}
	printf("\n");
	// display loops
	printf("      Integers:");
	for (int i = 0; i < 8; i++)
	{
		printf("%6d ", int_array[i]);
	}
	printf("\n");
	printf("Cumulative sum:");
	for (int i = 0; i < 8; i++)
	{
		printf("%6d ", cumulative_sum[i]);
	}
	printf("\n");

	return 0;
}
```

## 6-15
Write a program that reads in a line of input and then prints the line in reverse order. You can store the input in an array of char; assume that the line is no longer than 255 characters. Recall that you can use `scanf()` with the %c specifier to read a character at a time from input and that the newline character `(\n)` is generated when you press the Enter key.

---
```c
#include <stdio.h>

int main(void)
{
	char line[255];
	int i = 0; // array index
	printf("Enter a line to reverse:\n");
	while (scanf("%c", &line[i]), line[i] != '\n')
		i++;

	for (; 0 <= i; i--) // previous loop leaves i in right position
		printf("%c", line[i]);

	printf("\n");

	return 0;
}
```

## 6-16
Daphne invests $100 at 10% simple interest. (That is, every year, the investment earns an interest equal to 10% of the original investment.) Deirdre invests $100 at 5% interest compounded annually. (That is, interest is 5% of the current balance, including previous addition of interest.) Write a program that finds how many years it takes for the value of Deirdre’s investment to exceed the value of Daphne’s investment. Also show the two values at that time.

---
```c
#include <stdio.h>

int main(void)
{
	const float DEIRDE_PRINCIPLE = 100.0f;
	const float DAPHNE_PRINCIPLE = 100.0f;
	const float DEIRDE_INTEREST = 0.05f;
	const float DAPHNE_INTEREST = 0.10f;

	// initialize years and balances
	int years = 0; 
	float daphne_balance = DAPHNE_PRINCIPLE;
	float deirdre_balance = DEIRDE_PRINCIPLE;

	while (deirdre_balance <= daphne_balance)
	{
		// eq. for compound interest
		deirdre_balance *= 1.0f + DEIRDE_INTEREST;
		// eq. for simple interest
		daphne_balance += DAPHNE_PRINCIPLE * DAPHNE_INTEREST; 
		years++;
	}
	printf("After %d years, Daphne's investment is worth $%.2f and "
		   "Deirdre’s investment is worth $%.2f.\n", years,
		   daphne_balance, deirdre_balance);

	return 0;
}
```

## 6-17
Chuckie Lucky won a million dollars (after taxes), which he places in an account that earns 8% a year. On the last day of each year, Chuckie withdraws $100,000. Write a program that finds out how many years it takes for Chuckie to empty his account.

---
```c
#include <stdio.h>

int main(void)
{
	const float WINNINGS = 1000000.0f;
	const float INTEREST = 0.08f;
	const float SPENDING = 100000.0f;

	int years = 0;
	float balance = WINNINGS;

	// the problem is not quite clear, but I'm assuming
	// Chuckie makes his first withdrawal before collecting
	// any interest
	while (balance > 0)
	{
		balance -= SPENDING;
		balance *= 1.0f + INTEREST;
		years++;
	}

	printf("After %d years, Chuckie is in the red with a balance of"
		   " %.2f USD.\n", years, balance);

	return 0;
}
```

## 6-18
Professor Rabnud joined a social media group. Initially he had five friends. He noticed that his friend count grew in the following fashion. The first week one friend dropped out and the remaining number of friends doubled. The second week two friends dropped out and the remaining number of friends doubled. In general, in the Nth week, N friends dropped out and the remaining number doubled. Write a program that computes and displays the number of friends each week. The program should continue until the count exceeds Dunbar’s number. Dunbar’s number is a rough estimate of the maximum size of a cohesive social group in which each member knows every other member and how they relate to one another. Its approximate value is 150.

---
```c
#include <stdio.h>

int main(void)
{
	const int DUNBARS_NUMBER = 150;

	int friends = 5, week = 0;

	printf("Week | Friends\n");
	printf("-----+--------\n");
	while (friends < DUNBARS_NUMBER)
	{
		printf("%4d | %7d\n", week, friends);
		week++;
		friends -= week;
		friends *= 2;
	}

	return 0;
}
```