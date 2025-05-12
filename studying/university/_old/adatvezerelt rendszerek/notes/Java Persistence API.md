---
aliases:
  - jpa
  - JPA
tags:
  - datadriven
  - adatvez
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 10:31
---

# Java Persistance API

A JPA  egy szabványos [[Objektum-relációs Leképezés|ORM]] API a [[Java]] világában. Csak interfészeket specifikál, így több implementáció is lehetséges (*perzisztencia provider - PP*)
- pl: HIbernate, EclipseLink, OpenJPA

A JPA entitás egy olyan osztály, melynek példányait relációs adatbázisban perzisztensen tárolja a JPA. A **javax.persistence** csomag tartalmazza.

![[Pasted image 20241215103438.png]]

## OR leképezés annotációkkal

>[!summary]+ JPA entitás kötelező tulajdonságai:
>- No-arg konstruktor
>- **@Entity** annotáció
>- Elsődleges kulcs attribútum: **@Id** annotációval jelölve
>	- **@GeneratedValue**
>	- Lehet összetett: **@IdClass** vagy **@EmbeddedId**
>- A perzisztens attribútumok getterek-setterek formájában elérhetők
>- A PP elérheti közvetlenül az adatmezőket, ha azokat annotáljuk és nem a gettereket

```java
@Entity
public class Employee {
	@Id
	private Integer id;
	private String name;
	private Date birthDate;

	// getterek, setterek
}
```

### Az OR leképezés testreszabása

A részletek megadása opcionális, alapból a tábla és oszlopnevek az osztály és attribútumnevekkel megegyeznek. Osztályra a **@Table(name="MyTable")**, attribútumokra vagy getterekre pedig **@Column(name="MyColumn")** annotációval megadhatjuk a tábla és az oszlopok neveit. Ezeknek egyéb paraméterei is lehetnek, mint pl **schema, catalog, nullable, length, ...**

Az annotációk helyett [[XML]] fájl is használható, de ez ritka.

## Entitás attribútumok típusai

- Primitív típusok és wrappereik
- String, char[], Character[]
- BigInteger, BigDecimal
- java.util.Date, java.util.Calendar, java.sql.Date, java.sql.Time, java.sql.Timestamp
	- DB-ben dátum, idő vagy mindkettő? -> **@Temporal(DATE/TIME/TIMESTAMP)**
- byte[], Byte[]
	- **@Lob**
- Enum
	- Db-ben szám vagy szöveg -> **@Enumerated(ORDINAL/STRING)**
- Más entitások / nem-entitások gyűjteménye
- Beágyazott osztályok
- Ha nem akarjuk tárolni --> **@Transient**

### Beágyazott osztály

A *beágyazott osztály* olyan osztály, ami önmagában nem perzisztens entitás, csak egy perzisztens példányhoz csatlakozva.

```java
@Embeddable
public class EmploymentPeriod {
	Date startDate;
	Date endDate;
	//.. getterek, setterek
}

@Entity public class Employee {
// ...
	@Embedded
	@AttributeOverrides({
		@AttributeOverride(name="startDate", 
				column=@Column("EMP_START"))
		@AttributeOverride(name="endDate",
				column=@Column("EMP_END"))
	})
	private EmploymentPeriod empPeriod;
}
```

### Konverterek

A *konverterek* a DB oszlop értéke és az attribútum között konvertálnak, típus alapján automatikusan, vagy egyes attribútumokhoz kötve.

```java
@Converter(autoApply=false)
public class WeightConverter implements
		AttributeConverter<Double,Double>
{
 	public Double convertToDatabaseColumn(Double pounds) {
 		return pounds/2.2046;
 	}
 	public Double convertToEntityAttribute(Double kilograms) {
 		return kilograms*2.2046;
 	}
}
```

Konvertert be kell kapcsolni:

```java
@Entity
public class Part {
	@Id Integer partId;
	String name;
	// ...
	@Convert(converter=WeightConverter.class)
	Double shippingWeight;
	// ...
}
```

## JNDI

A **Java Naming and Directory Interface** egy egységes [[Java]] API, amelyen keresztül tetszőleges névfeloldási szolgáltatás elérhető. A névszolgáltatás lehetőséget nyújt, hogy valamely néven valamilyen objektumot regisztráljunk, később pedig név alapján megkeressük.

## Adatbázis elérése

**DriverManager.getConnection()**:
- a kapcsolat minden részét ismerni kell
- a megszerzett kapcsolatot nem tudjuk hatékonyan újrahasználni

**DataSource** interfész:
- A fejlesztő JNDI névre hivatkozva keresi meg
- Az üzemeltető feladata a szerverben az adott JNDI névhez beregisztrálni a DB elérés részleteit.
- Connection poolingot valósíthat meg, vagyis tőle szerzett kapcsolat bezárása nem zárja be fizikailag a kapcsolatot.

```java
@Stateless
public class MyBean {
	@Resource(lookup="mydb")
	DataSource ds;

	public void myMethod() {
		try (Connection conn = ds.getConnection()) {
			//..
		} catch(Exception e) {/*...*/}
	}
}
```

A **@Resource** hatására az [[Enterprise JavaBeans|EJB]] konténer végzi ela JNDI keresést (függőséginjektálás), ha nekünk kellene:
```java
DataSource ds = (DataSource) new InitialContext().lookup("mydb");
```


## Perzisztenciakontextus

A *perzisztenciakontextus* a provider által kezelt, memóriában levő entitások egy halmaza. Ezen keresztül kezeljük az entitásokat, ez a kapcsolat a memóriabeli entitások és az adatbázis között. API szinten az **EntityManager** interfészen érhető el.

