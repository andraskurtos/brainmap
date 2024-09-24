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




