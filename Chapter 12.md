# Chapter 12
## 12-1 
Rewrite the program in Listing 12.4 so that it does not use global variables.
***
```c
#include <stdio.h>

void critic(int *units);

int main(void)
{
	int units; /* an optional redeclaration */
	
	printf("How many pounds to a firkin of butter?\n");
	scanf("%d", &units);
	while (units != 56)
		critic(&units);
	printf("You must have looked it up!\n");
	
	return 0;
}

void critic(int *units) {
	printf("No luck, my friend. Try again.\n");
	scanf("%d", units);
}
```

## 12-2 
Gasoline consumption commonly is computed in miles per gallon in the U.S. and in liters per 100 kilometers in Europe. What follows is part of a program that asks the user to choose a mode (metric or U.S.) and then gathers data and computes fuel consumption. If the user enters an incorrect mode, the program comments on that and uses the most recent mode. Supply a header file and a pe12-2a.c source code file to make this work. The source code file should define three file-scope, internal-linkage variables. One represents the mode, one represents the distance, and one represents the fuel consumed. The get_info() function prompts for data according to the mode setting and stores the responses in the file-scope variables. The show_info() function calculates and displays the fuel consumption based on the mode setting. You can assume the user responds with numeric input.
***
```c
#include <stdio.h>
#include "exercise02.h"

int main(void)
{
	int mode;

	printf("Enter 0 for metric mode, 1 for US mode: ");
	scanf("%d", &mode);
	while (mode >= 0)
	{
		set_mode(mode);
		get_info();
		show_info();
		printf("Enter 0 for metric mode, 1 for US mode");
		printf(" (-1 to quit): ");
		scanf("%d", &mode);
	}

	printf("Done.\n");
	return 0; 
}
```

## 12-2-b
```c
#include <stdio.h>
#include "exercise02.h"

#define METRIC 0
#define US 1

static int mode;
static double distance;
static double fuel;

void clear_input_stream(void)
{
	while (getchar() != '\n')
		continue;
}

void set_mode(int new_mode)
{
	extern int mode;
	if (new_mode == METRIC || new_mode == US)
		mode = new_mode;
	else
		printf("Invalid mode specified. Mode %d(%s) used.\n",
			mode, mode == METRIC ? "metric" : "US");
}

void get_info(void)
{
	printf("Enter distance travelled in %s: ",
		mode == METRIC ? "kilometers" : "miles");
	while (scanf("%lf", &distance) != 1)
	{
		clear_input_stream();
		printf("Invalid input. Enter distance travelled in %s: ",
			mode == METRIC ? "kilometers" : "miles");
	}

	printf("Enter fuel consumed in %s: ",
		mode == METRIC ? "liters" : "gallons");
	while (scanf("%lf", &fuel) != 1)
	{
		clear_input_stream();
		printf("Invalid input. Enter fuel consumed in %s: ",
			mode == METRIC ? "liters" : "gallons");
	}
}

void show_info(void)
{
	double efficiency;

	if (mode == METRIC)
	{
		efficiency = fuel / distance * 100;
		printf("Fuel consumption is %.3f liters per 100 kilometers.\n",
			efficiency);
	}
	else if (mode == US)
	{
		efficiency = distance / fuel;
		printf("Fuel consumption is %.3f miles per gallon.\n",
			efficiency); 
	}
	else
	{
		printf("Error. Invalid mode: %d\n", mode);
	}

}
```

## 12-2-h
```c
void set_mode(int mode);
void get_info(void);
void show_info(void);
```

## 12-3
Redesign the program described in Programming Exercise 2 so that it uses only automatic variables. Have the program offer the same user interface— that is, it should prompt the user to enter a mode, and so on. You’ll have to come up with a different set of function calls, however.
***
```c
#include <stdio.h>
#include "exercise03.h"

int main(void)
{
	int mode = 0;
	double distance, fuel;

	set_mode(&mode);
	while (mode >= 0)
	{
		get_info(mode, &distance, &fuel);
		show_info(mode, distance, fuel);
		set_mode(&mode);
	}

	printf("Done.\n");
	return 0; 
}
```

