# Chapter 10

## 10-1
Modify the rain program in Listing 10.7 so that it does the calculations using pointers instead of subscripts. (You still have to declare and
initialize the array.)
***
```c
#include <stdio.h>
#define MONTHS 12 // number of months in a year 
#define YEARS 5 // number of years of data 

int main(void)
{
	// initializing rainfall data for 2010 - 2014
	const float rain[YEARS][MONTHS] = {
		{4.3,4.3,4.3,3.0,2.0,1.2,0.2,0.2,0.4,2.4,3.5,6.6}, 
		{8.5,8.2,1.2,1.6,2.4,0.0,5.2,0.9,0.3,0.9,1.4,7.3}, 
		{9.1,8.5,6.7,4.3,2.1,0.8,0.2,0.2,1.1,2.3,6.1,8.4}, 
		{7.2,9.9,8.4,3.3,1.2,0.8,0.4,0.0,0.6,1.7,4.3,6.2}, 
		{7.6,5.6,3.8,2.8,3.8,0.2,0.0,0.0,0.0,1.3,2.6,5.2}
	};


	float subtot, total;

	// declare pointers
	const float (*year_ptr)[MONTHS];
	const float *month_ptr;

	printf(" YEAR RAINFALL (inches)\n");
	for (year_ptr = rain, total = 0; year_ptr < rain + YEARS; year_ptr++)
	{
		// for each year, sum rainfall for each month
		for (month_ptr = *year_ptr, subtot = 0; month_ptr < *year_ptr + MONTHS; month_ptr++)
			subtot += *month_ptr;

		printf("%5d %15.1f\n", (int) (2010 + (year_ptr - rain)), subtot);
		total += subtot; // total for all years }
	}

	printf("\nThe yearly average is %.1f inches.\n\n", total/YEARS);
	
	printf("MONTHLY AVERAGES:\n\n");
	printf(" Jan Feb Mar Apr May Jun Jul Aug Sep Oct ");
	printf("Nov Dec\n");

	int month, year;

	for (month = 0; month < MONTHS; month++)
	{ 
		// for each month, sum rainfall over years
		for (year = 0, subtot = 0; year < YEARS; year++)
			subtot += *(*(rain + year) + month);
		printf("%4.1f", subtot/YEARS);
	}

	printf("\n");
	return 0; 
}
```

## 10-2
Write a program that initializes an array-of-double and then copies the contents of the array into three other arrays. (All four arrays should be declared in the main program.) To make the first copy, use a function with array notation. To make the second copy, use a function with pointer notation and pointer incrementing. Have the first two functions take as arguments the name of the target array, the name of the source array, and the number of elements to be copied. Have the third function take as arguments the name of the target, the name of the source, and a pointer to the element following the last element of the source.
***
```c
#include <stdio.h>
#define LENGTH 5

// prototype declarations
void copy_arr(double *target, double *source, int arr_len); // copy with array notation
void copy_ptr(double *target, double *source, int arr_len); // copy with pointer notation
void copy_ptrs(double *target, double *source_start, double *source_end); // copy with two pointers

int main(void)
{
	double source[LENGTH] = {1.1, 2.2, 3.3, 4.4, 5.5};
	double target1[LENGTH];
	double target2[LENGTH];
	double target3[LENGTH];

	// copy arrays
	copy_arr(target1, source, LENGTH);
	copy_ptr(target2, source, LENGTH);
	copy_ptrs(target3, source, source + LENGTH);

	// print array contents
	printf("%15s|%15s|%15s\n", "target1", "target2", "target3");
	for (int i = 0; i < LENGTH; i++)
		printf("%15.3f|%15.3f|%15.3f\n", target1[i], target2[i], target3[i]);

	return 0;
}

void copy_arr(double *target, double *source, int arr_len)
{
	// copy array using array notation

	for (int i = 0; i < arr_len; i++)
	{
		target[i] = source[i];
	}
}

void copy_ptr(double *target, double *source, int arr_len)
{
	// copy array using pointer notation

	for (int i = 0; i < arr_len; i++)
	{
		*(target + i) = *(source + i);
	}
}

void copy_ptrs(double *target, double *source_start, double *source_end)
{
	// copy arr using pointer notation and pointer endpoint

	for (double *ptr = source_start; ptr < source_end; ptr++, target++)
		*target = *ptr;
}
```

