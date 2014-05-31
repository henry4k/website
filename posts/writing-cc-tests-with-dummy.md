<!-- 
.. title: Writing C/C++ tests with dummy
.. slug: writing-cc-tests-with-dummy
.. date: 05/30/2014 10:28:56 PM UTC+02:00
.. tags: testing
.. link: 
.. description: 
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
