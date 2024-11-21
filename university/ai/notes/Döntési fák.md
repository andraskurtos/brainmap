---
aliases: 
tags:
  - 5semester
  - ai
  - mi
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 21:39
---

# Döntési fák


A döntési fa egy **logikai függvény**, melynek bemenete egy tulajdonsághalmazzal leírt objektum vagy szituáció, kimenete pedig igen/nem, egy döntés/cselekvés.
A belső csomópontok valamely tulajdonságok tesztjei, az élek a tesztek lehetséges értékei, a levelek pedig a logikai értékek, melyeket akkor adunk ki, ha ezt a levelet elértük.

![[Pasted image 20241121214418.png]]

>[!note] Döntési fák kifejezőképessége
>teljes - minden ítéletlogikai függvény felírható döntési faként
>DE: exponenciális


## Döntési fák kialakítása példák alapján

példa: (attribútumok értékei, célpredikátum értéke)
példa besorolása: a célpredikátum értéke
pozitív/negatív példa: a célpredikátum értéke igaz/hamis
tanító halmaz: a teljes példahalmaz

>[!summary]+ DEF
>**triviális fa**: könnyű - mindegyik példához egy önálló bejárási út