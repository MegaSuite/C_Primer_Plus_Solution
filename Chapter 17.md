# Chapter 17
## 17-binary_search
***
```c
#include "binary_search.h"

static int binarysearch_aux(int * arr, int target, int low, int high);

int binarysearch(int * arr, int arr_size, int target)
{
	return binarysearch_aux(arr, target, 0, arr_size - 1);
}


static int binarysearch_aux(int * arr, int target, int low, int high)
{
	// base case
	if (low == high)
		return arr[low] == target ? 1 : 0;

	// recursion case
	else
	{
		int mid = (low + high) / 2;
		if (arr[mid] < target)
			return binarysearch_aux(arr, target, mid + 1, high);
		else
			return binarysearch_aux(arr, target, low, mid);
	}
}
```

## 17-binary_search.h
***
```c
#ifndef BINARY_SEARCH_H_
#define BINARY_SEARCH_H_ 1

int binarysearch(int * arr, int arr_size, int target);

#endif
```

## 17-1
Modify Listing 17.2 so that it displays the movie list both in the original order and in reverse order. One approach is to modify the linked-list definition so that the list can be traversed in both directions. Another approach is to use recursion.

compile with film.c
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "film.h"

char * get(char * string, int n);

int main(void)
{
	List film_list = NULL;
	Film * current;
	char input[TSIZE];

	puts("Enter first movie title:");
	while (get(input, TSIZE) != NULL && input[0] != '\0')
	{
		current = (Film *) malloc(sizeof(Film));
		if (current == NULL)
		{
			fprintf(stderr, "Could not allocate memory.\n");
			exit(EXIT_FAILURE);
		}

		current->next = NULL;
		strcpy(current->title, input);
		printf("Enter your rating (0 - 10): ");
		scanf("%d", &(current->rating));
		while (getchar() != '\n') continue;

		if (film_list == NULL)
			film_list = current;
		else
			add_film(film_list, current);

		puts("Enter next movie title (empty line to stop):");
	}

	// show list of movies
	puts("Here is your list of movies:");
	list_films(film_list);
	puts("");

	// show list of movies in reverse order
	puts("Here is your list of movies in reverse order:");
	reverse_list_films(film_list);

	// clean up
	delete_list(&film_list);

}

char * get(char * string, int n)
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

## 17-2
Suppose list.h (Listing 17.3) uses the following definition of a list:
	typedef struct list {
		Node * head; /* points to head of list */
		Node * end; /* points to end of list */
	} List;
Rewrite the list.c (Listing 17.5) functions to fit this definition and test the resulting code with the films3.c (Listing 17.4) program.

Compile with list.c
***
```c
#include <stdio.h>
#include <stdlib.h>
#include "list.h"

void showmovies(Item item);
char * get(char * st, int n);
int main(void)
{
	List movies;
	Item temp;

	/* initialize */
	InitializeList(&movies);

	if (ListIsFull(&movies))
	{
		fprintf(stderr,"No memory available! Bye!\n");
		exit(1);
	}

	/* gather and store */
	puts("Enter first movie title:");
	while (get(temp.title, TSIZE) != NULL && temp.title[0] != '\0')
	{
		puts("Enter your rating <0-10>:");
		scanf("%d", &temp.rating);
		while(getchar() != '\n')
			continue;
		
		if (AddItem(temp, &movies)==false) 
		{
			fprintf(stderr,"Problem allocating memory\n");
			break;
		}

		if (ListIsFull(&movies))
		{
			puts("The list is now full.");
			break;
		}

		puts("Enter next movie title (empty line to stop):"); 
	}
	
	/* display */
	if (ListIsEmpty(&movies))
		printf("No data entered. ");
	else
	{
		printf ("Here is the movie list:\n");
		Traverse(&movies, showmovies);
	}

	printf("You entered %d movies.\n", ListItemCount(&movies));

	/* clean up */
	EmptyTheList(&movies);

	printf("Bye!\n");
	return 0;
}

void showmovies(Item item)
{
	printf("Movie: %s Rating: %d\n", item.title, item.rating);
}

char * get(char * string, int n)
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

## 17-3
Suppose list.h (Listing 17.3) uses the following definition of a list:

	#define MAXSIZE 100
	typedef struct list {
		Item entries[MAXSIZE];		/* array of items */ 
		int items; 					/* number of items in list */
	} List;

Rewrite the list.c (Listing 17.5) functions to fit this definition and test the resulting code with the films3.c (Listing 17.4) program.

compile with list2.c
***
```c
#include <stdio.h>
#include <stdlib.h>
#include "list2.h"

void showmovies(Item item);
char * get(char * st, int n);
int main(void)
{
	List movies;
	Item temp;

	/* initialize */
	InitializeList(&movies);

	if (ListIsFull(&movies))
	{
		fprintf(stderr,"No memory available! Bye!\n");
		exit(1);
	}

	/* gather and store */
	puts("Enter first movie title:");
	while (get(temp.title, TSIZE) != NULL && temp.title[0] != '\0')
	{
		puts("Enter your rating <0-10>:");
		scanf("%d", &temp.rating);
		while(getchar() != '\n')
			continue;
		
		if (AddItem(temp, &movies)==false) 
		{
			fprintf(stderr,"Problem allocating memory\n");
			break;
		}

		if (ListIsFull(&movies))
		{
			puts("The list is now full.");
			break;
		}

		puts("Enter next movie title (empty line to stop):"); 
	}
	
	/* display */
	if (ListIsEmpty(&movies))
		printf("No data entered. ");
	else
	{
		printf ("Here is the movie list:\n");
		Traverse(&movies, showmovies);
	}

	printf("You entered %d movies.\n", ListItemCount(&movies));

	/* clean up */
	EmptyTheList(&movies);

	printf("Bye!\n");
	return 0;
}

void showmovies(Item item)
{
	printf("Movie: %s Rating: %d\n", item.title, item.rating);
}

char * get(char * string, int n)
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