>[!warning]+ EntityManager életciklusának kezelése
>- Mikor nyissuk meg?
>- Ha megnyitottuk, mikor zárjuk be?
>	- Ha áthívunk másik metódusba, praktikus lenne abban ugyanazt az **EntityManager** referenciát látni, ezért fölösleges még becsukni --> legyen tagváltozó
>	- De ha tagváltozó, és másik osztály metódusába hívunk át, jó lenne ott is ugyanazt látni --> legyen metódus bemenő paraméter?
>
>*Megoldás: függőséginjektálás Java EE vagy Spring segítségével.*

### Entitások életciklusa

>[!summary]+ Entitások állapotai:
>- **new:** new-val létrehozva kerül ide, csak memóriában létezik
>- **managed:** létezik az adatbázisban, és hozzátartozik egy perzisztenciakontextushoz. A PC-n hívott **flush()** metódussal beíródnak a módosítások az adatbázisba.
>- **detached:** adatbázisban megvan, de nem tartozik PC-hoz.
>- **removed:** még PChoz tartozik, de már ki van jelölve, hogy törölve lesz az adatbázisból a tranzakció végén.

![[Pasted image 20241215113456.png]]

Új entitás menedzselté tétele:
- **persist()**: ha elsődleges kulcs ütközés van, kivétel
- **merge()**: ha elsődleges kulcs ütközés van, UPDATE, ha nincs, INSERT

A **merge()** visszatérési értéke a menedzselt entitás példánya.

Entitás lecsatolása:
- PC kiürtésével **em.clear()**
- PC bezárásával **em.close()**
- Entitás sorosításakor
- egyedileg: **em.detach(entity)**

### Adatbázis szinkronizáció

A PP az **EntityManager** 2 metódusa segítségével szinkronizál az adatbázis felé/felől:
- **flush()**: beírja a DB-be a teljes PC összes módosítását
	- tranzakció commit automatikusan meghívja
- **refresh(entity)** beolvassa a változtatásokat az adatbázisból egy entitásra

**EntityManager.setFlushMode()**:
- **AUTO**-ra álltíva minden lekérdezés előtt flush történik
- **COMMIT**-ra állítva a tranzakció végén van csak flush

### Lekérdezések

Minden lekérdezést az EntityManageren keresztül hajtunk végre.

Keresés elsődleges kulcs alapján:

```java
<T> T find(Class<T> entityClass, Object primaryKey)
```

Lekérdezés dinamikusan:
- JPQL nyelven: **public Query createQuery(String jpqlString)**
	- SQL-hez hasonló nyelv
	- entitáspéldánnyal tér vissza
	- pl "**SELECT e from Employee e WHERE e.name=:name**"
- natív SQL: **public Query createNativeQuery(String sqlString)**

```java
@NamedQueries({
	@NamedQuery(name="Employee.findAll",
				query = "SELECT e FROM Employee e")
})
```

#### Query fontosabb metódusai

**setParameter**: név vagy index alapján
**setMaxResult, setFirstResult**: lapozás támogatására
**getSingleResult** (kivétel, ha nem pontosan 1 találat)
**getResultList** (üres lista, ha nincs találat)
**executeUpdate** (UPDATE, DELETE esetén)

>[!note]+ JPQL lehetőségek
>- többes törlés / módosítás
>- JOIN, GROUP BY, HAVING, subquery
>- paraméterel ?1, ?2, vagy :paraméterNév formában
>- projekció
>- új objektum létrehozása selectben

A JPQL helyett használhatunk [[Criteria API]]-t is.


#### Natív lekérdezések

Bizonyos esetekben nem elég a JPQL/ [[Criteria API]], pl-
- UNION
- DB specifikus lehetőségek

**em.createNativeQuery(nativeSqlString, entityClass)**
**em.createNamedQuery(queryName, entityClass)**

>[!summary]+ Natív lekérdezés eredményének leképezése
>Egy natív lekérdezésben a getResultList() by default Object[]-ek listáját adja vissza.
>A nemtriviális esetben entitásosztályokon elhelyezhető az SqlResultSetMapping annotáció, pl:
>
> ```java
> @SqlResultSetMapping(name="JediResult", classes = {
> 	@ConstructorResult(
> 		targetClass=Jedi.class,
> 		columns = {@ColumnResult(name="name"),
> 							@ColumnResult(name="age")}
> 	)
> })
> ```

### Öröklés leképezése

A legfelső ősosztályon annotációval adjuk meg a választott [[Objektum-relációs Leképezés#Öröklés|O-R leképezési módot]]:
- **@Inheritance(strategy=SINGLE_TABLE)**
- **@Inheritance(strategy=JOINED)**
- **@Inheritance(strategy=TABLE_PER_CLASS)**

#### Diszkriminátor oszlop
 
 Csak **TABLE_PER_CLASS** esetben határozza meg a java típust.
 **SINGLE_TABLE** és **JOINED** esetben plusz egy oszlop szükséges a típus tárolásához -> diszkriminátor oszlop.
Az oszlop neve by default **DTYPE**
Testreszabható **@DiscriminatorColumn(name="...")**
Beleírt érték az entitás neve, **@DiscriminatorValue("mytype")**

### Kapcsolatok leképezése

A kapcsolatot reprezentáló tagváltozó lehet:
- Entitás
- **Collection, Set, List, Map**

A kapcsolatot annotálni kell kardinalitás alapján:
- **@OneToOne**
- **@OneToMany**
- **@ManytoOne**
- **@ManyToMany**
- **@JoinColumn**
- **@JoinTable**
- **@OrderBy**
- **@MapKey**




