# Inspecting Obj files symbols

Symbol is one of the basic terms when talking about object files, linking, and so on. In fact, in C/C++ language, symbol is the corresponding entity of most user-defined variables, function names, mangled with namespace, class/struct/name, and so on. For example, a C/C++ compiler may generate symbols in an object file when people define non-static global variables or non-static functions, which are useful for the linker to decide if different modules (object files, dynamic shared libraries, executables) would share same data or code.

## nm

We can inspect the symbols in object file using `nm` command line tool.

```console
nm object_file.o | c++filt 
```

using `nm` with `c++filt` allow us to demangle c++ names back. 

let's take the following example c++ code

```c++
// test.cpp
bool testFunc(){
        return true;
} // defined function
int declaredFunc(bool par); // declared but not defined
static double staticVar = 0.5; // static global variable
int main(){
    int var = 15;
    return declaredFunc(true);
}
```
We can compile this file as object file without linking it (using -c flag). 

```console
g++ -c test.cpp -o test.o
```

Then we can inspect the output object file using `nm`.

```console
nm test.o | c++filt
```
we should get something similar. 

```console
000000000000000f T main
                 U declaredFunc(bool)
0000000000000000 T testFunc()
0000000000000000 d staticVar

```
each line reperesents 

```
symbol value    symbol type    symbol name
```

### Symobl types

The following diagram shows the memory layout of the object file. 

![memory layout](media/memory-layout.jpg)

Object files are divided into sections, each serving a specific purpose. Common sections include:

- Text Section: Contains the machine code (executable instructions) of the program
- Data Section: Stores initialized global and static variables.
- BSS Section: Reserved for uninitialized global and static variables. It doesn't occupy space in the object file but is allocated when the program is loaded into memory.

We will use this info to understand the symbols types below. 

#### T t

This symbol is in the object file's .text code section. 

This is most of the time the case for any defined function in our code. 

example: `main` and `testFunc` in the code above. 

#### U

This symbol is undefined and no defination is found in the object file. 

example: `declaredFunc(bool)` in the code above. 

#### D d

The symbol is in the initialized data section. 

example: `staticVar` in the code above


## References

1. first
2. second
3. third



