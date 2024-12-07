---
aliases: 
tags:
  - mobweb
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-03 13:43
---

# HTML

A **HyperText Markup Language** egy jelölőnyelv, ami leírja az oldal struktúráját a böngészőnek. HTML elemekből épül fel, ez határozza meg, hogy mit jelentsen, illetve hogyan viselkedjen az adott rész.

## HTML elem felépítése

**Nyitó tag**: az elem neve '<' és '>' között. Nem case-sensitive, de kisbetűvel szokás írni.
**Lezáró tag:** nyitótaggel azonos, de '</'-el kezdődik.
**Tartalom**: A két tag közötti rész, többnyire szöveg.

## Attribútumok

Az attribútumok extra információt adnak az elemhez, pl egyedi azonosítót, nevet, CSS osztályokat, stb. A nyitó tagbe kell őket megadni, szóközökkel elválasztva. Az attribútum neve után = jel, az értékét idézőjelbe tesszük.

>[!warning]+ Probléma
>Egyes elemek különböző böngészőkben különféle módokon jelennek meg -> külön külön ellenőrizzük.

# Hello World

**<!doctype>**: html verziója és típusa
**\<html>**: az oldal gyökéreleme
**\<head>**: az oldal címét, és egyéb metaadatokat állítjuk be. Itt hivatkozunk CSS fájlokra. 
**\<body>**: az oldal tartalma, ami megjelenik a böngészőben. A lezáró tag előtt érdemes hivatkozni a [[JavaScript]] fájlokra.


Oldalváz:
**\<header>**
**\<nav>**
**\<aside>**
**\<section>**
**\<article>**
**\<footer>**

> [!question] Miért jobb a szemantikus oldalváz?
>A \<div id="..."> - nak nincs semmi jelentése. A tagekben viszont több szemantikai információ van, amit a böngészők, keresőmotorok felolvasószoftverek értelmezhetnek. A google pl. a headerökben megadott értéket fontosabbnak tartja, mint ami a footerben van.


## Blokk és Inline elemek

A blokkelemek mindig egy új sorban jelennek meg, és csak egymásba ágyazhatók. Pl: div, paragrafus, lista, navmenü, lábléc.
Inline elemek: A böngésző ugyanabban a sorában, az előtte lévő elem mögött jeleníti meg. Blokk szintű elem tartalmának egy része.



## Komponensek:

### Szűrhető lista: \<datalist>

```html
<datalist id="browsers">
	<option value="Internet Explorer">
	<option value="Firefox">
	<option value="Chrome">
</datalist>
```

### Fájl feltöltés

```html
<input type="file">
<input type="file" multiple>
<input type="file" multiple onchange="handleFile(this.files)">
```

### Zene lejátszása

```html
<audio controls>
	<source src="horse.ogg" type="audio/ogg">
	<source src="horse.mp3" type="audio.mp3">
	A böngésző nem támogatja az audio taget
</audio>
```

### Videó lejátszása

```html
<video controls autoplay>
	<source src="movie.mp4" type="video/mp4">	
	A böngésző nem támogatja a video lejátszását.
</video>
```
