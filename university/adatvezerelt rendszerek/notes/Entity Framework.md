---
aliases: 
tags:
  - 5semester
  - adatvez
  - datadriven
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-24 20:45
---


# Entity Framework

Az *entity framework* egy [[Objektum-relációs Leképezés|ORM]] rendszer, amely lehetővé teszi a *logikai* ([[Adatelérési réteg|adatbázis]]) és a *fogalmi* ([[Üzleti Logika Réteg|üzleti logika]]) modellek szétválasztását. Segítségével függetleníthetjük alkalmazásunkat az adatbázismotortól.

>[!warning]+ Entity Framework vs. Entity Framework Core
>![[Pasted image 20241024204918.png]]

---

## Entitások

A modell által leírt elemek az *entitás típusok*. Ezekhez az adatmodell *tulajdonságokat* rendel, de viselkedésüket nem definiálja. Az entitások más entitásokhoz *relációkkal* kapcsolódhatnak.

---

## Adatbázis

Az EF nem rendelkezik közvetlen ismeretekkel az adatbázismotorról, és annak típusa nincs közvetlen hatással működésére. Az entitásokon végzett műveleteket egy Provider fordítja le a motor műveleteire.

>[!example]+ Néhány támogatott szolgáltató:
>- Microsoft SQL Server, CE, Cosmos DB
>- SQLite, Postgres, IBM Data Servers, MySQL
>- Oracle
>- InMemory adatbázis teszteléshez

>[!summary]+ Leképezési módszerek
>![[Pasted image 20241024205358.png]]

## Database First leképezés

*Database First* leképezés esetén rendelkezünk egy meglévő adatbázissal, melyet tetszőleges DB eszközzel elkészítettünk, és kódot ez alapján generáljuk. A navigáció neveit utána meg kell adni/átírni, valamint esetenként további változtatásokat kell hozni (pl öröklés, stb.).

Ha az adatbázis módosul, megpróbálja megtartani a fent említett kézi változtatásokat, de ez nem mindig sikerül (provider függő).

## Model First leképezés

*Model First* leképezés esetén valamilyen designerben megtervezzük az entitásmodellt, ami generál egy adatbázis inicializáló scriptet. Frissítésnél a tervet módosítjuk, és új scriptet generálunk. Ilyenkor vagy a teljes adatbázist eldobjuk, és létrehozunk egy újat, vagy inkrementális scripttel dolgozunk. Az adatbázis generálása után a C# kódot is legeneráljuk.

## Code First

A *Code First* leképezés esetén adatbázist generálunk C# kód alapján, vagy C# kódunkat létező adatbázissal illesztjük össze. A modell és a leképezés is C# nyelven íródik.

Termék entitás osztálya:

```c#
public partial class Product
{
	public int Id {get;set;}
	public string? Name {get;set;}
	public double? Price {get;set;}
	public int? Stock {get;set;}
	public int? Vatid {get;set;}
	public int? CategoryId {get;set;}
	public string? Description {get;set;}
	public virtual Category? Category {get;set;}
	public virtual ICollection<OrderItem> OrderItems {get;} = 
		new List<OrderItem>();
	
}
```

Adatbázis környezet:

```c#
class MyDbContext : DbContext
{
	public DbSet<Product> Products {get; set;}
	public DbSet<Category> Categories {get;set;}

	protected override void OnModelCreating(ModelBuilder modelBuilder)
	{
		modelBuilder.Entity<Product>()
			.Property(b => b.Name).IsRequired();
		modelBuilder.Entity<Product>()
			.HasOne(p => p.Category).WithMany(c=>c.Products);
	}
}
```

>[!tip]+ Navigation Property
>"Adatbázis join automatikusan"
>
> ```sql
> from p in db.Products
> 	join c in db.Categories on p.CategoryId equals c.Id
> select c.Name
> ```
> helyett:
>```sql
>from p in db.Products select p.Category.Name
>```

>[!success]+ Code First Megközelítés
>- Nincs dizájner, kód határozza meg a leképezést
>- Konvenciók:
>	- Id/OsztálynévID - elsődleges kulcs
>- Annotációk:
>	- ComponentModel.DataAnnotations névtér
>	- Kulcs, oszlopnév, szöveg hossz, stb
>- DbModelBuilder API
>	- Programozottan
>	- OnModelCreating felüldefiniálásával, ModelBuilder használatával

---

## Modellépítés

![[Pasted image 20241024212055.png]]

Táblák, mezők, típusok, kényszerek

```c#
[Table(b)]
```


