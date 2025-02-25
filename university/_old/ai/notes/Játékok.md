---
aliases: 
tags:
  - ai
  - 5semester
  - uni
  - mi
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-20 22:55
---
# Játékok

## Játékok típusai

Rendkívűl sokféle játék létezik, melyeket tulajdonságok alapján csoportosíthatunk.

- Determinisztikus vagy sztochasztikus?
- Egy, kettő vagy több játékos?
- Zéró összegű játék?
- Teljes információ? (láthatjuk az állapotot?)

Algoritmus stratégiát ad, amellyel jó/optimális választ javasol minden egyes állapotban.

### Determinisztikus játékok

Egy lehetséges formalizmus:

Állapotok: S ($s_{0}$ kezdőállapot)
Játékosok: P= $\{1\dots N\}$ (rendszerint felváltva lépnek)
Cselekvések: A (a soron következő játékos és az aktuális állapot szabja meg)
Állapotátmenetfüggvény: SxA --> S
Célállapot teszt: S --> {igaz, hamis}
Célállapot hasznosság: SxP --> R

A megoldás egy játékos számára a stratégia S-->A

### Zéró összegű játékok

Az ágensek hasznossága ellenkező
Egyedüli értékként is tekinthető, amelyet az egyik játékos maximalizálni, a másik pedig minimalizálni akar.
Ellenséges játékos, tiszta versengés

### Általános játékok

Az ágensek hasznossága független
Kooperáció, közömbösség, versengés mind elképzelhető


## Kevert rétegű játék

[[Keresés ellenséges környezetben#Expectimax Keresés|Expectiminimax]]
- környezet plusz egy véletlenszerű ágens, ami minden min/max játékos után lép
- minden csomópont kiszámítja a 'megfelelő kombinációját' a gyermekeinek

## Több ágenses hasznosság

Mi van abban az esetben, ha ne zéróösszegű a játék, hanem több játékos van?

[[Keresés ellenséges környezetben|Minimax]] általánosítása:
- levelek hasznosság n-esekkel rendelkeznek
- csomópont értékek szintén hasznosság n-esek
- Mindegyik játékos saját komponensét maximalizálja
- ebből adódhat kooperatív és versengő viselkedés dinamikusan


## Monte Carlo tree search

Monte Carlo kiséreltek:
- ismételt véletlen mintavétel egy adott eloszlásból
- összegyűlt minták gyakorisága alapján közelíthetők numerikusan nehezen számítható értékek
Jól használható komplex döntések meghozatalához

**Lépései:**

1. Kiválaszás
2. Terjeszkedés
3. Szimuláció
4. Visszaterjesztés

![[Pasted image 20241121002207.png]]

