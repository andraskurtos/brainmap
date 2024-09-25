---
aliases:
  - JS
tags:
  - uni
  - client
  - 5semester
  - web
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-25 11:01
---





# JavaScript

A JavaScriptet **Brendan Eich**, a Netscape mérnöke alkotta meg *Mocha*, majd *LiveScript* néven 1995-ben. 1995 decembere óta JavaScript néven fut, egy licencmegállapodás után. Az eredeti elképzelés egy nem csak hivatásos fejlesztők számára készült szkript nyelv volt, amellyel a [[Java]]t kiegészítve *interaktív weboldalak készíthetők.* 

>[!summary]+ A JavaScript változatai
> - JScript -> A Microsoft által jogi problémák elkerülése végett más néven kiadott dialektus (1996)
> - ECMAScript (1. változat 1996 június) -> szabványosított változat
> - ActionScript -> MacroMedia (Adobe) Flash platform programozására kialakított dialektus
> - TypeScript -> Microsoft által készített bővítmény, mely típusossággal és valódi osztály-alapú objektum orientáltsággal egészíti ki a JS-et.

![[Pasted image 20240925111150.png]]



## JavaScript, [[CSS]] és [[HTML]] összekötése

```html
<!DOCTYPE html>
<html>
	<head>
		<link type="text/css" rel="stylesheet" href="mystyle.css"/>
	</head>
	<body>
		...
		<script type="text/javascript" src="myscript.js"></script>
	</body>
</html>
```

>[!warning]+ A Java és a JavaScript mindenben eltér!
>![[Pasted image 20240925111654.png]]


## A JavaScript nyelv alapjai

>[!info]+ [[HTML]] és a *JavaScript*
>- A `<script>` elemeket mindig explicit záró címkével használjuk.
>- A `<body>`-ba is tehető *JS* hivatkozás --> teljesítmény okokból az oldal végére, a `</body>` elé tesszük.
>- A `<script>` tag segítségével a [[HTML]] fájlba is kerülhet *JS* kód, azonban a kód karbantarthatósága, a funkciók szétválasztása, és teljesítmény okok miatt ez **nem célszerű**.
>- Az elemekhez a [[HTML]]-ben is rendelhetünk eseménykezelőket, de a kód átláthatósága miatt **ez sem célszerű**.

### Egyszerű értékadás

```JavaScript
var szoveg = 'Gipsz Jakab';
szoveg2 = "Gipsz Jakab";
var szam = 10;
var logikai = true;
```

Ha a *var* kulcsszót elhagyjuk, akkor is létrejön a változó, azonban ebben az esetben a *globális névtérben*, a window object-en jön létre.

>[!tip]+ Strict mode
>A var kulcsszó véletlen elhagyásából eredő problémákat a **strict mód** bekapcsolásával lehet elkerülni, mert ilyenkor a var elhagyása hibát eredményez.
>```JavaScript
>
>"use strict"
>var ev = 2016;
>honap = "január" // Hiba
>
>```

A JavaScript nyelv *dinamikusan típusos*, hiszen minden változót a `var` kulcsszóval hozunk létre, azaz *nem adunk meg típust*. Tehát egy változó, amiben sztring érték van, tartalmazhat később számot is.
A JavaScript nyelv *gyengén típusos*, hiszen az összeadás operátor működése változik a *változóban tárolt érték típusától* - stringnél konkatenáció, numbernél összeadás.

```JavaScript
var a = 3;
var b = 2;
alert(a+b); // 5 (number)

a = 'Gipsz';
b = 'Jakab';
alert(a+b); // Gipsz Jakab (string)
```

>[!info]+ Implicit típusos konverzió
> Mivel a *JavaScript* gyengén típusos, ha két változót összeadunk, előfordulhat, hogy egyikben string, másikban number szerepel. Ilyenkor implicit típus konverzió van.
>```JavaScript
>var ev = 2020;
>var honap = 'január';
>alert( ev + honap ) // '2020január'
>
>alert( ev + honap + 1 ); // '2020január1'
>alert( ev + 1 + honap); // '2021január'
>```

>[!question]+ Értékadás nélkül?
>Ha egy változót értékadás nélkül hozunk létre, értéke és típusa is `undefined` lesz.

>[!info]+ == és ===
>
>Az == csak az értéket hasonlítja össze, tehát az 1, mint szám, és az "1", mint sztring egyenlő lesz. Az === egyenlőség viszont a *típus infót is ellenőrzi*, tehát egy szám nem lehet egyenlő egy sztringgel.
>
>```JavaScript
>var ev = 2020;
>if ( ev == '2020' ) { // TRUE
>	alert( "2020 == '2020' igaz" )
>}
>if ( ev === '2020' ) { // FALSE
>	alert( "2020 === '2020' igaz" )
>} else {
>	alert( "2020 === '2020' hamis")
>}
>```

