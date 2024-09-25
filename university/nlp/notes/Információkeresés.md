---
created: 20240901 23:53
tags:
  - 5semester
  - uni
  - nlp
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# INFORMÁCIÓ[[Keresés|KERESÉS]]

## Természetes nyelvű szövegek gépi feldolgozása


### **A számítógép feladata**

- "ügyes" szövegszerkesző, diktafon
- **szöveg és információkeresés** (lokális, internetes)
- tudásbázis és lekérdezés
- természetes nyelvű interfész
- fordítás
- ...

---

### **Milyen adatokkal dolgozunk?**

- **szöveggyűjtemény**
- **keresési minta**
- **betű, szó**, mondat
- strukturált adat
- fogalomtár, ontológia
- **kép, hang, videó**
- ...

---

### **Elemi feladat: szövegkeresés minta alapján

- szövegfüzérek egy halmazára illeszkedő kifejezés
- pl [[Python]] [[Regex]]

---

### **Az egyszerű szövegkeresés teljesítménye**

- mennyire *hatékony* a kereső?
	- milyen gyors a [[Regex|regex]] keresés?
	- Mi a helyzet, ha pl 11 millió szó között keresünk?
- mennyire *jó* a keresés?
	- *visszaadás* - recall
	- *pontosság* - precision

*Mit tehetünk a helyzet javítása érdekében?*


---

### **Információkeresés**
Nehezebb feladat.

Cél: 
- mintaalapú szövegkeresés helyett
- információs igény kielégítése

Pontosság???
- nem szavakra keresünk
- fogalmi kapcsolatok
- jelentés

Ötletek?
- tematikus kulcsszavak statisztikái
- szemantikus annotálás
- "értő" olvasás

----

#### problémái:

- Hatékonyság, pontosság
- Meg tudjuk fogalmazni, mire van szükségünk?
	- elegendő a kulcsszavak felsorolása?
- A számítógép érti a keresett tartalmat?
	- *szemantikus* tárolás - a szöveg korlátozott *"értése"*
		- megjelöljük (XML)
		- értelmezzük (ontológia)
		- kikövetkeztetjük (logika)
	- A *szemantikus web* koncepció:
		- W3C
		- strukturált dokumentumok
			- szemantikus jelölés
			- adatcsere-formátum
			- fogalmi rendszerek
			- logika és következtetés

----

#### hogyan javíthatjuk a keresés minőségét?

- elmozdulunk a szövegtől az információtartalom felé
	"cicakaja" --> "állateledel"

- *Keresőkérés-kiegészítés* (semantic query expension)
	- Kiértékeljük a felhasználó eredeti keresési mintáját
	- Ha túl kevés találat: bővítjük általánosabb fogalmakkal
	- Ha túl sok: szűkítjük speciálisabb szavakkal
- *Relevancia-visszacsatolás* (relevance feedback)
	- Megkérjük a felhasználót a találati lista értékelésére
	- A megerősített találatok kulcsszavaival bővítjük a keresést