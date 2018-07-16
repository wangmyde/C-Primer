## 1 Basics
### 1. Every C++ program contains one or more functions, one of which must be named **main**.
```
int main() 
{             //curly brace.
    return 0;
}
```
- int: a return type
- main: a function name
- ( ): a paremeter list enclosed in parentheses
- a function body: a block of statements starting with an open curly brace and ending with a close curly
- When a return statement includes a value, the value returned must have a type that is compatible with the return type of the function. In this case, the return type of main is int and the return value is 0, which is an int.
- **Semicolons** markthe end of most statements in C++
### 2. Built-in type Type, such as int, defined by the language.
### 3. Running the Compiler from the Command Line
> - `cl /EHsc ex1_1.cpp`: cl command invokes the compiler, and /EHsc is the compiler option that turns on standard exception handling.  
> - To run an executable on Windows, we supply the executable file name and can omit the `.exe` file extension: 
`ex1_1`
> - To see the status on a Windows system, we write
> `echo %ERRORLEVEL%`
### 4. Standard Input and Output Objects
- Type: istream
Objective: cin
- Type: istream
Objective: cout, cerr, clog    
cerr, referred to as the standard error, for warning and error messages and clog for general information about the execution of the 
program.
### 5. A Program That Uses the IO Library
```
#include <iostream> // tells the compiler that we want to use the iostream library. The name inside angle
                    // brackets (iostream in this case) refers to a header.
int main()
{
std::cout << "Enter two numbers:" << std::endl;
int v1 = 0, v2 = 0;
std::cin >> v1 >> v2;
std::cout << "The sum of " << v1 << " and " << v2
<< " is " << v1 + v2 << std::endl;
return 0;
}
```
- `#include` directives must appear outside any function. All the `#include` directives for a program are put at the beginning of the source file.
- `endl`, which is a special value called a manipulator. Writing `endl` has the effect of ending the current line and flushing the buffer associated with that device.
- The prefix `std::` indicates that the names cout and endl are defined inside the namespace named std. All the names defined by the standard library are in the `std` namespace.
- `::`: scope operator
### 6. Comments
- `//`: a single line comment
- `/* and */`: a comment pair. One comment pair cannot appear inside another.
### 7. while
A while statement repeatedly executes a section of code so long as a given condition is true.
```
#include <iostream>
int main()
{
	int sum = 0, val = 1;
	// keep executing the while as long as val is less than or equal to 10
	while (val <= 10) {                           
		sum += val; // assigns sum + val to sum
		++val; // add 1 to val
	}
	std::cout << "Sum of 1 to 10 inclusive is "
            << sum << std::endl;
	return 0;
}
```
- A `while` has the form
> while (condition)
>>     statement
- `+=`: the compound assignment operator
- `++val`: add 1 to val, `val = val + 1`
### 8. for
```
#include <iostream>
int main()
{
    int sum = 0;
    // sum values from 1 through 10 inclusive
    for (int val = 1; val <= 10; ++val)  // header: an initstatement, a condition, and an expression
      sum += val;  // equivalent to sum = sum + val;
    std::cout << "Sum of 1 to 10 inclusive is "
              << sum << std::endl;
    return 0;
}
```
- Create val and initialize it to 1.
- Test whether val is less than or equal to 10. If the test succeeds, execute the for body. If the test fails, exit the loop and continue execution with the first statement following the for body.
- Increment val.
- Repeat the test in step 2, continuing with the remaining steps as long as the condition is true.
### 9. if
- `==`: the equality operator
### 10. class
A class defines a type along with a collection of operations that are related to that type.
```
Sales_item item;
```
- we are saying that item is an object of type Sales_item. We often contract the phrase “an object of type Sales_item” to “a Sales_item object” or even more simply to “a Sales_item.”
- the class author determines all the operations that can be used on objects of the class type.
```
#include <iostream>
#include "Sales_item.h"
```
- Headers from the standard library are enclosed in angle brackets (< >). 
- Those that are not part of the library are enclosed in **double quotes (" ")**.
- **member function/ methods**: A member function is a function that is defined as part of a class. 
```
item1.isbn() == item2.isbn()  // isbn is a member function or method
```
- `''.''`operator: The dot operator applies only to objects of class type.
- `()`operator: The call operator is a pair of parentheses that enclose a (possibly empty) list of arguments.
- argument: Value passed to a function.
