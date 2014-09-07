<!-- 
.. title: Writing C/C++ tests with Dummy
.. slug: writing-cc-tests-with-dummy
.. date: 05/30/2014 10:28:56 PM UTC+02:00
.. tags: testing
.. link: 
.. description: First part of the tutorial, which shows you the basics.
.. type: text
-->

## How to test C/C++ software with Prove and Dummy:

I assume that you already know a bit about testing in general and writing
testable C/C++ code.  [Referenz zu thoughts-on-writing-software-tests]

<!-- TEASER_END -->

At first you should think about the organization of your tests in your
source tree.  E.g. where are the test files placed and how should they be
named?

In general there are these two approaches:

- A dedicated test directory, which exists parallel to the source code.
  For each file in the source code directory, there is ideally has a
  counterpart in the test directory.

- Tests reside next to the source files they test and are distinguised by
  a post fix.  E.g. if the source file is called `Foo.c` the test could
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

At first we want to test the `Basename` function of the path module.

The according header looks like this:

```c
#ifndef __LIBFILESYSTEM_PATH_H__
#define __LIBFILESYSTEM_PATH_H__

// Returns the last path element.
const char* Basename( const char* path );

#endif // __LIBFILESYSTEM_PATH_H__
```

I intentionally keep quiet about the implementation here,
since we want to test whether the interface has been implemented correctly,
right?

For that we create a file called `Path.c` parallel to the source in our test
directory:

```c
#include "../src/Path.h"

void BasenameTest()
{
}

int main()
{
}
```

Since Dummys modular design, we first need to include the core functionallity
with `dummy/core.h`.  Depending on your needs, you probably need to include
some other modules.

The test results are created by a reporter.  Dummy informs reporters about
events, which in turn create some kind of of output.  Failing tests could be
printed to `stderr` for example.  Since we use `prove` in this tutorial to
run our test suite, which uses the Test Anything Protocol,
we need the `tap_reporter`.  [Referenz zu testanything.org]

To enable Dummy to detect program errors, without crashing immediately,
it uses so called sandboxes.  These allow Dummy to intercept and analyze
errors from different sources, while running a test.
In this case we use the `signal_sandbox`, which can intercept system signals
what already covers pretty much everything which can go wrong in a C
program.

```c
#include <dummy/core.h>
#include <dummy/tap_reporter.h>
#include <dummy/signal_sandbox.h>
#include "../src/Path.h"

// ...
```

New we need to initialize and configure Dummy.  At first, we'll just look at
a minimal example, so you better understand how Dummy works:

```c
int main()
{
    dummyInit(dummyGetTAPReporter(stdout));
    dummyAddTest("Basename", dummySignalSandbox, BenchmarkTest);
    return dummyRunTests();
}
```

The steps should be pretty straigt forward:  At first we initialize Dummy
with the TAP reporter.  Then we register our test: Beside the function we
also pass its name and the sandbox it should use.  The name is used by the
reporter to describe the test later on.  At the end we call `dummyRunTests`,
which runs the tests.  It returns the amount of failed tests, which can be
used as exit code.

It's now at the time to compile the program and try it out.
Tests run by prove usually end with `.t`, we'll da that here too.
In the end it should look like this:

```
> test/Path.t
1..1
ok 1 Basename
```

Nothing special - but we're happy that everything worked so perfectly.
But wait!  Maybe you remember, that we doesn't wrote the `BasenameTest` yet -
there's just an empty function!  We quickly change that:

```c
#include <string.h>

void BasenameTest()
{
    const char* result = Basename("foo/bar");
    if(strcmp(result, "bar") != 0)
        dummyAbortTest(DUMMY_FAIL_TEST, "Basename() returned a wrong result");
}
```

In sake of simplicity, we're lazy in this case and just test if `Basename`
returns the correct result for a simple input.  If you implemented `Basename`
correctly, the test should pass on its second execution too.

At the moment we have just a single module to test - so we could just
run `test/Path.t` every time to check if our code is correct.
But as soon as we have multiple modules, the whole thing won't be funny
anymore.  Thats where `prove` saves the day.  The tool automatically runs
tests in a project and originates from the Perl community.
Since it already exists for a long time, it's available for all major
operating systems.

But `prove` runs only Perl scripts by default.  We first need to adapt it to
be used with other languages.  For that we simple run `prove` with the
parameter `--exec ''` - which tells it to run all executable files.
Alternatively you can create a `.proverc` in your project or home directory,
which contains `--exec ''`.

On my machine it looks like that then:

```
> prove
./test/Path.t .. ok
All tests successful.
Files=1, Tests=1,  0 wallclock secs ( 0.03 usr  0.00 sys +  0.00 cusr  0.04 csys =  0.07 CPU)
Result: PASS
```

With that we've completed the first part of the tutorial.
In the next part I'll explain, how you write nicer tests with Dummy - till
now it was just about the basic API and concepts.
