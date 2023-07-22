# Chapter 13

## 13-1 
Modify Listing 13.1 so that it solicits the user to enter the filename and reads the user’s response instead of using command-line arguments.
***
```c
#include <stdio.h>
#include <stdlib.h>

#define SLEN 81

void get(char * string, int n);

int main(void)
{
	int ch;
	FILE * fp;
	char filename[SLEN];
	unsigned long chcount = 0;

	printf("Enter a file name: ");
	get(filename, SLEN);

	if ((fp = fopen(filename, "r")) == NULL)
	{
		printf("Could not open file %s\n", filename);
		exit(EXIT_FAILURE);
	}

	while ((ch = getc(fp)) != EOF)
	{
		putc(ch, stdout);
		chcount++;
	}

	printf("File %s has %lu characters.\n", filename, chcount);

	fclose(fp);

	return 0;
}

void get(char * string, int n)
{
	// wrapper for fgets - read from stdin and replace
	// first newline with null character

	fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}
		string++;
	}
}
```

## 13-2
Write a file-copy program that takes the original filename and the copy file from the command line. Use standard I/O and the binary mode, if possible.
***
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	int ch;
	FILE * fsource;
	FILE * fdest;

	if (argc != 3)
	{
		printf("Usage: %s source_file destination_file\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	if ((fsource = fopen(argv[1], "rb")) == NULL)
	{
		fprintf(stderr, "Could not open file %s for read\n", argv[1]);
		exit(EXIT_FAILURE);
	}
	if ((fdest = fopen(argv[2], "wb")) == NULL)
	{
		fprintf(stderr, "Could not open file %s for write\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	while ((ch = getc(fsource)) != EOF)
		putc(ch, fdest);

	fclose(fsource);
	fclose(fdest);

	return 0;
}
```

## 13-3
Write a file copy program that prompts the user to enter the name of a text file to act as the source file and the name of an output file. The program should use the toupper() function from ctype.h to convert all text to uppercase as it’s written to the output file. Use standard I/O and the text mode.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

#define SLEN 81

void get(char * string, int n);

int main(void)
{
	int ch;
	FILE * fsource;
	FILE * fdest;
	char source[SLEN];
	char dest[SLEN];

	printf("Enter a source file: ");
	get(source, SLEN);
	printf("Enter a destination file: ");
	get(dest, SLEN);

	if ((fsource = fopen(source, "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s for read.\n", source);
		exit(EXIT_FAILURE);
	}
	if ((fdest = fopen(dest, "w")) == NULL)
	{
		fprintf(stderr, "Could not open file %s for write.\n", dest);
		exit(EXIT_FAILURE);
	}
	while ((ch = getc(fsource)) != EOF)
	{
		if (islower(ch))
			ch = toupper(ch);
		putc(ch, fdest);
	}

	fclose(fsource);
	fclose(fdest);

	return 0;
}

void get(char * string, int n)
{
	// wrapper for fgets - read from stdin and replace
	// first newline with null character

	fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}
		string++;
	}
}
```

## 13-4
Write a program that sequentially displays onscreen all the files listed in the command line. Use argc to control a loop.
***
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	FILE * fp;
	int ch;

	if (argc == 1)
	{
		printf("Usage: %s file1 file2 ...\n", argv[0]);
		exit(EXIT_FAILURE);
	}
	for (int i = 1; i < argc; i++)
	{
		if ((fp = fopen(argv[i], "r")) == NULL)
		{
			fprintf(stderr, "Could not open file %s.\n", argv[i]);
			exit(EXIT_FAILURE);
		}

		while ((ch = getc(fp)) != EOF)
			putc(ch, stdout);

		fclose(fp);
	}

	return 0;
}
```

## 13-5
Modify the program in Listing 13.5 so that it uses a command-line interface instead of an interactive interface.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define BUFSIZE 4096

void append(FILE *source, FILE *dest);

int main(int argc, char *argv[])
{
	FILE * fa;
	FILE * fs;
	int files = 0;
	int ch;

	if (argc < 3)
	{
		printf("Usage: %s targetfile sourcefile1 [sourcefile2] ...\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	if ((fa = fopen(argv[1], "a+")) == NULL)
	{
		fprintf(stderr, "Could not open file %s\n", argv[1]);
		exit(EXIT_FAILURE);
	}

	if (setvbuf(fa, NULL, _IOFBF, BUFSIZE) != 0)
	{
		fprintf(stderr, "Could not create output buffer.\n");
		exit(EXIT_FAILURE);
	}

	for (int i = 2; i < argc; i++)
	{
		if (strcmp(argv[i], argv[1]) == 0)
		{
			fprintf(stderr, "Can't append file to itself.\n");
		}
		else if ((fs = fopen(argv[i], "r")) == NULL)
		{
			fprintf(stderr, "Could not open file %s\n", argv[i]);
		}
		else
		{ 
			if (setvbuf(fs, NULL, _IOFBF, BUFSIZE) != 0)
			{
				fprintf(stderr, "Could not create input buffer.\n");
				continue;
			}
			append(fs, fa);
			if (ferror(fs) != 0)
				fprintf(stderr, "Error in reading file %s.\n", argv[i]);
			if (ferror(fa) != 0)
				fprintf(stderr, "Error in writing to file %s.\n", argv[1]);
			fclose(fs);
			files++;
			printf("File %s appended.\n", argv[i]);
		}
	}
	printf("Done appending. %d files appended.\n", files);

	rewind(fa);
	printf("%s contents:\n", argv[1]);
	while ((ch = getc(fa)) != EOF)
		putchar(ch);
	puts("Done displaying.\n");
	fclose(fa);

	return 0;
}

void append(FILE *source, FILE *dest)
{
	size_t bytes;
	static char temp[BUFSIZE];

	while ((bytes = fread(temp, sizeof(char), BUFSIZE, source)) > 0)
		fwrite(temp, sizeof(char), bytes, dest);
}
```

## 13-6
Programs using command-line arguments rely on the user’s memory of how to use them correctly. Rewrite the program in Listing 13.2 so that, instead of using command-line arguments, it prompts the user for the required information.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SLEN 81

void get(char * string, int n);

int main(void)
{
	FILE *in;
	FILE *out;
	char source[SLEN];
	char dest[SLEN];
	int ch;
	unsigned int count = 0;

	printf("Enter a file to reduce: ");
	get(source, SLEN - 5);

	if ((in = fopen(source, "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", source);
		exit(EXIT_FAILURE);
	}

	strcpy(dest, source);
	strcat(dest, ".red");
	if ((out = fopen(dest, "w")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", dest);
		exit(EXIT_FAILURE);
	}
	while ((ch = getc(in)) != EOF)
		if (count++ % 3 == 0)
			putc(ch, out);

	if (fclose(in) != 0)
		printf("Error in closing file %s.\n", source);
	if (fclose(out) != 0)
		printf("Error in closing file %s.\n", dest);

	return 0;
}

void get(char * string, int n)
{
	// wrapper for fgets - read from stdin and replace
	// first newline with null character

	fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}
		string++;
	}
}
```

## 13-7
Write a program that opens two files. You can obtain the filenames either by using command-line arguments or by soliciting the user to enter them.

    a. Have the program print line 1 of the first file, line 1 of the second file, line 2 of the first file, line 2 of the second file, and so on,until the last line of the longer file (in terms of lines) is printed.
    b. Modify the program so that lines with the same line number are printed on the same line.

***
```c
#include <stdio.h>
#include <stdlib.h>

#define LLEN 70 

void fget(char * string, int n, FILE *file);

int main(int argc, char *argv[])
{
	FILE *file1, *file2;
	int ch;
	char tmp[LLEN];

	if (argc < 3)
	{
		printf("Usage: %s file1 file2\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	if ((file1 = fopen(argv[1], "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[1]);
		exit(EXIT_FAILURE);
	}
	if ((file2 = fopen(argv[2], "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	while (1)
	{
		tmp[0] = '\0';
		fget(tmp, LLEN, file1);
		printf("%-*s\t", LLEN, tmp);

		tmp[0] = '\0';
		fget(tmp, LLEN, file2);
		printf("%-*s\n", LLEN, tmp);

		if (feof(file1) && feof(file2))
			break;
	}

	return 0;
}

void fget(char * string, int n, FILE *file)
{
	// wrapper for fgets - replaces
	// first newline with null character

	fgets(string, n, file);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}
		string++;
	}
}
```

## 13-8
Write a program that takes as command-line arguments a character and zero or more filenames. If no arguments follow the character, have the program read the standard input. Otherwise, have it open each file in turn and report how many times the character appears in each file. The filename and the character itself should be reported along with the count. Include error- checking to see whether the number of arguments is correct and whether the files can be opened. If a file can’t be opened, have the program report that fact and go on to the next file.
***
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	char character;
	int ch;

	if (argc < 2)
	{
		printf("Usage: %s <char> [file1] [file2] ...\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	character = *(argv[1]);

	if (argc == 2)
	{
		int count = 0;

		// read from stdin
		while ((ch == getchar()) != EOF)
			if (ch == character)
				count++;

		printf("Character count for %c in ...\n", character);
		printf("Standard in: %d\n", count);
	}
	else
	{
		FILE * fp;
		int counts[argc - 2];
		for (int i = 2; i < argc; i++)
		{
			counts[i - 2] = 0;
			if ((fp = fopen(argv[i], "r")) == NULL)
			{
				fprintf(stderr, "Can't open file %s\n", argv[i]);
				continue;
			}

			while ((ch = getc(fp)) != EOF)
				if (ch == character)
					counts[i-2]++;

			fclose(fp);
		}

		printf("Character count for %c in ...\n", character);
		for (int i = 2; i < argc; i++)
			printf("%s: \t\t%d\n", argv[i], counts[i-2]);

	}

	return 0;
}
```

## 13-9
Modify the program in Listing 13.3 so that each word is numbered according to the order in which it was added to the list, starting with 1. Make sure that, when the program is run a second time, new word numbering resumes where the previous numbering left off.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 41

int main(void) 
{
	FILE *fp;
	char words[MAX];
	char line[MAX + 6];
	int word_count = 1;

	if ((fp = fopen("wordy", "a+")) == NULL)
	{
		fprintf(stdout,"Can't open \"wordy\" file.\n");
		exit(EXIT_FAILURE);
	}

	// count lines already written to file
	rewind(fp);
	while (fgets(line, MAX + 6, fp) != NULL)
		word_count++;
	
	puts("Enter words to add to the file; press the #");
	puts("key at the beginning of a line to terminate.");
	while ((fscanf(stdin,"%40s", words) == 1) && (words[0] != '#'))
		fprintf(fp, "%d %s\n", word_count++, words);

	puts("File contents:");
	rewind(fp); /* go back to beginning of file */
	while (fgets(line, MAX + 6, fp) != NULL)
		fputs(line, stdout);
	puts("Done!");
	
	if (fclose(fp) != 0)
		fprintf(stderr,"Error closing file\n");
	
	return 0; 
}
```

## 13-10
Write a program that opens a text file whose name is obtained interactively. Set up a loop that asks the user to enter a file position. The program then should print the part of the file starting at that position and proceed to the next newline character. Let negative or nonnumeric input terminate the user-input loop.
***
```c
#include <stdio.h>
#include <stdlib.h>

#define SLEN 81

void get(char * string, int n);

int main(void)
{
	FILE *fp;
	char filename[SLEN];
	long pos;
	int ch;

	printf("Enter a filename: ");
	get(filename, SLEN);

	if ((fp = fopen(filename, "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", filename);
		exit(EXIT_FAILURE);
	}

	printf("Enter a position: ");
	while (scanf("%ld", &pos) == 1)
	{
		if (pos < 0)
			break;

		fseek(fp, pos, SEEK_SET);
		while ((ch = getc(fp)) != EOF && ch != '\n')
			putchar(ch);
		putchar('\n');

		printf("Enter a positition: ");
	}

	fclose(fp);
	puts("Done.");
	return 0;
}

void get(char * string, int n)
{
	// wrapper for fgets - read from stdin and replace
	// first newline with null character

	fgets(string, n, stdin);

	while (*string != '\0')
	{
		if (*string == '\n')
		{
			*string = '\0';
			break;
		}
		string++;
	}
}
```


## 13-11
Write a program that takes two command-line arguments. The first is a string; the second is the name of a file. The program should then search the file, printing all lines containing the string. Because this task is line oriented rather than character oriented, use fgets() instead of getc(). Use the standard C library function strstr() (briefly described in exercise 7 of Chapter 11) to search each line for the string. Assume no lines are longer than 255 characters.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define LINEMAX 255

int main(int argc, char *argv[])
{
	FILE *fp;
	char line[LINEMAX];

	if (argc != 3)
	{
		fprintf(stderr, "Usage: %s <string> <filename>\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	if ((fp = fopen(argv[2], "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	while (fgets(line, LINEMAX, fp) != NULL)
	{
		if (strstr(line, argv[1]) != NULL)
			fputs(line, stdout);
	}

	fclose(fp);
	return 0;
}
```

## 13-12
Create a text file consisting of 20 rows of 30 integers. The integers should be in the range 0–9 and be separated by spaces. The file is a digital representation of a picture, with the values 0 through 9 representing increasing levels of darkness. Write a program that reads the contents of the file into a 20-by-30 array of ints. In a crude approach toward converting this digital representation to a picture, have the program use the values in this array to initialize a 20-by-31 array of chars, with a 0 value corresponding to a space character, a 1 value to the period character, and so on, with each larger number represented by a character that occupies more space. For example, you might use # to represent 9. The last character (the 31st) in each row should be a null character, making it an array of 20 strings. Have the program display the resulting picture (that is, print the strings) and also store the result in a text file.

Note: use data.txt as input file to test this program.

***
```c
#include <stdio.h>
#include <stdlib.h>

#define ROWS 20
#define COLS 30

int main(int argc, char *argv[])
{
	FILE *fp;
	int data[ROWS][COLS];
	char img[ROWS][COLS + 1];
	char ch;

	if (argc != 3)
	{
		printf("Usage: %s <data file> <image file>\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	// open data file
	if ((fp = fopen(argv[1], "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[1]);
		exit(EXIT_FAILURE);
	}

	// read ints from file
	for (int i = 0; i < ROWS; i++)
		for (int j = 0; j < COLS; j++)
			if (fscanf(fp, "%d", *(data + i) + j) != 1)
			{
				fprintf(stderr, "Invalid or corrupted data file.\n");
				exit(EXIT_FAILURE);
			}
	fclose(fp); // done with fp for now

	// convert ints to characters
	for (int i = 0; i < ROWS; i++)
	{	
		int j;
		for (j = 0; j < COLS; j++)
		{
			switch (data[i][j])
			{
				case(0):
					ch = ' ';
					break;
				case(1):
					ch = '.';
					break;
				case(2):
					ch = '\'';
					break;
				case(3):
					ch = ':';
					break;
				case(4):
					ch = '~';
					break;
				case(5):
					ch = '*';
					break;
				case(6):
					ch = '=';
					break;
				case(7):
					ch = '}';
					break;
				case(8):
					ch = '&';
					break;
				case(9):
					ch = '#';
					break;
				default:
					fprintf(stderr, "Error: int out of bounds.\n");
					exit(EXIT_FAILURE);
			}
			img[i][j] = ch;
		}
		img[i][j] = '\0'; // j = COLS here
	}

	// open image file
	if ((fp = fopen(argv[2], "w")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	for (int i = 0; i < ROWS; i++)
	{
		// print to console and file
		puts(img[i]);
		fprintf(fp, "%s\n", img[i]);
	}
	fclose(fp);

	return 0;
}
```

## 13-13
Do Programming Exercise 12, but use variable-length arrays (VLAs) instead of
standard arrays.

Note: use data.txt as input file to test this program.

***
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <stdbool.h>

int main(int argc, char *argv[])
{
	FILE *fp;
	int ch;
	int rows = 0, cols = 0, lastrow_cols;
	bool first_line;

	if (argc != 3)
	{
		printf("Usage: %s <data file> <image file>\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	// open data file
	if ((fp = fopen(argv[1], "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[1]);
		exit(EXIT_FAILURE);
	}

	// this passage of code counts the number of columns in each row
	// and compares them for consistency, exiting with an error if they're
	// not all equal
	first_line = true;
	while ((ch = getc(fp)) != EOF && isdigit(ch))
	{	

		cols = 1;
		while ((ch = getc(fp)) != EOF && ch != '\n')
			if (isdigit(ch))
				cols++;
		if (first_line)
		{
			first_line = false;
		}
		else
		{
			if (cols != lastrow_cols)
			{
				fprintf(stderr, "Invalid data file: unequal number of columns in rows.\n");
				exit(EXIT_FAILURE);
			}
		}
		lastrow_cols = cols;
	}
	if (cols == 0)
	{
		fprintf(stderr, "Invalid data file.\n");
		exit(EXIT_FAILURE);
	}

	// this passage counts the number of rows
	rewind(fp);
	while ((ch = getc(fp)) != EOF)
		if (ch == '\n')
			rows++;

	// with the number of rows and columns counted, the data and img arrays
	// can be allocated dynamically
	int data[rows][cols];
	char img[rows][cols + 1];

	// read ints from file
	rewind(fp);
	for (int i = 0; i < rows; i++)
		for (int j = 0; j < cols; j++)
			if (fscanf(fp, "%d", *(data + i) + j) != 1)
			{
				fprintf(stderr, "Invalid data file.\n");
				exit(EXIT_FAILURE);
			}
	fclose(fp); // done with data file

	// convert ints to characters
	for (int i = 0; i < rows; i++)
	{	
		int j;
		for (j = 0; j < cols; j++)
		{
			switch (data[i][j])
			{
				case(0):
					ch = ' ';
					break;
				case(1):
					ch = '.';
					break;
				case(2):
					ch = '\'';
					break;
				case(3):
					ch = ':';
					break;
				case(4):
					ch = '~';
					break;
				case(5):
					ch = '*';
					break;
				case(6):
					ch = '=';
					break;
				case(7):
					ch = '}';
					break;
				case(8):
					ch = '&';
					break;
				case(9):
					ch = '#';
					break;
				default:
					fprintf(stderr, "Error: int out of bounds.\n");
					exit(EXIT_FAILURE);
			}
			img[i][j] = ch;
		}
		img[i][j] = '\0'; // j = cols here
	}

	// open image file
	if ((fp = fopen(argv[2], "w")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	for (int i = 0; i < rows; i++)
	{
		// print to console and file
		puts(img[i]);
		fprintf(fp, "%s\n", img[i]);
	}
	fclose(fp);

	return 0;
}
```

## 13-14
Digital images, particularly those radioed back from spacecraft, may have glitches. Add a de-glitching function to programming exercise 12. It should compare each value to its immediate neighbors to the left and right, above and below. If the value differs by more than 1 from each of its neighbors, replace the value with the average of the neighboring values. You should round the average to the nearest integer value. Note that the points along the boundaries have fewer than four neighbors, so they require special handling.
Note: use data.txt as input file to test this program.

***
```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

#define ROWS 20
#define COLS 30

void deglitch(int data[ROWS][COLS]);

int main(int argc, char *argv[])
{
	FILE *fp;
	int data[ROWS][COLS];
	char img[ROWS][COLS + 1];
	char ch;

	if (argc != 3)
	{
		printf("Usage: %s <data file> <image file>\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	// open data file
	if ((fp = fopen(argv[1], "r")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[1]);
		exit(EXIT_FAILURE);
	}

	// read ints from file
	for (int i = 0; i < ROWS; i++)
		for (int j = 0; j < COLS; j++)
			if (fscanf(fp, "%d", *(data + i) + j) != 1)
			{
				fprintf(stderr, "Invalid or corrupted data file.\n");
				exit(EXIT_FAILURE);
			}
	fclose(fp); // done with fp for now

	// de-glitch the data
	deglitch(data);

	// convert ints to characters
	for (int i = 0; i < ROWS; i++)
	{	
		int j;
		for (j = 0; j < COLS; j++)
		{
			switch (data[i][j])
			{
				case(0):
					ch = ' ';
					break;
				case(1):
					ch = '.';
					break;
				case(2):
					ch = '\'';
					break;
				case(3):
					ch = ':';
					break;
				case(4):
					ch = '~';
					break;
				case(5):
					ch = '*';
					break;
				case(6):
					ch = '=';
					break;
				case(7):
					ch = '}';
					break;
				case(8):
					ch = '&';
					break;
				case(9):
					ch = '#';
					break;
				default:
					fprintf(stderr, "Error: int out of bounds.\n");
					exit(EXIT_FAILURE);
			}
			img[i][j] = ch;
		}
		img[i][j] = '\0'; // j = COLS here
	}

	// open image file
	if ((fp = fopen(argv[2], "w")) == NULL)
	{
		fprintf(stderr, "Could not open file %s.\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	for (int i = 0; i < ROWS; i++)
	{
		// print to console and file
		puts(img[i]);
		fprintf(fp, "%s\n", img[i]);
	}
	fclose(fp);

	return 0;
}

void deglitch(int data[ROWS][COLS])
{
	float total;
	int count;

	for (int i = 0; i < ROWS; i++)
		for (int j = 0; j < COLS; j++)
		{
			total = 0;
			count = 0;
			if (i + 1 < ROWS)
			{
				if (abs(data[i][j] - data[i + 1][j]) < 2)
					continue;

				total += data[i + 1][j];
				count++;
			}
			if (j + 1 < COLS)
			{
				if (abs(data[i][j] - data[i][j + 1]) < 2)
					continue;
				
				total += data[i][j + 1];
				count++;
			}
			if (i > 0)
			{
				if (abs(data[i][j] - data[i - 1][j]) < 2)
					continue;
				
				total += data[i - 1][j];
				count++;
			}
			if (j > 0)
			{
				if (abs(data[i][j] - data[i][j - 1]) < 2)
					continue;
				
				total += data[i][j - 1];
				count++;
			}
			
			// if none of the continue statements have been triggered,
			// replace data[i][j] with the rounded average of its neighbors
			data[i][j] = (int) rintf(total / count);
		}
}
```

## 13-data.txt
```
0 0 9 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 2 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 9 0 0 0 0 0 0 0 5 8 9 9 8 5 5 2 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 5 8 1 9 8 5 4 5 2 0 0 0 0 0 0 0 0 0
0 0 0 0 9 0 0 0 0 0 0 0 5 8 9 9 8 5 0 4 5 2 0 0 0 0 0 0 0 0
0 0 9 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 0 0 4 5 2 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 5 8 9 1 8 5 0 0 0 4 5 2 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 0 0 0 0 4 5 2 0 0 0 0 0
5 5 5 5 5 5 5 5 5 5 5 5 5 8 9 9 8 5 5 5 5 5 5 5 5 5 5 5 5 5
8 8 8 8 8 8 8 8 8 8 8 8 5 8 9 9 8 5 8 8 8 8 8 8 8 8 8 8 8 8
9 9 9 9 0 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 3 9 9 9 9 9 9 9
8 8 8 8 8 8 8 8 8 8 8 8 5 8 9 9 8 5 8 8 8 8 8 8 8 8 8 8 8 8
5 5 5 5 5 5 5 5 5 5 5 5 5 8 9 9 8 5 5 5 5 5 5 5 5 5 5 5 5 5 
0 0 0 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 0 0 0 0 6 6 0 0 0 0 0 0
0 0 0 0 2 2 0 0 0 0 0 0 5 8 9 9 8 5 0 0 5 6 0 0 6 5 0 0 0 0
0 0 0 0 3 3 0 0 0 0 0 0 5 8 9 9 8 5 0 5 6 1 1 1 1 6 5 0 0 0
0 0 0 0 4 4 0 0 0 0 0 0 5 8 9 9 8 5 0 0 5 6 0 0 6 5 0 0 0 0
0 0 0 0 5 5 0 0 0 0 0 0 5 8 9 9 8 5 0 0 0 0 6 6 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 5 8 9 9 8 5 0 0 0 0 0 0 0 0 0 0 0 0
```