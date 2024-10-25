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

Az osztályok nézetekre is leképezhetők, így a nézet típusosan elérhető. Ha nézetre és táblára is leképezzük, az EF a nézetet használja a lekérdezésekhez, és a táblát a módosításokhoz.

```c#
protected override void OnModelCreating(ModelBuilder mb) {
	mb.Entity<Blog>()
		.ToView("blogsView", schema: "blogging");
}
```


>[!summary]+ DbContext szerepe
>A DbContext az adatbáziselérés központi osztálya. Nyilvántartja az entitásokat és a rajtuk végzett változtatásainkat, majd a SaveChanges() függvénnyel menti ezeket az [[Adatbázis|adatbázisba]]. Életciklusa rövid, ezért kézi létrehozásnál **using**-al használjuk, DI keretrendszerben pedig Transient/Request scope segítségével. A SaveChanges() tipikusan tranzakcionális, emiatt a DbContext értelmezhető **unit of work** minta megvalósításként is.
>
> - Nem szálbiztos
> - Példányosítása nyitott DB kapcsolatot jelent tipikusan
> - Nem arra tervezték, hogy sok objektumot hatékonyan kezeljen
> - Egyetlen funckió lefutásához példányosítjuk, majd elengedjük.
> 	-> nincs konkurenciaprobléma, nincs sok objektum, kapcsolat minimális ideig van nyitva

---

## Műveletek

### Új entitások beszúrása és mentése

1. Új entitás létrehozása
	- `var newEntity = new Class()`
	- a kulcs még üres, adatbázisan nem jött létre semmi!
2. Az *entitás DbContexthez rendelése*
	- `DbContext.DbSet.Add(newEntity)`
	- Vagy egy másik entitáson keresztül:
	  `someEntity.Property = newEntity`
3. `DbContext.SaveChanges`
4. EF lefuttatja az *INSERT SQL*-t
5. Az elsődleges kulcsok és külső kulcsok bekerülnek a DbContext-hez tartozó entitásokba
	- **newEntity.Id innentől érvényes**

### Entitások módosítása

- Tulajdonság/Hivatkozás módosítása
- A változást a DbContext nyilvántartja
- SaveChanges meghívásával a változások átvezetődnek az adatbázisba

```c#
var course = context.Course.Single(q => q.Neptun== "VIAUAC01");
var aut = context.Department.Single( q => q.Code== "AUT");
course.Name = "Adatvezérelt rendszerek";
course.Department = aut;
context.SaveChanges();
```

### Entitások törlése

- Csak betöltött entitást tudunk explicit törölni
	- Illetve kaszkádosítás miatt a DB törli a kapcsolódó entitásokat magától
- DbSet.Remove(...)
- SaveChanges hívás

```c#
var c = context.Course.Single(q=>q.Neptun=="VIAUAC01");
context.Course.Remove(c);
context.SaveChanges()
```

### Entitásállapot és SaveChanges

- Az EF minden általa ismert entitást "követ"
- Az entitások a memóriában az alábbi állapotokban lehetnek:
	- Unchanged, Added, Modified, Deleted
- A SaveChanges metódus az entitásokat a fenti állapotoknak megfelelően menti az adatbázisba
	- generálja a szükséges SQL-t
- SaveChanges vagy lekérdezés után minden entitás Unchanged állapotban van.


### Bulk Update / Delete

Az entitás állapotával működő megoldás gyakran kényelmetlen és lassú. Be kell olvasni az adatokat memóriába, dbContextet rávezetni a kívánt változtatásokra, amiket egyesével végrehajt.

- DbSet.ExecuteDelete/ExecuteUpdate
	- Egyetlen táblán tudnak dolgozni
- Az entitások szűrhetők

```c#
db.Persons.Where(p=>p.Name.StartsWith("Ro")).ExecuteDelete();
```

Update esetén a feltétel mellett meg kell adni, melyik tulajdonságot mire írjuk át.

