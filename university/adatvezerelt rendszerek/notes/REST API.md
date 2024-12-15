---
aliases: 
tags:
  - adatvez
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-03 13:35
---
# REST API

A *Representational State Transfer* API egy webes API, amin keresztül adatokat kérdezhet le / módosíthat a kliens. Az API erőforrás (URI) alapú, az adat [[JSON]] -ban vagy [[XML]] -ben utazik. A lekérdezéshez [[Webes Architektúra#Metódusok|HTTP methodokat]] használ.

## Metódusok

**GET**: *Lekérdezi* az adott URL-en található erőforrást. A válaszüzenet törzse tartalmazza a kért erőforrás részleteit.
**POST**: *Létrehoz* egy új erőforrást a megadott URL-en. Az új erőforrás adatait az üzenet törzsében adjuk meg.
**PUT**: *Létrehozza, vagy lecseréli* az adott URL-en lévő adatforrást. A kérés törzsében adjuk meg az erőforrást.
**PATCH**: *Részlegesen frissíti* az erőforrást. A kérés törzsében meghatározott módosítások kerülnek végrehajtásra az erőforráson.
**DELETE**: *Törli* az adott URL-en levő erőforrást.

