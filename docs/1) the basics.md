
This diagram shows three memory locations (x, y, z) as blocks. Each block is labeled with its symbol, value, and address. For pointers, the value can be shown as the address it points to.
# Basics

## Copy By Value

let us consider the following statement

```cpp
unsigned char v1 = '\n';
```

here we are informing the compiler that we wish storage to be allocated for an unsigned char (byte) and initialise it to '\n' (10)

somewhere in the compilers data, it holds a symbol table. into which it will add the symbol v1, add its semantic information that its an unsigned char (byte) and that it should be initialised to 10

when its writing its binary output it will allocate this space (actually pushing a byte onto the stack) and add appriate machine code to initialise its value to 0x0A, probably a single push instruction 

however wherever it stores it, that location has an address. lets call it 0x01. 

```cpp
unsigned char v2 = v1;
```

we have added a second value to the symbol table. it may or may not be clever and take the value at compile time for v1 and copy it or it may add runtime ops, to get the value stored at 0x01 and copy it to the location of v2 lets call the location of v2 0x02.

important to note, that VALUES have been copied, there is no relationship between v1 and v2 so

```cpp
v1 = 'A';
```
now the value at 0x01 will be 'A' and the value at 0x02 will still be '\n'

```mermaid

Memory Diagram showing copy by value 

        --------            --------            --------
0x01    | '\n' |            | '\n'  |           |  'A' |
        --------            --------            --------
---                ---->                ----> 
        --------            --------            --------
0x02    |      |            | '\n' |            | '\n' |
        --------            --------            --------
```



## Copy By Reference

now lets consider the following statment

```cpp

unsigned char* v3 = nullptr;
```

this will add a new symbol as with v1 and v2 lets call its location 0x03. 

*(technical: it will infact be a multibyte symbol for the pointer but for simplicity we will consider it a byte)*

this will have the value 0x00 (0). assuming the byte size it is indistinguishable in memory from any other symbol.

however the compiler has the different semantic information in its symbol table, that indicates that its a pointer, not a regular variable. 

```cpp
v3 = &v1;
```

as v3 is a pointer type it will store the address of another location. this could under the hood be any address in memory as we will see, but for now we take the address of v1 and store it in v3. so now the value of v3 is 0x01. 

```cpp
std::cout << *v3 << std::endl;
```
ignoring the details this writes the value 'A' to stdout.

the & says take the address of, the * means take the value stored at the location it points at. 

```cpp
v1 = 'B';
std::cout << *v3 << std::endl;
```

would output 'B' to stdout. the value of v3 hasn't changed, it still holds the address of v1. the value of v1 has changed so when we access the value stored at where v3 points to (0x01) the updated value is returned. this is called *indirection*, because we are accessing the value of v1 indirectly via v3. 

```mermaid

Memory Diagram showing indirecton

        --------            --------
0x01    | 'A'  |            | 'B'  |
        --------            --------
---
        --------            --------
0x02    | '\n' |    ---->   | '\n' |
        --------            --------
---
        --------            --------
0x03    | 0x01 |            | 0x01 |
        --------            --------

```





