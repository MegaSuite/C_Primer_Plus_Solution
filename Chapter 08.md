# Chapter 08
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


