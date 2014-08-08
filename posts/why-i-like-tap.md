<!-- 
.. title: Why I like TAP
.. slug: why-i-like-tap
.. date: 05/30/2014 08:53:39 PM UTC+02:00
.. tags: testing
.. link: 
.. description: Test Anything Protocol could and should be used by way more people.
.. type: text
-->

- language agnostic
- very simple structure => easy to generate and easy to parse
- remember TDD rule #1: write tests
  and the easier it gets to write and execute tests -
  the more code is being tested!

- `echo **/*.t`
- `prove`
- `prove --state`


Most people use the default result format that their test library provides.

This is often either an JUnit compatible XML or a proprietary format.

Proprietary formats are obviously bad.
While they're maybe easy to read by humans, machines can't parse them that easy.
Simply because they're proprietary and though just have a small eco system at best.
And since you're doing TDD, you *want* the results to be automatically parsed, analyzed and whatnot.

JUnit XML is a better alternative.

ever tried to
is a pain without apropriate libraries

It has also the downside that
- it generates *a lot* of XML noise
- and if your 

So I wan't to introduce you to TAP.
While no being wildly used, it has a large eco system.



Just to conclude: A useful format for test results should have these properties:

- simple format: easily parsable by machines
- eco system: producers, parsers, etc. so you can make use of your test results
- generic: should not be tailored to a specific language