## 17-4
Rewrite mall.c (Listing 17.7) so that it simulates a double booth having two queues.

mall.c -- use the Queue interface // compile with queue.c
***
```c
#include <stdio.h>
#include <stdlib.h> 				// for rand() and srand()
#include <time.h>					// for time()
#include "queue.h"					// change Item typedef

#define MIN_PER_HR 60.0

bool newcustomer(double x); 		// is there a new customer?
Item customertime(long when);		// set customer parameters

int main(void) {
	Queue line1, line2;
	Item temp;						// new customer data
	int hours;						// hours of simulation
	int perhour;					// average # of arrivals per hour 
	long cycle, cyclelimit; 		// loop counter, limit
	long turnaways = 0; 			// turned away by full queue
	long customers = 0;				// joined the queue
	long served1 = 0;				// served in line 1 during the simulation
	long served2 = 0;				// served in line 2 during the simulation
	long sum_line1 = 0;				// cumulative line 1 length
	long sum_line2 = 0;				// cumulative line 2 length
	int wait_time1 = 0;				// time until Sigmund is free
	int wait_time2 = 0;				// time until Sigmund 2 is free
	double min_per_cust;			// average time between arrivals 
	long line_wait1 = 0;			// cumulative time in line 1
	long line_wait2 = 0;			// cumulative time in line 2
 
	InitializeQueue(&line1);
	InitializeQueue(&line2);
	srand((unsigned int) time(0)); 	// random initializing of rand()

	puts("Case Study: Sigmund Lander's Advice Booth");
	puts("Enter the number of simulation hours:");
	scanf("%d", &hours);
	cyclelimit = MIN_PER_HR * hours;
	puts("Enter the average number of customers per hour:");
	scanf("%d", &perhour);
	min_per_cust = MIN_PER_HR / perhour;

	for (cycle = 0; cycle < cyclelimit; cycle++) {
		if (newcustomer(min_per_cust)) {
			if (QueueIsFull(&line1) & QueueIsFull(&line2)) {
				turnaways++;
			}
			else {
				// add customer to shorter queue
				temp = customertime(cycle);
				if (QueueItemCount(&line1) <= QueueItemCount(&line2)) {
					EnQueue(temp, &line1);
				}
				else {
					EnQueue(temp, &line2);
				}
				customers++;
			}
		}
		if (wait_time1 <= 0 && !QueueIsEmpty(&line1)) {
			DeQueue (&temp, &line1);
			wait_time1 = temp.processtime;
			line_wait1 += cycle - temp.arrive;
			served1++;
		}
		if (wait_time2 <= 0 && !QueueIsEmpty(&line2)) {
			DeQueue (&temp, &line2);
			wait_time2 = temp.processtime;
			line_wait2 += cycle - temp.arrive;
			served2++;
		}
		if (wait_time1 > 0) {
			wait_time1--;
		}
		if (wait_time2 > 0) {
			wait_time2--;
		}
		sum_line1 += QueueItemCount(&line1);
		sum_line2 += QueueItemCount(&line2);
	}

	if (customers > 0) {
		printf("customers accepted: %ld\n", customers);
		printf("total customers served: %ld\n", served1 + served2);
		printf("turnaways: %ld\n", turnaways);
		printf("line 1 customers served: %ld\n", served1);
		printf("line 2 customers served: %ld\n", served2);
		printf("average queue size for line 1: %.2f\n", (double) sum_line1 / cyclelimit);
		printf("average queue size for line 2: %.2f\n", (double) sum_line2 / cyclelimit);
		printf("line 1 average wait time: %.2f minutes\n", (double) line_wait1 / served1);
		printf("line 2 average wait time: %.2f minutes\n", (double) line_wait2 / served2);
	}
	else {
		puts("No customers!");
	}
	
	EmptyTheQueue(&line1);
	EmptyTheQueue(&line2);
	puts("Bye!");

	return 0;
}


// x = average time, in minutes, between customers
// return value is true if customer shows up this minute
bool newcustomer(double x) {
	if (rand() * x / RAND_MAX < 1) {
		return true;
	}
	else {
		return false;
	}
}

// when is the time at which the customer arrives
// function returns an Item structure with the arrival time
// set to when and the processing time set to a random value
// in the range 1 - 3
Item customertime(long when) {
	Item cust;
	cust.processtime = rand() % 3 + 1;
	cust.arrive = when;
	return cust;
}
```


## 17-5
Write a program that lets you input a string. The program then should push the characters of the string onto a stack, one by one (see review question 5), and then pop the characters from the stack and display them. This results in displaying the string in reverse order.
***
```c
#include <stdio.h>
#include "stack.h"

int main(int argc, char *argv[])
{
	if (argc != 2)
	{
		printf("Usage: %s <string>\n", argv[0]);
		return 1;
	}

	Stack character_stack;
	Stack * stack_ptr = InitializeStack(&character_stack);

	// for (char * pchar = argv[1]; *pchar != '\0'; pchar++)
	// 	printf("%c", *pchar);

	for (char * pchar = argv[1]; *pchar != '\0'; pchar++)
		PushItem(pchar, stack_ptr);

	char pch[1];
	while (PopItem(pch, stack_ptr))
		printf("%c", *pch);

	puts("");

}
```

## 17-6
Write a function that takes three arguments: the name of an array of sorted integers, the number of elements of the array, and an integer to seek. The function returns the value 1 if the integer is in the array, and 0 if it isn’t. Have the function use the binary search technique.

compile with binary_search.c

***
```c
#include <stdio.h>
#include "binary_search.h"

int main(void)
{
	// binarysearch()

	int arr[10] = {1, 4, 6, 9, 11, 12, 15, 19, 25, 40};

	for (int i = 0; i < 15; i++)
	{
		printf("%d in array? %s\n", i, binarysearch(arr, 10, i) ? "yes" : "no");
	}
}
```


