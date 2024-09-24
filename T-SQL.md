---
aliases:
  - tsql
  - t-sql
tags:
  - 5semester
  - uni
  - adatvez
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-24 17:21
---





# T-SQL


>[!info]+
>A *T-SQL* a [[MS SQL|Microsoft SQL Server]] nyelve, amely a standard SQL utasításokon felül lehetőséget ad [[T-SQL#Változók|változók használatára]], elágazások és ciklusok kezelésére, tárolt eljárások írására, kurzorok használatára, triggerek definiálására, és még sok minden másra.

Nézzük meg a nyelv szintaktikáját példákon keresztül.


### Változók

A változókat használat előtt *deklarálni* kell. Konvenció szerint a változók neve `@` karakterrel kezdődik. Értékadás nélkül az inicializálatlan változó `NULL` értékű.

```sql
DECLARE @num int

SELECT @num
-- NULL
```

Értékadás a `SET` utasítással lehetséges, avagy közvetlenül a deklarációban.

```sql
DECLARE @num int = 5
SELECT @num
-- 5

SET @num = 3

SELECT @num
-- 3
```

A változó scope-ja nem kötődik `BEGIN - END` utasításblokkhoz. A változó *batch*-en (tárolt eljáráson) belül érvényes.

```sql
BEGIN
	DECLARE @num int
	SET @num = 3
END

SELECT @num
-- Ez működik, 3

GO -- új batch-et kezd

SELECT @num
-- HIBA: Must declare the scalar variable "@num"
```

Változónak lekérdezéssel is adhatunk értéket:

```sql
DECLARE @name nvarchar(max)

SELECT @name = Name
FROM Customer
WHERE ID = 1
```

Ha a lekérdezés több sorral térne vissza, az utolsó érték marad a változóban. Ha nem tér vissza eredménnyel, a változó értéke nem változik.

----

### Utasításblokkok, elágazások, ciklusok

Utasításblokkot a `BEGIN - END` pár közé írhatunk:

```sql
begin
	declare @num int
	set @num = 3
end
```

Elágazást `IF - ELSE` szerkezettel készíthetünk:

```sql
DECLARE @name nvarchar(max)

SELECT @name = Name
FROM Customer
WHERE ID = 123

IF @name IS NOT NULL
BEGIN
	PRINT 'Updating email'
	UPDATE Customer
		SET Email = 'agh******@gmail.com'
		WHERE ID = 123
END
ELSE
BEGIN
	PRINT 'No such customer'
END
```

Ciklusokhoz a `WHILE` feltételt és egy `BEGIN-END` utasításblokkot használunk:

```sql
WHILE (SELECT COUNT(*) FROM Product) < 1000
BEGIN
	INSERT INTO Product(Name,Price,Stock,VATID,CategoryID)
	VALUES ('Abc',1,1,3,13)
END
```


----

### Beépített függvények

A T-SQLben számtalan beépített függvény található.

String-kezelő függvények:

```sql

SELECT CONCAT('Happy', 'Birthday!')
-- Happy Birthday!

SELECT LEFT ('ABCDEF',2)
-- AB

SELECT LEN('ABCDEF')
-- 6

SELECT REPLACE('Happy Birthday!', 'day', 'month')
-- Happy Birthmonth!

SELECT LOWER('ABCDEF')
-- abcdef
```

Dátumkezelés:

```sql

SELECT GETDATE()
-- aktuális dátum és idő

SELECT YEAR(GETDATE())
-- dátum év komponense

SELECT DATEPART(day,'10/20/2021')
-- 20

SELECT DATEDIFF(day, '2021-09-28 12:10:09', '2021-11-04 13:45:09')
-- 37
```

Adattípus konvertálás:

```sql

SELECT CAST('12' as int)
-- 12

SELECT CONVERT(int, '12')
-- 12

SELECT CONVERT(int, 'aa')
-- HIBA

SELECT TRY_CONVERT(int, 'aa')
-- NULL

SELECT ISNULL(@a,@b)
-- Eredménye az első argumentum, ha az nem null, különben a második ( akkor is, ha ez is null )
```

---

### Kurzor

A kurzor *egy iterátor, amellyel egy rekordhalmazon tudunk elemenként végiglépni*. Akkor használjuk, amikor egy lekérdezés több elemmel tér vissza, és az *elemeket egyesével szeretnénk feldolgozni*.

Használata több lépésből áll:

1. A kurzort deklarálni kell, majd megnyitni
2. Az iteráció egy ciklussal történik
3. A kurzort bezárjuk és felszabadítjuk

Deklaráció és megnyitás:

```sql
DECLARE kurzornév CURSOR
	[ FORWARD_ONLY | SCROLL ]
	[ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]
	[ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]
FOR lekérdezés
[ FOR UPDATE [ OF oszlopnév [ ,... n ] ] ]
```

Az opcionális elemek jelentése:

- `FORWARD_ONLY`: csak `FETCH NEXT` lehetséges
- `SCROLL`: szabadon lehet előre és hátra is lépni a kurzorban
- `STATIC`: másolatból dolgozik, az eredmények megnyitáskor való állapotát mutatja
- `KEYSET`: megnyitáskori állapot adja a sorokat és sorrendjüket, de a rekord tartalma a `FETCH` pillanatában kerül lekérdezésre
- `DYNAMIC`: minden léptetéskor az aktuális állapotot adja, tehát konkurrens tranzakciók módosításait is láthatjuk.
- `READ_ONLY`: nem updatelhető a kurzor által léptetett tartalom
- `SCROLL_LOCKS`: léptetés zárolja a sorokat, ezzel garantálva, hogy a `FETCH` utáni módosítás sikeres lEsz
- `OPTIMISTIC`: nem zárol, optimista konkurrenciakezelést alkalmaz
- `FOR UPDATE`: updatelhető oszlopok listája.

A deklaráció után a kurzort meg kell nyitni az `OPEN` paranccsal, majd használat után bezárni a `CLOSE` utasítással. Bezárás után a kurzor újra megnyitható, így ha többet már nem használjuk, azt a `DEALLOCATE` paranccsal jeleznünk kell.

Kurzor léptetése:

```sql
FETCH [ NEXT | PRIOR | FIRST | LAST
	    | ABSOLUTE { n | @nvar }
	    | RELATIVE { n | @nvar }
	  ]
FROM cursor_name
INTO @variable_name [ ,...n ]
```

Azt, hogy a `FETCH` utasítás sikeres volt-e, a `@@FETCH_STATUS` váltózó lekérdezésével megtudhatjuk. Értéke `0` lesz sikeres `FETCH` esetén, `-1` sikertelen `FETCH` esetén, és `-2`, amennyiben a lekért sor hiányzik.

---

### Tárolt eljárás, függvény


>[!info]+ 
>A korábbi példákban írt utasításainkat beküldtük a szervernek, amely azonnal végrehajtotta azokat. Lehetőségünk van olyan kódot is írni, amit később bármikor meghívhatunk (mint egy függvény/metódus). [[MS SQL|MSSQL]] szerver esetén ezeket tárolt eljárásnak és tárolt függvényeknek nevezzük.

>[!note]+
>Az *eljárás* és a *függvény* közt annyi a különbség, hogy az eljárásoknak nincs visszatérési értéke, a függvényeknek pedig van. További megkötés az MSSQL platformon, hogy a függvények csak olvashatják az adatbázist, de módosítást nem végezhetnek.


Egy *tárolt eljárást* az alábbi szintaktikával hozhatunk létre:

```sql
CREATE [OR ALTER] PROC[EDURE] eljárás_név
 [ { @paraméter adattípus } ] [ ,...n ]
AS
[BEGIN]
	sql_utasítások [ ...n ]
[END]
```

A `CREATE OR ALTER` létrehozza a tárolt eljárást, vagy ha már létezett, módosítja. Egy tárolt eljárást a `DROP PROCEDURE` utasítással lehet törölni.


Egy *skalár függvényt* a következő módon tudunk létrehozni:

```sql
CREATE [OR ALTER] FUNCTION név
( [ { @paraméter adattípus } ] [ ,...n ] )
RETURNS adattípus
[ AS ]
BEGIN
	utasítások
	RETURN skalár_érték
END
```

Egy *tábla függvényt* pedig így:

```sql
CREATE [OR ALTER] FUNCTION név
( [ { @paraméter adattípus } ] [ ,...n ] )
RETURNS TABLE
[ AS ]
RETURN select utasításs
```

----

### Hibakezelés

Hibák, például egy duplikált adat beszúrása esetén *célszerű lenne jelezni a hibát a hívó számára*. Erre szolgál a strukturált hibajelzés és kezelés. Hiba esetén a `throw` paranccsal dobhatunk hibát. Ezen parancs hatására a kód végrehajtása megszakad és a hívóhoz visszakerül a végrehajtás (ahol a hiba lekezelhető, avagy továbbdobható). A hibának van egy hibaszáma (50000 és 2147483647 között), egy szövege, és egy 0-255 közötti hiba állapot azonosító.

Kivétel dobása: `THROW error_number, message, state`

Kivételkezelés:

```sql
BEGIN TRY
	utasítások
END TRY
BEGIN CATCH
	utasítások/THROW
END CATCH
```

----

### Triggerek

A triggerek speciális eszközök, amelyhez hasonlót máshol nemigen találunk. A triggerek *eseménykezelő tárolt eljárások*, melyek használatával az [[Adatbázis|adatbázisban]] történő különböző eseményekre tudunk feliratkozni és azok bekövetkeztekor kódot futtatni.

>[!abstract]+
>Az alábbiakban kifejezetten DML triggerekkel foglalkozunk. Ezek az adatmódosítás hatására lefutó triggerek. Léteznek más triggerek is, pl rendszereseményekre, ezekkel kapcsolatban lásd a hivatalos dokumentációt.


#### DML triggerek

A triggerek segítségével több, nélkülük nagyon bonyolult feladatot meg tudunk oldani. 

>[!example] 
>Például egy **audit naplózás** feladatnál: ha egy adott táblában módosítás történik, rögzítsünk egy rekordot a napló táblába. Ezt akár C#/Java/Python kóddal is megvalósíthatnánk, azonban ezek megkerülése sokkal egyszerűbb lenne egy felhasználó számára.
>Naplózzuk tehát bármely termék törlését egy napló táblába:
>
>```sql
>
>-- Napló tábla létrehozás
>create table AuditLog([Description] [nvarchar](max) NULL)
>go
>
>-- Naplózó trigger
>create or alter trigger ProductDeleteLog
>	on product
>	for delete
>as
>insert into AuditLog(Description)
>select 'Product deleted: ' + convert(nvarchar, d.Name) from deleted d
>
>```
>A fenti parancsok hatására létrejön a trigger, és az adatbázis minden érintett eseménynél lefuttatja azt. 


#### Instead of trigger

A triggerek egy speciális fajtája az *instead of* trigger. Ilyen triggert táblára és nézetre is definiálhatunk. A táblára definiált instead of trigger, ahogy a neve is sugallja, a *végrehajtandó utasítás helyett* fut le. Tehát pl. törlés esetén a sorok nem kerülnek törlésre, helyette a triggerben definiálhatjuk, hogyan kell a műveletet végrehajtani. 

>[!example]+
>Tipikus felhasználása pl. ha egy *törlést nem akarunk valójában végrehajtani*, csak *töröltnek jelölni* a rekordokat (**soft delete**). 
>```sql
>
>-- Soft delete flag oszlop a táblába 0 (azaz false) alapértelmezett értékkel
>alter table Product
>add [IsDeleted] bit NOT NULL CONSTRAINT DF_Product_IsDeleted >DEFAULT 0
>go
>-- Instead of trigger, azaz delete utasítás hatására a törlés nem hajtódik végre
>-- helyette az alábbi kód fut le
>
>create or alter trigger ProductSoftDelete
	>on Product
	>instead of delete
>as
>update Product
	>set IsDeleted=1
	>where ID in (select ID from deleted)
>
>```


Az **instead of** triggerek másik tipikus felhasználása a nézetek. A nézetbe nyilván nem szúrhatunk új adatot, hiszen az egy lekérdezés eredménye, egy **instead of** triggerrel azonban megadható, mit kell a "nézetbe szúrás" helyett végrehajtani.

>[!example]+
>Nézzünk erre egy példát. A nézetben a termék és áfa táblákból kapcsoljuk össze az adatokat úgy, hogy ne a hivatkozott áfa rekord azonosítója, hanem az áfa százaléka jelenjen meg a nézetben. Ebbe a nézetbe úgy tudunk beszúrni, ha a mögötte levő termékeket tároló táblába szúrunk be:
>```sql
>
>-- Nézet definiálása
>create view ProductWithVatPercentage
>as select p.Id, p.Name, p.Price, p.Stock, v.Percentage
>from Product p join Vat v on p.VATID=v.Id
>
>-- Instead of trigger a nézetre a beszúrás helyett
>create or alter trigger ProductWithVatPercentageInsert
>on ProductWithVatPercentage
>instead of insert
>as
>	-- A beszúrás a Product táblába kerül, minden inserted rekordnak egy új sora keletkezik
>	-- És közben kikeressük a százaléknak megfelelő áfa rekordot
>	-- A megoldás nem teljes, mert nem kezeli, ha nincs még ilyen áfa rekord
>	insert into Product(Name, Price, Stock, VATID, CategoryID)
>	select i.Name, i.Price, i.Stock, v.ID, 1
>		from inserted i join Vat v on v.Percentage = i.Percentage
>-- A trigger kipróbálható a nézetbe való beszúrással
>insert into ProductWithVatPercentage(Name, Price, Stock, Percentage) values ('Red ball', 1234, 22, 27)
>
>```
