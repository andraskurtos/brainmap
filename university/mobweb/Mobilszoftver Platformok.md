---
created: 20240901 23:53
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# MOBILSZOFTVER PLATFORMOK

## A mobilpiac szereplői
### hálózat operátor

A *hálózat operátor* kiépíti és karbantartja a hálózatot, lehetővé téve a készülékek közti kommunikációt. Ilyen például a **T-Mobil, Yettel, Vodafone**.

---

### szolgáltatók

A *szolgáltatók* különböző szolgáltatást nyújtanak a mobilkészülékek, illetve a hálózat felhasználóinak. Ilyen például a hangátvitel, ami jelenleg még sokszor azonos az [[Mobilszoftver Platformok#hálózat operátor|operátorral]], de egyre inkább szétválik: [[VMNO]] - Virtual Mobile Network Operator.

---

### készülékgyártók

A *készülékgyártók* a mobilkészülékek gyártói, pl. Apple, Samsung, Asus, Huawei, stb.

---

### felhasználók


## Mobilkészülék funkciói

Mire használható egy modern mobilkészülék?
- kommunikáció (hívás, email, chat, stb.)
- szervezői funkciók (naptár, jegyzet)
- webböngészés
- játék
- zene, videó, fénykép, film
- navigáció
- fizetés, azonosítás, kártyák
- VR

----

## Platformok
### Symbian OS - a történet kezdete

A Symbian OS egy kifejezetten mobilkészülékekre fejlesztett operációs rendszer. Hardver erőforrásokban gyenge készülékekre optimalizálták, hogy gyenge processzorral, kevés memóriával és korlátozott üzemidővel (akkumulátor) is működjön. Magas rendelkezésre állásra tervezték, mert az ilyen eszközök sokáig bekapcsolva maradhatnak, a reboot csak ritkán megengedett.

A személyi adatkezelő funkciók, mint például a naptár, a jegyzetek, a névjegyek OS szinten támogatottak. Kommunikációs platformok széles tárházát támogatja.

---
#### fejlesztés

- natív c++: Magát az operációs rendszert is c++-ban írták.
- open C/c++
- Qt, Qt quick
- [[Java ME]]
- [[Python]]
---

### Windows Mobile, Windows Phone

#### Windows mobile

A *Windows Mobile* a Windows mobil mutációja volt, okostelefonokra és PDA-kra. Az operációsrendszerhez hozzátartozik számos ismert alkalmazás mobilverziója is. (Excel mobile, Word mobile, stb.)

A fejlesztés Java ME-ben és .NET CF-ben zajlik.

---
#### Windows Phone 7,8
![[Pasted image 20240909110240.png]]

Könnyen testreszabható, mindig friss homescreen-el rendelkezett, az úgynevezett "live tile"-ok mindig a legfontosabb információkat jelenítették meg (hívás, sms, naptár, időjárás, stb.)

A fejlesztés vagy Silverlight alapon zajlott (c#, VB) -> XAML, vagy natív C++-ban. Olyan népszerű szolgáltatásokat integrált, mint az XBOX Live, vagy az Office.

---

#### Windows Phone 10

Egyesültek az asztali és a mobil OS, emiatt az asztalival azonos elemei vannak: Kernel UI, menük, beállítások, stb.

Unversial Windows Platform alkalmazások -> *egy API set és package* az összes device támogatásához

---
### Meego

A Meego egy Linux alapú operációs rendszer Internettáblákra, Mobilokra, Set-top boxokra. Internetezésre lett kifejlesztve, nagyméretű érintőképernyőkre. A fejlesztés régen csak scratchbox, linux alól, majd Python, C/C++.

Linux alkalmazások könnyen portolhatóak rá. Elsősorban "Linux hackerek" fejlesztettek rá.

---

### Tizen

A MeeGo fejlesztők (Linux Foundation) és az Intelesek is áttértek erre az operációs rendszerre. A Samsung és az Intel beálltak mögé, a Samsungon keresztül már több mint 50 millió eszköz használja ezt az OS-t, TV-k, órák, mobilok, stb.

A Bada platformot is beolvasztotta magába.

Fejlesztés:
- Natív appok:
	- C
	- C++
	- Python
	- Lua
- HTML5 és JavaScript alapú appok
- .NET support
- Android alkalmazások

---

### iOS

Eredetileg iPhone OS az Apple-től, exkluzívan az ő hardvereikre kifejlesztve. Objective C és Swift fejlesztés, XCode, Interface Builder, ...

Apple újdonsága volt: Központilag kontrollált app terjesztés, app modellek

Rengeteg elérhető irodalom és dokumentáció.

Speciális UX ötlet: multi-touch gesztúrák, közvetlen manipulálás. OSX-el közös alapok, de speciális UI.

---

### [[Android]]
---
