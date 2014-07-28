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

Wie man mit Prove und Dummy C und C++ Programme testet:

Ich gehe davon aus, dass du schon weisst was es mit dem Testen auf sich hat
und wie man gut testbaren C/C++ Code schreibt.
[Referenz zu thoughts-on-writing-software-tests]

Zuerst sollte man sich Gedanken über die Organisation der Tests im Dateisystem machen.
D.h. wo liegen die Tests im Projektverzeichnis und wie sind sie benannt?

Im Grunde gibt es folgende zwei Varianten:

- Test-Verzeichnis, welches parallel zum Source-Code-Verzeichnis existiert.
  Für jede Datei im Source-Code-Verzeichnis gibt es idealerweise ein pendant im Test-Verzeichnis.

- Tests liegen neben den Modulen, welche sie testen.
  Neben der Modul `Foo.c` gibt es einen Test namens `FooTest.c`.

Beide Varianten haben ihre vor und Nachteile, im Grunde würde ich aber die erste empfehlen.
Bei der Zweiten spart man sich zwar Tipparbeit wenn man zwischen Modul und Test wechseln möchte,
aber benötigt auch ein mächtigeres Build-System, was Tests und Module auseinanderhalten kann.

Unser Beispiel-Projekt schaut also so aus:


Als Beispiel dient eine Bibliothek, welche den Dateisystemzugriff abstrahiert.

    libfilesystem/
        src/
            Path.h
            Path.c
        test/

Anmerkung:
Ja ich weiss, dass `prove` standartmäßig `t` als Testordner verwendet.
Aber ich finde solche Abkürzungen nicht mehr Zeitgemäß.
Womit ich jetzt aber keineswegs den Wert von Prove herrunterspielen will. :)


## Einen Test anlegen

Als erstes wollen wir die `basename` Funktion aus dem Path-Modul testen.

Der entsprechende Header schaut so aus:

```c
#ifndef __LIBFILESYSTEM_PATH_H__
#define __LIBFILESYSTEM_PATH_H__

//Returns the last path element.
const char* basename( const char* path );

#endif // __LIBFILESYSTEM_PATH_H__
```

Die Implementation verschweige ich hier absichtlich,
denn wir wollen testen, ob die Implementation dem Interface entspricht.

Dazu legen wir im test-Verzeichnis, parallel zum src-Verzeichnis, `Path.c` an:

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

