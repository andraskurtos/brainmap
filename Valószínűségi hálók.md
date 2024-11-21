---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 11:01
---

# Valószínűségi hálók

## Függetlenség
##### Függetlenség definíció

>[!summary]+ Definíció:
>Két valószínűségi változó független, ha
> $\forall x,y: P(x,y)= P(x)P(y)$, azaz az együttes eloszlás két egyszerűbb eloszlás szorzata. Alternatív feltétel: $\forall x,y: P(x|y)=P(x)$

A valószínűség szubjektív értelmezése esetén a függetlenség információs irrelevanciát jelent: Y ismerete nem változtatja meg X lehetséges értékeiről a vélekedést.

A függetlenségek az eloszlásokat kvalitatívan jellemzik, adott függetlenségi relációk eloszlások sokaságában teljesülnek.

**A függetlenség ritka, és sokat ér, mert nagyban lecsökkenti a szabad paraméterek számát**

##### Feltételes függetlenség

>[!summary]+ Definíció
>A *feltételes függetlenség* a függetlenség leggyakoribb formája. X *feltételesen független* Y-tól adott/ismert Z esetén, ha:
> $\forall x,y,z: P(x,y|z)=P(x|z)P(y|z)$, avagy
> $\forall x,y,z: P(x|z,y)=P(x|z)$

>[!note]+ Jelölése:
> $I_{p}(X;Y|Z)$
> függésé: $D_{p}(X;Y|Z)=\urcorner I_{p}(X;Y|Z)$

Ha teljesül, ugyanúgy kevesebb paraméter.


A függetlenségek egyenletrendszerek megoldásai