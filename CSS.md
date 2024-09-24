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

### Egyszerű CSS selectorok


![[Pasted image 20240924194456.png]]
![[Pasted image 20240924194622.png]]
