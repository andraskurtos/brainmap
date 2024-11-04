---
aliases:
  - json
  - Json
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-04 15:34
---

# JSON

A JSON (*JavaScript Object Notation*) egy kompakt és olvasható szöveges adatreprezentáció. A nevével ellentétben nem csak [[JavaScript]] ben használható.

Alapelemei:
- Objektum: kulcs-érték párok halmaza
- Tömb: értékek halmaza
- Érték: string, szám, bool, null, objektum, tömb

>[!example]- Példa:
>
> ```json
> {
>	"firstName": "John",
>	"lastName": "Smith",
>	"isAlive": true,
>	"age": 25,
>	"address": {
>		"streetAddress": "21 2nd Street",
>		"city": "New York"
>	},
>	"phoneNumbers": [
>	{ "type": "home", "number": "212 555-1234" },
>	{ "type": "mobile", "number": "123 456-7890" }
>	],
>	"children": [],
>	"spouse": null
>}
> ```

>[!fail]+ Problémák:
> - Nincs komment
> - Byte order mark nem lehet fájl elején