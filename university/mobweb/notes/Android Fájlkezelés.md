---
aliases: 
tags:
  - 5semester
  - uni
  - mobweb
  - android
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 17:58
---

# Android Fájlkezelés

Az [[Android]] fájlkezelés lényegében ugyanaz, ami a [[Java]] esetében, néhány specialitással. Az Android két lemezterületet különböztet meg, ezek az:

- **Internal Storage**: az a védett tárhely, amit kizárólag az alkalmazás érhet el, se a user, se más appok nem férhetnek hozzá.
- **External Storage**: a felhasználó által is írható-olvasható terület.

*External Storage* is lehet belső memóriában, ha nincs SD kártya a készülékben.

---

## Internal Storage

- *openFileOutput(String filename, int mode)*
	- filename-ben nem lehet "\\", mert kivételt dob
	- Támogatott módok:
		- Context.MODE_PRIVATE: alapértelmezett megnyitási mód, felülírja a fájlt, ha már van benne valami
		- Context.MODE_APPEND: hozzáfűzi a fájlhoz, amit beleírunk
		- Lehet WORLD_READABLE vagy WORLD_WRITEABLE is, ha szükséges, de nem ez a javasolt módja az adatok kiajánlásának, hanem a ContentProvider.
	- Privát vagy Append mód esetén nincs értelme kiterjesztést megadni, mert máshonnan úgyse fogják megnyitni
	- Ha nem létezik a fájl, akkor létrehozza, a WORLD_* módok csak ekkor értelmezettek.
- Fájl olvasása ugyanígy:
	- `openFileInput(String filename)` hívása ( FileNotFound exceptiont dobhat ).
	- Byte-ok kiolvasása a visszakapott FileInputStream-ből read() metódussal
	- Stream bezárása close() metódussal
- Cache használata:
	- Beépített mechanizmus annak esetére, ha cacheként akarunk fájlokat használni.
	- getCacheDir() metódus visszaad egy fájl objektumot, ami a cache könyvtárra mutat.
	- Ezen belül létrehozhatunk cache fájlokat.
	- Kevés lemezterület esetén először ezeket törli az Android.
	- Google ajánlása: maximum 1 MB-os fájlok

---

## Statikus fájlok egy alkalmazáshoz

Szükséges lehet a fejlesztett alkalmazáshoz statikus fájlokat tárolni, mint például egy kezdeti, nagy méretű, bináris adatbázis fájl, vagy bármi egyéb fájl, ami nem illik a res könyvtárba. Ezeket fejlesztéskor a res/raw mappába kell tennünk, majd telepítéskor az Internal Storage-be kerülnek. Telepítés után read-only lesz, és később nem tudjuk módosítani.
Olvasásuk futásidőben a következő módon lehetséges:

```kotlin
val inStream: InputStream = 
	resources.openRawResoucre(R.raw.myfile)
```

---

## External Storage

A nyilvános lemezterület lehet SD kártyán, vagy belső memóriában is. Lényegében egy bárki által írható/olvasható fájlrendszer. Amikor a felhasználó összeköti a telefont a számítógépével, USB storage módra vált, és a fájlok read-onlyvá válnak az alkalmazások számára. Semmilyen korlátozás nincs jelen arra, hogy a nyilvános területen lévő fájlokat a felhasználó letörölje, lemásolja vagy módosítsa -> bármikor elveszhetnek.

>[!warning]+ Nyilvános terület legfontosabb tudnivalók:
>
>- Használat előtt ellenőrizni kell a tárhely elérhetőségét!
>- Fel kell készülni arra, hogy bármikor elérhetetlenné válik!
>
> ```kotlin
> val state = Enviroment.getExternalStorage()
> if (state.equals(Enviroment.MEDIA_MOUNTED)) {
> 	// olvashatjuk és írhatjuk
> } else if (state.equals(Enviroment.MEDIA_MOUNTED_READ_ONLY)) {
> 	// csak olvasni tudjuk
> } else {
> 	// valami más állapotban van, se olvasni, se írni nem tudjuk
> }
> ```

Fájlok elérése  2.2 verziótól fölfele:
```kotlin
val filesDir = getExternalFilesDir(int type)
```

Médiatípusonként különböző alapkönyvtárak vannak definiálva, így az azokat lejátszó/kezelő appoknak nem kell az egész lemezt végigkeresniük. Indexelésüket a MediaScanner osztály végzi.