## 12-3-b
```c
#include <stdio.h>
#include "exercise03.h"

#define METRIC 0
#define US 1
#define QUIT -1

void clear_input_stream(void)
{
	while (getchar() != '\n')
		continue;
}

void set_mode(int *mode)
{
	int new_mode = 2;
	printf("Enter 0 for metric mode, 1 for US mode (-1 to quit): ");

	if (scanf("%d", &new_mode) != 1)
		clear_input_stream();

	if (new_mode == METRIC || new_mode == US || new_mode == QUIT)
		*mode = new_mode;
	else
	{
		printf("Invalid mode specified. Mode %d(%s) used.\n",
			*mode, *mode == METRIC ? "metric" : "US");
	}

}

void get_info(int mode, double *distance, double *fuel)
{
	printf("Enter distance travelled in %s: ",
		mode == METRIC ? "kilometers" : "miles");
	while (scanf("%lf", distance) != 1)
	{
		clear_input_stream();
		printf("Invalid input. Enter distance travelled in %s: ",
			mode == METRIC ? "kilometers" : "miles");
	}

	printf("Enter fuel consumed in %s: ",
		mode == METRIC ? "liters" : "gallons");
	while (scanf("%lf", fuel) != 1)
	{
		clear_input_stream();
		printf("Invalid input. Enter fuel consumed in %s: ",
			mode == METRIC ? "liters" : "gallons");
	}
}

void show_info(int mode, double distance, double fuel)
{
	double efficiency;

	if (mode == METRIC)
	{
		efficiency = fuel / distance * 100;
		printf("Fuel consumption is %.3f liters per 100 kilometers.\n",
			efficiency);
	}
	else if (mode == US)
	{
		efficiency = distance / fuel;
		printf("Fuel consumption is %.3f miles per gallon.\n",
			efficiency); 
	}
	else
	{
		printf("Error. Invalid mode: %d\n", mode);
	}

}
```

## 12-3-h
```c
void set_mode(int *mode);
void get_info(int mode, double *distance, double *fuel);
void show_info(int mode, double distance, double fuel);
```

## 12-4
Write and test in a loop a function that returns the number of times it has been called.
***
```c
#include <stdio.h>

unsigned int counter(void);

int main(void)
{
	// test counter()
	int iterations = 0;
	printf("How many times do you want to call counter()? ");
	scanf("%d", &iterations);
	for (int i = 0; i < iterations; i++)
		printf("counter() returns %u\n", counter());

	return 0;
}

unsigned int counter(void)
{
	static unsigned int call_count = 0;
	return ++call_count;
}
```

## 12-5
Write a program that generates a list of 100 random numbers in the range 1–10 in sorted decreasing order. (You can adapt the sorting algorithm from Chapter 11, “Character Strings and String Functions,” to type int. In this case, just sort the numbers themselves.)
***
```c
#include <stdio.h>
#include <stdlib.h>

void random_ints(int *int_array);

int main(void)
{
	// test random ints
	int int_array[100];
	random_ints(int_array);

	for (int i = 0; i < 100; i++)
	{
		printf("%2d ", int_array[i]);
		if (i % 20 == 19)
			putchar('\n');
	}

	putchar('\n');
	return 0;
}

void random_ints(int *int_array)
{
	int tmp;

	// initialize array
	for (int i = 0; i < 100; i++)
		int_array[i] = rand() % 10 + 1;
	
	// sort array in decreasing order
	for (int i = 0; i < 99; i++)
		for (int j = i + 1; j < 100; j++)
		{
			if (int_array[i] < int_array[j])
			{
				tmp = int_array[i];
				int_array[i] = int_array[j];
				int_array[j] = tmp;
			}
		}
}
```


