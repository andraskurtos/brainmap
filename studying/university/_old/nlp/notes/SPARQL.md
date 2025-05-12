---
aliases: 
tags:
  - uni
  - 5semester
  - nlp
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2025-01-07 23:42
---

# SPARQL

A *SPARQL* egy lekérdező nyelv az [[RDF]] -hez. Egy gráfillesztésen alapuló lekérdező nyelv

Egy egyszerű SPARQL query:

Adat:
```
<http://example.org/book/book1>
<http://purl.org/dc/elements/1.1/title>
"SPARQL Tutorial""
```

Query:

```sparql
SELECT ?title
	WHERE { <http://example.org/book/book1>
			<http://purl.org/dc/elements/1.1/title>
			?title . }
```

![[Pasted image 20250107234456.png]]

![[Pasted image 20250107234635.png]]

![[Pasted image 20250107234704.png]]
![[Pasted image 20250107234750.png]]

