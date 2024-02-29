# Chapter 08

<h1>Review Questions</h1>

## 8-1
putchar(getchar())是一个有效表达式，它实现什么功能？
getchar(putchar())是否也是有效表达式？

	表达式 putchar(getchar())使程序读取下一个输入字符并打印出来。
	getchar()的返回值是putchar()的参数。但getchar(putchar())是无效的表达式，
	因为getchar()不需要参数，而putchar()需要一个参数。

## 8-2
下面的语句分别完成什么任务？
```c
a.putchar('H');
b.putchar('\007');
c.putchar('\n');
d.putchar('\b');
```

```c
a.显示字符H。
b.如果系统使用ASCII，则发出一声警报。
c.把光标移至下一行的开始。
d.退后一格。
```

## 8-3
假设有一个名为  count  的可执行程序，用于统计输入的字符数。设计一个使用 count 程序统计essay文件中字符数的命令行，并把统计结果保存在essayct文件中。
```bash
count <essay >essayct
或者
count >essayct <essay
```

## 8-4
给定复习题3中的程序和文件，下面哪一条是有效的命令？
```c
a.essayct <essay
b.count essay
c.essay >count
```
> 都不是

## 8-5
EOF是什么？

>EOF是由getchar()和scanf()返回的信号（一个特殊值），表明函数检测到文件结尾。

## 8-6
对于给定的输出（ch是int类型，而且是缓冲输入），下面各程序段的输出分别是什么？

a.输入如下:

`If you quit, I will.[enter]`

程序段如下：
```c
while ((ch = getchar()) != 'i')
putchar(ch);
```
b.输入如下：

`Harhar[enter]`

程序段如下：
```c
while ((ch = getchar()) != '\n')
{
	putchar(ch++);
	putchar(++ch);
}
```

a.输出是：`If you qu`.注意，字符I与字符i不同。还要注意，没有打印i，因为循环在检测到i之后就退出了。

b.如果系统使用ASCII，输出是：`HJacrthjacrt`。while的第1轮迭代中，为ch读取的值是H。第1个putchar()语句使用的ch的值是H，打印完毕后，ch的值加1（现在是ch的值是I）。然后到第2个putchar()语句，因为是++ch，所以先递增ch（现在ch的值是J）再打印它的值。然后进入下一轮迭代，读取输入序列中的下一个字符（a），重复以上步骤。需要注意的是，两个递增运算符只在ch被赋值后影响它的值，不会让程序在输入序列中移动。

## 8-7
C如何处理不同计算机系统中的不同文件和换行约定？

> C的标准I/O库把不同的文件映射为统一的流来统一处理。
>


## 8-8
在使用缓冲输入的系统中，把数值和字符混合输入会遇到什么潜在的问题？

> 数值输入会跳过空格和换行符，但是字符输入不会。假设有下面的代码：
```c
int score;
char grade;
printf("Enter the score.\n");
scanf("%s", %score);
printf("Enter the letter grade.\n");
grade = getchar();
```
>如果输入分数98，然后按下Enter键把分数发送给程序，其实还发送了一个换行符。这个换行符会留在输入序列中，成为下一个读取的值（grade）。如果在字符输入之前输入了数字，就应该在处理字符输入之前添加删除换行符的代码。

<h1>Programming Exercises</h1>

