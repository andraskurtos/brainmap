---
aliases: 
tags:
  - uni
  - 5semester
  - ai
  - mi
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 00:22
---

# Logika

>[!note] Logika szerepe a komplex [[Intelligencia#Mesterséges Intelligencia|MI]] alkalmazásokban:
>Logikai szabályalapú következtetés és beavatkozás
>Egy vagy több feltétel logikai állításként történő megfogalmazása
>Reakciók, beavetkozások megadása logikai szabályok alapján

A logika a reprezentáció és manipuláció eszköze.
Szintaktika - legálisan létrehozható szimbolikus mondatok
Szemantika - a világ tényei, melyekre a szimbolikus mondatok vonatkoznak
## Logikai következtetések fajtái

### Dedukció

A dedukció az ókori görögöknél jelent meg először. Olyan formálisan érvényes következtetés, olyan tényeket származtat, melyek a premisszákból mindenképpen következnek

### Indukció

Tények tömörítése, általánosítása --> példából tanulás modellje

### Abdukció

Az abdukció a belátás folyamata.
formálisan nem igaz, de annál hasznosabb

![[Pasted image 20241121002655.png]]

## Vonzatreláció és következtetés

Tények neveznek valós dolgokat, amelyek egymással kapcsolatban vannak. Ha az egyik igaz a világban, és a másik is szükségszerűen az.

> [!example]
> Vonzatreláció tudásbázis TB és egy $\alpha$ mondat között:
> $alpha$ vonzata TB-nek, ha $\alpha$ minden olyan világban igaz, ahol TB is

> [!summary] Definiíciók
> **vonzat**: logikai konzekvencia
> **Következtetés**: a vonzat kiszámítása a mondatok formális manipulálásával
> **Bizonyítás**: a következtetési algoritmus lépési sorozata

>[!note] Jelölések:
> $\alpha$ vonzata TB-nek: $TB|=\alpha$
> $\alpha$ bizonyítható TB-ből: $TB|--\alpha$

>[!question] Mi egy mondat jelentése?
>A mondat írójának interpretációt kell adnia hozzá.
>Az igazság függ a mondat interpretációjától és a világ állapotától.
>![[Pasted image 20241121003327.png]]

## Modellek

>[!summary]+ Def.
>Modell: Bármely (akár formális) világ, ahol egy mondat bizonyos interpretációban igaz. Egy mondatnak számos modellje lehet.

Ha adott egy TB, a következtetés létrehozhat egy új $\alpha$ mondatot, amely vonzata TB-nek. 
Ha adott $\alpha$ mondat, eldöntheti, hogy $\alpha$ vonzata-e a TB-nek.

>[!summary] Def
>Igazságtartó következtetés: ha csak igazságfüggvényekkel dolgozik.
>Formális bizonyítás: igazságtartó következtetési eljárás lépései
>Következtetési eljárás *teljes*: ha minden vonzatmondathoz talál egy bizonyítást, avagy ami igaz, az bebizonyítható
>helyes: ha minden bizonyított mondat vonzatrelációban áll a felhasznált tényekkel, avagy ami bebizonyított, az igaz is.
>Érvényes: ha minden világban minden lehetséges interpretációja igaz, függetlenül attól, hogy mit akar jelenteni, és mi a világ állapota.
>Kielégíthető: ha létezik olyan interpretáció, hogy valamely világban igaz.


## Ítéletkalkulus

Szintaktika: igaz és hamis logikai konstansok, P1, P2, ... ítélet szimbólumok
Szemantika:
Minden modell igaz/hamis értéket rendel minden ítéletszimbólumhoz, pl:
P1=igaz, P2=hamis, ...

Tetszőleges modell szemantikája rekurzív elemzéssel meghatározható.


![[Pasted image 20241121004021.png]]


> [!NOTE] Tétel
> **Ítéletlogika eldönthető és teljes:**
> Minden jól definiált mondat akár *igaz*, akár *hamis* volta belátható véges algoritmussal --> a következtetés igazságtábla módszere teljes, mindig lehetséges kiszámolni a tábla $2^n$ sorát bármely n ítéletszimbólumot tartalmazó bizonyítás esetében. Azonban n-ben exponenciális.
> 
> **Ítéletlogika monoton:**
> amikor új mondatot adunk hozzá TB-hoz, minden korábban maga után vonzott mondata az eredeti TB-nek továbbra is mondata marad az új TB-nek.




