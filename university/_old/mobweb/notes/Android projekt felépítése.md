---
aliases:
  - android projekt felépítése
  - android fordítás
  - android project
tags:
  - 5semester
  - uni
  - mobweb
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-26 18:50
---





# Android Projekt Felépítése


## Az [[Android]] .apk állomány

A *.apk* állomány leginkább a [[Java]] világban megszokott .jar-hoz hasonlítható, de vannak jelentős eltérések. Az apk tömörített állomány, mely tipikusan a következő tartalommal rendelkezik:
- META-INF könyvtár
	- CERT.RSA: alkalmazás tanúsítvány
	- MANIFEST.MF: meta információk kulcs érték párokban
	- CERT.SF: [[Erőforrások]] listája és SHA-1 hash értékük
- Res könyvtár: erőforrásokat tartalmazza
- AndroidManifest.xml: név, verzió, jogosultság, könyvtárak
- classes.dex: lefordított osztályok a VM számára érthető formátumban
- resources.arsc

>[!info]+ A fordítás menete:
>
>![[Pasted image 20240926185535.png]]

## Android alkalmazások telepítése

A Play-ből az alkalmazások egy .apk állományban, vagy App Bundle-ben kerülnek letöltésre. A telepítésére nem a Play alkalmazás, hanem az úgynevezett *PackageManagerService* felelős. A *PackageManagerService* akkor is látható, ha más úton töltjük le az .apk állományt. Az alkalmazások telepíthetőek a készülék memóriájára, és bizonyos körülmények közt külső SD kártyára is.

>[!hint]+ Android Logcat
>
> - Rendszer Debug Kimenet
> - Beépített rendszerüzenetek is monitorozhatók
> - Beépített Log osztály
> - `Log.i("MyActivity", "Pozíció: " + position);`
> - Átirányítható fájlba is: *>adb logcat>textfile.txt*


---

## [[Android]] alkalmazás komponensek

### [[Android]] alkalmazás felépítése

Egy [[Android]] alkalmazás egy vagy több alkalmazás komponensből épül fel:
- [[Activity]]-k
- Service-k
- Content Provider-ek
- Broadcast Reciever-ek

![[Pasted image 20240926190835.png]]


Minden komponensnek különböző szerepe van az alkalmazáson belül, és bármely komponens önállóan aktiválódhat, akár egy másik alkalmazás is aktiválhatja őket. Az alkalmazást leíró *manifest* állománynak deklarálnia kell a következőket:
 - Alkalmazás komponensek listája
 - Szükséges minimális [[Android]] verzió
 - Szükséges hardware konfiguráció
A nem forráskód jellegű erőforrásoknak (képek, szövegek, nézetek) rendelkezésre kell állnia különböző nyelvű és képernyőméretű telefonokon.

---

#### [[Activity]]-k:

Különálló nézettel és saját [[Felhasználói Felület (Android)|UI]]-al rendelkeznek. Egy alkalmazást sok független [[Activity]] alkot. Egy [[Activity]] akár más alkalmazásból is indítható, nem csak abból, amihez tartozik.

>[!example]+ Például:
>Emlékeztető alkalmazásnál 3 [[Activity]]:
>- ToDo lista
>- Új ToDo felvétele
>- ToDo részletek
>  
>  Kamera alkalmazás el tudja indítani a ToDo felvétele Activityt, és a képet hozzárendelni az emlékeztetőhöz

Az `android.app.Activity` osztályból származik le.

---

### Service-k

A Service komponens egy hosszabb ideig a háttérben futó feladatot jelképez, nincs felhasználói felülete. Például egy letöltő alkalmazás fut a háttérben, amíg egy másik programmal játszunk. Más komponens (*[[Activity]]*) elindíthatja, vagy csatlakozhat (*bind*) hozzá vezérelni.

Az `android.app.Service` osztályból öröklődik.

---

### Content Provider-ek

A Content provider (tartalom szolgáltató) komponens feladata egy megosztott adatforrás kezelése. Az adat tárolódhat fájlrendszerben, [[SQLite]] adatbázisban, weben, vagy egyéb perzisztens adattárban. A Content provideren át más alkalmazások is hozzáférhetnek az adatokhoz, vagy akár módosíthatják is őket.

Az `android.content.ContentProvider` osztályból származik le és kötelezően felül kell definiálni a szükséges API hívásokat.

---

### Broadcast Reciever-ek

A Broadcast Reciever komponens a rendszerszintű eseményekre reagál. Alkalmazás is indíthat broadcastot, ha jelezni akarja, hogy valamilyen művelettel végzett. Nem rendelkeznek saját felülettel, inkább valami figyelmeztetést írnak ki a *status bar*-ra, vagy elindítanak egy másik komponenst.

Az `android.content.BroadcastReciever` osztályból származik le; az esemény egy Intent formájában érhető el.

---

## A Manifest állomány

A *Manifest állomány* az alkalmazás leírója, mely definiálja az alkalmazás komponenseit egy *XML állományban*. Komponens indítása előtt a rendszer a manifest állományt ellenőrzi, hogy definiálva van-e benne a kért komponens. Alkalmazás telepítésekor ellenőrzi a rendszer.

>[!summary]+ Manifest állomány tartalma
>
>- Az alkalmazást tartalmazó java package - *egyedi azonosítóként szolgál*
>- Engedélyek, amelyekre az alkalmazásnak szüksége van
>- Futtatáshoz szükséges minimum API szint
>- Hardware és software funkciók, amit az alkalmazás használ
>- Külső API könyvtárak

---

## [[Erőforrások]] kezelése

Egy android alkalmazás nem csak forráskódból áll, hanem erőforrásokból is, úgy mint *képek*, *hanganyagok*, stb. Emellett [[Erőforrások]] az [[XML]]-ben definiált felületek is: *elrendezés*, *animáció*, *menü*, *szín*, *stílus*. Erőforrásokkal sokkal rugalmasabban változtatható az alkalmazás. Minden erőforráshoz a rendszer automatikusan azonosítót generál, amin keresztül elérhető a forráskódból.

>[!example]+ Erőforráshivatkozás példa
>
>- TFH készítettünk egy *logo.png*-t, és elmentettük a *res/drawable/* könyvtárba
>- Az SDK eszköz előállít egy egyedi erőforrást hozzá mentés után automatikusan.
>- Az azonosító: *R.drawable.logo*
>- Ezzel az azonosítóval lehet hivatkozni bárhol az erőforrásra
>  Az azonosítók az *R.java* állományban tárolódnak (soha ne módosítsuk!)

>[!tip]+ Az erőforráshasználat előnyei
>
>Az egyik legnagyobb előnye, hogy a készülék képességeihez lehet igazítani az erőforrásokat. A könyvtárak után "minősítő"-ket írhatunk, amellyel megadjuk, hogy mely tulajdonságokat teljesülése esetén vegye a rendszer ebből a könyvtárból az erőforrásokat.
>Többnyelvűség támogatására is jó: *strings.xml, res/values/, res/values-fr/, res/values-hu-land/*

