---
title: 'C++ Programming: Using the auto keyword'
date: 2021-10-05
permalink: /posts/2021/10/cpp_prog_auto_keyword/
tags:
  - programming
  - c++
---

## Overview

The ```auto``` keyword changed its semantics starting from the C++11 standard. From C++11 onwards, we have the following different flavors 
of the keyword [1]:

- ```auto```
- ```const auto&```
- ```auto&```
- ```auto&&```

Furthermore, we have ```decltype(auto)```. This post is a short guide on how to use ```auto``` and its various flavors.

## Using the ```auto``` keyword
The ```auto``` keyword changed its semantics starting from the C++11 standard. From C++11 onwards, the semantic of the keyword is 
automatic type deduction. This means that we can write code like the following:

```
...
// before c++11
int x = 5;

//from c++11
auto x = 5;
...
```

Thus, using ```auto``` may help us to write cleaner and less cluttered code. This is emphasized in particular when we consider function signatures. Compare the
code snippet below (example taken from [1]):

```
class Foo
{

int value()const{..}
const int& cref_value()const{...}
int& ref_value(){...}

};
```

with the following code snippet 

```
class Foo
{

auto value()const{..}
auto& cref_value()const{...}
auto& ref_value(){...}

};
```

The latter API is cleaner and simpler.The type returned is deduced by the compiler for us.

---
**Remark**

Althgough to a large extent using ```auto``` simplifies our code, overusing it can have the opposite resutl.
----

One other advantage of using ```auto``` is that we cannot leave the variable uninitialized. That is the following fails to compile

```
...
auto x;
...
```
This is reasonable as the compiler uses the right hand side value to deduce the type and hence the memory size it has to allocate. If there isn't a value there is nothing
to deduce from. Thus, unitialized variables are not allowed when using ```auto``` and the good news are that the compilers let us know the exact line number in our code that this
occurs. 

## References

1. Bjorn Andrist, Viktor Sehr, ```C++ High Performance, 2nd Edition```, Packt Publishing
