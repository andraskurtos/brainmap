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
created: 2024-12-02 21:45
---
# Webes Architektúra

>[!question] Mi a webszerver?
>Egy olyan alkalmazás, amely egy adott porton figyeli a beérkező kéréseket. Fel tudja oldani az URL-eket, és ez alapján statikus, vagy dinamikus tartalmat szolgáltatni. A beérkező HTTP kéréseket megérti és kiszolgálja, szabályozza, hogy hogyan mit lehet elérni a weben keresztül.

## URL és fizikai útvonal összerendelése

Ahhoz, hogy bizonyos fájlokat elérhetővé tegyünk a weben, létre kell hozni egy website-ot. A website-nak be kell állítani, hogy
- milyen fizikai útvonalon érhetők el a fájlok
- milyen porton figyeljen a szerver
- kinek a nevében fusson az oldalt kiszolgáló processz
- milyen bejelentkezés szükséges az oldal eléréséhez
- szükséges-e titkosítás?
- stb.

>[!summary]+ Request-Response kommunikáció
>Mindig a kliens kezdeményezi a kommunikációt, a szerver csak válaszol. Ez a pull modell. A kérés egy adott URL-re küldött, megfelelően formázott csomag.
>
>User Agent: a kliens általános megnevezése, bármilyen alkalmazás, ami HTTP kérés tud küldeni

A beérkező kérést a webserver feldolgozza és előállítja a szöveges HTTP válaszüzenetet. **Statikus** tartalmakat szolgál ki jellemzően az *URL -> fájlrendszer* megfeleltetés alapján. **Dinamikus** tartalmat állít elő a kérés paraméterei és az alkalmazás állapota (memória, DB) alapján.

>[!note] Statikus kiszolgálás
>- Statikus kiszolgálás != statikus weboldal
>	- Statikus JS kódból módosítjuk a tartalmat.
>- Statikus weboldal = statikus kiszolgálás
>	- Egyszerű HTML fájlok letöltése
>- Dinamikus weboldal != csak dinamikus kiszolgálás
>	- single page application: statikus HTML és JS fájlokat kell kiszolgálni, amik egy API-ról töltik le az adatokat

## A HTTP állapotmentes

Az egyes **HTTP kérések között nincs állapotmegőrzés**. Ez probléma lehet olyan esetekben, mikor pl. egy webshopnál tudni szeretnénk, ki a belépett felhasználó, aki a kosárba pakol.

A HTTP protokoll **fejlécekkel testreszabható**, és például **cookieval megőrizhető az állapot a kérések közt**. Állapotmegőrzésre használható még rejtett mező, URL parameter, HTTP fejléc.

### Kapcsolat kezelése

Nem a HTTP protokoll kezeli a kapcsolatokat, hanem az alatta levő TCP. Mielőtt a kliens és a szerver HTTP kérés/válasz formában kommunikálni tudna, létre kell hozni a TCP kapcsolatot.

## A kérés és válasz elemei

Metódusok
- GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS, TRACE

A kért erőforrás (resource)
- pl: http://www.aut.bme.hu

Fejléc mezők
- Szerverre, tartalomra, biztonságra és gyorsítótárazásra vonatkozó extra információk

Hibakódok + Hibaüzenetek
- pl: 404 - Not Found

### Metódusok

**GET**: a kért erőforrások letöltése a szerverről
**POST**: adatot küld fel a szerverre a kérés body részében
**PUT/PATCH**: frissíti a megadott erőforrást a szerveren
**DELETE**: törli a megadott erőforrást
**HEAD**: HTTP fejléc lekérdezése a megadott erőforrásról
**OPTIONS:** visszaadja a szerver által támogatott HTTP metódusok listáját
**TRACE**: visszaküldi a kapott kérést (diagnosztika)


#### Metódusok tulajdonságai

**Biztonságos metódus**:
- Csak információ letöltésére szolgál, nincs mellékhatása, nem változtat állapotot a szerveren.
- A kliens újra próbálkozhat
**Idempotens metódus**:
- Többszöri végrehajtása ugyanazt azt a hatást váltja ki, mint az egyszeri.
- Minden biztonságos metódus idempotens is + PUT, DELETE (DELETE nem dobhat hibát, ha az erőforrás nem található)

- POST tipikusan nem idempotens
- PATCH általában nem idempotens, mert a részeleges frissítés megvalósítása lehet olyan, hogy mellékhatása van.

