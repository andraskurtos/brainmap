---
aliases:
  - TS
  - typescript
tags:
  - uni
  - 5semester
  - web
  - client
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-25 15:51
---
# TypeScript


A *TypeScript* egy ingyenes és open-source programnyelv a Microsoft által fejlesztve, amely a [[JavaScript|JavaScriptet]] egészíti ki típusossággal és valódi osztály alapú objektum-orientáltsággal. Gyakorlatilag típusos JavaScript, ahol a típusok opcionálisak, ezért minden [[JavaScript|JS]] egyben *TS* is, de amint beírunk egy típust valahova, már csak *TS*. 

Tudásban a TypeScript == [[JavaScript]], azaz semmivel sem tud többet. Ha kivesszük a típusokat, sima [[JavaScript|JavaScriptet]] kapunk, ugyanúgy fog viselkedni a kód futásidőben. Az egyetlen különbség, hogy *van egy fordítási lépés*. TS esetén ez a lépés kiveszi a típusokat is.
## [[TypeScript Típusok & Modulok|típusok]]

A típust TypeScriptben a változó neve után írjuk.

>[!example]+ Példa
>```ts
>function A(a,b) { // .js vagy .ts fájl is lehetne
>	return a+b;
>}
>
>function A(a: number, b: number) { // csak .ts fájlban lehet
>	return a+b;
>
>}
>```

>[!question]+ Miért fontosak a típusok?
>*Kezdetleges dokumentáció*
>- Sokszor lehet következtetni, hogy mit csinál
>	- névből [[JavaScript|JS]]/TS
>	- Paraméterek nevéből [[JavaScript|JS]]/TS
>	- Paraméterek típusából - csak TS!!
>
>*Tooling*
>- Kódkiegészítés
>	- Kontextusfüggő: típus korlátozza a listát
>- Linter
>
>*Fordításidejű kódellenőrzés*
>- Hasonló linterhez, de sokkal hatékonyabb
>
>*Tesztelés segítése*
>- Típusok esetén a tesztelés költsége jelentősen csökken (akár felére)
>	- A hibák jelentős része nem kerül bele a kódba
>
>*Összességében a típusok csökkentik a költséget*

>[!question]+ Az OO kérdése
>
>A típusok használata nem segít az objektum orientáltságon.
>[[JavaScript|JS]] támogatja az OO irányelveket:
>- Vannak osztályok, egységbe zárás
>- Belső működés elrejtése, absztrakció
>- Öröklés
>- Polimorfizmus - majdnem
>	- Ez nincs [[JavaScript|JS]]-ben, sem TS-ben
>	- Egy függvény viszont több típussal is tud működni
>	- Az eredeti célt el tudja érni
>
>![[Pasted image 20240925161328.png]]

## OOP TS-ben

TypeScriptben támogatottak az osztályok, interfészek, absztrakt osztályok, az öröklés, a láthatóság módosítása, az osztályszintű változók/függvények, és az enum típusok, string literálok, unió és metszettípusok. Nem támogatott azonban a valódi metódus overloading, a valódi többszörös öröklés, és típusonként több konstruktor/azonos nevű függvény létrehozása.

*A legtöbb keretrendszer nem osztály alapú*, mert régen nem voltak osztályok. Komponens alapú fejlesztés, ahol öröklés helyett kompozíciót használunk. TS-től a típusosságot kérjük, osztályokkal külön nem foglalkozunk.

----

## Aszinkron programozás

Egyre több API használ promise-t, ami egy osztály, ami *támogat több feliratkozót, hívás-válasz mintát* (mint egy függvényhívás, de pl ismétlődő eseményekre nem alkalmas), *egységes hibakezelést* (van benne try-catch), és *láncolást* `(.then(valami).then(más)`).

A sima callbackhez képest kényelmesebb, hiszen *mindennek azonos az interfésze*, nem kell tudni, melyik paraméter a callback, és *azonos a hibakezelés is*. Nem tökéletes, ugyanis a kód még mindig callbackekben van.

>[!example]+ Egy példa a setTimeout Promise-ra alakítására
>```ts
>function delay( ms: number )
>{
>	return new Promise(( resolve, reject) =>
>		setTimeout( resolve, ms ) );
>}
>```
>-> Visszaadunk egy Promise-t
>-> Elindítunk egy timert
>-> Amikor lejár, meghívjuk a resolve-ot, ami meghív minden .then-t ami rá van téve.

>[!info]+ async, await
>Ha egy függvény Promise-t ad vissza, beírhatunk elé egy `await`-et, feltéve, hogy `async` függvényben vagyunk.
>
>```ts
>async function fa()
>{
>	await delay(500);
>	console.log("hello");
>}
>```
>
>Az `await` utáni kód a `.then`-be kerül fordításkor

