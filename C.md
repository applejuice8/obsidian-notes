![C](https://cdn.svglogos.dev/logos/c.svg)

---

# Commands
```bash
# Install c compiler (clang)
xcode-select --install

# Compile
gcc filename.c -o exefilename
gcc main.c -o main

# Run
./exefilename
./main
```

---

# main
```c
#include <stdio.h>  

int main() {  
	printf("Hello World!\n");
	printf("I am learning C.");
	return 0;
}
```

---

# Comments
```c
// Comment

/*
Multi-line comment
*/
```

---

# Data Types
```c
#include <stdio.h>
#include <stdbool.h>  // For bool

int main() {
    // Integer types
    int myInt = 15;                   // int
    short myShort = 10;               // short int
    long myLong = 1000000;            // long int
    long long myLongLong = 1000000000;// long long int
    unsigned int myUInt = 20;         // unsigned int (No negative)

    // Floating point types
    float myFloat = 5.99f;            // float
    double myDouble = 9.98;           // double
    long double myLongDouble = 19.99L;// long double

    // Character type
    char myChar = 'D';

    // Boolean type (Need import)
    bool myBool = true;

    // Printing variables
    printf("Integer: %d\n", myInt);
    printf("Short: %d\n", myShort);
    printf("Long: %ld\n", myLong);
    printf("Long Long: %lld\n", myLongLong);
    printf("Unsigned Int: %u\n", myUInt);

    printf("Float: %f\n", myFloat);
    printf("Double: %lf\n", myDouble);
    printf("Long Double: %Lf\n", myLongDouble);

    printf("Char: %c\n", myChar);
    printf("Boolean: %d\n", myBool);  // Prints 1 (true), 0 (false)

    return 0;
}
```

## Constants
```c
const int MINUTES_PER_HOUR = 60;

// Cannot separate for constant
int minutesPerHour;
minutesPerHour = 60;
```

## Decimal Precision
```c
float f = 3.5;
printf("%.4f", f);  // Default 6
```

## Memory Size
```c
// %zu for size_t data type
printf("%zu\n", sizeof(myInt));
printf("%zu\n", sizeof(myFloat));
printf("%zu\n", sizeof(myDouble));
printf("%zu\n", sizeof(myChar));
```

## Conversion
```c
// Implicit conversion
float myFloat = 9;  // int to float
int myInt = 9.99;  // float to int

// Explicit conversion
float sum = (float) 5 / 2;
float sum = 5 / 2;  // 2.000 (Conversion after operation)
```

---

# Conditionals
```c
if (20 >= 19 && 19 >= 18) {
	printf("1");
} else if (20 >= 19 || 19 >= 18) {
	printf("2");
} else if (!(10 > 9)) {
	printf("3");
} else {
	printf("4");
}
```

## Ternary Operator
```c
(time < 18) ? printf("Morning") : printf("Night");
```

## Switch Statement
```c
switch (day) {
	case 6:
		printf("1");
		break;
	case 7:
	case 8:
		printf("2");
		break;
	default:
		printf("3");
}
```

---

# Loops
```c
int i = 0;

// while loop
while (i < 5) {
	  printf("%d\n", i);
	  i++;
}

// do...while loop
do {
  printf("%d\n", i);
  i++;
} while (i < 5);

// for loop
for (int i = 0; i < 5; i++) {
	printf("%d\n", i);
}
```

## break, continue
```c
for (i = 0; i < 6; i++) {
	if (i == 2) {
		continue;
	} else if (i == 4) {
		break;
	}
	printf("%d\n", i);
}
```

---

# Arrays
```c
// Method 1
int myNumbers[] = {25, 50, 75, 100};

// Method 2
int myNumbers[4];

myNumbers[0] = 25;
myNumbers[1] = 50;
myNumbers[2] = 75;
myNumbers[3] = 100;
```

## Methods
```c
// Print element
printf("%d", myNumbers[0]);

// Modify
myNumbers[0] = 33;

// Length
int length = sizeof(myValues) / sizeof(myValues[0]);
```

## Loop Through Array
```c
int myNumbers[] = {25, 50, 75, 100};
int length = sizeof(myNumbers) / sizeof(myNumbers[0]);

for (int i = 0; i < length; i++) {
	printf("%d\n", myNumbers[i]);
}
```

## Multidimensional Array
```c
int matrix[2][3] = { {1, 4, 2}, {3, 6, 8} };
```

---

# String
- Array of chars
```c
char greetings[] = "Hello World!";
char greetings[] = {'H', 'e', 'l', 'l', 'o', '', 'W', 'o', 'r', 'l', 'd', '!', '\0'};

greetings[0] = 'J';

printf("%s", greetings);
printf("%c", greetings[0]);
```

## Length
```c
#include <string.h>

// sizeof counts \0 but strlen doesn't
char alphabet[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
printf("%zu\n", strlen(alphabet));   // 26
printf("%zu\n", sizeof(alphabet));   // 27

// sizeof returns memory space, includes unused space
char alphabet[50] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
printf("%zu\n", strlen(alphabet));   // 26
printf("%zu\n", sizeof(alphabet));   // 50
```

## String Functions
```c
#include <string.h>

// Concat
strcat(str1, str2);

// Copy to, from (str1 to str2)
strcpy(str2, str1);

// Compare strings (Returns 0 if equal)
strcmp(str1, str2)
```

---

# User Input
```c
printf("Enter your age: ");
scanf("%d", &age);

// Multiple inputs
printf("Enter two numbers: ");
scanf("%d %d", &a, &b);
```

## Char Input
- After we enter the char, we will click `ENTER`
- If no space before %c, `scanf` reads `\n (ENTER)` instead of the char
```c
printf("Enter a letter: ");
scanf(" %c", &letter);  // Space before %c
```

## String Input
- May read `\n (ENTER)` instead of string
- `scanf` stops reading after whitespace, cannot store input like 'hello world'
- `scanf` may overflow, which is unsafe
```c
// Method 1 (scanf)
char name[50];
printf("Enter your full name: ");
scanf("%s", name);

// Method 2 (fgets)
char name[50];
printf("Enter your full name: ");
fgets(name, sizeof(name), stdin);

// Clear buffer before reading from buffer with fgets
while (getchar() != '\n' && getchar() != EOF);
```

## sscanf
```c
char *str = "Ram Manager 30";

char name[10], designation[10];
int age;

sscanf(str, "%s %s %d", name, designation, &age);
```

---

# Math Library
```c
#include <math.h>

printf("%f", sqrt(16));
printf("%f", ceil(1.4));
printf("%f", floor(1.4));
printf("%f", pow(4, 3));
```

---

# Pointers
```c
int age = 20;

// Method 1
printf("%p\n", &age);  // Prints memory address (0x7ffee3b4a9c8)
printf("%d\n", age);  // Prints value (20)

// Method 2 (Dereference operator)
int *ptr = &age;  // Pointer points to location
printf("%p\n", ptr);  // Reference (Prints memory address)
printf("%d\n", *ptr);  // Dereference (Go to location and print value)
```

## * has 2 meanings
```c
int *ptr;  // Pointer variable
printf("%d\n", *ptr);  // Dereference operator
```

## Pointers for Arrays
- Array is actually pointer to the first element
```c
int nums[4] = {25, 50, 75, 100};

// Method 1 (Directly use nums)
printf("%d\n", *nums);  // First element (25)
printf("%d\n", *(nums + 1));  // Second element (50)

// Method 2a (Pointer doesn't move)
int *ptr = nums;
printf("%d\n", *ptr);  // First element (25)
printf("%d\n", *(ptr + 1));  // Second element (50)

// Method 2b (Or increment the pointer)
int *ptr = nums;
printf("%d\n", *ptr);
ptr++;
printf("%d\n", *ptr);
```

## Count Number of Elements Between
```c
int myNumbers[5] = {10, 20, 30, 40, 50};
int *start = &myNumbers[1];
int *end = &myNumbers[4];

printf("%ld\n", end - start); // 3 elements apart
```

## Pointer to Pointer
```c
int num = 10;
int *ptr = &num;  // int * (Pointer to int)
int **pptr = &ptr;  // int ** (Pointer to pointer int *)

printf("myNum = %d\n", num);
printf("*ptr = %d\n", *ptr);
printf("**pptr = %d\n", **pptr);
```

---

# Functions
```c
void myFunc(char name[]) {
	printf("Hello, %s\n", name);
}

// Multiple params
int sum(int x, int y) {
	return x + y;
}
```

```c
// Function declaration
int myFunction(int x, int y);

int main() {
	myFunction();  // If no declaration, here error (Unknown func)
	return 0;
}

// Function definition
int myFunction(int x, int y) {
	return x + y;
}
```

## Inline Functions
- Asks compiler to insert its code to where it's called, instead of jumping to it
- Make frequently used functions a bit faster
- Don't use for large / recursive functions
```c
int sum(int x, int y) {
	return x + y;
}

inline int sum(int x, int y) {
	return x + y;
}

// Example
int main(void) {
	printf("%d", sum(5, 3));  // printf("%d", 5 + 3);
	return 0;
}
```

---

# Function Pointers
```c
int add(int a, int b) {
	return a + b;
}

int method1(void) {
	int (*ptr)(int, int) = add;
	return ptr(5, 3);
}

int method2(int (*ptr)(int a, int b)) {
	return ptr(5, 3);
}

int main(void) {
	method1();
	method2(add);
	return 0;
}
```

## Function Pointer Array
- Cannot pass plain function into array
```c
int add(int a, int b);
int sub(int a, int b);

int main() {
	int (*operations[2])(int, int) = { add, sub };

	for (int i = 0; i < 2; i++) {
		operations[i](5, 3);
	}
	return 0;
}
```

## Allows Polymorphism
```c
typedef struct {
    void (*speak)();
} Animal;

Animal dog = { dogSpeak };
Animal cat = { catSpeak };

dog.speak();
cat.speak();
```

---

# qsort()
- Sort arrays
```c
int main() {
  int numbers[] = { 5, 2, 9, 1, 7 };
  int size = sizeof(numbers) / sizeof(numbers[0]);

  qsort(numbers, size, sizeof(int), compare);
  return 0;
}
```

## Write Own Compare Function
- Convert to pointer of int `(int*)a` and dereference that pointer `*(int*)a`
- Return left if <0
- Return right if >0
- Unchanged if == 0
```c
// Ascending
int compare_asc(const void *a, const void *b) {
    return *(int*)a - *(int*)b;
}

// Descending
int compare_desc(const void *a, const void *b) {
    return *(int*)b - *(int*)a;
}

// Custom sort (Even first, then odd)
int compare_even_first(const void *a, const void *b) {
    int x = *(int*)a;
    int y = *(int*)b;

    if ((x % 2 == 0) && (y % 2 != 0)) return -1;  // a first (Even)
    if ((x % 2 != 0) && (y % 2 == 0)) return 1;  // b first (Even)
    return x - y;  // If both same even/odd, then ascending
}
```

---

# File Handling
## Write, Append
```c
FILE *fp = fopen("filename.txt", "w");  // w, a
if (fp == NULL) return 1;

fprintf(fp, "Some text");
fclose(fp);
```

## Read
```c
FILE *fp = fopen("filename.txt", "r");
if (fp == NULL) return 1;

char line[100];
while(fgets(line, sizeof(line), fp)) {  // Max size to read
	printf("%s", line);
}
fclose(fp);
```

## Formatted Read / Write
```c
// Formatted write
FILE *fp = fopen("numbers.txt", "w");
if (fp == NULL) return 1;

char *name = "Jason", *age = "18", *contact = "123";
fprintf(fp, "%s,%s,%s\n", name, age, contact);
fclose(fp);

// Formatted read (scanf)
fp = fopen("numbers.txt", "r");
if (fp == NULL) return 1;

char name[50], age[10], contact[20];
fscanf(fp, "%s,%s,%s\n", &name, &age, &contact);
fclose(fp);
```

## fprintf() vs fput()
```c
fputs("Hello", fp);
fprintf(fp, "Hello");

// fprintf can format strings
fprintf(fp, "Hello %s", name);
```


