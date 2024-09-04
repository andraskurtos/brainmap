---
created: 20240901 23:53
tags:
  - ai
  - 5semester
  - mi
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# ÁGENS

Az *ágens* egy entitás, ami érzékel és cselekszik. Absztrakt módon egy függvényként írható le, amely az *érzékeléseket képzi le cselekvésre*. Minden környezethez és feladathoz a lehető legjobb teljesítményű ágenst keressük.

![[Pasted image 20240904130726.png]]

Az információbegyűjtés eszközei a **szenzorok**, a fizikai hatást kifejtő eszközök pedig a **beavatkozók**. Belül található a **rendszer**, kívül pedig a **környezet**.

---
## cél és trajektória

Az ágens célja a környezetének meghatározott állapotát elérni vagy megvalósítani, ami számára kívánatos. Az érzékelés és cselekvés alatt az ágens egy bizonyos *trajektória* mentén halad. Az ügyes ágens az egymástól különböző trajektóriák közül minél hatékonyabbat választ.

---
## [[Racionalitás]]

Racionális cselekvésnek nevezzük az olyan cselekvést, ami a cél felé irányul. Az *intelligens ágens* racionálisan választja meg a cselekvéseit, és ezek segítségével éri el sikeresen célállapotát. A tökéletes racionalitás azonban *lehetetlen*, hiszen a számítási szükségletek túl nagyok. A megoldás a *korlátozott racionalitás*, ami annyit tesz, mint megfelelően cselekedni akkor is, ha az összes számításra nincs elég idő.

---
## az intelligencia mérhetősége

Amíg a pillanatnyi és a célállapot közt különbség van, szükség van egy mechanizmusra, ami jó *trajektórián tartja az ágenst.* Az ágens feladata az *érzékelésből kiszámítani a cselekvést*. A rendszer intelligenciája ennek a számításnak *"ügyességével"* hozható kapcsolatba.

--- 

## Ágensek tervezése

A környezet nagy mértékben befolyásolja az alkalmazkadó ágens kialakítását, elvárt képességeit.
A környezet lehet:

 - *Teljesen/részlegesen megfigyelhető* => az ágensnek szüksége lehet memóriára
 - *Diszkrét/folytonos* => az ágens lehet, hogy nem képes az összes lehetséges állapot megkülönböztetésére
 - *Sztochasztikus/determinisztikus* => az ágens lehet, hogy több lehetséges forgatókönyvet kell kidolgozzon
 - *Egyedüli ágens/több ágens* => lehet, hogy az ágensnek **véletlenszerűen** kell viselkednie

==> [[TERVKÉSZÍTŐ ÁGENSEK]]
