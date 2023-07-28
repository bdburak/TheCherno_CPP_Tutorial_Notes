
## Showing And Manipulating Folders And Files In The Explorer
Normally you only have `filers` in your solution explorer panel, not folders. These are virtual folders that don't actually exist. If you want to create folders in your project you have to enable the `Show all files` option
![[Pasted image 20230716165155.png]]

## Project Properties

### Changing The Executable Output Directory

Right click the project on solution explorer and select `Properties` 
![[Pasted image 20230716165804.png]]
Change the configuration and platform to all.
![[Pasted image 20230716165904.png]]

Change the `Output Directory` value to 
> `$(SolutionDir)bin\$(Platform)\$(Configuration)\`

### Changing The Intermediary Output Directory

The only change will be to enter this value to the `Intermediary Directory` value

>`$(SolutionDir)bin\intermediates\$(Platform)\$(Configuration)\`

It should look something like this:

![[Pasted image 20230716170424.png]]
