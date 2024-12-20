---
created: 20240901 23:53
tags:
  - 5semester
  - datadriven
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# Adatforrások az [[Adatvezérelt rendszerek architektúrái#Mi az *adatvezérelt* alkalmazás?|Adatvezérelt Rendszerekben]]
## 1 - ADATBÁZIS

A leggyakoribb adatforrás az adatbázis. Ez lehet relációs, vagy akár [[NoSQL]] adatbázis is. Feladata az adatok *perzisztens* tárolása. Tipikusan egy *megbízható gyártótól származó szoftver*, amely egy külön kiszolgálón fut, az [[Adatelérési réteg]] hálózaton keresztül éri el. A tárgy folyamán többek között az [[MS SQL]] -el és a [[MongoDB]] -vel foglalkozunk.

## 2 - KÜLSŐ ADATFORRÁSOK

Lehetséges, hogy az adatok nem mind a saját adatbázisunkban találhatók, hanem *külső szolgáltatásokban*, melyeket adatbázisként használunk.

> [!example]+ Példa
> Gmail esetén a csatolt fájloknál lehetőség van Google Drive-ról csatolni fájlokat, ekkor a Drive nem adatbázis, de adatforrásként viselkedik a használat szempontjából.

--- 

A külső adatforrások tehát nem sokban különböznek az adatbázisoktól ebben a kontextusban, hiszen belső működésükbe nem látunk bele, csak *adatokat szolgáltatnak számunkra*.