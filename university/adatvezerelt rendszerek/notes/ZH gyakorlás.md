---
aliases: 
tags:
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-14 11:27
---
	
# Gyakorlás

![[Pasted image 20241114112743.png]]


*Hány nem teljesített megrendelésünk van (a státusz alapján)?*

```sql
select Count(*)
from [Order] o
join Status s on o.StatusID = s.ID
where s.Name != "completed"
```

*Melyek azok a fizetési módok, amit soha nem választottak a megrendelőink?*

```sql
select pm.Method
from [Order] o
right outer join PaymentMethod pm on o.PaymentMethodID = pm.ID
where o.ID is null
```

*Rögzítsünk be egy új vevőt! Kérdezzük le az újonnan létrejött rekord kulcsát!*

```sql
insert into Customer (Name, BankAccount,Login,Password, Email)
values ("Teszt", "Teszt", "teszt", "***", "a@a.com")

select @@IDENTITY
```

*A kategóriák között hibásan szerepel a _Tricycle_ kategória név. Javítsuk át a kategória nevét* Tricycles *-re!*

```sql
update Category
set Name = "Tricycles"
where Name = "Tricycle"
```

*Melyik termék kategóriában van a legtöbb termék?*

```sql
select c.Name, Count(*) as Count
from Category c
join Product p on c.CategoryID = p.ID
group by c.Name
order by Count desc
limit 1
```


*Hozzon létre egy tárolt eljárást, aminek a segítségével egy új kategóriát vehetünk fel. Az eljárás bemenő paramétere a felvételre kerülő kategória neve, és opcionálisan a szülőkategória neve. Dobjon hibát, ha a kategória létezik, vagy a szülőkategória nem létezik. A kategória elsődleges kulcsának generálását bízza az adatbázisra.*

```sql
create or alter proc NewCategory
	@Name nvarchar(40)
	@ParentName nvarchar(40)
as
begin tran

declare @ID int
select @ID = ID
from Category with (TABLOCKX)
where upper(Name) = upper(@Name)
if @ID is not null
begin
	rollback
	throw("Caregory already exists!")
	return
end

if @ParentName is not null
begin
	declare @ParentID int
	select @ParentID = ID
	from Category with (TABLOCKX)
	where upper(Name) = upper(@ParentName)
	if @ParentID is null
	begin
		rollback
		throw("Parent Category does not exist!")
		return
	end
end

insert into Category (Name, ParentCategoryID)
values (@Name, @ParentID)

commit
```

*Írjon triggert, ami a megrendelés státuszának változása esetén a hozzá tartozó egyes tételek státuszát a megfelelőre módosítja, ha azok régi státusza megegyezett a megrendelés régi státuszával. A többi tételt nem érinti a státusz változása.*

```sql
create or alter trigger StatusModificationTrigger
on Order
for update
as
begin tran
update OrderItem oi
set StatusID = i.StatusID
from OrderItem oi
inner join inserted i on i.ID = oi.OrderID
inner join deleted d on d.ID = oi.OrderID
where oi.StatusID = d.StatusID
and i.StatusID != d.StatusID
commit

```

*Tároljuk el a vevő összes megrendelésének végösszegét a Vevő táblában!*

```sql
alter table Customer add Total float

declare CustomerCursor cursor
	for select ID from Customer

declare @CustomerId int
declare @Total float

open CustomerCursor
fetch next from customerCursor into @CustomerId
while @@FETCH_STATUS = 0
begin
	select @Total = Sum(oi.Amount*oi.Price)
	from CustomerSite s
	inner join Order o on o.CustomerSiteId = s.ID
	inner join OrderItem oi on oi.OrderID = o.ID
	where o.CustomerID = @CustomerId

	update Customer
	set Total = ISNULL(@Total, 0)
	where ID = @CustomerId

	fetch next customerCursor into @CustomerId
end

close CustomerCursor
deallocate CustomerCursor

```

*Az előző feladatban kiszámolt érték az aktuális állapotot tartalmazza csak. Készítsünk triggert, amivel karbantartjuk azt az összeget minden megrendelést érintő változás esetén. Az összeg újraszámolása helyett csak frissítse a változásokkal az értéket!*

```sql

create or alter trigger TotalCounter
on Order
for insert, update, delete
as
begin tran

update Customer
set Total = isnull(Total, 0) + TotalChange
from Customer
inner join 

```


*Adott az jobb oldalon látható adatmodell MSSQL szerveren. Az AFA tábla TermekDarab oszlopában az adott áfakulcshoz tartozó termékek darabszámát (ahány féle termék tartozik az áfához) szeretnénk tárolni. Készítsen triggert, ami karbantartja az AFA.TermekDarab mezőt!*

![[Pasted image 20241114122700.png]]

