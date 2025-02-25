---
aliases: 
tags:
  - adatvez
  - uni
  - 5semester
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 17:11
---

# GraphQL

A *GraphQL* egy újabb API-szabvány, a [[SOAP]] / [[REST API|REST]] alternatívája. A REST-nél hatékonyabb, rugalmasabb, legalábbis bizonyos feltételek mellett. A Facebook fejlesztette ki, ma már opensource, és nagy community áll mögötte.

A GraphQL deklaratív adatlekérdezést tesz lehetővé, ahol a kliens pontosan meg tudja határozni, milyen adatokat szeretne megkapni az APItól.

A GraphQL API kiszolgáló **egyetlen végponttal** rendelkezik, melyen pontosan **azokat az adatokat adja vissza, amiket a kliens kér**.

>[!summary]+ Jellemzők
>Hatékony:
>- kliens pontosan meg tudja határozni, mit kér
>- nincs overfetching
>- nincs underfetching
>  
>Rugalmas API:
>- gyors termékfejlesztési iterációk támogatása frontenden
>- ugyanaz az API több klienst, illetve azok újabb és újabb verzióit ki tudja szolgálni.


## Típusos séma

A *GraphQL* erős típusleíró nyelvre épül, mellyel egy szolgáltatás API képességei leírhatók. Az API által elérhető típus egy sémában rögzítésre kerül *SDL* nyelven.

Ez a séma egy szerződésnek tekinthető a szerver és a kliens között, leírja, a kliensek milyen formában érhetik el az adatokat. A séma rögzítése után a frontend és a backend fejlesztőcsapatok egymástól függetlenül végezhetik a munkájukat.

>[!example]+ Típusdefiníció példa
>![[Pasted image 20241215172646.png]]

## Művelettípusok

- Query
- Mutation
	- Adatok módosítására
- Subscription
	- Változtatások előfizetésére

### Query

Lekérdezés és válasz:

![[Pasted image 20241215172538.png]]

#### Query paraméterek

Minden mezőnek lehetnek paraméterei, a sémában ezeket a típusoknál kell megadni. A mi példánkban egy helyen van paraméter:

```sdl
type Query {
	allPersons(last: Int): [Person!]!
}
```

![[Pasted image 20241215172747.png]]


### Adatmódosítás

Mutation a neve, és három fajtája létezik:
- Új adat létrehozása
- Módosítás
- Törlés

A sémában a **Mutation** kulcsszóval kell definiálni a típusukat

```sdl
type Mutation {
	createPerson(name: String!, age: Int!): Person!
}
```

A módosító kéréseket **mutation** kulcsszóval kell kezdeni.

```graphql
//kérés:

mutation {
	createPerson(name: "Alice", age: 35) {
		id
	}
}

//válasz:
{
	"data": {
		"createPerson": {
			"id":"ck3cn8xe70vq3016806asxw6g"
		}
	}
}
```


## Értékelés

>[!success]+ Előnyök
>- Fentebb említve
>- Kliensek számára teljes séma hozzáférhető, letölthető
>- Frontend fejlesztést leegyszerűsíti

>[!error]+ Hátrányok
>- Kiszolgáló logika összetettebb
>- Védelmet kell bevezetni a kiszolgálón, hogy ne terheljék túl a komplex kérések
>- Jó kiszolgáló és kliensoldali könyvtárakra van hozzá szükség (Apollo, HotChocolate, stb.)

- A GraphQL transzportréteget a könyvtárak teljesen elrejtik.
- Mobil és vékonykliensvilágban robbant be először.
- Sokszor praktikusabb, mint a [[REST API]], de nem mindig: egyedileg kell dönteni, melyiket célszerűbb (REST jóval elterjedtebb)
- Folyamatosan fejlődik, de a "hype" lecsengett