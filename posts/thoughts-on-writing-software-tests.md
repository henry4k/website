<!-- 
.. title: Thoughts on writing software tests
.. slug: thoughts-on-writing-software-tests
.. date: 05/30/2014 08:36:47 PM UTC+02:00
.. tags: testing,programming
.. link: 
.. description: 
.. type: text
-->

Some hints that one may find usefull when beginning with TDD.

I'm currently learning a lot about software testing
and software crafmaship in general.
And to safe others from the pitfalls I fell in, I decided to write this article.
Hope it helps someone! ^^


## Why write tests?

Why spend precious time on writing tests
for code written by a natural programmer talent like you?

Because even *if* you are expirienced or talented,
you *will* make mistakes. - Everyone make them and you are no exception.

That nothing to be ashamed of.

But you should be ashamed if you release software that is buggy -
especially if you can prevent it without ...

TDD is viral for dynamic languages that have few or no ways to check code before running it.
But it is also practical for stricter (?) languages where compilers help you prevent bugs.
^- Satz andersrum





## 

Test-Driven-Development workflow:

1. Design the interface
2. Write tests for that interface => black box testing
3. Implement the interface
4. Run tests


How to design a test:

Separate a test into several test phases:

1. setup
2. exercise
3. verify
4. cleanup

Each phase should be easily distinguishable from the other ones.

Tailor your project for testability.
This is especially an issue for languages like C/C++,
since programmers tend to use global variables there. (Nah)

...


I'll try to keep this post updated as I learn new things on this topic.
