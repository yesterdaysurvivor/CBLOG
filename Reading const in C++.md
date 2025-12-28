---
title: Reading const in C++
nav_order: 2
---

## What does `const` really mean?

`const` keyword restricts how an object can be accessed and modified, not how it exists in memory.


## Basic usage

```cpp
const int x = 10; //Const Object
x = 11; //Compile Error: as x is constant
```

## How to use const with pointers and References?

```cpp
//These expressions are read from right to left
const X* ptr;  //Pointer to const X
X* const  ptr; //Const ptr to X
const X* const ptr; //Const pointer to const X
```
## What does it mean by "Pointer to const X?"

There are 2 parts of this expression, 'ptr' and the object it is pointing to. When we say if a 'ptr' is a pointer to const X it means you cannot use 'ptr' to modify X but you can change the 'ptr' itself as it is not a constant.

```cpp
const X x;
X x1;
const X* ptr=&x;
ptr->mutate(); //ERROR as we cant change x through ptr
ptr = &x1; // We can reassign the pointer as it is not the one being affected by const
ptr->mutate(); //Now works fine as it is pointing to x1 which is a non-const
```

## What does it mean by "Const ptr to X?"

A const pointer to X means the pointer itself is constant and cannot be changed, but the object it points to can be modified. You cannot reassign the pointer to point to a different object.

```cpp
X x;
X x1;
X* const ptr = &x;
ptr->mutate(); // OK - we can modify x through ptr
ptr = &x1; // ERROR - we cannot reassign ptr as it is const
```

## What does it mean by "Const pointer to const X?"

This is the most restrictive form. Both the pointer and the object it points to are constant. You cannot modify the object through the pointer, nor can you reassign the pointer to point to a different object.

```cpp
const X x;
const X x1;
const X* const ptr = &x;
ptr->mutate(); // ERROR - cannot modify x through ptr
ptr = &x1; // ERROR - cannot reassign ptr as it is const
```

## Summary: The Right-to-Left Rule

Remember to read pointer declarations from **right to left**:
- `const X* ptr` → ptr is a pointer to const X (ptr is non-const, X is const)
- `X* const ptr` → ptr is const pointer to X (ptr is const, X is non-const)
- `const X* const ptr` → ptr is const pointer to const X (both are const)

## Using const with reference

The rules are similar for const with references. However, a reference is always const by nature — once assigned, it cannot be reassigned to refer to a different object.

```cpp
X& const ref; // ERROR - This is invalid because references are always const
const X& ref; // OK - const reference to X, cannot modify X through ref
```

## East const style

It is another way to write const expressions and is considered a recommended style because of its readablity(although less commonly used). Here we write `const` to the right of the type being "constified".

```cpp
X const * ptr; // const is right of X. It means X is const (pointer to const X)
X * const ptr; // const is right of pointer asterisk. It means pointer is constant (const pointer to X)
X const * const ptr; // const on both sides. It means both X and ptr are const (const pointer to const X)
```

**Comparison: West const vs East const style**

| Expression | West const (Traditional) | East const (Modern) | Meaning |
|---|---|---|---|
| Pointer to const X | `const X* ptr` | `X const * ptr` | Cannot modify X |
| Const pointer to X | `X* const ptr` | `X * const ptr` | Cannot reassign ptr |
| Const pointer to const X | `const X* const ptr` | `X const * const ptr` | Cannot modify X or reassign ptr |

## Exercises

### Exercise: Identify what is constant

For each declaration below, identify what is constant (the pointer, the object, or both) and explain why:

```cpp
// 1. const int* const ptr = &x;
//    What is constant? _________
//    Why? _________

// 2. int* const ptr = &x;
//    Can you reassign ptr? _________
//    Can you modify *ptr? _________

// 3. const char* str;
//    Can you modify the character? _________
//    Can you reassign str? _________

// 4. const std::vector<int>* const vec;
//    What restrictions does this have? _________
```