```sql
create or alter trigger AfaKarbantarto
on Termek
for insert, delete
as
begin tran
update AFA
set TermekDarab = TermekDarab+1
from Termek t
inner join AFA a on t.AFAID = a.ID
inner join inserted i on i.ID = t.ID
left outer join deleted d on d.ID = t.ID
where d.ID is null and i.ID is not null

update AFA
set TermekDarab = TermekDarab-1
from Termek t
inner join AFA a on t.AFAID = a.ID
left outer join inserted i on i.ID = t.ID
inner join deleted d on d.ID = t.ID
where i.ID is null and d.ID is not null

commit
```

*Tekintse a fenti adatbázis modellt és annak Entity Framework leképzését (a kulcsok mentén automatikusan generált navigation property-kkel). Írjon C# nyelven Linq-toEntities technológiával kódrészletet, amely törli azon Áfa kategóriákat, amelyhez nem tartozik termék.*

```csharp
var afaquery = db.AFA.Where(a=> !a.Termeks.Any()).ToList();
db.AFA.RemoveRange(afaquery);
db.SaveChanges();
```

*Nevezze meg, az objektum-relációs leképzés mire képezi le az objektumorientált világ alábbi fogalmait a relációs adatbázisban!*
*osztály:* tábla
*objektum példány:* rekord/sor
*asszociáció:* kapcsolat
*osztály tagfüggvénye:* -

*Mire szolgál Entity Framework esetén az Include függvény? Mikor célszerű használni?*

A táblához kapcsolódó táblák előtöltésére használható, a megadottt kapcsolatokat ekkor automatikusan betölti az EF. akkor célszerű használni, ha szükségünk lesz a kapcsolat által megadott tábla elemeire.


*Mik a szerver oldali programozás előnyei? Adjon meg kettőt. Az egyes aspektusokat indokolja, a túl általános kifejezéseket, mint a „gyorsabb”, „jobb” kerülje!*

1. megkerülhetetlen: ha interfészként férünk hozzá az adatbázishoz, a szerveroldalon megvalósított funkciók kikerülhetetlenek lesznek az üzleti logika számára. Ha a naplózást pl az üzleti logikában valósítanánk meg, akkor a naplózást kikerülve is módosíthatnánk az adatokon.
2. biztonságos: használatával elkerülhető fölösleges adatok utaztatása a hálózaton, ami növeli a biztonságot.

*. Döntse el, hogy az alábbi állítások igazak-e! Jelölje őket I és H betűvel! Helytelen válaszért -1 pont jár, üresen hagyott válasz 0 pont. A feladatrész összege nem lehet 0-nál kevesebb.*

**I** Az adatszótárban tároljuk a többnyelvű alkalmazásunk lefordítandó szövegeit.

**H** A lekérdezés-optimalizálás önállóan működik, az SQL parancsot író nem tudja befolyásolni.

**I** A lekérdezések újraírásával, átfogalmazásával el tudjuk érni, hogy hatékonyabb legyen a végrehajtásuk.

**I** A hierarchikus indexekben számít az oszlopok sorrendje.

**I** Létezik olyan lekérdezés, amelyet csak index használatával ki tud értékelni a rendszer, anélkül, hogy a táblát felolvasná.


*Az alábbi json dokumentumban szintaktikai hibák vannak. Húzza alá a hibás részeket, és javítsa őket!*
```json
{
	”version”: 1.0,
	”name”: "Adam",
	”hobbies”: [ "football", "card games"]
}
```

*Adj hozzá egy új oszlopot a `Customer` táblához `PasswordExpiry` néven, ami egy dátumot tartalmaz:.*

```sql
alter table Customer
add PasswordExpiry datetime
```

*1. Készíts egy triggert, amellyel jelszó változtatás esetén automatikusan kitöltésre kerül a `PasswordExpiry` mező értéke. Az új értéke a mai dátum plusz egy év legyen. Az értéket a szerver számítsa ki. Ügyelj arra, hogy új vevő regisztrálásakor (_insert_) mindig kitöltésre kerüljön a mező, viszont a vevő adatainak szerkesztésekor (_update_) csak akkor változzon a lejárat dátuma, ha változott a jelszó. (Tehát pl. ha az email címet változtatták csak, akkor a lejárat ne változzon.) A trigger csak a beszúrt/frissített rekorddal törődjön (tehát nem a tábla teljes tartalmára kell frissíteni a dátum oszlopot)! A feladatban készülnöd kell arra is, hogy egyszerre több rekord módosul.*

```sql
create or alter trigger UpdatePasswordExpiry
on Customer
for insert, update
as
begin
update Customer
set c.PasswordExpiry = DATEADD(year,1,GETDATE())
from Customer c
inner join inserted i on i.ID = c.ID
left outer join deleted d on d.ID = c.ID
where (d.ID is null) or i.Password != d.Password
end
```

