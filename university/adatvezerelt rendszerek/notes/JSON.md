---
aliases:
  - json
  - Json
tags:
  - programming
  - adatvez
  - 5semester
  - uni
  - datadriven
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
> - Gyakorlati adattípusokra nincs egyértelmű reprezentáció
> 	- pl: dátum
> 	- szabvány nem határozza meg
> 	- külön leírás szükséges a parsoláshoz
> - Biztonsági kockázat
> 	- Tipikus, de nem szerencsés gyakorlat: JSON eredményt [[JavaScript|JS]] motorral végrehajtjuk (*eval()*)

>[!question]+ Mikor használjuk?
>- Backend - vékonykliens kommunikáció
>	- Tömör, rövid
>		- Kevés hálózati forgalom, mobilkliensnek előnyös
>	- [[JavaScript]] tudja parsolni
>		- Webes rendszerekben
>- [[REST]]
>- JSON adatbázisban
>	- [[MS SQL]] Server 2016, Oracle Server 12c
>	- [[MongoDB]]


## JSON schema

A séma leírása, maga is JSON fájl.

> [!example]+ Példa
> 
> ```json
> {
> 	"$schema": "http://json-schema.org/schema#",
> 	"title": "Product",
> 	"type": "object",
> 	"required": ["id", "name"],
> 	"properties": {
> 		"id": {
> 			"type": "number",
> 			"description": "Product identifier"
> 		},
> 		"name": {
> 			"type": "string",
> 		},
> 		"stock": {
> 			"type": "object",
> 			"properties": {
> 				"warehouse": { "type": "number" },
> 				"retail": { "type": "number" }
> 			}
> 		}
> 	}
> }
> ```

## .NET és JSON
System.Text.Json

```csharp

var weatherForecast = new WeatherForecast {
	Date = DateTime.Parse("2019-08-01")
	TemperatureCelsius = 25,
	Summary = "Hot" };

string jsonString =
		JsonSerializer.Serialize(weatherForecast);

WeatherForecast? weatherForecast =
		JsonSerializer.Deserealize<WeatherForecast>(jsonString);s

```


## [[XML]] vs [[JSON]]

![[Pasted image 20241104172945.png]]


## JSON kezelés relációs adatbázisokban

> [!summary] MS SQL Json támogatás
> 
> 