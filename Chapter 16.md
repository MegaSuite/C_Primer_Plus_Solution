# Chapter 16
## 16-coordinates
***
```c
#include "coordinates.h"
#include <math.h>

Cartesian cartesianFromPolar(Polar coords)
{
	Cartesian retval;

	retval.x = coords.magnitude * cos(coords.angle);
	retval.y = coords.magnitude * sin(coords.angle);

	return retval;
}
```

## 16-coordinates.h
```c
#ifndef COORDINATES_H_
#define COORDINATES_H_

typedef struct polar 
{
	double angle;
	double magnitude;
} Polar;

typedef struct cartesian
{
	double x;
	double y;
} Cartesian;


// 	Cartesian cartesianFromPolar(Polar):
// 	converts polar coordinates to cartesian

Cartesian cartesianFromPolar(Polar coords); 

#endif
```

## 16-1
```c
#ifndef EXERCISE01_H_
#define EXERCISE01_H_

#include <stdio.h>

#define CLEARINPUT while (getchar() != '\n') continue

#endif
```

## 16-2
The harmonic mean of two numbers is obtained by taking the inverses of the two numbers, averaging them, and taking the inverse of the result. Use a #define directive to define a macro “function” that performs this operation. Write a simple program that tests the macro.
***
```c
#include <stdio.h>
#include "exercise01.h"

#define INVERSE(X) (1. / (X))
#define HARM_MEAN(X, Y) (1. / ((INVERSE(X) + INVERSE(Y)) / 2))

int main()
{
	// test HARM_MEAN

	double x, y;
	printf("Enter two numbers: ");
	while (scanf("%lf %lf", &x, &y) == 2)
	{
		CLEARINPUT;
		printf("The harmonic mean of %.3f and %.3f is: %.3f.\n", x, y,
			   HARM_MEAN(x, y));

		printf("Enter two numbers: ");
	}
	puts("Bye.");
}
```

## 16-3
Polar coordinates describe a vector in terms of magnitude and the counterclockwise angle from the x-axis to the vector. Rectangular coordinates describe the same vector in terms of x and y components (see Figure 16.3). Write a program that reads the magnitude and angle (in
degrees) of a vector and then displays the x and y components. The relevant equations are these:

	x = r cos A y = r sin A

To do the conversion, use a function that takes a structure containing the polar coordinates and returns a structure containing the rectangular coordinates (or use pointers to such structures, if you prefer).

compile with coordinates.c

***
```c
#include <stdio.h>
#include "exercise01.h"
#include "coordinates.h"

int main()
{
	Polar polar_coords;
	Cartesian cartesian_coords;

	printf("Enter an angle: ");
	while (scanf("%lf", &polar_coords.angle) == 1)
	{
		CLEARINPUT;
		printf("Enter a magnitude: ");
		while (scanf("%lf", &polar_coords.magnitude) != 1)
		{
			CLEARINPUT;
			printf("Enter a magnitude: ");
		}

		cartesian_coords = cartesianFromPolar(polar_coords);

		printf("angle: %.2f  magnitude: %.2f\n", polar_coords.angle,
			   polar_coords.magnitude);
		printf("x: %.2f  y: %.2f\n", cartesian_coords.x, cartesian_coords.y);

		printf("Enter an angle: ");
	}
	puts("Bye.");
}
```


## 16-4
The ANSI library features a clock() function with this description:
	 #include <time.h> clock_t clock (void);
Here, clock_t is a type defined in time.h. The function returns the processor time, which is given in some implementation-dependent units. (If
the processor time is unavailable or cannot be represented, the function returns a value of -1.) However, CLOCKS_PER_SEC, also defined in time.h, is the number of processor time units per second. Therefore, dividing the difference between two return values of clock() by CLOCKS_PER_SEC gives you the number of seconds elapsed between the two calls. Typecasting the values to double before division enables you to get fractions of a second. Write a function that takes a double argument representing a desired time delay and then runs a loop until that amount of time has passed. Write a simple program that tests the function.
***
```c
#include <stdio.h>
#include <time.h>

void timeout(double time);

int main(void)
{
	double time;

	printf("Enter desired time delay in seconds: ");
	while(scanf("%lf", &time) == 1)
	{
		puts("Starting.");
		timeout(time);
		printf("It is now %2f seconds later!\n", time);
		printf("Enter desired time delay in seconds: ");
	}
	puts("Bye.");
}

void timeout(double time)
{
	clock_t start, end;

	start = clock();

	while (((end = clock()) - start) /  (double) CLOCKS_PER_SEC < time)
		continue;

	return;
}
```

