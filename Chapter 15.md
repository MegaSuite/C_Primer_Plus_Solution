# Chapter 15
## 15-1
Write a function that converts a binary string to a numeric value. That is,
if you have
    char * pbin = "01001001";
you can pass pbin as an argument to the function and have the function return an int value of 25.

NOTE: "01001001" in base10 is actually 73

***
```c
#include <stdio.h>

int parsebinarystring(const char * string);

int main(void)
{
	// test parsebinarystring()
	int result;
	char * binstring = "01001001";
	printf("%s in base-10 is %d.\n", binstring, parsebinarystring(binstring));

	return 0;
}

int parsebinarystring(const char * string)
{
	// convert a binary string to a numeric value

	int retval = 0;
	while (*string != '\0')
	{
		retval <<= 1;
		if (*string == '1')
			retval |= 1;
		string++;
	}
	return retval;
}
```

## 15-2
Write a program that reads two binary strings as command-line arguments and prints the results of applying the ~ operator to each number and the results of applying the &, |, and ^ operators to the pair. Show the results as binary strings. (If you don’t have a command-line environment available, have the program read the strings interactively.)

***
```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

int parsebinarystring(const char * string);
char * tobinarystring(int n, char * string, int strlen);

int main(int argc, char *argv[])
{
	if (argc != 3)
	{
		fprintf(stderr, "Usage: %s <arg1> <arg2>\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	int n = parsebinarystring(argv[1]);
	int m = parsebinarystring(argv[2]);
	int strlen = sizeof(int) * CHAR_BIT + 1;
	char binstring[strlen];

	printf("~%s = %s\n", argv[1], tobinarystring(~n, binstring, strlen));
	printf("~%s = %s\n", argv[2], tobinarystring(~m, binstring, strlen));
	printf("%s & %s = %s\n", argv[1], argv[2], tobinarystring(n & m, binstring, strlen));
	printf("%s | %s = %s\n", argv[1], argv[2], tobinarystring(n | m, binstring, strlen));
	printf("%s ^ %s = %s\n", argv[1], argv[2], tobinarystring(n ^ m, binstring, strlen));

	return 0;
}

int parsebinarystring(const char * string)
{
	// convert a binary string to a numeric value

	int retval = 0;
	while (*string != '\0')
	{
		retval <<= 1;
		if (*string != '0' && *string != '1')
		{
			// handle input error
			fprintf(stderr, "Error: input string is not binary.\n");
			return 0;
		}
		if (*string == '1')
			retval |= 1;
		string++;
	}
	return retval;
}

char * tobinarystring(int n, char * string, int strlen)
{
	char * start = string;

	string += (strlen - 1);
	*string = '\0';
	string--;

	if (n == 0)
	{
		*string = '0';
		return string;
	}

	while (n != 0 && string >= start)
	{
		*string = (n & 1) + '0';
		n = n >> 1;
		string--;
	}
	return ++string;
}
```

## 15-3
Write a function that takes an int argument and returns the number of “on” bits in the argument. Test the function in a program.
***
```c
#include <stdio.h>
#define CLEARINPUT while (getchar() != '\n') continue

int onbits(int n);

int main(void)
{
	// test onbits

	int n, bits;

	printf("Enter an integer (non-integer to quit): ");
	while (scanf("%d", &n) == 1)
	{
		CLEARINPUT;
		
		bits = onbits(n);
		printf("There are %d on bits in %d\n", bits, n);

		printf("Enter an integer (non-integer to quit): ");
	}

	return 0;
}

int onbits(int n)
{
	// returns number of bits in n set to 1
	int count = 0;
	while (n != 0)
	{
		if (n & 1)
			count++;
		n >>= 1;
	}

	return count;
}
```

