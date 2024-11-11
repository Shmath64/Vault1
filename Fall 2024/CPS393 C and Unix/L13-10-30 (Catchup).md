## Starting C3.txt

**OKAY:** (however, it is unsafe to modify `*p`, "ABC" is read-only)
```C
char *p;
p="ABC";
printf("%s",p);
```

**BAD:**
```C
char *p;
fgets(p,40,stdin);
printf("%s",p);
```
This SUCKS because `p` has no storage allocated for its string.


#### Pointers as Parameters
**All arguments in C are pass-by-value.** 
- This includes arrays, however, the value happens to be pointer
This is ***still*** pass-by-value (a copy of the pointer is made and passed) even though it really *feels* like pass-by-reference

note that for an array A, `A == &A[0]` <-- `&` is "*address-of*" operator

**const** keyword
```C
//const means values POINTED TO by X cannot be changed
//X itself can be changed, but only locally (inside printit)
void printarray(const int *X) {
	int i;
	for (i=0; i<len; i++) {
		printf("&d " X[i]);
	}
	printf("\n");
	//X[i]=i; is NOT permitted
	//X=&i IS permitted but local change only
}
```
"Any change a function makes to its arguments are local only"

Although in C it's always **call-by-reference**,
We can get **call-by-reference**-like behaviour by passing the address (`&`) and then using `*` to **dereference** and modify the data.
Note that `a == *&a` ("a is the same as dereferenced address at a")

```C
void incrementUsingPointer (int *a) {
	*a = *a + 1; //Changes what's in memory 'global' scope
}
void incrementWithoutPointer (int a) {
	a = a + 1; //Only changes `local` a; no effect outside of function
}
int main(void) {
	int a = 5;
	incrementWithoutPointer(a);
	printf("%d", a); //prints 5 (no change)
	incrementUsingPointer(&a);
	printf("%d", a); //prints 6
}
```

##### Multiple Indirection
By using `**` we can define a pointer to a pointer!

### Command Line Processing
We pass arguments to main with:
- Argument Count
- Argument Vector
`int main(int argc, char *argv[]);`

#### File I/O
Can use `fgets` and `fprintf` in addition to:
- `fscanf (fp, format, arg_list);`
- `fgetc (fp);` `fputc(ch, fp);`
- `fputs(str, fp);`
- `getline(&str, &n, fp);`

Use `fopen` with one of the following modes:
`r`: read
`w`: write (create or *CLOBBER*)
`a`: append to the end
```C
#include<stdio.h>
FILE *fp;
if( (fp = fopen("myfile","r")) == NULL) { 
	fprintf(stderr, "fopen: Error opening file myfile \n");
	exit(1);
}
fscanf(fp, "%d", &i); //Save to variable i
fclose(fp); //Good to do as soon as you can
```

```C
/*Source cio.c
  Purpose: write a string to file myfile1
*/
#include <stdio.h>
#include <stdlib.h>
#define GOOD 0
#define BAD 1

int main(void) {
   char str[40] = "String to write onto disk";
   FILE *fp;
   char ch, *p;
   if ((fp = fopen("myfile1","w"))==NULL) {
      fprintf(stderr,"fopen: Cannot open file\n");
      exit( BAD);
   }
   p=str;
   while (*p) //No curly braces, single statement block
     if (fputc(*p++,fp)==EOF) { //EOF if error, `p` is incremented AFTER writing
        fprintf(stderr,"Error writing file\n");
        fclose(fp);
        exit(BAD);
      }
   fputc('\n',fp);
   printf("file myfile1 now contains string: %s\n",str);
   fclose(fp); /*good form*/
   exit(GOOD);
}
```

### Dynamic Memory Allocation (DMA)
We've been allocating storage (memory) at **compile-time** (stack memory)
In C, we can manually allocate/free heap memory at run-time with DMA

Reasons to use DMA (Dynamic Memory Allocation):
- When you need memory maintained after function return (stack memory is destroyed)
- When you need some real RAM (typical stack size is 1MB)
	- May happen when building some kind of structure (graph, list, array, etc.) which may get large, or when memory requirements may be uncertain.
- Ability to *free* no-longer-needed memory during program execution.
- Must be done if you want to create an array inside a function and return it to main.

#### calloc
"contiguous allocation"
`calloc(NumItems, Itemsize);`
**returns:** 
- a void (generic) pointer, may be cast to appropriate type.
OR
- NULL (if storage cannot be obtained)
Memory is initialized to zeroes.

#### malloc
"memory allocation"
`malloc(Size);` 
Is like `calloc`, except it just takes a number of bytes (we do the math)

The following C code generates an array of size *n* where *n* is inputted from the user at runtime. This is placed in Heap memory.
```C
/*Source: trycalloc.c */
#include<stdio.h>
#include<stdlib.h>

int main(void) {
	int n, *p, *tmp, i;
	//num of elts of future array, pointers to future array, iteration var
	scanf("%d", &n); //Read integer from CLI to n
	
	p = (int *) calloc (n, sizeof(int)); //calloc returns a void pointer!
	//We cast it as an integer pointer 
	//(kinda unnecessary, but keeps it C++ compatible)

	//Note: calloc(n, sizeof(int)) = malloc(n * sizeof(int))

	//Check for successful calloc
	if (p == NULL) {
		fprintf(stderr,"calloc failed\n"); //or perror("calloc");
		exit(0);
	}

	for(i=0; i<n; i++) {
		printf("p[%d] is %d\n",i,p[i]);
	} //Will show that all 

	tmp = p;//tmp now points to the same as p
	//load array with 0,1,2,...(n-1)
	for (i=0; i<n; i++) {
		*tmp++ = i;
		//Same as:
		//tmp[i]=i;
		//  p[i]=i;
	}

	for(i=0; i<n; i++) {
		printf("p[%d] is %d\n",i,p[i]);
	}
	return 0;
}
```

All the heap memory we allocate in the program is freed upon completion.
However, we can free it before hand.

For strings, do:
`p = (char *) calloc (n, sizeof(char));`

#### free
`free(p);`
CANNOT free stack memory, this is just all about heap memory


##### getline
in `<stdio.h>`
similar to readline, but may read from a file or stdin,
includes the newline character.

## End of c3.txt