## 10-3
Write a function that returns the largest value stored in an array-of-int.
Test the function in a simple program.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define SIZE 10

int max_int(int *arr, int arr_size); // function prototype

int main(void)
{
	int test[SIZE];

	srand(time(NULL)); // seed random number generator
	
	// initialize test array with random ints between 0 and 99
	for (int i = 0; i < SIZE; i++)
		test[i] = rand() % 100;

	printf("The maximum of ");
	for (int i = 0; i < SIZE; i++)
		printf("%d ", test[i]);
	printf("is: %d\n", max_int(test, SIZE));

	return 0;
}

int max_int(int *arr, int arr_size)
{
	int max = arr[0];
	for (int i = 1; i < arr_size; i++)
	{
		if (arr[i] > max)
			max = arr[i];
	}

	return max;
}
```

## 10-4
Write a function that returns the index of the largest value stored in an array-of-double.
Test the function in a simple program.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SIZE 10

int index_of_max(double *arr, int arr_size);

int main(void)
{
	// test index_of_max

	printf("Driver for index_of_max: returns index of the largest value stored "
		   "in an array of doubles.\n");
	putchar('\n');

	double test[SIZE];

	srand(time(NULL)); // seed random number generator

	// initialize test array with random doubles
	for (int i = 0; i < SIZE; i++)
		test[i] = rand() / (double) RAND_MAX;

	// print test array

	printf("%5s ", "Index");
	for (int i = 0; i < SIZE; i++)
		printf("| %6d ", i);
	printf("\n");
	printf("%5s ", "Value");
	for (int i = 0; i < SIZE; i++)
		printf("| %6.4f ", test[i]);
	printf("\n");
	printf("\n");

	// print results 
	printf("The maximum value occurs at index %d\n", index_of_max(test, SIZE));

	return 0;
}

int index_of_max(double *arr, int arr_size)
{
	// return index of max value in array of doubles

	int index_of_max = 0;
	for (int i = 1; i < arr_size; i++)
		if (*(arr + i) > *(arr + index_of_max))
			index_of_max = i;

	return index_of_max;
}
```

## 10-5
Write a function that returns the difference between the largest and smallest elements of an array-of-double. 
Test the function in a simple
program
***
```c
#include <stdio.h>

double max_difference(double *arr, int arr_size);

int main(void)
{
	// test max difference

	double test[5] = {1.0, 3.4, 2.2, 0.9, 2.0};

	// print test results
	printf("The difference between the largest and smallest values of ");
	for (int i = 0; i < 5; i++)
		printf("%.1f ", test[i]);
	printf("is %.1f\n", max_difference(test, 5));

	return 0;
}

double max_difference(double *arr, int arr_size)
{
	// return the difference between the largest and smallest elements
	// of an array-of-double

	double max = arr[0];
	double min = arr[0];

	for (int i = 0; i < arr_size; i++)
	{
		if (arr[i] > max)
			max = arr[i];
		else if (arr[i] < min)
			min = arr[i];
	}

	return max - min;
}
```

## 10-6
Write a function that reverses the contents of an array of double and test it in a simple program.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>


void reverse_array(double *arr, int arr_size);

