# Chapter 14
## 14-1
Redo Review Question 5, but make the argument the spelled-out name of the month instead of the month number. (Don’t forget about strcmp().) Test the function in a simple program.

Review Question 5:
Write a function that, when given the month number, returns the total days in the year up to and including that month. Assume that the structure template of question 3 and an appropriate array of such structures are declared externally.

Review Question 3:
Devise a structure template that will hold the name of a month, a three-letter abbreviation for the month, the number of days in the month, and the month number.
***
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MONTHS 12
#define LEN 20
#define ABREV_LEN 3

int daysupto(const char *);
void get(char *, int);

struct month {
	char name[LEN];
	char abr[ABREV_LEN];
	int month_no;
	int days;
};

struct month months[MONTHS];
int days_in_month[MONTHS] = {31,28,31,30,31,30,31,31,30,31,30,31};
char *names[MONTHS] = {"January", "February", "March", "April", "May",
					   "June", "July", "August", "September", "October",
					   "November", "December"};
char *abrevs[MONTHS] = {"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul",
                        "Aug", "Sep", "Oct", "Nov", "Dec"};

int main(void){

	char monthname[LEN];
	int totaldays;

	printf("Enter the full name of a month, capitalized (eg. March): ");
	get(monthname, LEN);

	totaldays = daysupto(monthname);

	if (totaldays != 0)
		printf("There are %d days in the year up to and including %s.\n",
			   totaldays, monthname);
	else
		printf("There is no month named %s\n", monthname);

	puts("Bye.");
	return 0;
}

