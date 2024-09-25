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

# MÉLYSÉGKORLÁTOZOTT [[Keresés|KERESÉS]]

A mélységkorlátozott keresés az *utak maximális mélységére egy vágási korlátot ad*. A mélységi levágásnál a keresés *visszalép*. A megoldást, amennyiben létezik és a mélységkorlátnál sekélyebben fekszik, *garantáltan megtaláljuk*. Viszont arra *semmilyen garancia* nincs, hogy a *legkisebb költségű* megoldást találjuk meg. Ha túl kicsi mélységi korlátot adunk meg, még *teljes sem lesz*.

----

## Iteratívan mélyülő keresés

Slate és Atkin **1977**-ben alkotta meg a CHESS 4.5 sakkprogramot. Ez a program mélységkorlátozott keresést alkalmazott, és fontos volt egy jó mélységkorlát meghatározása. A legtöbb esetben azonban lehetetlen jó mélységkorlátot választani, amíg nem oldottuk meg a problémát. Így született meg az ötlet: *egyesítsük a* [[Mélységi keresés|mélységi keresés]] *relatíve alacsony tárhelyigényét a* [[Szélességi keresés|szélességi keresés]] *relatíve alacsony időigényével* (legsekélyebben fekvő megoldás megtalálására).

1. futtassunk mélységi keresést 1-es mélységi korláttal. Ha nincs megoldás:
2. futtassunk mélységi keresést 2-es mélységi korláttal
3. ...

**Nem pazarlóan redundáns?**
--> A legtöbb munka a legalsó keresési szinten van, tehát nem olyan rossz a helyzet.

**Gyakorlatilag ötvözi a mélységi és a szélességi keresés előnyös tulajdonságait**

A [[Szélességi keresés|szélességi kereséshez]] hasonlóan *optimális* és *teljes*, de csak a [[Mélységi keresés|mélységi keresés]] *szerény* memóriaigényével rendelkezik. Viszont bizonyos állapotokat az algoritmus *többször is kifejleszt!*