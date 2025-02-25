---
aliases: 
tags:
  - ai
  - mi
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 21:27
---


# Tanulás minták alapján

>[!question] Felvetés
>Gyakran a számítógép tanulásához rendelkezésre áll egy csomó mintapélda, és ez hordozza az információt.
>Nincsenek előre kialakított szabályok, csak a minták.
>Persze ilyenkor is valamilyen struktúrában igyekszünk felhasználni a minták hordozta tudást.

--> a módszer tehát **problémafüggetlen**
hétköznapi életben nagyon gyakori a minta alapján való tanulás, klasszikus számítógépes megoldásokban azonban meghatározott szabályokat programozunk be (analitikus)

## Tanulás vs Analitikus tervezés

Egy algoritmust alapvetően kétféle módon lehet megvalósítani:

1. Analitikus tervezés
	- Begyűjteni az analitikus modelleket az adott problémára
	- Megtervezni analitikusan a konkrét mechanizmust, és ezt algoritmusként implementálni
2. Tanulás:
	- Megtervezni a tanulás mechanizmusát, és azt algoritmusként implementálni
	- Majd az alkalmazásával megtanulni vele a minták alapján a döntés mechanizmusát, és azt algoritmusként implementálni.

## Gépi tanulás

>[!summary]+ Főbb fajtái:
>- **felügyelt tanulás**: egy esetnél mind a bemenetet, mind a kimenetet észlelni tudjuk (bemeneti minta + kívánt válasz)
>- **megerősítéses tanulás**: az [[Ágens]] az általa végrehajtott tevékenység csak bizonyos értékelését kapja meg, nem is minden lépésben (jutalom, büntetés, megerősítés)
>- **felügyelet nélküli tanulás**: semmilyen információ sem áll rendelkezésünkre a helyes kimenetelről (az észlelések közötti összefüggések tanulása)
>- **féligellenőrzött tanulás**: a tanításra használt esetek egy részénél mind a bemenetet, mind a kimenetet észlelni tudjuk (bemeneti minta+kívánt válasz), a másik, tipikusan nagyobb részénél csak a bemeneti leírás ismert.

### Felügyelt tanulás

tanulási példa: (x,F(x)) adatpár, ahol f(x) ismeretlen
tanulás célja: f(x) értelmes közelítése egy h(x) hipotézissel

h(x) = f(x), x - ismert példákon gyakran teljes pontosság
h(x') $\approx$ f(x') x' - a tanulás közben nem látott esetek (**általánosító képesség**)

Sokféle tanítható modell van:
- [[Neurális Hálók]]
- Bayes-hálók
- kernelgépek
- [[Döntési fák]]
- legközelebbi szomszéd oszályok