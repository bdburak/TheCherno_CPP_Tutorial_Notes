We will go over how we can use external libraries in C++. When it comes to using external libraries in languages such as Java, C# or Python, adding libraries is a trivial task. You might use a package manager or add them manually but the process is usually pretty straight forward. When it comes to C++, everyone seems to have problems. But basically, there are a few strategies we can follow when it comes to adding external libraries.

## Cherno's Way
He hates package managers. He says that, when you check his repositories, you should have everything you need to compile and run the application/project/whatever. For that reason, he tends to keep versions of his dependencies, of the libraries he is using, in the actual solution/project folder.

### Should You Compile Them Yourself Or Link Precompiled Libraries
For most serious projects, he absolutely recommends building the source code. In visual studio, you can add another project, which contains the source code of your dependency/library. And compile that into either a static or a [dynamic library](obsidian://open?vault=c%2B%2B&file=Using%20Dynamic%20Libraries). 

However, if you don't have access to the source code or if this is a quick project and you don't want to set it up properly, you can link against the libraries. Which would be quicker and easier.


## Using Libraries
Today specifically, we will be linking against binaries. We will not be grabbing the source code of an actual dependency/library and compiling it ourselves. This will be taught at a later point.

In this tutorial we will be dealing with a library called, [GLFW](https://www.glfw.org/). This is a library for OpenGL. 

Sometimes binaries might not be available for our project. You may have to build it yourself. This is especially true for Mac and Linux.

To download the GLFW library's precompiled binary, we cab head to the [official website](https://www.glfw.org/) and click on the "Download" button. This will take us to the download page where we need to find the "Windows pre-compiled binaries" and select either 32 or 64 bit binaries. Now this brings us to the question; 
"Should I download the 32 or 64 bit version?"
***This has nothing to do with your operating system but it has a lot to do with your application and what you target.*** 

- If we target the x86 architecture, we need to select the 32 bit option because we are targeting a 32 bit operating system.
- If we target the x64 architecture, we need to select the 64 bit option because we are targeting a 64 bit operating system.

Since we have been targeting x86 for our previous project, we will do the same for this one and therefore you should select the 32 bit version.

Once you download the 32 bit version you should unzip them and open the resulting folder.

![resulting folder](Pasted%20image%2020230801073811.png)

This is the typical layout of a C++ library.

There are two parts to a library(usually):
- Includes
- Libraries
There will be an include and a library(lib) directory

The Include directory contains a bunch of header files. If you need a refresher on how linking works in C++, you can head to this [file](Linker.md). Basically the include directory has a bunch of header files that we need to use so that we can actually use functions that are in the prebuilt binaries. And the lib directory has those binaries.

There are usually two faces to this:
- Dynamic Libraries
- Static Libraries

The gist of them is:

Static linking means that the library actually gets put into your executable, i.e. inside your `.exe` file.

Dynamic libraries get linked in runtime and are not put into the executable file. You can choose to link a dynamic library on the fly in which case you could use the function, `LoadLibrary()` which you can use with the windows API. This will load your dynamic library. Or you could also pull the dynamic libraries at launch. We will talk in more detail later. 

When you dynamically link libraries, you would need to have `.dll` files alongside your `.exe` files. This is the reason why Cherno likes to link his libraries statically(put them into the `.exe`). This also provides a performance boost as well. Because, the linker can perform link-time optimizations.

### Setting Up Our Project Folder For The GLFW Library
We have the include and library files. We need to set both of them up. This will give us the function definitions and symbol definitions from our library.

We need to point our **compiler** to the **header** files. And we also need to point our **linker** to the **library** files. We need to link the files inside the library directory to our project in order to use the definitions. They contain the declarations. We will do that for both the **static** and **dynamic** libraries:

Inside our project folder we will create a folder called, "Dependencies":

![dependencies](Pasted%20image%2020230801090201.png)

Inside that folder, we will create another folder called, "GLFW":

![glfw](Pasted%20image%2020230801090318.png)

And we will unzip and copy the contents of the unzipped "GLFW" library into the "GLFW" folder that we created:

![picture](Pasted%20image%2020230801090709.png)

You only need to copy the include and library file that suits your toolchain, mine is visual studio 2022, here. I also copied the docs folder for future reference if I wanted to actually do something with this library. Also, the `readme.md` file contains helpful things about importing libraries the static and dynamic way. So, I would recommend reading it.

#### Seeing What's Inside The Folders
The include folder contains the header files that are inside a folder called, "GLFW":

![picture](Pasted%20image%2020230801091334.png)

The library folder:

![picture](Pasted%20image%2020230801091422.png)

It contains the `glfw3.dll` file for dynamic linking. It's called `.dll` because it stands for: "dynamic link library".

The `glfw3dll.lib` is the static library that we use with the `.dll` so that we don't have to ask the `.dll` for function pointers. The idea is that the `glfw3dll.lib` keeps all the function pointers and symbols inside the `glfw3.dll` so that we can link against them at compile time. I we didn't have the `glfw3dll.lib`, we would have to ask the `glfw3.dll`, for each function by name. But the `glfw3dll.lib` file already contains all those locations for us so that the linker can link to them immediately. Kind of like a DNS.

`glfw3.lib`, which is significantly bigger than the other ones, contains the static library. This is what we link if we don't want compile time linking. This way we don't need the `.dll` file with our `.exe` file.

### Setting Up The Library Inside Visual Studio
We will start by right clicking on our project and selecting the "Properties":
![picture](Pasted%20image%2020230801092406.png)

In C/C++, we will go to the "Additional Include Directories":

![picture](Pasted%20image%2020230801092607.png)

>Note that we have selected Win32 for the platform and All Configurations for the configurations.

Here we will specify our include directory. You can either copy the absolute path to it or you could also include the relative address like this:

![picture](Pasted%20image%2020230801093125.png)

>We have used the `${SolutionDir}` macro, which returns the solution path. You can explore all the macros by selecting the field. Then, clicking the downward arrow and selecting `<Edit...>`. There you will see a button called `Macros>>`. You can see all the macros there and learn how to use them.

Now that we have linked our include directory, we can reference our header files relative to the path we entered. Which would be, `GLFW/glfw3.h` and `GLFW/glfw3native.h` :

```cpp
#include <iostream>
//#include "GLFW/glfw3.h"
//Because this is a compiler specified library(i.e. we added it
// from the properties of our project) we can use <> as well
#include <GLFW/glfw3.h>


/*
If glfw was inside our solution he would use "". If the library was actually
inside our solution folder with our main.cpp
If it's a completely external dependency and we are not compiling 
it with visual studio, he would use angular brackets(<>) to indicate it's 
external
*/
int main() {
	int a = glfwInit();

}
```

If we try to run this we will encounter a linker error. The error will be an unresolved external dependency. This means we haven't actually linked the actual library. The linker can't find the actual definitions of functions and symbols. They are stored in the library file.

To make it work and add our actual library and not just the header file for it, we have to actually link against the library. 

To do that, we can head over to the project properties and go to the linker option and general:

![](Pasted%20image%2020230802024912.png)

Here we will specify our library directory which is `$(SolutionDir)Dependencies\GLFW\lib-vc2022`. 
![](Pasted%20image%2020230802025528.png)

Then, we will head over to the input tab in linker and add our name of the library there:

![](Pasted%20image%2020230802025155.png)

I will change it to `glfw3.lib;$(CoreLibraryDependencies);%(AdditionalDependencies)`.

![](Pasted%20image%2020230802025232.png)

Let's apply and hit okay. This time it should run without a hitch.