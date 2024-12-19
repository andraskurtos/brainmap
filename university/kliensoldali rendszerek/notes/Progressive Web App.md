---
aliases:
  - pwa
tags:
  - 5semester
  - client
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-19 16:48
---

# Progressive Web Apps

A *PWA* egy web alapú alkalmazás, ami app szerű (főleg UX kérdés), tehát pl. nem görgethető az egész, nem linkel ki, mértezhető, stb.

A PWA-k telepíthetők, indíthatók a bolt/web látogatása nélkül, akár offline is működőképesek. Normál ablakból indulnak, mobilon appként.

Lehetséges benne OS integráció megvalósítása, olyan funkciókhoz, mint:
- megosztás támogatás (forrás és cél)
- push notification
- protocol handlers
- file handlers
- custom title bar

>[!SUMMARY]+ Követelmények
>- HTTPS
>	- Ez nem gond, amúgy is így kommunikálunk
>- Manifest fájl
>	- alkalmazás paramétereit írja le
>- Service Worker
>	- Offline működést oldja meg

## HTML

HTML head-be a következő:

```html
<meta name="theme-color" content="white" />
<meta name="apple-mobile-web-app-title" content="My Chat" />
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="white" />
<link rel="manifest" href="manifest.json" />
<link rel="apple-touch-icon" href="Images/Logo180w.png" />
```

A manifest kötelező, a többi az iOS miatt kell.

## Manifest

*manifest.json*:

```json
{ 
	"short_name": "My Chat",
	"name": "My Chat – Group Chat",
 	"start_url ": "index.html",
 	"display": "standalone",
 	"theme_color ": "white",
 	"background_color": "white",
 	"icons": [
 		{
 			"src": "Images/Logo48.png",
 			"type": "image/png ",
 			"sizes": "48x48"
 		}
 	]
}
```

>[!NOTE]+ manifest.json beállításai
>name: az alkalmazás neve
>start_url: ezt indítja el
>display: böngésző ablakban, vagy anélkül
>theme_color: a böngésző ablakát átszínezi, ha van
>background_color: betöltés közbeni szín
>icons: telepített icon

