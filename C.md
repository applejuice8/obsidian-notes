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
- If no space before %c, `scanf` reads `\n (ENTER)` instead of the char
```c
printf("Enter a letter: ");
scanf(" %c", &letter);  // Space before %c
```

## String Input
- May read `\n (ENTER)` instead of string
```c
// Method 1 (Stops reading after whitespace) (May overflow)
char name[50];
printf("Enter your full name: ");
scanf("%s", name);

// Method 2
char name[50];
printf("Enter your full name: ");
fgets(name, sizeof(name), stdin);

// Clear buffer before reading from buffer with fgets
while (getchar() != '\n' && getchar() != EOF);
```