int main(void)
{
	// test reverse_array()

	printf("Testing reverse_array()\n");

	double test1[9];
	double test2[10];
	double test3[2];

	srand(time(NULL));

	// initialize test array 1 with 9 random doubles
	for (int i = 0; i < 9; i++)
		test1[i] = rand() / (double) RAND_MAX;

	// initialize test array 2 with 10 random doubles
	for (int i = 0; i < 10; i++)
		test2[i] = rand() / (double) RAND_MAX;

	// initialize test array 1 with 2 random doubles
	for (int i = 0; i < 2; i++)
		test3[i] = rand() / (double) RAND_MAX;

	// test array 1

	printf("First Test\n");
	// print original array
	printf("%10s: ", "Original");
	for (int i = 0; i < 9; i++)
		printf("%5.2f ", test1[i]);
	putchar('\n');

	//print reversed array
	reverse_array(test1, 9);
	printf("%10s: ", "Reversed");
	for (int i = 0; i < 9; i++)
		printf("%5.2f ", test1[i]);
	putchar('\n');

	// test array 2

	printf("Second Test\n");
	// print original array
	printf("%10s: ", "Original");
	for (int i = 0; i < 10; i++)
		printf("%5.2f ", test2[i]);
	putchar('\n');

	//print reversed array
	reverse_array(test2, 10);
	printf("%10s: ", "Reversed");
	for (int i = 0; i < 10; i++)
		printf("%5.2f ", test2[i]);
	putchar('\n');

	// test array 3

	printf("Third Test\n");
	// print original array
	printf("%10s: ", "Original");
	for (int i = 0; i < 2; i++)
		printf("%5.2f ", test3[i]);
	putchar('\n');

	//print reversed array
	reverse_array(test3, 2);
	printf("%10s: ", "Reversed");
	for (int i = 0; i < 2; i++)
		printf("%5.2f ", test3[i]);
	putchar('\n');

	return 0;
}

void reverse_array(double *arr, int arr_size)
{
	// reverse an array of double

	double tmp;

	for (int i = 0; i < arr_size / 2; i++)
	{
		// swap values between indexes i and (arr_size - 1 - i)
		tmp = arr[i];
		arr[i] = arr[arr_size - 1 - i];
		arr[arr_size - 1 - i] = tmp;
	}
}
```

## 10-7
Write a program that initializes a two-dimensional array-of-double and uses one of the copy functions from exercise 2 to copy it to a second two dimensional array. (Because a two-dimensional array is an array of arrays, a one-dimensional copy function can be used with each subarray.)
***
```c
#include <stdio.h>
#include <stdlib.h>

void copy_ptr(double *target, double *source, int arr_len);
void print_row(double (*arr)[10], int row);

int main(void)
{
	// copy one 10 by 10 array to another 10 by 10 array

	double source[10][10];
	double target[10][10];

	// initialize source array
	for (int i = 0; i < 10; i++)
		for (int j = 0; j < 10; j++)
			source[i][j] = rand() / (double) RAND_MAX;

	// copy source array to target
	for (int i = 0; i < 10; i++)
		copy_ptr(target[i], source[i], 10);

	// print arrays
	printf("%-50s", "Source");
	printf("   ");
	printf("%-50s", "Target");
	putchar('\n');
	for (int i = 0; i < 103; i++)
		putchar('-');
	putchar('\n');
	for (int i = 0; i < 10; i++)
	{
		print_row(source, i);
		printf("   ");
		print_row(target, i);
		putchar('\n');
	}

	return 0;
}

void copy_ptr(double *target, double *source, int arr_len)
{
	// copy array using pointer notation

	for (int i = 0; i < arr_len; i++)
	{
		*(target + i) = *(source + i);
	}
}

void print_row(double (*arr)[10], int row)
{
	// print a row from a n by 10 array of doubles

	for (int i = 0; i < 10; i++)
	{
		printf("%.2f ", arr[row][i]);
	}
}
```

## 10-8
Use a copy function from Programming Exercise 2 to copy the third through fifth elements of a seven-element array into a three-element array. The function itself need not be altered; just choose the right actual arguments. (The actual arguments need not be an array name and array size. They only have to be the address of an array element and a number of elements to be processed.)
***
```c
#include <stdio.h>

