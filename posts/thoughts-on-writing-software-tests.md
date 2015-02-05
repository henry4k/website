<!--
.. title: Thoughts on writing software tests
.. slug: thoughts-on-writing-software-tests
.. date: 05/30/2014 08:36:47 PM UTC+02:00
.. tags: testing,programming
.. link: 
.. description: Some hints that one may find usefull when beginning with test driven development.
.. type: text
-->

Some hints that one may find usefull when beginning with test driven
development (TDD):

I'm currently learning a lot about software testing and software craftmanship
in general. To safe others from the pitfalls I fell in, I decided
to write this article. Hope it helps someone! ^^

<!-- TEASER_END -->


## Why write tests?

Why spend precious time on writing tests for code written by a natural
programmer talent like you?  Because even *if* you are expirienced or
talented, you *will* make mistakes.  -  Everyone make them and you are no
exception. That's nothing to be ashamed of.  But you should be ashamed if
you release software that is buggy - especially if you can prevent it easily.

TDD is vital for dynamic languages that have few or no ways to check code
before running it.  But even if compilers of static languages can help you
detect faults, they can't detect all of them.  And they never will.
Simply because they don't know what you want the computer to do.

That's where unit tests come into play.  They let you tell the computer how
a program should behave.  And since they require no human intervention,
they can be run automatically to prove that your software still works as it
should.


## TDD in your workflow.

For me, the following procedure has been proven as practical:

The first thing you should do when creating a new module or class, is
to lay out its interface and maybe even write documentation.
Now before you implement it, write tests that ensure the implementation
is correct. By doing that you'll also automatically notice if your
interface has any serious design design flaws.

This is called black box testing, since your tests don't know anything
about the implementation. And if you ask me, this is the best kind of unit
test you can create.  White box tests, which rely on the implementation,
have to be adapted far more often, even if the interface didn't change at all.

It's also important, that you try to keep your tests as simple as possible:
Tests that are easy to understand, make a good usage example.

Now that we've written the tests and fixed eventual usability problems,
we can start implementing the interface. While writing, you now can
run the tests anytime to check what parts of your implementation already
work.

Sure, you could put the bar even higher and e.g. follow an iterative
approach. But thats beyond the scope of this article.


## How to design a test?

To keep things clean it makes sense to divide a test into separate phases:

1. Setup phase:
   Here you initialize the environment, that is needed by the test.
2. Exercise phase:
   Run the action you want to test here.
3. Verify phase:
   Check if the action did what it should *and* did not what it shouldn't.
4. Cleanup phase:
   Free the resources used by the test environment.
   Your test framework should ensure that this phase is executed in any case.
