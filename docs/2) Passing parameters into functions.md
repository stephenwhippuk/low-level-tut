# Passing Parameters into Functions

if we take the example in chapter 1, then we have the following data

```cpp

unsigned char v1 = 'B';
unsigned char v2 = '\n';
unsigned char* v3 = &v1 

```

we will build on this

## Passing by Value

let us take the function

```cpp

void printChar(unsigned char v4){
    std::cout << v4 << std::endl;
}

```

this tells the compiler to create an unsigned char value on the stack and then to output this to stdout followed by a new line


we could invoke this from our parent function to print the value of v1 by using either

```cpp

printChar(v1)

```

or

```cpp

printChar(*v3)

```

both of which will pass the value of v1 into the function, or more accurately cause the program to push the value of v1 onto the top of the stack. so a copy is available in the function. it has therefore done a Copy by Value as per chapter 1 and nothing we do to that variable inside the function will change the value of v1 for example

```cpp

void printChar(unsigned char v4){
    if(v4 < 'A'){
        v4 = 'A'; // legal if not advised and may throw a compiler warning
    }
    std::cout << v4 << std::endl;
}

...

int main(){
    unsigned char v1 = 'B';
    unsigned char v2 = '\n';
    unsigned char* v3 = &v1 

    printChar(v2)

    std::cout << v2 << std::endl;

}

```

will output 

```
A

```

copy by value means we cannot inadvertently alter things in the calling function, however requires a complete copy
or the argument to be passed in (Copy By Value). If we call the argument passed in 0xE0 then

```
        --------            --------            --------
0x01    | '\n' |            | '\n' |            | '\n' |
        --------            --------            --------
---                ---->                ----> 
        --------            --------            --------
0xE0    |      |            | '\n' |            | 'A'  |
        --------            --------            --------
```


## Passing By Reference

if instead we defined the function

```cpp
void printChar(unsigned char* v4){
    if(v4 < 'A'){
        v4 = 'A'; // legal if not advised and may throw a compiler warning
    }
    std::cout << v4 << std::endl;
}
```

then things would be different, we will not be passing in a pointer (reference) to the variable in main. This means
we get local copy of the pointer not the value. 

```cpp

void printChar(unsigned char* v4){
    if(*v4 < 'A'){
        *v4 = 'A'; 
    }
    std::cout << *v4 << std::endl;
}

...

int main(){
    unsigned char v1 = '\n';
    unsigned char v2 = 'A';
    unsigned char* v3 = &v1 

    printChar(&v1) // or printChar(v3);
    
    std::cout << v1 << std::endl;

}

```

we will get the output 

```
A
A
```

As we got a local copy of the pointer, when we access the value stored at it, its to the value it pointed at, which was v1. So by changing the value stored at the pointer we change the value of v1. The same is true if we pass an existing pointer in. A copy of the pointer is made, in the same way as copying a value, and this points to v1 

```

Memory DIagram for Pass By Reference (passing address in directly)
                            printChar(&v1)
        --------            --------            --------
0x01    | '\n' |            | '\n' |            | 'A'  |
        --------            --------            --------
---                ---->                ----> 
        --------            --------            --------
0xE0    |      |            | 0x01 |            | 0x01  |
        --------            --------            --------
```

or

```

Memory Diagram showing indirecton (passing a pointer in )

                            printChar(v3)
        --------            --------            --------
0x01    | '\n' |            | '\n' |            ! 'A'  |
        --------            --------            --------
---
        --------            --------            --------
0x03    | 0x01 |    ---->   | 0x01 | ---->      | 0x01 |
        --------            --------            --------
---
        --------            --------            --------
0xE0    |      |            | 0x01 |            | 0x01 |
        --------            --------            --------
```
