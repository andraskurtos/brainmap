---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 12:31
---
# Activity

Az *Activity* tipikusan egy képernyő, amin a felhasználó valamilyen műveletet végezhet (login, beállítások, térkép nézet, stb.). Az Activity leginkább egy ablakként képzelhető el. Az ablak vagy teljes képernyős, vagy pop-up jelleggel jelenik meg. Egy alkalmazás tipikusan több Activity-ből áll, amik lazán csatoltak. A legtöbb esetben létezik egy fő Activity, ahonnét a többi elérhető. Bármely Activity indíthat újabbakat, de tipikusan a fő Activity jelenik meg az alkalmazás indításakor.

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
>- *onStart()*: Az Activity látható, a vezérlők is. Például BroadcastRecieverekre feliratkozás, amik módosítják a UI-t.
>- *onStop()*: Az Activity nem látható, például BroadcastRecieverekről leiratkozás
>- *onRestart()*: Az Activity leállítása (*onStop()*) majd újraindítása után hívódik meg, még az indítás (*onStart()*) előtt.
>- *onResume()*: Az Activity láthatóvá válik és előtérben van, a felhasználó eléri a vezérlőket és tudja kezelni azokat.
>- *onPause()*: Az Activity háttérbe kerül, de valamennyire látszik a háttérben, például egy másik Activity pop-up jelleggel előjön, vagy sleep állapotba kerül a készülék