## 17-7
Write a program that opens and reads a text file and records how many times each word occurs in the file. Use a binary search tree modified to store both a word and the number of times it occurs. After the program has read the file, it should offer a menu with three choices. The first is to list all the words along with the number of occurrences. The second is to let you enter a word, with the program reporting how many times the word occurred in the file. The third choice is to quit.

compile with tree.c
***
```c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>
#include "tree.h"

void printMenu(void);
int getOption(void);
void printWordCount(Item item);
char * toLowercase(char * string);
char * get(char * string, int n);

int main(int argc, char *argv[]) {
	
	if (argc != 2) {
		fprintf(stderr, "Usage: %s <filename>\n", argv[0]);
		exit(1);
	}

	FILE * fp = fopen(argv[1], "r");
	if (fp == NULL) {
		fprintf(stderr, "Could not open file '%s'.\n", argv[1]);
		exit(1);
	}

	char tmp[WORD_SIZE];
	Tree words;
	Tree * ptree = InitializeTree(&words);
	while (fscanf(fp, "%s", tmp) == 1) {
		toLowercase(tmp);
		if (!AddWord(tmp, ptree)) {
			fprintf(stderr, "Could not add word to search tree.\n");
			exit(1);
		}
	}

	int ch;
	char word[WORD_SIZE];
	while ((ch = getOption()) != 'q') {
		if (ch == 'a') {
			ApplyToAll(ptree, printWordCount);
		} else if (ch == 'b') {
			printf("Enter a word to search for: ");
			get(word, WORD_SIZE);
			ApplyToNode(word, ptree, printWordCount);
		}
		puts("");
	}
	puts("Bye.");
}

void printMenu(void) {
	puts("Options:");
	puts("a) List all words with their respective counts");
	puts("b) Search for a word");
	puts("q) Quit");
	printf("Select an option: ");
}

int getOption(void) {
	printMenu();
	int opt;
	while ((opt = getchar()) != 'a' && opt != 'b' && opt != 'q') {
		puts("That is not a valid option. Try again.");
		printf("Select an option: ");
		while (getchar() != '\n')
			continue;
	}
	while (getchar() != '\n')
		continue;
	return opt;
}

void printWordCount(Item item) {
	printf("%s: %d\n", item.word, item.count);
}

char * toLowercase(char * string) {
	char * retval = string;
	for (; *string != '\0'; string++) {
		*string = tolower(*string);
	}
	return retval;
}

char * get(char * string, int n) {
	// wrapper for fgets - read from stdin and replace
	// first newline with null character

	char * retval = fgets(string, n, stdin);

	for (; *string != '\0'; string++) {
		if (*string == '\n') {
			*string = '\0';
			break;
		}
	}
	return retval;
}
```


## 17-8
Modify the Pet Club program so that all pets with the same name are stored in a list in the same node. When the user chooses to find a pet, the program should request the pet name and then list all pets (along with their kinds) having that name.

compile with pettree.c
***
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>
#include "pettree.h"

char menu(void);
void addpet(Tree * pt);
void printpet(Pet pet);
void droppet(Tree * pt);
void showpets(const Tree * pt);
void findpet(const Tree * pt);
void uppercase(char * str);
char * get(char * st, int n);

int main(void) {
	Tree pets;
	char choice;
	InitializeTree(&pets);

	while ((choice = menu()) != 'q') {
		puts("");
		if (choice == 'a') {
			addpet(&pets);
		} else if (choice == 'l') {
			showpets(&pets);
		} else if (choice == 'f') {
			findpet(&pets);
		} else if (choice == 'n') {
			printf("%d pets in club\n", TreeItemCount(pets));
		} else if (choice == 'd') {
			droppet(&pets);
		} else {
			printf("Not a valid option.\n");
		}
		puts("");
	}
	DeleteAll(&pets);
	puts("Bye.");

	return 0;
}

char menu(void) {
	int ch;

	puts("Nerfville Pet Club Membership Program");
	puts("Enter the letter corresponding to your choice:");
	puts("a) add a pet 			l) show list of pets");
	puts("n) number of pets		f) find pets");
	puts("d) delete a pet 		q) quit");
	while ((ch = getchar()) != EOF) {
		while (getchar() != '\n')
			continue;
		ch = tolower(ch);
		if (strchr("alfndq", ch) == NULL)
			puts("Please enter an a, l, f, n, d or q:");
		else
			break;
	}
	if (ch == EOF)
		ch = 'q';

	return ch;
}

void addpet(Tree * pt) {
	Pet * ppet = (Pet *) malloc(sizeof(Pet));
	if (ppet == NULL) {
		fprintf(stderr, "Could not allocate memory.\n");
		return;
	}

	if (TreeIsFull(pt)) {
		puts("No room in the club!");
	}
	else {
		puts("Please enter name of pet:");
		get(ppet->name, STRLEN);
		puts("Please enter pet kind:");
		get(ppet->type, STRLEN);
		uppercase(ppet->name);
		uppercase(ppet->type);
		ppet->next = NULL;
		AddPet(ppet, pt);
	}
}

void droppet(Tree * pt) {
	Pet temp;

	if (TreeIsEmpty(pt)) {
		printf("No entries!\n");
		return;
	}

	puts("Please enter name of pet you wish to delete:");
	get(temp.name, STRLEN);
	puts("Please enter type of pet:");
	get(temp.type, STRLEN);
	uppercase(temp.name);
	uppercase(temp.type);
	printf("%s the %s ", temp.name, temp.type);
	if (DeletePet(&temp, pt))
		printf("is dropped from the club.\n");
	else
		printf("is not a member.\n");
}

void showpets(const Tree * pt) {
	if (TreeIsEmpty(pt))
		puts("No entries!");
	else
		TraverseTree(pt, printpet);
}

void printpet(Pet pet) {
	printf("Pet: %-19s Kind: %-19s\n", pet.name, pet.type);
}

void findpet(const Tree * pt) {
	Pet temp;

	if (TreeIsEmpty(pt)) {
		puts("No entries.");
		return;
	}

	puts("Please enter name of pet you wish to find:");
	get(temp.name, STRLEN);
	uppercase(temp.name);
	printf("Here are all pets named %s:\n", temp.name);
	ApplyToNode(&temp, pt, printpet);
}

