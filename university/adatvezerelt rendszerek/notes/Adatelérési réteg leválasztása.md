---
aliases: 
tags:
  - 5semester
  - uni
  - adatvez
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-24 14:25
---

# Adatelérési réteg leválasztása


Elképzelhető, hogy az alkalmazásunkban több adattárolót is szeretnénk használni, pl. egy [[MS SQL]], egy Oracle, és egy [[NoSQL]] [[Adatbázis|adatbázist]] is. Ilyenkor célszerű a cserélhető részt egy interfész mögé tenni. Így megvalósul a laza csatolás, a részek jobban elkülönülnek, és az alkalmazás könnyebben tesztelhető lesz.

![[Pasted image 20241024142814.png]]

>[!success]+ Előnyök:
>- Adattár cseréje
>- Unit test miatt, mockoláshoz
>- Adatelérési réteg lecserélése
>- Adatbázis refaktorálás
>- Másik technológiára kell áttérni
>- Át kell térni másik adatbázisra
>- ...

A leválasztás tipikusan egy interfész kialakítása útján történik. Ezen az intefészen explicit jelennek meg az alsó réteg szolgáltatásai és funkciói. Széles körben használt megoldás az úgynevezett *repository pattern*.

![[Pasted image 20241024143023.png]]

---

## Repository tervezési minta

Entitásonként vagy entitáscsoportonként fogalmazunk meg repositorykat, valamint 1 általános repository-t, egy generikus osztály képében. Ezt a repository-t tipikusan interfésszel használjuk, és üzleti entitásokkal dolgozik. Minden adatbázis specifikus rész ide van bezárva. CRUD műveleteket valósít meg.

```java
class ProductRepository : IProductRepo { 
	List List() { /*...*/ }
	Product FindById(int Id) { /*...*/ }
	void Add(Product entity) { /*...*/ }
	void Delete(Product entity) { /*...*/ }
	void Update(Product entity) { /*...*/ } 
}
```

>[!summary]+ Jellemzői:
>- Absztrakció az adatréteg fölött
>- Specifikus hely, ahol az adatok elérésének módját módosítani kell.
>- Adattáblák helye
>- Könnyen cserélhető másik megvalósításra
>- Elrejti a részleteket
>- Több alternatív megvalósítás

>[!example]+ Repository funkciók a gyakorlatban:
> - Entitások hozzáadása és elvétele
> - Az interfész táblákat, gyűjteményeket használ
> - Tranzakciókezelés/lezárás nem itt van
> - Lekérdezések a kívánt kritériumoknak megfelelően
