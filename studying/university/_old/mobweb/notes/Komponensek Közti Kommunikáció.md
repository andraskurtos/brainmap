---
aliases: 
tags:
  - uni
  - 5semester
  - mobweb
  - android
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 18:32
---





# Kommunikáció a komponensek között


## Intent

Az intent az egyik komponensből a másikba való kommunikáció módja. Ilyen például a következő képernyőre lépés, vagy a zenelejátszó service indítása.

![[Pasted image 20241015183425.png]]

Az intent átadása mindig az [[Android]] runtime-on keresztül történik.

![[Pasted image 20241015183455.png]]

>[!Summary]+ Az intent típusai:
>- Explicit Intent:
>	- Konkrétan meg van nevezve a célkomponens
>- Implicit Intent:
>	- A végrehajtandó feladat kerül leírásra
 
>[!summary]+ Az intent részei:
>- Címzett komponens osztályneve: ha üres, akkor az [[Android]] megkeresi a megfelelőt
>- Akció: az elvárt vagy megtörtént esemény
>- Adat: az adat, amin az esemény értelmezett
>- Kategória: további kritériumok a feldolgozó komponenssel kapcsolatban
>- Extrák: saját kulcs-érték párok, amiket át akarunk adni a címzettnek.
>- Kapcsolók: Activity indításának lehetőségei.

---

### Explicit Intent

Mindkét esetben a *startActivity()* függvényt használjuk:

```kotlin
startActivity(Intent)
```

Explicit hívás, ha az Intentben kitöltjük a címzett komponens nevét konstruktorral vagy setterrel:

```kotlin
var i: Intent = Intent(getApplicationContext(),
					  ListProductsActivity::class.java)
startActivity(i)
```

Ha a *ListProductsActivity*-ből már van egy példány a memóriában, folytatódik, ha nincs, az Android példányosítja és elindítja.

---

>[!tldr]+ Intent képességek
>Az Android komponensek (Activity, Service, Broadcast Reciever) között Intentekkel zajlik a kommunikáció. Nem csak alkalmazásokon belül, hanem azok között is lehetséges Intentekkel kommunikálni, használhatjuk más alkalmazás komponensét, vagy kiajánlhatjuk sajátunkat. Rendszerszintű eseményeket kezelhetünk így.

>[!tip]+ Intent filter
>Lehetséges saját alkalmazásunk funkcióinak kiajánlása mások számára, ezt az AndroidManifestben kell deklarálni. Ha nincs Intent Filter deklarálva, a komponens csak explicit intentet képes fogadni.

