---
created: 20240901 23:53
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# Adatelérési réteg

Feladata az adatforrások kényelmes elérésének biztosítása. Biztosítja az *elemi adatszolgáltatási műveleteket*, mint pl. egy új rekord tárolása, meglévő módosítása vagy törlése.

---
Az [[Adatforrások az Adatvezérelt Rendszerekben|adatforrásokat]] és az adatelérési réteget szokás még egyben **adatrétegnek** is nevezni.
___

![[Pasted image 20240904162816.png]]

## Data Access Components

Az adatbázisok eléréséhez szükséges funkcionalitásokat valósítják meg. Elrejtik a komplexitást, ami az adatbázis kezeléséből adódik, és a felsőbb réteg számára *egyszerűen használható szolgáltatásként nyújtják az adattárolást*.

Ide tartozik pl. az SQL parancsok kezelése, és az adatbázis modelljének leképezése az [[Üzleti Logika Réteg|üzleti logika]] számára kényelmesen használható módon.

Ha nem adatbázisban, hanem [[Adatforrások az Adatvezérelt Rendszerekben#2 - KÜLSŐ ADATFORRÁSOK|külső szolgáltatásban]] találhatók az adatok, azzal az ún. *service agent*-ek kommunikálnak. 

Ebben a rétegben a komponensek egy konkrét technológia köré csoportosulnak, pl az adatbázisrendszer eléréséhez használt technológia.

---

## Réteg feladatai

### Leképezés

Mivel az adatbázisban tárolás gondolkodásmódja és az objektumorientált modellezés nem fedik egymást egy az egyben, ezen réteg feladata a *két világ közötti leképezés és megfeleltetés.*

A relációs DB-kben használt kulcsokat [[Objektum-relációs Leképezés|objektumorientált asszociációkra és kompozíciókra fordítjuk]], és ha kell, *konvertáljuk* az adattípusokat.

---

### Kommunikáció külső rendszerekkel

Az adatelérési réteg felel azért is, hogy a kapcsolatokat a *megfelelő módon kezelje*, és ha lehetséges, újrahasználja azokat (*connection pooling*). Teljesítmény szempontból ugyanis nem mindegy, hogy a kiszolgáló megszólítása milyen gyakorisággal és módon történik. Egyes módok (pl HTTP, TCP kapcsolat felépítése gyors, míg például egy DB szerverhez kapcsolódás folyamata lassú és komplikáltabb.

---

### Adatelérés kezelése

Az adatok *konkurens* elérésének és módosításának kezelése is ezen réteg feladata. Mivel az alkalmazást egyszerre több felhasználó használja, előfordulhat, hogy *többen módosítanak egyszerre*, az adatok azonban ebben az esetben is *konzisztensek kell maradjanak*.


