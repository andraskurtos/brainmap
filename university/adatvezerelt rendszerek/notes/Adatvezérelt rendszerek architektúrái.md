---
created: 20240901 23:53
tags:
  - datadriven
  - adatvez
  - uni
  - 5semester
cssclasses:
  - center-titles
  - markdown-preview-view
  - center-images
---

# Adatvezérelt rendszerek architektúrái
## Mi az *adatvezérelt* alkalmazás?

- szoftver elsődleges *célja* a tartalmazott adatok kezelése
- azért jön létre, hogy adatokat *tároljon, megjelenítsen, manipuláljon*
- **az adat határozza meg, miként működik a szoftver**

___

### **példa:**

- Az adat maga határozza meg a rajta végrehajtható műveleteket (Neptun vizsgajelentkezés).
- Adatvezérelt alkalmazás pl a Neptun, Gmail.

___


## Adatvezérelt rendszer felépítése
### **Háromrétegű architektúra**

Az *adatvezérelt rendszereket* tipikusan *3 rétegű architektúra* szerint építjük fel. Ez az architektúra megkülönbözteti az alkalmazás 3 fő komponenscsaládját / rétegét:

- [[Megjelenítési Réteg|megjelenítési réteg]]
- [[Üzleti Logika Réteg|üzleti logika réteg]]
- [[Adatelérési réteg]]

*Ezen kívül az architektúrához kapcsolódik még:*

- az [[Adatforrások az Adatvezérelt Rendszerekben#1 - ADATBÁZIS|adatbázis]]
- a [[Adatforrások az Adatvezérelt Rendszerekben#2 - KÜLSŐ ADATFORRÁSOK|külső adatforrások]]
- és a [[Rétegfüggetlen Szolgáltatások|rétegfüggetlen szolgáltatások]].

---

Az alkalmazásban egyes komponensek rétegekbe szerveződnek, *minden réteg más funkcionalitásért felel*, ezzel megkönnyítve a fejlesztők munkáját.

Az egyes rétegek meghatározzák azt az *interfészt*, amit a ráépülő rétegek használhatnak. Az *[[adatelérési réteg]]* definiálja, milyen műveleteken keresztül érhető el az *üzleti logika réteg* számára, és az hasonlóan a *megjelenítési* *réteg* számára.

--> Minden réteg **csak az alatta lévővel kommunikál**
--> A rétegek által definiált **interfész mögötti implementáció cserélhető** ==> elősegíti az alkalmazás karbantarthatóságát

---

Gyakran az alkalmazás nem egy helyen, hanem a rétegek mentén több kiszolgálón fut. Például leválaszható a [[megjelenítési réteg]], ami egy *webalkalmazás esetén a böngészőben fut*, vagy az adatréteg, ami *saját kiszolgálót kap teljesítmény okokból.*

Egy jó architektúrájú alkalmazás hosszú életciklus alatt is **karbantartható marad**. 

Általában a kód szervezése is *tükrözi a rétegek felépítését*. Az adott programozási környezet lehetőségeivel az egyes rétegeket *külön projektekbe, csomagokba szokás elhelyezni*, ez egyértelművé teszi az egymásra épülést is, hisz a csomagok közt csak *egyirányú hivatkozás lehetséges*.

![[Pasted image 20240904152616.png]]


---
### **Backend és Frontend**

Más szemszögből nézve megkülönböztetünk *backend* és *frontend* részeket. A frontend nagyrészt a [[Megjelenítési Réteg|felhasználói felület]], illetve annak változatos megjelenítési formái (böngészős alkalmazás, natív mobil app, stb.), a felhasználó ezzel lép kapcsolatba. A backend a háttérben futó rendszer, a *szolgáltatási API-k*, az [[Üzleti Logika Réteg|üzleti logika réteg]], az [[Adatelérési réteg|adatelérés]], [[Adatforrások az Adatvezérelt Rendszerekben#1 - ADATBÁZIS|adatbázisok]].

A frontend nem csak a felhasználó számítógépén jelenik meg, és a frontend technológia függvényéven előfordul, hogy a UI egy részét a backend készíti el. Ezt *szerver oldali renderelésnek* nevezik.

---
![[Pasted image 20241025170036.png]]

---

## Állapotkövetés

Az [[Entity Framework]] többrétegű rendszerekben való állapotkövetéséről az [[Entity Framework#Többrétegű alkalmazásokban|alábbi]] szócikknél találsz információkat.