void uppercase(char * str) {
	for (; *str != '\0'; str++)
		*str = toupper(*str);
}

char * get(char * string, int n) {
	// wrapper for fgets - read from stdin and replace
	// first newline with null character

	char * retval = fgets(string, n, stdin);

	for (; *string != '\0'; string++) {
		if (*string == '\n') {
			*string = '\0';
			break;
		}
	}
	return retval;
}
```


## 17-film
***
```c
#include "film.h"

// delete the entire film list and free allocated memory
void delete_list(List * list_ptr)
{

	Film * tmp, * list = *list_ptr;
	while (list != NULL)
	{
		tmp = list->next;
		free(list);
		list = tmp;
	}

	*list_ptr = NULL;
}

// add a film to the list;
void add_film(List filmlist, Film * new_film)
{
	Film * film_ptr = filmlist;
	while (film_ptr->next != NULL)
		film_ptr = film_ptr->next;
	film_ptr->next = new_film;
}

// print the list of films in original order
void list_films(List film_ptr)
{
	while (film_ptr != NULL)
	{
		printf("%s: %d\n", film_ptr->title, film_ptr->rating);
		film_ptr = film_ptr->next;
	}
}

// print the list of films in reverse order
void reverse_list_films(List film_ptr)
{
	if (film_ptr == NULL)
		return;
	else
	{
		reverse_list_films(film_ptr->next);
		printf("%s: %d\n", film_ptr->title, film_ptr->rating);
	}
}
```

## 17-film.h
***
```c
#ifndef FILM_H_
#define FILM_H_ 1

#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>

#define TSIZE 45

typedef struct film 
{
	char title[TSIZE];
	int rating;
	struct film * next;
} Film;

typedef Film * List;

// delete the entire film list and free allocated memory
void delete_list(List * list);

// add a film to the list;
void add_film(List filmlist, Film * item);

// print the list of films in original order
void list_films(List filmlist);

// print the list of films in reverse order
void reverse_list_films(List filmlist);

#endif
```


## 17-list
***
```c
#include <stdio.h>
#include <stdlib.h>
#include "list.h"

/* local function prototype */
static void CopyToNode(Item item, Node * pnode);

/* interface functions */

/* set the list to empty */
void InitializeList(List * plist)
{
	plist->head = NULL;
	plist->end = NULL;
}

/* returns true if list is empty */
bool ListIsEmpty(const List * plist) 
{
	if (plist->head == NULL)
		return true;
	else
		return false;
}

/* returns true if list is full */
bool ListIsFull(const List * plist)
{
	Node * pt;
	bool full;

	pt = (Node *) malloc(sizeof(Node));
	full = (pt == NULL) ? true : false;
	free(pt);

	return full;
}

/* returns number of nodes */
unsigned int ListItemCount(const List * plist) 
{
	unsigned int count = 0;
	Node * pnode = plist->head;

	while (pnode != NULL)
	{
		++count;
		pnode = pnode->next; /* set to next node */
	}
	return count; 
}

/* creates node to hold item and adds it to the end of */
/* the list pointed to by plist (slow implementation) */ 
bool AddItem(Item item, List * plist)
{
	Node * pnew;
	pnew = (Node *) malloc(sizeof(Node));
	if (pnew == NULL)
		return false;		 /* quit function on failure */
	CopyToNode(item, pnew);
	pnew->next = NULL;
	
	if (plist->head == NULL) 		/* empty list, so place */
		plist->head = pnew;  		/* pnew at head of list */
	else
		(plist->end)->next = pnew; 	/* set next field of last */
									/* element to point to pnew */
	plist->end = pnew;				/* set end of list to pnew */
	
	return true;
}

/* visit each node and execute function pointed to by pfun */ 
void Traverse (const List * plist, void (* pfun)(Item item) ) 
{
	Node * pnode = plist->head; /* set to start of list */
	while (pnode != NULL)
	{
		(*pfun)(pnode->item); /* apply function to item */
		pnode = pnode->next; /* advance to next item */
	}
}

/* free memory allocated by malloc() */
/* set list pointer to NULL */
void EmptyTheList(List * plist)
{
	Node * psave, * pnode = plist->head;
	while (pnode != NULL)
	{
		psave = pnode->next; /* save */
		free(pnode); /* free */
		pnode = psave;
	}
}

/* local function definition */
/* copies an item into a node */
static void CopyToNode(Item item, Node * pnode)
{
	pnode->item = item; /* structure copy */ 
}
```


## 17-list.h
***
```c
/* list.h -- header file for a simple list type */
#ifndef LIST_H_
#define LIST_H_

#include <stdbool.h> 
#define TSIZE 45 /* size of array to hold title */

struct film
{
	char title[TSIZE];
	int rating;
};

/* general type definitions */
typedef struct film Item;

typedef struct node {
	Item item;
	struct node * next;
} Node;

typedef struct list {
	Node * head;
	Node * end;
} List;

/* function prototypes */


// operation: initialize a list 								
// preconditions: plist points to a list 						
// postcondition: the list is initialized to empty 			
void InitializeList(List * plist);


// operation: determine if list is empty 						
// preconditions: plist points to an initialized list 			
// postcondition: function returns true if list is empty 		
// 				   and false otherwise							
bool ListIsEmpty(const List *plist);


// operation: determine if list is full
// preconditions: plist points to an initialized list
// postcondition: function returns true if list is full and false
// 				otherwise
bool ListIsFull(const List *plist);


// operation: determine number of items in list
// preconditions: plist points to an initialized list
// postconditions: function returns number of items in list
unsigned int ListItemCount(const List *plist);


// operation: add item to end of list
// preconditions: item is an item to be added to list
//				  plist points to an initialized list
// postcondition: if possible, function adds item to end
//				   of list and returns True; otherwise the
//				   function returns False
bool AddItem(Item item, List * plist);


