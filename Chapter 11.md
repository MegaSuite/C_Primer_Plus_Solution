# Chapter 11
## 11-1
Design and test a function that fetches the next n characters from input (including blanks, tabs, and newlines), storing the results in an array whose address is passed as an argument.
***
```c
#include <stdio.h>
#include <string.h>

#define SIZE 20

char * sgetnchar(char *array, int n);

int main(void)
{
	// test sgetnchar

	char hello[SIZE] = "Hello, ";
	int space = SIZE - strlen(hello) - 1;

	printf("What's your name? (enter %d characters)\n", space);
	sgetnchar(hello + 7, space);
	puts(hello);

	return 0;
}

char * sgetnchar(char *array, int n)
{
	// gets n characters from input and stores them in character array

	char ch;

	for (int i = 0; i < n; i++)
	{
		if ((ch = getchar()) == EOF)
			break;

		*(array + i) = ch;
	}

	return array;
}
```

## 11-2
Modify and test the function in exercise 1 so that it stops after n characters or after the first blank, tab, or newline, whichever comes
first. (Don’t just use scanf().)
***
```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

#define SIZE 20

char * sgetnchar(char *array, int n);

int main(void)
{
	// test sgetnchar

	char hello[SIZE] = "Hello, ";
	int space = SIZE - strlen(hello) - 1;

	puts("What's your name?");
	sgetnchar(hello + 7, space);
	puts(hello);

	return 0;
}

char * sgetnchar(char *array, int n)
{
	// get characters from input and store them in character array,
	// stopping after n characters or first whitespace

	int i = 0;
	char ch;

	while ((ch = getchar()) != EOF && !isspace(ch) && i < n)
	{
		*(array + i) = ch;
		i++;
	}

	return array;
}
```

## 11-3
Design and test a function that reads the first word from a line of input into an array and discards the rest of the line. It should skip over leading whitespace. Define a word as a sequence of characters with no blanks, tabs, or newlines in it. 
***
```c
#include <stdio.h>
#include <ctype.h>
#include <stdbool.h>

#define SIZE 20

char * getword(char *target);

int main(void)
{
	// test getword()

	char hello[SIZE] = "Hello, ";

	printf("What's your name?");
	getword(hello + 7);
	puts(hello);

	return 0;
}

char * getword(char *target)
{
	// read input into character array target
	// stop after first word or EOF
	// discard rest of the line

	char ch;
	int i = 0;
	bool inword = false;

	while ((ch = getchar()) != EOF)
	{
		if (isspace(ch))
		{
			if (inword)
				break; // word is over, exit while loop
			else
			{
				continue; // skip leading whitespace
			}
		}

		// if ch is not whitespace
		if (!inword)
			inword = true;

		*(target + i) = ch;
		i++;
	}

	// discard rest of the line if any
	if (ch != '\n')
		while ((ch = getchar()) != '\n')
			continue;

	return target;
}
```

## 11-4
Design and test a function like that described in Programming Exercise 3 except that it accepts a second parameter specifying the maximum number of characters that can be read.
***
```c
#include <stdio.h>
#include <ctype.h>
#include <stdbool.h>
#include <string.h>

#define SIZE 20

char * getword(char *target, int max);

int main(void)
{
	// test getword()

	char hello[SIZE] = "Hello, ";
	int space = SIZE - strlen(hello) - 1;

	puts("What's your name?");
	getword(hello + 7, space);
	puts(hello);

	return 0;
}

char * getword(char *target, int max)
{
	// read input into character array target
	// stop after first word , EOF or max characters
	// discard rest of the line

	char ch;
	int i = 0;
	bool inword = false;

	while ((ch = getchar()) != EOF && i < max)
	{
		if (isspace(ch))
		{
			if (inword)
				break; // word is over, exit while loop
			else
			{
				continue; // skip leading whitespace
			}
		}

		// if ch is not whitespace
		if (!inword)
			inword = true;

		*(target + i) = ch;
		i++;
	}

	// discard rest of the line if any
	if (ch != '\n')
		while ((ch = getchar()) != '\n')
			continue;

	return target;
}
```

