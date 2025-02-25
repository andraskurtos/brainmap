---
aliases:
  - kotlin
tags:
  - mobweb
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-26 18:25
---





# Kotlin

A Kotlin programozási nyelv JVM byte kódra fordul, meglévő Java API-ok, keretrendszerek és könyvtárak is használhatók vele. [[Java]]-ról automatikus konverzió érhető el Kotlinra.


## Kotlin alapok

### konstansok, változók

Egyszeri értékadás - "val":

```kotlin
val score: Int = 1 // azonnali értékadás
val idx = 2 //típus elhagyható
val age: Int // típus szükséges, ha nincs azonnali értékadás
age = 3 // későbbi értékadás
```

Változók (megváltoztatható) - "var":

```kotlin
var score = 0 // típus elhagyható
score +=1
```

String sablonok

```kotlin
var score = 1
val scoreText = "$score pont"

score = 2
// egyszerű kifejezések stringek esetében:
val newScoreText = "${scoreText.replace("pont", "volt, most")} $score"
```


>[!note]+ Változók null értéke
>Alapból a változók értéke nem lehet `null`
>```kotlin
>var a: Int = null
>// error: null can not be a value of a non-null type Int
>```
>
>A '?' operátorral engedélyezhetjük a `null` értéket.
>```kotlin
>var a: Int? = null
>
>// lista, amelyben lehetnek null értékek:
>var x: List<String?> = listOf(null,null,null)
>
>// lista, amely lehet null:
>var x: List<String\>? = null
>
>// Lista, mely lehet null és az elemei is lehetnek nullok:
>var x: List<String?>? = null
>x = listOf(null, null, null)
>```

>[!tip]+ Nulltesztelés és az Elvis operátor
>```kotlin
>var nullTest : Int? = null
>nullTest?.inc()
>```
>
>--> `inc()` nem hívódik meg, ha `nullTest` `null`.
>
>```kotlin
>var x : Int? = 4
>var y = x?.toString() ?: ""
>```
>
>--> ha `x` `null`, akkor `y` `""` értéket kap.

>[!error]+ Double bang operátor
>Kivételt dob, ha a változó értéke `null`
>```kotlin
>var x: Int? = null
>x!!.toString()
>// kotlin.KotlinNullPointerException
>```

---

### függvények

Függvény szintaxis:

```kotlin
fun add(a: Int, b: Int): Int {
	return a+b
}
```

Kifejezés törzs, visszatérési típus elhagyható

```kotlin
fun add(a: Int, b: Int) = a+b
```

Érték nélküli visszatérés - Unit

```kotlin
fun printAddResult(a: Int, b: Int): Unit {
	println("$a + $b értéke ${a+b}")
}
```

Unit elhagyható

```kotlin
fun printAddResult(a: Int, b: Int) {
	println("$a + $b értéke ${a+b}")
}
```

---

### osztályok

```kotlin

class Car(val type: String) {
	val typeUpper = type.toUpperCase()
	
	init {
		Log.d("TAG_DEMO", "Car created: ${type}")
	}

	constructor(type: String, model: String) : this(type) {
		Log.d("TAG_DEMO", "Car model: ${model}")
	}
}
```

Leszármaztatás:

```kotlin

// alapesetben minden final!!!
open class Item(price: Int){
	open fun calculatePrice() {}
	fun load() {}
}

class SpecialItem(price: Int) : Item(price) {
	final override fun calculatePrice() {}
}
```

>[!info]+ Osztály elemek
>
>![[Pasted image 20240926184815.png]]

