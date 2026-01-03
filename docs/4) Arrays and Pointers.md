# Arrays and Pointers

Whilst I could have done types later on, I chose to do that general topic first because it will make life easier talking about arrays in detail.

## What is an Array
so in low level parlance, 

> an array is a contiguous sequence of bytes, of a singular type, which is to say all elements are of the same size at runtime

The requirement that they are of the same size will become obvious shortly, and as a result of this, arrays must be of a known size and homogenous.

in C++ we can create an array as follows

```cpp
int myArray[5] = {1,2,3,4,5};
```

## Using Arrays 

ignoring the details, the followung diagram illustrated this in memory

```
           +0    +1   +2    +3    +4
        -------------------------------
0x01    |  1  |  2  |  3  |  4  |  5  |
        -------------------------------
```

and this allows us to write something like the following to access the values sequentially

```cpp

    for (int i = 0; i < 5; i++){
        std::cout << myArray[i] << (i < 4) ? ',' : std::endl;
    }
```

its important to note that in C++ low level arrays have no bounds checking, they are literally just a sequence of allocations of the specified type. This means that if you make a mistake in indexing the results will kind of depend on what else is in memory. If the location is unallocated then the operating system will throw a segfault, crashing the program. However if by bad luck there is something else there, it will just access it leading the subtle and extremely hard to locate bugs. 

## Pointers
There is a very close relationship between pointers and arrays. 

the statements 

```cpp

int x = myArray[0];
int y = *myArray;

```

are both valid and do precisely the same thing. If we think back to the last chapter this means that you can use pointer arithmetic as follows

```cpp

int x1 = myArray[1];
int y1 = *(myArray +1);
```

## Important Understandings

An array is just sugar syntax over pointers. This is why an array has to be homogenous and also why the reference type is so important, it exists at compile time only, but ensures the memory calculation is correct. 

for instance, the above array has 5 * sizeof(int) bytes, that is 20 bytes in a modern C++ compiler. If the pointer has to a char then *(myArray +1) wouldn't lead to the start of the next integer but to the second byte of the first integer.

As the array will be statically allocated, the size argument has to be a constant, so it can provide enough space on the stack for this structure. If we need array lengths to be mutable or depending on some other dynamic factor, then we will need to dynamically allocate memory from the heap which is the topic of the next chapter. 

