# Types and Pointers

The first 2 topics kind of flow first, but the next few chapters could really have been done in any order, however I have decided to discuss types and their pointers first. 

# What is a Type
Whilst it may seem pretty obvious to anyone with a bit of programmig experience, what a type is. When we start looking at things at a lower level it increasingly becomes important to understand them in terms of the machine ratrher than abstractions that we usually think of when talking about them. 

> Put simply a type is data and the operations that work upon that data to give it its semantics. 

However, this needs some additional explanation: when we talk about data here we are simply talking about a collection of unformatted bytes, it is the operations that give specific sequences of bits meaning. So really when we talk about data at the machine level we are just talking a size in bytes that the type needs. everything else is defined in terms 
of how the operation manipulates those bytes.

# Run Time and Compile Time Types
The above, strictly defines a runtime type, a compile time type typically has lots of structure in the high level language, this is lost to varying degrees depending on the language when translated into a run time type

- at the one end we have a language like C# or Java, it maintains basically the full information of the type at both runtime and compile time

- at the other end we have something like C, where strictly speaking there is no extant type information left with the runtime object at all. It remains only virtually in the operations. 

- in the middle you get languages like C++, where some runtime information (that which is required for runtime polymorphism ) is retained, but where it is static, it behaves like C. 

# Types in C++ and pointers
We will be considering this from the perspective of C++ but it is important to note that different languages vary in their ways of storing runtime type information. 

Complex types are handled by classes/structs that we will come back to, for now I wish to only focus on primitive types: from which I will be using *char*, *int* and *double*.

In modern C++ compilers *chars* have a size of 1 byte, *ints* a size of 4 bytes and *doubles* a size of 8 bytes. the first 2 are integral types and the last a floating point type. We can see that each, (as arithemtic types), will support the same operations in general +, - etc. However, equally as they are different formats, (sizes at least), different ops will be needed to interact with them in the binary. This makes them distinct types both at compile time and run time.

If we come back to the symbol table, it is the semantics of the type for the symbol in this, that determine which ops the compiler should select. However, once this has been done, the knowledge of the type is lost. 

For all static types this is completely ok. the compiler will know in advance precisely what it is dealing with and can use the correct ops for it. 

But what about pointers? 

Well becauase it needs to know what the type of the object is to compile the right ops, it means the pointer must be to a fixed type too. so an *int** is a pointer to an *int*. 

When i use something like: 

```cpp
    int v = 10;
    int* i = &v;
    (*i)++; 
```

as i is a pointer to an *int* then it knows that incrementing the value of what it points to will also be an *int*. 

so a pointer itself is a type, it has a size great enough to hold the addresses of the architecture upon which it will run, it also has a reference type that determines the type of what it can point to. It also has distinct operations for deferencing and assignment, copy etc that define it as a type.

What's important to note, is that all this is a compile time feature. At run time there is no such information and there doesn't need to be because it is static. 


# Pointer Arithmetic
This will be covered in detail in future chapters, however at this point I wish to note that a pointer can be provided an offset in memory
```cpp
    *(i+1)++ 
```

Where this makes sense will become clear, but its important to note that the size of thing it points at matters here. If it was a *char* pointer then this would be (addr of v + 1 byte), however as its an *int* pointer it would be (addr of v + 4 bytes) 

**important**

Pointer arithmetic can be very dangeous if not used carefully and is one of the biggest causes of C++'s bad reputation. In general you never need to do it in modern c++ at least directly. but understanding the principle matters. 

# C and C++ Philosphy around typing
Languages like C# and Java and even moreso Pascal like languages, tend to be highly finickity about semantics and enforce them upon the programmer, whether it prevents them from doing what they want or need to do or not. This price is paid for runtime safety. 

C and to a slightly lesser extent C++ have a very different philosophy. C will let you hang yourself, because it doesn't enforce things upon you in the same way, or at all. It simply gives you facilities to be able to do it more safely than trying it at machine level. C is statically typed but its not strongly typed (or at least its easy to get round it).

C++ again is a middle road here. It does hold the philosophy that in general the programmer knows what they are doing, but on the other hand it also provides all the facilities to do it truly safely ,(post c++11 anyway). What it will do is warn you, in many cases make it ugly and cumbersome to do more dangerous operations, or at very least force you to make the choice if you are writing in modern idioms. I would still say C++ is strongly typed, it will enforce it on you are compile time, unless you deliberately do something to circumvent it.  

The exception here, (sadly), is around pointers themselves, which for legacy C and older C++ support reasons, cannot actually change and it leaves the programmer being encouraged, (cause its simpler), to use raw rather than safe pointer types. That doesn't mean the code is necessarilly bad code of course; it just removes a very powerful safety net. 

This is a powerful feature of C++ that cannot be understated, its also its current downfall, cause sadly the programmer often doesn't know what they are doing, or at least isn't paying attention all the time and silly and potentially dangerous bugs slip through the net. On the other hand for performant small projects its still better than Rust in my opinion. I see why Rust for larger projects. 

This all said, the fact is you can if you obey the rules, write very safe C++ code, in a fraction of the time it takes to write a Rust alternative, and it'll be a whole lot more fun unless you really enjoy fighting with a compiler... you just cannot be as absolutely certain there aren't hidden gremlins in it.  