## 16-5
Write a function that takes as arguments the name of an array of type int elements, the size of an array, and a value representing the number of picks. The function then should select the indicated number of items at random from the array and prints them. No array element is to be picked more than once. (This simulates picking lottery numbers or jury members.) Also, if your implementation has time() (discussed in Chapter 12) or a similar function available, use its output with srand() to initialize the rand() random- number generator. Write a simple program that tests the function.
***
```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>
#include <stdbool.h>

#define ARR_SIZE 100

void sample(int * arr, int arr_size, int n);

int main(void)
{
	// test sample()

	int array[ARR_SIZE];
	for (int i = 0; i < ARR_SIZE; i++)
		array[i] = i;

	int n;
	printf("How many items would you like to sample: ");
	while(scanf("%d", &n) == 1 && n > 0)
	{
		sample(array, ARR_SIZE, n);
		printf("How many items would you like to sample: ");
	}

	puts("Bye.");
}

void sample(int * arr, int arr_size, int n)
{
	// pick and print a sample of n items from arr at random

	if (n > arr_size)
	{
		printf("Error: sample size can not be larger than the size of the array being sampled.\n");
		return;
	} 
	
	bool chosen[arr_size];
	for (int i = 0; i < arr_size; i++)
		chosen[i] = false;
	
	srand(time(NULL));
	for (int i = 0; i < n; i++)
	{
		int index = rand() % arr_size;
		while (chosen[index]) // if index has already been chosen, pick again
			index = rand() % arr_size;
		printf("%d\n", arr[index]);
		chosen[index] = true;
	}
	return;
}
```


## 16-6
Modify Listing 16.17 so that it uses an array of struct names elements (as defined after the listing) instead of an array of double. Use fewer
elements, and initialize the array explicitly to a suitable selection of names.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define NUM 15

typedef struct name {
	char first[40];
	char last[40];
} NAME;

int comparenames(const void * p1, const void * p2);
void printnames(const NAME * arr, int arr_size);

int main(void)
{
	NAME insideout[NUM] = {
		{"Amy", "Poehler"},
		{"Phyllis", "Smith"},
		{"Richard", "Kind"},
		{"Bill", "Hader"},
		{"Lewis", "Black"},
		{"Mindy", "Kaling"},
		{"Kaitlyn", "Dias"},
		{"Diane", "Lane"},
		{"Kyle", "MacLachlan"},
		{"Paula", "Poundstone"},
		{"Bobby", "Moynihan"},
		{"Paula", "Pell"},
		{"Dave", "Goelz"},
		{"Frank", "Oz"},
		{"Josh", "Cooley"},
	};

	puts("Here is the list of names:");
	printnames(insideout, NUM);
	puts("");

	qsort(insideout, NUM, sizeof(NAME), comparenames);

	puts("Here is the sorted list of names:");
	printnames(insideout, NUM);
}

int comparenames(const void * p1, const void * p2)
{
	NAME * name1 = (NAME *) p1;
	NAME * name2 = (NAME *) p2;

	int c = strcmp(name1->last, name2->last);
	return c ? c : strcmp(name1->first, name2->first);
}

void printnames(const NAME * arr, int arr_size)
{
	for (int i = 0; i < arr_size; i++)
		printf("%s, %s\n", arr[i].last, arr[i].first);
}
```

## 16-7
Here’s a partial program using a variadic function:

	#include <stdio.h>
	#include <stdlib.h>
	#include <stdarg.h>

	void show_array(const double ar[], int n);
	double * new_d_array(int n, ...);

	int main() {
		double * p1;
		double * p2;
		p1 = new_d_array(5, 1.2, 2.3, 3.4, 4.5, 5.6);
		p2 = new_d_array(4, 100.0, 20.00, 8.08, -1890.0);
		show_array(p1, 5);
		show_array(p2, 4);
		free(p1);
		free(p2);
		return 0;
	}

The new_d_array() function takes an int argument and a variable number of double arguments. The function returns a pointer to a block of memory allocated by malloc(). The int argument indicates the number of elements to be in the dynamic array, and the double values are used to initialize the elements, with the first value being assigned to the first element, and so on. Complete the program by providing the code for show_ array() and new_d_array().
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdarg.h>

void show_array(const double ar[], int n);
double * new_d_array(int n, ...);

int main()
{
	double * p1;
	double * p2;
	p1 = new_d_array(5, 1.2, 2.3, 3.4, 4.5, 5.6);
	p2 = new_d_array(4, 100.0, 20.00, 8.08, -1890.0); 
	show_array(p1, 5);
	show_array(p2, 4);
	free(p1);
	free(p2);
	return 0;
}

void show_array(const double ar[], int n)
{
	printf("{");
	for (int i = 0; i < n; i++)
	{
		printf("%.3f", ar[i]);
		if (i < n - 1)
			printf(", ");
		if (i % 20 == 19)
			printf("\n");
	}
	puts("}");
}

double * new_d_array(int n, ...)
{
	va_list ap;
	va_start(ap, n);

	double * arr = (double *) malloc(n * sizeof(double));
	if (!arr)
		fprintf(stderr, "Could not allocate memory.\n");
	else
		for (int i = 0; i < n; i++)
			arr[i] = va_arg(ap, double);

	return arr;
}
```