### Konstansok

EcmaScript 6-tól van konstans is. Létrehozásakor az értékét is meg kell adni, és ez később nem módosítható.

```JavaScript
const PI; // SyntaxError: missing = in const declaration

const PI = 3.14;
alert( PI ); // 3.14
PI = 500; // SyntaxError: invalid assignment to const PI
```

### Változók láthatósága

A var-al létrehozott változók az egész függvényen belül láthatók. ES6-tól lehet változót létrehozni `let`-el is. Az így létrehozott változók csak a blokkon belül láthatók.

```JavaScript
for ( var i = 0; i<2; i++ ) {
	let loc = i;
}
console.log(loc); // ReferenceError: loc is not defined
```

### Logikai típusok

>[!tip]+ *JavaScriptben* minden bool-á alakítható!
>![[Pasted image 20240925115353.png]]
>```JavaScript
>var b = !!'valami';
>alert(typeof b); // boolean
>alert(b); // true
>```
>Az alábbi kódrészlettel megoldható, hogy az `alert` csak akkor hívódjon meg, ha **x** értéke nem *truthy*.
>```javascript
>var x = '';
>x && alert('lefutott');
>```

---
## Tömbök és Függvények

### Tömbök kezelése JavaScriptben

Tömböket az alábbi két, egymáshoz ekvivalens módon lehet létrehozni:

```JavaScript
var napok = ['hétfő', 'kedd', 'szerda'];
var evszakok = new Array('tavasz','nyar','osz','tel');

alert (typeof napok); // object
alert (typeof evszakok); // object
```

A tömbhöz *új elemet* a `push()`-al *tudunk adni*, az *utolsó elemet* pedig a `pop()`-al tudjuk *kivenni*.

```JavaScript
napok.push('csütörtök');
napok.push('péntek');
var utolso = napok.pop();
alert(utolso); // 'péntek';
```

A splice(index, howMany \[ , el1, ... elN]) függvénnyel módosíthatjuk a tömböt. Az *index* paraméterben megadjuk, hogy a tömb módosítása melyik elemnél kezdődjön. Ha negatív számot adunk meg, a tömb végéről fog számolni. A *howMany* változó megadja, hogy hány elemet szeretnénk törölni. Az opcionális *el1 ... elN* változók a tömbbe beszúrandó új elemek.

```JavaScript
var szinek = ['piros','sárga','kék'];
var toroltek = szinek.splice(1,2,'fehér','zöld');
alert("Törölt színek:");
for (var t in toroltek) {
	alert(toroltek[t]); // Törölt elemek: sárga, kék
}
alert("Ami maradt:");
for (var sz in szinek) {
	alert(szinek[sz]); // Ami maradt: piros, fehér, zöld
}
```

### Függvények

>[!info]+
>JavaScriptben a függvényekben *nem lehet megadni* a paraméterek típusát, vagy a visszatérési érték típusát. Ha a paraméterek száma híváskor és deklarációkor eltér, *nem okoz gondot*, hiszen minden paraméter opcionális --> ha nem adjuk meg, undefined lesz az értéke. Overload nincsen, tehát ha van két azonos nevű függvény, a később deklarált nyer.

```JavaScript
function kiir(szoveg) {
	alert(szoveg);
}

function osszead(a,b) {
	return a+b;
}

var osszeg = osszead(a,b);
kiir(osszeg); // 5
```

>[!tip]+ Paraméter alapértelmezett értékkel
>ES6 óta lehetőség van alapértelmezett értéket adni egy paraméternek, így ha azt elhagyjuk, *nem undefined lesz*.
>```JavaScript
>function osszeadAlap(a, b=4) {
>	return a+b;
>}
>alert(osszeadAlap(2)); // 6
>```

JavaScriptben a függvény is teljesértékű típus, és mint olyan, megadható paraméterként. A paraméterként kapott függvényt meg tudjuk hívni, azonban figyelni kell, hogy tényleg függvény-e.

>[!example]+ Példa
>```JavaScript
>function mind(tomb, fv) {
>	for (let i in tomb) {
>		fv( tomb[i] );
>	}
>}
>mind(napok, kiir);
>```

---

## Változók Láthatósága

### window objektum

