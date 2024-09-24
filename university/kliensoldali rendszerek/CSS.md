---
aliases:
  - Cascading Style Sheets
tags:
  - uni
  - client
  - web
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-24 19:14
---





# CSS

A CSS *(Cascading Style Sheets)* egy nyelv, amely a [[HTML]] elemek designját és megjelenítését szabályozza. Alapvetően *kulcs-érték párokat állítunk be* olyan magától értetődő tulajdonságokra, mint a szín vagy háttér. A szabályokat deklaratívan adhatjuk meg a dokumentum adott részére vonatkozóan. Amíg csak szöveget definiálnánk, minden rendben, de layout definiálásnál bonyolódik a helyzet.


### Inline stílusok

A HTML `<head>`-jében hozhatunk létre stílusokat, melyekben kulcs-érték párokat állítunk be az oldal egyes elemeire. Ezek a stílusok adják meg az oldal design-ját. Az inline stílusokat azért nem szeretjük, mert a *HTML a tartalomért felelős, nem a designért* --> szervezzük a stílusokat külön fájlba.

```html
index.html:

<head>
	<link rel="stylesheet" href="style.css">
</head>
```
```css
style.css:

h1 {
	color: #A4001C;
	font-size: 26px;
}
```

### Alapok:

Egy [[HTML]] oldal egy vagy több CSS fájlt is hivatkozhat. A szabályok *selectorral* kezdődnek, ez megadja, mely elemekre kell őket alkalmazni. A selectoron belül kulcs-érték párok állítják a tulajdonságokat. Egyes selectorok magasabb prioritásúak másoknál, és felülírhatják egymást.

>[!tip] CSS szabályok felépítése
>![[Pasted image 20240924193150.png]]

>[!example]+ CSS példa
>
>```css
>h2 {
>	color: #A4001C;
>	font-size: 26px;
>}
>/* Minden link legyen bordó */
>a {
>	color: #A4001C; 
>}
>/* Kivéve ami a nav-ban van, mert az fekete */
>nav a {
>	/* A specifikusabb selectorok erősebbek */
>	color: black; 
>}
>```

---

### Egyszerű CSS selectorok


![[Pasted image 20240924194456.png]]
![[Pasted image 20240924194622.png]]

A tag, osztály, ld és attribútum selectorokat szóköz nélkül egymás után írva olyan selectort kapunk, ami csak azokat az elemeket választja ki, amire mindegyik rész igaz.

---

### Hierarchikus selectorok:

![[Pasted image 20240924195827.png]]

Univerzális selector:

- az univerzális selectorral az oldalon lévő összes elemet kijelölhetjük
- pl minden elem kerete legyen 1 pixeles szaggatott piros vonal: ```
```css
* {border: 1px dashed red;}
```
- Az univerzális selectort nagyon ritkán használjuk, nagyon gyenge, hiszen minden szabály felülírja. Ha debugoláshoz használjuk, akkor a '!*important*'-ot érdemes rátenni.

---

### Pszeudo osztályok

Az egyes elemek állapotai alapján tudunk megjelenést módosítani.

```css
/* Meglátogatott link: */
:visited { color: red;}

/* Letiltott inputok: */
:disabled { color: gray;}

/* Csak olvasható inputok: */
:readonly { color: gray;}

/* Első gyerek: */
:first-child { color: orange;}

/* Ha fölötte van az egér: */
:hover { color: green;}
```

---

### Pszeudo elemek

A kiválasztott elem egy részét lehet vele testreszabni.

```css

/* Új HTML elem létrehozása első gyerekként: */
section::before { content: ''; }

/* Új HTML elem létrehozása utolsó gyerekként: */
section::after { content: ''; }

/* Kiválasztott elemek (pl: szövegrész) formázása */
::selection { background-color: yellow; }

/* Első betűre egyedi megjelenés: */
::first-letter { font-size: 40px; }
```

----

### A befoglaló négyszög

Az oldalon minden elem egy doboz, és a befoglaló négyszög határozza meg, mekkora helyet foglal el egy elem az oldalon.

>[!info] A befoglaló négyszögek
>![[Pasted image 20240924201415.png]]

Az elemek két fő típusa az *inline* elemek, melyek egymás mellé kerülnek, pl a *span, a, img, stb.*, illetve a *blokkszerű* elemek, melyek új sort kezdenek, pl. a *div, form, stb.*

#### blokk elemek tulajdonságai

- Mindig új soron kezdődnek, és kitöltik szülőjük teljes szélességét.
- Következésképp az egymás utáni blokkelemek (ha a stílus felül nem írja) egymás alá rendeződnek.
- Ez az úgynevezett *"block flow"*
- A blokkok egymásba ágyazhatók, szülő-gyerek viszont létrehozva, de ezen belül is a *block flow* érvényesül.

#### inline elemek tulajdonságai

- Az inline elemek egy soron belül követik egymást
- Leginkább szövegfolyamba illő elemekről van szó
- pl. linkelt szöveg, hangsúlyos szöveg, vagy mondjuk emoji
- a szövegbe belesimulnak, nem kezdenek új sort
- következésképpen a folyam iránya vízszintes, és követi a dokumentum szövegirányát.

#### az egyes típusok által felvehető tulajdonságok

- Az imént említett viselkedés csak az alapértelmezett, minden felülírható
- Hogy egy elem blokk vagy inline jellegű, nem csak a dokumentumfolyambeli viselkedésére van hatással.
	- befolyásolja a rá érvényesülő doboz modellt
	- befolyásolja az általa felvehető CSS tulajdonságok halmazát


Az *inline elemek* nem reagálnak a szélesség, magasság és a függőleges padding / margó beállításokra. Reagálnak viszont a vízszintes rendezésre (`text-align`), sormagasság beállításra (`line-height`), és az azon belüli rendezésre (`vertical-align`). *Blokk elemekre* a `text-align, line-height, vertical-align` tulajdonságok nem hatnak, de *inline típusú* gyerekeik megöröklik.