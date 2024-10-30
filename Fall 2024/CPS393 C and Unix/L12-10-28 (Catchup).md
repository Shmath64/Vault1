Time to do some catchup, skipped some online lectures!

use `fprintf` to print error messages
Remember that exit code 1 means error and 0 means success
### End of C1, now C2:

#### Arrays and strings
`int A[20];` allocates 20 integers.
Note that "garbage" memory will be here; it will be full of random integers
	`for (int i=0; i<20; i++) a[i]=0` will set it all to zero
Note that there is no "out of bounds checking"; `A[40]` will NOT throw an error

Can do 
`int A[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}`

If you have `a[10]` and `b[10]` DO NOT do `a=b`, instead do:
`for (int i=0; i<10; i++) a[i]=b[i]`

### Variable Length Array (VLA)
```C
int n;
scanf("%d",&n);
int A[n];
```
Cannot use variable length array if:
- array is *static* or *extern*
- array is *member of a structure or union*
- Compiler doesn't allow VLAs.
VLA's can be either **block scope** (within `{}`) or **function prototype scope** (within function declaration)

We can use `calloc` (more on this later)
```C
int n;
scanf("%d", &n);
int *A = calloc (n, sizeof(int));
```
This will allocate enough memory for A to be an array of $n$ integers (and initialize them all to zero). 
What `calloc` is doing here is allocating enough memory for $n$ integers (`sizeof(int)`)
Differs from `malloc` which just takes a number of bytes

Remember that `int *` means a pointer to an integer OR an array of integers

### Strings!
No `string` type, instead its a char array.
Strings are **null-terminated char arrays**, this means they end in the null character `\0`
`\0` looks like the byte: `00000000`
Compiler typically automatically null-terminates string constants
e.g.
`char S[12] = "Hello There!";`
Causes:
```
0:  'H': 01001000
1:  'e': 01100101
2:  'l': 01101100
3:  'l': 01101100
4:  'o': 01101111
5:  ' ': 00100000
6:  'T': 01010100
7:  'h': 01101000
8:  'e': 01100101
9:  'r': 01110010
10: 'e': 01100101
11: '!': 00100001
12: '': 00000000
```
see `~/cps393/playingAroundC/stringTest.c`

`gets()` can be used to get strings from the user even though it is deprecated. (memory unsafe, it can take in a longer string and assign it to a variable for a shorter array)
`fgets()` is the new cool because it is memory safe, and has no potential memory overwrite.

Unsafe (with `gets`)
```C
char str[10]; //String MAX 9 characters (because it needs \0)
gets(str); //Unsafe, may make str more than 9 characters
puts(str); //Going to output all of str
```

Safe (with `fgets`)
```C
char str[10]; //Max 9 chars
fgets(str, 10, stdin); //Will limit to 9 chars + \0, read from stdin
puts(str)
```

`fgets` goes until:
1. `\n` read, OR
2. (length-1) chars have been read, OR
3. EOF

If the inputted string is shorter than 9 chars, it will include `\n` in str. (can replace this with `\0`)

##### <string.h.>
Gives us:
- `strcpy`
	- `strcpy(a, b);` copies string `b` into string `a`
- `strcat`
	- `strcat(a, b);` concatenates $a+b$  into `a`
- `strcmp`
	- Compares lexicographically (=0 if they're the same string)
- `strlen`
	- Doesn't include `\0`
- `sprintf`
	- `sprintf(S, "%.2lf", 94.1234)` prints "94.12" into string `S`
- `sscanf(str, formatString)`
	- Like a `scanf` but takes a string instead

#### Multi-Dimensional Arrays
`int X[3][4];` is an array with 3 rows \[0-2] and 4 columns \[0-3]
`int X[2][3][4];` is like a cube!

Can initialize like so:
```C
int D[2][3] = {
               2,4,7,
               6,9,5
              }; /*D[0][0] is 2; D[0][1] is 4, etc */
```
OR, use extra braces:
```C
int D[2][3] = {
               { 2,4,7 },
               { 6,9,5 }
              }; /*D[0][0] is 2; D[0][1] is 4, etc */
```

Don't forget that automatic (local) arrays are NOT initialized by compiler, however,
global and static arrays initialized to 0 by compiler (like all global and static variables)

\* Note that **static** variables are like **global** variables but with *internal linkage* (i.e. they can't be accessed by other files using `extern`)
They're called "static" because they're initialized in a static place in memory unlike local variables put on the stack

##### Unsized Arrays:
This is all okay:
`int A[] = {1, 4, 2, 6};`
`char userMsg[] = "Enter a number: ";`
`int b[][2] = {1,4,7,9,3,5}; //Compiler needs one dimension-count (can't be vague)`

#### Call-by-Value
```C
...{...
	int localVar=0;
	int arr[] = {0, 0, 0};
	f1(arr, localVar);
	printf("local Variable = %d\n", localVar) //Prints 0
	printf("arr[0] = %d\n", arr[0]) //Prints 99
...}...

void f1(int nums[], int loc) {
	nums[0] = 99;
	loc = 99;
}
```
When passing integers, C passes call-by-value (a copy of the integer)
When passing arrays, C passes the **address** of the array. (therefore, it's LIKE call-by-reference - but its not really)


### Pointers!

```C
int num; //Allocates, lets say, 1200
num=5; //Sets the value there to 5
int *NumPtr; //Allocates memory, lets say 1531
NumPtr = &Num; //Sets NumPtr(at 1531) to 1200
``` 
Afterwards,
`num` = `*NumPtr` = 5
`NumPtr` = `&Num` = 1200

Can of course do assignment operations,
`*NumPtr = 7;` <- Now, `num` will be 7

```C
int A[5] = {10,20,30,40,50};
printf("%p\n", A); //Prints address of the array's beginning
printf("%d\n", *A); //`*` "Dereferences" so that this will print 10
printf("%d\n", *(A+2)); //Prints 30 -note how it increments by index NOT bytes
printf("%d\n", *(A)+2); //Prints 12
return 0;
```

...Continue at c2:589

