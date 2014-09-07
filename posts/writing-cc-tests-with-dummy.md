<!-- 
.. title: Writing C/C++ tests with Dummy
.. slug: writing-cc-tests-with-dummy
.. date: 05/30/2014 10:28:56 PM UTC+02:00
.. tags: testing,draft
.. link: 
.. description: A tutorial on how to write C++ tests using Dummy.
.. type: text
-->

How to test C/C++ software with prove and Dummy:

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

Dummy ist modular aufgebaut, daher müssen wir zum einen
die Kernfunktionalität mit `dummy/core.h` einbinden.
Und dann müssen, je nach Anforderung, noch weitere Module inkludiert
werden.

Die Test-Ergebnisse werden durch einen Reporter ausgegeben.
Reporter werden über Ereignisse von Dummy informiert und erzeugen
ihrerseits irgendeine Art von Ausgabe.  Zum Beispiel könnten fehlschlagende
Tests über `stdout` gemeldet werden.  Da wir in diesem Tutorial unseren Test
von `prove` ausführen lassen, benötigen wir den `tap_reporter` weil `prove`
`TAP` als Format erwartet.  [Referenz zu testanything.org]

Damit Dummy Fehler im Programm erkennen kann, ohne gleich komplett
abzustürzen, verwendet es sogenannte Sandboxen.
Diese ermöglichen es Dummy beim ausführen eines Tests, Fehler aus
unterschiedlichen Quellen abzufangen und auszuwerten.
In diesem Fall benutzen wir die `signal_sandbox`, welche Signale abfängt
und damit schon so ziemlich alles abdeckt, was in einem C Programm so
schiefgehen kann.

```c
#include <dummy/core.h>
#include <dummy/tap_reporter.h>
#include <dummy/signal_sandbox.h>
#include "../src/Path.h"

// ...
```

Jetzt geht es daran Dummy zu initialisieren und zu konfigurieren.
Wir werden uns zuerst einmal ein Minimalbeispiel ansehen,
damit du besser verstehst wie Dummy funktioniert:

```c
int main()
{
    dummyInit(dummyGetTAPReporter(stdout));
    dummyAddTest("Basename", dummySignalSandbox, BenchmarkTest);
    return dummyRunTests();
}
```

Die Schritte dürften recht logisch sein: Zuerst initialisieren wir Dummy
mit dem TAP Reporter.  Dann registrieren wir unseren Test, dabei geben wir
neben der eigentlichen Testfunktion einen Namen und die Sandbox an.
Der Name wird vom Reporter benutzt um den Test besser beschreiben zu können.
Und zum Schluss werden die Tests ausgeführt, `dummyRunTests` gibt die
Anzahl der fehlgeschlagenen Tests zurück - praktischerweise kann der Wert
direkt als Exit-Code benutzt werden.

Es ist jetzt an der Zeit das Programm zu kompilieren und das ganze mal
auszuprobieren.  Bei `prove` ist es übrigens üblich die Tests auf `.t`
enden zu lassen, das werde ich hier auch so halten.
Am Ende sollte es dann etwa so aussehen:

```
> test/Path.t
1..1
ok 1 Basename
```

Nichts großartiges, wir freuen uns dass alles so perfekt funktioniert hat.
Doch warte, vielleicht erinnerst du dich, dass wir den `BasenameTest` noch
überhaupt nicht geschrieben haben - da ist nur eine leere Funktion!
Das ändern wir doch schnell:

```c
#include <string.h>

void BasenameTest()
{
    const char* result = Basename("foo/bar");
    if(strcmp(result, "bar") != 0)
        dummyAbortTest(DUMMY_FAIL_TEST, "Basename() returned a wrong result");
}
```

Um der Anschaulichkeit willen, machen wir uns es in diesem Fall einfach und
testen ledeglich, ob `Basename` für eine simple Eingabe das korrekte
Ergebnis liefert.  Je nachdem ob `Basename` nun korrekt implementiert wurde
wird unser Test auch bei der zweiten Ausführung erfolgreich durchlaufen.

Aktuell haben wir nur ein Modul zu testen - wir könnten also einfach
jedes mal `test/Path.t` ausführen um zu gucken ob es richtig funktioniert.
Sobald wir aber mehrere Module haben, macht das ganze keinen Spaß mehr.
Dazu gibt es `prove`.  Das Tool kommt aus der Perl-Community und existiert
bereits seit geraumer Zeit.  Daher ist es für alle wichtigen Betriebssysteme
verfügbar.  `prove` führt nach bestimmten Regeln alle Tests eines Projektes
aus.

Ein kleines Manko gibt es allerdings: `prove` führt standartmäßig nur
Perl-Scripte aus.  Wir müssen es erst für die Verwendung mit anderen Sprachen
konfigurieren.  Dazu rufen wir `prove` einfach mit dem Parameter `--exec ''`
auf.  Alternativ kann man sich im Projekt- oder im Home-Verzeichnis auch eine
`.proverc` anlegen, welche `--exec ''` enthält.

Bei mir schaut das Ergebnis dann so aus:

```
> prove
./test/Path.t .. ok
All tests successful.
Files=1, Tests=1,  0 wallclock secs ( 0.03 usr  0.00 sys +  0.00 cusr  0.04 csys =  0.07 CPU)
Result: PASS
```

Damit haben wir den ersten Teil des Tutorials abgeschlossen.
Im nächsten Teil erkläre ich dir, wie du mit Dummy schönere Tests schreiben
kannst - bisher ging es erstmal nur ums Verständnis der API.
