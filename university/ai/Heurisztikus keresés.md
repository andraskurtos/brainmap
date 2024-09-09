---
created: 20240901 23:53
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---
# HEURISZTIKUS KERESÉS
## Keresési heurisztika

A *heurisztika* egy függvény, ami becslést ad arra, hogy az adott állapot mennyire van közel a célállapothoz. Egy adott keresési problémához kell tervezni. Ilyen például a Manhattan távolság, vagy útvonalaknál az Euklidészi távolság.

---

## Heurisztikus [[Keresés|keresés]]

Ötlet: *büntessük a hátra (céltól elfele) keresést!*
De ugyanazt éri el, és könnyebben megvalósítható *díjazni az előrekeresést célirányba*.
Ehhez szükségünk van egy elképzelésre, hogy a cél merrefelé és nagyjából milyen messzire fekszik. Ez az információ az úgynevezett *heurisztikus függvény*.

---

## Heurisztikus függvény

A probléma minden *n* állapotára ki kell tudnunk számolni. Kifejezi *n*-re a célig előrehaladás becsült költségét. Ha pontos, akkor elvben fölöslegessé teszi a keresést, ha nagyon pontatlan, akkor viszont semmit sem segít. *Minden problémára más, problémaspecifikus!*.

--> [[mohó keresés]]

