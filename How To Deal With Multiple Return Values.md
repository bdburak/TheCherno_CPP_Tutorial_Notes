In this tutorial we will go over tuples, pairs and how to deal with multiple return types. Traditionally a C++ function can only return one type and only one variable of that type. 

If it's just the same type but multiple variables, we can just return it as a vector or an array.

There are a couple ways to return a different variable types in one function in C++. Here are some:

## Creating A Struct That Will Envelop Your Data Types
Let's say we want to return an int and a float. To do that, we can create a struct that will have an int and a float variable:

```cpp
#include <iostream>

struct A {
	int number;
	float fl;

	A(int number, float fl)
		: number(number), fl(fl) {}
};

A function(int aNumber, float fl) {
	return A(aNumber + 5, fl + 0.2f);
}

int main() {
	A test = function(5, 3.2f);
	std::cout << test.number << ", " << test.fl << std::endl;
}
```

The output:

```
10, 3.4
```

## Passing Return Variables Inside Parameters
### Passing The Types As References In The Parameters
If we pass references to let's say an int and a float, we can manipulate those and not even return anything:

```cpp
#include <iostream>

void function(int& outNumber, float& outFloat) {
	outNumber += 3;
	outFloat *= 2.3f;
}

int main() {
	int number = 2;
	float fl = 1.2f;

	function(number, fl);
	std::cout << number << ", " << fl << std::endl;
}
```

The output:

```
5, 2.76
```

Technically speaking, this is one of the most optimal ways of doing this


### Passing The Types As Pointers In The Parameters
This is basically the same thing as  passing it as a reference. The only difference being, you don't need a valid variable. It can be a null pointer and it would still work;

```cpp
#include <iostream>

void function(int* outNumber, float* outFloat) {
	//cheking if they are valid
	if (outNumber && outFloat) {
		(*outNumber) += 3;
		(*outFloat) *= 2.3f;
	}
}

int main() {
	int number = 2;
	float fl = 1.2f;

	function(&number, &fl);
	std::cout << number << ", " << fl << std::endl;
}
```

The output:

```
5, 2.76
```

As you can see, it works the same.

>Naming output parameters starting with `out` can make your code more readable. Instead of calling it `number` you can call it `outNumber` to specify it as an output variable.

## A Universal Way Of Returning Multi-Type/Single-Type Variables
We can achieve this by either returning a `tuple` or a `pair`.


### Tuples
A `tuple` being, a type of class that may have n amount of variables, which can consist of multiple types.

```cpp
#include <iostream>

#include <tuple>

std::tuple<int, float,float> function(int aNumber, float aFloat, float anotherFloat) {
	return std::tuple<int, float, float>(aNumber + 2, aFloat * 3.0f, anotherFloat - 2.0f);
}

int main() {
	int number = 2;
	float fl1 = 1.2f;
	float fl2 = 4.0f;

	std::tuple<int, float, float> retTuple = function(number, fl1, fl2);

	//accessing the indexes of a tuple: std::get<index>(tuplename)
	std::cout << std::get<0>(retTuple) << ", " << std::get<1>(retTuple) << ", " << std::get<2>(retTuple) << std::endl;
}
```

The output:

```
4, 3.6, 2
```

### Pairs
The major difference between a pair and a tuple is that a pair only holds two things. You also can access indexes of a pair by using the `std::get<index>(pairName)` function but there is a more elegant way. You can use `pairName.first` and `pairName.second` to access the members of a pair. 

```cpp
#include <iostream>

#include <tuple>

std::pair<int, float> function(int aNumber, float aFloat) {
	return std::pair<int, float>(aNumber + 2, aFloat * 3.0f);
}

int main() {
	int number = 2;
	float fl = 1.2f;

	std::pair<int, float> retPair = function(number, fl);

	//wanted to show that you can use both
	std::cout << retPair.first << ", " << std::get<1>(retPair) << std::endl;
}
```

The output:

```
4, 3.6
```


### The Problem With Using Pairs And Tuples
When you return a tuple or a pair, you don't know how the pair or the tuple looks like. You access the members by index or the order for pairs. Using the struct method gives our variables names.