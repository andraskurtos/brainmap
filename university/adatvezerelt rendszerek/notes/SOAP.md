---
aliases: 
tags:
  - 5semester
  - adatvez
  - uni
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 14:58
---

# SOAP

A *SOAP* egy [[Webszolgáltatás|webszolgáltatások]] készítésére használatos protokoll, amelyben [[XML]] alapú üzenetek egy-egy művelethívást/választ reprezentálnak. A szolgáltatásoknak van **szabványos leírása** WSDL nyelven. Tipikusan [[Webes Architektúra#URL és fizikai útvonal összerendelése|HTTP]] felett működik. 

>[!example]+ SOAP üzenet példa
> 
> kérés:
> 
> ```
> POST /InStock HTTP/1.1
> Host: www.example.org
> Content-Type: application/soap+xml; charset=utf-8
> Content-Length: nnn
> 
> <?xml version="1.0"?>
> <soap:Envelope
> 	xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"
> 	soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">
> 	
> 	<soap:Header> ...
> 	</soap:Header>
> 	
> 	<soap:Body xmlns:m="http://www.example.org/stock">
> 		<m:GetStockPrice>
> 			<m:StockName>IBM</m:StockName>
> 		</m:GetStockPrice>
> 	</soap:Body>
> </soap:Envelope>
> ```
> 
> válasz:
> 
>```
>HTTP/1.1 200 OK
>Content-Type: application/soap+xml; charset=utf-8
>Content-Length: nnn
><?xml version="1.0"?>
><soap:Envelope
>		xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">
>	<soap:Body xmlns:m="http://www.example.org/stock">
>		<m: GetStockPriceResponse>
>			<m: Price>34.5</m: Price>
>		</m:GetStock PriceResponse>
>	</soap:Body>
></soap:Envelope>
>```

A Soap üzeneteket nem kézzel rakjuk össze, legtöbb platform magasszintű szolgáltatást nyújt. A SOAP üzenetek helyett C# / Java interfészeket és entitásosztályokat írunk (melyek műveletekben szerepelnek paraméterként/visszatérésben). Egy SOAP kérés küldése így már csak egyszerű művelethívás:

```java
decimal price = stockService.GetStockPrice("IBM");
```

## WSDL

A *SOAP* szolgáltatások önleírók, a *WebService Description Language* segítségével, ami egy [[XML]] alapú leíró nyelv. Elsődlegesen a webszolgáltatás által fogadni képes és válaszban küldött üzenetek sémáját definiálja. Tartalmazhat egyéb információkat, például, hogy milyen protokollal érhető el, valamint biztonságos kommunikációra vonatkozó szabályokat.

*A WSDL szintaktika nehezen olvasható/szerkeszthető* --> nem szoktuk

Tipikus folyamat:
- Szolgáltatás
	- A műveleteket platformfüggő módon definiáljuk
	- Egyből a platform által biztosított eszközzel generáljuk
- Kliens
	- A WSDL-ból egy platform által biztosított eszközzel generáljuk a C# / Java interfészt + entitásobjektumokat, így a SOAP kérés egyszerű művelethívás


## Hibakezelés

Szabványos beépített hibakezeléssel rendelkezik a SOAP.

>[!summary]+ Fault
>Minden műveletre megadható, milyen fault-ot eredményezhet. Ez gyakorlatilag egy *speciális válaszüzenet*, ami hasonló a kivételekhez. A SOAP szabvány részletesen definiálja a fault üzenetek szerkezetét (hibakód, szöveg, stb.). A legtöbb modern platformon kivételként kezelhető.


