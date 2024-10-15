---
aliases:
  - activity
tags:
  - 5semester
  - mobweb
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 12:31
---
# Activity

Az [[Android]] *Activity* tipikusan egy képernyő, amin a felhasználó valamilyen műveletet végezhet (login, beállítások, térkép nézet, stb.). Az Activity leginkább egy ablakként képzelhető el. Az ablak vagy teljes képernyős, vagy pop-up jelleggel jelenik meg. Egy alkalmazás tipikusan több Activity-ből áll, amik lazán csatoltak. A legtöbb esetben létezik egy fő Activity, ahonnét a többi elérhető. Bármely Activity indíthat újabbakat, de tipikusan a fő Activity jelenik meg az alkalmazás indításakor.

---

## Activity életciklus

Amikor egy Activity leáll egy másik indulása miatt, az eseményről értesítést kap az úgynevezett [[Activity#Életciklus callback függvények|életciklus-callback metódusokon]] keresztül. Számos callback metódus támogatott (create, stop, resume, destroy, stb.), amikre megfelelően reagálhat az Activity. Például stop esemény hatására a nagyobb objektumokat érdemes elengedni. Amikor az Activity visszatér, újra kell kérni az erőforrásokat. Ezek a tipikus részei az Activity életciklusának. Egy megbízható és flexibilis alkalmazás esetén elengedhetetlen az életciklus-callback események felüldefiniálása. Az életciklust az Activity-vel együttműködő többi Activity határozza meg. Elengedhetetlen az Activity tesztelése különböző életciklus állapotokban.

![[Pasted image 20241015124232.png]]

>[!danger]+ Activity bezárása a rendszer által
>
>Paused, vagy Stopped állapotban a rendszer bármikor leállíthatja a memória-felszabadítás céljából. A leállítás történhet *finish()* hívással, vagy kritikusabb esetben a Process leállításával. Ha az Activity-t újranyitják, a rendszer újra létrehozza.

---

### Életciklus callback függvények

Amikor az Activity állapotot vált, megfelelő callback függvények hívódnak meg. Ezek a callback függvények "hook" jellegű függvények, melyeket a rendszer hív. Fontos a metódusok felüldefiniálása és a megfelelő részek implementálása. **Mindig meg kell hívni az ősosztály implementációját is!!**
A rendszer felelőssége meghívni ezeket a függvényeket, de a fejlesztő felelőssége a helyes implementáció.

>[!example]+ Activity skeleton
>
>```kotlin
>class ExampleActivity : Activity() {
>	override fun onCreate(savedInstance : Bundle?) {
>		super.onCreate(savedInstance)
>		// most jön létre az Activity
>	}
>	
>	override fun onStart() {
>		super.onStart()
>		// most válik láthatóvá az Activity
>	}
>	
>	override fun onResume() {
>		super.onResume()
>		// Fókuszt kap az Activity
>	}
>	
>	override fun onPause() {
>		super.onPause()
>		// Az Activity már nem látható
>	}
>	
>	override fun onDestroy() {
>		super.onDestroy()
>		// Az Activity meg fog semmisülni
>	}
>} 
>``` 
>
>- *onCreate()*: Activity létrejön és beállítja a megfelelő állapotokat (layout, munkaszálak, stb.)
>- *onDestroy()*: Minden lefoglalt erőforrás felszabadítása
>- *onStart()*: Az Activity látható, a vezérlők is. Például BroadcastRecieverekre feliratkozás, amik módosítják a [[Felhasználói Felület (Android)|UI]]-t.
>- *onStop()*: Az Activity nem látható, például BroadcastRecieverekről leiratkozás
>- *onRestart()*: Az Activity leállítása (*onStop()*) majd újraindítása után hívódik meg, még az indítás (*onStart()*) előtt.
>- *onResume()*: Az Activity láthatóvá válik és előtérben van, a felhasználó eléri a vezérlőket és tudja kezelni azokat.
>- *onPause()*: Az Activity háttérbe kerül, de valamennyire látszik a háttérben, például egy másik Activity pop-up jelleggel előjön, vagy sleep állapotba kerül a készülék

>[!tip]+ Activity váltás
>
>Életciklus-callback függvények meghívási sorrendje:
>1. **A** Activity *onPause()* függvénye
>2. **B** Activity *OnCreate*, *onStart* és *onResume* függvénye
>3. **A** Activity onStop() függvénye, mivel már nem látható
>
>>[!warning]+
>>Ha **B** Activity valamit adatbázisból olvas ki, amit az **A** ment el, akkor ez a mentés **A** Activity *onPause* kell megtörténjen, hogy a **B** aktuális legyen, mire megjelenik.

---

## Activity backstack

Egy feladat végrehajtásához tipikusan a felhasználó több Activity-t használ. A rendszer az Activity-ket ún. *Back Stack*-en tárolja. Az előtérben lévő Activity van a BackStack tetején. Ha a felhasználó átvált egy másik Activity-re, akkor eggyel lejjebb kerül a BackStacken, és a következő lesz legfelül. Visszagomb esetén legfelülről veszi ki az Activityt a rendszer.


---

## Activity vezérlés

Legtöbb esetben az alapértelmezett BackStack kielégíti az igényeinket, néha azonban szükség lehet az alapértelmezett viselkedés felülírására. Ha például a vissza hatására mindig egy kezdő Activity-re szeretnénk visszatérni, törölnünk kell a BackStacket. Az alapértelmezett viselkedés felülírását a [[Android projekt felépítése#A Manifest állomány|Manifest állományban]] az `<activity>`-ben, vagy a *startActivity* fv paramétereként valósíthatjuk meg. Amennyiben ezt módosítsuk, mindenképpen teszteljük az alkalmazást navigálás és [[Felhasználói Élmény|felhasználói élmény]] szempontjából.

>[!summary]+ Az activity tag attribútumai:
>- taskAffinity: melyik taskhoz tartozik
>- launchMode: indítási mód
>- allowTaskReparenting: új taskhoz kerül át
>- clearTaskOnLaunch: minden Activity-t töröl a taskból
>- alwaysRetainTaskState: a rendszer kezelje-e a Task állapotát?
>- finishOnTaskLaunch: le kell-e állítani az Activity-t, ha a felhasználó kilép a Task-ból.

>[!summary]+ *startActivity* függvény paraméterértékei:
>- FLAG_ACTIVITY_NEW_TASK
>- FLAG_ACTIVITY_CLEAR_TOP
>- FLAG_ACTIVITY_SINGLE_TOP

---

## Új Activity indítása

```kotlin
fun runSecondActivity() {
	val myIntent: Intent = Intent()
	myIntent.setClass(this@MainActivity, SecondActivity::class.java)

	myIntent.putExtra("KEY_DATA", "Hi there!")
	startActivity(myIntent)
}
```

---