Böngészőben futtatott JavaScript kód esetén a `window` objektum *az oldalhoz tartozó ablak vagy keret*. Mivel felső szintű objektum, sok általános tulajdonság, metódus és esemény *rajta keresztül érhető el.*

>[!question]+ Hol jön létre a változó?
>A `var`-al létrehozott változók, amik nincsenek függvénybe zárba, a window objecten jönnek létre.
>```JavaScript
>var a = 'Szia Világ!';
>alert(window.a);
>```
>A függvények is a window objektumra kerülnek:
>```JavaScript
>function say(name) {
>	alert('Szia ' + name);
>}
>window.say('Világ');
>```
>
>--> Törekednünk kell arra, hogy elkerüljük a globális névtér szennyezését.
>==> Mindig be kell zárnunk a változóinkat egy objektumba.
>--> A [[Modul Tervezési Minta|modul tervezési mintával]] kódunkat minden mástól független
 névtérbe csomagolhatjuk, elkerülve a globális névtér szennyezését.
 >
A `var` kulcsszóval deklarált változók function scopingot használnak, azaz a teljes függvényben elérhetőek. A `let` kulcsszósok azonban block scopingot használnak, tehát csak az adott {} blokkban érhetők el.
 
---

## DOM - Document Object Model

A DOM egy olyan modell, amely leírja a [[HTML]] oldal felépítését egy [[fa]] struktúrában, melynek gyökér eleme a "Document" objektum, ami alatt az oldalon lévő elemek találhatók hierarchikusan.

### Elemek lekérdezése DOM-ból

- Elem lekérdezése tag alapján:
	- document.getElementsByTagName("img")
- Adott CSS osztállyal rendelkező elem:
	- document.getElementsByClassName()
- Adott ID-jú elem:
	- document.getElementById()
- Adott Name-el rendelkező elem:
	- document.getElementByName()
- Tetszőleges elem lekérdezése selectorral
	- document.querySelector('a');
	- document.querySelectorAll('a');

### Elemek létrehozása

A `createElement()` segítségével új [[HTML]] elemeket hozhatunk létre.

```javascript
var para = document.createElement('p');
```

Állítsuk be a szükséges attribútumokat, vagy a tartalmát:

```JavaScript
para.textContent = 'Új paragrafus';
```

Keressünk ki egy [[HTML]]-ben már létező elemet és ahhoz fűzzük hozzá az újat, hogy megjelenjen.

```JavaScript
var sect = document.querySelector('section');
sect.appendChild(para);
```

A lekérdezett adatokat módosítani is lehet.

>[!warning]+ Tulajdonságok CSS-ben és JS-ben
>Ügyeljünk arra, hogy a tulajdonságokat máshogyan írjuk CSS-ben és JS-ben. CSS-ben csupa kisbetűvel és kötőjellel (`background-color`), JS-ben azonban camelCase-el (`backgroundColor`).

---

## Események kezelése

Ahhoz, hogy a JavaScript segítségével dinamikus weboldalakat hozhassunk létre, a felhasználói interakciókat kliens oldalon kezelni kell.
Ehhez számos eseményre tudunk feliratkozni, pl *click, keypress, focus, mouseover, change*.

### Inline eseménykezelő

A [[HTML]]-ből közvetlenül inline fel tudunk iratkozni az adott elem egy-egy eseményére.

>[!example]+ Példa
>```html
><button onclick="alert('click')">Gomb</button>
>```

Ez a megoldás egyszerű, hiszen a gomb létrejöttekor már fel is iratkoztunk a kattintás eseményre. Átláthatóság szempontjából rossz viszont, hiszen a HTML-ből állítjuk a gomb viselkedését.

### Oldal betöltődése esemény

Eseményekzelőt csak akkor tudunk egy elemhez regisztrálni, ha az az elem már létezik. Szükséges tehát egy olyan esemény, ami azt jelzi, hogy a [[HTML]]-ek már betöltődtek. A window *onLoad* eseménye pont erre való.

>[!example]+ Példa
>```JavaScript
>window.onload = function() {
>	alert('Az oldal betöltődött!');
>}
>```

### Eseménykezelő regisztrálása JS-ből

Az onLoad lefutása után szabad csak beregisztrálni eseménykezelőt, mert előtte nem biztos, hogy létezik a [[HTML]] elem.

```JavaScript
// keressük meg a HTML elemet:
var btn = document.querySelector('#myBtn');
// Adjuk meg, hogy melyik eseménykezelőre melyik függvényt szeretnénk regisztrálni
btn.onclick = function() {...}
btn.onclick = kiir;
```

