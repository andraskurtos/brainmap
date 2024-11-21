---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 20:02
---
# Megerősítéses tanulás

>[!question]+ Felvetés
>Feltételezzünk egy [[Markov döntési folyamat]]ot:
>- állapothalmaz $s \in S$
>- cselekvések halmaza A
>- állapotátmenet-modell T(s,a,s')
>- jutalomfüggvény R(s,a,s')
>
> $\pi(s)$ eljárásmódot keressük
> **Új fordulat**: nem ismerjük T-t vagy az R-t
> - Nem tudjuk, mely állapotok jók, vagy mit tesznek a cselekvések
> - A cselekvéseket és állapotokat ki kell próbálni a tanuláshoz
>MDF vs RL:
>![[Pasted image 20241121200910.png]]

## Modellalapú tanulás

Tapasztalaton alapuló közelítő modell tanulása. Értékek megoldása úgy, mintha a megtanult modell helyes lenne

1. lépés: Empirikus MDF modell tanulása
	- Számoljuk meg az s' kimeneteleket minden s, a-ra
	- Normalizáljuk ezeket, hogy becslést adjunk $T(s,a,s')$ -re
	- Fedezzük fel az egyes $R(s,a,s')$ jutalmakat (s, a, s') átmenetek megfigyelése esetén
2. lépés: A tanult MDF megoldása
	- Például használjuk az értékiterációt

>[!example]+ Várható Életkor példa:
>![[Pasted image 20241121201438.png]]


## Modellmentes tanulás

### Passzív megerősítéses tanulás

![[Pasted image 20241121201535.png]]

Egyszerűsített feladat: **eljárásmód értékelés**
- Bemenet: rögzített eljárásmód $\pi(s)$
- Nem ismerjük az átmeneteket T(s,a,s')
- Nem ismerjük a jutalmakat R(s,a,s')
- Cél: tanuljuk meg az **egyes állapotok hasznosságát**

Ekkor:
- Nincs választási lehetőség cselekvések tekintetében
- Eljárás végrehajtása, és modell tanulása
- Ez nem offline tervezés
- Ténylegesen cselekvéseket kell végrehajtani és megfigyelni a következményeit

### Közvetlen kiértékelés

Cél: az egyes állapotok hasznosságának kiszámítása $\pi$ eljárásmód szerint
Ötlet: vegyük a megfigyelt mintaértékek átlagát
- cselekvés $\pi$ szerint
- minden alkalommal, amikor belép egy állapotba, rögzítjük, hogy mennyi lett a leszámítolt jutalmak összege
- minták átlagolása

>[!success] Előnyök
>- könnyen érthető
>- nem igényli T, R ismeretét
>- végül kiszámolja a helyes átlagértékeket, csak az átmenetek felhasználásával

>[!error]+ Hátrány
>- Az állapotok közötti kapcsolatokra vonatkozó információkat elpazarolja
>- Minden állapotot külön kell megtanulni
>- Hosszú időbe telik

### Mintaalapú eljárásmód kiértékelés

>[!note]+ Eljárásmód kiértékelés használata
>Egyszerűsített Bellmann frissítéssel számítható U egy rögzített eljárásra:
>- Minden fordulóban cseréljük ki U-t egylépéses előretekintéssel
> $V_{0}^\pi (s)=0$
> $V_{k+1}^\pi(s)\leftarrow \sum_{s'}T(s,\pi(s),s')[R(s,\pi(s),s')+\gamma V_{k}^\pi(s')]$
>- sajnos ehhez szükség van T-re és R-re
>- **Hogyan tudjuk elvégezni U frissítését T és R ismerete nélkül?**
>- --> hogyan vegyünk súlyozott átlagot a súlyok ismerete nélkül?

Ölet: Vegyünk mintákat az s' kimenetelekből, és átlagoljuk őket:
$$
sample_{1}=R(s,\pi(s),s'_{1})+\gamma V_{k}^\pi(s'_{1})
$$
$$
sample_{2}=R(s,\pi(s),s_{2}')+\gamma V_{k}^\pi(s_{2}')
$$
$$
 sample_{n}=R(s,\pi(s),s_{n}')+\gamma V_{k}^\pi(s'_{n})
$$
$$
V_{k+1}^\pi(s)\leftarrow \frac{1}{n}\sum_{i}sample_{i}
$$
**Majdnem jó, de nem állíthatjuk vissza az időt, hogy egymás után több s' mintát kapjunk s-ből!**

### Időbeli különbség tanulás

Ötlet: Tanuljunk **minden megtapasztalt mintából**
- frissítjük U(s)-t minden egyes állapotátmenetnél (s,a,s',r)
- a jellemző (gyakori) hasznosságok gyakrabban hozzájárulnak a frissítéshez

A hasznosságok időbeli különbség szerinti tanulása
- Az eljárásmód még mindig rögzített, még mindig értékelést végez!
- A hasznosságok változtatása a következő hasznosságok irányába, mozgóátlaggal.
$$
 U(s): sample = R(s,\pi(s),s')+\gamma V^\pi(s')
$$
$$
V^\pi(s)\leftarrow(1-\alpha)V^\pi(s)+(\alpha)sample
$$
$$
v^\pi(s)\leftarrow V^\pi(s)+\alpha(sample-V^\pi(s))
$$
>[!summary]+ Exponenciális mozgóátlag
>Elfelejti a múltat
>![[Pasted image 20241121204520.png]]

>[!error] Problémák
>- Az IK tanulás egy modellmentes módja az eljárásmódok értékelésének, amely mozgóátlagokkal utánozza a Bellman-frissítéseket
>- Ha azonban a hasznosságokaz eljárásmóddá akarjuk alakítani, akkor gond adódik: 
>  $\pi(s)=\arg \max_{a}Q(s,a)$
>  $Q(s,a)=\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V(s')]$
>  Ötlet: Q-értékeket tanuljunk, ne hasznosságokat
>  Ez a cselekvés kiválasztását is modellmentessé teszi

## Aktív megerősítéses tanulás

>[!summary]+ Teljes megerősítéses tanulás
>optimális eljárásmód meghatározása
>- Nem ismerjük a T(s, a, s') átmeneteket
>- Nem ismerjük az R(s,a,s') jutalmakat
>- Ki kell választani a cselekvéseket
>- Cél: **optimális eljárásmód meghatározása**

ekkor: 
- a tanuló döntéseket hoz
- alapvető kompromisszum: felfedezés vs kihasználás
- ez nem offline tervezés
- ténylegesen lépéseket tesz a világban, és megtudja, mi történik

### Q-tanulás

minta alapú Q-érték iteráció
$$
 Q_{k+0}(s,a)\leftarrow \sum_{s'}T(s,a,s')[R(s,a,s')+\gamma \max_{a'}Q_{k}(s',a')]
$$
Q(s,a) értékeket kell tanulni menet közben
- beérkezik egy minta: (s, a, s', r)
- tekintsük a korábbi becslést: Q(s,a)
- tekintsük új  minta alapján kapott becslést:
$$
	 sample = R(s,a,s')+\gamma \max_{a'}Q(s',a')
$$
- építsük be az új értékeket egy mozgóátlagba
$$
 Q(s,a) \leftarrow (1-\alpha)Q(s,a)+(\alpha)[sample]
$$

>[!summary]+ Tulajdonságai
>- Érdekes eredmény: A Q-tanulás konvergál az optimális eljárásmódhoz, még akkor is, ha szuboptimálisan cselekszünk.
>- Ezt hívják off-policy/eljárásmódon kívüli tanulásnak

>[!error] Lehetséges problémák
>- Az állapotokat elégséges mértékben fel kell fedezni
>- A tanulási rátának alacsonnyá kell válnia, de nem szabad túl gyorsan csökkenteni
>- Alapvetően, hosszú távon nem számít, hogyan választjuk ki a cselekvéseket




