---
title: 'Java: Garbage Collection, GC'
date: 2025-09-08 02:58:44
tags:
  - Java
  - GC
  - JVM
  - Interview
categories:
  - Java
cover: /images/cover.jpg  # 站内图片
---
# what is GC?
GC is a kind of Memory Collection system, when you do something in Java. you must use memory.
You must to control the memeory, that make you application going longer.

Used memory how to auto collection, that what it doing.

## OutOfMemoryError
### when use too much memory, it will throw Error (OutOfMemoryError)
| Memory Part Name         | private/public | ErrorName                     |
|--------------------------|----------------|-------------------------------|
| Program Counter Register | private | OutOfMemoryError                     |
| JVM Stacks               | private | OutOfMemoryError, StackOverflowError |
| Native Method Stacks     | private | OutOfMemoryError, StackOverflowError |
| Java Heap                | public  | OutOfMemoryError                     |
| Method Area              | public  | OutOfMemoryError                     |
| Runtime Constant Pool    | public  | OutOfMemoryError                     |
### Program Counter Register
Every thread have a self private commmand line number, for point the command
### JVM Stacks
One of Two stacks, for User's Java Code.
Store the 
- <span style="color:red"> Local Variables </span> //base data and class
- <span style="color:red"> Operand Stack </span> // + - * /
- <span style="color:red"> Dynamic Linking </span> // bind the function invokevirtual
    - <span style="color:red"> Polymorphism </span> : just know the function invokevirtual (like a tip)
    Animal.speak, Dog.speak, Cat.speak three class
    invokevirtual know the Cat speak, so get the Cat speak function, not Animal speak function
- <span style="color:red"> Return Address </span> // function return address

### Native Method Stacks
One of Two stacks, for others Language like C/C++ Code. like below
```Java
public native void someNativeMethod(); // that's the Native fucntion
```
- for Computer hardware
- for speed using other Language
- for powerful third party Lib (like OpenCV)
- for some method .getclass() .hashCode() GC etc.

### Java Heap
Important part (GC Main Manager)
store the <span style="color:red"> Instance Object </span> and <span style="color:red"> Array </span>
for GC, It have alsp have some parts
- Young Generation
    - Eden // new Object
    - Survivor (S0/S1) // when Eden is full found the reference Object to another Empty Survivor and age +1, and clear Eden and this Survivor
- Old Generation
    - some age over (default 15) it will move this place

### Method Area, -XX:MaxMetaspaceSize
Metaspace is Method Area (GC Manager)
before Java 1.7, Named <span style="color:red"> PermGen </span> and in Java Heap
after Java 1.8+, Named <span style="color:red"> Metaspace </span> using a specical space in Memory

-XX:MaxMetaspaceSize：(Max MetaspaceSize), Default: unlimited
when can't get the space in Metaspace(Method Area)

### Runtime Constant Pool
- String // String s = "text"
- final int a = 10
- Primitive Types Value

- Symbolic References (Table for Dynamic Linking)
    - Fully Qualified Name (class)
    - Field Descriptor (field)
    - Method Descriptor (method)
use this table and the Dynamic Linking Information to find all things
---
***
___
# Some Questions
### Is GC only about Heap Memory ?
No, it manager <span style="color:red">Java Heap</span> and <span style="color:red">Method Area</span>
###  <span style="color:red"> Dynamic Linking (Symbolic References) to Direct Reference </span>
user.getName() -> com/example/User.getName:()Ljava/lang/String;
form Table (Symbolic References) get the Address
replace by this address

---
***
___
