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
created: 2024-11-20 22:54
---

# [[Keresés]] ellenséges környezetben

>[!note]+ Játékfák
>Egy ágens:
>![[Pasted image 20241120230714.png]]
>
>Két ágens:
>![[Pasted image 20241120230731.png]]
>![[Pasted image 20241120230821.png]]


Determinisztikus, zéró összegű játékokban:
- Tictactoe, sakk, dáma, stb
- Egyik játékos minimalizálja, másik maximalizálja az eredményt

Minimax keresés:
- keresés állapottér gráfban: fa
- játékosok felváltva lépnek
- kiszámítja minden csomópnt minimax értékét: a legnagyobb elérhető hasznosság egy optimálisan játszó ellenséggel szemben

>[!summary]+ Implementáció:
> 
>def max-érték(állapot):
>	inicializál $v=-\infty$
>	for each *követő állapot* of *állapot*:
>		v=max(v,min-érték(*követő állapot*))
>	return v
>
>def min-érték(állapot):
>	inicializál $v=+\infty$
>	for each *követő állapot* of *állapot*:
>		v = min(v,max-érték(követő állapot))
>	return v
>
>def érték(állapot):
>	if *állapot* végállapot: return az *állapot* hasznossága
>	if a következő ágens MAX: return max-érték(állapot)
>	if a következő ágens MIN: return min-érték(állapot)


>[!question]+ Hatékonysága?
>Ugyanannyira, mint a [[Mélységi keresés|DFS]]
>Idő: $O(b^m)$
>Tár: $O(bm)$

>[!example]+ Példa:
>Sakk: $b\approx 35, m \approx 100$
>Egzakt megoldás kivitelezhetetlen
>Szükséges e felderíteni a teljes játékfát?

## A játékfa nyesése:

### Alfa-béta nyesés

Általános eset(MIN szerint)
- Éppen az *n* csomópont MIN-ÉRTÉKét számítjuk ki
- Végiglépkedünk *n* gyerekein
- *n* gyerekeinek minimum értéke egyre csökken
- Kinek fontos *n* értéke? MAX-nak
- Legyen *m* és *m'* két másik, már megvizsgált csomópont, amelyeket MAX választhat
- Ha *n* rosszabbá válik, mint *m* vagy *m'*, akkor MAX el fogja kerülni *n*-t, így nem kell vizsgálnunk *n* gyerekeit

>[!summary]+ implementáció:
> $\alpha$: MAX legjobb választási lehetősége eddig
> $\beta$: MIN legjobb választási lehetősége eddig
> 
> def max-érték(állapot):
> 	inicializál $v=-\infty$
> 	for each *követő állapot* of *állapot*:
> 		v=max(v, érték(követő-állapot, $\alpha, \beta$))
> 		if $v\geq \beta$ return v
> 		$\alpha=max(\alpha,v)$
> 	return v
> 	
> def min-érték(állapot):
> 	inicializál $v=+\infty$
> 	for each *követő állapot* of *állapot*:
> 		v=min(v,érték(követő állapot, $\alpha, \beta$))
> 		if $v\leq \alpha$ return v
> 		$\beta = min(\beta, v)$
> 	return v

>[!note]+ Tulajdonságai:
>- A nyesés nem befolyásolja a gyökér csomópont minimax értékét!
>- Köztes csomópontok értéke eltérhet egymástól
>- A gyerek csomópontok jó sorrendezése növeli a nyesés hatékonyságát
>- Tökéletes sorrendezéssel:
>	- Az időkomplexitás $O(b^m)$ -ről O $(b^{m/2})$ re csökken
>	- Dupla keresési mélység
>	- Sok esetben ekkor sem kivitelezhető a teljes mélységű keresés

## Erőforrások limitációja

>[!warning]+ Probléma:
>A realisztikus játékokban a keresés nem tud eljutni a levelekig!
>
>Megoldás: Mélységkorlátozott keresés
>Teljes mélység helyett csak korlátos mélységig megyünk le
>végállapotot cseréljük le egy kiértékelő függvénnyel
>
>--> optimális játék nem garantált
>ha bármikor le kell tudni állítani: iteratívan mélyülő keresés

Kiértékelő függvények:
Ideális függvény az adott állapot minimax értékét adja vissza
gyakorlatban tipikusan jegyek súlyozott lineáris kombinációja

- A kiértékelő függvények mindig pontatlanok
- Minél mélyebben van a kiértékelő függvény annál kevésbé számít a pontossága
- A kiértékelő függvény komplexitása vs a keresés komplexitása


## Bizonytalan kimenetelek

véletlen irányítja, nem az ellenfél

> [!question] Miért nem tudjuk mindig, hogy egy cselekvésnek mi a kimenetele?
> - Explicit véletlen: kockadobás
> - Véletlenszerűen viselkedő ellenfelek
> - Cselevés nem sikerül: robot mozgásánál megcsúszik a kerék

Az értékeknek az átlagos kimenetelt kellene tükrözniük, nem pedig a lehető legrosszabb kimenetelt

### Expectimax Keresés

Számítsuk ki az átlagos pontszámot optimális játékot feltételezve:

- Max csomópontok, mint minimax keresésben
- Véletlenszetű csomópontok, mint Min csomópontok, de a kimenetel bizonytalan.
- Számítsuk ki a várható hasznosságot
- Azaz a gyerekcsomópontok súlyozott átlagát

![[Pasted image 20241120235717.png]]

### Informált valószínűségek

Tegyük fel, hogy az ellenfél ténylegesen egy 2 mélységű minimaxot futtat, amelynek eredményét az esetek 80%-ában használja fel, a többiben véletlenszerűen lép.
Milyen fakeresést használjunk ehhez?

Expectimaxot!
- 