## 11-5
Design and test a function that searches the string specified by the first function parameter for the first occurrence of a character specified by the second function parameter. Have the function return a pointer to the character if successful, and a null if the character is not found in the string. (This duplicates the way that the library strchr() function works.) Test the function in a complete program that uses a loop to provide input values for feeding to the function.
***
```c
#include <stdio.h>

#define LIMIT 50

char * findchar(char *str, const char ch);

int main(void)
{
	char string[LIMIT];
	char *chloc;
	char ch;

	// test findchar()
	printf("Enter a string to search: ");
	fgets(string, LIMIT, stdin);
	while (string[0] != '\n')
	{
		printf("Enter a character to search for: ");
		ch = getchar();
		// discard rest of line if any
		if (ch != '\n')
			while (getchar() != '\n')
				continue;

		chloc = findchar(string, ch);
		if (chloc == NULL)
			printf("Character %c not found in %s", ch, string);
		else
			printf("Character %c found  at index %lu in %s", ch, chloc - string, string);

		// get new input
		printf("Enter a string to search (empty line to quit): ");
		fgets(string, LIMIT, stdin);
	}

	puts("Bye");

	return 0;
}

char * findchar(char *str, const char ch)
{
	// find and return pointer to first occurence of
	// char ch in string str. return NULL if not found

	while (*str != '\0')
	{
		if (*str == ch)
			return str;
		str++;
	}

	// if ch is not found, return null
	return NULL;
}
```

## 11-6
Write a function called is_within() that takes a character and a string pointer as its two function parameters. Have the function return a
nonzero value (true) if the character is in the string and zero (false) otherwise. Test the function in a complete program that uses a loop to provide input values for feeding to the function.
***
```c
#include <stdio.h>

#define LIMIT 50

int is_within(char ch, const char *string);
char * get(char *string, int n);

int main(void)
{
	// test is_within()

	char string[LIMIT];
	char ch;

	printf("Enter a string to search: ");
	get(string, LIMIT);
	while (string[0] != '\0')
	{
		printf("Enter a character to search for: ");
		ch = getchar();

		if (ch != '\n')
			while (getchar() != '\n')
				continue;

		char *contains = is_within(ch, string) ? "" : " not";

		printf("\"%s\" does%s contain %c\n", string, contains, ch);

		printf("Enter a string to search (empty line to quit): ");
		get(string, LIMIT);
	}
	puts("Bye");

	return 0;
}

int is_within(char ch, const char *string)
{
	// returns 1 if char ch is in string
	// returns 0 otherwise

	while(*string != '\0')
	{
		if (*string == ch)
			return 1;

		string++;
	}

	return 0;
}

char * get(char *string, int n)
{
	// wrapper for fgets that replaces first newline with null

	char *return_value = fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}

		string++;
	}

	return return_value;
}
```

## 11-7
The strncpy(s1,s2,n) function copies exactly n characters from s2 to s1, truncating s2 or padding it with extra null characters as necessary. The target string may not be null-terminated if the length of s2 is n or more. The function returns s1. Write your own version of this function; call it mystrncpy(). Test the function in a complete program that uses a loop to provide input values for feeding to the function.
***
```c
#include <stdio.h>

#define LIMIT 50

char * mystrcpy(char *s1, char *s2, int n);
char * get(char *string, int n);
void clear_string(char * string, int n);

int main(void)
{
	char s1[LIMIT];
	char s2[LIMIT];
	int n;

	printf("Enter a string to copy: ");
	get(s2, LIMIT);
	while (s2[0] != '\0')
	{
		printf("How many characters do you want to copy? (maximum %d) ", LIMIT);
		scanf("%d", &n);

		while (getchar() != '\n')
			continue;

		if (n > LIMIT)
			n = LIMIT;

		printf("Original string: %s\n", s2);
		mystrcpy(s1, s2, n);
		printf("Copy: %s\n", s1);

		clear_string(s1, LIMIT);

		printf("Enter a string to copy (empty line to quit): ");
		get(s2, LIMIT);
	}

	puts("Bye");

	return 0;
}

char * mystrcpy(char *s1, char *s2, int n)
{
	// copy n characters from s2 to s1
	// warning: if length of s2 is n or greater
	// then s1 may not be null terminated

	int i = 0;

	// copy n characters or until null char
	while (s2[i] != '\0' && i < n)
	{
		s1[i] = s2[i];
		i++;
	}

	// if i < n pad s1 with null character
	while (i < n)
	{
		s1[i] = '\0';
		i++;
	}

	return s1;
}

char * get(char *string, int n)
{
	// wrapper for fgets that replaces first newline with null

	char *return_value = fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}

		string++;
	}

	return return_value;
}

void clear_string(char *string, int n)
{
	// replace first n characters in string with nulls

	for (int i = 0; i < n; i++)
		string[i] = '\0';
}
```

