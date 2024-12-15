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
created: 2024-12-15 13:32
---

# SPRING

A *Spring* egy pehelysúlyú, neminvezív konténert nyújt, amely segít az alkalmazás objektumainak konfigurálásában és *"összedrótozásában"*, Dependency Injection-el vagy Inversion of Control-al.

>[!question]+ És még?
>- A tranzakciókezelés egységes absztrakcióját, lecserélhető tranzakciómenedzserrel
>- Egyszerűbb JDBC kód megírását támogató segédosztályokat
>- Különféle ORM eszközök egyszerűbb használatát
>- Teljes AOP támogatást
>- MVC felépítést követő, több megjelenítési technológiát támogató webalkalmazás keretrendszert
>- ...


## Spring modulok

![[Pasted image 20241215133526.png]]


## Függőséginjektálás

>[!success]+ A függőséginjektálás előnyei
>
>- Az objektumok nem drótozzák be az általuk tipikusan tagváltozóként használt konkrét osztályokat.
>- Az ún. injektor végzi el a komplex objektumgráfok előállítását
>- Komplex inicializáló kód megspórolható
>- Különböző környezetekben, eltérő működést érhetünk el csak az injektor átkonfigolásával
>- Unit teszteknél mock objektumokkal helyettesíthetjük a tesztelendő objektum függőségeit

A Spring tagváltozó, konstruktor és setterinjektálást is támogat. A spring által kezelt osztályok a *bean*-ek, melyek viselkedését az általuk nyújtott beanekbe injektált más beanekkel tudjuk testreszabni.

A *beaneknek* hatókörük (scope-juk) van, by default singleton (lehet még prototype, request, session)

Spring Boot-al automatikus default konfiguráció classpath alapján

```java
public class CommandService {
	private SettingsService settingsService;
	
	public void setSettingsService(SettingsService setServ) {
		this.settingsService = setServ;
	}

	public void sendCommand(String command) {
		DateFormat df = settingsService.getDateFormat();
	}
}

public class SettingsService {
	public SimpleDateFormat getDateFormat() {/*...*/}
}

ApplicationContext ctx =
	new ClassPathXmlApplicationContext(
		new String[] {"beans.xml"});

CommandService service =
	(CommandService) ctx.getBean("commandService");

service.sendCommand("Command1");
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"...>
	<bean id="settingsService" class="hu.bme.aait.SettingsService"/>
	<bean id="commandService" class="hu.bme.aait.CommandService">
		<property name="settingsService" ref="settingsService"/>
	</bean>
</beans>
```

**@Autowired**-del kell megjelölni az injektálási pontokat
beans.xml-be elég egyetlen sor:
`<context:component-scan base-package="hu.bme.aait"/>` 

A használni kívánt osztályokat meg kell annotálni

```java
@Service
public class CommandService {
	@Autowired
	private SettingsService settingsService;
	// ...
}

@Service
public class SettingsService {/*...*/}
```


## Adatelérés támogatása

Támogatás a JDBC, JPA, JDO, Hibernate, iBatis egyszerűbb használatához:
- Injektáltatható az **EntityManager**, HIbernate-s **SessionFactory**, **JDBCTemplate**
- Egységes tranzakciókezelési modell

Egységes interfész XML binding technológiákhoz


### JDBCTemplate

Mit old meg?
- **Connection** nyitása
- **Statement** létrehozása
- iteráció a **ResultSet**-en
- kivételek kezelése
- **Connection**, **Statement**, **ResultSet** bezárása


### Spring Data

Külön modul az adatelérés támogatására.
Az ún. Repository interfészek segítségével az adatelérési kód nagyrésze megspórolható.

```java
public interface CrudRepository<T, ID extends Serializable>
				extends Repository<T, ID> {

	<S extends T> S save(S entity);
	T findOne (ID primaryKey);
	

	Iterable<T> findAll();

	Long count();
	
	void delete(T entity);

	boolean exists(ID primaryKey);
}
```

Repositoryk: saját entitásra specifikus repositoryt írunk:
`interface PersonRepository extends JpaRepository<Person, Long> {...}

Konfig:
- @EnableJpaRepositories
- vagy <jpa:repositories base-package="com.acme.repositories"/>
- vagy spring-boot-starter-data-jpa

Injektálás másik beanbe:
```java
@Autowired private PersonRepository repository;
```


## Tranzakciókezelés

Egységes API a tranzakciókezelésre, ami mögött bekonfigolható a konkrét tranzakciómenedzser implementáció.
- múködhet JDBC szinten
- használhatja a JPA EntityTransaction-jét
- elérhet elosztott tranzakciómenedzser JTA API-n keresztül
- használhatja a JDO API-t (a JPA régebbi alternatívája)

pl:
```xml
<bean id="transactionManager"
	class="org.springframework.org.jpa.JpaTransactionManager">
	<property name="entityManagerFactory"
		ref="entityManagerFactory"/>
</bean>
```


vagy

```java
@Bean
public PlatformTransactionManager
			transactionManager(EntityManagerFactory emf) {
	JpaTransactionManager transactionManager =
								new JpaTransactionManager();
	transactionManager.setEntityManagerFactory(emf);

	return transactionManager;
}
```

Típusai:

Programozott tranzakciókezelés:
- A Spring által adott API-n keresztül kódból indítjuk/zárjuk le a tranzakciót
- Ritkán használjuk

Deklaratív:
- Metódus szinten annotációkkal (vagy XML-ben) szabályozzuk a tranzakciók indítását/végét
- Metódusnál kisebb egységekről nem rendelkezhetünk
- Engedélyezése:
	- <tx:annotation-driven>
	- vagy @EnableTransactionManagement

Programozott:
```java

@Autowired
PlatformTransactionManager txManager;

DefaultTransactionDefinition def = new DefaultTransactionDefinition();
def.setPropagationBehaviour(TransactionDefinition.PROPAGATION_REQUIRED);

TransactionStatus = txManager.getTransaction(def);
try {
	// businesslogic
} catch (MyException ex) {
	txManager.rollback(status);
	throw ex;
}
txManager.commit(status);
```

Deklaratív:

```java
@Service
public class LogService {
	@PersistenceContext
	EntityManager em;

	@Transactional
	public void create(LogItem logitem) {
		em.persist(logItem);
	}
}
```

>[!summary]+ A @Transactional paraméterei
>- rollbackFor: milyen kivételek esetén rollbackeljen
>- timeout
>- isolation: izolációs szint
>- value: tranzakciómanager bean id-je
>- propagation: mi történjen, ha tranzakcionális metódusból másik tranzakcionális metódust hívunk.

