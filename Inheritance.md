Inheritance allows us to have a hierarchy of classes that relate to each other. You can create sub classed of classes that **inherit** functionality and variables from your parent class. Inheritance allows us to alleviate unnecessary code duplication.

## Creating Inherited Classes

We want to create a parent class that is called `entity` and a child class called `player`. The `player` class will need all the variables and functions in the entity class an it will have its own requirements. 

### First Creating Classes Without Inheritance

Here we will see what would happen without inheritance. There is quite a bit of code duplication:

```cpp
class entity {
public:
	//Every entity in a game might have x and y positions
	float x,y;
	
	//Every entity in a game might move
	void move(float x, float y){
		this-> x += x;
		this-> y += y;
	}
};

class player{
public:
	//Only the player will have a name
	const char* name;
	//Every player in a game might have x and y positions
	float x,y;
	
	//Every player in a game might move
	void move(float x, float y){
		this-> x += x;
		this-> y += y;
	}
	
	//Only the player will have a printName function
	void printName(){
		cout << this-> name << endl
	}
}
```

### Creating Classes With Inheritance

We can `extend` our entity class to the `player` class to get rid of code duplication.

> We can inherit parent classes with `: public parentClassName`.

When we inherit the `entity` class inside of the `player` class, we will get to use the `entity` class's variables and functions.

```cpp
class entity {
public:
	//Every entity in a game might have x and y positions
	float x,y;
	
	//Every entity in a game might move
	void move(float x, float y){
		this-> x += x;
		this-> y += y;
		cout << this-> x << ", " << this-> y << endl; 
	}
};

class player : public entity {
public:
	//Only the player will have a name
	const char* name;
	
	//Only the player will have a printName function
	void printName(){
		cout << this-> name << endl;
	}
}
```

The `player` class is looking cleaner now.

> Anything inside the parent class that is public can be used in sub classes.

Let's create some instances for `entity` and  `player` classes:

```cpp
int main() {
	player p;
	//this is a native player variable
	p.name = "playerName";
	//this is a native player function
	p.printName();
	
	//these are inherited variables
	p.x = 10.0f;
	p.y = 12.0f;
	//this is an inherited function
	p.move(4.0f, 13.2f);
}
```

The output would be:

```
playerName
14.0, 25.2
```

We get to use both inherited and native functions and variables in our sub-class now.

> You can actually pass a `player` type object to an `entity` type function because it is guarantied that the `player` object will have the same things as the `entity` object. But this wouldn't work in reverse.  

> You can also override anything from a parent class such as variables and functions. You can find more in [[Virtual Functions]].