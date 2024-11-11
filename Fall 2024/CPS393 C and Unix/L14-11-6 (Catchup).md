---

---

Little tip:
to see all **format specifiers** (e.g. `%d`), use `man 3 scanf`

## Structures!
- Similar to NamedTuple in python (Collections) and class in Java
- Made up of "members" 

```C
//Looks like:
struct structName {
	type item1;
	type item2;
};
//Example:
struct car {
	char make[40];
	char model[40];
	unsigned int year;
	float price;
} chan, woit; //<- this declares 2 car variables named chan and woit
```
To declare as a variable:
Use the dot operator to access attributes:
```C
struct car woit;
strcpy(woit.make "Ferrari"); //strcpy from <string.h>
scanf("%s", woit.make);
scanf("&u", &woit.year);
woit.price = 198190.00;
```

Can define a function that returns a struct by:
`struct car getCarInfo(void) {...}`

### Typedefs
- defines another name for a type
- Differs from `#define` because it obeys compiler scoping rules, and has some more features
- Cannot use typedefs for pointers, but you CAN use define:
```C
define int* intPtr`
intPtr px, py;
```

Common to use Typedefs with structs to avoid writing "struct" so much:
```C
typedef struct {
	char make[40];
	char model[40];
	unsigned int year;
	float price;
} car; 
```
This creates a struct called car with all the attributes we want, but we can say `car` instead of `struct car`


C's time and Date functions `<time.h>` use structures!
```C
struct tm {
  int tm_sec;   //seconds, 0-60
  int tm_min;   //minutes, 0-59
  int tm_hour;  //hours, 0-23
  int tm_mday;  //day of month, 1-31
  int tm_mon;   //months since Jan, 0-11
  int tm_year;  //years since 1900
  int tm_wday;  //days since Sunday 0-6
  int tm_yday;  //days since Jan 1, 0-365
  int tm_isdst; //daylight savings time flag
  };
```

If you have a pointer to a structure, you can access members of a structure using `->`
`carPtr->make` = `(*carPtr).make`
This is short for dereferencing the pointer and then using the dot operator.

##### Nested Structures!
You can use structures inside others:
```C
typedef struct {
	float x;
	float y;
} point;

typedef struct {
	point vertex1;
	point vertex2;
	point vertex3;
} triangle;
```

##### Linked Lists!
```C
#include<stdio.h>
struct node {
	char ch;
	struct node *next;
}
```

c4.txt has more stuff on linked lists (searching, adding, etc.), (optional) abstraction,
