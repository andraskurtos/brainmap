---
created: 20240901 23:53
tags:
  - 5semester
  - ai
  - mi
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# MOHÓ [[Keresés|KERESÉS]]

A legközelebbi csomópont kifejtése lenne célszerű -> mi mehet félre?

Fejtsük ki azt a csomópontot, *amiről azt gondoljuk, legközelebb van a célállapothoz* ([[Heurisztikus keresés|heurisztika]] alapján).

Gyakori esetben a mohó keresés eljuttat egy célállapothoz, ami nem biztos, hogy a legjobb. Legrosszabb esetben azonban egy rosszul irányított mélységi keresést kapunk.

Minden lépésben azt a csomópontot fejti ki, amelyhez rendelt probléma-állapotot a legközelebbinek ítéli a célállapothoz.

![[Pasted image 20240909182312.png]]![[Pasted image 20240909182319.png]]

A mohó algoritmus általában *nagyon gyorsan megtalálja* a megoldást, de *nem mindig az optimálisat*. A mohó keresés érzékeny a hibás kezdő lépésekre is. Hasonlít a [[Mélységi keresés|mélységi kereséshez]], egyetlen út végigkövetését preferálja a célig, zsákutcákból visszalép. Ugyanazokkal a problémákkal is rendelkezik: *nem optimális* és *nem teljes*. Az összes csomópontot a memóriában tartja, ezért a worst-case idő- és tárigény:

- $O(b^m)$ ha $m$ a problématér mélysége