int daysupto(const char *monthname){
	
	// takes a string containing the name of a month and returns
	// the total days in the year up to and including that month

	// initialize array of month structs
	for (int i = 0; i < MONTHS; i++)
	{
		strncpy(months[i].name, names[i], LEN);
		strncpy(months[i].abr, abrevs[i], ABREV_LEN);
		months[i].month_no = i + 1;
		months[i].days = days_in_month[i];
	}

	int totaldays = 0;

	for (int i = 0; i < MONTHS; i++)
	{
		totaldays += months[i].days;

		// if month_name matches name of current month, return count of days
		if (strcmp(monthname, months[i].name) == 0)
			return totaldays;
	}

	// no match found, return 0
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

## 14-2
Write a program that prompts the user to enter the day, month, and year. The month can be a month number, a month name, or a month abbreviation. The program then should return the total number of days in the year up through the given day. (Do take leap years into account.)
***
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define LEN 10
#define MONTHS 12

void get(char *, int);

struct month
{
	char name[LEN];
	char abrev[4];
	char monthno[3];
	int days;
};

struct month months[MONTHS] = {
	{"January", "JAN", "01", 31},
	{"February", "FEB", "02", 28},
	{"March", "MAR", "03", 31},
	{"April", "APR", "04", 30},
	{"May", "MAY", "05", 31},
	{"June", "JUN", "06", 30},
	{"July", "JUL", "07", 31},
	{"August", "AUG", "08", 31},
	{"September", "SEP", "09", 30},
	{"October", "OCT", "10", 31},
	{"November", "NOV", "11", 30},
	{"December", "DEC", "12", 31}
};

int main(void)
{
	int year, day, i, total;
	char month[LEN];

	printf("Please enter a year: ");
	while (scanf("%d", &year) != 1)
	{
		// clear input stream
		while (getchar() != '\n') continue;

		printf("Please enter a year: ");
	}
	// clear input stream
	while (getchar() != '\n') continue;

	printf("Please enter a month by name, abbreviation\n");
	printf("or two-digit number (eg. January, JAN or 01): ");
	get(month, LEN);

	printf("Please enter a day: ");
	while (scanf("%d", &day) != 1)
	{
		// clear input stream
		while (getchar() != '\n') continue;

		printf("Please enter a day: ");
	}
	// clear input stream
	while (getchar() != '\n') continue;

	// match input to month
	for (i = 0; i < MONTHS; i++)
	{
		// if match is found, break
		if (strcmp(month, months[i].name) == 0 ||
			strcmp(month, months[i].abrev) == 0 ||
			strcmp(month, months[i].monthno) == 0)
			break;
	}

	// if no match for month found, exit
	if (i == MONTHS)
	{
		printf("No month matching %s found.\n", month);
		exit(1);
	}

	// check for leap year and adjust days in February if necessary
	if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0)
		months[1].days = 29;


	// check validity of date
	if (day > months[i].days)
	{
		printf("Invalid date: there is no date %d in %s %d.\n",
			   day, months[i].name, year);
		exit(1);
	}

	// get total days in year up to given date
	total = 0;
	for (int j = 0; j < i; j++)
		total += months[j].days;

	total += day;

	printf("There are %d days in %d through %s %d.\n",
		   total, year, months[i].name, day);

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

## 14-3
Revise the book-listing program in Listing 14.2 so that it prints the book descriptions in the order entered, then alphabetized by title, and then in order of increased value.
***
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>

#define MAXTITL 40
#define MAXAUTL 40
#define MAXBKS 100

struct book
{
	char title[MAXTITL];
	char author[MAXAUTL];
	float value;
};

char * get(char *, int);
void titlesort(struct book **books, int count);
void valuesort(struct book **books, int count);

int main(void)
{
	struct book *library[MAXBKS];
	char temp[MAXTITL];
	int count = 0;
	int index;

	printf("Please enter the book title.\n");
	printf("Press [ENTER] at the start of a line to stop.\n");
	while (count < MAXBKS && get(temp, MAXTITL) != NULL
		   && temp[0] != '\0')
	{
		library[count] = (struct book *) malloc(sizeof(struct book));
		strncpy(library[count]->title, temp, MAXTITL);

		printf("Now enter the author.\n");
		get(library[count]->author, MAXAUTL);

		printf("Now enter the value.\n");
		scanf("%f", &library[count]->value);

		while (getchar() != '\n')
			continue;

		if (count < MAXBKS)
			printf("Enter the next title.\n");

		count++;
	}

	if (count > 0)
	{
		printf("Here is the list of your books in the original order:\n");
		for (int i = 0; i < count; i++)
			printf("%s by %s: $%.2f\n", library[i]->title, library[i]->author,
				   library[i]->value);
		puts("");

		// sorted by title
		printf("Here is the list of your books alphabetized by title:\n");
		titlesort(library, count);
		for (int i = 0; i < count; i++)
			printf("%s by %s: $%.2f\n", library[i]->title, library[i]->author,
				   library[i]->value);
		puts("");

		// sorted by value
		printf("Here is the list of your books ordered by value:\n");
		valuesort(library, count);
		for (int i = 0; i < count; i++)
			printf("%s by %s: $%.2f\n", library[i]->title, library[i]->author,
				   library[i]->value);
	}
	else
		printf("No books? Too bad.\n");

	for (int i = 0; i < count; i++)
		free(library[count]);

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

void titlesort(struct book **books, int count)
{
	struct book *temp;
	for (int i = 0; i < count - 1; i++)
	{
		for (int j = i + 1; j < count; j++)
		{
			if (tolower(books[i]->title[0]) > tolower(books[j]->title[0]))
			{
				temp = books[i];
				books[i] = books[j];
				books[j] = temp;
			}
		}
	}
}

void valuesort(struct book **books, int count)
{
	struct book *temp;
	for (int i = 0; i < count - 1; i++)
	{
		for (int j = i + 1; j < count; j++)
		{
			if (books[i]->value > books[j]->value)
			{
				temp = books[i];
				books[i] = books[j];
				books[j] = temp;
			}
		}
	}
}
```

## 14-4
Write a program that creates a structure template with two members according to the following criteria:
a. The first member is a social security number. The second member is a structure with three members. Its first member contains a first name, its second member contains a middle name, and its final member contains a last name. Create and initialize an array of five such structures. Have the program print the data in this format:

    Dribble, Flossie M. –– 302039823

Only the initial letter of the middle name is printed, and a period is added. Neither the initial (of course) nor the period should be printed if the middle name member is empty. Write a function to do the printing; pass the structure array to the function.

b. Modify part a. by passing the structure value instead of the address.

***
```c
#include <stdio.h>
#include <ctype.h>

#define LEN 5
#define MAXNAME 20

struct Name
{
	char first[MAXNAME];
	char middle[MAXNAME];
	char last[MAXNAME];
};

struct Person
{
	int ssn;
	struct Name name;
};

void printpersona(struct Person *); // pass struct by address
void printpersonb(struct Person); // pass struct by value

int main(void)
{
	struct Person people[LEN] = {
		{123456789, {"Marvin", "The", "Martian"}},
		{987654321, {"Scrooge", "Mc", "Duck"}},
		{888777666, {"Mantis", "Froggy", "Tobogan"}},
		{123432467, {.first="Night", .last="Man"}},
		{354257623, {.first="Day", .last="Man"}}
	};

	// part a -- pass by address
	for (int i = 0; i < LEN; i++)
		printpersona(&people[i]);
	puts("");

	// part b -- pass by value
	for (int i = 0; i < LEN; i++)
		printpersonb(people[i]);

	return 0;
}


void printpersona(struct Person *person)
{
	printf("%s, %s ", person->name.last, person->name.first);
	if (person->name.middle[0] != '\0')
		printf("%c. ", toupper(person->name.middle[0]));
	printf("--- %d\n", person->ssn);
}

void printpersonb(struct Person person)
{
	printf("%s, %s ", person.name.last, person.name.first);
	if (person.name.middle[0] != '\0')
		printf("%c. ", toupper(person.name.middle[0]));
	printf("--- %d\n", person.ssn);
}
```


## 14-5
Write a program that fits the following recipe:
a. Externally define a name structure template with two members: a string to hold the first name and a string to hold the second name.
b. Externally define a student structure template with three members: a name structure, a grade array to hold three floating-point scores, and a variable to hold the average of those three scores.
c. Have the main() function declare an array of CSIZE (with CSIZE = 4) student structures and initialize the name portions to names of your choice. Use functions to perform the tasks described in parts d., e., f., and g.
d. Interactively acquire scores for each student by prompting the user with a student name and a request for scores. Place the scores in the grade array portion of the appropriate structure. The required looping can be done in main() or in the function, as you prefer.
e. Calculate the average score value for each structure and assign it to the proper member.
f. Print the information in each structure.
g. Print the class average for each of the numeric structure members.
***
```c
#include <stdio.h>

#define CSIZE 4
#define NAMELEN 20
#define GRADELEN 3

struct Name
{
	char first[NAMELEN];
	char last[NAMELEN];
};

struct Student
{
	struct Name name;
	float grades[GRADELEN];
	float average;
};

void getgrades(struct Student *);
void getaverage(struct Student *);
void showstudentinfo(struct Student *);

int main(void)
{
	struct Student students[CSIZE] = {
		{.name = {"Zack", "Morris"}},
		{.name = {"Kelly", "Kapowski"}},
		{.name = {"AC", "Slater"}},
		{.name = {"Screech", "Powers"}}
	};

	// get student grades
	for (int i = 0; i < CSIZE; i++)
		getgrades(&students[i]);

	// get grade averages
	for (int i = 0; i < CSIZE; i++)
		getaverage(&students[i]);

	// print student info
	for (int i = 0; i < CSIZE; i++)
		showstudentinfo(&students[i]);

	return 0;
}

void getgrades(struct Student * student)
{
	// interactively acquire and set grades for student

	for (int i = 0; i < GRADELEN; i++)
	{
		printf("Enter grade %d for student %s %s: ", i + 1, student->name.first,
			   student->name.last);
		while (scanf("%f", student->grades+i) != 1)
			while (getchar() != '\n') continue;
		while (getchar() != '\n') continue;
	}
	puts("");
}

void getaverage(struct Student * student)
{
	// calculate and set the average grade for a student

	float total = 0;
	for (int i = 0; i < GRADELEN; i++)
		total += student->grades[i];
	student->average = total / GRADELEN;
}

void showstudentinfo(struct Student * student)
{
	printf("Name: %s %s\n", student->name.first, student->name.last);
	printf("Grade 1: %.1f\n", student->grades[0]);
	printf("Grade 2: %.1f\n", student->grades[1]);
	printf("Grade 3: %.1f\n", student->grades[2]);
	printf("Average Grade: %.1f\n", student->average);
	puts("");
}
```

## 14-6
A text file holds information about a softball team. Each line has data arranged as follows:
	4 Jessie Joybat 5 2 1 1
The first item is the player’s number, conveniently in the range 0–18. The second item is the player’s first name, and the third is the player’s last name. Each name is a single word. The next item is the player’s official times at bat, followed by the number of hits, walks, and runs batted in (RBIs). The file may contain data for more than one game, so the same player may have more than one line of data, and there may be data for other players between those lines. Write a program that stores the data into an array of structures. The structure should have members to represent the first and last names, the at bats, hits, walks, and RBIs (runs batted in), and the batting average (to be calculated later). You can use the player number as an array index. The program should read to end- of-file, and it should keep cumulative totals for each player.

The world of baseball statistics is an involved one. For example, a walk or reaching base on an error doesn’t count as an at-bat but could possibly produce an RBI. But all this program has to do is read and process the data file, as described next, without worrying about how realistic the data is.

The simplest way for the program to proceed is to initialize the structure contents to zeros, read the file data into temporary variables, and then add them to the contents of the corresponding structure. After the program has finished reading the file, it should then calculate the batting average for each player and store it in the corresponding structure member. The batting average is calculated by dividing the cumulative number of hits for a player by the cumulative number of at-bats; it should be a floating-point calculation. The program should then display the cumulative data for each player along with a line showing the combined statistics for the entire team.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define NAMELEN 12
#define ROSTERSIZE 19
#define MXLINE 40

struct Player
{
	char first[NAMELEN];
	char last[NAMELEN];
	unsigned int atbats, hits, walks, rbis;
	float battingaverage;
};

void getbattingaverage(struct Player *);
void showteamdata(const struct Player *, int);

int main(void)
{
	// declare and initialize array of players
	struct Player players[ROSTERSIZE];
	for (int i = 0; i < ROSTERSIZE; i++)
		players[i] = (struct Player) {"", "", 0, 0, 0, 0, 0};

	FILE *fp;
	int number;
	char first[NAMELEN];
	char last[NAMELEN];
	unsigned int atbats;
	unsigned int hits;
	unsigned int walks;
	unsigned int rbis;

	if ((fp = fopen("roster.txt", "r")) == NULL)
	{
		fprintf(stderr, "Could not open file 'roster.txt'.\n");
		exit(EXIT_FAILURE);
	}

	// read data from roster.txt
	while (fscanf(fp, "%d %s %s %u %u %u %u", &number, first, last, &atbats,
		          &hits, &walks, &rbis) == 7)
	{
		if (players[number].first[0] == '\0')
		{
			strncpy(players[number].first, first, NAMELEN);
			strncpy(players[number].last, last, NAMELEN);
		}
		players[number].atbats += atbats;
		players[number].hits += hits;
		players[number].walks += walks;
		players[number].rbis += rbis;
	}

	// calculate batting averages
	for (int i = 0; i < ROSTERSIZE; i++)
		getbattingaverage(&players[i]);

	// print player data
	showteamdata(players, ROSTERSIZE);

	if (fclose(fp) != 0)
		fprintf(stderr,"Error closing file\n");

	return 0;
}

void getbattingaverage(struct Player * player)
{
	player->battingaverage = player->hits / (float) player->atbats;
}

void showteamdata(const struct Player * player, int size)
{
	unsigned int atbats = 0, hits = 0, walks = 0, rbis = 0;

	printf("Team statistics (number, first, last, at bats, hits, walks,"
		   " rbis, average):\n");
	for (int i = 0; i < size; i++, player++)
	{
		printf("%2d ", i);
		printf("%*s %*s %u %u %u %u %.3f\n", NAMELEN, player->first, NAMELEN,
		       player->last, player->atbats, player->hits, player->walks,
		       player->rbis, player->battingaverage);

		atbats += player->atbats;
		hits += player->hits;
		walks += player->walks,
		rbis += player->rbis;
	}
	printf("Combined stats: %u %u %u %u %.3f\n", atbats, hits, walks, rbis,
		   hits / (float) atbats);
}
```

## 14-7
Modify Listing 14.14 so that as each record is read from the file and shown to you, you are given the chance to delete the record or to modify its contents. If you delete the record, use the vacated array position for the next record to be read. To allow changing the existing contents, you’ll need to use the "r+b" mode instead of the "a+b" mode, and you’ll have to pay more attention to positioning the file pointer so that appended records don’t overwrite existing records. It’s simplest to make all changes in the data stored in program memory and then write the final set of information to the file. One approach to keeping track is to add a member to the book structure that indicates whether it is to be deleted.
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAXTITL 40
#define MAXAUTL 40
#define MAXBKS 10

char * get(char *, int);

struct book
{
	char title[MAXTITL];
	char author[MAXAUTL];
	float value;
};

void editbook(struct book *);

int main(void)
{
	struct book library[MAXBKS];
	int count = 0;
	int index, filecount, ch;
	FILE * pbooks;
	int size = sizeof(struct book);

	if ((pbooks = fopen("book.dat", "rb")) == NULL)
	{
		fprintf(stderr, "Could not open file 'book.dat'.\n");
		exit(EXIT_FAILURE);
	}

	printf("Current contents of book.dat:\n");
	while (count < MAXBKS && fread(&library[count], size, 1, pbooks) == 1)
	{
		printf("%s by %s: %.2f\n", library[count].title,
			   library[count].author, library[count].value);

		printf("Please select an action:\n"
			   "[n] next record [d] delete this record "
			   "[m] modify this record.\n");
		ch = getchar();
		while (getchar() != '\n') continue;
		switch (ch)
		{
			case ('n'):
			case ('N'):
				break;
			case ('m'):
			case('M'):
				editbook(&library[count]);
				break;
			case('d'):
			case('D'):
				count--;
				break;
			default:
				break;
		}
		count++;
	}

	fclose(pbooks);

	if (count == MAXBKS)
		printf("The file 'books.dat' is full.\n");
	else
	{
		// if 'books.dat' is not at capacity, prompt user for more books

		puts("Please add new book titles.");
		puts("Press [enter] at the start of a line to stop.");
		while (count < MAXBKS && get(library[count].title, MAXTITL) != NULL
			   && library[count].title[0] != '\0')
		{
			puts("Now enter the author.");
			get(library[count].author, MAXAUTL);
			puts("Now enter the value.");
			scanf("%f", &library[count].value);
			while (getchar() != '\n') continue;
			count++;

			if (count < MAXBKS)
				puts("Enter the next title.");
		}
	}

	if ((pbooks = fopen("book.dat", "wb")) == NULL)
	{
		fprintf(stderr, "Could not open file 'book.dat' for write.\n");
		exit(EXIT_FAILURE);
	}

	if (count > 0)
	{
		puts("Here is your list of books:");
		for (index = 0; index < count; index++)
			printf("%s by %s: %.2f\n", library[index].title,
				   library[index].author, library[index].value);
		fwrite(library, size, count, pbooks);
	}
	else
	{
		puts("No books? too bad.\n");
	}

	puts("Bye.\n");
	fclose(pbooks);

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

void editbook(struct book * bk)
{
	puts("Enter new title for book:");
	get(bk->title, MAXTITL);
	puts("Enter new author for book:");
	get(bk->author, MAXAUTL);
	puts("Enter new value for book:");
	scanf("%f", &bk->value);

	while (getchar() != '\n') continue;
}
```

## 14-8
The Colossus Airlines fleet consists of one plane with a seating capacity of 12. It makes one flight daily. Write a seating reservation program with the following features:

a. The program uses an array of 12 structures. Each structure should hold a seat identification number, a marker that indicates whether the seat is assigned, the last name of the seat holder, and the first name of the seat holder.

b. The program displays the following menu:
	To choose a function, enter its letter label: 
	a) Show number of empty seats
	b) Show list of empty seats
	c) Show alphabetical list of seats
	d) Assign a customer to a seat assignment 
    e) Delete a seat assignment
	f) Quit

c. The program successfully executes the promises of its menu. Choices d) and e) require additional input, and each should enable the user to abort an entry.

d. After executing a particular function, the program shows the menu again, except for choice f).

e. Data is saved in a file between runs. When the program is restarted, it first loads in the data, if any, from the file.
***
```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>

#define CAPACITY 12
#define MAXNAML 20

struct Seat
{
	int id;
	bool assigned;
	char last[MAXNAML];
	char first[MAXNAML];
};

int size = sizeof(struct Seat);

struct Seat seats[CAPACITY];

char * get(char *, int);

void printmenu(void);
void countempty(void);
void listempty(void);
void showalphabetical(void);
void assigncustomer(void);
void deleteassignment(void);

int main(void)
{
	FILE * fp;
	int ch = 0;
	int id;
	struct Seat tmp;

	for (int i = 0; i < CAPACITY; i++)
	{
		seats[i].id = i;
		seats[i].assigned = false;
	}

	if ((fp = fopen("seatassignments.dat", "rb")) == NULL)
	{
		fprintf(stderr, "Could not open file 'seatassignments.dat'.\n");
		exit(EXIT_FAILURE);
	}

	// read saved data into seats array
	while (fread(&tmp, size, 1, fp) == 1)
	{
		seats[tmp.id] = tmp;
	}

	// main program loop
	while (ch != 'f' && ch != 'F')
	{
		printmenu();
		if ((ch = getchar()) != '\n')
			while (getchar() != '\n') continue;
		switch (ch)
		{
			case ('a'):
			case ('A'):
				countempty();
				break;
			case ('b'):
			case ('B'):
				listempty();
				break;
			case ('c'):
			case ('C'):
				showalphabetical();
				break;
			case ('d'):
			case ('D'):
				assigncustomer();
				break;
			case ('e'):
			case ('E'):
				deleteassignment();
				break;
			case ('f'):
			case ('F'):
				break;
			default:
				puts("Invalid input. Try again.");
		}
		puts("");

	}

	fclose(fp);
	if ((fp = fopen("seatassignments.dat", "wb")) == NULL)
	{
		fprintf(stderr, "Could not open file 'seatassignments.dat' for write.\n");
		exit(EXIT_FAILURE);
	}

	if (fwrite(seats, size, CAPACITY, fp) != CAPACITY)
	{
		fprintf(stderr, "Error writing to file 'seatassignments.dat'.\n");
	}

	fclose(fp);
	puts("Bye.");

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

void printmenu(void)
{
	puts("To choose a function, enter its letter label:");
	puts("a) Show number of empty seats");
	puts("b) Show list of empty seats");
	puts("c) Show alphabetical list of seats");
	puts("d) Assign a customer to a seat assignment");
	puts("e) Delete a seat assignment");
	puts("f) Quit");
}

void countempty(void)
{
	int count = 0;
	for (int i = 0; i < CAPACITY; i++)
		if (!seats[i].assigned)
			count++;
	printf("%d\n", count);
}

void listempty(void)
{
	for (int i = 0; i < CAPACITY; i++)
		if (!seats[i].assigned)
			printf("Seat number %d is unassigned.\n", seats[i].id);
}

void showalphabetical(void)
{
	printf("%-*s  %-*s  %-12s\n", MAXNAML, "Last Name", MAXNAML, "First Name",
		   "Seat Number");
	for (int i = 0; i < CAPACITY; i++)
	{
		if (seats[i].assigned)
			printf("%-*s  %-*s  %-12d\n", MAXNAML, seats[i].last, MAXNAML,
				   seats[i].first, seats[i].id);
	}
}

void assigncustomer(void)
{
	int id, ch;
	struct Seat new;

	printf("Enter seat id number: ");
	scanf("%d", &id);
	while (getchar() != '\n') continue;
	if (id < 0 && id >= CAPACITY)
	{
		puts("This seat number is invalid. Aborting.");
		return;
	}
	if (seats[id].assigned)
	{
		puts("This seat is already assigned. Aborting.");
		return;
	}

	new.id = id;
	new.assigned = true;
	printf("Enter passenger first name: ");
	get(new.first, MAXNAML);
	printf("Enter passenger last name: ");
	get(new.last, MAXNAML);

	puts("Here is the overview for your new seat assignment:");
	printf("Seat number: %d  First name: %s  Last name: %s\n", new.id,
		   new.first, new.last);
	puts("Is this correct? Enter [n/N] to abort or any other key to continue.");
	
	if ((ch = getchar()) != '\n')
		while (getchar() != '\n') continue;
	if (ch == 'n' || ch == 'N')
	{
		puts("Aborting seat assignment.");
		return;
	}
	seats[id] = new;
	return;
}

void deleteassignment(void)
{
	int id, ch = 0;
	puts("Enter seat number of the assignment to delete: ");
	scanf("%d", &id);
	while (getchar() != '\n') continue;
	if (id < 0 && id >= CAPACITY)
	{
		puts("This seat number is invalid. Aborting.");
		return;
	}
	if (!seats[id].assigned)
	{
		puts("This seat is not currently assigned. Aborting.");
		return;
	}
	puts("Here is the seat assignment you have selected:");
	printf("Seat number: %d  First name: %s  Last name: %s\n", seats[id].id,
		   seats[id].first, seats[id].last);
	puts("Would you like to delete this assignment? Enter [n/N] to abort "
		 "or any other key to continue.");
	if ((ch = getchar()) != '\n')
		while (getchar() != '\n') continue;
	if (ch == 'n' || ch == 'N')
	{
		puts("Aborting seat assignment.");
		return;
	}
	seats[id].first[0] = '\0';
	seats[id].last[0] = '\0';
	seats[id].assigned = false;

	return;
}
```

## 14-9
Colossus Airlines (from exercise 8) acquires a second plane (same capacity) and expands its service to four flights daily (Flights 102, 311, 444, and 519). Expand the program to handle four flights. Have a top-level menu that offers a choice of flights and the option to quit. Selecting a particular flight should then bring up a menu similar to that of exercise 8. However, one new item should be added: confirming a seat assignment. Also, the quit choice should be replaced with the choice of exiting to the top-level menu. Each display should indicate which flight is currently being handled. Also, the seat assignment display should indicate the confirmation status.
***
```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>

#define FLIGHTS 4
#define CAPACITY 12
#define MAXNAML 20

struct Seat
{
	int id;
	bool assigned;
	bool confirmed;
	char last[MAXNAML];
	char first[MAXNAML];
};

struct Flight
{
	int id;
	struct Seat seats[CAPACITY];
};

struct Flight flights[FLIGHTS] = {
	{102, {}},
	{311, {}},
	{444, {}},
	{519, {}}
};

int flight_size = sizeof(struct Flight);

static struct Flight * flight;

char * get(char *, int);

void print_top_menu(void);
void flightmenu(void);
void printmenu(void);
void countempty(void);
void listempty(void);
void showalphabetical(void);
void assigncustomer(void);
void deleteassignment(void);
void confirmassignment(void);

int main(void)
{
	FILE * fp;
	int ch = 0, id;
	extern struct Flight * flight;
	struct Flight tmp;

	for (int i = 0; i < FLIGHTS; i++)
		for (int j = 0; j < CAPACITY; j++)
		{
			flights[i].seats[j].id = j;
			flights[i].seats[j].assigned = false;
		}

	if ((fp = fopen("Colossus.dat", "rb")) == NULL)
	{
		fprintf(stderr, "Could not open file 'Colossus.dat'.\n");
		exit(EXIT_FAILURE);
	}

	// read saved data into flights array
	while (fread(&tmp, flight_size, 1, fp) == 1)
	{
		int number = tmp.id;
		for (int i = 0; i < FLIGHTS; i++)
		{
			if (flights[i].id == number)
			{
				flights[i] = tmp;
				break;
			}
		}
	}

	// main program loop
	while (ch != 'e' && ch != 'E')
	{
		print_top_menu();
		if ((ch = getchar()) != '\n')
			while (getchar() != '\n') continue;
		puts("");
		switch (ch)
		{
			case ('a'):
			case ('A'):
				flight = &flights[0];
				break;
			case ('b'):
			case ('B'):
				flight = &flights[1];
				break;
			case ('c'):
			case ('C'):
				flight = &flights[2];
				break;
			case ('d'):
			case ('D'):
				flight = &flights[3];
				break;
			case ('e'):
			case ('E'):
				goto Exit;
			default:
				puts("Invalid input. Try again.");
				continue;
		}
		
		flightmenu();
	}

	Exit:
	fclose(fp);
	if ((fp = fopen("Colossus.dat", "wb")) == NULL)
	{
		fprintf(stderr, "Could not open file 'Colossus.dat' for write.\n");
		exit(EXIT_FAILURE);
	}

	if (fwrite(flights, flight_size, FLIGHTS, fp) != FLIGHTS)
	{
		fprintf(stderr, "Error writing to file 'Colossus.dat'.\n");
	}

	fclose(fp);
	puts("Bye.");

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

void print_top_menu(void)
{
	puts("To choose a flight, enter its letter label:");
	puts("a) 102");
	puts("b) 311");
	puts("c) 444");
	puts("d) 519");
	puts("e) Quit");

	return;
}

void flightmenu(void)
{
	int ch = 0;
	extern struct Flight * flight;

	while (ch != 'g' && ch != 'G')
	{	
		printf("You have selected flight number %d.\n", flight->id);
		puts("To choose a function, enter its letter label:");
		puts("a) Show number of empty seats");
		puts("b) Show list of empty seats");
		puts("c) Show alphabetical list of seats");
		puts("d) Assign a customer to a seat assignment");
		puts("e) Delete a seat assignment");
		puts("f) Confirm a seat assignment");
		puts("g) Back to main menu");

		if ((ch = getchar()) != '\n')
			while (getchar() != '\n') continue;
		puts("");
		switch (ch)
		{
			case ('a'):
			case ('A'):
				countempty();
				break;
			case ('b'):
			case ('B'):
				listempty();
				break;
			case ('c'):
			case ('C'):
				showalphabetical();
				break;
			case ('d'):
			case ('D'):
				assigncustomer();
				break;
			case ('e'):
			case ('E'):
				deleteassignment();
				break;
			case ('f'):
			case ('F'):
				confirmassignment();
				break;
			case ('g'):
			case ('G'):
				break;
			default:
				puts("Invalid input. Try again.");
		}
	}
	return;
}

void countempty(void)
{
	extern struct Flight * flight;

	// count empty seats on flight
	int count = 0;
	for (int i = 0; i < CAPACITY; i++)
		if (!flight->seats[i].assigned)
			count++;
	printf("There are %d empty seats on flight number %d.\n", count, flight->id);
}

void listempty(void)
{
	extern struct Flight * flight;
	// list empty seats
	printf("Here is a list of empty seats on flight number %d:\n", flight->id);
	for (int i = 0; i < CAPACITY; i++)
		if (!flight->seats[i].assigned)
			printf("Seat number %d is unassigned.\n", flight->seats[i].id);
}

void showalphabetical(void)
{
	extern struct Flight * flight;

	printf("Here is a list of seat assignments for flight number %d:\n", flight->id);
	printf("%-*s  %-*s  %-12s %-12s\n", MAXNAML, "Last Name", MAXNAML, "First Name",
		   "Seat Number", "Status");
	for (int i = 0; i < CAPACITY; i++)
	{
		if (flight->seats[i].assigned)
			printf("%-*s  %-*s  %-12d %-12s\n", MAXNAML, flight->seats[i].last, 
				   MAXNAML, flight->seats[i].first, flight->seats[i].id,
				   flight->seats[i].confirmed ? "Confirmed" : "Unconfirmed");
	}
}

void assigncustomer(void)
{
	int id, ch;
	struct Seat new;
	extern struct Flight * flight;

	printf("Enter seat id number: ");
	scanf("%d", &id);
	while (getchar() != '\n') continue;
	if (id < 0 && id >= CAPACITY)
	{
		puts("This seat number is invalid. Aborting.");
		return;
	}
	if (flight->seats[id].assigned)
	{
		puts("This seat is already assigned. Aborting.");
		return;
	}

	new.id = id;
	new.assigned = true;
	new.confirmed = false;
	printf("Enter passenger first name: ");
	get(new.first, MAXNAML);
	printf("Enter passenger last name: ");
	get(new.last, MAXNAML);

	puts("Here is the overview for your new seat assignment:");
	printf("Flight number: %d\nSeat number: %d\nFirst name: %s\n"
		   "Last name: %s\n", flight->id, new.id, new.first, new.last);
	puts("Is this correct? Enter [n/N] to abort or any other key to continue.");
	
	if ((ch = getchar()) != '\n')
		while (getchar() != '\n') continue;
	if (ch == 'n' || ch == 'N')
	{
		puts("Aborting seat assignment.");
		return;
	}
	flight->seats[id] = new;
	return;
}

void deleteassignment(void)
{
	int id, ch = 0;
	extern struct Flight * flight;

	puts("Enter seat number of the assignment to delete: ");
	scanf("%d", &id);
	while (getchar() != '\n') continue;
	if (id < 0 && id >= CAPACITY)
	{
		puts("This seat number is invalid. Aborting.");
		return;
	}
	if (!flight->seats[id].assigned)
	{
		puts("This seat is not currently assigned. Aborting.");
		return;
	}
	puts("Here is the seat assignment you have selected:");
	printf("Flight number: %d\nSeat number: %d\nFirst name: %s\n"
		   "Last name: %s\n", flight->id, flight->seats[id].id,
		   flight->seats[id].first, flight->seats[id].last);
	puts("Would you like to delete this assignment? Enter [n/N] to abort "
		 "or any other key to continue.");
	if ((ch = getchar()) != '\n')
		while (getchar() != '\n') continue;
	if (ch == 'n' || ch == 'N')
	{
		puts("Aborting seat assignment.");
		return;
	}
	flight->seats[id].first[0] = '\0';
	flight->seats[id].last[0] = '\0';
	flight->seats[id].assigned = false;
	flight->seats[id].confirmed = false;

	return;
}

void confirmassignment(void)
{
	int id;
	extern struct Flight * flight;

	printf("Enter seat id number to confirm: ");
	scanf("%d", &id);
	while (getchar() != '\n') continue;
	if (id < 0 && id >= CAPACITY)
	{
		puts("This seat number is invalid. Aborting.");
	}
	else if (!flight->seats[id].assigned)
	{
		puts("There is no current assignment for this seat. Aborting.");
	}
	else if (flight->seats[id].confirmed)
	{
		puts("This seat assignment is already confirmed.");
	}
	else
	{
		flight->seats[id].confirmed = true;
		puts("Your assignment has been confirmed.");
	}
	return;
}
```

## 14-10
Write a program that implements a menu by using an array of pointers to functions. For instance, choosing a from the menu would activate the function pointed to by the first element of the array.
***
```c
#include <stdio.h>
#include <math.h>
#define NOPTIONS 5

void menu(void);
double sum(double, double);
double difference(double, double);
double power(double, double);
double product(double, double);
double quotient(double, double);

int main(void)
{
	// declare array of function pointers
	double (*funcs[NOPTIONS])(double, double) = {sum, difference, product, quotient,
											     power};
	double x, y;
	int ch;

	
	while (1)
	{
		menu();
		if ((ch = getchar()) != '\n')
			while (getchar() != '\n') continue;
		ch -= 'a';
		if (ch < 0 || ch > 5)
		{
			puts("This is not a valid option. Try again.");
			continue;
		}
		else if (ch == 5)
			break;
		
		printf("Enter two numbers: ");
		while(scanf("%lf %lf", &x, &y) != 2)
		{
			while (getchar() != '\n') continue;
			printf("Invalid input. Enter two numbers: ");
		}
		while (getchar() != '\n') continue;

		printf("%f\n", funcs[ch](x, y));
		puts("");
	}
	puts("Bye.");
}

void menu(void)
{
	puts("Choose an operation:");
	puts("a) add");
	puts("b) subtract");
	puts("c) multiply");
	puts("d) divide");
	puts("e) power");
	puts("f) quit");
	return;
}

double sum(double x, double y)
{
	return x + y;
}

double difference(double x, double y)
{
	return x - y;
}

double power(double x, double y)
{
	return pow(x, y);
}

double product(double x, double y)
{
	return x * y;
}

double quotient(double x, double y)
{
	return x / y;
}
```

## 14-11
Write a function called transform() that takes four arguments: the name of a source array containing type double data, the name of a target array of type double, an int representing the number of array elements, and the name of a function (or, equivalently, a pointer to a function). The transform() function should apply the indicated function to each element in the source array, placing the return value in the target array. For example, the call transform(source, target, 100, sin); would set target[0] to sin(source[0]), and so on, for 100 elements. Test the function in a program that calls transform() four times, using two functions from the math.h library and two suitable functions of your own devising as arguments to successive calls of the transform() function.
***
```c
#include <stdio.h>
#include <math.h>

#define ARRLENGTH 20

void transform(const double * source, double * target, int n, double (*func)(double));
double squared(double);
double cubed(double);

int main(void)
{
	// test transform function 
	
	double source[ARRLENGTH];
	double target[ARRLENGTH];

	for (int i = 0; i < ARRLENGTH; i++)
		source[i] = i;

	transform(source, target, ARRLENGTH, sin);
	for (int i = 0; i < ARRLENGTH; i++)
	{
		printf("sin(%.2f) = %.2f\n", source[i], target[i]);
	}
	puts("");

	transform(source, target, ARRLENGTH, tan);
	for (int i = 0; i < ARRLENGTH; i++)
	{
		printf("tan(%.2f) = %.2f\n", source[i], target[i]);
	}
	puts("");

	transform(source, target, ARRLENGTH, squared);
	for (int i = 0; i < ARRLENGTH; i++)
	{
		printf("%.2f ^ 2 = %.2f\n", source[i], target[i]);
	}
	puts("");

	transform(source, target, ARRLENGTH, cubed);
	for (int i = 0; i < ARRLENGTH; i++)
	{
		printf("%.2f ^ 3 = %.2f\n", source[i], target[i]);
	}
	puts("");

	return 0;
}

void transform(const double * source, double * target, int n, double (*func)(double))
{
	for (int i = 0; i < n; i++)
		target[i] = func(source[i]);

	return;
}

double squared(double x)
{
	return x * x;
}

double cubed(double x)
{
	return x * x * x;
}
```

## 14-roster.txt
```
4 Jessie Joybat 5 2 1 1
10 Wade Boggs 5 3 4 1
0 Sleepy Dwarf 4 1 0 0
1 Grumpy Dwarf 3 2 1 1
2 Dopey Dwarf 6 4 0 2
10 Wade Boggs 3 2 0 1
3 Snow White 2 2 0 1
5 Pecorino Romano 4 0 1 0
6 Parmigiano Reggiano 2 1 0 0
```