# Chapter 5 Exs

## 5-1
Write a program that converts time in minutes to time in hours and minutes. Use `#define` or `const` to create a symbolic constant for 60. Use a while loop to allow the user to enter values repeatedly and terminate the loop if a value for the time of 0 or less is entered. 
***
```c
#include <stdio.h>

const int MINUTES_PER_HOUR = 60;

int main(void)
{
	int minutes;

	printf("Enter an amount of time in minutes: "); // get first input
	scanf("%d", &minutes);
		
	while (minutes > 0)
	{
		printf("%d minute(s) is %d hour(s) and %d minute(s).\n",
			   minutes,
			   minutes / MINUTES_PER_HOUR, // hours
			   minutes % MINUTES_PER_HOUR); // minutes

		printf("Enter an amount of time in minutes: "); // get new input
		scanf("%d", &minutes);
	}

	return 0;
}
```

## 5-2
Write a program that asks for an integer and then prints all the integers from (and including) that value up to (and including) a value larger by 10. (That is, if the input is 5, the output runs from 5 to 15.) Be sure to separate each output value by a space or tab or newline.

---
```c
#include <stdio.h>

int main(void)
{
	int input;
	int i = 0;

	printf("Enter an integer: ");
	scanf("%d", &input);
	while (i <= 10)
	{
		printf("%d\n", input + i);
		i++;
	}

	return 0;
}
```


## 3-5
Write a program that asks the user to enter the number of days and then converts that value to weeks and days .For example, it would convert 18 days to 2 weeks, 4 days. Display results in the following format:

    18 days are 2 weeks, 4 days.

Use a while loop to allow the user to repeatedly enter day values; terminate the loop when the user enters a nonpositive value, such as 0 or -20.

---
```c
#include <stdio.h>

const int DAYS_PER_WEEK = 7;

int main(void)
{
	int days;

	printf("Enter a number of days (or enter 0 to quit): ");
	scanf("%d", &days);
	while (days > 0)
	{
		printf("%d days are %d weeks, %d days.\n", days, days / DAYS_PER_WEEK,
		       days % DAYS_PER_WEEK);

		printf("Enter a number of days (or enter 0 to quit): ");
		scanf("%d", &days);
	}

	return 0;
}
```

## 5-4
Write a program that asks the user to enter a height in centimeters and then displays the height in centimeters and in feet and inches. Fractional centimeters and inches should be allowed, and the program should allow the user to continue entering heights until a nonpositive value is entered. A sample run should look like this:

    Enter a height in centimeters: 182
    182.0 cm = 5 feet, 11.7 inches
    Enter a height in centimeters (<=0 to quit): 168.7 
    168.0 cm = 5 feet, 6.4 inches
    Enter a height in centimeters (<=0 to quit): 0
    bye

---
```c
#include <stdio.h>

const float CM_PER_IN = 2.54;
const int IN_PER_FT = 12;

int main(void)
{
	float height_cm, height_in, inches;
	int feet;

	printf("Enter a height in centimeters: ");
	scanf("%f", &height_cm);

	while (height_cm > 0)
	{
		height_in = height_cm / CM_PER_IN; // convert height to inches
		feet = (int) height_in / IN_PER_FT; // get number of feet in height
		inches = height_in - feet * IN_PER_FT; // get remaining inches

		printf("%.1f cm = %d feet, %.1f inches\n",
		       height_cm, feet, inches);

		printf("Enter a height in centimeters (<= 0 to quit): ");
		scanf("%f", &height_cm);
	}

	printf("bye\n");
	return 0;
}
```

## 5-5
Change the program addemup.c (Listing 5.13), which found the sum of the first 20 integers. (If you prefer, you can think of addemup.c as a program that calculates how much money you get in 20 days if you receive $1 the first day, $2 the second day, $3 the third day, and so on.) Modify the program so that you can tell it interactively how far the calculation should proceed. That is, replace the 20 with a variable that is read in.

---
```c
#include <stdio.h>

int main(void)
{
	int count, sum, max_count;
	sum = 0;
	count = 1;

	printf("How many integers would you like to sum? ");
	scanf("%d", &max_count);
	while (count <= max_count)
	{
		sum = sum + count;
		count++;
	}
	printf("sum = %d\n", sum);

	return 0;
}
```

## 5-6
Now modify the program of Programming Exercise 5 so that it computes the sum of the squares of the integers. (If you prefer, how much money you receive if you get $1 the first day, $4 the second day, $9 the third day, and so on. This looks like a much better deal!) C doesn’t have a squaring function, but you can use the fact that the square of n is `n * n`.