## 11-8
Write a function called string_in() that takes two string pointers as arguments. If the second string is contained in the first string, have the function return the address at which the contained string begins. Forinstance, string_in("hats", "at") would return the address of the a in hats. Otherwise, have the function return the null pointer. Test the function in a complete program that uses a loop to provide input values for feeding to the function.
***
```c
#include <stdio.h>

#define LIMIT 50

char * string_in(char *string, char *substring);
char * get(char *string, int n);

int main(void)
{
	// test string_in()

	char string[LIMIT];
	char substring[LIMIT];

	char *substr_loc;

	printf("Enter a string: ");
	get(string, LIMIT);
	while (string[0] != '\0')
	{
		printf("Enter a substring to look for: ");
		get(substring, LIMIT);

		substr_loc = string_in(string, substring);

		if (substr_loc == NULL)
			printf("%s not in %s\n", substring, string);
		else
			printf("%s found in %s at index %lu\n",
				   substring, string, substr_loc - string);

		printf("Enter a string (empty line to quit): ");
		get(string, LIMIT);
	}

	puts("Bye");

	return 0;
}

char * string_in(char *string, char *substring)
{
	// checks if substring is in string
	// returns pointer to first location of substring
	// in string or NULL if substring not in string

	int i;

	while (*string != '\0')
	{
		i = 0;

		// check for substring at current location
		while (*(string + i) == *(substring + i))
		{
			i++;

			// if next char in substring is null, then match
			// is found. return current location
			if (*(substring + i) == '\0')
				return string;
		}

		string++;
	}

	// no match
	return NULL;
}


char * get(char *string, int n)
{
	// wrapper for fgets that replaces first newline with null

	char *return_value = fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}

		string++;
	}

	return return_value;
}
```

## 11-9
Write a function that replaces the contents of a string with the string reversed. Test the function in a complete program that uses a loop to
provide input values for feeding to the function.
***
```c
#include <stdio.h>

#define LIMIT 50

void reverse_string(char *string);
char * get(char *string, int n);

int main(void)
{
	// test reverse_string()

	char string[LIMIT];

	printf("Enter a string to reverse: ");
	get(string, LIMIT);
	while (string[0] != '\0')
	{
		reverse_string(string);
		printf("Your string reversed: %s\n", string);

		printf("Enter a string to reverse (empty line to quit): ");
		get(string, LIMIT);
	}

	puts("Bye");
	return 0;
}

void reverse_string(char *start)
{
	// replace contents of a string with the string reversed

	char *end = start;
	char tmp;

	// find location of the end of the string
	while (*(end + 1) != '\0')
		end++;

	// switch characters
	while (start < end)
	{
		tmp = *start;
		*start = *end;
		*end = tmp;

		start++;
		end--;
	}
}


char * get(char *string, int n)
{
	// wrapper for fgets that replaces first newline with null

	char *return_value = fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}

		string++;
	}

	return return_value;
}
```

## 11-10
Write a function that takes a string as an argument and removes the spaces from the string. Test it in a program that uses a loop to read lines until you enter an empty line. The program should apply the function to each input string and display the result.
***
```c
#include <stdio.h>

#define LIMIT 50

void remove_spaces(char *string);
char * get(char *string, int n);

int main(void)
{
	// test remove_spaces()

	char string[LIMIT];

	printf("Test remove_spaces()\n");

	printf("Enter a string: ");
	get(string, LIMIT);
	while (string[0] != '\0')
	{
		remove_spaces(string);
		printf("Your string, without spaces: %s\n", string);

		printf("Enter a string (empty line to quit): ");
		get(string, LIMIT);
	}
	puts("Bye");

	return 0;
}

void remove_spaces(char *string)
{
	// remove all spaces from a string

	unsigned long spaces_found = 0;

	while (1)
	{
		if (*string == ' ')
			spaces_found++;
		else
			*(string - spaces_found) = *string;

		// if end of string, break
		if (*string == '\0')
			break;

		string++;
	}
}

char * get(char *string, int n)
{
	// wrapper for fgets that replaces first newline with null

	char *return_value = fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}

		string++;
	}

	return return_value;
}
```

## 11-11
Write a program that reads in up to 10 strings or to EOF, whichever comes first. Have it offer the user a menu with five choices: print the original list of strings, print the strings in ASCII collating sequence, print the strings in order of increasing length, print the strings in order of the length of the first word in the string, and quit. Have the menu recycle until the user enters the quit request. The program, of course, should actually perform the promised tasks.
***
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define COUNT 10
#define LIMIT 50

