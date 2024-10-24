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
[Table("blogs", Schema="blogging")]
class Blog
{
	[Column("blog_id")]
	public int BlogId {get;set;}
	[Column(TypeName="varchar(200)")]
	public string Url {get;set;} = "";
	
	[Required]
	[MaxLength(500)]
	public string Url {get;set;}="";
}
```

>[!warning]+ Azonosító generálása
>1. Nincs generálás - manuálisan adjuk meg
>2. Csak új entitás létrehozásakor
>3. Entitás létrehozásakor és módosításakor
>
>Generálás módja SQL szerver esetén:
>	\> int, Guild: automatikusan generálódik a DB-ben
>    \> byte[] concurrency tokenként automatikusan rovwersion lesz és automatikusan generálódik
>    \> DateTime: külön kell megadni, pl FluentApival.
>
>Annotációk:
> 1.  `[DatabaseGenerated(DatabaseGeneratedOption.None)]`
> 2.  `[DatabaseGenerated(DatabaseGeneratedOption.Identity)]`
> 3.  `[DatabaseGenerated(DatabaseGeneratedOption.Computed)]` 
insert közben

### Több kapcsolat két entitás közt

Ha több kapcsolat lenne két entitás között, explicit megadjuk, melyik micsoda.

```c#
public class Post {
	public int AuthorUserId {get;set;}
	public User? Author {get;set;}

	public int ContributorUserId {get;set;}
	public User? Contributor {get; set;}
}

public class User {
	[InverseProperty("Author")]
	public List<Post> AuthoredPosts {get;} = new();
	[InverseProperty("Contributor")]
	public List<Post> ContributedToPosts {get;} = new();
}
```

### Több-több kapcsolat

Több-több kapcsolat esetén két osztály gyűjteményekkel hivatkozik egymásra, ilyenkor automatikusan létrejön a kapcsolótábla. A relációt reprezentáló entitás testreszabható.

```c#
public class Post {
	public ICollection<Tag> Tags {get;} = new List<Tag>();
}

public class Tag {
	public ICollection<Post> Posts {get;} = new List<Post>();
}
```

### Shadow properties

A *shadow property*-k olyan tulajdonságok, amik nem jelennek meg az entitásosztályon propertyként. Fluent API-al deklarálhatók.

```c#
class BlogContext : DbContext
{
	public DbSet<Blog> Blogs {get;set;}
	public DbSet<Post> Posts {get;set;}

	protected override void OnModelCreating(ModelBuilder mb) {
		mb.Entity<Blog>()
			.Property<DateTime>("LastUpdated");
	}
}
```

Automatikusan létrejönnek, ha egy kapcsolat csak referenciaként jelenik meg, de nincs külső kulcs az osztályon. Csak a *ChangeTracker*-en keresztül kezelhetőek, tipikusan infrastruktúrához tartozó tulajdonságok vagy funkciók. Lekérdezésekben felhasználhatók.

```c#
context.Entry(myBlog).Property("LastUpdated").CurrentValue=DateTime.Now
var blogs = context.Blogs.OrderBy(
	b=>EF.Property<DateTime>(b, "LastUpdated")
);
```

### Index

Alapértelmezetten minden külső kulcsként használt mezőhöz létrejön index. Csak API-al lehet megadni.

```c#
protected override void OnModelCreating(ModelBuilder mb) {
	mb.Entity<Blog>()
		.HasIndex(b => b.Url)
		.IsUnique();
	mb.Entity<Person>()
		.HasIndex(p=>new {p.FirstName, p.LastName});
}
```

[NotMapped] - nem leképezett típus, property

### Leképezés nézetekre

