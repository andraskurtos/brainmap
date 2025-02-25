---
aliases: 
tags:
  - 5semester
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 12:23
---

# Criteria API

A JPA 2.0 által bevezetett, string alapú [[Java Persistence API|JPA]] -kban használatos JPQL objektumorientált, típusbiztos alternatívája.

```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Employee> cq =
	cb.createQuery(Employee.class);
Root<Employee> emp = cq.from(Employee.class);
cq.select(emp);
cq.where(cb.equal(emp.get("lastName"), "Smith"))
TypedQuery<Employee> query = em.createQuery(cq);
List<Employee> rows = query.getResultList();
```

Látható, hogy ez több kód, mint JPQL esetén, cserébe típusbiztos. Ez a megoldás tartalmaz string alapú navigációt, de ez is kiküszöbölhető Metamodel API segítségével. A perzisztencia egység metamodelje metaadatokat tartalmaz az entitásokról és a beágyazott osztályokról. Általában az annotációk feldolgozásával tudja generálni a PP segédeszköze.

```java
@Entity
public class Employee {
	@Id Long id;
	String firstName;
	String lastName;
	Department dept;
}

@StaticMetamodel(Employee.class)
public class Employee_ {
	public static volatile SingularAttribute id;
	public static volatile SingularAttribute firstName;
	public static volatile SingularAttribute lastName;
	public static volatile SingularAttribute dept;
}
```

Kanonikus metamodel:
- osztálynév után _
- public static volatile attribútumok, **SingleAttribute**, vagy **Collection/List/Map/SetAttribute** típusúak
- az attribútumok nevei azonosak

