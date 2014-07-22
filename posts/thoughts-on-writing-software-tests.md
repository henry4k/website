<!--
.. title: Thoughts on writing software tests
.. slug: thoughts-on-writing-software-tests
.. date: 05/30/2014 08:36:47 PM UTC+02:00
.. tags: testing,programming,draft
.. link: 
.. description: 
.. type: text
-->

Some hints that one may find usefull when beginning with TDD.

I'm currently learning a lot about software testing and software crafmaship
in general. To safe others from the pitfalls I fell in, I decided
to write this article. Hope it helps someone! ^^


## Why write tests?

Why spend precious time on writing tests
for code written by a natural programmer talent like you?

Because even *if* you are expirienced or talented,
you *will* make mistakes. - Everyone make them and you are no exception.

That's nothing to be ashamed of.

But you should be ashamed if you release software that is buggy -
especially if you can prevent it easily.

TDD is viral for dynamic languages that have few or no ways to check code before running it.
But even if compilers of static languages can help you detect faults,  
they can't detect all of them.
And they never will.
Simply because they don't know what you want the computer to do.

That's where unit tests come into play.
They let you tell the computer how a program should behave.

Since they require no human intervention,
they can be run automatically to prove that your software still works like it should.

Which leads us to the first and most important rule of TDD:
1. Write tests.


## TDD im Alltag

Für mich hat es sich als sinnvoll erwiesen folgendermaßen Vorzugehen:

Für jede neue Klasse bzw jedes neue Modul, legst du zuerst den Interface
fest. Statt diesen dann gleich zu Implementieren, schreibst du zuerst
Tests, welche sicherstellen, dass der Interface auch so funktioniert
wie er soll. Damit siehst du auch gleich, ob dein Interface sich
als praktikabel erweist.

Dies nennt man auch Black-Box testing, weil man nicht die Implementation,
sondern den Interface eines Modules testet.
*TODO: Stimmt das?*

Wichtig ist ausserdem, dass Tests immer möglichst simpel gehalten werden,
denn Tests geben so auch eine Dokumentation bzw. ein Nutzungsbeispiel ab.

Nachdem die Tests nun geschrieben und etwaige Usabillity Probleme behoben
wurden, kann der Interface implementiert werden.
Währenddessen kannst du nun jederzeit die Tests laufen lassen und so
prüfen wie weit deine Interface schon ordnungsgemäß funktioniert.

Man kann das ganze natürlich noch detaillierter Betreiben,
zum Beispiel indem man einen Iterativen-Ansatz verfolgt.
Das heisst, das die [...]


## How to design a test:

Der Übersicht halber macht es Sinn einen Test in verschiedene Phasen
zu unterteilen:

1. Setup-Phase:
   Die für den Test benötigte Umgebung, wird hier initialisiert.
2. Exercise-Phase:
   Die zu testende Aktion wird durchgeführt.
3. Verify-Phase:
   Prüfen, ob die ausgeführte Aktion das gewünschte Resultat erbracht hat.
4. Cleanup-Phase:
   Die von der Testumgebung belegten Ressourcen werden hier wieder
   freigegeben. Das Test-Framework sollte sicherstellen, dass diese Phase
   in jedem Fall ausgeführt wird!

Each phase should be easily distinguishable from the other ones.

Tailor your project for testability.
This is especially an issue for languages like C/C++,
since programmers tend to use global variables there.