*Hozz létre egy tárolt eljárást `cancel_invoice` mely két paramétert fogad: a vevő nevét `name` néven, és a megrendelés azonosítóját `orderId` néven.
A tárolt eljárás ellenőrizze le, hogy van-e a megadott adatokkal számla, ha nincs, dobjon kivételt. A kivétel `error_number` értéke legyen 51000.
Ha az adatok jók, akkor a tárolt eljárás vegye az összes számlán szereplő terméket, nézze meg, hogy mennyit vettek belőlük, és a megrendelt mennyiséget adja hozzá a raktárkészlethez. (TIPP: az adatok összeszedéséhez több tábla, esetleg kurzor is kellhet).*

```sql
create or alter proc cancel_invoice
	@name varchar(40)
	@orderId int
as
begin
begin tran
declare @InvoiceId int
select @InvoiceId = i.ID
from Invoice i
where i.CustomerName = @Name
and i.OrderID = @orderId

if @InvoiceId is null
begin
	rollback
	throw("Nincs ilyen számla", 51000)
	return
end

declare cur_order cursor
for (select oi.ID
	from OrderItem oi
	where oi.OrderID = @orderId)

declare @OrderItemID int
open cur_order
fetch next from cur_order into @OrderItemId
while @@FETCH_STATUS == 0
begin
	declare @Amount int
	declare @ProductId int
	
	select @Amount = oi.Amount, @ProductId = oi.ProductID
	from OrderItem oi
	where oi.ID == @OrderItemId

	update Product
	set Stock = Stock + @Amount
	where ID = @ProductId
	fetch next from cur_order into @OrderItemId
end	

close cur_orderitem
deallocate cur_orderitem

commit tran
end
```

*Ha egy adatbázis nem képes egyedi azonosító / elsődleges kulcs generálására, akkor azt a többrétegű architektúrában hol kell generálni, és miért ott?*

Szerver oldalon az üzleti logikában. Így az adatbázistól függetlenül működhet a rendszer.

*A keresés, szűrés funkció a többrétegű architektúrában eltérő módokon valósítható meg. Lehet csak és kizárólag a [[megjelenítési réteg]] felelős érte, de bízhatjuk ezt az adatbázisra/adatelérési rétegre is. Válassza ki az egyik alternatívát és érveljen mellette, mikor praktikus azt választani.*

A keresést érdemes az adatbázisra bízni, ha nagy mennyiségű adattal dolgozunk, vagy komplex keresési logikát végzünk, hiszen az nagy mennyiségű adat gyors keresésére van optimalizálva. Elkerülhetjük vele továbbá, hogy fölösleges adatok továbbítódjanak a hálózaton, ezzel növelve a biztonságot és a teljesítményt.

*Adott az alábbi JSON példának megfelelő dokumentumokat tartalmazó gyűjtemény MongoDB-ben. Írjon C# nyelven a tanult megközelítéssel 1-1 utasítást a megadott feladatra. A gyűjtemény a collection nevű IMongoCollection\<T> típusú változóban elérhető. Emlékeztető: a szűréshez és módosításhoz használható a Builders\<T>. Filter és Builders\<T>. Update factory segédosztály.)
Minta dokumentum, Product osztály:*
```json
{
	"name": "Apple",
	"price": 120,
	"categories": [ "fruits", "on sale" ]
}
```
*a) Listázza az 1000-nél olcsóbb (price) elemeket*

```csharp 
var result = 
collection.Find(Builders<Product>.Filter.Lt(x=>x.Price,1000));
```

*Emelje meg az „Apple" nevű elemek árát duplájukra. Atomi módosítást használjon!  
(Tipp: a szorzás operátor neve ,,Mul".)*

```csharp
collection.UpdateMany(Builders<Product>.Filter.Eq(x=>x.Name, "Apple"),
Builders<Product>.Update.Mul(p=>p.Price,2));
```

*1. Készíts egy `DbVat` osztályt a `VAT` tábla leképzésére az `ef` névtérbe a `DbProduct` -hoz hasonlóan. Ne felejtsd el felvenni a DbSet property-t a `ProductDbContext` -be `Vat` néven.*

```csharp
namespace ef
{
	[Table("Product")] public class DbProduct
	{
		[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
		public int ID { get; set; }
		public string Name { get; set; }
		public double Price { get; set; }
		public int Stock { get; set; }
		[ForeignKey(nameof(DbVat))]
		public DbVat Vat {get; set;}
	}
	
	[Table("Vat")]
	public class DbVat
	{
		[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
		public int ID {get; set;}
		public int Percent {get; set;}
		public List<DbProduct> Products {get;} = new();
	}
}
```

```sql
create or alter trigger PasswordExpiryReset
on Customer
for insert, update
begin
	update Customer
	set PasswordExpiry = DATEADD(year,1,GETDATE())
	from Customer c
	inner join inserted i on i.ID = c.ID
	left outer join deleted d on d.ID = c.ID
	where d.ID is null or i.Password != d.Password
end
```