## 12-6
Write a program that generates 1,000 random numbers in the range 1–10. Don’t save or print the numbers, but do print how many times each number was produced. Have the program do this for 10 different seed values. Do the numbers appear in equal amounts? You can use the functions from this chapter or the ANSI C rand() and srand() functions, which follow the same format that our functions do. This is one way to examine the randomness of a particular random-number generator.
***
```c
#include <stdio.h>
#include <stdlib.h>

#define SIZE 1000


void random_sample(void);

int main(void)
{
	for (int i = 1; i < 11; i++)
	{
		printf("Random Sample: run #%d\n", i);
		srand(i);
		random_sample();
		putchar('\n');
	}

	return 0;
}

void random_sample(void)
{
	int counts[10] = {0,0,0,0,0,0,0,0,0,0};
	const int RAND_LIMIT = (RAND_MAX / 10) * 10;
	int rand_int;

	int i = 0;
	while (i < SIZE)
	{
		rand_int = rand();
		if (rand_int >= RAND_LIMIT)
			continue;
		else
		{
			rand_int %= 10;
			counts[rand_int]++;
		}
		i++;
	}
	puts("Counts");
	for (int i = 0; i < 10; i++)
		printf("%2d: %3d  ", i + 1, counts[i]);
	putchar('\n');

}
```

## 12-7
Write a program that behaves like the modification of Listing 12.13, which we discussed after showing the output of Listing 12.13. That is, have the program produce output like the following:
    > Enter the number of sets; enter q to stop: 18
    > How many sides and how many dice? 6 3
    > Here are 18 sets of 3 6-sided throws.
    > 12 10  6  9  8 14  8 15  9 14 12 17 11  7 10
      13  8 14
    > How many sets? Enter q to stop: q

***
```c
#include <stdio.h>
#include "exercise07.h"

void clear_input_stream(void);

int main(void)
{
	int sets, dice, sides;

	puts("Enter the number of sets; enter q to stop.");
	while (scanf("%d", &sets) == 1 && sets > 0)
	{
		dice = sides = -1;
		printf("How many sides and how many dice? ");
		scanf("%d %d", &sides, &dice);
		while (dice < 0 || sides < 0)
		{
			clear_input_stream();
			puts("Invalid input. Positive integers only.");
			puts("How many sides and how many dice? ");
			scanf("%d %d", &sides, &dice);
		}

		dicerolls(sets, dice, sides);

		puts("How many sets? Enter q to stop.");
	}

	return 0;
}

void clear_input_stream(void)
{
	while (getchar() != '\n')
		continue;
}
```

## 12-7-b
```c
#include "exercise07.h"
#include <stdio.h>
#include <stdlib.h>

static int rolldie(int sides)
{
	// roll one n-sided die and return outcome

	return rand() % sides + 1;
}

int rolldice(int dice, int sides)
{
	// roll multiple n-sided dice and return outcome

	int total = 0;

	for (int i = 0; i < dice; i++)
		total += rolldie(sides);

	return total;
}

void dicerolls(int sets, int dice, int sides)
{
	// roll multiple sets of multiple n-sided dice and print results

	printf("Here are %d sets of %d %d-sided throws.\n", sets, dice, sides);
	for (int i = 0; i < sets; i++)
		printf("%2d ", rolldice(dice, sides));
	putchar('\n');
}
```

## 12-7-h
```c
int rolldice(int dice, int sides);

void dicerolls(int sets, int dice, int sides);
```