void sort_ASCII(char *strings[], int n);
void sort_length(char *strings[], int n);
int fwlen(char *string);
void sort_firstword_length(char *strings[], int n);
char * get(char *string, int n);
void print_menu(void);

int main (void)
{
	char strings[COUNT][LIMIT];
	char *strptrs[COUNT];
	char * success;
	char ch;

	// initialize array of pointers
	for (int i = 0; i < COUNT; i++)
		strptrs[i] = strings[i];

	printf("Enter up to 10 strings (EOF to stop): \n");

	// read up to ten strings from input
	for (int i = 0; i < COUNT; i++)
	{
		printf("%d: ", i + 1);
		success = get(strings[i], LIMIT);

		// if EOF encountered, stop reading strings
		if (!success)
			break;
	}

	printf("\n");

	print_menu();
	while ((ch = getchar()) != 'q')
	{
		// discard rest of the line
		if (ch != '\n')
			while (getchar() != '\n')
				continue;

		// sort strings
		switch (ch)
		{
			case ('a'):
				sort_ASCII(strptrs, COUNT);
				break;
			case ('l'):
				sort_length(strptrs, COUNT);
				break;
			case ('f'):
				sort_firstword_length(strptrs, COUNT);
				break;
			case ('o'):
				break;
			default:
				printf("Invalid input. Try again.\n\n");
				print_menu();
				continue;
		}

		// print sorted strings
		puts("");
		for (int i = 0; i < COUNT; i++)
			puts(strptrs[i]);

		puts("");
		print_menu();
	}

	puts("Bye");
	return 0;
}

void sort_ASCII(char *strings[], int n)
{
	// sort array of string pointers by ASCII collating sequence

	char *tmp;

	for (int i = 0; i < n - 1; i++)
		for (int j = i + 1; j < n; j++)
		{
			if (strcmp(strings[i], strings[j]) > 0)
			{
				tmp = strings[i];
				strings[i] = strings[j];
				strings[j] = tmp;
			}
		}
}

void sort_length(char *strings[], int n)
{
	// sort array of string pointers by length

	char *tmp;

	for (int i = 0; i < n - 1; i++)
		for (int j = i + 1; j < n; j++)
		{
			if (strlen(strings[i]) > strlen(strings[j]))
			{
				tmp = strings[i];
				strings[i] = strings[j];
				strings[j] = tmp;
			}
		}
}

int fwlen(char *string)
{
	// return length of first word of string

	int length = 0;

	// skip leading whitespace
	while (isspace(*string))
		string++;

	// count first word length
	while(!isspace(*string))
	{
		length++;
		string++;
	}

	return length;
}

void sort_firstword_length(char *strings[], int n)
{
	// sort array of string pointers by ASCII collating sequence

	char *tmp;

	for (int i = 0; i < n - 1; i++)
		for (int j = i + 1; j < n; j++)
		{
			if (fwlen(strings[i]) > fwlen(strings[j]))
			{
				tmp = strings[i];
				strings[i] = strings[j];
				strings[j] = tmp;
			}
		}
}

char * get(char *string, int n)
{
	// wrapper for fgets that replaces first newline with null

	char *return_value = fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}

		string++;
	}

	return return_value;
}

void print_menu(void)
{
	puts("Choose an option:");
	puts("(o) Print strings in original order.");
	puts("(a) Print strings in ASCII collating sequence.");
	puts("(l) Print strings ordered by length.");
	puts("(f) Print strings ordered by length of the first word.");
	puts("(q) Quit.");
	puts("");
	puts("Enter a character: ");
}
```

## 11-12
Write a program that reads input up to EOF and reports the number of words,
the number of uppercase letters, the number of lowercase letters, the number
of punctuation characters, and the number of digits. Use the ctype.h family
of functions.
***
```c
#include <stdio.h>
#include <ctype.h>
#include <stdbool.h>

