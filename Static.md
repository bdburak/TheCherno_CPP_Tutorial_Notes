In C++, static has 2 meanings:
- **Static used outside of a class:** Only going to be visible inside that translation unit.
- **Static used inside of a class/struct:** It will share memory with all the instances of that class/struct. Across all the instances of this class/struct, there will only be 1 instance of this variable.

## What Static Means Outside Of A Class

Defining a variable as static.

>Static.cpp
```cpp
//s_ is a convention to specify a static variable
static int s_variable = 5;
```

This static variable is going to be only linked internally to this translation unit. When it comes to build this project, the linker will not look outside of the scope of this translation unit.
What this means is when we try to access it in a different file, let's say main.cpp file, we won't be able to. This way you can declare variables to be only in one translation unit. 

>main.cpp
```cpp
int s_variable = 10;

int main(){
	cout << s_variable << endl;
}
```

The output of this program would be:

```shell
10
```

If we declared the `s_variable` as a non-static variable in `Static.cpp`:
```cpp
int s_variable = 5;
```

With the same `main.cpp` file we would get an error like this:

```
Severity	Code	Description	Project	File	Line	Suppression State
Error	LNK2005	"int s_variable" (?s_variable@@3HA) already defined in Main.obj	project2	C:\dev\project2\project2\Static.obj	1	
```
Which says, `s_variable` was defined in another file. 

> If you want to access a variable on another file you can use the [[extern]] keyword;

Even if you use `extern` to access a static variable/function in another file, you will get an error, because it will only be compiled for the translation unit it was inside of.


>The same things would apply for a function, class or a struct.


## What Static Means Inside Of Classes And Structs


### Static Variables Inside Classes/Structs
We will create static variables in a struct to show what happens:

```cpp
struct Entity {
	static int x, y;

	void Print() {
		std::cout << x << ", " << y << std::endl;
	}
};
```

You have to re-define the static variables outside the class/struct to access them

```cpp
int Entity::x;
int Entity::y;
```

```cpp
int main() {
	Entity e1;
	e1.x = 2;
	e1.y = 10;

	Entity e2;
	e2.x = 123;
	e2.y = 63;
	
	e1.Print();
	e2.Print();
}
```

The output:
```
123,63
123,63
```

Why?
Because once you created static variables in a struct/class you will have the exact same variable, the exact same memory address, for all instances of that class/struct.

Actually you don't even need to access them through an instance either. You can just do this:

```cpp
int main() {
	Entity e1;
	Entity::x = 2;
	Entity::x = 10;

	Entity e2;
	Entity::x = 123;
	Entity::x = 63;
	
	e1.Print();
	e2.Print();
}
```

Here you can see why the last assigned value was applied for both instances.

This is used when you want to have the same variable across all your class instances. Kind of like a global variable. But they are contained inside a class/struct entity.

### Static Functions Inside Classes/Structs

If we changed our code to have the same print method as a static method;

```cpp
struct Entity {
	static int x, y;

	static void Print() {
		std::cout << x << ", " << y << std::endl;
	}
};
```

>We wouldn't have to change anything inside it thanks to `x` and `y` being static.

We can change the `main` function to be:

```
int main() {
	Entity e1;
	Entity::x = 2;
	Entity::x = 10;

	Entity e2;
	Entity::x = 123;
	Entity::x = 63;
	
	Entity::Print();
	Entity::Print();
}
```

And in this particular instance, we don't even need a class instance in the first place at all:

```cpp
int main() {
	Entity::x = 2;
	Entity::x = 10;
	
	Entity::x = 123;
	Entity::x = 63;
	
	Entity::Print();
	Entity::Print();
}
```

But if we decide to keep our static function but change our variables to be non-static.

#### Using A Static Function With Non-Static Variables Inside Class

The code for that:

```cpp
struct Entity {
	int x, y;

	static void Print() {
		std::cout << x << ", " << y << std::endl;
	}
};

int main() {
	Entity e1;
	e1.x = 2;
	e1.y = 10;

	Entity e2;
	e2.x = 123;
	e2.y = 63;
	
	Entity::Print();
	Entity::Print();
}
```

If we were to compile this code, we would get an error, `illegal reference to non-static member 'Entity::x'`.

The reason for this is; a static method does not have  a class instance. 

> A static method inside a class/struct is essentially the same as a globally declared method that is contained in a class/struct. 

```cpp
struct Entity {
	int x, y;

	static void Print() {
		std::cout << x << ", " << y << std::endl;
	}
};
static void Print() {
	std::cout << x << ", " << y << std::endl;
}
```

These functions are basically the same, in the way they can't access `x` and `y`. 

We could change the outside function by passing an `Entity` instance as a parameter.

```cpp
static void Print(Entity e) {
		std::cout << e.x << ", " << e.y << std::endl;
}
```

This is essentially what we have to do inside our struct.

```cpp
struct Entity {
	int x, y;

	static void Print(Entity e) {
		std::cout << e.x << ", " << e.y << std::endl;
	}
};



int main() {
	Entity e1;
	e1.x = 2;
	e1.y = 10;

	Entity e2;
	e2.x = 123;
	e2.y = 63;
	
	Entity::Print(e1);
	Entity::Print(e2);

}
```

This code would output:

```shell
2, 10 
123, 63
```

## Why Using Static Is Important

When you declare a variable, function, class or struct globally, you can't declare anything with the same name in your entire project again. Because they will all be assessable throughout your project. That's why, unless you need to, you should declare these things as static in your code.