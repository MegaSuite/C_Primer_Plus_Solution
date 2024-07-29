# Chapter 11

<h1>Review Questions</h1>

## 11-1
下面字符串的声明有什么问题？
```c
int main(void)
{
	char name[] = {'F', 'e', 's', 's' };
	...
}
```

> 如果希望得到一个字符串，初始化列表中应包含'\0'。当然，也可以用另一种语法自动添加空字符：`char name[] = "Fess";`

## 11-2
下面的程序会打印什么？
```c
#include <stdio.h>
int main(void)
{
	char note[] = "See you at the snack bar.";
	char *ptr;
	ptr = note;
	puts(ptr);
	puts(++ptr);
	note[7] = '\0';
	puts(note);
	puts(++ptr);
	return 0;
}
```

	See you at the snack bar.
	ee you at the snack bar.
	See you
	e you

## 11-3
下面的程序会打印什么？
```c
#include <stdio.h>
#include <string.h>
int main(void)
{
	char food [] = "Yummy";
	char *ptr;
	ptr = food + strlen(food);
	while (--ptr >= food)
		puts(ptr);
	return 0;
}
```

	y
	my
	mmy
	ummy
	Yummy

## 11-4
下面的程序会打印什么？
```c
#include <stdio.h>
#include <string.h>
int main(void)
{
	char goldwyn[40] = "art of it all ";
	char samuel[40] = "I read p";
	const char * quote = "the way through.";
	strcat(goldwyn, quote);
	strcat(samuel, goldwyn);
	puts(samuel);
	return 0;
}
```

> I read part of it all the way through.

## 11-5
下面的练习涉及字符串、循环、指针和递增指针。首先，假设定义了下面的函数：
```c
#include <stdio.h>
char *pr(char *str)
{
	char *pc;
	pc = str;
	while (*pc)
		putchar(*pc++);
	do {
		putchar(*--pc);
	} while (pc - str);
	return (pc);
}
```
考虑下面的函数调用：
```c
x = pr("Ho Ho Ho!");
```

	a.将打印什么？
	b.x是什么类型？
	c.x的值是什么？
	d.表达式*--pc是什么意思？与--*pc有何不同？
	e.如果用*pc--替换*--pc，会打印什么？
	f.两个while循环用来测试什么？
	g.如果pr()函数的参数是空字符串，会怎样？
	h.必须在主调函数中做什么，才能让pr()函数正常运行？

```c
	a.Ho Ho Ho!!oH oH oH
	b.指向char的指针（即，char *）。
	c.第1个H的地址。
	d.*--pc的意思是把指针递减1，并使用储存在该位置上的值。--*pc的意思是解引用pc指向的值，然后把该值减1（例如，H变成G）。
	e.Ho Ho Ho!!oH oH o
	注意在两个！之间有一个空字符，但是通常该字符不会产生任何打印的效果。
	f.while  (*pc)检查  pc  是否指向一个空字符（即，是否指向字符串的末尾）。while 的测试条件中使用储存在指针指向位置上的值。
	while (pc - str)检查pc是否与str指向相同的位置（即，字符串的开头）。while的测试条件中使用储存在指针指向位置上的值。
	g.进入第1个while循环后，pc指向空字符。进入第2个while循环后，它指向空字符前面的存储区（即，str  所指向位置前面的位置）。把该字节解释成一个字符，并打印这个字符。然后指针退回到前面的字节处。永远都不会满足结束条件(pc == str)，所以这个过程会一直持续下去。
	h.必须在主调程序中声明pr()：char * pr(char *);
```

## 11-6
假设有如下声明：`char sign = '$';`，`sign`占用多少字节的内存？`'$'`占用多少字节的内存？`"$"`占用多少字节的内存？

> 字符变量占用一个字节，所以sign占1字节。但是字符常量储存为int类型，意思是`'$'`通常占用2或4字节。但是实际上只使用int的1字节储存`'$'`的编码。字符串`"$"`使用2字节：一个字节储存`'$'`的编码，一个字节储存的`'\0'`编码。

## 11-7
下面的程序会打印出什么？
```c
#include <stdio.h>
#include <string.h>
#define M1 "How are ya, sweetie? "
char M2[40] = "Beat the clock.";
char * M3 = "chat";
int main(void)
{
	char words[80];
	printf(M1);
	puts(M1);
	puts(M2);
	puts(M2 + 1);
	strcpy(words, M2);
	strcat(words, " Win a toy.");
	puts(words);
	words[4] = '\0';
	puts(words);
	while (*M3)
		puts(M3++);
	puts(--M3);
	puts(--M3);
	M3 = M1;
	puts(M3);
	return 0;
}
```
```c
打印的内容如下：
How are ya, sweetie? How are ya, sweetie?
Beat the clock.
eat the clock.
Beat the clock. Win a toy.
Beat
chat
hat
at
t
t
at
How are ya, sweetie?
```

