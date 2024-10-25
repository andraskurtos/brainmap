---
aliases: 
tags:
  - 5semester
  - nlp
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-25 18:00
---

# Hangulatelemzés

A hangulatelemzés egy [[Statisztikai nyelvi modellek|statisztikai nyelvi modell]], melyben szövegekról próbáljuk megállapítani szókészletük alapján, hogy milyen hangulati kategóriába sorolhatók. A lenti példában egy egyszerű megvalósítást specifikálunk.

>[!question] Feladat
>- Állapítsuk meg, hogy a felhasználónak tetszett-e vagy sem
 egy adott app!
 >- Határozzuk meg, mennyire volt pontos az értékelésünk!
 
 A módszer a következő lesz: először **meghatározzuk a pozitív és negatív értékelésekre jellemző szavakat** (pozitív: "good", "great", "recommended", stb; negatív: "bad", "useless", "annoying", stb). Ezek után megnézzük, egy-egy véleményben melyik csoport jellemző, majd az app értékelése alapján ellenőrizzük a klasszifikációnk. Ha az appot \* vagy \*\*-ra értékelték, feltételezzük, hogy negatív az értékelés, ha \*\*\*\* vagy \*\*\*\*\*, akkor, hogy pozitív. A valódi és a jósolt kategóriákat egy [[Tévesztési mátrix|tévesztési mátrix]]ban vetjük össze.