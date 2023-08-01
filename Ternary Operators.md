Ternary operators are used to create inline [if-else](obsidian://open?vault=c%2B%2B&file=Conditions%20And%20Branches)statements.

Normal way of writing if-else conditions:

```cpp
static int s_level = 1;
static int s_speed = 2;

int main(){
	if (s_level > 5)
		s_speed = 10;
	else
		s_speed = 5;
}
```

We can clean this up using ternary operators:

```cpp
static int s_level = 1;
static int s_speed = 2;

int main(){
	s_speed = s_level > 5? 10 : 5;
}
```

The general syntax is as follows:

```cpp
variableName = condition ? ifTrueAssignTHis : elseAssignThis
```

## Nesting Ternary Operators
We can nest ternary operators:

```cpp
s_speed = s_level > 5 ? s_level > 10 ? 15 : 10 : 5; 
```

This code would be the equivalent of this code:

```cpp
if (s_level > 5){
	if(s_level > 10)
		s_speed = 15;
	else
		s_speed = 10;
}
else
	s_speed = 5;
```

As you can see, nesting ternary operators might make it harder to understand them.

## Using Logic Operators While Using Ternary Operators
While using `=, <, >, <=, >=` operators we don't really have to think about order. We have to think about it when we use logic operators. In the example below:

```cpp
s_speed = s_level > 5 && s_level < 100 ? s_level > 10 ? 15 : 10 : 5; 
```

How would the `&&` operator be processed? The logic operators are processed after conditional operators. That would mean the code above would be equivalent to this:

```cpp
s_speed = (s_level > 5 && s_level < 100) ? s_level > 10 ? 15 : 10 : 5;
```

Writing code like this will make your code unreadable. Try to write simpler ternary operators.