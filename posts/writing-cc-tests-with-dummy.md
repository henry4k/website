<!-- 
.. title: Writing C/C++ tests with dummy
.. slug: writing-cc-tests-with-dummy
.. date: 05/30/2014 10:28:56 PM UTC+02:00
.. tags: testing
.. link: 
.. description: A tutorial on how to write C++ tests using dummy.
.. type: text
-->

A tutorial on how to write C++ tests using dummy.
Also shows how to face common problems that arise when you try to test C++ code.

- Dummy boilerplate (Init, AddTest, RunTests)
  - Reporter-Initialization
- Sandboxes (RunInSandbox, AbortSandbox, ...)
  - Practical examples of nested sandboxes
- Require-, BDD-, Inline-Examples

```cpp
InlineTest("Identical boxes overlap.", dummySignalSandbox)
{
    Box box;
    box.position = vec3(0,0,0);
    box.halfWidth = vec3(1,1,1);
    box.velocity = vec3(0,0,0);

    Require(TestAabbOverlap(box, box) == true);

    vec3 penetration(9,9,9);
    Require(TestAabbOverlap(box, box, &penetration) == true);
    Require(penetration == vec3(2,0,0));
}
```

----

How to test C/C++ software with prove and dummy:

I assume that you already know a bit about testing in general and writing
testable C/C++ code.  [Referenz zu thoughts-on-writing-software-tests]

At first you should think about the organization of your tests in your
source tree. E.g. where are the test files placed and how should they be
named?

In general there are these two approaches:

- A dedicated test directory, which exists parallel to the source code.
  For each file in the source code directory, there is ideally has a
  counterpart in the test directory.

- Tests reside next to the source files they test and are distinguised by
  a post fix. E.g. if the source file is called `Foo.c` the test could
  be called `FooTest.c`.

Both variants have their advantages and disadvantages, but if your build
system is not that mighty you should stick to the first one.
The second one has just the advantage that it saves you some time when
switching between module and test.

So our example project looks like this:


Our example is a library, which abstracts file system access.

    libfilesystem/
        src/
            Path.h
            Path.c
        test/

Note:
Yeah, I know that `prove` uses `t` as default test directory.
But I think that such abbreviations are not needed anymore.
Which shall not mean that prove isn't an incredibly powerful tool! :)


## Creating a test

At first we want to test the `basename` function of the path module.

The according header looks like this:

```c
#ifndef __LIBFILESYSTEM_PATH_H__
#define __LIBFILESYSTEM_PATH_H__

// Returns the last path element.
const char* basename( const char* path );

#endif // __LIBFILESYSTEM_PATH_H__
```

I intentionally keep quiet about the implementation here,
since we want to test whether the interface has been implemented correctly,
right?


For that we create a file called `Path.c` parallel to the source in our test
directory:

```c
#include <dummy/core.h>
#include <dummy/signal_sandbox.h>

#include "../src/Path.h"

int main()
{

}
```

`Path.h`
## Prove ausführen
[Referenz zu why-i-like-tap]
## Schönere Tests schreiben
=> inline
=> BDD
## Komplexere Tests schreiben
=> extra/gen_stubs.py

