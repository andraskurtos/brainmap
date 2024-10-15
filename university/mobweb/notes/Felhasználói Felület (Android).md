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


