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

### 3. Vector
A `vector` is a collection of objects, all of which have the same type.
A vector is a `class template`. To use a `vector`, we must include the appropriate header.
```
#include <vector>
using std::vector;
```
`vector` is a template, not a type. Types generated from `vector` must include the element type, for example, `vector<int>`.
`reference`s are not objects, we cannot have a vector of references.
#### 1. Defining and Initializing `vector`s
##### 1.1. 
```
vector<string> svec; // default initialization; svec has no elements
vector<int> ivec; // initially empty
// give ivec some values
vector<int> ivec2(ivec); // copy elements of ivec into ivec2
vector<int> ivec3 = ivec; // copy elements of ivec into ivec3
vector<string> svec(ivec2); // error: svec holds strings, not ints
```
When we copy a `vector`, each element in the new `vector` is a copy of the corresponding element in the original `vector`. The two `vector`s must be the same type
##### 1.2. List Initializing a `vector` 
```
vector<string> articles = {"a", "an", "the"};
```
The resulting `vector` has three elements; the first holds the `string` "a", the second holds "an", and the last is "the".
```
vector<string> v1{"a", "an", "the"}; // list initialization
```
##### 1.3. Creating a Specified Number of Elements
```
vector<int> ivec(10, -1); // ten int elements, each initialized to -1
vector<string> svec(10, "hi!"); // ten strings; each element is "hi!"
```
##### 1.4 Value Initialization
```
vector<int> ivec(10); // ten elements, each initialized to 0
vector<string> svec(10); // ten elements, each an empty string
```
two restrictions
> some classes require that we always supply an explicit initializer. It is not possible to create vectors of such types by
supplying only a size.  
> The second restriction is that when we supply an element count without also supplying an initial value, we must use the direct form of initialization.
##### 1.5 List Initializer or Element Count?
```
vector<int> v1(10); // v1 has ten elements with value 0
vector<int> v2{10}; // v2 has one element with value 10
vector<int> v3(10, 1); // v3 has ten elements with value 1
vector<int> v4{10, 1}; // v4 has two elements with values 10 and 1
vector<string> v5{"hi"}; // list initialization: v5 has one element
vector<string> v6("hi"); // error: can't construct a vector from a string literal
vector<string> v7{10}; // v7 has ten default-initialized elements
vector<string> v8{10, "hi"}; // v8 has ten elements with value "hi"
```
#### 2. Adding Elements to a `vector`
`push_back`:
```
vector<int> v2; // empty vector
for (int i = 0; i != 100; ++i)
v2.push_back(i); // append sequential integers to v2
// at end of loop v2 has 100 elements, values 0 . . . 99
```
```
// read words from the standard input and store them as elements in a vector
string word;
vector<string> text; // empty vector
while (cin >> word) {
text.push_back(word); // append word to text
}
```
#### 3. Other `vector` Operations 见p148

#### 4. Computing a `vector` Index
```
// count the number of grades by clusters of ten: 0--9, 10--19, . .. 90--99, 100
vector<unsigned> scores(11, 0); // 11 buckets, all initially 0
unsigned grade;
while (cin >> grade) { // read the grades
if (grade <= 100) // handle only valid grades
++scores[grade/10]; // increment the counter for the current cluster
}
```

### 5. Arrays
#### 1. Arrays have fixed size. If you don’t know exactly how many elements you need, use a `vector`.
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

#### 2. Explicitly Initializing Array Elements
```
int ia1[sz] = {0,1,2}; // array of three ints with values 0, 1, 2
int a2[] = {0, 1, 2}; // an array of dimension 3
int a3[5] = {0, 1, 2}; // equivalent to a3[] = {0, 1, 2, 0, 0}
string a4[3] = {"hi", "bye"}; // same as a4[] = {"hi", "bye", ""}
int a5[2] = {0,1,2}; // error: too many initializers
```

#### 3. Character Arrays Are Special
```
char a1[] = {'C', '+', '+'}; // list initialization, no null
char a2[] = {'C', '+', '+', '\0'}; // list initialization, explicit null
char a3[] = "C++"; // null terminator added automatically
const char a4[6] = "Daniel"; // error: no space for the null!
```
The dimension of a1 is 3; the dimensions of a2 and a3 are both 4. The definition of a4 is in error. Although the literal contains only six explicit characters, the array size must be at least seven—six to hold the literal and one for the null.

#### 4. **No Copy or Assignment**
We cannot initialize an array as a copy of another array, nor is it legal to assign one array to another是不准数组和数组之间，而不是数组元素之间的赋值:
```
int a[] = {0, 1, 2}; // array of three ints
int a2[] = a; // error: cannot initialize one array with another
a2 = a; // error: cannot assign one array to another
```

#### 5. Understanding Complicated Array Declarations
```
int *ptrs[10]; // ptrs is an array of ten pointers to int
int &refs[10] = /* ? */; // error: no arrays of references
int (*Parray)[10] = &arr; // Parray points to an array of ten ints
int (&arrRef)[10] = arr; // arrRef refers to an array of ten ints
```
解释待补充

