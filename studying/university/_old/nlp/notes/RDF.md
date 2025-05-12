---
aliases: 
tags:
  - 5semester
  - uni
  - nlp
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2025-01-07 23:40
---

# RDF

## RDF tervezési szempontok

- Egyszerű adatmodell
- Formális szemantika és egyszerű következtetési lehetőségek
- Bővíthető URI
- [[XML]] alapú szintaktika
- Bárki megfogalmazhat állításokat az erőforrásokról

RDF kifejezések leírhatóak hármasokkal:
\<alany> \<állítmány> \<tárgy>

### Alapelvek

- Külön értelmezhetően definiálva
	- modell struktúra - gráf
	- interpretációs szemantika
	- szintaktikák
- Mindössze két alap adattípus
	- URI/URIref: minden URI-val azonosított
	- Literálisok ( String vagy más XSD adattípus)
- Integrálható webes információkkal
	- [[XML]] séma adattpusok
	- Referenciák http elérésű információkhoz
- Nyílt világ feltételezés
	- Bárki megfogalmazhat állításokat bármilyen erőforráshoz
	- Nem garantált a teljesség és a konzisztencia

### Alapelemek

- Gráf adatmodell
- URI alapú szótárak
- Adattípusok
- Literálisok
- [[XML]] szerializációs szintaktika
- Egyszerű tények leírása
- Következtetés

Az RDF lekérdezhető a [[SPARQL]]-el