void copy_ptrs(double *target, double *source_start, double *source_end);

int main(void)
{
	double source[7] = {2.4, 5.9, 7.8, 1.5, 3.3, 5.3, 6.8};
	double target[3];

	copy_ptrs(target, source + 2, source + 5);

	// print arrays
	for (int i = 0; i < 7; i++)
		printf("%.1f ", source[i]);
	putchar('\n');

	for (int i = 0; i < 3; i++)
		printf("%.1f ", target[i]);
	putchar('\n');

	return 0;
}

void copy_ptrs(double *target, double *source_start, double *source_end)
{
	// copy arr using pointer notation and pointer endpoint

	for (double *ptr = source_start; ptr < source_end; ptr++, target++)
		*target = *ptr;
}
```

## 10-9
Write a program that initializes a two-dimensional 3×5 array-of-double and uses a VLA- based function to copy it to a second two-dimensional array. Also provide a VLA-based function to display the contents of the two arrays. The two functions should be capable, in general, of processing arbitrary N×M arrays. (If you don’t have access to a VLA-capable compiler, use the traditional C approach of functions that can process an N×5 array).
***
```c
#include <stdio.h>
#define ROWS 3
#define COLUMNS 5

void copy_2dim_arr(int rows, int cols, double source[rows][cols],
	               double target[rows][cols]);
void print_2dim_arr(int rows, int cols, double arr[rows][cols]);

int main(void)
{
	double array1[ROWS][COLUMNS] = {{4.3, 5.7, 2.1, 6.6, .8},
						            {5.6, 23.5, 73.2, 12.3, 123},
						            {22.1, 35.3, 6.35, 0.132, 11.1}};
	double array2[ROWS][COLUMNS];

	// copy array1 to array2
	copy_2dim_arr(ROWS, COLUMNS, array1, array2);

	// print contents of arrays
	printf("Array 1:\n");
	print_2dim_arr(ROWS, COLUMNS, array1);
	putchar('\n');

	printf("Array2:\n");
	print_2dim_arr(ROWS, COLUMNS, array2);

	return 0;
}

void copy_2dim_arr(int rows, int cols, double source[rows][cols],
	               double target[rows][cols])
{
	// copy one two-dimensional array to another

	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			target[i][j] = source[i][j];
		}
	}
}

void print_2dim_arr(int rows, int cols, double arr[rows][cols])
{
	// print the contents of a two-dimensional array

	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
			printf(" %10.3f ", arr[i][j]);

		putchar('\n');
	}
}
```

## 10-10
Write a function that sets each element in an array to the sum of the corresponding elements in two other arrays. That is, if array 1 has the
values 2, 4, 5, and 8 and array 2 has the values 1, 0, 4, and 6, the function assigns array 3 the values 3, 4, 9, and 14. The function should
take three array names and an array size as arguments. Test the function in a simple program.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SIZE 10

void add_arrays(int *addend1, int *addend2, int *sum, int array_length);

int main(void)
{
	// test add_arrays

	srand(time(NULL));

	int array1[SIZE];
	int array2[SIZE];
	int sum[SIZE];

	// initialize arrays with random ints
	for (int i = 0; i < SIZE; i++)
	{
		array1[i] = rand() % 20;
		array2[i] = rand() % 20;
	}

	// get sum of arrays
	add_arrays(array1, array2, sum, SIZE);

	// print arrays
	printf("%8s %8s %8s\n", "Array 1", "Array 2", "Sum");
	for (int i = 0; i < SIZE; i++)
		printf("%8d %8d %8d\n", array1[i], array2[i], sum[i]);

	return 0;
}

void add_arrays(int *addend1, int *addend2, int *sum, int array_length)
{
	// calculate elementwise sum of two arrays

	for (int *tar = sum; tar < sum + array_length; tar++, addend1++, addend2++)
		*tar = *addend1 + *addend2;
}
```

