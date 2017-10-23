---
layout: single
title:  "代码评审Checklist"
date:   2017-08-08 16:16:01 -0600
categories: Technology Management
toc: true
toc_label: 目录
toc_icon: group
---

![代码评审](http://7xil47.com1.z0.glb.clouddn.com/2017-08-08-Code-Review-Meeting-Macadamian-Checklist.png)

本文内容主要介绍了代码评审的关注点，在合并代码之前，应该按照如下的规则进行检查。

**目录**

- General Code Smoke Test 通用测试
- Comments and Coding Conventions 注释和代码风格
- Error Handling 错误处理
- Resource Leaks 资源泄漏
- Control Structures 控制结构
- Performance 性能
- Functions 函数
- Bug Fixes bug修复
- Math 数学

## 通用测试 ##

**Does the code build correctly?**

No errors should occur when building the source code. No warnings should be introduced by changes made to the code. 
> 代码可以正确编译：编译代码时应无错误。

**Does the code execute as expected?**

When executed, the code does what it is supposed to.
> 代码是否像预期结果那样执行？

**Do you understand the code you are reviewing?**

As a reviewer, you should understand the code. If you don't, the review may not be complete, or the code may not be well commented.
> 你理解正在review(评审)的代码了吗？作为一个评审者，你应该理解这些代码；如果不能理解，将导致评审不充分或效果不太好；如果不能理解，也许是代码的注释做得不够好。

**Has the developer tested the code?**

Insure the developer has unit tested the code before sending it for review. All the limit cases should have been tested.
> 确保开发者本人已经测试过代码。在提交评审前，确保开发者已经完成了代码的单元测试。所有可能的情况都应该测试。

## 注释和代码风格 ##

**Does the code respect the project coding conventions?**

Check that the coding conventions have been followed. Variable naming, indentation, and bracket style should be used. 
> 代码是否遵循项目的编码风格？检查编码风格是否已经遵循，比如变量命名、缩排、括号风格等

**Does the source file start with an appropriate header and copyright information?**

Each source file should start with an appropriate header and copyright information. All source files should have a comment block describing the functionality provided by the file.
> 每个源代码文件都有合适的头信息和版权信息，所有的源文件都应该有一个描述本文件主要作用的注释块。

**Are variable declarations properly commented?**

Comments are required for aspects of variables that the name doesn't describe. Each global variable should indicate its purpose and why it needs to be global.
> 如果变量的声明不能表明自己的用途，则需要有合适的注释。全局变量需要说明它的用途以及为什么要声明为全局变量。

**Are units of numeric data clearly stated?**

Comment the units of numeric data. For example, if a number represents length, indicate if it is in feet or meters.
> 数值数据块有否明确描述。比如，如果一个数字代表长度，应标明单位是英尺还是米

**Are all functions, methods and classes documented?**

Describe each routine, method, and class in one or two sentences at the top of its definition. If you can't describe it in a short sentence or two, you may need to reassess its purpose. It might be a sign that the design needs to be improved.
> 所有的函数、方法和类都能说明用途，在定义程序、方法、类以前，用一两句简短的话描述它们。 如果不能在一两句话之内描述清楚，应该重新考虑它的目的；这也是需要改进设计的一个信号。

**Are function parameters used for input or output clearly identified as such?**

Make it clear which parameters are used for input and output. 
> 明确说明哪些参数用于输入、哪些用于输出。

**Are complex algorithms and code optimizations adequately commented?**

Complex areas, algorithms, and code optimizations should be sufficiently commented, so other developers can understand the code and walk through it.
> 复杂算法和代码优化 需要足够的注释。以确保开发人员可以理解并审核。

**Does code that has been commented out have an explanation?**

There should be an explanation for any code that is commented out. "Dead Code" should be removed. If it is a temporary hack, it should be identified as such.
> 注释掉的代码是否有解释？"Dead Code" 必须删除。

**Are comments used to identify missing functionality or unresolved issues in the code?**

A comment is required for all code not completely implemented. The comment should describe what's left to do or is missing. You should also use a distinctive marker that you can search for later (For example: "TODO:francis"). 
> 缺少的函数功能、没有完全解决的问题需要注释表示。注释要描述剩下的工作以及缺少的东西。最好加一个标记以便将来查找，例如“TODO:fancis”。

## 错误处理 ##

**Are assertions used everywhere data is expected to have a valid value or range?**

Assertions make it easier to identify potential problems. For example, test if pointers or references are valid. 
> 在需要有效 值或范围 的任何地方使用断言。如指针和引用。

**Are errors properly handled each time a function returns?**

An error should be detected and handled if it affects the execution of the rest of a routine. For example, if a resource allocation fails, this affects the rest of the routine if it uses that resource. This should be detected and proper action taken. In some cases, the "proper action" may simply be to log the error.
> 所有出错恰当的处理。如果该错误影响到接下来程序的执行（比如资源分配失败），通常可以简单的写log文件来处理。

**Are resources and memory released in all error paths?**

Make sure all resources and memory allocated are released in the error paths.
> 在出错时，确保所有资源包括内存 分配被释放。

**Are all thrown exceptions handled properly?**

If the source code uses a routine that throws an exception, there should be a function in the call stack that catches it and handles it properly. 
> 所有抛出的异常恰当处理。如果抛出了异常，应该有对应合适的catch函数

**Is the function caller notified when an error is detected?**

Consider notifying your caller when an error is detected. If the error might affect your caller, the caller should be notified. For example, the "Open" methods of a file class should return error conditions. Even if the class stays in a valid state and other calls to the class will be handled properly, the caller might be interested in doing some error handling of its own.
> 发生错误时，最好能够通知调用者。

**Has error handling code been tested?**

Don't forget that error handling code that can be defective. It is important to write test cases that exercise it. > 错误处理 的代码是否已经测试过。

## 资源泄漏 ##

**Is allocated memory (non-garbage collected) freed?**

All allocated memory needs to be freed when no longer needed. Make sure memory is released in all code paths, especially in error code paths. 
> 分配的资源都被释放了吗？所有的资源（内存）在不需使用时都应释放。特别注意异常条件下的情况。

**Are all objects (Database connections, Sockets, Files, etc.) freed even when an error occurs?**

File, Sockets, Database connections, etc. (basically all objects where a creation and a deletion method exist) should be freed even when an error occurs. For example, whenever you use "new" in C++, there should be a delete somewhere that disposes of the object. Resources that are opened must be closed. For example, when opening a file in most development environments, you need to call a method to close the file when you're done.
> 即使发生异常，所有的对象（数据连接，Sockets和文件等）是否都已释放。资源打开对应就该有关闭。

**Is the same object released more than once?**

Make sure there's no code path where the same object is released more than once. Check error code paths.
> 不要重复释放同一对象。检查异常处理代码的情况。

**Does the code accurately keep track of reference counting?**

Frequently a reference counter is used to keep the reference count on objects (For example, COM objects). The object uses the reference counter to determine when to destroy itself. In most cases, the developer uses methods to increment or decrement the reference count. Make sure the reference count reflects the number of times an object is referred. 
> 确保 代码与引用计数（reference counting）保持同步。
P.S.: STL中允许使用SmartPointer,而不能自动实现引用计数（请参见开放源代码 Boost 库中的 shared_ptr 类，或者参见STL中的更加简单的 auto_ptr 类）。COM中有相关的应用。

## 线程安全性 ##

**Are all global variables thread-safe?**

If global variables can be accessed by more than one thread, code altering the global variable should be enclosed using a synchronization mechanism such as a mutex. Code accessing the variable should be enclosed with the same mechanism.
> 所有全局变量都是线程安全的。如果允许一个以上线程访问全局变量，应该采用互斥之类的机制。

**Are objects accessed by multiple threads thread-safe?**

If some objects can be accessed by more than one thread, make sure member variables are protected by synchronization mechanisms. 
> 确保多线程安全存取（访问）对象。

**Are locks released in the same order they are obtained?**

It is important to release the locks in the same order they were acquired to avoid deadlock situations. Check error code paths. 
> 解锁顺序与它们获得的顺序相同，以避免死锁情况发生。

**Is there any possible deadlock or lock contention?**

Make sure there's no possibility for acquiring a set of locks (mutex, semaphores, etc.) in different orders. For example, if Thread A acquires Lock #1 and then Lock #2, then Thread B shouldn't acquire Lock #2 and then Lock #1.
>是否存在可能的死锁或线程争抢资源？

## 控制结构 ##

**Are loop ending conditions accurate?**

Check all loops to make sure they iterate the right number of times. Check the condition that ends the loop; insure it will end out doing the expected number of iterations.
> 循环结束条件是否精确？检查递增的次数、循环结束条件。

**Is the code free of unintended infinite loops?**

Check for code paths that can cause infinite loops. Make sure end loop conditions will be met unless otherwise documented.
> 代码不要陷入无穷循环。检查可能导致无穷循环的代码路径。

## 性能 ##

**Do recursive functions run within a reasonable amount of stack space?**

Recursive functions should run with a reasonable amount of stack space. Generally, it is better to code iterative functions. 
> 递归函数在合理的堆栈空间内运行，通常来说，最好用迭代函数实现而不用递归。

**Are whole objects duplicated when only references are needed?**

This happens when objects are passed by value when only references are required. This also applies to algorithms that copy a lot of memory. Consider using algorithm that minimizes the number of object duplications, reducing the data that needs to be transferred in memory. 
> 全体对象拷贝是否在需要引用时进行？尽量减少需要传送的内存数据。

**Does the code have an impact on size, speed, or memory use?**

Can it be optimized? For instance, if you use data structures with a large number of occurrences, you might want to reduce the size of the structure. 
> 代码是否影响规模、速度和内存使用？能否再优化？

**Are you using blocking system calls when performance is involved?**
 
Consider using a different thread for code making a function call that blocks.

**Is the code doing busy waits instead of using synchronization mechanisms or timer events?**

Doing busy waits takes up CPU time. It is a better practice to use synchronization mechanisms.

**Was this optimization really needed?**

Optimizations often make code harder to read and more likely to contain bugs. Such optimizations should be avoided unless a need has been identified. Has the code been profiled?

## 函数 ##

**Are function parameters explicitly verified in the code?**

This check is encouraged for functions where you don't control the whole range of values that are sent to the function. This isn't the case for helper functions, for instance. Each function should check its parameter for minimum and maximum possible values. Each pointer or reference should be checked to see if it is null. An error or an exception should occur if a parameter is invalid.

**Are arrays explicitly checked for out-of-bound indexes?**

Make sure an error message is displayed if an index is out-of-bound. Are functions returning references to objects declared on the stack? Don't return references to objects declared on the stack, return references to objects created on the heap.

**Are variables initialized before they are used?**

Make sure there are no code paths where variables are used prior to being initialized. If an object is used by more than one thread, make sure the object is not in use by another thread when you destroy it. If an object is created by doing a function call, make sure the object was created before using it. 

**Does the code re-write functionality that could be achieved by using an existing API?**

Don't reinvent the wheel. New code should use existing functionality as much as possible. Don't rewrite source code that already exists in the project. Code that is replicated in more than one function should be put in a helper function for easier maintenance.

## Bug修复 ##

**Does a fix made to a function change the behavior of caller functions?**

Sometimes code expects a function to behave incorrectly. Fixing the function can, in some cases, break the caller. If this happens, either fix the code that depends on the function, or add a comment explaining why the code can't be changed. 
> 修复Bug是否导致调用函数发生变化。

**Does the bug fix correct all the occurrences of the bug?**

If the code you're reviewing is fixing a bug, make sure it fixes all the occurrences of the bug.
>对Bug的修复是否已经改正了所有并发可能的错误。（不能引入新的Bug）

## 数学考量 ##

**Is the code doing signed/unsigned conversions? Check all signed to unsigned conversions: Can sign completion cause problems? Check all unsigned to signed conversions: Can overflow occur?**

Test with Minimum and Maximum possible values.
> 代码有否进行有符号/无符号的转换？有符号-〉无符号，符号有否出现问题？无符号-〉有符号，是否发生溢出？用最大、最小值来测试。


*上述代码评审检查项里，有一部分是可以通过Jenkins自动化构建工具完成的。*
