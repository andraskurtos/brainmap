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





# TypeScript típusok

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


