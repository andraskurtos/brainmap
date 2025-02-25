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


Az *inline elemek* nem reagálnak a szélesség, magasság és a függőleges padding / margó beállításokra. Reagálnak viszont a vízszintes rendezésre (`text-align`), sormagasság beállításra (`line-height`), és az azon belüli rendezésre (`vertical-align`). *Blokk elemekre* a `text-align`, `line-height`, `vertical-align` tulajdonságok nem hatnak, de *inline típusú* gyerekeik megöröklik.

#### inline elemek blokkosítása

Bármely *inline* elem *blokk* elemmé alakítható a `display` tulajdonság `block`-ra állításával. Ezen felül egyes CSS propertyk "kiszakítják" az inline elemeket a dokumentumfolyamból, effektíve blokkosítva őket (pl `float`, `position`). Így a `width`, `height` stb. beállíthatóvá válik rajtuk.

---
### Blokk elemek

#### width

Alapértelmezése `auto`, ennek hatására a blokk kitölti szülője szélességét. A legtöbb CSS mértékegységet megeszi, így pl `px`, `%`, `em`, `rem`, `vw`, `vh`...
A `%` a szülő szélességének százalékában értendő, az `em` a szülő betűméretéhez, a `rem` a gyökér (`html`) betűméretéhez képest relatív. A `vw` és a `vh` a viewport aktuális méretéhez képest relatív.

#### height

A `width`-hez hasonlóan `auto` az alapértelmezése, és mindenféle CSS hosszúság beállítható rajta. DE: az `auto` jelentése itt a *tartalmazott elemek* megjelenítéséhez szükséges magasság, tehát nincs helykitöltés, mint a szélesség esetén. A `%` a szülő magasságának %-ában értendő, de ez *nem mindig adja a várt eredményt*.

>[!warning]+ Problémák a %-os magassággal
>A %-os magasság sajnos csak akkor működik, ha a szülő magassága nem `auto` vagy `%`, vagy ha `%`, akkor az ő szülőjére kell igaz legyen az előbbi. %-os magassághoz tehát a fában az elem fölött kell egy fix magasságnak lennie, anélkül, hogy `auto` félbeszakítaná.
>Ha egyáltalán nem szeretnénk fix magasságot használni, egészen a `html` elemig fölfele mindennek kell legyen megadott magassága --> ezt pedig nagyon nehéz teljesíteni
>
>![[Pasted image 20240924215402.png]]


#### margin és padding

A *margó* a blokk köré, a *padding* a blokk külső körvonala és a tartalma közé helyez térközt. Ez egyszerűnek tűnik, de mindkettő váratlan mellékhatásokat tud okozni.

![[Pasted image 20240924215537.png]]