## 11-8
下面的程序会打印出什么？
```c
#include <stdio.h>
int main(void)
{
	char str1 [] = "gawsie";
	char str2 [] = "bletonism";
	char *ps;
	int i = 0;
	for (ps = str1; *ps != '\0'; ps++) 
	{
		if (*ps == 'a' || *ps == 'e')
			putchar(*ps);
		else
			(*ps)--;
		putchar(*ps);
	}
	putchar('\n');
	while (str2[i] != '\0') 
	{
		printf("%c", i % 3 ? str2[i] : '*');
		++i;
	}
	return 0;
}
```
```c
打印的内容如下：
faavrhee
*le*on*sm
```

## 11-9
本章定义的s_gets()函数，用指针表示法代替数组表示法便可减少一个变量i。请改写该函数。
```c
#include <stdio.h> // 提供fgets()和getchar()的原型
char * s_gets(char * st, int n)
{
	char * ret_val;
	ret_val = fgets(st, n, stdin);
	if (ret_val)
	{
		while (*st != '\n' && *st != '\0')
			st++;
		if (*st == '\n')
			*st = '\0';
		else
			while (getchar() != '\n')
				continue;
	}
	return ret_val;
}
```

## 11-10
strlen()函数接受一个指向字符串的指针作为参数，并返回该字符串的长度。请编写一个这样的函数。
```c
int strlen(const char * s)
{
	int ct = 0;
	while (*s++)   // 或者while (*s++ != '\0')
		ct++;
	return(ct);
}
```

## 11-11
本章定义的s_gets()函数，可以用strchr()函数代替其中的while循环来查找换行符。请改写该函数。
```c
#include <stdio.h>   // 提供 fgets()和getchar()的原型
#include <string.h>  // 提供 strchr()的原型
char * s_gets(char * st, int n)
{
	char * ret_val;
	char * find;
	ret_val = fgets(st, n, stdin);
	if (ret_val)
	{
		find = strchr(st, '\n');  // 查找换行符
		if (find)          // 如果地址不是 NULL,
			*find = '\0';     // 在此处放置一个空字符
		else
			while (getchar() != '\n')
				continue;
	}
	return ret_val;
}
```

## 11-12
设计一个函数，接受一个指向字符串的指针，返回指向该字符串第1个空格字符的指针，或如果未找到空格字符，则返回空指针。
```c
#include <stdio.h>  /* 提供 NULL 的定义 */
char * strblk(char * string)
{
	while (*string != ' ' && *string != '\0')
		string++;    /* 在第1个空白或空字符处停止 */
	if (*string == '\0')
		return NULL;   /* NULL 指空指针 */
	else
		return string;
}

//下面是第2种方案，可以防止函数修改字符串，但是允许使用返回值改变字符串。表达式(char*)string被称为“通过强制类型转换取消const”。

#include <stdio.h>  /*提供 NULL 的定义*/
char * strblk(const char * string)
{
	while (*string != ' ' && *string != '\0')
		string++;    /*在第1个空白或空字符处停止*/
	if (*string == '\0')
		return NULL;   /* NULL 指空指针*/
	else
		return (char *)string;
}
```

## 11-13
重写程序清单11.21，使用ctype.h头文件中的函数，以便无论用户选择大写还是小写，该程序都能正确识别答案。
```c
/* compare.c -- 可行方案 */
#include <stdio.h>
#include <string.h> // 提供strcmp()的原型
#include <ctype.h>
#define ANSWER "GRANT"
#define SIZE 40
char * s_gets(char * st, int n);
void ToUpper(char * str);
int main(void)
{
	char try[SIZE];
	puts("Who is buried in Grant's tomb?");
	s_gets(try, SIZE);
	ToUpper(try);
	while (strcmp(try, ANSWER) != 0)
	{
		puts("No, that's wrong.Try again.");
		s_gets(try, SIZE);
		ToUpper(try);
	}
	puts("That's right!");
	return 0;
}

void ToUpper(char * str)
{
	while (*str != '\0')
	{
		*str = toupper(*str);
		str++;
	}
}

char * s_gets(char * st, int n)
{
	char * ret_val;
	int i = 0;
	ret_val = fgets(st, n, stdin);
	if (ret_val)
	{
		while (st[i] != '\n' && st[i] != '\0')
			i++;
		if (st[i] == '\n')
			st[i] = '\0';
		else
			while (getchar() != '\n')
				continue;
	}
	return ret_val;
}
```

<h1>Programming Exercises</h1>

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

