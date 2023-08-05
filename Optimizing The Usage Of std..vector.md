We have been over the `vector` class in the [previous](obsidian://open?vault=c%2B%2B&file=Dynamic%20Arrays(std..vector)) tutorial. We will explore how we can use the `vector` class in a more optimal way. This will be the first optimization tutorial in this series. Many people choose C++ knowing that they can do low-level optimizations. 

The most important rule of optimization is to know your environment. By your environment we mean, how things work and what you need to do exactly. We will go in depth later on.

Focusing into the vector class, you really should know how the `vector` class works and how you could change it to work better.

The most simple explanation for the `vector` class is; you create a vector and you start pushing elements into it. If the size of the vector isn't enough, the vector has to allocate new memory, copy the elements there and delete the old ones. The new memory location should be at least large enough to hold the previous element and the new element you added. The pushing of elements is a slowing point for vectors. Because it has to copy all the previous elements as well. This is something we want to avoid. This will largely be our optimization strategy for today. ***Copying, how can we avoid copying***. Specifically, object based vectors and not pointer based vectors.

We have already said that knowing your environment is key to be able to optimize your code. Here we have to know how [copying](obsidian://open?vault=c%2B%2B&file=Copying%20And%20Copy%20Constructors) works and how we can avoid it.

## Actually Optimizing

From the last tutorial, we had a `Vertex` class. We will be using the same code:

```cpp
#include <iostream>
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
	vertices.push_back({1, 2, 3 });
	vertices.push_back({ 4, 5, 6 });
}
```

Now let's see how many, if any, copies are happening in this code. A good way of doing this, is to create a copy constructor where you either put a break point or a console print:

```cpp
struct Vertex {
	float x, y, z;

	//we need the constructor
	Vertex(float x, float y, float z)
		:x(x), y(y), z(z)
	{}
	
	Vertex(const Vertex& vertex)
		:x(vertex.x), y(vertex.y), z(vertex.z)
	{
		std::cout << "object copied" << std::endl;
	}
};
```

The output:

```
object copied
object copied
object copied
```

As we can see, there were three instances of copying happening. When we add another object to the vector:

```cpp
...
int main() {
	std::vector<Vertex> vertices;
	//changed this to make it more readable
	vertices.push_back(Vertex(1, 2, 3));
	vertices.push_back(Vertex(4, 5, 6));
	vertices.push_back(Vertex(7, 8, 9));
}
```

The output:

```
object copied
object copied
object copied
object copied
object copied
object copied
```

As we can see, objects were copied a total of 6 times. We can also observe which ones are being copied if we change our code:

```cpp
struct Vertex {
	...
	Vertex(const Vertex& vertex)
		:x(vertex.x), y(vertex.y), z(vertex.z)
	{
		std::cout << "object copied x: " << vertex.x << std::endl;
	}
};
```

The output:

```
object copied x: 1
object copied x: 4
object copied x: 1
object copied x: 7
object copied x: 1
object copied x: 4
```

At the first moment of pushing the first element into the vector, in the background, our object gets created on the main function's stack frame. This has to be copied over to the vector.

So if we go line by line and analyze the the output:

```cpp
vertices.push_back(Vertex(1, 2, 3));
```

The output of the first push is:

```
object copied x: 1
```

This is the first element being created and then copied over to the vector.

Then, when we run the next line of code:

```cpp
vertices.push_back(Vertex(4, 5, 6));
```

The output becomes:

```
...
object copied x: 4
object copied x: 1
```

Firstly, the second `Vertex` was created and copied over to the vector to the new memory address. Which has already allocated a new address and is ready to copy all the elements over. After the creation and copying of the new element, the vector will copy the first element from the previous address to the new address and delete it from the previous one. This resulted in the output above.

When we proceed to the next push:

```cpp
vertices.push_back(Vertex(7, 8, 9));
```

The output becomes:

```
...
object copied x: 7
object copied x: 1
object copied x: 4
```

Where, the last element is created at the main function's stack frame and copied over to the new memory location of vector. The vector already allocated a new space and as mentioned copied the last element to its spot. Then, it copies the previous elements into the newly allocated area and deletes the previous ones.

As you can see, we have created and pushed 3 objects into the new `vertices` vector. But copied a total of 6 times. This would go on with a formula of:

( n * ( n - 1 ) ) / 2

Which can be simplified as:

( ( n ^ 2 ) / 2 ) - ( n / 2 )

The term with the highest degree in this expression is n^2/2. We can take that and simplify it to get our big o notation:

O(n^2)

This is highly inefficient. We should try to optimize this.


### First Optimization
This is the biggest optimization. This is due to the vector class always allocating just enough memory for the elements it will have and not allocating extra. Even though allocating extra space might use extra memory, it can be worth it if we have to add or delete elements from the vector. 

The easiest way of optimizing would just be to make our vector `reserve` 3 spots:

```cpp
int main() {
	std::vector<Vertex> vertices;
	vertices.reserve(3);
	vertices.push_back(Vertex(1, 2, 3));
	vertices.push_back(Vertex(4, 5, 6));
	vertices.push_back(Vertex(7, 8, 9));
}
```

The output of this would become:

```
object copied x: 1
object copied x: 4
object copied x: 7
```

Where each object only had to be copied from the main function's stack frame.



### Second Optimization
If we can just create the `Vertex` objects inside the vector and not copy them from the main function's stack frame. We can get more performance. 

The output for:

```cpp
int main() {
	std::vector<Vertex> vertices;
	vertices.push_back(Vertex(1, 2, 3));
	vertices.push_back(Vertex(4, 5, 6));
	vertices.push_back(Vertex(7, 8, 9));
}
```

Would become:

```
object copied x: 1
object copied x: 1
object copied x: 4
```

Theoretically it would make its big o notation the same. But it would still be an improvement because for the normal way the pushes and the amount of copying goes like this:

```
1->1
2->3
3->6
4->10
5->15
```

It increased by n each time which has a formula of:

( n * ( n - 1 ) ) / 2

The improved way would look something like this:

```
1->0
2->1
3->3
4->7
5->11
```

You can already see the difference. It increases by n-1. The formula for something like this would be:

( ( n - 1) * ( ( n - 1) - 1 ) ) / 2

Simplified:

( ( n - 1) * ( ( n  - 2 ) ) / 2

Simplified further:

( (n ^ 2) / 2 ) - ( (3 * n ) / 2) + 1

Even though the big o notation hasn't changed, we can observe in our example that it had a major change.

Now, to implement this change we will use `emplace_back()` instead of `push_back()`:

```cpp
int main() {
	std::vector<Vertex> vertices;
	vertices.emplace_back(Vertex(1, 2, 3));
	vertices.emplace_back(Vertex(4, 5, 6));
	vertices.emplace_back(Vertex(7, 8, 9));
}
```

In this case, instead of passing in an object that was created in the main function's stack frame, we are directly creating the object in the vector.

The output would be:

```
object copied x: 1
object copied x: 4
object copied x: 1
object copied x: 7
object copied x: 1
object copied x: 4
```


We can see that the output hasn't changed. This is because we have to combine the first optimization with the second one:

```cpp
int main() {
	std::vector<Vertex> vertices;
	vertices.reserve(3);
	vertices.emplace_back(1, 2, 3);
	vertices.emplace_back(4, 5, 6);
	vertices.emplace_back(7, 8, 9);
}
```

The output would become:

```
```

Technically speaking, this would reduce our big o notation of O(n^2) to O(1). Which is constant. This is a huge jump from where we started.

> Keep in mind that you have to change `vertices.emplace_back(Vertex(1, 2, 3));` to `vertices.emplace_bac(1, 2, 3);` in order to avoid copying.