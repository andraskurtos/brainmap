---
created: 20240901 23:53
tags:
  - 5semester
  - adatvez
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# Rétegfüggetlen Szolgáltatások az Adatvezérelt rendszerekben

A *rétegfüggetlen szolgáltatások* (crosscutting concerns) olyan szolgáltatások, melyek nem csak egy [[Adatvezérelt rendszerek architektúrái#Adatvezérelt rendszer felépítése|rétegben]] jelennek meg. Az ilyen szolgáltatások implementálásakor igyekszünk elérni, hogy egy közös implementációt használjunk.

---

## Biztonság

A biztonsági szolgáltatások lefedik

- a felhasználó beléptetését (*autentikáció*)
- a hozzáférés ellenőrzését (*autorizáció*)
- valamint a *nyomkövetést és auditálást*

---

### Autentikáció

Az *autentikáció* nem csak a [[Megjelenítési Réteg|felhasználói felületen]] való bejelentkezés. Tipikusan az [[Adatforrások az Adatvezérelt Rendszerekben#1 - ADATBÁZIS|adatbázis]] szerverek felé is szükség van bejelentkezésre --> több rétegben is jelen van.

Bejelentkezésre többféle megközelítést választhatunk. Készíthetünk *saját bejelentkezést*, használhatunk valamilyen *címtáras megoldást*, vagy *OAuth* bejelentkezést.

---
### Autorizáció

A hozzáférésszabályozás megszabja, mely funkcióhoz ki férhet hozzá.

---
### Nyomkövetés és auditálás

A nyomkövetés, auditálás feladata pedig, hogy visszakereshető legyen a rendszerben, hogy ki, mikor, mit csinált.

---

## Üzemeltetés

Az üzemeltetési szempontok elősegítik, hogy a szoftver a fejlesztési ciklus után is ellássa a felhasználók igényeit. Feladatai például az **egységes kivételkezelés**, **megfelelő naplózás** és a **konfiguráció-kezelés**.

---

### Kivételkezelés

Olyan egységes módszert kell kialakítani, ami minden esetlegesen keletkező kivételt megfog. A hibákat mindenképpen *rögzíteni kell*, és emellett a felhasználót is értesíteni kell róluk.

---

### Naplózás és Monitorozás

Az üzemszerű és nem rendeltetésszerű viselkedés lekövetéséhez is szükséges. 

--> **Naplózás**: szöveges formátumú rendszerüzenetek rögzítése egy *log*-ba. 

--> **Monitorozás**: rendszer állapotát meghatározó jellemzők (*key performance indicatorok*) követése.

---

### Konfigurációkezelés

Az alkalmazás működését befolyásoló beállítások tárolása, illetve azok bekötése az alkalmazásba.

Konfiguráció például:

- a **kiszolgálók elérési útvonalai**
- a **megjelenítési rétegben használt háttérszín**

***Alapelv: a beállításokat minél kevésbé égessük be a forráskódba!!***

A konfiguráció történhet például konfigurációs fájlokkal.

---

## Kommunikáció

A *kommunikációs szolgáltatás* a rétegek kommunikációjáért felel. A kommunikáció formájának megválasztása és kezelése nem csak architekturális kérdés, a telepítés mikéntjétől is függ.

Ha egyes rétegek eltérő kiszolgálókon kerülnek elhelyezésre, hálózati kommunikáció kell, míg az azonos kiszolgálón futó rétegek esetében célszerű egyszerűbb megoldásokat alkalmazni.

Manapság szinte minden alkalmazásban megjelenik a *hálózati kommunikáció*, tipikusan az [[Adatforrások az Adatvezérelt Rendszerekben|adatbázis/adatforrások]] irányába. Itt általában HTTP kommunikációt alkalmaznak. Nagy rendszerek esetén gyakori az *üzenetsorok* használata is.

A kommunikáció része továbbá a *titkosítás* is. Ha az egyes rétegek más kiszolgálókon futnak, fontos, hogy megfelelően titkosítsuk a kommunikációjukat. A UI és a szolgáltatási interfészek közt ez általában HTTPS/TLS kommunikációt jelent.