## 10-11
Write a program that declares a 3×5 array of int and initializes it to some values of your choice. Have the program print the values, double all the values, and then display the new values. Write a function to do the displaying and a second function to do the doubling. Have the functions take the array name and the number of rows as arguments.
***
```c
#include <stdio.h>

void print_Nx5_int_array(int (*array)[5], int rows);
void double_Nx5_int_array(int (*array)[5], int rows);

int main(void)
{
	int array[3][5] = {{ 2,  5,  6,  9,  1},
					   {23,  1,  5, 66, 54},
					   { 9, 73, 23, 39,  2}};

	printf("Original array:\n");
	print_Nx5_int_array(array, 3);

	// double array and print new values
	double_Nx5_int_array(array, 3);
	printf("Doubled array:\n");
	print_Nx5_int_array(array, 3);

	return 0;
}

void print_Nx5_int_array(int (*array)[5], int rows)
{
	// print the contents of an N x 5 array of ints

	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < 5; ++j)
		{
			printf(" %5d ", array[i][j]);
		}
		putchar('\n');
	}
}

void double_Nx5_int_array(int (*array)[5], int rows)
{
	// double the elements of an N x 5 array of ints

	for (int i = 0; i < rows; ++i)
		for (int j = 0; j < 5; j++)
			array[i][j] *= 2;
}
```

## 10-12
Rewrite the rain program in Listing 10.7 so that the main tasks are performed by functions instead of in main().
***
```c
#include <stdio.h>
#define MONTHS 12 // number of months in a year 
#define YEARS 5 // number of years of data

void print_annual_average(int years, int months, const float data[years][months]);
void print_monthly_averages(int years, int months, const float data[years][months]);

int main(void)
{
	// initializing rainfall data for 2010 - 2014
	const float rain[YEARS][MONTHS] = {{4.3,4.3,4.3,3.0,2.0,1.2,0.2,0.2,0.4,2.4,3.5,6.6},
									   {8.5,8.2,1.2,1.6,2.4,0.0,5.2,0.9,0.3,0.9,1.4,7.3},
									   {9.1,8.5,6.7,4.3,2.1,0.8,0.2,0.2,1.1,2.3,6.1,8.4},
									   {7.2,9.9,8.4,3.3,1.2,0.8,0.4,0.0,0.6,1.7,4.3,6.2},
									   {7.6,5.6,3.8,2.8,3.8,0.2,0.0,0.0,0.0,1.3,2.6,5.2}};


	print_annual_average(YEARS, MONTHS, rain);
	print_monthly_averages(YEARS, MONTHS, rain);

	return 0;
}

void print_annual_average(int years, int months, const float data[years][months])
{
	float subtotal, total;

	printf(" YEAR RAINFALL (inches)\n");

	for (int year = 0, total = 0; year < years; year++)
	{
		// for each year, sum rainfall for each month
		for (int month = 0, subtotal = 0; month < months; month++)
			subtotal += data[year][month];

		printf("%5d %15.1f\n", 2010 + year, subtotal);
		total += subtotal; // total for all years
	}
	printf("\nThe yearly average is %.1f inches.\n\n", total/years);	
}

void print_monthly_averages(int years, int months, const float data[years][months])
{
	float subtotal;

	printf("MONTHLY AVERAGES:\n\n");
	printf(" Jan Feb Mar Apr May Jun Jul Aug Sep Oct "); printf(" Nov Dec\n");

	for (int month = 0; month < months; month++)
	{ 
		// for each month, sum rainfall over years
		for (int year = 0, subtotal = 0; year < years; year++)
			subtotal += data[year][month];

		printf("%4.1f ", subtotal/YEARS);
	}
	printf("\n");
}
```

