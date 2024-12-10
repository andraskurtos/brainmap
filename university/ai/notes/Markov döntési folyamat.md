---
aliases: 
tags:
  - ai
  - mi
  - uni
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 14:20
---
# Markov döntési folyamat

Szekvenciális döntési probléma - jutalmakkal

Leszámítolás:
- ésszerű a jutalmak maximalizására törekedni
- Ésszerű a jelenlegi jutalmat preferálni a jövőbeli jutalom helyett
- Egy megoldás: a jutalmak értékei exponenciálisan csökkennek

>[!summary]+ Szekvenciális jutalmak hasznossága
>Leszámítolt jutalmak, egy végtelen sorozat hasznossága:
> $U_{h}([s_{0},s_{1},s_{2},\dots])=\sum_{t=0\dots \infty}\gamma^tR(s_{t})\leq \sum_{t=0\dots \infty}\gamma^tR_{max}=\frac{R_{max}}{1-\gamma}$

Alternatív esetek:
1. Ha van végállapot, és ha garantált, hogy az [[Ágens]] végül belekerül, akkor nincs szükség vételen sorozatok összehasonlítására
       Egy eljárásmód, ami garantáltan végállapotba juttat, véges eljárásmód, $\gamma = 1$
2. Időegységenkénti átlagjutalom

## Optimális eljárásmód

>[!summary]+ DEF
>Optimális eljárásmód:
> $\pi^*=\arg\max E\left[ \sum_{t=0\dots \infty}\gamma^{t}R(s_{t}) \right|\pi]$
> 
> Egy állapot hasznossága: a belőle kiinduló állapotsorozatok várható hasznossága.

Az állapotsorozatok függnek a végrehajtott eljárásmódtól, így elsőként egy adott $\pi$ eljárásmódra definiáljuk a hasznosságot.
$$
U^{\pi}(s)=E\left[ \sum_{t=0}^{\infty}\gamma^{t}R(s_{t}) |\pi,s_{0}=s\right]
$$
Optimális eljárásmód:
$$
\pi^{*}(s)=\arg\max\sum_{s'}T(s,a,s')U(s')
$$
>[!summary]+ DEF
>Állapot hasznossága: az állapotban tartózkodás közvetlen jutalmának és a következő állapot várható leszámítolt hasznosságának összege, feltéve, hogy az ágens az optimális cselekvést választja.
> $U(s)=R(s)+\gamma\max_{a}\sum_{s'}T(s,a,s')U(s')$

## Markov döntési folyamat

>[!summary]+ DEF
>S: állapotok halmaza
> $s_{0}$: kezdőállapot
> A: cselekvések halmaza
> $P(s'|s,a)$: állapotátmenet
> $R(s,a,s')$: jutalom
> $\gamma$: leszámítolási tényező
> 
> Eljárás: egy cselekvés választása minden állapotban
> Hasznosság: leszámítolt jutalmak összege

Minden MDF állapot egy expectimax szerű keresési fát állít elő

>[!success] MDF megoldása
>- Az s állapot hasznossága $U^{*}(s)$ = várható hasznosság s állapotban kezdve, optimálisan cselekedve
>- Egy q(s,a) cselekvésérték hasznossága: $Q^{*}$ = a várható hasznosság *a* optimális cselekvés után *s* állapotból kezdve
>- Az optimális eljárásmód: $\pi^{*}$ = optimális cselekvés *s* állapotból
>
>Alapvető művelet:
>**Egy állapot értékének kiszámítása**
>- várható hasznoság optimális cselekvés mellett
>- a leszámítolt jutalmak átlagos összege
>- --> [[Keresés ellenséges környezetben#Expectimax Keresés|Expectimax]]!
>  
>A hasznosság meghatározása rekurzív!


Expectimaxxal túl sok munkát végzünk --> csak egyszer számoljuk ki a szükséges mennyiségeket
A fa örökké tart --> mélységkorlátos számítás, növekvő mélységgel, amíg a változás elég kicsi nem lesz

### Időben korlátozott hasznosság

Kulcsgondolat: időben korlátozott hasznosságok
Legyen $U_{k}(s)$ az *s* optimális hasznossága, ha a játék *k* időlépésen belül ér véget. Ez az, amit egy k-mélységű [[Keresés ellenséges környezetben#Expectimax Keresés|expectimax]] adna az *s* állapotból indulva.


**Értékiteráció**:
$U_{0}(s)=0$ -val kezdünk: ha nincs hátralevő időlépés, a várható jutalom összege 0.
Adott $U_{k}(s)$ értékek vektora, végezzünk minden állapotból egy-egy expectimax lépést:
$$
U_{k+0}(s)\leftarrow\max_{a}\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma U_{k}(s')]
$$
Ismételjük egészen konvergenciáig
Az egyes iterációk bonyolultsága $O(S^{2}A)$
Tétel: **egyetlen optimális értékhez konvergál**