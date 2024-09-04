---
created: 20240901 23:53
tags:
  - ai
  - mi
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# ÁLLAPOTTÉR REPREZENTÁCIÓK
## ÁLLAPOTTÉRGRÁFOK

Az *állapottérgráf* a cselekvés problémájának matematikai reprezentációja. Csomópontjai a világ absztrakt reprezentációi, élei az állapotátmeneteket jelölik, melyek cselekvések következményei. A célteszt a *célállapotok elérését vizsgálja*.
Minden állapot pontosan egyszer fordul elő benne, de teljes egészében csak ritkán felépíthető a memóriában, a tárhelykorlátok miatt.

![[Pasted image 20240904193746.png]]

---

## KERESÉSI FÁK

A [[Keresés|keresési]] fa egy "mi lenne ha?" tervekből és azok következményeiből épült fa. A *kezdőállapota a gyökércsomópont*, a *gyermekek* pedig a *követő állapotoknak felelnek meg*.

Minden csomópont egy állapotot jelöl, és olyan tervekhez tartozik, melyek elérik/tartalmazzák azt az állapotot.

**Legtöbb probléma esetében nem lehet ténylegesen felépíteni a keresési fát.**

----

![[Pasted image 20240904194256.png]]

