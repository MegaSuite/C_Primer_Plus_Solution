# Chapter 4 Exs

## 4-1
Write a program that asks for your first name, your last name, and then prints the names in the format last, first.
***
```c
#include <stdio.h>

int main(void)
{
	char first_name[20];
	char last_name[20];

	printf("Enter your first and last name (e.g.: John Doe): ");
	scanf("%s %s", first_name, last_name);
	printf("%s, %s\n", last_name, first_name);

	return 0;
}
```
## 4-2
Write a program that requests your first name and does the following with it:
a. Prints it enclosed in double quotation marks
b. Prints it in a field 20 characters wide, with the whole field in quotes and the name at the right end of the field
c. Prints it at the left end of a field 20 characters wide, with the whole field enclosed in quotes
d. Prints it in a field three characters wider than the name
***
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
	char name[20];
	int name_length;

	printf("Enter your first name: ");
	scanf("%s", name);
	name_length = strlen(name);
	printf("\"%s\"\n", name); // a. enclosed in double quotes
	printf("\"%20s\"\n", name); // b. double quotes, 20 char wide, right-justified
	printf("\"%-20s\"\n", name); // c. double quotes, 20 char wide, left-justified
	printf("\"%*s\"\n", name_length + 3, name); // d. double quotes, 3 char wider than name

	return 0;
}
```

## 4-3
Write a program that reads in a floating-point number and prints it first in
decimal-point notation and then in exponential notation. Have the output use
the following formats (the number of digits shown in the exponent may be
different for your system):

a. The input is 21.3 or 2.1e+001.
b. The input is +21.290 or 2.129E+001.
***
```c
#include <stdio.h>

int main(void)
{
	float num;

	printf("Enter a number: ");
	scanf("%f", &num);
	printf("The input is %.1f or %.1e\n", num, num);
	printf("The input is %+.3f or %.3E\n", num, num);

	return 0;
}
```

## 4-4
Write a program that requests your height in inches and your name, and then displays the information in the following form:

	Dabney, you are 6.208 feet tall

Use type float, and use / for division. If you prefer, request the height in centimeters and display it in meters. 
***
```c
#include <stdio.h>

int main(void)
{
	const float INCHES_PER_FEET = 12;
	float height;
	char name[40];

	printf("What is your name?: ");
	scanf("%s", name);
	printf("What is your height in inches?: ");
	scanf("%f", &height);
	printf("%s, you are %.3f feet tall.\n", name, height / INCHES_PER_FEET);

	return 0;
}
```

## 4-5
Write a program that requests the download speed in megabits per second (Mbs) and the size of a file in megabytes (MB). The program should calculate the download time for the file. Note that in this context one byte is eight bits. Use type float, and use / for division. The program should report all three values (download speed, file size, and download time) showing two digits to the right of the decimal point, as in the following:

	At 18.12 megabits per second, a file of 2.20 megabytes downloads in 0.97 seconds. 
***
```c
#include <stdio.h>

int main(void)
{
	const float BITS_PER_BYTE = 8;
	float download_speed_Mps;
	float file_size_MB;

	printf("Enter the download speed (in megabits/second): ");
	scanf("%f", &download_speed_Mps);
	printf("Enter the file size (in megabytes): ");
	scanf("%f", &file_size_MB);
	printf("At %.2f megabits per second, a file of %.2f megabytes"
		   " downloads in %.2f seconds.\n", download_speed_Mps, file_size_MB,
		   file_size_MB * BITS_PER_BYTE / download_speed_Mps);

	return 0;
}
```

## 4-6
Write a program that requests the user’s first name and then the user’s last
name. Have it print the entered names on one line and the number of letters in
each name on the following line. Align each letter count with the end of the
corresponding name, as in the following:

    Melissa Honeybee 
          7        8

Next, have it print the same information, but with the counts aligned with the
beginning of each name.

    Melissa Honeybee 
    7       8
***
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
	char first_name[20];
	char last_name[20];

	printf("Enter your first and last name: ");
	scanf("%s %s", first_name, last_name);
	printf("\n");
	printf("%s %s\n", first_name, last_name);
	printf("%*lu %*lu\n", // right justified
		   (int) strlen(first_name), strlen(first_name),
		   (int) strlen(last_name), strlen(last_name));
	printf("\n");
	printf("%s %s\n", first_name, last_name);
	printf("%-*lu %-*lu\n", // left justified
		   (int) strlen(first_name), strlen(first_name),
		   (int) strlen(last_name), strlen(last_name));
	printf("\n");

	return 0;
}
```
## 4-7
Write a program that sets a type double variable to 1.0/3.0 and a type float variable to 1.0/3.0. Display each result three times—once showing four digits to the right of the decimal, once showing 12 digits to the right of the decimal, and once showing 16 digits to the right of the decimal. Also have the program include float.h and display the values of FLT_DIG and DBL_DIG. Are the displayed values of 1.0/3.0 consistent with these values?
***
```c
#include <stdio.h>
#include <float.h>

int main(void)
{
	double db_one_third = 1.0 / 3.0;
	float ft_one_third = 1.0 / 3.0;

	printf("Float                Double              \n");
	printf("-------------------- --------------------\n");
	printf("%-20.4f %-20.4f\n", ft_one_third, db_one_third); // show 4 digits
	printf("%-20.12f %-20.12f\n", ft_one_third, db_one_third); // show 12 digits
	printf("%-20.16f %-20.16f\n", ft_one_third, db_one_third);
	printf("\n");
	printf("FLT_DIG: %d\n", FLT_DIG);
	printf("DBL_DIG: %d\n", DBL_DIG);

	/* results: both float and double are accurate to at least the amount of sig
	figs specified by FLT_DIG and DBL_DIG */

	return 0;
}
```
## 4-8
Write a program that asks the user to enter the number of miles traveled and the number of gallons of gasoline consumed. It should then calculate and display the miles-per-gallon value, showing one place to the right of the decimal. Next, using the fact that one gallon is about 3.785 liters and one mile is about 1.609 kilometers, it should convert the mile- per-gallon value to a liters-per-100-km value, the usual European way of expressing fuel consumption, and display the result, showing one place to the right of the decimal.Note that the U. S. scheme measures the distance traveled per amount of fuel (higher is better), whereas the European scheme measures the amount of fuel per distance (lower is better). Use symbolic constants (using const or #define) for the two conversion factors.
***
```c
#include <stdio.h>

int main(void)
{
	const float KM_PER_MILE = 1.609;
	const float LT_PER_GALLON = 3.785;
	float miles_travelled, gallons_gas_consumed;
	float miles_per_gallon, liters_per_100km;

	printf("Enter your distance travelled in miles: ");
	scanf("%f", &miles_travelled);
	printf("Enter the amount of gas consumed in gallons: ");
	scanf("%f", &gallons_gas_consumed);

	// calculate miles per gallon and liters per km
	miles_per_gallon = miles_travelled / gallons_gas_consumed;
	liters_per_100km = 100. / miles_per_gallon * LT_PER_GALLON / KM_PER_MILE;

	printf("Miles per gallon: %.1f\n", miles_per_gallon);
	printf("Liters per 100 kilometers: %.1f\n", liters_per_100km);

	return 0;
}
```
