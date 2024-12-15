---
aliases:
  - ejb
  - EJB
tags:
  - adatvez
  - 5semester
  - uni
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 12:46
---

# Enterprise JavaBeans

Az *EJB* egy szabványos felülettel rendelkező, elosztott, szerver oldali komponensek, melyek az üzleti logikát tartalmazzák.
EJB konténerben futnak, amely
- elfedi a hálózati kommunikációt
- elfedi a tövvszálúságot
- elfedi a tranzakciókezelést
- egyéb szolgáltatásokat nyújt

>[!summary]+ Típusai
>- Session bean
>	- üzleti logikai funkciók
>- Entity bean
>	- Elavult ORM megoldás, felváltotta a [[Java Persistence API]]
>- Message-driven bean
>	- Aszinkron üzenetkezelő rendszerekből érkező üzenetek feldolgozása

## Implicit middleware

Külön leíró fájl tartalmazza, milyen middleware szolgáltatásokat veszünk igénybe. A kérésmegszakító a leíró fájl alapján generálódik. A forráskód így csak üzleti logikát tartalmaz, a leíró fájlt módosíthatja a vevő, a forráskódot nem kell kiadnunk. Egy lehetséges megvalósítás az interfész és az implementáció szétválasztása.

![[Pasted image 20241215125914.png]]

## Session Bean felépítése

*Enterprise bean osztály*
*Business interfész*
- Ha távolról el akarjuk érni: távoli interfész -> paraméterátadás az objektumok teljes másolásával + (de)szerializálási overhead
- Ha csak ugyanabban az alkalmazásszerverben futó más *EJB*kből vagy webkomponensekből: lokális interfész -> paraméterátadás objektumok esetén a referencia másolásával
- Ha nem akarunk távoli elérést, interfész nélkül

*A konténer által generált csomagoló osztály*
- Megvalósítja a business interfészt, vagy annak hiányában leszármazik a bean osztályból
- Middleware szolgáltatásokat hív és delegálja a kliens kéréseit az implementációs osztálynak

### Referencia szerzése egy EJP példányra

Az az objektum, amin a kliens metódusokat hív, sosem az implementációs osztályból egy példány, hanem a konténer által generált kliens oldali csonk, mert:
- Így lehet a middleware szolgáltatásokat közbeiktatni
- távoli kliens esetén a hálózati kommunikációt is meg kell valósítani

--> **nem lehet sima new-al példányosítani**
- nem ismerjük az osztályát
- nem akarjuk megadni a forráskódban, hogy melyik gépen lévő EJB példányt akarjuk elérni

**MEGOLDÁS:** névszolgáltatás, hasonlóan a [[Java Persistence API#Adatbázis elérése|DataSource megkereséséhez]]

### Szálkezelés

Az EJB-konténer garantálja, hogy egy implementációs osztálybeli példányt egyszerre csak egy szálból hív meg --> szinkronizációval nem kell foglalkozni.
Ugyanakkor több konkurens klienst is ki kell szolgálni, tehát implementációs osztályból mindig több példány van (*instance pool*), melynek méretét konténer kezeli konfigurálható szabályok alapján.

Kérdés: *az egy klienstől jövő kérések mindig ugyanahhoz az implementációs osztálybeli példányhoz futnak be?*

### Állapotkezelés

Lehet **állapotmentes**, **állapottal rendelkező**, vagy **singleton**.

Állapotmentes: a kliens nem számíthat arra, hogy végig ugyanazzal a bean példánnyal fog kommunikálni, emiatt nem tárolhat benne állapotot.

Állapottal rendelkező: az egy kliens referencián hívott metódusok mindig ugyanahhoz a szerveroldali példányhoz futnak be.

Singleton: egy példány létezik, minden hívás ahhoz fut be.

Annotációval - **@Stateless, @Stateful, @Singleton**

## [[Java Persistence API|JPA]] használata EJB környezetben

JPA perzisztenciakontextus injektálható, pl:

```java
@Stateless
class PersonService {
	@PersistenceContext
	EntityManager em;

	public void createEmployee {
		em.persist(new Employee(12345, "Gabor"));
	}

}
```


Az **EntityManagerFactory** csak az alkalmazás indulásakor jön létre, egyetlen példányban. Alapból a tranzakció elején jön létre, és annak végén záródik be. Ha ugyanabban a tranzakcióban más osztályban is van injektált **EntityManager**, ugyanazt a perzisztenciakontextust fogják látni.

> [!summary]+  **@PersistenceContext** paraméterei:
>- **unitName**: ha több unit van a persistence.xml-ben, megadandó
>- **type**: az élettartamát határozza meg
>	- TRANSACTION: tranzakció végén zár be
>	- EXTENDED: állapottal rendelkező session bean eltávolításakor záródik be

>[!summary]+ A Java EE tranzakciók szereplői
>- Tranzakciós objektum (komponens)
>	- olyan alkalmazáskomponens (lehet nem Java EE-ben írt is), amely érintett egy tranzakcióban
>- Tranzakciós erőforrás
>	- írni, olvasni lehet, pl. adatbázis, üzenetsor
>- Erőforrás menedzser
>	- amin keresztül elérjük az erőforrást, pl JDBC driver adatbázishoz, JMS driver üzenetsorhoz
>- Tranzakciómenedzser
>	- elosztott tranzakció esetén mindenképp szükség van rá
>	- levezényliu a teljes tranzakciót a kétfázisú commit segítségével
>	- Java EE esetlben az alkalmazásszerver része


### Tranzakciók

A persistence.xml-ben megadható, hogy lokális vagy JTA tranzakciókezelést választunk.

Lokális: az **EntityManager**-től elkért **EntityTransaction** interfészen keresztül kell kezelni a tranzakciót

JTA: az EJB-konténerben levő tranzakciómenedzseren keresztül indítjuk el a tranzakciókat, vagyis az EJB-ben indított tranzakció a perzisztencia providert kikerülve, közvetlenül a JDBC drivert szólítja meg

Gyakori tervezési minta a *session facade*, ahol a webrétegbeli vagy vastag kliensek session beanek tranzakcionális metódusait hívják, melyek az **EntityManager**-en keresztül JPA entitásokon végeznek perzisztens műveleteket.

**Az entitások módosításával járó műveleteket csak tranzakción belül szabad meghívni**, ha perzisztenciakontextus tranzakció-élettartamú.
Ha a lekérdezésekkel megtalált példányok tranzakción belül olvasódnak be, menedzselt állapotba kerülnek.

Ha a tranzakción kívül futtatunk lekérdezést, a megtalált példányok lecsatoltak lesznek, ha a PC tranzakció élettartamú, és menedzseltek, ha kibővített élettartamú.

A tranzakciókezelés módja annotációval változtatható:
	**@TransactionManagement(BEAN/CONTAINER)**

#### Tranzakciós attribútumok

Deklaratív tranzakciókezelés esetén a telepítésleíróban **@TransactionAttribute** annotációval metódusszinten megadható egy tranzakciós attribútum. **REQUIRED** bydefault, ha egyiket sem tesszük ki, így alapból minden EJB metódus tranzakcióban fut.

![[Pasted image 20241215133030.png]]



### Konkurenciakezelés

A JPA további lehetőségeket definiál a konkurenciakezelés finomhangolására:
- optimista (**@Version** annotáció)
- explicit zárak (**em.lock(Object entity, LockMode)**)
- segítségükkel alacsonyabb izolációs szint mellett is kezelni tudunk bizonyos problémákat.