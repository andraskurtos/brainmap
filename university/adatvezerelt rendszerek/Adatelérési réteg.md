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

## Data Access Components
Az adatbázisok eléréséhez szükséges funkcionalitásokat valósítják meg. Elrejtik a komplexitást, ami az adatbázis kezeléséből adódik, és a felsőbb réteg számára *egyszerűen használható szolgáltatásként nyújtják az adattárolást*.

![[Pasted image 20240904155130.png]]

Ide tartozik pl. az SQL parancsok kezelése, és az adatbázis modelljének leképezése az üzleti logika számára kényelmesen használható módon.

Ha nem adatbázisban, hanem [[Adatforrások az Adatvezérelt Rendszerekben#2 - KÜLSŐ ADATFORRÁSOK|külső szolgáltatásban]] találhatók az adatok, azzal az ún. *service agent*-ek kommunikálnak. 

Ebben a rétegben a komponensek egy konkrét technológia köré csoportosulnak, pl az adatbázisrendszer eléréséhez használt technológia.