<!-- 
.. title: Thoughts on writing software tests
.. slug: thoughts-on-writing-software-tests
.. date: 05/30/2014 08:36:47 PM UTC+02:00
.. tags: testing,programming
.. link: 
.. description: 
.. type: text
-->

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
