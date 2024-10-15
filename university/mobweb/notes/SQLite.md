---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 15:40
---





# SQLite

Az android alapból tartalmaz egy teljesértékű relációs adatbáziskezelőt, az SQLite képében. Strukturált adatok tárolására ez a legjobb választás. Alapból nincsen objektum-relációs réteg felette, nekünk kell meghatároznunk a sémákat, és megírni a queryket.

>[!info]+ Android SQLite jellemzői
>- Standard relációs adatbázis szolgáltatások
>	- SQL szintaxis
>	- Tranzakciók
>	- Prepared statemetn
>- Támogatott oszloptípusok:
>	- TEXT
>	- INTEGER
>	- REAl
>- Az SQLite nem ellenőrzi a típust adatbeíráskor
>- Az SQLite adatbáziselérés fájlrendszerelérést jelent, ami miatt lassú lehet.
>- Adatbázis műveleteket érdemes aszinkron módon végrehajtani (pl Coroutine-ok vagy Thread)

>[!bug]+ SQLite debug
>Az [[Android#Android SDK (Software Development Kit)|Android SDK]] tools mappájában található egy konzolos adatbáziskezelő: *sqlite3*
>Ennek segítségével futás közben láthatjuk az adatbázist, akár emulátoron, akár telefonon
>Hasznos eszköz, de sajnos nincs grafikus felülete
>Használata:
>- Konzolban megnyitjuk a **platform-tools** könyvtárat
>- "adb shell" futtatása
>- "**sqlite3 data/data/[Package Name]/databases/[DB Name]**" futtatása
>- Megkapjuk az SQL konzolt, itt már az [[Adatbázis|adatbázison]] futtathatunk közvetlen utasításokat

## Alap SQL API ([[Java]])

### SQLite használata

Az adatbázisok megnyitásához és a séma létrehozásához egy segédosztály használható: *SQLiteOpenHelper*.

Első lépéskén tehát példányosítjuk a saját SQLiteOpenHelper osztályunkat. A példányon meghívunk egy *getWriteableDatabase()* vagy *getReadableDatabase()* metódust, amik egy SQLiteDatabase objektummal térnek vissza. Ez a teljes adatbázisunkat reprezentálja.

Ezen az objektumon futtathatunk queryket. Minden query egy *Cursor* objektummal tér vissza, ami az eredményhalmazra mutat. A cursor használatára rengeteg metódus létezik, célszerű do-while ciklussal végigmenni az adatokon.

>[!example]+ INSERT és UPDATE
> ```java
> public long createOra( Ora ora ){
> 	ContentValues values = new ContentValues();
> 	values.put(KEY_NAME, ora.getName());
> 	values.put(KEY_START, ora.getStart());
> 	values.put(KEY_END, ora.getEnd());
> 	values.put(KEY_LOCATION, ora.getLocation());
> 	long rowId = mDb.insert(DATABASE_TABLE, null, values);
> 	return rowId;
> }
> ```

>[!example]+ Összes rekord lekérdezése
> ```java
> public Cursor fetchAll() {
> 	return mDb.query(
> 		DATABASE_TABLE, // tábla
> 		new String[] {KEY_NAME, KEY_START, KEY_END,
> 		KEY_LOCATION}, // oszlopok
> 		null, null, null, null, // WHERE, GROUP, HAVING
> 		KEY_START); // ORDER BY
> }
> ```

>[!example] Cursor használata
> ```java
> OraDBAdapter oraDBAdapter = new OraDBAdapter(getApplicationContext());
> Cursor cursor = oraDBAdapter.fetchAll();
> Ora ora;
> if (cursor.moveToFirst()) {
> do {
> 		String oraStart = 
> 		cursor.getString(cursor.getColumnIndex(
> 		OraDBAdapter.KEY_START));
> 		// ...
> 		ora = new Ora(oraStart, oraEnd, oraName, oraLocation);
> 	} while(cursor.moveToNext());
> }
> ```