Accessing the Elements of an Array
When we use a variable to subscript an array, we normally should define that variable to have type `size_t`. `size_t` is a machine-specific unsigned type that is guaranteed to be large enough to hold the size of any object in memory. The size_t type is defined in the `cstddef` header, which is the C++ version of the stddef.h header from the C library.

#### 6. **Pointers and Arrays**
```
string nums[] = {"one", "two", "three"}; // array of strings
string *p = &nums[0]; // p points to the first element in nums
// 输出 *p：one
string *p2 = nums; // equivalent to p2 = &nums[0]
// 输出 *p2：one
```
也就是说，使用数组类型的对象==使用一个该数组首元素的指针。即拍p2其实是首元素的地址。
we can use the increment operator to move from one element in an array to the next:
```
int arr[] = {0,1,2,3,4,5,6,7,8,9};
int *p = arr; // p points to the first element in arr
++p; // p points to arr[1]
```

#### 7. The Library `begin` and `end` Functions
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

#### 8. Pointer Arithmetic
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

#### 9. Interaction between Dereference and Pointer Arithmetic
```
int ia[] = {0,2,4,6,8}; // array with 5 elements of type int
int last = *(ia + 4); // ok: initializes last to 8, the value of ia[4]
```
The expression `*(ia + 4)` calculates the address four elements past `ia` and dereferences the resulting pointer. This expression is equivalent to writing `ia[4]`.`*(ia + 4)`的括号是必须的。

10. Subscripts and Pointers
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

#### 1. Initializing the Elements of a Multidimensional Array
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
#### 2. Subscripting a Multidimensional Array
```
// assigns the first element of arr to the last element in the last row of ia
ia[2][3] = arr[0][0][0];
```
```
constexpr size_t rowCnt = 3, colCnt = 4;

#### 3. Pointers and Multidimensional Arrays

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
#### 4.int `*p[n]`和`int (*p)[n]`
```
int a = 1, b = 2, c = 3, d = 4;
int *p[4] = {&a, &b, &c, &d};
cout << "&a: " << &a << endl;
cout << "p: " << p << endl;
cout << "&p: " << &p << endl;
cout << "*p[0]: " << *p[0] << endl;
```
输出结果：
```
&a: 00CFF7CC
p: 00CFF790
&p: 00CFF790
*p[0]: 1
```
可见，p是一个有四个元素的数组，每个元素是一个指针，每个元素里存放的是a~e的地址。当执行  
> `cout << p: " << p << endl;`
>> `p: 00CFF790`
也就是说，和其他所有有数组一样，p代表p数组的首个元素的指针，即&p[0]。

```
int e[4] = {12, 23, 34, 45};
int (*q)[4];
q = &e;
cout << "q: " << q << endl;
cout << "&e: " << &e << endl;
cout << "e: " << e << endl;
for (int i = 0; i < 4; ++i)
{
	cout << "&e[" << i << "]: " << &e[i] << endl;
	cout << "q[" << i << "]: " << q[i] << endl;
}	
 ```
 输出为：
 ```
q: 00CFF778
&e: 00CFF778
e: 00CFF778
&e[0]: 00CFF778
q[0]: 00CFF778
&e[1]: 00CFF77C
q[1]: 00CFF788
&e[2]: 00CFF780
q[2]: 00CFF798
&e[3]: 00CFF784
q[3]: 00CFF7A8
```
`p`即为一个指针，是一个指向含有四个整数的数组，即`q = &e;`==> `q: 00CFF778` `&e: 00CFF778`。q不是一个数组，只是一个指针，故  
> `cout << "q: " << q << endl;`
>> `q: 00CFF778` == `&e: 00CFF778`  
即`q == &e`，输出q就是输出q里存放的e[0]的地址。

```
int ia[3][4] = { 11,10,9,8,7,6,5,4,3,2,1,0 };
	int (*p)[4] = ia;
	cout << "*p: " << *p << endl;
	// 输出 *p: 006FF750
	for (auto p = ia; p != ia + 3; ++p)
	{
		cout << "p: " << p << endl;
        // 输出 p: 006FF750
		for (auto q = *p; q != *p + 4; ++q)
		{
			cout << "q type: "<< typeid(q).name() << endl;
            // 输出 q type: int *
			cout << "*p type: "<< typeid(*p).name() << endl;
            // 输出 *p type: int [4]
			cout << "q: " << q << endl;
            // 输出 q: 006FF750
			cout << "*p: " << *p << endl;
            // 输出 *p: 006FF750
			cout << "*q: " << *q << endl;
            // 输出 *q: 11
		}
         }
```
```
ia是一个由3个一维数组，每一维数组有4个元素的数组组成的数组，即：
[0]: 11 10 9 8
[1]: 7  6  5 4
[2]: 3  2  1 0
int (*p)[4] = ia;
即p是一个指针，此赋值则是使p指向ia的[0]这一行，即把这一行看作是
一个三个元素数组的第一个元素。这个元素是一个包含4个元素的数组，
即p是一个指向含有4个整数的数组。见北大公开课slides。
```
