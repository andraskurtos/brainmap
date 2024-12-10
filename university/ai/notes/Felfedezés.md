---
aliases: 
tags:
  - ai
  - mi
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 20:55
---
# Felfedezés

Számos séma létezik a felfedezés kikényszerítéséhez a [[Megerősítéses tanulás|tanulási]] folyamatokban.

## Véletlen akciók

Legegyszerűbb: véletlen akciók ($\epsilon$ -mohó)
Minden időlépésnél érmedobás:
- kis valószínűséggel $\epsilon$, véletlen cselekvés
- nagy valószínűséggel $1-\epsilon$, az aktuális eljárásmód szerint cselekszünk

>[!error] Problémák a véletlenszerű cselekvésekkel
>tanulás befejezése után is folytatni kell a "túrázást"
>egy megoldás: idővel alacsonyabb $\epsilon$
>másik megoldás: felfedezési függvények

## Felfedezési függvények

- Mikor kell felfedezni?
	- Véletlenszerű akciók: fedezzen fel egy meghatározott mennyiségűt
	- Jobb ötlet: fedezzen fel olyan területeket, amelyek rosszaságát még nem állapították meg, végül hagyja abba a felfedezést


A felfedezési függvények vesznek egy becsült *u* értéket, és egy *n* látogatási számot, és "optimista" hasznosságot adnak vissza, pl:
$$
 f(u,n)= u+\frac{k}{n}
$$
![[Pasted image 20241121210132.png]]

