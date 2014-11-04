<!-- 
.. title: Code readability
.. slug: code-readability
.. date: 05/30/2014 10:23:08 PM UTC+02:00 # <- THE DATE IS WRONG MAN!
.. tags: programming,draft
.. link: 
.. description: 
.. type: text
-->

<!-- TEASER_END -->

As we all know, code is being read much more often than being written.
Therefore it's surely a good investion, to write easily readable code ..
which in turn is also easily understable code.
Here are some practices that may help you with this task:


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


Intelligentes Einruecken
------------------------

Bzw wie man durch gutes Einreucken lesbareren code schreiben kann.

Dass sich hier ein Mehraufwand durchaus lohnen kann, besagt ja schon die Regel,
dass Code deutlich oefter gelesen als geschrieben wird.

```c
glm::perspective(camera->fieldOfView, camera->aspect, camera->zNear, camera->zFar);
```

```c
glm::perspective(camera->fieldOfView,
                 camera->aspect,
                 camera->zNear,
                 camera->zFar);
```

Das Auge kann sich wiederholende Muster schnell erkennen.
Wenn wir uns das zunutze machen, koennen wir Code schreiben den man schneller
lesen und dadurch schneller/leichter verstehen kann.

Zudem lassen sich an solchen Stellen die Block-Edit-Modis mancher Editoren
gut einsetzen.

TODO: Ich koennte noch ein paar Bilder erstellen welche die Augenbewegung beim
lesen von gut und schlecht formatiertem Code darstellt.
