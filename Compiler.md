The compiler produces intermediary files called, object files. Before the compilation process starts, the compiler will preprocess, copy & paste, the preprocessor statements. 
The compiler will compile our code using an [abstract syntax tree](https://dev.to/balapriya/abstract-syntax-tree-ast-explained-in-plain-english-1h38). After our code goes through, it will either become constant data or instruction code, machine code. 

## Files
In C++, there is no such things as files. The C++ compiler doesn't care about your files.  It doesn't expect any predetermined folders or files to be present. Each .cpp file will be converted to a [translation unit](https://learn.microsoft.com/en-us/cpp/cpp/program-and-linkage-cpp?view=msvc-170) , then an object file. You are only responsible for telling the compiler what type of file it is and how the compiler should treat that(e.g. cpp, h files). 

### Preprocessor Statements

#### `#include <externalDependancy name>` or `#include "filename"`
The include preprocessor statement  will find the file you have input and copy & paste the contents into the cpp file.

#### `#define targetWord replacingWord`
The define preprocessor statement finds the first parameter and changes it to be the second parameter

```cpp
#define INTEGER int

INTEGER number = 5;
```
This will look like the following after it has been preprocessed:

```cpp
int number = 5
```

#### `#if`
The if preprocessor can let us include or exclude code based on a given condition.

```cpp
#if 2+4==6
int twoPlusFour = 6;
#endif

#if 2+4==7
int twoPlusFour = 7;
#endif
```

This code will turn into:

```cpp
int twoPlusFour = 6;
```

Because 2+4 == 7 is a false statement.