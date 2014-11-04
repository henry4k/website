<!-- 
.. title: Voxel Manager
.. slug: vman
.. date: 05/30/2014 10:53:19 PM UTC+02:00
.. tags: vman
.. link: 
.. description: 
.. type: text
.. image: /todo.png
-->

VMan shall become an API for voxel management,
therefore I simply called it *V*oxel *Man*ager.

You may ask why one would bother writing such a thing,
if you could just use 3D arrays:
As soon as your voxel volumes grow huge,
like when you're creating procedural landscapes,
a more sophisticated storage solution than naive arrays will pay off.

...

There can be many implementations
like 

Da es sich um eine reine API Spezifikation handelt,
kann es mehrere Implementationen geben.
Dadurch kann für jeden Anwendungsfall, die optimale Implementation
verwendet werden und bestehender Code muss nicht speziell angepasst werden.

Beispiele für Implementationen:
- Volumen wird in Chunks eingeteilt
  - Erlaubt unendlich große Landschaften
- Volumen wird als großer Array behandelt
  - sehr schneller Zugriff
  - macht für kleinere begrenzte Volumen Sinn
- Client/Server Architektur
  - macht Sinn bei größeren Systemen, wo mehrere Anwendungen auf ein Volumen zugreifen
