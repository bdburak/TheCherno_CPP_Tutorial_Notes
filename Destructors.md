Constructors' evil twin would be **destructors**. While constructors create objects, destructors destroy them.
You might need to use a destructor when you may need to get rid of an object or free-up memory space. 
Destructors work for both heap and stack stored objects.

Here is an example:

```cpp
class entity {
public:
	int x,y;
	
	//This is a constructor
	entity() {
		cout << "entity created"<< endl;
	}
	
	//This is also a constructor
	entity(int x, int y) {
		cout << "entity created"<< endl;
		this-> x = x;
		this-> y = y;
	}

	//This is a destructor
	~entity() {
		cout << "entity destroyed"<< endl;
	}

	void print() {
		cout << x << ", " << y << endl;
	}
};
```

> You have to create a destructor with `~` in the beginning.

We can use this `entity` class to create an object:

```cpp
int main() {
	entity e = entity(10,20);
	e.print();
	e.~entity();
}
```

If we were to run this we would get this result:

```
entity created
10, 20
entity destroyed
entity destroyed
```

Have you noticed that the `entity destroyed` text was printed twice? It Was printed twice because our program was finished and the destructor was called automatically. But we also called it ourself. So it was called twice and outputted the same message twice.

> Generally the destructor isn't manually called in your code. The example above is just showing that you can call it yourself.

> Objects are automatically destroyed after they go out of scope.

## Why Create Destructors?

If objects are automatically destroyed after they go out of scope, why do we need to write destructors?

The simple explanation would be to not cause a memory leak. While stack allocated variables get deleted in a class. heap allocated variables are not. If your class creates heap allocated variables, generally using the `new` keyword to create them, then you have to delete them manually. 

That's when custom destructors come in handy. If the object gets out of scope, the destructor function will be called. That's where you can delete your heap allocated variables.