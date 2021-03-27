# msclang

msclang is a custom compiler written in python made to compile C to MSC bytecode. MSC bytecode is a bytecode language used by Super Smash Brothers for Wii U in order to control character logic. It's name comes from the mix of "clang" a popular C compiler and, of course, MSC itself, however no part of clang is used and the name is only intended for pleasure of the user. This in no way indicates any affiliation with clang or it's developers.

### Changes made to make the script work with Gundam Extreme Vs. Full Boost
1. The original msclang.py only accepts sys_functions with args passed in.
2. Tht game system doesn't recognize operation opposite for unaryOp "!", and will insist on using 0x2b on every occasion, thus I removed the removeLastCommand in the If operation.
3. This is the weirdest one: For conditions where the whole boolean operations are not (!), my system requires the binaryOp's "||" 0x35 and 0x34 changed.
So the condition is as follows: if (!(var1 != 0 || var2 != 1 && var3 == 3))
This will change the boolean operation for 0x35 to 0xAB and 0x34, which is equal to notIf, and trueLabel needs to be changed to falseLabel.
Interestingly, if there is only 1 binaryOp of either || or &&, it will not require the 0xAB and 0x34.
I am super confused by this, and upon deciphering it in terms of stack and commands, what I found is that the boolean logic is the same.
It is not like the script doesn't use 0x35 in other parts, so I don't get why the game requires this specific change. 
4. The game either accepts binaryOperationsFloat, or the conditions for isCommandFloat is not the same for my game's script. 
5. The game only accepts pushInt, so -i arg is needed

### Requirements

* Python 3 (3.5 or higher suggested)
* pycparser (Can be obtained using `pip install pycparser`)

### Installation

* Extract contents (unit_tests folder not needed) anywhere
* Add that folder to PATH if it is not already
* Ensure the proper script (msclang.bat on windows, msclang.sh on linux) uses python 3 (by default it uses the `python` command, however this may need to be changed to `python3` or your system's equivelant)

### Features

msclang is based heavily on C but has lots of differences in order to best suite the target environment. While parsing follows the C99 standard, some features are missing. Here is a small list of differences:

* Types are primarily limited to int/string
* strings cannot be modified, only offered in a string literal. A string literal is held as a reference to the string in the form of an id in ascending order starting from 0. References should be held by the `int` type.
* While a type must be provided when declaring a variable much type leniency is given to the user.  
* `struct`s are non-existant
* Due to constraints of the bytecode there is no memory access. The only allowed type of pointer is function pointers which should be stored in the `int` type from which they can be called the same as in C.
* Global variables cannot have an initial value, any initial value must be set within a function.
* Some areas may not properly auto-cast between float and int. This is a bug, please report it in the issue tab.

### Notes

MSClang is currently a work in progress. Some features may be incomplete or missing. While bug reports of currently implemented features are most definitely appreciated, please avoid bug reports that relate to features missing entirely. 

[Follow me on twitter for updates, similar projects and jokes](https://twitter.com/jam1garner)

[Check out my medium for write ups on reverse engineering MSC (and probably other stuff)](https://medium.com/@jam1garner)
