Enum is short for enumeration. It is just a collection of values with names. It's just a way of naming values. Enums can be used when you want integers to represent certain values, like when we created `LogLevel...` log levels in the [[Classes]] file.
At the end of the day, Enums are still just integers. But they help with readability.

## Creating And Using Enums

### Creating An Enum
You can create an Enum with or without a type. If you don't assign a type, it will be inferred as an integer.

```cpp
// this will constrain the values to be between 0 and 255, 8 bit positive number
enum enumWithType : unsigned char {
	a = 4,
	b = 10,
	...
}

enum enumWithoutType {
	a = 4,
	b = 10,
	...
}
```

>The type has to be an integer type variable type. Such as; unsigned char, char, unsigned int, int, unsigned long, long, unsigned long long or long long.


### Using An Enum
You can use an Enum as it is:

```cpp
enum Example {
	A = 10
}

int main() {
	if(A > 5)
		cout << "this enum value is bigger than 5" << endl;
}
```

And the output would be:

```shell
this enum value is bigger than 5
```

You can also use them like this:

```cpp
enum Example {
	A = 10
}

int main() {
	Example value = A;
	
	if(value > 5)
		cout << "value is bigger than 5" << endl;
}
```

And the output would be:

```shell
this enum value is bigger than 5
```

## Simple Example
### Simple Enum-less Example

```cpp
int a = 0;
int b = 1;
int c = 2;

int main(){
	int value = b;
	if (value == b){
		//do something...
	}
}
```

This would work. But the variables aren't grouped at all. And the code is basically illegible. 

We basically want to have a type where it can only be a certain set of values. That's where Enum's come in handy.

### Simple With Enum's Example

```cpp
enum Example{
	a, b, c
};

int main(){
	Example value = b;
	if (value == b){
		//do something...
	}
}
```

This is a better way of doing things. And here `a,b,c` still are equal to 0, 1, 2.

If you wanted to specify a value you can do this:

```cpp
enum Example{
	a = 5,
	b = 20,
	c = -2
};

int main(){
	Example value = c;
	if(value < 0)
		cout << "value is less than zero" << endl;
	cout << value << endl;
}
```
This would output:

```shell
value is less than zero
-2
```


## Improving Log Class From [[Classes]] 

In our Log example in classes, we were using int's with different names as our log levels:

```cpp
int LogLevelError = 0;
int LogLevelWarn = 1;
int LogLevelInfo = 2;
```

Now that we now how to use Enums, we can make our code better:

```cpp
enum Level: int {
	LogLevelError,
	LogLevelWarn,
	LogLevelInfo
};
```


Of course we still have to change the types on our functions and the names of the variables. The entire code would be:

```cpp
class Log {
public:
	enum Level: int {
		LogLevelError,
		LogLevelWarn,
		LogLevelInfo
	};
private:
	//m_ is used as a convention to specify a variable as a private class member
	Level m_LogLevel = LogLevelInfo;
public:
	void setLogLevel(Level level) {
		m_LogLevel = level;
	}
	void Error(const char* message) {
		if(m_LogLevel >= LogLevelError)
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
	//would work aswell
	//log.setLogLevel(Log::LogLevelWarn);
	log.Error("error test");
	log.Warn("warning test");
	log.Info("info test");
}
```