## 12-8
Complete the program by providing function definitions for make_array() and show_array(). The make_array() function takes two arguments. The first is the number of elements of an int array, and the second is a value that is to be assigned to each element. The function uses malloc() to create an array of a suitable size, sets each element to the indicated value, and returns a pointer to the array. The show_array() function displays the contents, eight numbers to a line.
***
```c
#include <stdio.h>
#include <stdlib.h>

int * make_array(int elem, int val);

void show_array(const int ar[], int n);

int main(void)
{
	int * pa;
	int size;
	int value;

	printf("Enter the number of elements: ");
	while (scanf("%d", &size) == 1 && size > 0)
	{
		printf("Enter the initialization value: ");
		scanf("%d", &value);

		pa = make_array(size, value);
		if (pa)
		{
			show_array(pa, size);
			free(pa);
		}
		printf("Enter the number of elements (<1 to quit): ");
	}
	printf("Done.\n");
	return 0;
}

int * make_array(int elem, int val)
{
	// generate and return pointer to a new array of ints
	// of length elem with each element initialized to val

	int *arr = (int *) malloc(elem * sizeof(int));

	if (arr != NULL)
		for (int i = 0; i < elem; i++)
			arr[i] = val;

	return arr;
}

void show_array(const int ar[], int n)
{
	// print contents of an array of ints, 8 elements to a line
	int i;
	for (i = 0; i < n; i++)
	{
		printf("%d ", ar[i]);
		if (i % 8 == 7)
			putchar('\n');
	}
	if (i % 8 != 0)
		putchar('\n');
}
```

## 12-9
Write a program with the following behavior. First, it asks you how many words you wish to enter. Then it has you enter the words, and then it
displays the words. Use malloc() and the answer to the first question (the number of words) to create a dynamic array of the corresponding number of pointers-to-char. (Note that because each element in the array is a pointer-to-char, the pointer used to store the return value of malloc() should be a pointer-to-a-pointer-to-char.) When reading the string, the program should read the word into a temporary array of char, use malloc() to allocate enough storage to hold the word, and store the address in the array of char pointers. Then it should copy the word from the temporary array into the allocated storage. Thus, you wind up with an array of character pointers, each pointing to an object of the precise size needed to store the particular word. A sample run could look like this:
	> How many words do you wish to enter? 
        5
    > Enter 5 words now:
	  I enjoyed doing this exerise
	> Here are your words:
	  I 
   enjoyed 
   doing
   this
   exercise

***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define LIMIT 400

char ** get_words(int n);
static void find_word(char **start, char **end);

int main(void)
{
	int n;
	char ** words;

	printf("How many words do you wish to enter?\n");
	if (scanf("%d", &n) == 1 && n > 0)
	{
		while (getchar() != '\n')
			continue;
		words = get_words(n);

		printf("Here are your words:\n");
		for (int i = 0; i < n; i++)
			printf("%s\n", words[i]);
	}

	return 0;
}

char ** get_words(int n)
{
	char tmp[LIMIT];
	int i;
	char * word_start;
	char * word_end;
	int word_length;

	char ** word_array = (char **) malloc(n * sizeof(char *));

	printf("Enter %d words now:\n", n);
	word_start = fgets(tmp, LIMIT, stdin);

	for (i = 0; i < n; i++)
	{
		// find the start and end of next word
		find_word(&word_start, &word_end);
		
		// if word_start points to null char, there are no more
		// words to find; exit loop
		if (*word_start == '\0')
			break;

		// allocate memory for new word and copy from tmp
		word_length = word_end - word_start;
		word_array[i] = (char *) malloc((word_length + 1) * sizeof(char));
		if (word_array[i] != NULL)
		{
			strncpy(word_array[i], word_start, word_length);
			word_array[i][word_length] = '\0';
		}
		
		word_start = word_end;
	}

	// if less than n words found, set remaining elements of
	// word_array to null string
	if (i < n)
		for (; i < n; i++)
		{	
			word_array[i] = (char *) malloc(sizeof(char));
			*word_array[i] = '\0';
		}

	return word_array;
}

static void find_word(char **start, char **end)
{
	// takes two pointers to pointers to char
	// sets *start to point to first character of first
	// word occuring after **start and sets *end to point
	// to the character immediately after the word

	while (isspace(**start))
		(*start)++;

	*end = *start;

	while (!isspace(**end) && **end != '\0')
		(*end)++;
}
```

