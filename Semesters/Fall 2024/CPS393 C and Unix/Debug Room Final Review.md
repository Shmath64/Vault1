## BASH
#### Indexed Arrays:
`nums[0]=70`
`nums[1]=71`
`echo "${nums[0]}"` >>`70`
`echo "${nums[*}}"` >> `70 71`
`echo "${#nums[*]}"` >> `2` ; length of array

#### Associative Arrays
(Kinda like Dictionaries)
Uses *string* indexing
```Bash
declare -A greyscale #typeset -A greyscale
greyscale[white]=255
greyscale[black]=0
greyscale[grey]=150
greyscale[light_grey]=200
echo "${greyscale[light_grey]}" #>> 200
echo "${greyscale[*]}" #>> 0 255 150 200 (hash order)
echo "${#greyscale[*]}" #>> 4
```
Be careful! Bash won't throw errors when accessing by index that isn't defined

#### Foreground / Background processes (Don't think this will be on the exam, check notes tho)
For commands that take a long time, we can end a command with ` &` to make it run in the background.
`find / -name firefox > find.out 2>/dev/null &`
`fg %1` will bring it to the foreground, `%1` refers to job id. `bg` resumes a stopped process in the background
`ps` - list all the current processes. Mainly concerned with PID and CMD
`jobs` will list the jobs that are running, stopped, terminated 
`ctrl+z` suspends foreground process
`ctrl+c` ends foreground process
`kill` terminates process based on its job id

### Command History
`history` lists recently used commands
`!!` re-execute the last command
`!cd` re-execute last command starting with "cd"

`^string1^string2` re-executes last command replacing string1 with string2
```Bash
>> echo "This is a command"
This is a command
>> ^command^dog
This is a dog
```

### Functions and Scope
Can be created in terminal, but easier to see with a bash script
```Bash
func_name() {
...
}
```
Note that you can't specify the arguments required for this function. Use function inputs with $1, $2; this is easily confused with script arguments. Can rename script inputs (as variables) to not confuse them

There's no way to return values, but we can set a global variable and use its value later on.
```Bash
myFun() {
	echo "Function argument 1: $1"
	echo "Function argument 2: $2"
	answer=$(($1+$2)) #Answer is a global variable
}
```

We can call our function from a "subshell":
`result=$(myFun $2 $1)` This uses "parameter substitution", it's a different scope! The only thing that "leaves" this function call is stdout. I.e. it CANT change variables outside of itself.
Similar things happen when *piping*! Runs in a subshell, but it can't change variables outside of it!

```Bash
concatFunc() {
	newStr="$1$2"#" kinda unnnecessary, just make sure its definitely a string
	echo $newStr
}
numArgs = $#
concatFunc $1 "${!numArgs}"
```

Piping uses a sub-shell!

#### Debugging!
`bash -v funcEx 10 11`
will display every line just before executing it

`bash -x funcEx 10 11`
Show values of variables being passed, shows sub-shell

`shift` moves cmd line argumnets "ahead" one position. Useful for parsing inputs one at a time.

#### xargs
`xargs` is good for piping!

`echo "file1 file2 file3 file4" | xargs grep "key"`
Pass the output from one command into the next as an argument, rather than stdin.
More used as a prefix, *not* an intermediate argument, but can go both ways

```Bash
>> echo "qw.ert.yui.sdfgh.ppp" | xargs -d.
qw ert yui sdfgh ppp
>> echo "qw.ert.yui.sdfgh.ppp" | xargs -d. -n2
qw ert
yui sdfgh
ppp
>> echo "qw.ert.yui.sdfgh.ppp" | xargs -d. -I{} echo __{}__
__qw__
__ert__
__yui__
__sdfgh__
__ppp
__
>> echo "qw.ert.yui.sdfgh.ppp" | tr -d "\n" | xargs -d. -I{} echo __{}__
__qw__
__ert__
__yui__
__sdfgh__
__ppp__
```

xargs as an intermediate command AND a prefix:
```Bash
>>x="apple pop pain grain grout slam pounce"
>> echo $x | xargs -n1 | grep "^p" | xargs touch
```
#CBH WHY IS THE `-n1` NECESSARY?

#### eval
Given a command in the form of a string, evaluate the command
```bash
x="echo Yay!"
eval $x
>> Yay!
eval "echo $((1+1))"
>> 2
```
#CBH slides: eval 2 and eval 3

#### Shell Variables
`set -u`
#CBH 

### Here Document
#CBH 
Used to display large blocks of text.
- Lot better than echoing every line

