<!-- 
.. title: Code readability
.. slug: code-readability
.. date: 01/08/2015 10:23:08 PM UTC+02:00
.. tags: programming
.. link: 
.. description: Hints wich help writing code thats easy and fast to read.
.. type: text
-->

As we all know, code is being read much more often than being written.
Therefore it's surely good invested time, to write easily readable code -
which in turn is also easily understandable code.
Here are some practices that may help you with this task:

<!-- TEASER_END -->


Carefully placed empty lines
----------------------------

Like with regular text, it makes sense to divide the code into paragraphs.

Lines which serve a particular purpose, can be written as a paragraph to
distinguish them from lines which have another purpose.
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

You will agree that its harder to read than the following version,
which only contains two more lines:

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

```cpp
glm::perspective(camera->fieldOfView, camera->aspect, camera->zNear, camera->zFar);
```

```cpp
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

Some editors feature a so called block edit mode, which makes editing such
code parts much easier.  It allows you to select a two dimensional 'block'
of text and modify it.  As far as I know, every better code editor can do
this.