// operation: apply a function to each item in list 
// preconditions: plist points to an initialized list
//			  	  pfun points to a function that takes an
//			 	  Item argument and has no return value
// postcondition: the function pointed to by pfun is
//				  executed once for each item in the list
void Traverse (const List *plist, void (* pfun)(Item item) );

// operation: free allocated memory, if any
// preconditions: plist points to an initialized list
// postcondition: any memory allocated for the list is freed
// 				  and the list is set to empty
void EmptyTheList(List * plist);

#endif
```

## 17-list2
***
```c
/* list.c -- functions supporting list operations */

#include <stdio.h>
#include <stdlib.h>
#include "list2.h"

/* interface functions */

/* set the list to empty */
void InitializeList(List * plist)
{
	plist->items = 0;
}

/* returns true if list is empty */
bool ListIsEmpty(const List * plist) 
{
	return (plist->items == 0);
}

/* returns true if list is full */
bool ListIsFull(const List * plist)
{
	return (plist->items == MAXSIZE);
}

/* returns number of items */
unsigned int ListItemCount(const List * plist) 
{
	return plist->items;
}

/* adds item to list */
bool AddItem(Item item, List * plist)
{
	if (plist->items < MAXSIZE)
	{
		plist->entries[plist->items] = item;
		(plist->items)++;
		return true;
	}
	else
		return false;
}

/* traverses the list, calling *pfun on every item */
void Traverse (const List * plist, void (* pfun)(Item item) ) 
{
	for (int i = 0; i < plist->items; i++)
		(*pfun)(plist->entries[i]);
}

/* empty the list */
void EmptyTheList(List * plist)
{
	plist->items=0;
}
```

## 17-list2.h
***
```c
/* list.h -- header file for a simple list type */
#ifndef LIST_H_
#define LIST_H_

#include <stdbool.h> 
#define TSIZE 45 /* size of array to hold title */
#define MAXSIZE 100

struct film
{
	char title[TSIZE];
	int rating;
};

/* general type definitions */
typedef struct film Item;

typedef struct list {
	Item entries[MAXSIZE];		/* array of items */ 
	int items; 					/* number of items in list */
} List;

/* function prototypes */

// operation: initialize a list 								
// preconditions: plist points to a list 						
// postcondition: the list is initialized to empty 			
void InitializeList(List * plist);


// operation: determine if list is empty 						
// preconditions: plist points to an initialized list 			
// postcondition: function returns true if list is empty 		
// 				   and false otherwise							
bool ListIsEmpty(const List *plist);


// operation: determine if list is full
// preconditions: plist points to an initialized list
// postcondition: function returns true if list is full and false
// 				otherwise
bool ListIsFull(const List *plist);


// operation: determine number of items in list
// preconditions: plist points to an initialized list
// postconditions: function returns number of items in list
unsigned int ListItemCount(const List *plist);


// operation: add item to end of list
// preconditions: item is an item to be added to list
//				  plist points to an initialized list
// postcondition: if possible, function adds item to end
//				   of list and returns True; otherwise the
//				   function returns False
bool AddItem(Item item, List * plist);


// operation: apply a function to each item in list 
// preconditions: plist points to an initialized list
//			  	  pfun points to a function that takes an
//			 	  Item argument and has no return value
// postcondition: the function pointed to by pfun is
//				  executed once for each item in the list
void Traverse (const List *plist, void (* pfun)(Item item) );

// operation: free allocated memory, if any
// preconditions: plist points to an initialized list
// postcondition: any memory allocated for the list is freed
// 				  and the list is set to empty
void EmptyTheList(List * plist);

#endif
```


## 17-pettree
***
```c
// pettree.c -- implementation for binary search tree linked list data type

#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <string.h>
#include "pettree.h"

static bool AddItemToList(Pet * ppet, List * plist);
static int ListCount(List * plist);
static bool InList(Pet * ppet, List * plist);
static void TraverseList(List * plist, void (*fn)(Pet));
static void DeleteList(List * plist);
static bool DeleteListItem(Pet * ppet, List * plist);

Tree * InitializeTree(Tree * ptree) {
	*ptree = NULL;
	return ptree;
}

bool TreeIsEmpty(const Tree * ptree) {
	return (*ptree == NULL);
}

bool TreeIsFull(const Tree * ptree) {
	Node * pnode = (Node *) malloc(sizeof(Node));
	Pet * ppet = (Pet *) malloc(sizeof(Pet));
	if (pnode == NULL || ppet == NULL) {
		return true;
	}
	else {
		free(pnode);
		free(ppet);
		return false;
	}
}

bool AddPet(Pet * ppet, Tree * ptree) {
	if (TreeIsFull(ptree)) {
		return false;
	}

	int cmp;
	while (*ptree != NULL) {
		int cmp = strcmp(ppet->name, (*ptree)->name);
		if (cmp == 0) {
			// add new pet item to list in Node
			return AddItemToList(ppet, &((*ptree)->head));
		}
		else if (cmp < 0) {
			ptree = &((*ptree)->left);
		}
		else {
			ptree = &((*ptree)->right);
		}
	}

	// create a new node if one doesn't already exist
	Node * pnode = (Node *) malloc(sizeof(Node));
	if (pnode == NULL) {
		fprintf(stderr, "Could not allocate memory.\n");
		return false;
	}

	strncpy(pnode->name, ppet->name, STRLEN);
	pnode->head = ppet;
	pnode->left = NULL;
	pnode->right = NULL;

	*ptree = pnode;
	return true;
}