int main(void)
{
	// read input to EOF and report number of words, lowercase letters,
	// uppercase letters, punct. marks and digits

	char ch;
	int upcase_letters, lowcase_letters, punct_chars, digits, words;
	upcase_letters = lowcase_letters = punct_chars = digits = words = 0;
	bool inword = false;

	while ((ch = getchar()) != EOF)
	{
		if (isalpha(ch))
		{
			if (!inword) // count new word
			{
				inword = true;
				words++;
			}

			if (isupper(ch))
				upcase_letters++;
			if (islower(ch))
				lowcase_letters++;
		}
		else if (ispunct(ch))
		{
			punct_chars++;

			if (ch != '-' && ch != '\'')
				inword = false;
		}
		else if (isdigit(ch))
		{
			digits++;
			inword = false;
		}
		else if (isspace(ch))
			inword = false;
	}

	printf("Number of %s: %d\n", "lowercase letters", lowcase_letters);
	printf("Number of %s: %d\n", "uppercase letters", upcase_letters);
	printf("Number of %s: %d\n", "punctuation characters", punct_chars);
	printf("Number of %s: %d\n", "digits", digits);
	printf("Number of %s: %d\n", "words", words);

	return 0;
}
```

## 11-13
Write a program that echoes the command-line arguments in reverse word order. That is, if the command-line arguments are see you later, the program should print later you see.
***
```c
#include <stdio.h>

int main(int argc, char *argv[])
{
	if (argc < 2)
	{
		printf("Error: at least one command-line argument required.\n");
		return 1;
	}
	else
		for (int i = argc - 1; i > 0; i--)
			printf("%s ", argv[i]);

	puts("");
	return 0;
}
```

## 11-14
Write a power-law program that works on a command-line basis. The first command-line argument should be the type double number to be raised to a certain power, and the second argument should be the integer power.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

void print_error_message(void);

int main(int argc, char *argv[])
{
	double base;
	long power;
	char *end;

	if (argc != 3)
	{
		print_error_message();
		return 1;
	}

	// get base
	end = argv[1];
	while (*end != '\0')
		end++;
	base = strtod(argv[1], &end);

	if (*end) // error condition
	{
		print_error_message();
		return 1;
	}

	// get exponent
	end = argv[2];
	while (*end != '\0')
		end++;
	power = strtol(argv[2], &end, 10);

	if (*end) // error condition
	{
		print_error_message();
		return 1;
	}

	printf("%f ^ %ld = %f\n", base, power, pow(base, power));

	return 0;
}

void print_error_message(void)
{
	puts("Usage: <program_name> <arg1 base:double> <arg2 power:int>");
}
```

## 11-15
Use the character classification functions to prepare an implementation of atoi(); have this version return the value of 0 if the input string is not a pure number.
***
```c
#include <stdio.h>
#include <ctype.h>

int my_atoi(char *string);

int main(int argc, char *argv[])
{
	// driver for my_atoi()

	int input;

	if (argc != 2)
		puts("Usage: <program_name> <arg_1>");
	else
	{
		input = my_atoi(argv[1]);
		printf("%d + 5 = %d\n", input, input + 5);
		printf("%d / 2 = %d\n", input, input / 2);
		printf("%d %% 3 = %d\n", input, input % 3);
	}

	return 0;
}

int my_atoi(char *string)
{
	// convert string to integer
	// returns 0 on error

	int total = 0;

	while (*string != '\0')
	{
		if (!isdigit(*string))
			// string is not a pure integer -- return 0
			return 0;
		else
		{
			total *= 10; // shift digits one place to left
			total += *string - '0'; // add newest digit in ones place
		}

		string++;
	}

	return total;
}
```

## 11-16
Write a program that reads input until end-of-file and echoes it to the display. Have the program recognize and implement the following command-line arguments:
	- p Print input as is
	- u Map input to all uppercase
	- l Map input to all lowercase
Also, if there are no command-line arguments, let the program behave as if the –p argument had been used.
***
```c
#include <stdio.h>
#include <ctype.h>

void map_identity(void);
void map_uppercase(void);
void map_lowercase(void);

int main(int argc, char *argv[])
{
	if (argc == 1)
	{
		map_identity();
		return 0;
	}

	else if (argc > 2 || (argc == 2 && argv[1][0] != '-'))
	{
		printf("Usage: program_name [-p | -l | -u]\n");
		return 1;
	}

	else
		switch(argv[1][1])
		{
			case ('p'):
				map_identity();
				break;
			case ('u'):
				map_uppercase();
				break;
			case ('l'):
				map_lowercase();
				break;
			default:
				printf("Usage: program_name [-p | -l | -u]\n");
				return 1;
		}

	return 0;
}

void map_identity(void)
{
	char ch;

	while((ch = getchar()) != EOF)
		putchar(ch);
}

void map_uppercase(void)
{
	char ch;

	while((ch = getchar()) != EOF)
	{
		if (islower(ch))
			ch = toupper(ch);
		putchar(ch);
	}
}

void map_lowercase(void)
{
	char ch;

	while((ch = getchar()) != EOF)
	{
		if (isupper(ch))
			ch = tolower(ch);
		putchar(ch);
	}
}
```

