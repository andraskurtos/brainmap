---
aliases:
  - TypeScript types
  - typescript típusok
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-25 16:42
---





# TypeScript Típusok

## Alaptípusok

Az alaptípusok a [[JavaScript|JS]] alaptípusok, mint a:

- number
- string
- boolean
- bigint
- symbol
- null
- undefined.

Ezen kívül is van még pár alaptípus, a következők:

```ts
let a: number[]; // tömb
let b: [number, string]; // tuple
enum Color {Red, Green, Blue};
let c: Color; // enum
let d: any; // nincs ellenőrzés
let e: "red" | "green" | "blue"; // string literal
```

---

## Összetett típusok

*Unió:* `string | number`
- Vagy az egyik, vagy a másik
- Nagyon sokat használjuk:
	- Mert azonos neve nem lehet a függvényeknek --> polimorfizmus megoldására jó
	- Nem a fordító dönti el, melyiket kell hívni --> függvényen belül `if`-elünk

*Metszet:* `ObjA & ObjB`
- Minden A-ból és B-ből

---
## Függvények

Default és opcionális paraméterek: 

```ts
function fd(a: string = "hello", b?: string) {}
```

>[!warning]+
>Undefinedot kapunk, ha nincs megadva, vagy kézzel azt adtak át, tehát a default paraméter is lehet undefined. Azonos működés a [[JavaScript|JS]]-el - nem fordul le, ha nem adunk meg egy kötelető paramétert.

---

## Osztályok

Konstruktorban tulajdonság:

```ts
class C {
	constructor(public name: string) {}
}
```

Public, protected, private működik, de *csak fordítási időben*.
\#field is működik, ez futásidőben is.
`readonly`, `static`, `abstract` kulcsszavak
accessors: `get`, `set`.

>[!important] this!
>A *TS* nem oldja meg teljesen a *this* problémát, de segít rajta.
>Nekünk kell megoldani --> minden callbacknél használjuk az arrow function szintaktikát:
>```ts
>setInterval( () =>
>{
>	// itt a this azonos a külsővel
>}, 1000);
>```

---

## Type Guards

A fordító követi a kód logikáját.

```ts
function toS(x: string | number) {
	if (typeof x === "string")
		return x;	
	else
		return x.toFixed();
}
```

Működik `instanceof` esetén is.

----

## Paraméteres típusok - Generics

Használhatunk előre nem ismert típusokat osztályokban és függvényekben is. Több paraméter is lehetséges. Itt a fordító látja, mivel használjuk, nem kell megadni, mint C#-ban vagy C++-ban.

```ts
function concat<T>( a: T, b: T) {
	return a.toString() + b.toString();
}
concat(1, "2"); // Error
```

Kényszerekkel:

```ts
interface HasLength
{
	length: number;
}

function getTotalLength<T extends HasLength>(a: T, b: T)
{
	return a.length + b.length;
}
```

---

## Interfészek - interface kulcsszó

- Objektum tulajdonság
- Objektum függvény
- Objektum konstruktor függvény
- Függvény
- Indexer

```ts
interface HasLenth<T>{
	new(): T,
	length: number;
	getLength(): number;
}
interface Indexable {
	[key: string]: string;
}
interface Action<T>{
	(param1: T);
}
let x: Action<string> =
	s => console.log(s);
```

---

## Strukturálisan típusos

Két változó akkor azonos típusú, ha strukturálisan azonos a típusuk

>[!example]+ Például:
>```ts
>type SoN = string | number
>function FA()
>{
>	let a: SoN = 1;
>	let b: number | string = a;
>}
>
>```
>
>A típus neve nem számít.

>[!tip]+ Ez igaz interfészekre és osztályokra is:
>```ts
>interface IA {
>	a: string;
>}
>interface IB {
>	a: string;
>}
>```
>
>Még függvények is követik a kompatibilitás elvét, trükkös esetekben is:
>```ts
>let x = ( a: string ) => {};
>let y = ( a: string, b: string ) => {};
>y = x; // OK
>x = y; // Error
>```


 ----
# TypeScript Modulok

## Névterek

Egy fordítási egységen belül:

```ts
namespace NS
{
	export class C
	{
	}
}
```

Használata: `/// <reference path="x.ts"/>`
Kód darabolása a cél --> Nagyon hasonló az osztály egységbe záráshoz.

---

## Modulok

```ts
module M
{
	export class C { }
}
```

Ezt csak modul betöltővel lehet használni: `import {C} from 'my-class';`
Fordításnál állíthatjuk, milyen kódot generáljon (CommonJS, RequireJS,...)