## 10-13
Write a program that prompts the user to enter three sets of five double numbers each. (You may assume the user responds correctly and doesn’t enter non-numeric data.) The program should accomplish all of the following:
a. Store the information in a 3×5 array.
b. Compute the average of each set of five values.
c. Compute the average of all the values.
d. Determine the largest value of the 15 values.
e. Report the results.
Each major task should be handled by a separate function using the traditional C approach to handling arrays. Accomplish task “b” by using a
function that computes and returns the average of a one-dimensional array;use a loop to call this function three times. The other tasks should take the entire array as an argument, and the functions performing tasks “c” and “d” should return the answer to the calling program.
***
```c
#include <stdio.h>
#define ROWS 3
#define COLUMNS 5

double compute_row_average(double array[COLUMNS]);
double compute_array_average(double (*array)[COLUMNS], int rows);
double largest_value(double (*array)[COLUMNS], int rows);

int main(void)
{
	double data[ROWS][COLUMNS];

	for (int i = 0; i < ROWS; i++)
	{
		printf("Enter set of %d doubles: ", COLUMNS);
		for (int j = 0; j < COLUMNS; j++)
		{
			scanf("%lf", data[i] + j);
		}
	}

	// print row averages
	printf("Row Averages:\n");
	for (int i = 0; i < ROWS; i++)
	{
		printf("\tAverage for row %d: %.3f\n", i + 1, compute_row_average(data[i]));
	}

	// print array average
	printf("Average for entire array: %.3f\n", compute_array_average(data, ROWS));

	// print largest value
	printf("Maximum array value: %.3f\n", largest_value(data, ROWS));

	return 0;
}

double compute_row_average(double array[COLUMNS])
{
	double total = 0;
	for (int i = 0; i < COLUMNS; i++)
		total += array[i];

	return total / COLUMNS;
}

double compute_array_average(double (*array)[COLUMNS], int rows)
{
	double total = 0;
	for (int i = 0; i < rows; i++)
		for (int j = 0; j < COLUMNS; j++)
			total += array[i][j];

	return total / (rows * COLUMNS);
}

double largest_value(double (*array)[COLUMNS], int rows)
{
	double max = array[0][0];
	for (int i = 0; i < rows; i++)
		for (int j = 0; j < COLUMNS; j++)
			if (array[i][j] > max)
				max = array[i][j];

	return max;
}
```

## 10-14
Do Programming Exercise 13, but use variable-length array function parameters.
***
```c
#include <stdio.h>
#define ROWS 3
#define COLUMNS 5

double compute_row_average(double *array, int cols);
double compute_array_average(int rows, int cols, double array[rows][cols]);
double largest_value(int rows, int cols, double array[rows][cols]);

int main(void)
{
	double data[ROWS][COLUMNS];

	for (int i = 0; i < ROWS; i++)
	{
		printf("Enter set of %d doubles: ", COLUMNS);
		for (int j = 0; j < COLUMNS; j++)
		{
			scanf("%lf", data[i] + j);
		}
	}

	// print row averages
	printf("Row Averages:\n");
	for (int i = 0; i < ROWS; i++)
	{
		printf("\tAverage for row %d: %.3f\n", i + 1,
			   compute_row_average(data[i], COLUMNS));
	}

	// print array average
	printf("Average for entire array: %.3f\n",
		   compute_array_average(ROWS, COLUMNS, data));

	// print largest value
	printf("Maximum array value: %.3f\n",
		   largest_value(ROWS, COLUMNS, data));

	return 0;
}

double compute_row_average(double *array, int cols)
{
	double total = 0;
	for (int i = 0; i < cols; i++)
		total += array[i];

	return total / cols;
}

double compute_array_average(int rows, int cols, double array[rows][cols])
{
	double total = 0;
	for (int i = 0; i < rows; i++)
		for (int j = 0; j < cols; j++)
			total += array[i][j];

	return total / (rows * cols);
}

double largest_value(int rows, int cols, double array[rows][cols])
{
	double max = array[0][0];
	for (int i = 0; i < rows; i++)
		for (int j = 0; j < cols; j++)
			if (array[i][j] > max)
				max = array[i][j];

	return max;
}
```