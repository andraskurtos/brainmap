---
aliases: 
tags:
  - 5semester
  - adatvez
  - ai
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-02 16:34
---

# Objektum-Relációs Leképezés


Az O/R leképezés feladata az üzleti objektumok leképezése relációs adatmodellre, ezzel az [[Adatelérési réteg|adattárolás]] és az [[Üzleti Logika Réteg|üzleti folyamatok]] összekötése. 

>[!warning]+ Problémák:
>- Eltérő koncepciók
>- Öröklődés
>- Shadow információk
>- Kapcsolatok leképezése

## Alapötlet

Az alapötlet szerint az osztályokat táblákba, az adattagokat oszlopokba, a kapcsolatokat pedig foreign keyekbe képezzük le.
![[Pasted image 20241024131606.png]]

Ezzel is adódnak azonban problémák.

>[!warning]+ Problémák:
>- Összetett mezők:
>	- Vásárló
>		- Cím (irányítószám, utca, város)
>-  Eltérő adattípusok --> konverzió
>  
>  ![[Pasted image 20241024131758.png]]

---

## Shadow információk

A *shadow információk* szükségesek a perzisztencia megvalósításához. Ilyenek például a kulcsok, időbélyegek, verziószámok, stb. Az üzleti objektumba helyezni őket nem szükséges, de mégis kezelni kell valahogyan.

---

## Öröklés

Az *öröklődés* modellezése a következőképpen történik:

1. Hierarchia leképezése egy *közös táblába*
2. Minden *valós osztály* leképezése saját táblába (akikből objektumok képződhetnek)
3. *Minden osztály* leképezése saját táblába
4. Osztályok és hierarchia szintek *általános leképezése*

>[!example]+ Példa:
>