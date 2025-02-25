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
created: 2025-01-08 14:55
---
# RDFS

Mire használható az RDFS?

Tekinthető [[RDF]] kifejezés szótárként is, hagyományos megközelítésként séma
Alkalmazási területek szemantikájának leírása
Tulajdonságok újrafelhasználhatóságának biztosítása. Értelmezési tartomány és értékkészlet megadása.
Tulajdonságok osztályokhoz,alosztályokhoz rendelése specifikálható


## Állítások validációja

Alkalmas-e az alany az állításhoz?

```rdf
{
	exp:CandidateFor,
	[http://www.epolitix.com/austin-michell],
	[http://www.microsoft.com/]
}
```

Alkalmas-e az állítmány az alanyhoz?

```
{exp:DOB,
[http://www.pa.press.net/constituencies/281],
"19 September, 1934"
}
```

## Erőforrások tipizálása

Honnan tudjuk, hogy ez egy választókörzet?

```
<rdf:Description rdf:about="URI">
	<exp:Name>Great Grimsby</exp:Name>
	<exp:MainIndustry>Fishing</exp:MainIndustry>
</rdf:Description>

```

Választókerület típusba kell sorolni az erőforrást

![[Pasted image 20250108150312.png]]

Megfelelő állítmányok használata az alanyokhoz:
![[Pasted image 20250108150403.png]]

--> hozzunk létre rdf:propety CandidateFor
![[Pasted image 20250108150429.png]]

