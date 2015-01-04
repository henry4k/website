<!-- 
.. title: Code readability
.. slug: code-readability
.. date: 05/30/2014 10:23:08 PM UTC+02:00 # <- THE DATE IS WRONG MAN!
.. tags: programming,draft
.. link: 
.. description: 
.. type: text
-->

As we all know, code is being read much more often than being written.
Therefore it's surely a good investion, to write easily readable code ..
which in turn is also easily understable code.
Here are some practices that may help you with this task:

<!-- TEASER_END -->


Carefully placed empty lines
----------------------------

Like with regular text, it makes sense to use divide the code into paragraphs.

Codezeilen, welche eine gemeinsame Aufgabe erledigen, lassen sich als Absatz
deutlich vom restlichen code abheben.

Lines which serve a particular purpose, can be written as a paragraph to
distinguish them from other lines which have another purpose.
Okay this may sound abstract - lets take a look at this code:

```lua
function table.shuffle( values )
    local weightedValues = {}
    for i, value in ipairs(values) do
        local weight = math.random()
        weightedValues[i] = { value=value, weight=weight }
    end
    table.sort(weightedValues, function( a, b )
        return a.weight < b.weight
    end)
    for i, weightedValue in ipairs(weightedValues) do
        values[i] = weightedValue.value
    end
end
```

You will agree that its arguably harder to read than this version,
which only contains two more lines:
*TODO: Arguably*

```lua
function table.shuffle( values )
    local weightedValues = {}
    for i, value in ipairs(values) do
        local weight = math.random()
        weightedValues[i] = { value=value, weight=weight }
    end

    table.sort(weightedValues, function( a, b )
        return a.weight < b.weight
    end)

    for i, weightedValue in ipairs(weightedValues) do
        values[i] = weightedValue.value
    end
end
```

Just don't overdo it!  Don't chop your code into tiny paragraphs, this will
make it worse than without any paragraphs.


Smart Indentation
-----------------

We can recognize repeating patterns quite fast.  If we take advantage of that,
we're able to write code which can be read faster and therefore can be
understood easier.

Lets take a look at the following to code segments:

```c
glm::perspective(camera->fieldOfView, camera->aspect, camera->zNear, camera->zFar);
```

```c
glm::perspective(camera->fieldOfView,
                 camera->aspect,
                 camera->zNear,
                 camera->zFar);
```

You'll see, that it takes longer to get the gist of the first segment, than
of the second one - even though the first segment is technically shorter.

Writing code, which is indented like this, takes longer than just writing
it in a single line - but as code is being read far more often than being
written is worth the effort.
*OR: pays of in the end.*

Some editors feature a so called block edit mode, which makes editing such
code parts much easier.  It allows you to select a two dimensional 'block'
of text and modify it.  As far as I know, every better code editor can do
this.
