---
aliases: 
tags:
  - 3semester
  - valszam
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 11:01
---
# Függetlenség
##### Függetlenség definíció

>[!summary]+ Definíció:
>Két valószínűségi változó független, ha
> $\forall x,y: P(x,y)= P(x)P(y)$, azaz az együttes eloszlás két egyszerűbb eloszlás szorzata. Alternatív feltétel: $\forall x,y: P(x|y)=P(x)$

A valószínűség szubjektív értelmezése esetén a függetlenség információs irrelevanciát jelent: Y ismerete nem változtatja meg X lehetséges értékeiről a vélekedést.

A függetlenségek az eloszlásokat kvalitatívan jellemzik, adott függetlenségi relációk eloszlások sokaságában teljesülnek.

**A függetlenség ritka, és sokat ér, mert nagyban lecsökkenti a szabad paraméterek számát**

### Feltételes függetlenség

##### feltételes függetlenség definíció

>[!summary]+ Definíció
>A *feltételes függetlenség* a függetlenség leggyakoribb formája. X *feltételesen független* Y-tól adott/ismert Z esetén, ha:
> $\forall x,y,z: P(x,y|z)=P(x|z)P(y|z)$, avagy
> $\forall x,y,z: P(x|z,y)=P(x|z)$

>[!note]+ Jelölése:
> $I_{p}(X;Y|Z)$
> függésé: $D_{p}(X;Y|Z)=\urcorner I_{p}(X;Y|Z)$

Ha teljesül, ugyanúgy kevesebb paraméter.
A függetlenségek egyenletrendszerek megoldásai.

### Kontextuális függetlenségek

A feltételes függetlenség egy univerzálisan kvantált állítás. Lehetséges azonban, hogy csak bizonyos $f_{0}$ kontextus esetén áll fenn a függetlenség. Pl ha nincs füst, akkor a tűz maga is aktiválhatja a riasztót.

### Intranzitív függés

Lehet-e függések láncolata független?
Lehet-e releváns információk láncolatából álló magyarázat irreleváns?
Lehet-e oksági mechanizmusok láncolata teljesen hatástalan?

--> **LEHETSÉGES**, de ritka

### Szinergia

Létrejöhet e függetlenségekből függés?
Lehet-e irreleváns információk együttese releváns?
Lehetnek-e okok önmagukban hatástalanok?


## Függőségi modellek

Egy bizonytalan probléma strukturális jellegzetességei leírhatók függetlenségekkel. 

>[!summary]+ Definíció
>Egy P együttes valószínűségi eloszlás *függőségi modellje* a P-ben érvényes feltételes függetlenségek halmaza:
> $M_{p}=\{I_{p,1}(X_{1};Y_{1}|Z_{1}),\dots,I_{p,k}(X_{k};Y_{k}|Z_{k})\}$

Egy adott eloszlásban érvényes feltételes függetlenségek egy rendszert alkotnak logikai szabályszerűségekkel.
Lehet-e egy helyes és teljes logikai kalkulus függetlenségekre?
Lehet-e gráfokat használni függetlenségek egzakt reprezentálására?

Általában igen, de nem mindig.



