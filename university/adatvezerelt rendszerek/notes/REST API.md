---
aliases: 
tags:
  - adatvez
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-03 13:35
---
# REST API

A *Representational State Transfer* API egy webes API, amin keresztül adatokat kérdezhet le / módosíthat a kliens. Az API erőforrás (URI) alapú, az adat [[JSON]] -ban vagy [[XML]] -ben utazik. A lekérdezéshez [[Webes Architektúra#Metódusok|HTTP methodokat]] használ. Megvalósításához használhatjuk a [[dotNET Web API|.NET Web API]]-t. [[REST API]]-k hívására .NET környezetben a [**HttpClient**](https://dotnet.microsoft.com/apps/aspnet/apis) osztály használata a legcélszerűbb.

Alapelve: az api nem más, mint címezhető erőforrások halmaza.
	Az erőforrás tipikusan adatentitás.
	A címzés URL-el történik.

A szerveroldalon állapotmentes.

## Metódusok

**GET**: *Lekérdezi* az adott URL-en található erőforrást. A válaszüzenet törzse tartalmazza a kért erőforrás részleteit. A visszaadáson kívül *ne legyen semmi mellékhatása.*

**POST**: *Létrehoz* egy új erőforrást a megadott URL-en. Az új erőforrás adatait az üzenet törzsében adjuk meg. *Nem idempotens, mindig új erőforrást hoz létre.*

**PUT**: *Létrehozza, vagy lecseréli* az adott URL-en lévő adatforrást. A kérés törzsében adjuk meg az erőforrást. *Legyen idempotens*.

**PATCH**: *Részlegesen frissíti* az erőforrást. A kérés törzsében meghatározott módosítások kerülnek végrehajtásra az erőforráson. *Nem idempotens*, visszaadja a módosított objektumot.

**DELETE**: *Törli* az adott URL-en levő erőforrást. *Legyen idempotens*.


## Adatküldés lehetőségek

**URL Path**: 
- erőforrás típus és azonosító
- GET /tasks/23

**URL Query string**
- szűrés, rendezés, lapozás, egyéb opciók
- GET tasks?sort=priority

**Request headers & cookie**
- API kulcs
- Hitelesítési tokenek
- Session ID

**Body**
- Tartalom

## Sorrendezés/szűrés példák

**Feladatok listázása csökkenő prioritással**
	GET /tasks?sort=-priority

**Feladatok listázása csökkenő prioritással +  létrehozási sorrendben**
	GET /tasks?sort=-priority,created_at

**Csak nyitott jegyek listázása**
	GET /tasks?state=open

**Szűrés és sorrendezés kombinálása (lehetne lapozás is)**
	GET /tasks?state=open&sort=-priority,created_at

## Relációk kezelése

A 23 megjegyzéseinek listája:
	*GET /tasks/23/notes*
		vagy
	*GET /notes?task=23* (mint szűrő gondolunk rá)

A 23-as feladat 12-es megjegyzése
	*GET /tasks/23/notes/12*

## Hibakezelés

Használjunk megfelelő HTTP státuszkódot:
- 4xx: kliens oldali hibák (rossz kérést küldött a kliens)
- 5xx: szerveroldali hibák

Adjunk vissza JSON objektumot, mely tartalmazza a hiba leírását.


## Fontosabb HTTP státuszkódok

**GET**:
	200 (OK), 404 (Not Found)

**POST**:
	201 (Created) + location header az új objektumra, 409 (Conflict)

**PUT és PATCH**:
	204 (No Content) vagy esetleg 200 (OK), 404 (Not Found)

**DELETE**:
	204 (No Content) vagy esetleg 200 (OK), 404 (Not Found)


## Biztonság

Mindig használjunk HTTPS-t. 
OAuth2, OIDC - harmadik fél általi azonosítás

Törekedjünk szerver oldali állapotmentességre
- SessionID helyett tokenek használata, mely minden információt tartalmaz
- pl JWT


## REST előnyök

- Nagyon jól skálázható
- Nem köti meg az adatformátumot ([[XML]], [[JSON]], ...)
- Egyszerűség, könnyű használat
- A platformok sokkal szélesebb körében elérhető, mint a [[SOAP]]
	- [[JavaScript]] és [[Mobilszoftver Platformok|mobil]] kliensek is
- Nagyobb teljesítmény


Ma már nagyságrendekkel népszerűbb, mint a [[SOAP]], az egyszerűsége, széleskörű támogatása miatt. Ritkán előfordulhat, hogy a SOAP alkalmasabb, például "heavyweight" vállalati rendszerintegráció esetén.

## Dokumentálása

### Ad-hoc megoldások

Egyes erőforrásokhoz URL-ek + verb-ök + támogatott paraméterek táblázatos megadása, JSON kérés és válasz példák. Pl: [twitter dokumentáció](https://developer.x.com/en/docs/x-api/v1/accounts-and-users/follow-search-get-users/api-reference/get-followers-list)
### OpenAPI (Swagger)

A [[SOAP#WSDL|SOAP WSDL-hez]] hasonló a REST világában. Egy API leíró formátumspecifikáció REST API-khoz, beleértve:
- Elérhető végpontok
- Minden végponthoz az elérhető műveletek
- Paraméterek minden műveletre
- Hitelesítési módszerek
- Licenc, használati feltételek, és egyéb információk

Szemmel is jól olvasható, és géppel is jól feldolgozható nyelv. (YAML és [[JSON]] támogatott).

A SwaggerUI segítségével az OpenAPI leírás alapján interaktív HTML "help" oldalakat generálhatunk az API végpontjaihoz, amelyek hívhatók is.

#### .NET környezetben:

**Swashbuckle.AspNetCore** NuGet csomag segítségével.