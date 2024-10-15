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
	- CERT.SF: erőforrások listája és SHA-1 hash értékük
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
- Activity-k
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

#### Activity-k:

Különálló nézettel és saját UI-al rendelkeznek. Egy alkalmazást sok független Activity alkot. Egy Activity akár más alkalmazásból is indítható, nem csak abból, amihez tartozik.

>[!example]+ Például:
>Emlékeztető alkalmazásnál 3 Activity:
>- ToDo lista
>- Új ToDo felvétele
>- ToDo részletek
>  
>  Kamera alkalmazás el tudja indítani a ToDo felvétele Activityt, és a képet hozzárendelni az emlékeztetőhöz

Az `android.app.Activity` osztályból származik le.

---

### Service-k

A Service komponens egy hosszabb ideig a háttérben futó feladatot jelképez, nincs felhasználói felülete. Például egy letöltő alkalmazás fut a háttérben, amíg egy másik programmal játszunk. Más komponens (*Activity*) elindíthatja, vagy csatlakozhat (*bind*) hozzá vezérelni.

Az `android.app.Service` osztályból öröklődik.

---

### Content providerek

A Content provider (tartalom szolgáltató) komponens feladata egy megosztott adatforrás kezelése