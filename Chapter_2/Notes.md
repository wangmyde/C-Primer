# Chapter 2. Variables and Basic Types
### 1. Arithmetic Types
- Integral types: may be signed or unsigned. We obtain the corresponding `unsigned` type by adding unsigned to the type
> - character
>> char: a char is the same size as a single machine byte. char is **not** the same type as signed char
> - boolean types
- floating point types
### 2. Deciding which Type to Use
- the value can not be negative --> use an `unsigned` type
- `int` is not enough --> use `long long`
- only use `signed char` or `unsigned char`, but not `char`
- only use `double` for floating-point computations
### 3. Type Conversions
```
bool b = 42; // b is true
int i = b; // i has value 1
i = 3.14; // i has value 3
double pi = i; // pi has value 3.0
unsigned char c = -1; // assuming 8-bit chars, c has value 255
signed char c2 = 256; // assuming 8-bit chars, the value of c2 is undefined. 
                      //The program might appear to work, it might crash, or it might produce garbage values.
```
### 4. Don’t Mix Signed and Unsigned Types in Expressions
### 5. Literal
```
20 /* decimal */ 
024 /* octal */ 
0x14 /* hexadecimal */
```
- scientific notation
> the exponent is indicated by either `E` or `e`: `3.14159E0` `0e0`. By default, floating-point literals have type double.
```
'a' // character literal
"Hello World!" // string literal
```
- The type of a string literal is array of constant `char`s
- The compiler appends a null character (`’\0’`) to every string literal. Thus, the actual size of a string literal is one more than its apparent size.
- nonprintable
```
newline \n          horizontal tab \t    alert (bell) \a
vertical tab \v     backspace \b double  quote \"
backslash \\        question mark \?     single quote \'
carriage return \r  formfeed \f
```
- `ture` or `false`: are literals of type bool
### 6. Variable Definitions
```
int sum = 0, value, units_sold = 0;  
// value without an initializer, the variable is default initialized.
// Such variables are given the “default” value. What that default value is depends on
// the type of the variable and may also depend on where the variable is defined.
// Variables defined outside any function body are initialized to zero.
// variables of built-in type .defined inside a function are uninitialized.
// The value of an uninitialized variable of built-in type is undefined causing an error.
```
A simple variable definition consists of a **type specifier**, followed by a list of one or more **variable names** separated by **commas**, and ends with a **semicolon**.
- objective: an object is a region of memory that has a type.
```
double price = 109.99, discount = price * 0.16;  
//it is possible to initialize a variable to the value of one defined
//earlier in the same definition.
```
- Initialization and assignment are different operations in C++.
初始化不是赋值，初始化的含义是创建变量时赋予其一个初始值，而赋值的含义是把对象的当前值擦除，而以一个新值来替代。
```
// four different ways to define an int variable
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
```
- **initializing every object of built-in type**