bool DeletePet(Pet * ppet, Tree * ptree) {
	while (*ptree != NULL) {
		int cmp = strcmp(ppet->name, (*ptree)->name);
		if (cmp == 0) {
			List * plist = &((*ptree)->head);
			bool deleted = DeleteListItem(ppet, plist);
			// the list at this node is empty, delete the entire node
			if ((*ptree)->head == NULL) {
				Node * tmp = (*ptree);
				if ((*ptree)->left == NULL) {
					*ptree = (*ptree)->right;
				}
				else {
					Node * right = (*ptree)->right;
					*ptree = (*ptree)->left;
					// reattach right subtree
					if (right != NULL) {
						while ((*ptree)->right != NULL) {
							*ptree = (*ptree)->right;
						}
						(*ptree)->right = right;
					}
				}
				free(tmp);
			}
			return deleted;
		}
		else if (cmp < 0) {
			ptree = &((*ptree)->left);
		}
		else {
			ptree = &((*ptree)->right);
		}
	}
	return false;
}

bool InTree(Pet * ppet, const Tree * ptree) {
	Node * pnode = *ptree;
	while (pnode != NULL) {
		int cmp = strcmp(ppet->name, pnode->name);
		if (cmp == 0) {
			return InList(ppet, &(pnode->head));
		}
		else if (cmp < 0) {
			pnode = pnode->left;
		}
		else {
			pnode = pnode->right;
		}
	}
	return false;
}

int TreeItemCount(Tree tree) {
	int count = 0;
	if (tree == NULL) {
		return count;
	}
	count += ListCount(&(tree->head));
	count += TreeItemCount(tree->left);
	count += TreeItemCount(tree->right);
	return count;
}

void ApplyToNode(Pet * ppet, const Tree * ptree, void (*fn)(Pet)) {
	Node * pnode = *ptree;
	while (pnode != NULL) {
		int cmp = strcmp(ppet->name, pnode->name);
		if (cmp == 0) {
			TraverseList(&(pnode->head), fn);
			return;
		}
		else if (cmp < 0) {
			pnode = pnode->left;
		}
		else {
			pnode = pnode->right;
		}
	}
	return;
}

void TraverseTree(const Tree * ptree, void (*fn)(Pet)) {
	Node * pnode = *ptree;
	if (pnode == NULL) {
		return;
	}
	TraverseTree(&(pnode->left), fn);
	TraverseList(&(pnode->head), fn);
	TraverseTree(&(pnode->right), fn);
}

void DeleteAll(Tree * ptree) {
	if (*ptree == NULL) {
		return;
	}
	Node * pnode = *ptree;
	DeleteAll(&(pnode->left));
	DeleteAll(&(pnode->right));
	DeleteList(&(pnode->head));
	free(pnode);
}

static bool AddItemToList(Pet * ppet, List * plist) {
	if (*plist == NULL) {
		*plist = ppet;
	}
	else {
		Pet * current = *plist;
		while(current->next != NULL) {
			current = current->next;
		}
		current->next = ppet;
	}
	ppet->next = NULL;
	return true;
}

static int ListCount(List * plist) {
	int count = 0;
	Pet * ppet = *plist;
	while (ppet != NULL) {
		count++;
		ppet = ppet->next;
	}
	return count;
}

static bool InList(Pet * ppet, List * plist) {
	while (*plist != NULL) {
		if (strcmp(ppet->name, (*plist)->name) == 0)
			if (strcmp(ppet->type, (*plist)->type) == 0)
				return true;
	}
	return false;
}

static void TraverseList(List * plist, void (*fn)(Pet)) {
	Pet * ppet = *plist;
	while (ppet != NULL) {
		fn(*ppet);
		ppet = ppet->next;
	}
}

static void DeleteList(List * plist) {
	Pet * save, * ppet = *plist;
	while (ppet != NULL) {
		save = ppet;
		ppet = ppet->next;
		free(save);
	}
	*plist = NULL;
}

static bool DeleteListItem(Pet * ppet, List * plist) {
	Pet * save;
	while (*plist != NULL) {
		if (strcmp((*plist)->type, ppet->type) == 0 &&
			strcmp((*plist)->name, ppet->name) == 0 ) {
				save = *plist;
				*plist = (*plist)->next;
				free(save);
				return true;
		}
		plist = &((*plist)->next);
	}
	return false;
}
```

## 17-pettree.h
***
```c
// pettree.h -- interface for a binary search tree linked list data type

#ifndef PETTREE_H_
#define PETTREE_H_ 1

#include <stdbool.h>

#define STRLEN 30

// Type definitions

typedef struct pet
{
	char type[STRLEN];
	char name[STRLEN];
	struct pet * next;
} Pet;

typedef Pet * List;

typedef struct node {
	char name[STRLEN];
	List head;
	struct node * left;
	struct node * right;
} Node;

typedef Node * Tree;

// Operations

// Operation: Initialize a Tree
// Preconditions: ptree points to an unitialized Tree
// Postconditions: ptree points to an initialized Tree
Tree * InitializeTree(Tree * ptree);


// Return true if Tree pointed to by ptree is empty, otherwise
// return false
bool TreeIsEmpty(const Tree * ptree);

// Return true if Tree pointed to by ptree is full, otherwise
// return false
bool TreeIsFull(const Tree * ptree);

// Add Pet item with given name and type to Tree pointed
// to by ptree
// Return: true if operation succeeds, otherwise false
bool AddPet(Pet * ppet, Tree * ptree);

// Find and remove the Node corresponding the to string word
// in the Tree pointed to by ptree if it exists.
// Return: true if the Node corresponding to word is found
// and deleted, otherwise false
bool DeletePet(Pet * ppet, Tree * ptree);

// Search for Node corresponding to string name in Tree
// pointed to by ptree.
// Return: true if word is found in Tree, otherwise false
bool InTree(Pet * ppet, const Tree * ptree);

int TreeItemCount(Tree tree);

// Visit each Node in Tree pointed to by ptree, calling
// function pointed to by fn on each Pet member of each Node
void TraverseTree(const Tree * ptree, void (*fn)(Pet));

// Search for Node matching string name and if found, call
// function pointed to by fn on each Pet member of Node
void ApplyToNode(Pet * ppet, const Tree * ptree, void (*fn)(Pet));

// Delete all Nodes in Tree pointed to by ptree and free all
// allocated memory
void DeleteAll(Tree * ptree);