>[!warning]+ A margókban sem bízhatunk..
>Ha vízszintes margók találkoznak, azok összeolvadnak, és csak a nagyobbik lesz látható.
>
>![[Pasted image 20240924215717.png]]
>
>**Ez nem bug, feature!! :(**
>
>-> Hogyan szabaduljunk meg tőle?
>Akadályozzuk meg, hogy a margók összeérjenek, vezessünk be közéjük bordert, paddingot, vagy fura *"háziszabályokat"* pl: margót mindig lefele, paddingot csak felfele definiálunk.


### Inline vagy Blokk elem?

Az inlineban az a jó, hogy az elemek szépen egymás mellé kerülnek, és reagálnak a vertical-align propertyre. A blockban pedig, hogy játszhatunk a margókkal, paddinggal, méretekkel. *Nem lehetne mindkettőből kérni egy kicsit?*

#### display: inline-blokk

Itt az elem az inline flow része marad, de emellett blokk tulajdonságok is állíthatók rajta. Kiválóan alkalmas pl. egy menüben az elemek egymás mellé helyezésére. Nem "hülyíti meg" a környező elemeket, mint a `float`, és `vertical-align` is állítható rajta.

```html
<ul>
	<li>
		<a href="#">home</a>
	</li>
	<li>
		<a href="#">about</a>
	</li> 
</ul>
<main>
	<img src="..." />
</main>
```
```css
ul { 
	font-size: 0;
}
li {
	font-size:1rem;
	background:#cc3f85;
	padding:1em 2em;
	display:inline-block;
}

a {
	text-decoration:none;
	font-family:verdana;
	color:white;
}
```

![[Pasted image 20240924221231.png]]

Mi inline-block alapból?
- A `button` és a `select`, egyes böngészőkben az `input` is.
- Az `img`-t a legtöbb böngésző `inline`-nak mutatja, de `inline-block`-szerűen viselkedik.
- Mindezt a HTML nem definiálja, egyszerűen a böngésző alapértelmezett stíluslapjából jön.

>[!important]+ 
>Tehát minden elemnek, amit stílusozunk, már be van állítva minden CSS tulajdonság, vagy a CSS alapértelmezése, vagy a böngésző alap stíluslapja, vagy valamely más stíluslap által. Bármilyen hatást akarunk tehát elérni, tudnunk kell, *mit írunk felül*.


## Stíluskaszkád

>[!question]+ Honnan jönnek a stíluslapok?
>
>- böngésző default
>- hivatkozás `\<link>` taggel
>- hivatkozás `@import`-tal
>- `<style>` tag
>- `style` attribútum
>
>Ha pedig ugyanarra az elemre több szelektor is illik, és a megadott szabályok közül egy vagy több ugyanarra a tulajdonságra próbál hatni, akkor valamilyen rendszer szerint *el kell döntetnünk, melyik jusson érvényre.*

### Mi alapján számítódik a súlyozás?


#### 1. Fontosság:

Fontos az ami `!important`, minden más nem fontos.

```css
.alert-message {color: red !important; }
```

Főleg a debug folyamat mellékhatásaként, vagy a bootstrap alapértelmezéseivel való bunyózás közben fordulhat elő, hogy ilyesmire vetemedünk. Kényelmes, egyszerű, de gyakran nem működik. De ha működik is, nem helyes, mert karbantarthatósági problémákat okozhatunk vele.

>[!tip]- Házi szabályok az !important használatára
>- Alapvetően nem-nem, soha!
>	- Értsük meg, melyik szabályt nem tudjuk nélküle felülírni, és írjunk "erősebb" selectort, és/vagy
>	- fontoljuk meg, szükséges-e, hogy az előző selector szabálya ennyire erős legyen, és próbáljuk meg azt gyengíteni
>		- LESS vagy SCSS használatakor nagyon könnyű "véletlenül" túl erős szabályokat írni!
>- Ha este 10 van és holnap release...
>	- Ha ez a megoldja a problémát, csináljuk, de írunk ki róla egy taszkot, hogy ezt majd valaki csinálja meg rendesen.
>	- TODO komment nem ér, mert az a világ végéig ott marad.


#### 2. Származás

A **fontosság** és a stíluslap származási helye együtt határozzák meg a szabály prioritását/erősségét. Növekvő sorrendben a böngésző alapértelmezett stíluslapja, normál szabályok a felhasználói stíluslapban, normál szabályok a szerzői stíluslapban, fontos szabályok a szerzői stíluslapban, fontos szabályok a felhasználói stíluslapban.

#### 3. Specifikusság

A fontossággal ellentétben ez nem tulajdonság beállítás, *selector szinten dől el*. Mindig a nagyobb specifikusságú selector lesz az erősebb, tehát ütköző beállítások esetén a specifikusabb selector által definiált érték érvényes.

#### 4. Forrás sorrend

Ha két deklarációnak azonos súlyú a forrása, és azonos a specifikussága, akkor a forrás sorrend dönt, vagyis egyszerűen az, hogy melyik stílust definiálták "lejjebb" a fájlban.