## C++
C with OOP
Use `::` to access contents from a namespace
#### I/O
`#include <iostream>`
`std::cout` - console output, like a print statement. Use `<<` to provide it an argument
`std::endl` - line termination, just like `"\n"`
```Cpp
#include <iostream>
std::cout << "Hello, World!\n"
std::cout << "concat" << "en" << "8" << "\n";
std::cin >> mydata;
```
### Classes
Very similar to Java classes
```CPP
class Coordinate {
	private:
		int x, y;
	public:
		Coordinate() { //Constructor
			x=0;
			y=0;
		}
		Coordinate(int newX, int newY) {
			this->x = newX; //Note the arrow operator!
			this->y = newY;
		}
		//Getters:
		int getX() { return x; }
		int getY() { return y; }
}; //Classes need a semicolon!
```

Destructors are defined as:
`~Coordinate()`, they aren't necessary as C++ defines one by default. We can put some instructions to execute before its removed. This is called when using the `delete` command. Needed to free DMA. 

#### Defining outside of the class
Use `::` like so:
```CPP
double Coordinate::getDistOrigin() {
	return sqrt(x*x + y*y); //Note that we don't neeed to use `this`
}
```
#### Special Methods
We can override base behaviour of our objects.
```C++
Coordinate operator+(Coordinate b) {
	Coordinate result; //Create new Coordinate
	result.x = this->x + b.x;
	result.y = this->y + b.y;
	return result; //Return the new Coordinate
}
Coordinate newCoord = coordA + coordB;
```

#### public, private, static, const
**Public** means its visible and usable to anyone. 
- Methods are typically public
**Private** means its only visible from within the class. Attributes are often defined as private to force programmer to use accessors and mutators.
**Static** variables are shared among instances (with the class, not the instance). Think of keeping track of how many objects have been created.

**Const** means data can't be modified in a method. Not required, but good safeguarding.
```CPP
int getX() const {
	return x;
}
```

#### Vectors
`#include <vector>`
Vectors are dynamic. Create one with the specified type:
```C++
vector <type> varName;
vector <type>::iterator iterName;
iterName = varName.begin();
```

Useful vector commands include:
- `vec.push_back(element)` Append
- `vec.pop_back()` delete last
- `vec.capacity()` allocated memory
- `vec.size()` used memory
- `vec.insert(iterator + index, element)` location insertion
- `vec.erase(iterator + index)` location removal

`vec[index]`
`vec.at(index)`

**auto** means we don't need to specify the type of the vector
```CPP
for (auto& x: myVector)
	cout << x << " " ;
```


### Malloc and Calloc
Good to typecast them to the correct pointer type! Otherwise it's a `void` pointer (no idea what data type it's pointing to)
`int* newArr (int*) malloc(3*sizeof(int));`
`int* newArr (int*) calloc(3, sizeof(int));`

Malloc and Calloc means memory will be reserved until `free` is used OR the program terminates.


### Structures
kinda like a class, but just attributes (no methods, no private/public stuff, etc.)
```C
struct person {
	int age;
	char name[20];
	int height;
} p1, p2; //Creates two struct "objects"
struct person p3; //Note how we NEED `struct` keyword
p3 = {19, "Kevin", 100};
```
Note that all this is **contiguous** memory!

**Accessors**
```C
p1.age = 15;
strcpy(p1.name, "Michael");
p1.height = 180;
```

### Typedef
```C
typedef struct {
	int length;
	int width;
} rectangle; //Don't need `struct`, we can just use `rectangle`
rectangle r1, r2;

//We can also, later on after defining a struct,
typedef struct myStruct myType;
```


**Pointers to Structs**
```C
struct myStruct c, *p;
p->r === (*p).r //Arrow dereferences and accesses

void modifyAge(struct person *myP) {
	myP->age = myP->age + 1;
	(*myP).height = (*myP).height + 5;
}
```

Structures can be nested!
```C
struct comp{
	int ram_gb;
	char graphics[10];
};

struct cs_stud {
	char name[20];
	int year;
	float gpa;
	struct comp pc; //`struct comp` is the `type` here!
};

//Therefore, we maydo something like:
stud.comp.ram_gb; //if stud is an instance of cs_stud
```

#### Self-Nested Pointer!
```C
struct node {
	int key;
	stuct node *next;
};

struct node n1, n2;
n1.key = 15;
n1.next = &n2;

n2.key = 21;
n2.next = NULL;

printf("n1 = %d, n2 = $d\n", n1.key, n1.next->key);
```

### Linked Lists!
Think of CPS305 lists! Same basic concept!

In C, we must create a structure with an element acting as a pointer to the next element in the sequence. As long as we have a reference to the first element in the linked list, we can progress through the nodes to access any element.

We may want to have a `List` struct in addition to `Node` struct:
```C
struct Node {
	int val;
	struct Node *next;
};

struct List {
	int count;
	struct Node *head;
};
```
