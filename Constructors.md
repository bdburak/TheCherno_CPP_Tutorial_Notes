Creating constructors is quite simple. 

```cpp
class className {
	int x, y;
public:
	className(int x, int y) {
		this->x = x;
		this->y = y;
	}
	
};
```

> You have to remember to name your constructor the same name as your class name.

> Constructors don't have return types.

You can also have multiple constructors for your class:

```cpp
class className {
	int x, y;
public:
	//This constructor with both x and y
	className(int x, int y) {
		this->x = x;
		this->y = y;
	}
	//This is a constructor with only x
	className(int x){
		this->x = x;
	}
	void print(){
		cout << x << ", " << y << endl;
	}
	
};
```

Creating instances with constructors:

```cpp
int main(){
	//an instance without initialisation
	className a;
	a.print();
	//an instance with x and y values
	className b = className(10, 20);
	b.print();
	//an instance with only the x value
	className c = className(10);
	c.print();
}
```

The output:

```
0, 0
10, 20
10, 0
```

## How To Hide Your Constructor

You may need to hide your constructor to make sure your class is used without a constructor. For example in a logging class, you wouldn't need to create instances, but use the logging functions directly.

```cpp
class Log {
private:
	Log(){}
public:
	static void log(const char* message){
		cout << message << endl;
	}
};

int main() {
	Log::log("hello world");
	
	//this would result in an error
	Log l;
	l.log("this wouldn't work");
}
```

>We are overriding the default empty constructor to be private. Without a constructor, you can't create an instance of a class.

As you can see, we can make sure our class can be non-instanced and used for static functions.

### Deleting The Default Constructor

You can also just delete the default constructor and it would basically have the same result.

```cpp
class Log {
public:
	//deleting the default Log constructor
	Log() = delete;
	
	static void log(const char* message){
		cout << message << endl;
	}
};

int main() {
	Log::log("hello world");
	
	//this would result in an error
	Log l;
	l.log("this wouldn't work");
}
```

The results are the same.