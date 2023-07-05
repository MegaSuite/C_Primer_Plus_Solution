# Chapter 3 Exs

## 3-1
Find out what your system does with integer overflow, floating-point overflow,and floating point underflow by using the experimental approach; that is,write programs that have these problems.
***
```c
#include <stdio.h>
#include <limits.h>
#include <float.h>

int main(void)
{
	int int_overflow;
	int MAX_INTEGER = INT_MAX;
	float flt_overflow, flt_underflow;
	float MIN_FLOAT = FLT_MIN;
	float MAX_FLOAT = FLT_MAX;
	
	// artificially create over/underflow
	int_overflow = INT_MAX + 1;
	flt_overflow = FLT_MAX * 2.;
	flt_underflow = FLT_MIN / 2.;
	
	// print results
	printf("Max integer: %d \tMax integer + 1: %d\n", INT_MAX, int_overflow);
	printf("Max float: %f \tMax float * 2: %f\n", FLT_MAX, flt_overflow);
	printf("Min float: %f \tMin float / 2: %f\n", FLT_MIN, flt_underflow);

	return 0;
}
```
## 3-2
Write a program that asks you to enter an ASCII code value, such as 66, and then prints the character having that ASCII code.
***
```c
#include <stdio.h>
int main(void) 
{
	int ascii_code;
	printf("Enter an ASCII code: ");
	scanf("%d", &ascii_code);
	printf("Character for ASCII code %d: %c\n", ascii_code, ascii_code);

	return 0;
}
```
## 3-3
Write a program that sounds an alert and then prints the following text: Startled by the sudden sound, Sally shouted,"By the Great Pumpkin, what was that!"
***
```c
#include <stdio.h>
int main(void)

{
	printf("\a"); // sound alert
	printf("Startled by the sudden sound, Sally shouted, \"By the Great Pumpkin, what was that!\"\n");

	return 0;
}
```
## 3-4
Write a program that reads in a floating-point number and prints it first in decimal-point notation,then in exponential notation, and then, if your system supports it, p notation. Have the output use the following format (the actual number of digits displayed for the exponent depends on the system):

Enter a floating-point value: 64.25
fixed-point notation: 64.250000
exponential notation: 6.425000e+01
p notation: 0x1.01p+6 
***
```c
#include <stdio.h>
int main(void) 
{
	float flt_input;

	printf("Enter a floating-point value: ");
	scanf("%f", &flt_input);
	printf("Fixed-point notation: %f\n", flt_input);
	printf("Exponential notation: %e\n", flt_input);
	printf("P notation: %a\n", flt_input);

	return 0;
}
```
## 3-5
There are approximately 3.156 × 10^7 seconds in a year. Write a program that requests your age in years and then displays the equivalent number of seconds.
***
```c
#include <stdio.h>

int main(void)
{
	unsigned int SECONDS_PER_YEAR = 31560000;
	unsigned int age;

	printf("What is your age (in years)?: ");
	scanf("%u", &age);
	printf("You are %u seconds old!\n", SECONDS_PER_YEAR * age);

	return 0;
}
```
## 3-6
The mass of a single molecule of water is about 3.0×10^-23 grams. A quart of water is about 950 grams. Write a program that requests an amount of water, in quarts, and displays the number of water molecules in that amount.
***
```c
#include <stdio.h>

int main(void)
{
	float H20_MASS = 3.0e-23;
	float GRAMS_H20_PER_QUART = 950.;
	float quarts;

	printf("Enter an amount of water (in quarts): ");
	scanf("%f", &quarts);
	printf("There are %f molecules in %f quarts of water.\n", quarts * GRAMS_H20_PER_QUART / H20_MASS, quarts);

	return 0;
}

```
## 3-7
There are 2.54 centimeters to the inch. Write a program that asks you to enter your height in inches and then displays your height in centimeters. Or, if you prefer, ask for the height in centimeters and convert that to inches.
***
```c
#include <stdio.h>

int main(void)
{
	float CM_PER_INCH = 2.54;
	float height;

	printf("Enter your height (in inches): ");
	scanf("%f", &height);
	printf("You are %f centimeters tall.\n", height * CM_PER_INCH);

	return 0;
}
```
## 3-8
In the U.S. system of volume measurements, a pint is 2 cups, a cup is 8 ounces, an ounce is 2 tablespoons, and a tablespoon is 3 teaspoons. Write a program that requests a volume in cups and that displays the equivalent volumes in pints, ounces, tablespoons, and teaspoons. Why does a floating-point type make more sense for this application than an integer type? 
***
```c
#include <stdio.h>

int main(void)
{

	/* If the number of cups is not an even whole number, then the number of
		pints will not be a whole number. */
	float PINTS_PER_CUP = .5;
	float OUNCES_PER_CUP = 8;
	float TBS_PER_CUP = 2 * OUNCES_PER_CUP; // tablespoons/ounce * ounces/cup
	float TSP_PER_CUP = 3 * TBS_PER_CUP; // teaspoons/tablespoon * tablespoons/ounce * ounces/cup
	float cups;

	printf("Enter an amount in cups:");
	scanf("%f", &cups);
	printf("%f cups is equivalent to:\n", cups);
	printf("%f pints\n", cups * PINTS_PER_CUP);
	printf("%f ounces\n", cups * OUNCES_PER_CUP);
	printf("%f tablespoons\n", cups * TBS_PER_CUP);
	printf("%f teaspoons\n", cups * TSP_PER_CUP);

	return 0;
}
```
