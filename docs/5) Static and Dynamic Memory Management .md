# Static and Dynamic Memory Management
up until this point we have been dealing purely with static that is compiler managed memory. In this chapter we will look at whats going on here in a little more detail and look at how the programmer can manage memory themself where the allocations may be unknowable at build time. 

## Static Memory
I have mentioned in passing that the compiler will use the stack to provide static memory management, it does this because its highly convenient to do so. However it helps to see this in action. Global variables can and probbably will be hadled in their own way, so we will be talking about local scoped variables here. Most machines have a Base Register and a Stack Frame register, but I'm not truly interested at this point how hardware actualy works in a given CPU etc, instead I will be using a fictious one just for understanding. 

We will call this FR. When a block is entered, this frame register is set to the top of the current stack to serve as a local base and then the compiler will push to the top of the stack, all memory it needs for thos routine. This will all be popped back off again when the block is finished, and the FR returned to its previous location, ensuring that they have a lifetime of just the block in which they were declared. 

This is true whether they are parameters into a function, local variables or block scoped ultra local variables within the function. 

So lets take the following block:

```cpp
int v1 = 10;
int v2 = 20;
```

This would give us a stack that looks something along the lines of:

```
FP ->   [10] 4 BYTES
        [20] 4 BYTES
```

and when the compiler is compiling the binary in pseudo assembler it would look something along the lines of: 

```cpp
v2++;
```

```asm
LDA AX, FP+4
INC AX
```

Ignoring the details, the important point to note is that it uses the FP and an offset to refer to a specific variable. All that remains in the code, is its position, (up to the compiler), and the number of bytes that are pushed, determined by its type. 

We can refer to variables in parent scope with negative offsets. Anything like shadowing etc etc, these are just language details for the compiler to figure out in determining the correct offsets.

This is statically typed static memory management, and hense at runtime, the type is simply what ops and what offsets were chosen.  

## Dynamic Memory Management 
We note that the creation and lifetimes of memory allocations are simply managed by the compiler when memeory is statically allocated. Simple stack operations suffice to ensure that this is managed, and offsets will be set appropriately

However, the programmer can take ownership of this where this isn't possible. this means two things: (1) we need a way of allocating memory when we need it, and releasing it when its no longer required and (2) we are going to need a pointer to keep track of where the memory is, so we can interact with it.

In C++ we can create a dynamic variable as follows:

```cpp
double* myDynamic = new double(10.0);
```

This will allocate space in the freestore for a single double (8 bytes) and return a pointer with a reference type of double. 

Rhe variable, myDynamic itself, is a static allocation and therefore on the stack.

When we are done with the memory, before the scope of the pointer is left we need to delete the memory as follows

```cpp
delete myDynamic;
```

The system will clean up the memory automatically when the program exits (the OS ensurers cleannup of processes). However, if the pointer drops out of scope and is thus popped off the stack then the address of the memory is lost; the allocation remains however cause we never informed the system it wasn't needed anymore, this is therefore a *memory leak*

### Dynamic Arrays
It is possible to allocate multiple variables at one time to create an array, indeed this is one of the more common uses of dynamic memory management in a C++ program. (aside: often using a standard container that does it for you). 

The syntax is as follows

```cpp
double* myArray = new double[5];
...
delete [] myArray;
```

## Important Understandings
Static Memory Management is handled by the compiler, you don't need to worry about it and unlike in C#/Java etc, it is default, anything can be created as a static and will be on the stack. 

Static allocations, because they are simply sizes of bytes and will be managed as offsets to a set base, need their sizes known in advance, 

Dynamic Memory management has no garbage collector: whereas in a garbage collected language the programmer needs only worry about allocating the memory and never needs to worry about its lifetime, in C++ and other low level languages, the programmer needs to be very concerned about this. 

A pointer being deleted because its static and this lost from the stack when the block exits, does not lead to the memory it points to disappearing and cannot. If you thiknk about passing a pointer into a function you will see why. 

C++ offers safe types that do encapsulate lifetimes such as sharedptr and uniqueptr, but they are high level standard library details to be considered at a future date. 


