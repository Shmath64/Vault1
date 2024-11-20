Use `atoi` to get integer from character (`<stdlib.h>`)
`int x = atoi;`  -> `x == 7`
```C
int main(void) {
        char *c = "7";
        int x = atoi(c);
        printf("%d\n",x);
        return 0;
}
```

When taking user input and putting it into a variable, use the address-of operator (`&`):
`scanf("%d", &variable);` 
also:
`gets` (unsafe) and `fgets`

Declaration of pointers:
```C
int x = 15;
int *p = &x;
//IS EQUAL TO:
int *p;
p=&x;
//BUT NOT EQUAL TO:
int *p;
*p=&x; //WRONG!
```
The confusion lies in `*` being used to both define pointers and as the dereference operator!

#### Pointers and Arrays:
Array variable is really just a pointer to the first element of the array.
`int arr[] = {1, 2, 3, 4}`

```C
#include<stdio.h>
#define LOOPNUM 5
int main(void) {
	int arr[] = {1, 2, 3, 4};

	//These two loops do the SAME thing:
	for(int i=0;i<LOOPNUM;i++) {
		fprintf(stdout, "%d\n",arr[i]); //fprintf we must define where we write to
	}

	int *ptr = arr;
	for(int i=0; i<LOOPNUM; i++) {
		printf("%d\n", *ptr++); //Note the dereferencing *then* incrementing
	}
	return 0;
}
```

`arr[] == *arr` in function parameters, but they differ in declarations
```C
scanf("%d", &myval); //Read an integer using an &
scanf("%s", mystring); //Strings are just pointers to char arrays
```

### Multiple Indirection for Multi-Dimensional Arrays
```C
int grid[3][3] = {{0,0,0},{1,1,1},{2,2,2}};
**grid = 17;
printf("%d\n",grid[0][0]); //>> 17

*(*(grid+2)+1=8;

for (int i=0; i<9; i++) {
	printf("%d", grid[i/3][i%3]); //>> 17 0 0 1 1 1 2 8 2
}

//To make a valid ptr to grid,
int *ptr = &grid[0][0];
//Can treat a multi-dimension array like a single dimensional array this way:
for(int i=0; i<9; i++) { 
	printf("%d\n", *ptr);
	ptr++;
}
//To go the other way (probably not important)
int grid[9] = {1, 2, 3, 11, 12, 13, 21, 22, 23}; 
int (*ptr)[3] = (int (*)[3])grid; // Treat grid as a 2D array with 3 columns
```

When adding integers to a pointer, it will increment the address by the appropriate number of bytes for the data type the pointer is pointing to. 
- These even works in nested arrays!

`int moreData[4][2] = { {0,1}, {2,3}, {4,5}, {6,7} };`
- Good idea to play around with pointers here!


```C
void changeVar(int *x) { //Need to use a pointer to change the variable
	*x = 1;
}
int w=0; 
changeVar(&w);
```

C won't let you define multidimensional arrays without defining the length of the dimensions so that it's not ambiguous

Making an array in a function and then trying to return it won't work because the array itself is a local variable (reallocated the *instant* the function returns.)  This brings us to malloc and calloc

Note that `malloc` and `calloc` return a void pointer, so it's a good idea (you better) to cast it as a specific pointer. Also, it points to the "beginning" of that block
```C
int* newArr = (int*) malloc(3 * sizeof(int));
int* newArr = (int*) calloc(3, sizeof(int));
```

### File I/O
in c3.txt

Remember `fopen`, `fputc`, `fgets`/`fgetc`
Note that `fopen` takes a string NOT a char!
```C
FILE *fp; //Note that its a POINTER to a file!
if ( (fp = fopen("myFileName", "r")) == NULL) {
	fprintf(stderr, "fopen: Error opening file myfile \n");
}
```
Modes: "r", "w", "a"; read, write append


### Other last minute reminders
`char** argv` = `char *argv[]`
`stdlib.h`, `string.h` (`strlen`) are solid

`scanf("%f", &f);` <- No `\n` in it
When reading from files, `getline` is solid
`fgetc` for reading single character at a time

may need to get an initial value of something before doing the loop!

```C
int main(int argc, char* argv[]) {
	FILE *fp;
	fp = fopen(argv[1], "r");
	char ch;
	while(( ch = fgetc(fp)) != EOF) {
		if (ch == '\n')
			printf("\nNEW LINE\n");
		else
			fputc(ch,stdout);
	}
}
```