## 15-4
Write a function that takes two int arguments: a value and a bit position.Have the function return 1 if that particular bit position is 1, and have it return 0 otherwise. Test the function in a program.
***
```c
#include <stdio.h>
#include <limits.h>

#define CLEARINPUT while (getchar() != '\n') continue

int checkbit(int value, int position);

int main(void)
{
	// test checkbit()

	int value, position, ch;

	printf("Enter an integer: ");
	while (scanf("%d", &value) == 1)
	{
		printf("Enter a position: ");
		while (scanf("%d", &position) == 1)
		{
			printf("Position %d of %d is %d\n", position, value,
				   checkbit(value, position));
			printf("Enter a position: ");
		}
		CLEARINPUT;
		printf("\nEnter an integer: ");
	}

	puts("Bye.");
	return 0;
}

int checkbit(int value, int position)
{
	// return the state of the bit in the given position of the integer value

	value >>= (position - 1);
	return value & 1;
}
```

## 15-5
Write a function that rotates the bits of an unsigned int by a specified number of bits to the left. For instance, rotate_l(x,4) would move the bits in x four places to the left, and the bits lost from the left end would reappear at the right end. That is, the bit moved out of the high-order position is placed in the low-order position. Test the function in a rogram.
***
```c
#include <stdio.h>
#include <limits.h>

#define CLEARINPUT while (getchar() != '\n') continue

unsigned int rotate_l(unsigned int value, int n);

const unsigned int BITS = sizeof(unsigned int) * CHAR_BIT;
const unsigned int LBITMASK = 1 << (BITS - 1);

int main(void)
{
	// test rotate_l()

	unsigned int value, rotated;
	int n;

	printf("Enter a non-negative integer value: ");
	while (scanf("%u", &value) == 1)
	{
		printf("Enter a number of positions to rotate: ");
		while (scanf("%d", &n) == 1)
		{
			puts("Before rotation:");
			for (int i = 0; i < BITS; i++)
			{
				if ((value << i) & LBITMASK)
					putchar('1');
				else
					putchar('0');
			}
			putchar('\n');
			printf("After rotation by %d positions:\n", n);
			rotated = rotate_l(value, n);
			for (int i = 0; i < BITS; i++)
			{
				if ((rotated << i) & LBITMASK)
					putchar('1');
				else
					putchar('0');
			}
			putchar('\n');
			printf("Enter a number of positions to rotate: ");
		}
		CLEARINPUT;
		printf("\nEnter a non-negative integer value: ");
	}

	puts("Bye.");
	return 0;

}

unsigned int rotate_l(unsigned int value, int n)
{
	// rotate the bits in value n postitions to the left

	int leftbit;

	for (int i = 0; i < n; i++)
	{
		leftbit = value & LBITMASK;
		value <<= 1;
		if (leftbit)
			value |= 1;
	}

	return value;
}
```

## 15-6
Design a bit-field structure that holds the following information:
	  Font ID: A number in the range 0–255
	  Font Size: A number in the range 0–127
	  Alignment: A number in the range 0–2 represented the choices Left,Center, and Right
      Bold: Off (0) or on (1)
	  Italic: Off (0) or on (1)
	  Underline: Off (0) or on (1)
Use this structure in a program that displays the font parameters and uses a looped menu to let the user change parameters. For example, a sample run might look like this:

	   ID SIZE ALIGNMENT  B   I   U 
    1  12   left      off off off
    
    f)change fonts	s)change size 	 a)change alignment
    b)toggle bold 	i)toggle italic  u)toggle underline
	   q)quit
	   s
	   Enter font size (0-127): 36

	   ID SIZE ALIGNMENT  B   I   U 
    1  36   left      off off off

    f)change fonts	s)change size 	 a)change alignment
    b)toggle bold 	i)toggle italic  u)toggle underline
	   q)quit
	   a
	   Select alignment:
	   l)left    c)center   r)right
	   r
	   ...

