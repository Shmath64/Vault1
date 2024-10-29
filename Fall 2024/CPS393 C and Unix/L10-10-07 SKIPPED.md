#CBH 
Watch video/review note and fill this in

Also make sure to check out `gcc` AND `makefile`

`gcc fileName.c` will compile into `a.out`
`gcc -o outFileName fileName.c` 

#### Variable Types
Type, usual size (bytes), format specifer
- `char` 1 %c 
- `short int` 2 %hd
- `int` 4 or 2, %d or $i
- `long int` 4 or 8, %ld
- `float` 4, %f
- `double` 8, %lf
\* note that single quotes are used for characters while double quotes are used for strings (which are arrays of chars)

##### Modifiers
signed, unsigned (char or integer), short (integer only), long (integer or double)
`#define xx 3.9 //Taken as double`
`#define xx 3.9L //taken as long double`
L for double, U for "unsigned"

```C
#include<std.io.h>
int main(void) {
	char letter = 'A' //Note single quotes for letter
	printf("As decimal: %d\n", letter);
	printf("As character: %c\n", letter);
	printf("As address: %p\n", &letter); //note ampersand for address
	//printf("As string: %s", letter); <-- Can't treat char as string
	char *ptr = &letter;
	printf("Pointer: %p\n", ptr)
}
```

#### Implicit Type Conversion
==char < int < float < double==  
is like "automatic widening casting in Java" (widening is converting a "smaller" (less precise) data type into a "larger" (more precise) one)

"Type casts" are explicit
```C
int i = 4;
double result;
result = i*(double)8; 
//OR
result = (double)(i*8)
```
Note that `y=(double)(i/8)` will yield `0` while `y=i/(double)8` will yield `0.5`

### "Function Prototypes"
```C
#include<stdio.h>
float half(float a); //Function prototype!
// Tells the compiler that float "half" exists (and it takes a float)
// Would be unnecessary if the function was defined up here

int main(void) {
	float num,hf;
	printf("Enter number: ");
	scanf("%f", &num); //Note the ampersand. "Address to write to"
	hf = half(num);
	printf("Half of %.2f is %.2f\n", num, hf);
	return 0;
}

float half(float a) {
	return (a/2);
}
```

#### Library Functions
(e.g. scanf, printf)
Must include the appropriate `.h` file in the `.c` to use.
These functions are implemented by the *Standard Library* (`libc` or `glibc`)

When using `<math.h>`, may require `gcc fileName.c -lm` to explicitly link the math library (`libm`)

```C
int p(int y, int z) {
	return (pow(y, z)); //While `pow` computes a double,
	//The function will implicitly convert the result to a integer
}
```
There can also be implicit conversion when passing say, an integer into a double argument

```C
double x = sqrt(1/2); //Integer division done here! 
printf("%lf\n", x); //Will print 0.000000.
```

Exit function (`<stdlib.h>`) is usually in main but can be anywhere, it terminates the whole program.

Arguments are ALWAYS passed by value, not reference.


### Makefile

```
target: prerequisites
	command
	command
	command
```

can be used with the `make` keyword to run commands to streamline compiling and whatnot (we can get it to run any linux commands)

so the `target` can be the command or file.
the `prerequisites` are the files it depends upon

When running a `rule`, if the `target` does not exist (or is older than the dependencies). The commands below will run.

```
main: main.o functions.o
    gcc -o main main.o functions.o
```
The gcc will run when `main` which depends on `main.o` and `functions.o` is "out of date" (i.e. newer than main).

```
main.o: main.c
	gcc -c main.c
```
This will be ran by the above function (or by `make main.o`) if `main.c` is "newer" than `main.o`. 
It's kinda "recursive".



Remember that anything in single quotes is a "literal character".
"Characters" are really integers in C. 

#### Loops
`for ( ; ;) ;` is an infinite loop

`break;` and `continue;` are the same as in other languages.

```C
switch(c) {
	case 'a':
		printf("Letter A!");
		break;
	default:
		printf("Not the letter A!");
}
```

`<ctype.h>` contains a lot of useful functions for working with characters.
`'A' == 65` SO true!

