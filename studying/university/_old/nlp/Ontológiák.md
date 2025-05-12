---
aliases: 
tags:
  - 5semester
  - nlp
  - termnyelv
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2025-01-08 22:30
---
# OWL

- Osztályok
	- Person osztály
	- Man, Woman alosztály
- Tulajdonságok
	- isWifeOf, isHusbandOf
- Tulajdonság jellemzők, korlátok
	- inverseOf
	- domain
	- range
	- cardinality
- Osztályok közti relációk
	- disjointWith

Ontológiák:
RDFS hasznos, de nem ad megoldást a szemantika megfelelő szintű leírására
Összetett alkalmazások további igényei:
- tulajdonságok leírása, jellemzése
- különböző URI-val rendelkező objektumok azonosságának leírása (ekvivalencia)
- osztályok diszjunkt vagy éppen ekvivalens jellege
- osztályok konstruálása (nemcsak megnevezése)
- következtetési igények támogatása:
	- Pl.: "Ha két Person erőforrás A és B azonos **foaf:email** tulajdonságokkal rendelkeznek, akkor A és B identikus"


### Web Ontology Language

OWL az SZW struktúrába egy újabb réteg, az [[RDFS]] bővítése
- Saját névterek saját elemek, kifejezések
- RDFS-re épül (tartalmazza)

Önálló SZW aláírás
	"OWL 2"

## Ekvivalencia relációk

Osztályokra:
	owl:equivalentClass: ekvivalensek
	owl:disjointWith: nincs közös elemük

Tulajdonságokra:
	owl:equivalentProperty
	owl:propertyDisjointWith

Egyedekre:
	owl:sameAs
	owl:differentFrom: negáltja a sameAs-nek


> [!EXAMPLE]+ Példa logikai kapcsolatokra
> Ha egy tulajdonság inverse function-al lett definiálca, akkor a tulajdonság tárgya egyértelműen meghatározza a tulajdonság alanyát.
> 
> Ha a következő igaz két állításra:
>```
>:email rdf:type owl:InverseFunctionalProperty.
>\<A> :email "mailto: a@b.c "
>\<B>:email: "mailto: a@b.c "
>```
>
>Akkor ebből levezethető
> ```
> \<A> owl:sameAs \<B>.
> ```
> 
> Így új relációkhoz jutunk.


Owl osztályok konstruálhatóak más osztályok alapján
	elemek felsorolásával
	osztályok relációinak alkalmazásával: unio, metszet, komplemens, stb
Osztályok, egyedek
	önálló owl:Class osztály van definiálva
	Egyedek definiálása külön osztályban történik: owl:Thing
![[Pasted image 20250108232545.png]]

![[Pasted image 20250108233946.png]]
 