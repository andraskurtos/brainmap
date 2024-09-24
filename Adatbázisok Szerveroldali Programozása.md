---
aliases:
  - Szerveroldali Programozás
tags:
  - 5semester
  - adatvez
  - datadriven
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-24 16:55
---





# Adatbázisok Szerveroldali Programozása


Az *adatbázis szerveroldali programozásával* az eredetileg deklaratív [[SQL]] nyelv imperatív eszközökkel egészül ki, mint pl. változók használata, elágazások, eljárások, triggerek, kurzorok. 

>[!summary]+ Definíció
>*Szerveroldali programozás* alatt tehát azt értjük, amikor az adatbázisban nem csak lekérdező és módosító parancsokat hajtunk végre, hanem [[Üzleti Logika Réteg|üzleti logikai]] feladatokat is itt valósítunk meg.


>[!question]+ Miért akarnánk az adatbázisban üzleti logikai feladatokat megvalósítani?
>A [[Adatvezérelt rendszerek architektúrái|rétegzett architektúrában]] egy alsóbb réteg szolgáltatást nyújt a felsőbb rétegek számára, a felsőbb réteg tehát *nem kerülheti ki* az alatta lévőt, feladatát azon keresztül kell elvégezze. Azonban [[Java]]/[[C#]]/[[C++]]/stb. kódbázisban ezt nehezen tudjuk garantálni.
>
>Ha a logika azonban az adatbázisban van megvalósítva, sokkal nehezebb megkerülni azt. Ez abból is adódik, hogy a szerveroldali programozás *olyan eszközöket ad a kezünkbe, amelyek garantálják, hogy logikánk minden esetben végrehajtásra kerül*.


---

## Előnyei:


Ha az üzleti logikát az adatbázisban valósítjuk meg, a következő előnyökkel találkozunk:

- Az adatbázis konzisztenciáért való felelőssége még egyértelműbbé válik. A [[relációs modell]] nagy hangsúlyt fektet a konzisztenciára, de nem minden konzisztenciafeltétel írható le vele közvetlenül.
- Csökkenthető az adatbázisból kifelé irányuló adatforgalom. Az olyan esetekben, mikor az üzleti logika számára kérdeznénk le adatokat, kihagyható az adat utaztatása az [[Adatbázis]] 
