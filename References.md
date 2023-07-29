References are an extension of pointers. References are just pointers in disguise. They make our code a little easier to read and follow.
- A reference is just a way of reference an **existing** variable.
- Unlike a pointer though, you can't create an empty reference by assigning a `nullptr` to it. References **have to reference an already existing variable**.
- References are not actually variables like pointers are. They don't actually take up space in memory.
- **References are just basically alias.**

## Creating References

```cpp
int a = 5;
Log(a);

int& ref = a;
Log(ref);
```

The output would be:
```bash
5
5
```

Another example:

```cpp
int a = 5;
Log(a);

int& ref = a;

ref = 10;

Log(a);
```

The output would be:

```bash
5
10
```

Even though we assigned `ref` as 10, the `a` variable was also assigned 10. Because `a` and `ref` are the same thing. `ref` is just an alias for `a`.

## How Can We Use It In An Application

Here is a function that should increment variable. This code doesn't function. We are going to explore how we can make it function by comparing pointer an referencing methods.

```cpp
void Increment(int value){
	value++;
}
int main(){
	int a = 5;
	Increment(a);
	Log(a);
}
```

The output:

```shell
5
```

### Pointer Example

The value of `a` hasn't incremented in the example above. But we don't want that. Instead of this, we can do it by passing the memory address as a parameter and dereferencing it in the function.

```cpp
//Taking the pointer of our value as a parameter
void Increment(int* value){
	//dereferencing and incrementing the value of the memory address.
	*value++;
}

int main(){
	int a = 5;
	//Passing the memory address of the value a
	Increment(&a);
	Log(a);
}
```

The output would be:
```shell
5
```

This happens because of the order of operations. The incrementation would happen first and then it would dereference. So to fix this we can:

```cpp
//Taking the pointer of our value as a parameter
void Increment(int* value){
	//dereferencing and incrementing the value of the memory address.
	(*value)++;
}

int main(){
	int a = 5;
	//Passing the memory address of the value a
	Increment(&a);
	Log(a);
}
```

The output would be:
```shell
6
```


### Reference example

```cpp
void Increment(int& value){
	//You can get rid of the whole process of dereferencing if we only take the reference of a value.
	//before:
	//(*value)++;
	//now:
	value++;
}

int main(){
	int a = 5;
	//since we take it in by reference we don't even need to pass it as an address.
	Increment(a);
	Log(a);
}
```

The output:

```shell
6
```

As you can see, references made our life soo much easier. 

## Important Note

Once you declare a reference, then you cannot change what it references.

An example:

```cpp
int a = 5;
int b = 8;

int& ref = a;
ref = b;
```
This effectively would be equal to:

```cpp
int a = 5;
int b = 8;

a = b;
```

If you really had to do it. You can create a pointer that will point to the variable.

```cpp
int a = 5;
int b = 8;

// ref is now assigned to the address of a
int* ref = &a;
//dereferencing ref and getting to the a variable's value and setting it to 2
*ref = 2;

// ref is now assigned to the address of b
ref = &b;
//dereferencing ref and getting to the b variable's value and setting it to 1
*ref = 1;

Log(a);
Log(b);
```

The output would be:
```shell
2
1
```