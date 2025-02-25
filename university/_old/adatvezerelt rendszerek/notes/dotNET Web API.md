---
aliases:
  - .NET Web API
tags:
  - adatvez
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 16:32
---

# .NET Web API


>[!summary]+ Definíció
>A .NET Web API egy keretrendszer .NET alapú [[REST API|REST]] szolgáltatások fejlesztésére. Nem csak szigorú [[REST API|REST]] szemlélethez (ezért is Web API a neve).
>Moduláris kiterjeszthető pipeline támogatás.
>

Alapkoncepciók:
- Erőforrásonként (Product, Order, stb) tipikusan egy **Controller** osztályt írunk
	- ProductsController, OrdersController, stb.
	- Fogadja a kérést és előállítja a választ
- A WebAPI a .NET routing enginet használja az URLek Controllerekre való leképezéséhez
- Válasz esetén típusos objektummal térünk vissza a Controller metódusaiból.
	- A [[JSON]] leképezés automatikus, megoldja a keretrendszer
	- Ezeket az entitásokat model osztályoknak nevezzük

![[Pasted image 20241215164114.png]]


## Példák

ControllerBase ősosztály
### Minden elem lekérdezése

```csharp
[Route("api/[controller]")]
[ApiController]
public class TodosController: ControllerBase
{
	[HttpGet]
	public ActionResult<IEnumerable<TodoItem>> GetTodoItem()
	{
		return _context.TodoItems.ToList();
	}
}
```

Származtatás a ControllerBase-ből.
Routing:
- egy adott bejövő kérés esetén meg kell találni a megfelelő controller osztályt, és annak megfelelő műveletét
- A leképezést attribútumokkal szabályozzuk
	- Route attribútum a Controller osztályon
	- HttpGet, HttpPost, HttpPut, HttpDelete

### Adott elem lekérdezése  

```csharp
[Route("api/[controller]")]
[ApiController]
public class TodosController : ControllerBase
{
	[HttpGet("id")]
	public ActionResult<TodoItem> getById(string id)
	{
		var item = _context.TodoItems.find(id);
		
		if (item == null) return NotFound();
		return item;
	}
}
```

### Új elem létrehozása

```csharp
[Route("api/[controller]")]
[ApiController]
public class TodosController : ControllerBase
{
	[HttpPost]
	public IActionResult createNewTodo([FromBody] TodoItem todo)
	{
		_context.TodoItems.Add(todo);
		_context.SaveChanges();
		
		return CreatedAtAction(nameof(getById),
						new {id=todo.Id}, todo);
	}
}
```

## Lépések

- Model osztályok
	- A kérésekben szereplő entitásoknak
- Adathozzáférés ( ez csak a példában )
	- Pl. Repository vagy DbContext
- Controller
	- Fogadja a bejövő kéréseket és előállítja a választ
- Tesztelés
	- Pl. Postman-el

