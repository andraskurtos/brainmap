---
aliases:
  - visszalépéses keresés
tags:
  - uni
  - 5semester
  - ai
  - mi
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-26 15:32
---





# Visszalépéses Keresés


A *visszalépéses keresés* egy [[Neminformált keresés|neminformált keresés]]i algoritmus [[Kényszerkielégítési Problémák|kényszerkielégítési problémák]] megoldására. Gyakorlatilag a [[Mélységi keresés|mélységi keresés]] egy speciális esete, minden szinten *egy-egy változóhozzárendeléssel*, és valamely *kényszer sérülése esetén visszalép.*

>[!summary]+ A Visszalépéses Keresés algoritmusa
>
>![[Pasted image 20240926151827.png]]

>[!success]+ A [[Visszalépéses Keresés|visszalépéses keresés]] hatékonyságának növelése
>
>- Általános, tárgyterület-független heurisztikákkal növelhetjük a hatékonyságot
>- [[Visszalépéses Keresés#Szűrés|Szűrés]]:
>	- Ki tudjuk korán szűrni a kudarcra ítélt megoldásokat?
>- [[Visszalépéses Keresés#Sorrendezés|Sorrendezés]]:
>	- **Változók sorrendezése**: Melyik változóhoz rendeljünk értéket a következő lépésben?
>	- **Értékek sorrendezése**: Melyik értéket rendeljük hozzá először a változóhoz?
>- [[Visszalépéses Keresés#Struktúra|Struktúra]]:
>	- Ki tudjuk használni a probléma struktúráját?

---

## Szűrés

### előretekintő ellenőrzés


Az **előretekintő ellenőrzés** minden egyes alkalommal, amikor egy X változó értéket kap, minden, az X-hez kényszerrel kapcsolt, lekötetlen Y-t megvizsgál, és Y tartományából *törli* az X számára választott értékkel *inkonzisztens értékeket*.

>[!warning]+ Korlátai
>- Az előretekintő ellenőrzés ugyan sok inkonzisztenciát észrevesz, de *nem mindet*.
>- Ráadásul nem látja jól előre a kudarcokat.

---

### élkonzisztencia ellenőrzés

Az **élkonzisztencia ellenőrzés** alkalmazhatő *előfeldolgozó lépésként* a [[Keresés|keresés]] megkezdése előtt, vagy a keresési folyamat minden egyes hozzárendelését követően, *terjesztési lépésként*. Előbb tárja fel a problémákat, mint az [[Visszalépéses Keresés#előretekintő ellenőrzés|előretekintő ellenőrzés]]. 

>[!summary]+ Élkonzisztenica 
>$X\to Y$ él konzisztens akkor és csak akkor, ha $X$ minden $x$ értékére $\exists Y$-nak valamilyen megengedett $y$ értéke.
>
>==> terjesszük el az [[Él|él]] konzisztenciáját a kényszergráf összes élére, biztosítsuk az összes él konzisztenciáját!
>==> Ha $X$ tartományból törlünk egy élet, $X$ szomszédjait újra ellenőrizzük!


---

## Sorrendezés

### A *legkevesebb fennmaradó érték* ötlete

A legkisebb számú megengedett értékkel rendelkező változóval kezdjünk, illetve folytassunk ==> **lokálisan kicsi az elágazási tényező!**

Ez a legkevesebb fennmaradó érték heurisztika (*minimum remaining values, MRV*)

### *Fokszám heurisztika* ötlete

Az MRV [[Heurisztikus keresés#Keresési heurisztika|heurisztika]] semmit sem segít abban, hogy melyik változóval kezdjünk, ha mindnek ugyanannyi megengedett értéke van kiinduláskor.
A *későbbi választások* elágazási tényezőjét csökkentheti, ha azt a változót választjuk, amely a legtöbbször szerepel a még hozzárendeletlen változókra vonatkozó kényszerekben.

### *Legkevésbé korlátozó érték* ötlete

Előnyben részesítjük azt az **értéket**, amely a legkevesebb választást zárja ki a kényszergráfban a szomszédos változóknál.

---

## Struktúra

>[!question]+ Miben segíthet a [[Kényszerkielégítési Problémák|CSP]] struktúrája?
>
>[[Kényszerkielégítési Problémák|CSP]] [[Gráf|gráfja]]: független komponensek
>
>[[Kényszerkielégítési Problémák|CSP]] fa [[Gráf|gráf]]: megoldás könnyű, változószámban lineáris
>
>1. válasszunk egy levelet gyökérnek, majd sorrendezzük a változókat (élek irányítása)
>2. élkonzisztencia-nyesés gyerekektől a szülőik felé
>3. értékek hozzárendelése a gyökér csomóponttól kezdve
>
>![[Pasted image 20240926181844.png]]

