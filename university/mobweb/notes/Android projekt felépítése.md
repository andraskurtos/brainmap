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