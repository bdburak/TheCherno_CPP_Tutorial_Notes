We will explore dynamic arrays, specifically the `vector` class. Now that we are finally beginning to write some code in C++, it's very important that we get accustomed to C++'s standard library or in this case something called a [standard template library](https://www.geeksforgeeks.org/the-c-standard-template-library-stl/). The standard template library is essentially a library filled with container types, things that contain certain data. It's called the standard template library because you can [template](Templates.md) it into anything. The whole library is templated. Meaning; the underlying data type of the container, in other words the data type that the container contains, is up to you to decide.

You don't need to know how to use templates in order to use the standard template library. You only need to know that you are able to provide the underlying data type that this data structure actually handles.

C++ provides us with a class called, `vector`. And that `vector` is in the `std` [namespace](Namespaces.md). It doesn't really make sense that it was called vector. It should have been called something like `array_list`. You can see why it is called vector [here](https://stackoverflow.com/questions/581426/why-is-a-c-vector-called-a-vector).

What `vector` is, it's a dynamic array. Unlike a normal array, it can dynamically resize. When you create this `vector`, it doesn't have a set size. Though you can also give it a set size as well. You basically create this dynamic array and put certain things inside it. Every time you put an element into it, the size grows. It is useful for applications where we don't know how many elements we will need in an array and we can push however many we need into it. If you want to learn how it works, we will write most of these data structures further along the tutorial. You can see it [here](obsidian://open?vault=c%2B%2B&file=VECTOR-DYNAMIC%20ARRAY). 

## Creating And Using Vectors
We will create a `Vertex` struct and an operator overload to be able to print it. This will serve as our baseline:

```cpp
#include <iostream>

struct Vertex {
	float x, y, z;
};

std::ostream& operator<<(std::ostream& stream, const Vertex& vertex) {
	stream << vertex.x << ", " << vertex.y << ", " << vertex.z;
	return stream;
}

int main() {

}
```

If we wanted to create a "static" array, one which never grows. We can do it like this:

```cpp
...
int main() {
	Vertex vertexesStack[10];

	//or a heap allocated version:
	Vertex* vertexesHeap = new Vertex[10];
}
```

This array would be a fixed size. This might not be good enough for our case. An example would be, if we wanted a user to enter things. Stopping them at a certain number of elements entered might not be plausible. In that case we would need dynamic arrays:

```cpp
#include <iostream>
//we need to include this:
#include <vector>

struct Vertex {
	float x, y, z;
};

std::ostream& operator<<(std::ostream& stream, const Vertex& vertex) {
	stream << vertex.x << ", " << vertex.y << ", " << vertex.z;
	return stream;
}

int main() {
	std::vector<Vertex> vertices;
}
```

The data type inside `<>` will determine the data type of the array. Unlike other languages like java, we can actually also pass in primitive types like `int`.

You can also notice that we are actually storing `Vortex`'s and not the pointers to them like so:
`std::vector<Vector*> vertices;`
This will depend on your application. You might do either one of them according to your needs.
It is technically more optimal to store the objects and not the pointers in a dynamic array like this. This is because if we store the objects, they will be inline inside our memory and easier to access. Dynamic arrays are arrays in the sense that their members are stored contiguously, or inline. It's all in a row. This is optimal in the sense that it's so much easier to iterate over them or set all of them... 

If it's at all possible try to allocate dynamic array variables as themselves instead of pointers whenever possible. The only issue is, when you have a vector that contains strings and the vector has to grow. The vector would need to copy all the data to another memory address and string copying is a really performance heavy task. With pointers, you would only need to copy the pointers to the memory address and the strings would stay in the same memory address that the pointer points to.

>Tip: Try to store your data types as themselves in vectors whenever possible. If storing themselves becomes a non-optimal option, then store them as pointers. As with stack and heap allocations the order should be: stack->heap and  object -> pointer

### Adding Elements
Now that we have a vector, we would want to add things to it. We will use `push_back()` to add things:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	//we don't have a constructor for the Vortex so we used an initializer list
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });
}
```

### Iterating Through Elements
The most basic way would be a `for` loop:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });
	
	for (int i = 0; i < vertices.size(); i++)
		std::cout << vertices[i] << std::endl;
}
```

The output would be:

```
1, 2, 3
4, 5, 6
```

We can also use range based for loops:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });
	
	for (Vertex v : vertices)
		std::cout << v << std::endl;
}
```

In this case because we wrote `Vertex` like this, it's going to be copying each vertex into the scope of the for loop. We don't want to do that. We want to avoid copying wherever possible. So we will change it like this:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });
	
	for (Vertex& v : vertices)
		std::cout << v << std::endl;
}
```

Or even mark it as const, since we don't change the value:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });
	
	for (const Vertex& v : vertices)
		std::cout << v << std::endl;
}
```

We can avoid copying. The output would be the same as the normal for loop, like this:

```
1, 2, 3
4, 5, 6
```

### Clearing The Vector (Array)
If we wanted to completely delete the contents of the vector, we can use the `.clear()` function:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });

	vertices.clear();

	for (const Vertex& v : vertices)
		std::cout << v << std::endl;
}
```

This would output:

```
```


### Removing Vector Elements
We can also remove vector elements using the `.erase()` function:

```cpp
...
int main(){
	std::vector<Vertex> vertices;
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });

	//to remove the 2nd element
	vertices.erase(verices.begin() + 1);

	for (const Vertex& v : vertices)
		std::cout << v << std::endl;
}
```

The `.erase()` function doesn't take indexes. Instead, it takes in, iterators. In the code above, we gave it the beginning or the first element and added 1 to it to reach the 2nd element.

The output would be:

```
1, 2, 3
```

### Passing Vectors Into Functions As Parameters
When passing vectors into functions, you really want to pass them in as ***references***. Because copying is a really performance taxing thing, you wouldn't want to copy the whole array, which can be hundreds or thousands of elements wide.

```cpp
void function(std::vector<Vertex>& vertices){}
```

And if you won't be modifying it, passing it as a const reference:

```cpp
void function(const std::vector<Vertex>& vertices){}
```

