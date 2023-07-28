The linker will gather all the object files that were created during the [compilation](Compiler.md) stage and convert them to an executable file. You will have to select the target instruction set for the linker.

>The entry point for a program doesn't have to be the Main.cpp file. You can change the entry point by modifying your linker settings.

## Errors

If the linker can't link the project together(e.g. can't find the entry point (main())), it will throw out a linker error code. 

You can differentiate between [compiler](Compiler.md) and linker errors via the error code. 

- The error code for linker errors start with `LNK` (e.g. `LNK1561`).
- The error code for compilation errors start with `C` (e.g. `C2143`).
	- Syntax errors are compiler errors.