```c#
db.Persons.Where(p=>p.Name.StartsWith("Rozsa")).ExecuteUpdate(
	p=>p
		.setProperty(p=>p.Name, p=>"Rózsa"+p.Name.Substring(5))
		.setProperty(p=>p.Age, p=>p.Age+1));
```

### Tranzakciók

#### Alapértelmezett viselkedés

A SaveChanges egy nagy tranzakcióban fut, ezért minden változtatás, amit összegyűjtöttünk a DbContext példányban, vagy egyszerre érvényre jut, vagy nem. Akár több BLL művelet is gyűjtheti egy DbContext példányba a változtatásokat, egymásról nem tudva, végül egyetlen ponton dől el, hogy mindet sikerül-e betenni az adatbázisba.

#### Transaction API

Adatbázison indítható tranzakció

```c#
using (var transaction = db.Database.BeginTransaction())
{
	...
}
```

Tranzakció objektumon lehet kommitálni és abortálni

```c#
transaction.Commit();
```

SavePoint: nevesített pont, ameddig visszagörgethetjük a tranzakciót, és véglegesíthetjük az addigi műveleteket.

### Lekérdezés

*Lekérdezéshez* egy DbContext példányra van szükség. Ez *listát vezet* az újonnan felvett és törölt entitásokról, *nyilvántartja* az objektumon történt változásokat, és a lekérdezett entitásokat. További lekérdezéseknél figyelembe veszi a módosításokat. Mikor a lekérdezett objektumokat nem akarjuk módosítani, használjuk az ***AskNoTracking()***-et, ilyenkor állapotkövetés nélkül kérjük az entitásokat.

>[!example]+ Példa: Budapesti telephelyű vevők
> ```sql
> from c in db.Customers
> where c.MainSite.Address.City == "Budapest"
> select c;
> ``` 

### Navigáció

A kapcsolódó entitások is elérhetőek a navigációs propertyken keresztül:

```c#
var q = from p in db.Products
		where p.Price == db.Products.Max(pp => pp.Price)
		select p;
foreach(var p in q) {
	Console.WriteLine(p.Category?.Name);
}
```

---

## Entitások

### POCO Proxy

Ez az alapértelmezett típus, konfigban kapcsolható ki. Jelentése *Plain Old CLR Object*, tehát nincs alaposztálya. Automatikus változáskövetéssel, navigációval és lazy loading támogatással jön.

>[!warning]+ Követelmények:
>- publikus, nem abstract és nem sealed osztály
>- public/protected paraméter nélküli konstruktor
>- property get: public, non sealed, virtual

- Sallangmentes osztályok
- Változáskövetés
	- Automatikus, de *összehasonlítás alapú*: DbContext tárolja az adatbázisból lekérdezett állapotot, és összehasonlítja az entitás aktuális állapotával.
		- Nincs lazy loading támogatás
	- A hivatkozott entitásokat kézzel kell betölteni, **nincs navigáció**


### Kapcsolódó entitások betöltése

- *Implicit*: a memóriában benne levő kapcsolódó entitásokat az EF automatikusan hozzáköti
- *Előtöltés*: a kapcsolódó entitások betöltése az első lekérdezéssel együtt.
- *Explicit betöltés*: már memóriában lévő entitáshoz további kapcsolódó entitások betöltése
- *Késleltetett betöltés*: kapcsolódó entitások transzparens betöltése a navigáció property bejárásakor -> **NE HASZNÁLJUK!**

#### Előtöltés

DbSet Include metódusa:

```c#
var blogs = context.Blogs
				.Include(blog => blog.Posts)
				.ToList();
```

IncludeThen: lefúrás

```c#
var blogs = context.Blogs
				.Include(blog=>blog.Posts)
					.ThenInclude(post=>post.Author)
					.ThenInclude(author=>author.Photo)
				.ToList();
```

#### Explicit betöltés

