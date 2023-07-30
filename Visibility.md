Visibility really belongs to `oop` programming. It refers to, how visible certain members of a class are to the outside. Such as; variables and functions.

There are 3 basic visibility modifiers in C++:
- private
- protected
- public

## The 3 Visibility Modifiers

### Private
This is the default visibility modifier for all members of a class. This means all members that were defined within `private` are ***only*** visible and interactable inside our class.

It can be specifically defined like this:

```cpp
struct entity {
private:
	//x and y are only accesible within the struct
	int x,y;
public:
	//a and b can be accessed both inside and outside this struct
	int a,b;
};

//or 

class entity {
//the private: is inferred
	//x and y are only accesible within the class
	int x,y;
public:
	//a and b can be accessed both inside and outside this class
	int a,b;
};
```
Here, `x` and `y` are only accessible within our class.

> Even sub-classes can't access private members of a parent class.

### Public
This is the default visibility modifier for structs. This means all members that were defined within `public` are visible and interactable both outside and inside our class.

It can be specifically defined like this:

```cpp
struct entity {
//the public: is inferred
	int x,y;
};

//or 

class entity {
public:
	int x,y;
};
```

### Protected
It is between public and private in terms of visibility. It makes members only accessible within the same class and ***any sub-classes***. 

An example would be:

```cpp
class entity {
private:
	int x,y;
protected:
	int a,b;
};

class player : public entity {
	//we can't access these variables:
	cout << x << ", " << y << endl;

	//but we can access these variables
	cout << a << ", " << b << endl;
}

int main(){
	entity e;
	e.x = 10;
	e.y = 20;
	e.a = 51;
	a.b = 52;

	//but we can only access a and b in player:
	player p;
	p.a = 32;
	p.b = 76;
}
```


>We can use visibility to create getter and setter functions inside our classes/structs.