---
aliases: 
tags:
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-13 21:08
---

# MongoDB programozása

## MongoDB "nyers" nyelve

```json
db.inventory.find( {
	$and: [
		{
			price: { $ne: 1.99 }
		},
		{
			price: { $exists: true }
		}
	]
})
```

## MongoDB .NET

### Kód - Adatbázis leképezés

Kódgenerálás:
- séma alapján - ha van
- példa jsonok alapján
Kézzel - code first

```csharp
public class Product
{
	public ObjectId Id {get; set;}
	public string Name {get; set;}
	public float Price {get; set;}
	public int Stock {get; set;}
	public string[] Categories {get; set;}
	public VAT VAT {get; set;}
}

public class VAT
{
	public string VATCategoryName {get; set;}
	public float Percentage {get; set;}
}
```

### Leképezés testreszabása

A Pascalcasinget a beépített konvenció konvertálja, de készíthetünk sajátokat is. A testreszabás attribútumokkal zajlik.

```csharp
public class Product
{
	[BsonId]
	public ObjectId Azonosito {get; set;}

	[BsonElement("price")]
	public string Ar {get; set;}

	[BsonIgnore]
	public string NemMentett {get; set;}
}
```

### Lekérdezések

Collectiönökön végezzük a műveleteket:

```csharp
var collection = db.GetCollection<Product>("products");
```

Find:
Lambda kifejezéssel:

```csharp
collection.Find(x=>x.Price < 123 && x.Name.Contains("red"));
```

Nyers MongoDB lekérdezés:

```csharp
collection.Find(
	Builders<Product>.Filter.And(
		Builders<Product>.Filter.Lt(x=>x.Price,123),
		Builders<Product>.Filter.Regex(x=>x.Name, "/red/s"),
	)
);
```

### Operátorok

Szűrések csak konstans értékekre vonatkoznak:

```csharp
collection.Find(x=>x.Price!=123);
collection.Find(Builders<Product>.Filter.Ne(x=>x.Price,123));
```

Null / nem létező kulcs:

 ```csharp
 collection.Find(x=>x.VAT != null);
 collection.Find(Builders<Product>.Filter.Exists(x=>x.VAT));
 ```

Tömb mező szűrése:

```csharp
collection.Find(Builders<Product>.Filter.AnyEq(x=>x.Categories, "Labdák"));
```

Rendezés, lapozás:

```csharp
collection.Find(...)
	.Sort(Builders<Product>.Sort.Ascending(x=>x.Name))
	.Skip(100).Limit(100);
```

### Eredmény feldolgozása

Listázás - `.ToList()`
Egy elem lekérdezése - `.First() / FirstOrDefault() / Single() / ...`
Kurzor:
	- Lista darabokat kérdezhetünk le
	- Folyamszerűen dolgozzuk fel az eredményt

```csharp
var cur = collection.Find(...).ToCursor();

while (cur.MoveNext())
{
	foreach (var t in cur.Current)
	{
		...
	}
}
```


### Aggregációs pipeline

Az aggregációs csővezetékkel több dokumentumot feldolgozhatunk szerveroldalon. 

Egycélú (single purpose): 
- distinct
- count

#### Általános aggregáció

Egymás után több művelet definiálható
- szűrés
- csoportosítás
- számolás
- stb

```csharp
foreach (var g in collection.Aggregate()
			.Match(Builders<Product>.Filter.AnyEq(x=>x.Categories,
														"Labdák"))
			.Group(x=>x.VAT.Percentage, x=>x)
			.ToList())
{
	Console.WriteLine($"VAT percentage: {g.Key}");
	foreach (var p in g) 
		Console.WriteLine($"\tProduct: {p.Name}");
}
```


### "join"

Left outer joint lehet kérni aggregációs műveletben a $lookup funkcióval
Nem erre van kitalálva --> ha túl sok a join, gondoljuk át
	-> sémát
	-> ez-e a megfelelő eszköz?

```csharp
var query = from p in collection.AsQueryable()
			join o in otherCollection on p.Name equals o.Key into joined
			select new {p.Name, AgeSum, joined.Sum(x=>x.Age)};
```


### Beszúrás

```csharp
var newProduct = new Product
{
	Name = "alma",
	Price=800,
	Categories=new[] {"Gyümölcsök"}
};
collection.InsertOne(newProduct);
```


### Törlés
Törlés szűrőfeltétel alapján
-> egy vagy több dokumentumot
-> DeleteOne/DeleteMany

```csharp
var deleteResult = collection.DeleteOne(x=>x.Id == new ObjectId("..."));
Console.WriteLine($"Törölve: {deleteResult.DeletedCount} db");
```