Kapcsolódó entitások, entitáshalmazok explicit betöltése

```c#
using (var context = new BloggingContext())
{
	var blog = context.Blogs
				.Single(b=>b.BlogId == 1);
	context.Entry(blog)
			.Collection(b=>b.Posts)
			.Load();

	context.Entry(blog)
			.Reference(b=>b.Owner)
			.Load();
}
```

#### Lazy loading

A relációk olvasásakor (egy másik entitás lekérdezése propertyn keresztül) automatikusan olvassa az adatbázist.

```c#
foreach (var post in posts)
	Console.WriteLine($"----- post blogid: {post.Blog?.BlogId}");
```

Megvalósítás: proxy osztályok, minden tagnak virtuálisnak kell lennie, stb.

**NE HASZNÁLD!!!**

>[!warning]+ Lazy loading probléma
>
>A lazy loading túl sokszor fordul az adatbázishoz.
>![[Pasted image 20241025165707.png]]

### Közvetlen SQL lekérdezések

Egyszerű lekérdezés: 

```c#
var blogs = context.Blogs
				.FromSql($"SELECT * FROM dbo.Blogs")
				.ToList();
```

Paraméterezve, tárolt eljárás hívása:

```c#
var user = "johndoe";

var blogs = context.Blogs
	.FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
	.ToList();
```

## Állapotkövetés [[Adatvezérelt rendszerek architektúrái|többrétegű alkalmazásokban]]

Az entitás kikerül a DBContext alkalmazástartományából, és átkerül egy másik tierbe, fizikai rétegbe. Ilyenkor a DBContext példány megszűnik, az állapotkövetés már nem bízható rá. A változásokat tehát tudtára kell hozni.

### Többrétegű működés:

1. Adatbázis nyitás
2. Entitások *lekérdezése* és átadása a *kliensnek*
3. Adatbázis zárás, dbContext törlődik

...

1. *Kliens* átadja az adatokat, állapot: **Unchanged**
2. Adatbázis nyitva, dbContext üres
3. Entitások elhelyezése dbContextben:
	- Attach? -> állapot: Unchanged/Added
	- Update? -> állapot: Modified, Added
	- Delete?
4. Mentés: SaveChanges
5. Adatbázis zárás, dbContext törlődik

>[!warning]+ Ütközésfelismerés
>1. több kliens megkapja az entitásokat
>2. az adatbáziskapcsolat lebomlik
>3. minden kliens módosít
>4. az egyik elmenti az adatbázisba
>5. a másik kliens is írná az adatbázist
>6. **Észre vesszük-e, hogy volt közben módosítás?**

>[!tip]+ Ütközésfelismerés EF-el
>
>Ellenőrizzük, hogy nem változott-e meg az adatbázis tartalma azóta, hogy mi elkértük az eredeti entitást.
>
>- **Concurrency** tokenek használatával
>	- Bármelyik saját property lehet ilyen
>	- Ezeket a propertyket fogja összehasnolítani a rendszer
>- **Timestamp** segítségével
>	- Az adatbázis automatikusan megnöveli minden módosítás során
>	- Adatbázis motor/provider függő
>
>
>Minden **UPDATE** és **DELETE** parancs tartalmaz egy **WHERE** feltételt, ami ellenőrzi, hogy a rekord nem változott-e meg a legutolsó lekérdezés óta
>	- A dbContext ismeri az entitás eredeti értékeit, azok segítségével generálta az **UPDATE** parancsot is

>[!summary]+ Ütközéskezelés
>1. **DbUpdateConcurrencyException** kivételt kapunk
>	- Az Entries tartalmazza az ütköző entitások listáját
>2. Lekérdezhetjük az adatbázis aktuális állapotát (pl NoTracking opcióval)
>3. Ismerni fogjuk
>	- A menteni kívánt adatokat
>	- Az adatbázis jelenlegi tartalmát
>	- Az általunk korábban le