## 8-1
Devise a program that counts the number of characters in its input up to the end of file.
***
```c
#include <stdio.h>

int main(void)
{
	int count = 0;

	while (getchar() != EOF)
	{
		count++;
	}
	printf("Character count: %d\n", count);

	return 0;
}
```
## 8-2
Write a program that reads `input` as a stream of characters until encountering `EOF`. Have the program `print` each input character and its `ASCII` decimal value. Note that characters preceding the space character in the `ASCII` sequence are nonprinting characters. Treat them specially. If the nonprinting character is a newline or tab, print `\n` or `\t`, respectively. Otherwise, use control-character notation. For instance, `ASCII 1` is `Ctrl+A`, which can be displayed as `^A`. Note that the `ASCII` value for` A `is the value for `Ctrl+A` plus `64`. A similar relation holds for the other nonprinting characters. Print 10 pairs per line, except start a fresh line each time a newline character is encountered. (Note: The operating system may have special interpretations for some control characters and keep them from reaching the program.)
***
```c
#include <stdio.h>

int main(void)
{
	int ch, char_count = 0;

	while ((ch = getchar()) != EOF)
	{
		if (ch >= ' ')
			printf("\'%c\': %d", ch, ch);
		else if (ch == '\n')
			printf("\'\\n\': %d", ch);
		else if (ch == '\t')
			printf("\'\\t\': %d", ch);
		else // ascii control characters
			printf("\'^%c\': %d", ch + 64, ch );

		char_count++;
		if (char_count % 10 == 0)
			printf("\n"); // print new line for every 10 characters
		else
			printf("  "); // otherwise, print spaces
	}

	printf("\n");

	return 0;
}
```
## 8-3
Write a program that reads input as a stream of characters until encountering `EOF`. Have it report the number of uppercase letters, the number of lowercase letters, and the number of other characters in the input. You may assume that the numeric values for the lowercase letters are sequential and assume the same for uppercase. Or, more portably, you can use appropriate classification functions from the `ctype.h `library.
***
```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
	int ch;
	int uppercase_count = 0, lowercase_count = 0, other_count = 0;

	while ((ch = getchar()) != EOF)
	{
		if (isupper(ch))
			uppercase_count++;
		else if (islower(ch))
			lowercase_count++;
		else
			other_count++;
	}

	printf("Character Counts\n");
	printf("Uppercase letters: %d\n", uppercase_count);
	printf("Lowercase letters: %d\n", lowercase_count);
	printf("Other: %d\n", other_count);

	return 0;
}
```
## 8-4
Write a program that reads input as a stream of characters until encountering `EOF`. Have it report the average number of letters per word. Don’t count whitespace as being letters in a word. Actually, punctuation shouldn’t be counted either, but don’t worry about that now. (If you do want to worry about it, consider using the ispunct() function from the `ctype.h` family.)
***
```c
#include <stdio.h>
#include <ctype.h>
#include <stdbool.h>

int main(void)
{
	int ch;
	bool in_word = false;
	int letter_count = 0, word_count = 0;

	while ((ch = getchar()) != EOF)
	{
		if (isalpha(ch)) // if ch is a letter
		{
			letter_count++;
			// if not currently in a word, then switch state to in word
			// and increment the word count
			if (!in_word) 
			{
				in_word = true;
				word_count++;
			}
		}
		// if ch is not a letter, set program state to out of word
		else 
			in_word = false;
	}
	// divide letter count by word count to get average letters/word
	printf("Average letters per word: %.2f\n", (float) letter_count / word_count);

	return 0;
}
```
## 8-5
Modify the guessing program of `Listing 8.4` so that it uses a more intelligent guessing strategy. For example, have the program initially guess 50, and have it ask the user whether the guess is high, low, or correct. If, say, the guess is low, have the next guess be halfway between 50 and 100, that is, 75. If that guess is high, let the next guess be halfway between 75 and 50, and so on. Using this binary search strategy, the program quickly zeros in on the correct answer, at least if the user does not cheat.
***
```c
#include <stdio.h>

int main(void)
{
	// initial search parameters
	int upper_bound = 100;
	int lower_bound = 0;
	int guess = 50;

	char ch;

	printf("Pick an integer from 1 to 100. I will try to guess ");
	printf("it.\nRespond with a y if my guess is right, with a h if it's");
	printf("\ntoo high and an l if it's too low.\n");
	printf("Uh...is your number %d?\n", guess);

	while ((ch = getchar()) != 'y')
	{
		while (getchar() != '\n') // clear input stream
			;
		if (ch == 'h')
			upper_bound = guess;
		else if (ch == 'l')
			lower_bound = guess;
		else
		{
			printf("Invalid valid input. Try again.\n");
			continue;
		}
		guess = (upper_bound + lower_bound) / 2.0;
		printf("Well, then, is it %d?\n", guess);
	}

	printf("I knew I could do it!\n");
	return 0;
}
```
## 8-6
Modify the `get_first()` function of Listing 8.8 so that it returns the first non- whitespace character encountered. Test it in a simple program.
***
```c
#include <stdio.h>
#include <ctype.h>

int get_first(void);

int main(void)
{
	int ch;

	printf("Test program for get_first():\n");
	printf("Enter a line; you should see the first non-whitespace\n");
	printf("character echoed in the terminal:\n");

	ch = get_first();
	printf("%c\n", ch);

	return 0;
}

int get_first(void)
{
	// returns first non-whitespace character and clears
	// remaining input until next line break or EOF

	int ch, garbage;

	do {
		ch = getchar();
	}
	while (isspace(ch));
		

	while((garbage = getchar()) != '\n' && garbage != EOF)
		;

	return ch;
}
```
## 8-7
Modify Programming `Exercise 8` from `Chapter 7` so that the menu choices are labeled by characters instead of by numbers; use `q `instead of `5` as the cue to terminate input.
***
```c
#include <stdio.h>
#include <stdbool.h>
#include <ctype.h>

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
int get_first(void);

int main(void)
{
	bool exit_flag = false;
	int rate_option;
	float rate, hours, gross_pay, taxes;

	while (1) // main program loop
	{

		// print usage instructions
		printf("********************************************************************\n");
		printf("Enter the character corresponding to the desired pay rate or action:\n");
		printf("a) $%.2f/hr 				b) $%.2f/hr\n", RATE_1, RATE_2);
		printf("c) $%.2f/hr 				d) $%.2f/hr\n", RATE_3, RATE_4);
		printf("q) quit \n");
		printf("********************************************************************\n");

		rate_option = get_first();
		switch (rate_option)
		{
			case ('a') : 	
				rate = RATE_1;
				break;
			case ('b') : 	
				rate = RATE_2;
				break;
			case ('c') :
				rate = RATE_3;
				break;
			case ('d') :
				rate = RATE_4;
				break;
			case ('q') :
				printf("Bye.\n");
				return 0; // exit program
			default : // invalid input
				printf("Invalid input. Try again.\n\n");
				continue; // repeat main program loop
		}

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

int get_first(void)
{
	// returns first non-whitespace character and clears
	// remaining input until next line break or EOF

	int ch, garbage;

	do {
		ch = getchar();
	}
	while (isspace(ch));
		

	while((garbage = getchar()) != '\n' && garbage != EOF)
		;

	return ch;
}
```
## 8-8
Write a program that shows you a menu offering you the choice of addition, subtraction, multiplication, or division. After getting your choice, the program asks for two numbers, then performs the requested operation. The program should accept only the offered menu choices. It should use type
float for the numbers and allow the user to try again if he or she fails to enter a number. In the case of division, the program should prompt the user to enter a new value if 0 is entered as the value for the second number.
***
```c
#include <stdio.h>
#include <ctype.h>

int get_first(void);
void print_menu(void);
float get_number(void);

int main(void)
{
	int operation;
	float num1, num2;

	print_menu();
	while ((operation = get_first()) != 'q')
	{
		printf("Enter first number: ");
		num1 = get_number();
		printf("Enter second number: ");
		num2 = get_number();

		switch (operation)
		{
			case ('a') :
				printf("%.3f + %.3f = %.3f\n", num1, num2, num1 + num2);
				break;
			case ('s') :
				printf("%.3f - %.3f = %.3f\n", num1, num2, num1 - num2);
				break;
			case ('m') :
				printf("%.3f * %.3f = %.3f\n", num1, num2, num1 * num2);
				break;
			case ('d') :
				while (num2 == 0)
				{
					printf("Enter a number other than 0: ");
					num2 = get_number();
				}
				printf("%.3f / %.3f = %.3f\n", num1, num2, num1 / num2);
				break;
			default :
				printf("I do not recognize that input. Try again.");
		}
		print_menu();
	}


}

int get_first(void)
{
	// return first non-whitespace character
	int ch;

	do ch = getchar(); while (isspace(ch));

	while (getchar() != '\n')
		;

	return ch;
}


void print_menu(void)
{
	printf("Enter the operation of your choice:\n");
	printf("a. add            s. subtract\n");
	printf("m. multiply       d. divide\n");
	printf("q. quit\n");
}

float get_number(void)
{
	int ch;
	float num;

	while (scanf("%f", &num) != 1)
	{
		while ((ch = getchar()) != '\n') // echo user input and clear stream
			putchar(ch);

		printf(" is not a number.\n");
		printf("Please enter a number, such as 2.5, -1.78E8, or 3: ");
	}

	return num;
}
```


