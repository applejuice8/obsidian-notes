![JavaScript](https://cdn.svglogos.dev/logos/javascript.svg)

---

# 8 Data Types
```javascript
// String
let color = 'Yellow';
let car = '';

// Number
let length = 16;
let weight = 7.5;

// BigInt
let x = 1234567890123456789012345n;
let y = BigInt(1234567890123456789012345);

// Boolean
let x = true;
let y = false;

// Object
const person = {firstName: 'John', lastName: 'Doe'};
const cars = ['Saab', 'Volvo', 'BMW'];  // Array object
const date = new Date('2022-03-25'); // Date object

// Undefined
let x;

// Null
let x = null;

// Symbol
const x = Symbol();
```

```javascript
typeof 'John'  // 'string'
typeof 0.1  // 'number'
```

---

# Operators
```javascript
// Equal value
x == 5
x != 5

// Equal value and type
x === 5  // Equal value AND type
x !== 5  // Not equal value OR type
```

## Logical AND / OR Assignment Operator
```javascript
// Assign if value is truthy
let name = 'Adam';
name &&= 'Bob';  // name = 'Bob'

let name = '';
name &&= 'Bob';  // name = ''

// Assign if value is falsy
let name = 'Adam';
name ||= 'Bob';  // name = 'Adam'

let name = '';
name ||= 'Bob';  // name = 'Bob'
```

## Nullish Coalescing Assignment Operator (??=)
```javascript
// Assign if value is null / undefined
let name = null;
let name = undefined;
let name;
name ??= 'Bob';  // name = 'Bob'

let age = 0;
age ??= 10;  // age = 0 (0 is not undefined)
```

---

# Spread Operator (...)
```javascript
// Unpack
const a = [1, 2, 3];
myFunc(...a);  // myFunc(1, 2, 3)

// Merge arrays
const a = [1, 2];
const b = [3, 4];
const c = [...a, ...b];  // [1, 2, 3, 4]

// Merge objects
const a = {name: 'Alice', age: 20};
const b = {city: 'KL', age: 21};  // Override age
const c = {...a, ...b};  // {name: 'Alice', age: 21, city: 'KL'}

// Insert between
const a = [1, 2, 3];
const b = [0, ...a, 4, 5];  // [0, 1, 2, 3, 4, 5]

// Convert nodelist, string to array
const divs = document.querySelectorAll('div');
const a = [...divs];  // Now can use .map(), .filter()
const a = [...'Hello'];  // ['H', 'e', 'l', 'l', 'o']

// Separate arrays, objects
const [first, ...rest] = [1, 2, 3, 4];
console.log(first);  // 1
console.log(rest);  // [2, 3, 4]

const {name, ...details} = {name: 'Alice', age: 20, city: 'KL'};
console.log(name);  // 'Alice'
console.log(details);  // {age: 20, city: 'KL'}
```

---

# Spread vs Rest
```javascript
// Spread (Separate arguments)
const nums = [1, 2, 3];
Math.max(...nums);  // Math.max(1, 2, 3)

// Rest (Gather arguments)
function sum(...nums) {
	return nums.sort();
}
sum(1, 2, 3);  // sum([1, 2, 3])
```

---

# Conditionals
```javascript
// If statement
if (age > 60) {
	res = 'senior';
} else if (age > 18) {
	res = 'adult';
} else {
	res = 'child';
}

// Ternary operator
age = (age > 18) ? 'adult' : 'child';

// Switch statement
switch (role) {
	case 'customer':
		print('Hello');
		break;
	case 14:
	case 16:
		print('Hey 14 or 16');
		break;
	default:
		print('oops');
}
```

---

# Loops
## For Loop
```javascript
for (let i = 0; i < 10; i++) {
	console.log(cars[i]);
}

// Each expression is optional
for (; i < 10; i++)
for (let i = 0; ; i++)
for (let i = 0; i < 10;)
```

## While Loop, Do While Loop
```javascript
// While
let i = 0;
while (i < 10) {
	console.log(cars[i]);
	i++;
}

// Do while
let i = 0;
do {
	console.log(cars[i]);
	i++;
} while (i < 10);
```

## Loop Scope
```javascript
// let inside loop
let i = 5;
for (let i = 0; i < 10; i++) {}
// i = 5

// No let inside loop
let i = 5;
for (i = 0; i < 10; i++) {}
// i = 10
```

## Label Loops
- break can only break closest loop, labels can break outer loop
```javascript
loop1: for (let i = 1; i < 5; i++) {
	loop2: for (let j = 1; j < 5; j++) {
		if (i === 3) {
			break loop1;
		}
		console.log(cars[i][j]);
	}
}
```

---

# Strings
## Template Strings
```javascript
// Can use both single, double quotes inside
let a = `It's "show time".`;

// Can multiple lines
let a =
`abc
ad`;

// f strings
let a = `Hello, ${name}.`;
let a = `Price: ${(a + b) * 1.06}.`
```

## String Methods
```javascript
let a = 'abc';

a.length;
a.concat(' ', 'def', 'ghi');  // 'abc' + ' ' + 'def' + 'ghi'
a.repeat(10);
a.split(',');

// Get letter
a.charAt(2);
a.at(2);  // Same as charAt() but supports negative index (Same as a[2])

// Extract substring
a.slice(1, 4);  // From index 1 to 3
a.slice(1);  // From index 1 to end

// Upper, lower
a.toUpperCase();
a.toLowerCase();

// Remove whitespace
a.trim();
a.trimStart();
a.trimEnd();

// Add padding
a.padStart(4, 'X');  // Add X until length reaches 4
a.padEnd(4, 'X');

// Replace
a.replace('a', 'e');
a.replace(/MICROSOFT/i, 'W3Schools');  // Supports regular expressions
a.replaceAll('a', 'e');

// Find substring
a.indexOf('house');  // Returns index of h in a (-1 if not found)
a.indexOf('house', 3);  // Start search from index 3
a.lastIndexOf('house');

a.search(/house/);  // Cannot take second parameter, but can take regular expressions

a.includes('house');
a.includes('house', 3);  // Start search from index 3
a.startsWith('house');
a.startsWith('house', 3);
a.endsWith('house');
a.endsWith('house', 3);
```

---

# Numbers
- Numbers in js always double (64-bit float)
- No byte, short, int, long, float
- JS always try to convert to number for arithmetic (Except + for concat)
```javascript
let a = '100' / '10';

// NaN
let a = 100 / 'apple';
isNaN(a);
typeof NaN;  // Number

// Infinity
let a = 100 / 0;
typeof -Infinity;  // Number

// Hexadecimal
let x = 0xFF;
```

## Number Methods
```javascript
// Change base
a.toString(32);
a.toString(16);
a.toString(8);

// Round (Number of decimals)
a.toFixed();  // 10
a.toFixed(2);  // 9.66

// Significant digits
a.toPrecision();  // 9.656
a.toPrecision(2);  // 9.7

// Exponential (Number of decimals)
a.toExponential();  // 9.656e+0
a.toExponential(6);  // 9.656000e+0
```

## Static Methods for Conversion
```javascript
a = Number(true);  // Convert to number

// Convert to int
parseInt('-10.33');  // Truncate(-10)
parseInt('2 7 19');  // Only first number (2)
parseInt('3 years');  // 3
parseInt('years 3');  // NaN

parseFloat('2.1 4.2 10');  // 2.1
```

---

# Functions
```javascript
function myFunc() {
	console.log('Hi');
}

const myFunc = function() {
	console.log('Hi');
}
```

---

# Arrow Functions
```javascript
// Normal
function add(a, b) {
	return a + b;
}

// Arrow
const add = (a, b) => a + b;

// Need parentheses when return objects
const createObject = (name, age) => ({name, age});

// Multiple lines
const myFunc = (a, b) => {
	c = a + b;
	console.log(c);
	return c;
}

// Return functions
const multiplyBy = factor => num => num * factor;
const double = multiplyBy(2);  // double = num => num * factor;
console.log(double(5));

// Array methods
const nums = [1, 2, 3, 4, 5];

const doubled = nums.map(n => n * 2);  // [2, 4, 6, 8, 10]
const evens = nums.filter(n => n % 2 === 0);  // [2, 4]
const total = nums.reduce((a, b) => a + b, 0);  // 15
```

---

# Objects
```javascript
// Method 1
const car = {type: 'Fiat', model: '500', color: 'white'};

// Method 2
const person = {};
person.firstName = 'John';
person['lastName'] = 'Doe';

// Can also store function as an attribute
person.move = function() {
	console.log('moving');
}
```

## Object Constructor
```javascript
function Person(firstName, lastName, age) {
	this.firstName = firstName;
	this.lastName = lastName;
	this.age = age;
}

const person1 = new Person('John', 'Cena', 10);
```

---

# Arrays
```javascript
// Method 1
const cars = ['BMW', 'Mercedes', 'Volvo'];

// Method 2
const cars = [];
cars[0] = 'BMW';
cars[1] = 'Mercedes';
```

## Array Methods
```javascript
cars.toString();  // BMW,Mercedes,Volvo
cars.join(' * ');  // BMW * Mercedes * Volvo
cars.length;
cars.sort();
cars.concat(arr1, arr2);

cars.unshift('abc');  // Add front
cars.push('abc');  // Add last
c = cars.pop();  // Last
c = cars.shift();  // First

cars.copyWithin(2, 1);  // Copy to index 2, all elements from index 1
cars.copyWithin(2, 1, 3);  // Copy to index 2, all elements from index 1 to index 3
```

## Loop Array
```javascript
// Method 1
for (let i = 0; i < cars.length; i++) {
	console.log(cars[i]);
}

// Method 2
cars.forEach(myFunc);

cars.forEach(car => console.log(car));

cars.forEach(car => {
	console.log(car);
});
```

---

# Constant Arrays, Objects
- Can change / add element
- But cannot reassign
```javascript
const cars = ['Saab', 'Volvo', 'BMW'];

// Can
cars[0] = 'Toyota';  // Change element
cars.push('Toyota');  // Add element

// Cannot reassign array
cars = ['Saab', 'BMW', 'Volvo'];
```

---

# Animations
```javascript
setInterval(() => {
	console.log('helo');
}, 5);
```

