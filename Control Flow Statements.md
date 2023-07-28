The main control flow statements are:
- `continue`
- `break`
- `return`

## Continue
Continue can only be used in a loop. It will make the code go to the next iteration of the loop

```cpp
void aFunction(){
	for(int i = 0; i < 5; i++){
		if(i == 3)
			continue;
		cout << i << endl;
	}
	cout << "end of loop" << endl
}
```
>This code will output:
```bash
0
1
2
4
```
You can see that it skipped 3 and went right to 4. 

## Break
Break is primarily used in loops but it also appears in switch cases. The break command will end the current loop.

```cpp
void aFunction(){
	for(int i = 0; i < 5; i++){
		if(i == 3)
			break;
		cout << i << endl;
	}
	cout << "end of loop" << endl
}
```
>This code will output:
```bash
0
1
2
end of loop
```
You can see that it ended the loop after 3.

## Return
This can be considered the most powerful of the three. Return will break out of the function entirely.  Return generally is used outside loops, in functions, but can be used in loops aswell.

```cpp
void aFunction(){
	for(int i = 0; i < 5; i++){
		if(i == 3)
			return;
		cout << i << endl;
	}
	cout << "end of loop" << endl
}

```
>This code will output:
```bash
0
1
2
```
You can see that it didn't even print `end of loop`. 