>[!error]+ Böngészőnként eltér
>Sajnos a támogatás böngészőnként eltér :(
>Szerencsére az unio működik: mindent beállítunk minden módon


## Service Worker

A *service worker* egy külső [[JavaScript|JS]] fájl, ami nincs benne a csomagban, mi írjuk meg. Az alkalmazásunktól függetlenül fut, és akár akkor is futhat, ha nem futtatjuk az alkalmazást, és ha a böngésző nincs elindítva. Háttér szolgáltatás, amit az OS futtat. Feladata az offline működés (cachelés), értesítések kezelése, stb.

> [!NOTE] Web Worker
> Web Worker != Service Worker
>  Többszálúságot biztosít
>  Minden szál az adott alkalmazásban fut
>  Felülethez nem férhetnek hozzá
>  Üzenetekkel kommunikál (mint a SW)
>  Elér pár API-t, amit az SW nem.
>  - pl indexedDB, WebSocket

### Telepítés

```javascript
let reg = await navigator.serviceWorker.register('sw.js');
```

Itt az *sw.js* a Service Worker kódja.
Sikeres register hívás esetén elkezd működni, tehát tudunk push notificationöket fogadni, cachelés elindul.

### fetch esemény

Ha nincs benne a cache-ben, akkor töltse le.

```javascript
self.addEventListener('fetch', event =>
	event.respondWith(
		caches.match(event.request)
			.then(response=>response??fetch(event.request))));s
```

### Életciklusa

Amikor először elindul az alkalmazásunk, az eredeti címről letöltve, még nincs telepítve a *service workerünk*. A böngésző elkezdi letölteni és telepíteni, majd el is indítja, amiről kapunk egy eseményt. Ez a példány nem fog *SW*-t használni, csak a következő indítástól.

![[Pasted image 20241219170721.png]]

Amikor frissítjük a verziót, a régi verzió fut, amíg be nem záródik minden példánya az alkalmazásunknak.

### Service Worker események

Életciklus:
	install: telepítéskor és új verziónál
	activate: működik, régi cache törölhető
Cache:
	fetch: az oldal le akar tölteni valamit
Kommunikáció:
	message: az alkalmazás küldött üzenetet
		ez az egyetlen mód a kommunikációra
Push notification:
	push: értesítés jött
	notificationclick: felhasználó az értesítésre klikkelt
	notificationclose: felhasználó bezárta az értesítést
	pushsubscriptionchange: megszűnik a push notification feliratkozás


## Cache API

> [!NOTE] Cache
> Tárolhatunk adatot helyben: localStorage, indexedDB.
> Akár GB feletti méretű is lehet. Nem feltétlen elég nagy méretű képek, videók tárolására, de a kezdőoldal pl. beleférhet.

A Cache API-t tipikusan SW-ben használjuk. Minden függvénye Promise alapú. Több cache is létezhet egyszerre, melyeket a *caches* globális objektumon keresztül érthetünk el, melynek *open* függvénye megnyitja a megadott cachet.

Cache.addAll mindent letölt:

```javascript
self.addEventListener('install', event =>
	event.waitUntil(caches.open(CACHE_NAME)
		.then(cache=>cache.addAll(urlsToCache))));
```

A régi cache-t törölni kell, ha aktiválódott az új:

```javascript
self.addEventListener('activate', event =>
	event.waitUntil(
		caches.keys().then(cacheNames=> Promise.all(
			cacheNames
				.filter(cacheName => cacheName !== CACHE_NAME)
				.map(cacheName => caches.delete(cacheName))))));
```

### Cache stratégiák

Nincsen jó megoldás. Minden letöltendő fájlról meg kell mondani, benne legyen-e a cache-ben, frissítsük-e, ha igen, mikor. A build tool összeállít egy fájllistát, ezért a dinamikusan előálló tartalom gond lehet. Van pár tervezési minta, a cache stratégiák, melyek közül lekérdezéstípusonként kiválasztjuk a megfelelőt.

#### Mindent Cache-be

Egyszerű, gyors

```javascript
self.addEventListener('fetch',e=>e.respondWith(caches.match(e.request)));
```

Csak offline alkalmazásoknál jó.
Minden fájlnak benne kell lennie, dinamikus fájlok nem lehetnek.
Kezdeti letöltés lassabb, mert minden fájl kell.
Ha egy fájl nincs meg, akkor 404.

#### Mindent hálózatról

Egyszerű, de minek

```javascript
self.addEventListener('fetch',e=>e.respondWith(fetch(e.request)));
```

Azonos működés, mintha nem is lenne, lassabb, mintha nem írtunk volna semmit. Valóságban ezt sosem használjuk.

#### Cache, majd hálózat

Ha megvan, akkor nem töröljük le. Az előző kettőnél több esetben használható. Cache itt nem dinamikus: ha valami nincs benne, akkor azt letöltjük, de nem adjuk hozzá. Ez bizonyos fájlok/kérések esetén fontos, például állandóan változó hírlista.
Ez egy jó megoldás lehet bizonyos appoknál.


#### Hálózat, majd cache

Ha nincs net, akkor a tárold adatot adjuk vissza.
Cél:
- offline is kéne működni
- de ha van friss adat, akkor azt mutassuk
Dinamikus Cache
- a cache-t frissítjük a letöltött adattal
Hátrány: lassú hálózat belassít mindent
- ritkán használható

#### Hálózat és cache

Elindítjuk mindkét lekérést
- cache előbb visszatér, azt adjuk
- majd hálózat visszatér
	- frissítjük a cache-t
	- értesítjük az appot, hogy van új adat

Komplex megoldás
- az appnak is együtt kell működnie
Egyszerűsítési lehetőség:
- nem értesítjük az appot
- legközelebb már friss adatot adunk vissza

## Kommunikáció

### XHR

AJAX - Asynchronous JavaScript and XML
Régi módszer
- az új verzió a fetch
HTTP kérést indít egy szerver felé
Aszinkron módon kap választ
A body-t teljesen egészében mi írjuk
A válasz headerjeit részben olvashatjuk
A válasz body-t teljesen olvashatjuk

Tömörítés
- támogatott: gzip, deflate, brotli (20%-al jobb, mint a gzip)
- Ez a Content-Type headerben van jelezve, és csak akkor jön, ha az Accept-Encoding headerben kérte a böngésző

```javascript
let xhr = new XMLHttpRequest();
xhr.addEventListener("load", ()=>console.log(xhr.response));
xhr.open("GET","http://www.example.org/example.txt");
xhr.send();
```

### fetch

XHR helyett, de nem tud mindent
Promiset ad vissza, lehet awaitelni.

```javascript
let res = await fetch("http://example.com/movies.json");
let obj = await res.json();

fetch("http://example.com/movies.json")
	.then(res=>res.json())
	.then(ojb=>console.log(obj));
```

### WebSocket

Üzenetező alapú keretező protokoll
Kétirányú csatorna jön létre
- Nem szűnik meg csomagonként
- Titkosítás azonos, mint HTTP esetén
Kliens is és szerver is küldhet csomagot
- alacsony késleltetés
Kicsi az overhead: max 8 bájt csomagonként
Ezt használja számos klienst értesítő keretrendszer

Tömörítés van, de csak deflate
Cache nincs
Kapcsolódás HTTP kéréssel indul
	szerver leokézza, és nem zárja be a kapcsolatot
		eredeti TCP-n keresztül megy minden kommunikáció

Nem csak a böngészőkben van implementálva
- Két szerver is kommunikálhat WebSockettel
- De botok nem támogatják

```javascript
let ws = new WebSocket("ws://echo.websocket.org/");
ws.onmessage= e=>console.log(e.data);
ws.onopen() = ()=>ws.send("Hello, World!");
```

## Share API

navigaror.share meghívja az OS beépített megosztáskezelőjét
- meg lehet adni: url, title, text
- csak mobilon csinál valamit
- asztali os-en copy-paste a megszokott mód

feltehetjük appunkat a megosztási listára
- az OS beépített megosztáskezelőjén ott lesz
- manifest-be kell megadni, mi történjen
- el lehet kapni SW-ben