---
```c
#include <stdio.h>

int main(void)
{
	int sum, count, max_count;
	sum = 0;
	count = 1;

	printf("How many squares would you like to sum? ");
	scanf("%d", &max_count);
	while (count <= max_count)
	{
		sum = sum + count * count;
		count++;
	}
	printf("The sum of the first %d squares is: %d\n", max_count, sum);

	return 0;
}
```

## 5-7
Write a program that requests a type double number and prints the value of the number cubed. Use a function of your own design to cube the value and print it. The `main()` program should pass the entered value to this function.

---
```c
#include <stdio.h>

double cubed(double n); // prototype declaration for cubed

int main(void)
{
	double input;
	printf("Enter a number to cube: ");
	scanf("%lf", &input);

	printf("%.3f cubed is %.3f\n", input, cubed(input));

	return 0;
}

double cubed(double n)
{
	return n * n * n;
}
```

## 5-8
Write a program that displays the results of applying the modulus operation. The user should first enter an integer to be used as the second operand, which will then remain unchanged. Then the user enters the numbers for which the modulus will be computed, terminating the process by entering 0 or less. A sample run should look like this:

    > This program computes moduli.
    > Enter an integer to serve as the second operand: 256
    > Now enter the first operand: 438
    > 438 % 256 is 182
    > Enter next number for first operand (<= 0 to quit): 1234567
    > 1234567 % 256 is 135
    > Enter next number for first operand (<= 0 to quit): 0
    > Done

---
```c
#include <stdio.h>

int main(void)
{
	int first, second;
	printf("This program computes moduli.\n");
	printf("Enter an integer to serve as the second operand: ");
	scanf("%d", &second);
	printf("Now enter the first operand: ");
	scanf("%d", &first);
	while (first > 0)
	{
		printf("%d %% %d is %d\n", first, second, first % second); //print results

		printf("Enter next number for first operand (<= 0 to quit): ");
		scanf("%d", &first); // get new input
	}

	return 0;
}
```

## 5-9
Write a program that requests the user to enter a Fahrenheit temperature. The program should read the temperature as a type double number and pass it as an argument to a user-supplied function called `Temperatures()`. This function should calculate the Celsius equivalent and the Kelvin equivalent and display all three temperatures with a precision of two places to the right of the decimal. It should identify each value with the temperature scale it represents. Here is the formula for converting Fahrenheit to Celsius:`Celsius = 5.0 / 9.0 * (Fahrenheit - 32.0)` The Kelvin scale, commonly used in science, is a scale in which 0 represents absolute zero, the lower limit to possible temperatures. Here is the formula for converting Celsius to Kelvin: `Kelvin = Celsius + 275.16` The `Temperatures()` function should use const to create symbolic representations of the three constants that appear in the conversions. The `main()` function should use a loop to allow the user to enter temperatures repeatedly, stopping when a q or other nonnumeric value is entered. Use the fact that `scanf()` returns the number of items read, so it will `return 1` if it reads a number, but it won’t return 1 if the user enters q. The `== `operator tests for equality, so you can use it to compare the return value of `scanf()` with 1.

---
```c
#include <stdio.h>

void Temperatures(double fahr); // prototype declaration of Temperatures

int main(void)
{
	double fahr;
	printf("This program converts fahrenheit to celsius and kelvin.\n");
	printf("Enter a temperature in degrees fahrenheit (q to quit): ");
	while (scanf("%lf", &fahr) == 1) // continue executing loop if user enters valid number
	{
		Temperatures(fahr); // convert fahr to celsius and kelvin

		// prompt for new input
		printf("Enter a temperature in degrees fahrenheit (q to quit): ");
	}

	printf("bye\n");
}

void Temperatures(double fahr)
{
	const double FAHR_TO_CEL_SCALE = 5.0 / 9.0;
	const double FAHR_TO_CEL_OFFSET = -32.0;
	const double CEL_TO_KEL_OFFSET = 273.16;

	double celsius = (fahr + FAHR_TO_CEL_OFFSET) * FAHR_TO_CEL_SCALE;
	double kelvin = celsius + CEL_TO_KEL_OFFSET;

	printf("%.2f degrees fahrenheit is %.2f degrees celsius or %.2f degrees kelvin.\n",
			fahr, celsius, kelvin);
}
```