>[!hint]+ Több eseménykezelő regiszrálása
>Az előző példával csak egy eseménykezelőt tudunk megadni, ha újat adunk meg, felülírjuk a korábbit. Ha több eseménykezelőt szeretnénk regisztrálni, a megoldás az `addEventListener()`.
>```JavaScript
>var btn = document.querySelector('#myBtn');
>btn.addEventListener("click", modifyText);
>```
>
>Az eseménykezelők a regisztráció sorrendjében egymás után futnak le.

>[!question]+ Mit jelent a bubble?
>Ha egy fában több elemben is *feliratkozunk mondjuk az onClick eseményre*, akkor először fentről lefelé megkeresi a böngésző, hogy *melyik elemre kattintottak*, meghívja az ott beregisztrált eseménykezelőt, majd ha lefutott, a gyökér elemig felgyűrűzik (**bubble up**). A `stopPropagation()`-al tudjuk leállani a felgyűrűzést.
>
>![[Pasted image 20240925131016.png]]

---

## Állapotkezelési megoldások

### Web Storage (DOM Storage)

Probléma a cookie-kal: 
- korlátozott méret
- minden HTTP kérésben jelen van
	- biztonság, teljesítmény gondok
- több böngészőablakban egy időben nehezen használható

--> megoldás: **Web Storage** ( más néven DOM storage ).

A Web Storage kulcs-érték párok tárolására alkalmas, ahol a kulcs és az érték is csak string lehet. Minden más típusú objektumot a böngészők automatikusan stringre konvertálnak.

A méret korlátozására a böngészőknek kvótákat kell alkalmazniuk, és ha azt elérik, a felhasználótól kérnek felhatalmazást több adat tárolására. A javasolt limit 5MB/origin.

>[!tldr] Cookie vs Storage
>![[Pasted image 20240925141620.png]]


#### Web storage típusai

- *Session storage* esetén az információk csak a böngésző/tab bezárásáig maradnak meg. Az informáicókat csak az adott böngészőablak érheti el.
- *Local storage* esetén az adatok megőrződnek, amíg a felhasználó nem törli azokat. Így az adott domainhez tartozó összes oldal eléri a tárolt adatokat, azonban a felhasználó bármikor kitörölheti --> készen kell rá állni.

>[!tip]+ Local Storage használata
>
>Elem hozzáadása:
>- `localStorage.setItem('myCat','Tom');`
>
>Elem kiolvasása:
>- `const cat = localStorage.getItem('myCat');`
>
>Elem törlése:
>- `localStorage.removeItem('myCat');`
>
>Összes elem törlése:
>- `localStorage.clear();`

>[!example]+ Session Storage példa
>```JavaScript
>// Input mező keresése a HTML-ben ID alapján
>let field = document.getElementById("field");
>
>// Ha a mező korábbi állapotát már elmentettük.
>if (sessionStorage.getItem("autosave")) {
>	// Mező értékének visszaállítása
>	field.value = sessionStorage.getItem("autosave");
>}
>	
>// Feliratkozás a mező értékének változására
>field.addEventListener("change" , function() {
>	// Érték mentése a session storage-ba.
>	sessionStorage.setItem("autosave" , field.value);
>});
>```


#### A DOM storage hiányosságai

A DOM storage csak kis mennyiségű adat tárolására alkalmas és optimalizált, és csak string kulcs-érték párokat tud tárolni. Az adatok között nem lehet keresni, és csak szinkron API van.

---

### Indexed Database API 3.0

Célja nagy mennyiségű adat tárolása kliens oldalon + gyors keresés indexekkel. Fő felhasználási területe a gyorsítótárazás kliens oldalon, teljesen offline is működik.

Az *Indexed Database API* aszinkron API, amiben kéréseket lehet definiálni, amik callback függvényeket hívnak meg siker és hiba esetén. Régen volt szinkron api is. Itt is kulcs-érték párokat tárolunk, de az *érték lehet összetett objektum*, a kulcs pedig az objektum tulajdonsága(i). Tranzakcionális modell.

Mérete általában 10MB-2GB közötti, same origin policy vonatkozik rá. Nyelvfüggő rendezéseket nem támogat, nem tud szerver oldali [[Adatbázis|adatbázissal]] szinkronizálni, és a szabadszöveges keresés nem támogatott, nincs LIKE operátor (mint [[SQL]]-ben).

---

### History API

A HTML5 részeként a WhatWG dolgozta ki, a navigációval kapcsolatos állapotok tárolására. A teljes oldalt nem frissítjük, de a böngésző Back-Forward gombjainak működnie kell. Korábbi megoldás erre az ún. #! (hashbang) URL-ek.

