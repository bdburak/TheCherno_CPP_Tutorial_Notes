# How C++ Works

  

## Preprocessor Statements

  

Basically anything that starts with the `#` character in C++ will be a preprocessor statement. That would include the `#include <iostream>` as well.

The first thing the compiler does when it receives a source file, is it pre processes all of your preprocessor statement that's why they called preprocessor statements because they happen just before the actual compilation. In this case is something called `include`. What include will do is, find a file. So in this case we're looking for a file called iostream take all of the contents of that file and just paste it into this current file.

  

## Entry Point

  

The `main()` function is our entry point to our application. The main function is a special case where you don't have to return anything. If you don't return anything in the main function, it will assume it's returning 0. **This only applies for the main function.**

  

## The [Compilation](Compiler.md) Process

  

The process will be in this order:

  

1. Each .cpp file will be compiled into an .obj file individually. **Header files don't get compiled, they get copied and pasted into the .cpp files where they are included**

2. The linker will stich the .obj files that were created during the compilation process and create the .exe file. This will differ with each OS target.

  

## Creating Multiple Files To Separate Functions

  

You can just create new .cpp files on the same source folder and it will be included in the main file. But you have to declare all the functions in the main file before the main function. An example:

  

> Log.cpp

  

```c++

#include <iostream>

  

void Log(const char* message) {

    std::cout << message << std::endl;

}

```

  

> Main.cpp

  

```c++

void Log(const char* message);

  

int main() {

    Log("hello world");

}

```

  

> If you don't declare the Log function above before the main() function, the program won't be able to run.

  

> Should the linker not find your external file, it will throw a [linker](Linker.md) error.