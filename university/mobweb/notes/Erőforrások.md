---
aliases: 
tags:
  - 5semester
  - uni
  - mobweb
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 14:00
---





# Erőforrások

### erőforrás típusok

>[!summary]+ Gyakran használt erőforrások
>- Drawable -> kép és dinamikus XML drawable
>- Hangok, videók
>- [[Felhasználói Felület (Android)|felhasználói felület]] leíró
>- Animáció
>- [[CSS|Stílusok]], témák
>- Szöveges erőforrások
>- Bármilyen nyers állomány

### szöveges erőforrások:

A *res/valuse/strings.xml*-ben tároljuk őket. Segítségükkel könnyen megvalósítható a többnyelvűség, valamint paraméterezhetőek:

```XML
<string name ="timeFormat">%1$d minutes ago</string>
```

Használat: `Context.getString(R.strings.timeFormat,14)`

---

### internalizáció / lokalizáció

- Többnyelvűség támogatása
- Nyelvfüggő terület és erőforrások
- Lokalizációt támogató erőforrástípusok:
	- Képek
	- Elrendezések
	- Szöveges erőforrások
- Alapértelmezett könyvtárak: itt keres a rendszer, ha nincs a lokalizációnak megfelelő erőforrás
	- res/drawable
	- res/layout
	- res/value

---

## Futásidejű működés

A megjelenés optimalizálásának érdekében lehetőség van alternatív erőforrások megadására a különböző méretek és sűrűségek támogatásához. Tipikusan különböző layoutok és eltérő felbontású képek definiálása szükséges. A rendszer futási időben kiválasztja a megfelelő erőforrást.

>[!summary]+ Erőforrás választó algoritmus:
>
>- Futás közben egy meghatározott logika alapján választ a rendszer
>- Megkeresi a passzoló erőforrást az erőforrásminősítő alapján
>- Ha nincs az aktuálishoz passzoló, akkor egy kisebb/alacsonyabb sűrűségűt választ.
>- Ha az elérhető erőforrások csak nagyobb képernyőkhöz vannak, mint az eszköz képernyője, hibát dob az alkalmazás.

---

## Sűrűségfüggetlenség

Az alkalmazás akkor is lehet sűrűségfüggetlen, ha a felhasználói felületi elemek a felhasználó szemszögéből megőrzik fizikai méretüket. Nagyon fontos a megtartása.
A képernyősűrűséghez kapcsolódó problémák jelentősen befolyásolhatják az alkalmazás felhasználhatóságát.

>[!tip]+ Legfontosabb tényezők:
>- Használjuk a *wrap-content*, *match_parent*, vagy *dp* egységeket, amikor felületet készítünk!
>- Súlyozás
>- Ne használjunk beégetett pixel értékeket!
>- Ne használjunk AbsoluteLayout-ot!
>- Mindenképp készítsünk különböző kép erőforrásokat az eltérő képernyősűrűségekhez!
>- Szövegek méretéhez érdemes az *sp*-t használni.