#endif
```


## 17-queue
***
```c
// queue.c -- implementation of a queue type

#include "queue.h"
#include <stdlib.h>
#include <stdbool.h>
#include <stdio.h>

static void CopyToNode(Item item, Node * pnode)
{
	pnode->item = item;
}

static void CopyToItem(Item * pitem, Node *pnode)
{
	*pitem = pnode->item;
}

void InitializeQueue(Queue * pq) {
	pq->front = NULL;
	pq->rear = NULL;
	pq->items = 0;
}

bool QueueIsFull(const Queue * pq) {
	return pq->items == MAXQUEUE;
}

bool QueueIsEmpty(const Queue *pq) {
	return pq->items == 0;
}

int QueueItemCount(const Queue * pq){
	return pq->items;
}

bool EnQueue(Item item, Queue * pq) {
	if (QueueIsFull(pq)) {
		return false;
	}

	Node * new = (Node *) malloc(sizeof(Node));
	if (new == NULL) {
		fprintf(stderr, "Unable to allocate memory!\n");
		exit(EXIT_FAILURE);
	}
	CopyToNode(item, new);
	new->next = NULL;

	if (QueueIsEmpty(pq)) {
		pq->front = new;
	}
	else {
		(pq->rear)->next = new;
	}
	pq->rear = new;
	pq->items++;	// increment items count

	return true;
}

bool DeQueue(Item *pitem, Queue * pq){
	if (QueueIsEmpty(pq)) {
		return false;
	}
	
	Node * first = pq->front;
	CopyToItem(pitem, first);
	pq->front = first->next;
	free(first);

	// decrement items count and set rear pointer to null if necessary
	pq->items--;
	if (pq->items == 0) {
		pq->rear = NULL;
	}
	return true;
}

void EmptyTheQueue(Queue * pq){
	Item tmp;
	while (DeQueue(&tmp, pq))
		continue;
}
```

## 17-queue.h
***
```c
/* queue.h -- interface for a queue */
#ifndef _QUEUE_H_
#define _QUEUE_H_ 1

#include <stdbool.h>

/* INSERT ITEM TYPE HERE */
/* FOR EXAMPLE, */
// typedef int Item; // for use_q.c
/* OR typedef struct item {int gumption; int charisma;} Item; */

typedef struct item {
	long arrive; /* the time when a customer joins the queue */
	int processtime; /* the number of consultation minutes desired */
} Item;


#define MAXQUEUE 10

typedef struct node {
	Item item;
	struct node * next;
} Node;

typedef struct queue {
	Node * front;		// pointer to front of queue
	Node * rear;		// pointer to rear of queue
	int items;			// number of items in queue
} Queue;

/* operation:		initialize the queue 						*/
/* precondition:	pq points to a queue 						*/
/* postcondition:	queue is initialized to being empty 		*/
void InitializeQueue(Queue * pq);

/* operation: 		check if queue is full 						*/
/* precondition: 	pq points to previously initialized queue 	*/
/* postcondition: 	returns True if queue is full, else False 	*/
bool QueueIsFull(const Queue * pq);

/* operation:		check if queue is empty 					*/	
/* precondition:	pq points to previously initialized queue 	*/
/* postcondition:	returns True if queue is empty, else False 	*/
bool QueueIsEmpty(const Queue *pq);

/* operation:		determine number of items in queue 			*/
/* precondition:	pq points to previously initialized queue 	*/
/* postcondition:	returns number of items in queue 			*/
int QueueItemCount(const Queue * pq);

/* operation:		add item to rear of queue 					*/
/* precondition:	pq points to previously initialized queue 	*/
/*					item is to be placed at rear of queue 		*/
/* postcondition:	if queue is not full, item is placed at 	*/
/*					rear of queue and function returns 			*/
/*					True; otherwise, queue is unchanged and 	*/
/*					function returns False 						*/
bool EnQueue(Item item, Queue * pq);

/* operation:		remove item from front of queue 			*/
/* precondition:	pq points to previously initialized queue 	*/
/* postcondition:	if queue is not empty, item at head of 		*/
/*					queue is copied to *pitem and deleted from 	*/
/*					queue, and function returns True; if the 	*/
/*					operation empties the queue, the queue is   */
/*					reset to empty. If the queue is empty to	*/
/*					begin with, queue is unchanged and the 		*/
/*					function returns False						*/
bool DeQueue(Item *pitem, Queue * pq);

/* operation:		empty the queue 							*/
/* precondition:	pq points to a previously initialized queue */
/* postconditions:	the queue is empty 							*/
void EmptyTheQueue(Queue * pq);

#endif
```

## 17-stack
***
```c
// stack.c -- implementation file for stack type

#include <stdlib.h>
#include "stack.h"
#include <stdio.h>

Stack * InitializeStack(Stack * pstack)
{
	*pstack = NULL;
	return pstack;
}

bool PushItem(const char * pch, Stack * pstack)
{
	Node * pnode = (Node *) malloc(sizeof(Node));
	if (pnode == NULL)
	{
		fprintf(stderr, "Stack is full. Could not add item.\n");
		return false;
	}
	else
	{
		pnode->item = *pch;
		pnode->previous = *pstack;
		*pstack = pnode;
		return true;
	}
}

bool PopItem(char * pch, Stack * pstack)
{
	if (*pstack == NULL)
		return false;
	else
	{
		*pch = (*pstack)->item;
		Node * tmp = *pstack;
		*pstack = (*pstack)->previous;
		free(tmp);
		return true;
	}
}

void EmptyStack(Stack * pstack)
{
	Node * tmp, * pnode = *pstack;
	while (pnode != NULL)
	{
		tmp = pnode->previous;
		free(pnode);
		pnode = tmp;
	}
}
```

## 17-stack.h
***
```c
#ifndef STACK_H_
#define STACK_H_ 1

// stack.h -- header file for a stack type

#include <stdbool.h>

// Type definitions

