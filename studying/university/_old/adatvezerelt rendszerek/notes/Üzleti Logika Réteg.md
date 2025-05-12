---
created: 20240901 23:53
tags:
  - 5semester
  - adatvez
  - datadriven
cssclasses:
  - center-images
  - markdown-preview-view
---
# Üzleti logika réteg az Adatvezérelt Rendszerekben

Az üzleti logika réteg adja az alkalmazás "lelkét". Az [[Adatforrások az Adatvezérelt Rendszerekben|adattárolás]], [[Adatelérési réteg|elérés]] és a [[Megjelenítési Réteg|megjelenítési réteg]] is mind azért van, hogy a logikai rétegben megadott funkcionalitásokat nyújthassuk.

A pontos funkciókat annak függvényében alakítjuk ki, hogy milyen alkalmazásról van szó.

**Általánosan ebben a rétegben**

- *üzleti entitások* (business entities)
- *üzleti komponensek* (business components)
- és *üzleti folyamatok* (business workflows) **találhatóak.**

---

### Üzleti Entitások

Az *entitások* hordozzák a kezelt információkat. A problématerület függvényében olyan entitásokat hozunk létre, mint például a **termék és értékelés egy webshopban**, vagy a **kurzus és vizsga a Neptunban**.

---

### Üzleti Komponensek

Az entitások manipulálásáért felelnek a *komponensek*. Ők implementálják a szolgáltatásokat, melyek az alkalmazás funkcióinak alapját képezik. Ilyen funkció például a **termékek listázása**, vagy a **keresés**.

---

### Üzleti folyamatok

A komponensek által megadott alapfunkciókból építjük fel a *folyamatokat*. Egy folyamat legtöbbször több komponens által biztosított alapműveletet is használ. Ilyen például a **rendelés véglegesítése**, ami ellenőrzi a terméket, kiállítja a számlát, elküldi a megerősítést, stb. stb.

---

## Szolgáltatási interfész alréteg

Az üzleti logika legfelső alrétege, feladata, hogy a külső hívó számára *interfészt biztosítson*, az üzleti logika réteg funkcióinak eléréséhez.

Azért említendő külön, mert manapság gyakran több ilyen interfészt is publikálnak az alkalmazások, hiszen többféle megjelenítési réteggel is rendelkeznek (pl webalkalmazás, desktop app, mobilapp). Ezek a UI-ok hasonlóak, de nem teljesen azonosak, így gyakran külön interfészt igényelnek.

Gyakran az alkalmazás rendelkezik saját *API*-al is, ami egy harmadik fél számára teszi elérhetővé az alkalmazás funkcióit.