Classes are a way of grouping data and functionality.

## Intro To Creating Classes

```cpp
class Player{
	int x, y;
	int speed;
};
```
>Note that we need a `;` at the end of our class declaration.

We then can create a `Player` instance in our code:

```cpp
int main(){
	Player p1;
}
```

Variables that are made from `class types` are called `objects`, and a new object variable is called an `instance`.

We can assign the variables inside our new `p1` object like this:

```cpp
int main(){
	Player p1;
	p1.x = 10;
	p1.y = 21;
	p1.speed = 100; 
}
```

Of course this code will throw an error. Because all the variables inside `p1` are technically `private` variables. **By default all variables inside classed are `private`**. We can change this by changing the variables inside `Player` class to be public.

```cpp 
class Player{
	public:
		int x, y;
		int speed;
};
```

Now the code from before will work fine.

### Creating Functions Inside Classes

For the same `Player` class:

```cpp
class Player{
	public:
		int x, y;
		int speed;

		void MovePlayerTo(int xTarget, int yTarget){
			x = xTarget;
			y = yTarget;
			cout << "player moved to " << x <<  ", " << y << endl;
		}
};
```

Using it 

```cpp
int main(){
	Player p1;
	p1.x = 10;
	p1.y = 21;
	p1.speed = 100; 
	p1.MovePlayerTo(20, 30);
}
```

Output:
```shell
player moved to 20, 30
```

## Classes Vs Structs

In most ways they are basically the same thing. 
- Everything inside a ***class*** is, by default, **private**.
- Everything inside a ***struct*** is, by default. **public**.

Struct:
```cpp
struct Player{
	int x,y,speed;
	void aFunction(){
		return;
	}
};

int main(){
	Player p1;
	p1.x = 10;
	p1.aFunction();
}
```

Class:
```cpp
class Player{
	int x,y,speed;
	void aFunction(){
		return;
	}
};

int main(){
	Player p1;
	p1.x = 10;
	p1.aFunction();
}
```
>This would give an error because everything inside classes are private. You would have to change this to:

```cpp
class Player{
	public:
		int x,y,speed;
		void aFunction(){
			return;
		}
};
```

This is basically the main difference. The only reason C++ has structs is because, it wants to maintain backwards compatibility with C.

### Usage Differences (When You Should Use Either One)

#### When To Use Structs
Cherno only uses structs when he wants to bundle a set of data(variables) into a structure.

An example, 
```cpp
struct Vec2{
	float x,y;
	
	void Add(const Vec2& other){
		x += other.x;
		y += other.y;
	}
};
```

It's mostly used for simple things.

Generally we don't use inheritance with structs.

#### When To Use Classes
When you want more complexity and when you want to use things like inheritance and polymorphism...

## How To Write A C++ Class

We will be using a logging class as an example here. We will start with somewhat terrible code that is simple, to a better code in the end. 

### Simple Log Class

```cpp
class Log {
public:
	int LogLevelError = 0;
	int LogLevelWarn = 1;
	int LogLevelInfo = 2;
private:
	//m_ is used as a convention to specify a variable as a private class member
	int m_LogLevel = LogLevelInfo;
public:
	void setLogLevel(int level) {
		m_LogLevel = level;
	}
	void Error(const char* message) {
		if(m_LogLevel >=LogLevelError)
			cout << "[ERROR]: " << message << endl;
	}
	void Warn(const char* message) {
		if (m_LogLevel >= LogLevelWarn)
			cout << "[WARNING]: " << message << endl;
	}
	void Info(const char* message) {
		if (m_LogLevel >= LogLevelInfo)
			cout << "[INFO]: " << message << endl;
	}
};

int main() {
	Log log;
	log.setLogLevel(log.LogLevelWarn);
	log.Error("error test");
	log.Warn("warning test");
	log.Info("info test");
}
```

This code would output:

```shell
[ERROR]: error test
[WARNING]: warning test
```
 you can see that it didn't print the info text. It didn't print it because the `m_LogLevel` was set at `LogLevelWarn`.


## Creating Pointer Instances

You can also create instances that are pointers. We do this with `*` after class type and `new` keyword before constructor.

```cpp
class className {
public:
	int x;
	className(){}
	className(int x){
		this-> x = x;
	}
};

int main(){
	// normal instance
	className a;
	// also normal instance
	className b = className(15);

	//pointer class intance
	className* c;
	//also pointer class instance
	className* d = new className;
	//also pointer class instance with x initiated
	className* e = new className(10);
}
```