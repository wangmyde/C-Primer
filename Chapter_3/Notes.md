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

### 5. Arrays
Arrays have fixed size. If you don’t know exactly how many elements you need, use a `vector`.
```
unsigned cnt = 42; // not a constant expression
constexpr unsigned sz = 42; // constant expression
// constexpr see § 2.4.4 (p. 66)
int arr[10]; // array of ten ints
int *parr[sz]; // array of 42 pointers to int
string bad[cnt]; // error: cnt is not a constant expression
string strs[get_size()]; // ok if get_size is constexpr, error otherwise
```
When we define an array, we must specify a type for the array. We cannot use `auto` to deduce the type from a list of initializers. There are no arrays of `reference`.

Explicitly Initializing Array Elements
```
int ia1[sz] = {0,1,2}; // array of three ints with values 0, 1, 2
int a2[] = {0, 1, 2}; // an array of dimension 3
int a3[5] = {0, 1, 2}; // equivalent to a3[] = {0, 1, 2, 0, 0}
string a4[3] = {"hi", "bye"}; // same as a4[] = {"hi", "bye", ""}
int a5[2] = {0,1,2}; // error: too many initializers
```

Character Arrays Are Special
```
char a1[] = {'C', '+', '+'}; // list initialization, no null
char a2[] = {'C', '+', '+', '\0'}; // list initialization, explicit null
char a3[] = "C++"; // null terminator added automatically
const char a4[6] = "Daniel"; // error: no space for the null!
```
The dimension of a1 is 3; the dimensions of a2 and a3 are both 4. The definition of a4 is in error. Although the literal contains only six explicit characters, the array size must be at least seven—six to hold the literal and one for the null.

**No Copy or Assignment**
We cannot initialize an array as a copy of another array, nor is it legal to assign one array to another是不准数组和数组之间，而不是数组元素之间的赋值:
```
int a[] = {0, 1, 2}; // array of three ints
int a2[] = a; // error: cannot initialize one array with another
a2 = a; // error: cannot assign one array to another
```

Understanding Complicated Array Declarations
```
int *ptrs[10]; // ptrs is an array of ten pointers to int
int &refs[10] = /* ? */; // error: no arrays of references
int (*Parray)[10] = &arr; // Parray points to an array of ten ints
int (&arrRef)[10] = arr; // arrRef refers to an array of ten ints
```
解释待补充

Accessing the Elements of an Array
When we use a variable to subscript an array, we normally should define that variable to have type `size_t`. `size_t` is a machine-specific unsigned type that is guaranteed to be large enough to hold the size of any object in memory. The size_t type is defined in the `cstddef` header, which is the C++ version of the stddef.h header from the C library.

**Pointers and Arrays**
```
string nums[] = {"one", "two", "three"}; // array of strings
string *p = &nums[0]; // p points to the first element in nums
string *p2 = nums; // equivalent to p2 = &nums[0]
```
也就是说，使用数组类型的对象==使用一个该数组首元素的指针。即拍p2其实是首元素的地址。
we can use the increment operator to move from one element in an array to the next:
```
int arr[] = {0,1,2,3,4,5,6,7,8,9};
int *p = arr; // p points to the first element in arr
++p; // p points to arr[1]
```

The Library `begin` and `end` Functions
```
int ia[] = {0,1,2,3,4,5,6,7,8,9}; // ia is an array of ten ints
int *beg = begin(ia); // pointer to the first element in ia
int *last = end(ia); // pointer one past the last element in ia
```
`begin` returns a pointer to the first, and `end` returns a pointer one past the last element in the given array: These functions are defined in the `iterator` header.
```
// pbeg points to the first and pend points just past the last element in arr
int *pbeg = begin(arr), *pend = end(arr);
// find the first negative element, stopping if we've seen all the elements
while (pbeg != pend && *pbeg >= 0)
++pbeg;
```

Pointer Arithmetic
```
constexpr size_t sz = 5;
int arr[sz] = {1,2,3,4,5};
int *ip = arr; // equivalent to int *ip = &arr[0]
int *ip2 = ip + 4; // ip2 points to arr[4], the last element in arr
```

```
int *p = arr + sz; // use caution -- do not dereference! 翻译：使用警告：不要解引用
int *p2 = arr + 10; // error: arr has only 5 elements; p2 has undefined value
```
当给arr加上sz时，编译器自动地将arr转换成指向数组arr中首元素的指针。

```
auto n = end(arr) - begin(arr); // n is 5, the number of elements in arr
```
两个指针相减的结果是它们之间的距离。此例即为arr元素的个数。
The result of subtracting two pointers is a library type named `ptrdiff_t`. Like `size_t`, the `ptrdiff_t` type is a machine-specific type and is defined in the `cstddef` header. Because subtraction might yield a **negative** distance, ptrdiff_t is a **signed** integral type.
```
int *b = arr, *e = arr + sz;
while (b < e) {
// use *b
++b;
}
```
两个指针指向同一个数组的元素可进行比较。但指向不同的数组则不行。

Interaction between Dereference and Pointer Arithmetic
```
int ia[] = {0,2,4,6,8}; // array with 5 elements of type int
int last = *(ia + 4); // ok: initializes last to 8, the value of ia[4]
```
The expression `*(ia + 4)` calculates the address four elements past `ia` and dereferences the resulting pointer. This expression is equivalent to writing `ia[4]`.`*(ia + 4)`的括号是必须的。

Subscripts and Pointers
```
int ia[] = {0,2,4,6,8}; // array with 5 elements of type int
int i = ia[2]; // ia is converted to a pointer to the first element in ia
               // ia[2] fetches the element to which (ia + 2) points
int *p = ia; // p points to the first element in ia
i = *(p + 2); // equivalent to i = ia[2]
int *p = &ia[2]; // p points to the element indexed by 2
int j = p[1]; // p[1] is equivalent to *(p + 1),
// p[1] is the same element as ia[3]
int k = p[-2]; // p[-2] is the same element as ia[0]
```

### 7. Multidimensional Arrays
**There are no multidimensional arrays in C++. What are commonly referred to as multidimensional arrays are actually arrays of arrays.**
```
int ia[3][4]; // array of size 3; each element is an array of ints of size 4
// array of size 10; each element is a 20-element array whose elements are arrays of 30
ints
int arr[10][20][30] = {0}; // initialize all elements to 0
```

Initializing the Elements of a Multidimensional Array
> 
```
int ia[3][4] = { // three elements; each element is an array of size 4
{0, 1, 2, 3}, // initializers for the row indexed by 0
{4, 5, 6, 7}, // initializers for the row indexed by 1
{8, 9, 10, 11} // initializers for the row indexed by 2
};
```
>
```
// equivalent initialization without the optional nested braces for each row
int ia[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};
```
>
```
// explicitly initialize only element 0 in each row
int ia[3][4] = {{ 0 }, { 4 }, { 8 }};
```
>
```
// explicitly initialize row 0; the remaining elements are value initialized
int ix[3][4] = {0, 3, 6, 9};
```
Subscripting a Multidimensional Array
```
// assigns the first element of arr to the last element in the last row of ia
ia[2][3] = arr[0][0][0];
```
```
constexpr size_t rowCnt = 3, colCnt = 4;

Pointers and Multidimensional Arrays

int ia[rowCnt][colCnt]; // 12 uninitialized elements
// for each row
for (size_t i = 0; i != rowCnt; ++i) {
// for each column within the row
    for (size_t j = 0; j != colCnt; ++j) {
    // assign the element's positional index as its value
        ia[i][j] = i * colCnt + j;
    }
}
```