***
```c
#include <stdio.h>

#define CLEARINPUT while (getchar() != '\n') continue

#define LEFT 0
#define CENTER 1
#define RIGHT 2

#define ALSTR(al) al ? (al == 1 ? "center" : "right") : "left"

#define OFF 0
#define ON 1
#define ONOFF(status) status? "on" : "off"

typedef struct {
	unsigned int id: 			8;
	unsigned int size: 			7;
	unsigned int alignment:		2;
	unsigned int bold:			1;
	unsigned int italic:		1;
	unsigned int underline:		1;
} Font;

void display_font(Font *);
void print_menu(void);
void setfont(Font *);
void setsize(Font *);
void setalignment(Font *);
void togglebold(Font *);
void toggleitalic(Font *);
void toggleunderline(Font *);

int main()
{
	Font font = { 1, 12, 1, 0, 0, 0};

	while(1)
	{
		display_font(&font);
		print_menu();
		int ch = getchar();
		if (ch != '\n')
			CLEARINPUT;

		if (ch == EOF || ch =='q') // exit conditions
			break;
		else if (ch == 'f')
			setfont(&font);
		else if (ch == 's')
			setsize(&font);
		else if (ch == 'a')
			setalignment(&font);
		else if (ch == 'b')
			togglebold(&font);
		else if (ch == 'i')
			toggleitalic(&font);
		else if (ch == 'u')
			toggleunderline(&font);
		else
			puts("Sorry, I don't understand that input. Try again.");
	}

	puts("Bye.");
	return 0;
}

void display_font(Font * font)
{
	puts("");
	puts("ID  SIZE ALIGNMENT  B   I   U ");
	printf("%-3u %-4u %-9s %3s %3s %3s\n", font->id, font->size,
		   ALSTR(font->alignment), ONOFF(font->bold), ONOFF(font->italic),
		   ONOFF(font->underline));
	puts("");
}

void print_menu(void)
{
	puts("f)change fonts	s)change size 	 a)change alignment");
	puts("b)toggle bold 	i)toggle italic  u)toggle underline");
	puts("q)quit");
}

void setfont(Font * font)
{
	unsigned int id;
	printf("Enter font id (0 - 255): ");
	while (scanf("%u", &id) != 1)
	{
		CLEARINPUT;
		puts("Invalid input. Try again.");
		printf("Enter font id (0 - 255): ");
	}
	font->id = id;
	CLEARINPUT;
}

void setsize(Font * font)
{
	unsigned int size;
	printf("Enter font size (0 - 127): ");
	while (scanf("%u", &size) != 1)
	{
		CLEARINPUT;
		puts("Invalid input. Try again.");
		printf("Enter font size (0 - 127): ");
	}
	font->size = size;
	CLEARINPUT;
}

void setalignment(Font * font)
{
	puts("Select alignment:");
	puts("l)left   c)center   r)right");
	int ch;
	while (!((ch = getchar()) == 'l' || ch == 'c' || ch == 'r')) 
	{
		puts("That is not a valid option. Try again.");
		puts("Select alignment:");
		puts("l)left   c)center   r)right");
	}

	if (ch == 'l')
		font->alignment = LEFT;
	else if (ch == 'c')
		font->alignment = CENTER;
	else if (ch == 'r')
		font->alignment = RIGHT;

	CLEARINPUT;
}

void togglebold(Font * font)
{
	font->bold = font->bold ? OFF : ON;
}

void toggleitalic(Font * font)
{
	font->italic = font->italic ? OFF : ON;
}
void toggleunderline(Font * font)
{
	font->underline = font->underline ? OFF : ON;
}
```