---
###

>[!question]+ Hol tároljuk az állapotot?
>
>*Ha az állapot bookmarkolható:*
>- URL-ben.
>
>*Ha szerver oldalon is szükség van az adatra:*
>- URL, hidden field, cookie
>
>*Ha kis mennyiségű, egyszerű adat:*
>- DOM Storage
>
>*Ha nagyobb mennyiségű adat, vagy bonyolultabb lekérdezések:*
>- IndexedDB
>
>*Ha navigációval összefüggő állapotot kell tárolni*
>- HistoryAPI
>
>**Biztonság** szempontjából nem szabad elfelejteni, hogy:
>- a *tárolt adatokat bárki láthatja* --> titkosítani kell őket
>- *bárki módosíthatja* --> integritásvédelemről manuálisan
 gondoskodni kell
 >
 >**Megbízhatóság** szempontjából:
 >- *az adatokat bárki törölheti* teljes egészében vagy részben --> fallback
 >- a kvóta limitet elérhetjük
 >- a felhasználó bármikor megnyithat több böngésző ablakot 
 
---

## A függvény, mint teljes értékű típus


A JavaScriptben a függvények teljes értékű típusként viselkednek, ami további izgalmas lehetőségeket biztosít.
### closure

>[!example]+ Példa
>```JavaScript
>var kulso = function () {
>	var x = 8;
>	var belso = function () {
>		alert(++x);
>	};
>	return belso;
>};
>
>var b = kulso(); // a belső fv-t adja vissza
>b(); // kiírja, hogy 9
>```

Függvények vannak egymásba ágyazva, de a külső függvény *elérhetővé teszi a külvilág számára a belső függvényt*. A belső függvény ilyenkor megőrzi azt az állapotot, ami létrehozása pillanatában volt. Amikor egy függvény egy belső függvényét láthatóvá teszi a külvilág számára, egy ún. **closure** jön létre, ami nem más, mint *az adott belső függvény, és a hozzá tartozó állapot együttvéve.*

>[!tip]+ Névtelen függvények
>A fenti példában a függvényt akár név nélkül is megadhatjuk:
>```javascript
>var kulso = function () {
>	var x=8;
>	return function () {
>		alert(++x);
>	}
>};
>var b = kulso();
>b(); // 9
>b(); // 10
>```

### self-executing functions

Egy másik érdekes lehetőség JavaScriptben a függvények automatikus futtatása.

>[!example]+ Példa
>```JavaScript
>var fv = function (nev) {
>	alert('Szia ' +nev);
>};
>
>fv('Világ');
>``` 

### a new és a this

Az automatikus futtatás elhagyásával és a new operátor használatával osztály-szerű viselkedést is el tudunk érni.

>[!example]+ Példa
>```JavaScript
>var MyClass = function () {
>	var priValt = 3;
>	var priFv = function () { alert('Privát!'); };
>	return {
>		pubValt: 5,
>		pubFv: function () { alert('Publikus!'); };
>	};
>};
>
>var c = new MyClass();
>c.pubFv();
>alert(c.pubValt);
>```

>[!note]+ A *this* kulcsszó
>A *this* kulcsszó JavaScriptben is létezik, és legtöbb esetben arra az objektumra mutat, amin a függvényt meghívjuk. Az alábbi esetben pl. a függvény a *window*-hoz tartozik, ezért a this a *window*-ra mutat.
>```JavaScript
>var F = function () {
>	this.A = 1;
>};
>F();
>alert(window.A);
>```
>
>Ha az előző kódot kicsit módosítjuk, és használjuk a *new* operátort, megváltozik a viselkedés.
>```javascript
>var f = new F();
>alert(window.A); // undefined
>alert(typeof f); // object
>alert(f.A); // 1
>```
>
>A *new* operátor a megadott konstruktor függvény segítségével létrehoz egy új Object-et, *és beállítja a this-t erre az objektumra.* A *new* operátor ezzel az objektummal tér vissza.

---

## Osztályok

Az ECMAScript6 lehetőséget ad oszályok létrehozására is, a `class` kulcsszó segítségével. Ez a szintaxis egy kicsivel közelebb áll az [[OO]] nyelvekhez, mint a korábban megismert.

>[!example]+ Példa
>```JavaScript
>class Point {
>	constructor(x,y) {
>		this.x = x; this.y = y;
>	}
>	toString() {
>			return '(' + this.x + ', ' + this.y + ')';
>	}
>}
>var p = new Point(25,8);
>console.log(p.toString()); // '(25,8)'
>```



