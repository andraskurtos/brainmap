---
created: 20240901 23:53
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---
# WEB ALAPÚ ALKALMAZÁSOK

A *web alapú alkalmazások* webes technológiákat használnak, mint például a [[HTML]], [[CSS]], [[JavaScript]]. Asztali és [[Mobilszoftver Platformok|mobilalkalmazások]] is lehetnek, de nem weboldalak / webalkalmazások. Célja a multiplatform lét, hogy a lehető legtöbb eszközön fusson.

Web alapú technológiákkal valósul meg, de alkalmazásként viselkedik, *úgy néz ki, mint egy alkalmazás*, és *úgy is viselkedik*: nem linkel ki, együttműködik a többi alkalmazással, OS integrációval (share, drag&drop, stb.).

---
## Webes technológia

**[[Megjelenítési Réteg|Felhasználói felület]] készítésére**
- [[HTML]] elemek + [[CSS]]
	- Tartalomfogyasztó alkalmazások (Twitter, ...)
		- Ezek lehetnek sima webalkalmazások is
	- Utility és productivity alkalmazások
	- Egyszerűbb játékok
- [[Canvas]]
	- Tipikusan játékok (legnagyobb bevétel mobilon)
	- Esetleg multimédia alkalmazások

**Hogyan lehet hatékonyan fejleszteni HTML-en és CSS-en alapuló alkalmazást?**

---

## Történelem

- **2007**: Steve Jobs vizionálja a webet, mint appot telefonon
- **2010** körül váltak a böngészők futtatókörnyezetté
	- Előtte lassú volt minden, nem volt általános web alapú alkalmazás
- **2010**: NPM - Node Packet Manager
- **2010**: AngularJS, később [[Angular]] váltja
- **2013**: [[React]]
	- **2019**: [[Hooks]]
- **2013**: Electron
	- Böngésző elég gyors
- **2014**-től jelennek meg a csomagolók
	- Webpack (2014), Rollup (2015), Parcel (2017) és társai
	- Fejlesztés közben is képesek csomagolni, akár hot reload
	- A végső csomag optimalizált
		- Tree-shaking
		- Lazy-loading, modulokra bontott
		- Minimalizált
		- Verzionált
- **2014**: Vue
- **2015** PWA
	- ötlet, hogy átvegyük a vezérlést a PWA felett
	- De ekkor még használhatatlan
	- **2015** Visual Studio Code, Webstorm
- **2016**: [[Angular]]
- **2020** ESM csomagolók


---

## Architektúra
![[Pasted image 20240909134931.png]]

### Szerver oldal

Példa: ASP.NET, PHP, JSP
Minden kérést a webrendszer kezel
Bemenet: HTTP kérés
Kimenet: HTTP válaszban [[HTML]] (+[[JavaScript|JS]] +[[CSS]])
- teljes oldal, vagy csak részem
- ha nem teljes oldal, akkor kliensen kell JS a feldolgozáshoz
Működik JS nélkül is
- Ha van JS, akkor lehet validálást, felhasználót segítő apróságokat végezni

---

### Kliens oldal

#### előnyök:
- Jobban skálázódik
	- szerver oldalról egy nagy tétel eltűnik (render)
- Gyorsabb
	- Kliens nem felejt navigálások közt
		- Nem kell újra letölteni az oldal részeit
	- Maga a lekérés is kisebb
		- [[JSON]] kisebb, mint az abból készített [[HTML]]
- Gyorsabbnak tűnik
	- Kliens oldalon lehet animációkkal, stb. úgy tenni, mintha az adat már itt is lenne
- Hozzáfér csak kliens oldalon levő szolgáltatásokhoz
- Aktív kapcsolatot tarthat fenn a szerverrel
	- Frissítheti magát
- Több szerverrel is kommunikálhat
	- Nem kell minden kommunikációnak átmenni saját szerveren
- PWA (Progressive Web Apps)
- A hibrid megoldások (szerver render, de van kliens oldali része is) ezek egy részét tudják

---
### Kliens oldali keretrendszer

pl: [[Angular]], [[React]], Vue, Blazor

- Webszerver statikus oldalakat ad vissza
- Alkalmazásszerver [[JSON]]-okat ad vissza
	- kliens készíti el a [[HTML]] oldalt adatokból
- Nem működik [[JavaScript]] nélkül
- Blazor -> WebAssembly
- Szervert nem terheli a rendszer

---

### Kihívások kliens oldalon

- környezet
	- Heterogén (Windows, Android, iOS)
	- Ismeretlen
	- Ellenőrizetlen
- Böngészők
	- kisebb különbségek lehetnek köztük
- Felhasználó
	- Heterogén
	- Kiszámíthatatlan
	- Türelmetlen
	- ...
- Hibakezelés
	- Hibák detektálása
	- Hibák kezelése
	- Felhasználói visszajelzés?
- Ergonómia
	- Prototipizálás, sketch
	- Felhasználói élmény (UX) tervezés
	- Intuitív felhasználói felületek
	- Reszponzivitás
- Botok
	- **Indexelő crawlerek:**
		- Google, Bing, DuckDuckGo, stb.
		- Linkeket követik
		- Legtöbb nem hajt végre [[JavaScript|JS]]-t
		- Néhány igen
		- Komplex oldalakon hibáznak
			- Lehet/kell segítséget adni, mindegyiknek van leírása
			- Sajnos több száz crawler van
	- **Share link botok**
		- Facebook, Twitter, Skype, Viber, Telegram, ...
		- Egy link miatt elemzik az oldalt, keresnek
			- Képet
			- Címet
			- Leírást
		- Nem keresnek linkeket, nem követik őket
		- Kliens oldali kódot általában nem hajtanak végre
		- Bonyolult oldal esetén hibáznak
			- segíteni kell
			- meta tagek formájában
	- **Szerver oldal**
		- Szerver oldalon renderelt oldal hátrányai nem problémák botok esetén:
			- nem kell validálni
			- nem kell interakció
		- Minden botnak más a fontos:
			- pl képméret függő, hogy a Facebook hogyan teszi ki a linket
			- Több különböző oldalt/variációt kell gyártani
				- akár 3-4
			- Szerver oldali render kell
	- **Amikor nem számít**:
		- Ha az oldal nem indexelhető
			- Intranet site
			- Védett tartalom
			- Alkalmazás
			- Vagy ezek kombinációja
		- Akkor nem kell foglalkozni a botokkal