## 15-7
Write a program with the same behavior as described in exercise 6, but use an unsigned long variable to hold the font information and use the bitwise operators instead of bit members to manage the information.
***
```c
#include <stdio.h>

#define CLEARINPUT while (getchar() != '\n') continue

#define LEFT 0
#define CENTER 1
#define RIGHT 2

#define OFF 0
#define ON 1
#define ONOFF(status) status? "on" : "off"

#define ID_SHIFT 0
#define SIZE_SHIFT 8
#define ALIGN_SHIFT 15
#define BOLD_SHIFT 18
#define ITAL_SHIFT 19
#define UNDERL_SHIFT 20

#define ID_MSK (255U << ID_SHIFT)
#define SIZE_MSK (127U << SIZE_SHIFT)
#define ALIGN_MSK (3U << ALIGN_SHIFT)
#define BOLD_MSK (1U << BOLD_SHIFT)
#define ITAL_MSK (1U << ITAL_SHIFT)
#define UNDERL_MSK (1U << UNDERL_SHIFT)

#define getid(font) ((font & ID_MSK) >> ID_SHIFT)
#define getsize(font) ((font & SIZE_MSK) >> SIZE_SHIFT)
#define getalign(font) ((font & ALIGN_MSK) >> ALIGN_SHIFT)
#define getalignstr(font) (getalign(font) ? (getalign(font) == CENTER ? "center" : "right") : "left")
#define getbold(font) ((font & BOLD_MSK) >> BOLD_SHIFT)
#define getital(font) ((font & ITAL_MSK) >> ITAL_SHIFT)
#define getunderline(font) ((font & UNDERL_MSK) >> UNDERL_SHIFT)
#define setid_(font, id) (font &= ~ID_MSK, font |= ((id << ID_SHIFT) & ID_MSK))
#define setsize_(font, size) (font &= ~SIZE_MSK, font |= ((size << SIZE_SHIFT) & SIZE_MSK))
#define setalign_(font, align) (font &= ~ALIGN_MSK, font |= ((align << ALIGN_SHIFT) & ALIGN_MSK))
#define togglebold(font) (font ^= BOLD_MSK)
#define toggleital(font) (font ^= ITAL_MSK)
#define toggleunderline(font) (font ^= UNDERL_MSK)


typedef unsigned int Font;

void display_font(Font);
void print_menu(void);
void setfont(Font *);
void setsize(Font *);
void setalignment(Font *);

int main()
{
	Font font =  0;
	setid_(font, 1);
	setsize_(font, 12);
	setalign_(font, 1);

	while(1)
	{
		display_font(font);
		print_menu();
		int ch = getchar();
		if (ch != '\n')
			CLEARINPUT;

		if (ch == EOF || ch =='q') // exit conditions
			break;
		else if (ch == 'f')
			setfont(&font);
		else if (ch == 's')
			setsize(&font);
		else if (ch == 'a')
			setalignment(&font);
		else if (ch == 'b')
			togglebold(font);
		else if (ch == 'i')
			toggleital(font);
		else if (ch == 'u')
			toggleunderline(font);
		else
			puts("Sorry, I don't understand that input. Try again.");
	}

	puts("Bye.");
	return 0;
}

void display_font(Font font)
{
	puts("");
	puts("ID  SIZE ALIGNMENT  B   I   U ");
	printf("%-3u %-4u %-9s %3s %3s %3s\n", getid(font), getsize(font),
		   getalignstr(font), ONOFF(getbold(font)), ONOFF(getital(font)),
		   ONOFF(getunderline(font)));
	puts("");
}

void print_menu(void)
{
	puts("f)change fonts	s)change size 	 a)change alignment");
	puts("b)toggle bold 	i)toggle italic  u)toggle underline");
	puts("q)quit");
}

void setfont(Font * font)
{
	unsigned int id;
	printf("Enter font id (0 - 255): ");
	while (scanf("%u", &id) != 1)
	{
		CLEARINPUT;
		puts("Invalid input. Try again.");
		printf("Enter font id (0 - 255): ");
	}
	setid_(*font, id);
	CLEARINPUT;
}

void setsize(Font * font)
{
	unsigned int size;
	printf("Enter font size (0 - 127): ");
	while (scanf("%u", &size) != 1)
	{
		CLEARINPUT;
		puts("Invalid input. Try again.");
		printf("Enter font size (0 - 127): ");
	}
	setsize_(*font, size);
	CLEARINPUT;
}

void setalignment(Font * font)
{
	puts("Select alignment:");
	puts("l)left   c)center   r)right");
	int ch;
	while (!((ch = getchar()) == 'l' || ch == 'c' || ch == 'r')) 
	{
		puts("That is not a valid option. Try again.");
		puts("Select alignment:");
		puts("l)left   c)center   r)right");
	}

	if (ch == 'l')
		setalign_(*font, LEFT);
	else if (ch == 'c')
		setalign_(*font, CENTER);
	else if (ch == 'r')
		setalign_(*font, RIGHT);

	CLEARINPUT;
}
```