typedef struct node
{
	char item;
	struct node * previous;
} Node;

typedef Node * Stack;

// Operations

// Initialize a list
Stack * InitializeStack(Stack * pstack);

// Attempt to add char ch to the stack pointed to by pstack.
// Returns true if operation is successful, else false
bool PushItem(const char * pch, Stack * pstack);

// Pop item from stack and place in location pointed to by pch.
// Returns true if operation succeeds, false if stack is empty
bool PopItem(char *pch, Stack * pstack);

// Empty Stack and free memory
void EmptyStack(Stack * pstack);

#endif
```

## 17-tree
***
```c
// tree.c -- implementation for binary search tree data type

#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <string.h>
#include "tree.h"

Tree * InitializeTree(Tree * ptree) {
	*ptree = NULL;
	return ptree;
}

bool TreeIsEmpty(const Tree * ptree) {
	return (*ptree == NULL);
}

bool TreeIsFull(const Tree * ptree) {
	Node * pnode = (Node *) malloc(sizeof(Node));
	if (pnode == NULL) {
		return true;
	}
	else {
		free(pnode);
		return false;
	}
}

bool AddWord(const char * word, Tree * ptree) {
	if (TreeIsFull(ptree)) {
		return false;
	}

	int cmp;
	while (*ptree != NULL) {
		int cmp = strcmp(word, (*ptree)->item.word);
		if (cmp == 0) {
			(*ptree)->item.count++;
			return true;
		}
		else if (cmp < 0) {
			ptree = &((*ptree)->left);
		}
		else {
			ptree = &((*ptree)->right);
		}
	}

	Item item;
	strncpy(item.word, word, WORD_SIZE);
	item.count = 1;

	Node * pnode = (Node *) malloc(sizeof(Node));
	if (pnode == NULL) {
		fprintf(stderr, "Could not allocate memory.\n");
		return false;
	}

	pnode->item = item;
	pnode->left = NULL;
	pnode->right = NULL;

	*ptree = pnode;
	return true;
}

bool DeleteWord(const char * word, Tree * ptree) {
	while (*ptree != NULL) {
		int cmp = strcmp(word, (*ptree)->item.word);
		if (cmp == 0) {
			// delete this node
			Node * right = (*ptree)->right;
			Node * tmp = (*ptree);
			*ptree = (*ptree)->left;
			while ((*ptree)->right != NULL) {
				ptree = &((*ptree)->right);
			}
			(*ptree)->right = right;

			free(tmp);
			return true;
		}
		else if (cmp < 0) {
			ptree = &((*ptree)->left);
		}
		else {
			ptree = &((*ptree)->right);
		}
	}
	return false;

}

bool InTree(const char * word, const Tree * ptree) {
	Node * pnode = *ptree;
	while (pnode != NULL) {
		int cmp = strcmp(word, pnode->item.word);
		if (cmp == 0) {
			return true;
		}
		else if (cmp < 0) {
			pnode = pnode->left;
		}
		else {
			pnode = pnode->right;
		}
	}
	return false;
}

void ApplyToNode(const char * word, const Tree * ptree, void (*fn)(Item)) {
	Node * pnode = *ptree;
	while (pnode != NULL) {
		int cmp = strcmp(word, pnode->item.word);
		if (cmp == 0) {
			(*fn)(pnode->item);
			return;
		}
		else if (cmp < 0) {
			pnode = pnode->left;
		}
		else {
			pnode = pnode->right;
		}
	}
	return;
}

void ApplyToAll(const Tree * ptree, void (*fn)(Item)) {
	Node * pnode = *ptree;
	if (pnode == NULL) {
		return;
	}
	ApplyToAll(&(pnode->left), fn);
	(*fn)(pnode->item);
	ApplyToAll(&(pnode->right), fn);
}

void DeleteAll(Tree * ptree) {
	Node * pnode = *ptree;
	DeleteAll(&(pnode->left));
	DeleteAll(&(pnode->right));
	free(pnode);
}
```

## 17-tree.h
***
```c
// tree.h -- interface for a binary search tree data type

#ifndef TREE_H_
#define TREE_H_ 1

#include <stdbool.h>

#define WORD_SIZE 30

// Type definitions

typedef struct 
{
	char word[WORD_SIZE];
	int count;
} Item;

typedef struct node {
	Item item;
	struct node * left;
	struct node * right;
} Node;

typedef Node * Tree;

// Operations

// Operation: Initialize a Tree
// Preconditions: ptree points to an unitialized Tree
// Postconditions: ptree points to an initialized Tree
Tree * InitializeTree(Tree * ptree);


// Return true if Tree pointed to by ptree is empty, otherwise
// return false
bool TreeIsEmpty(const Tree * ptree);

// Return true if Tree pointed to by ptree is full, otherwise
// return false
bool TreeIsFull(const Tree * ptree);

// Add occurence of string word to Tree pointed to by ptree:
// if a node corresponding to word already exists, increment
// the count member of that node, otherwise create a new Node
// and add it to the Tree, setting its count to 1.
// Return: true if operation succeeds, otherwise false
bool AddWord(const char * word, Tree * ptree);

// Find and remove the Node corresponding the to string word
// in the Tree pointed to by ptree if it exists.
// Return: true if the Node corresponding to word is found
// and deleted, otherwise false
bool DeleteWord(const char * word, Tree * ptree);

// Search for Node corresponding to string word in Tree
// pointed to by ptree.
// Return: true if word is found in Tree, otherwise false
bool InTree(const char * word, const Tree * ptree);

// Visit each Node in Tree pointed to by ptree, calling
// function pointed to by fn on the Item member of each Node
void ApplyToAll(const Tree * ptree, void (*fn)(Item));

// Search for Node matching string word and if found, call
// function pointed to by fn on its item member
void ApplyToNode(const char * word, const Tree * ptree, void (*fn)(Item));

// Delete all Nodes in Tree pointed to by ptree and free all
// allocated memory
void DeleteAll(Tree * ptree);

#endif
```
