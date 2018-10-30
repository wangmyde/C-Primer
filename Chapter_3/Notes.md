### 1. Namespace using Declarations
A `using` declaration has the form
`using namespace::name;`
Once the `using` declaration has been made, we can access name directly:
```
// using declaration; when we use the name cin, we get the one from the namespace
std
using std::cin;
int main()
{
    int i;
    cin >> i; // ok: cin is a synonym for std::cin
    cout << i; // error: no using declaration; we must use the full name
    std::cout << i; // ok: explicitly use cout from namepsace std
    return 0;
}
```
Each `using` declaration introduces a single namespace member.
```
#include <iostream>
// using declarations for names from the standard library
using std::cin;
using std::cout; using std::endl;
int main()
{
    cout << "Enter two numbers:" << endl;
    int v1, v2;
    cin >> v1 >> v2;
    cout << "The sum of " << v1 << " and " << v2
    << " is " << v1 + v2 << endl;
    return 0;
}
```
**Headers Should Not Include `using` Declarations**
Code inside headers ordinarily should not use using declarations. The reason is that the contents of a header are copied into the including program’s text. If a header has a using declaration, then every program that includes that header gets that same using declaration. As a result, a program that didn’t intend to use the specified library name might encounter unexpected name conflicts.
