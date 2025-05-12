---
aliases:
  - felhasználói felület
  - ui
tags:
  - mobweb
  - uni
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 13:36
---





# Felhasználói felület


## Alapfogalmak

- Képernyőméret: 
	- fizikai képátló, az egyszerűség kedvéért az [[Android]] 4 kategóriát különböztet meg: *small*, *normal*, *large*, *extra large*.
- Képernyű sűrűség:
	- A pixelek száma egy adott fizikai területen, tipikusan inchenkénti képpont (*dpi - dots per inch*.
	- Az android 6 kategóriát különböztet meg: *low*, *medium*, *high*, *extrahigh*, *xxhigh*, *xxxhigh*.
- Orientáció: A képernyő orientációja a felhasználó nézőpontjából
	- Álló
	- Fekvő
	- futási időben is változhat
	- lehetőség van rögzíteni.
- Felbontás: képernyő pixelek száma
- Sűrűség független pixel (*Density-independent pixel, dp*):
	- Virtuális pixelegység, amit UI tervezéskor célszerű használni
	- 1 dp = 1 fizikai pixel egy 160 dpi-s képernyőn
	- A rendszer futási időben kezel minden szükséges skálázást --> *px=dp\*(dpi/60)*
- Sűrűségfüggetlen méretezés a szövegekhez: sp

>[!attention]+ Sp vs Dp
>„A dp is a density-independent pixel that corresponds to the physical size of a pixel at 160 dpi. An sp is the same base unit, but is scaled by the user's preferred text size (it’s a scale-independent pixel), so you should use this measurement unit when defining text size (but never for layout sizes).”


---

## UI felépítése

>[!summary]+ View hierarchia
>
>![[Pasted image 20241015141441.png]]
>
>- Minden elem a View-ból származik le
>- Layout-ok:
>	- ViewGroup leszármazottak
>	- ViewGroup is View-ból származik
>- ViewGroup-ok egymásba ágyazhatók
>- Saját View és ViewGroup is készíthető, illetve a meglévők kiterjeszthetők

Layout-ok:
- **LinearLayout**
- RelativeLayout
- **ConstraintLayout**
- AbsoluteLayout
- GridLayout
- ...

---

### Súlyozás Layout tervezéskor

- Megadható egy layout teljes súlyértéke (*weightSum*)
- Elemek súly értéke megadható, és az alapján töltődik ki a layout.
	- **layout_weight** érték
	- *A megfelelő width/height ilyenkor 0 legyen!*
- Hasonló, mint [[HTML]]-ben a %-os méretmegadás.

---

### ConstraintLayout

Összetett, komplex layoutok flat view hierarchiával -> nincs szükség egymásba ágyazott layoutokra. RelativeLayouthoz hasonló. Layout Editor támogatással rendelkezik. Android 2.3-tól támogatott.

#### Áttekintés:

- Pozíció megadásához szükséges:
	- Horizontális és vertikális "szabály" (constraint)
- Minden szabály egy kapcsolat (connection)/igazítás (alignment):
	- Egy másik view-hoz képest
	- Szülőhöz képest
	- Egy láthatatlan sorvezetőhöz képest.
- Attól még, hogy a *LayoutEditorban* jól néz ki, nem biztos, hogy eszközön is jó lesz.
- Android Studio jelzi a hiányzó szabályokat.

![[Pasted image 20241015142209.png]]

>[!hint]+ Constraint lehetőségek:
>- Szülőhöz képest
>- Másik View széleihez képest
>- Másik View alapvonalához képest
>- Guidelinehoz képest

---

### [[Jetpack Compose]] - az új UI framework

A *Jetpack Compose* egy modern toolkit UI készítéshez, amelyben a fejlesztő [[Kotlin]] kódban leírhatja a UI paramétereit, és a Compose motor generálja a felületet. Könnyű a UI frissítése az alkalmazás állapota alapján, valamint [[Erőforrások|erőforrásokat]] ugyanúgy használhat.

>[!success]+ Előnyök:
>- Kevesebb kód
>- Nincs szükség XML layoutokra
>- Nincs szükség UI widgetek készítésére
>- A UI elemek kódból készíthetőek
>- Könnyebb újrafelhasználhatóság
>- Kompatibilis a meglévő UI toolkittel.

---

