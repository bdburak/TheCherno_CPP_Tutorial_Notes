We have already talked about static linking in the previous tutorial. In this tutorial, we will go over how we can dynamically link libraries and when to do so. We will be linking the GLFW library dynamically. 

The gist of dynamic linking is; linking libraries at runtime. Dynamic linking can effect the performance slightly. Dynamic linking requires libraries to be precompiled binaries in the form of `.dll` files. Applications can either have what are called "static dynamic linking" where you require that the library be present at the start of your program even thou you will not be using it at that time. And you can have the opposite where your program will start just fine and will work fine until you need that library and only then the program will need it.

In this tutorial we will do the latter one.

## Setting Up Visual Studio For Dynamic Linking GLFW
We will begin with a fresh project and we have to link the include directory again. This part doesn't change for both types of linking. GLFW supports both static and dynamic linking with the same header file.

First, we need to create the same Dependencies and GLFW folder and unzip and copy the files from GLFW

![](Pasted%20image%2020230802034509.png)

Now, we will add the include directory to the project properties. To do that we have to go to the project properties. Under C++, Additional Include Directories:

![](Pasted%20image%2020230802030932.png)

![](Pasted%20image%2020230802034200.png)

>Don't forget to choose win32!

Now we need to go to the linker and add the additional library directories there:

![](Pasted%20image%2020230802035131.png)

Then we need to go to the input for linker and add our `glfw3dll.lib` to the additional dependencies. This is all we have to do on this side:

![](Pasted%20image%2020230802033820.png)

>The `glfw3dll.lib` file contains the pointers to the functions inside the `glfw3.dll` and they have to be compiled at the same time when they have been created. 

Now we can use the library. We will create the same program as before and run it:

```cpp
#include <iostream>
#include <GLFW/glfw3.h>

int main() {
	int a = glfwInit();
	std::cout << a << std::endl;
}
```

Now when we try to run it we will get this:

![](Pasted%20image%2020230802040403.png)

It says that `glfw3.dll` was not found event though the file is in the directory.

This is where we have to show our program the `glfw3.dll` file. The simplest way of doing that is to copy the `glfw3.dll` file where our output path is. In this case in the debug folder.

![](Pasted%20image%2020230802040735.png)

When we try to run it this time, we will se that it works fine now.

![](Pasted%20image%2020230802040809.png)
