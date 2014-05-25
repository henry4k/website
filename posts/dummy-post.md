<!-- 
.. title: Dummy Post
.. slug: dummy-post
.. date: 05/16/2014 11:51:28 PM UTC+02:00
.. tags: programming
.. link: 
.. description: DOs und DONTs beim Programmieren
.. type: text
-->

## Allgemeines

### Zeilenumbrüche

Hat nichtmal was mit Programmierung zu tun.
Man kann Sätze nicht einfach irgendwo umbrechen,
denn das erschwert die Lesbarkeit.

- Vor Präpositionen (mit, in, von, auf, vor, am, ...)
> Präpositionen sollten immer  
> *am* Anfang der nächsten Zeilen sein.

- Vor Konjunktionen (und, oder, aber, ...)
> Niemand .. Ich meine niemand, sollte diese Regel brechen  
> *oder* missachten.

- Vor Artikeln (der, die, das, ein, eine, einer, ...)
> Man soll den Tag nicht vor  
> dem Abend loben.

<!-- TEASER_END -->


### Code Craft

Dinge, die mir beim lesen von 'Code Craft' besonders wichtig erschienen:


#### Die Einstellung zählt

Wie bei vielen Dingen im Leben geht es auch hier zuallererst
um die innere Einstellung zum Thema. Wenn man nicht wirklich schönen/guten
Code schreiben *will*, wird man auch keinen hervorbringen.


#### Programmiere defensiv

Defensiv programmieren bedeutet keine Ahnnahmen
über die Nutzung seines Codes zu machen.

- Was mache ich, wenn Parameter X ausserhalb von 0-1 ist?


Lösungen:

- Variablen immer initialisieren.
- Unterstützende Sprachfeatures wie `const` oder C++-Casts immer nutzen.
- Return-Werte und Fehler-Codes prüfen.

Integritäts Tests:

    int div( int a, int b )
    {
        assert(b > 0); // precondition => callers fault
        const r = a / b;
        assert(r > 0); // postcondition => our fault
    }

- Array-Indizes prüfen
- Pointer prüfen
- Funktionsargumente prüfen
- Funktionsergebnisse prüfen
