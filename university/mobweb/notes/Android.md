---
created: 20240901 23:53
tags:
  - 5semester
  - mobweb
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---
# ANDROID
## Az android keletkezése

Az *Android* napjaink egyik sláger mobil operációs rendszere, amiből valóságos brand lett, reklámokkal és hirdetésekkel együtt. Az "IT óriás" Google áll mögötte.

Egy egységes, kiválóan működő rendszer képét nyújtja, és forradalmasította a mobil OS-ekről alkotott képet.

Az Android amellett hogy operációs rendszer, az *ezen rendszert futtató eszközök összessége is egyben*. Ilyenek a tabletek és mobiltelefonok, de emellett gépjárművek fedélzeti számítógépei és navigációja, Android Wear, Ipari automatizációs eszközök, stb.

---

## Története

- **2005**-ben felvásárlásra kerül az *Android Incorporated* nevű kaliforniai cég.
- **2007** elején elkezdenek kiszivárogni a hírek, hogy a Google belép a mobil piacra.
- **2007 november 5**-én az Open Handset Alliance bejelentette az Android platformot.
- **2008** végén került piacra a T-Mobile által forgalmazott HTC G1-es készülék.

### mi volt a Google célja?

A Google az Android képében egy nyílt forráskódú, rugalmas és könnyen alakítható rendszert akart fejleszteni, amelyre könnyen fejleszthetők külső alkalmazások. Moduláris Linux kernel alapú lett tehát, és a kód túlnyomó része Apache, nyílt forráskódú vagy szabad program licenc alatt van.

---
## A platform szerkezete

![[Pasted image 20240909112829.png]]

Távolról tekintve a platform felépítése egyszerű és világos. A *narancssárga* Linux Kernel tartalmazza a hardver által kezelendő eszközök meghajtóprogramjait. Ezeket azon cégek készítik el, akik az Android platformot használni szeretnék, hiszen a gyártónál jobban nem ismeri senki az eszközbe integrált perifériákat. Ezen kívül a Linux kernel még a memóriakezelés, az ütemezés és az alacsony energiafogyasztást elősegítő teljesítmény-kezelés.

A linux kernelen futnak a *lila* részben található programkönyvtárak / szolgáltatások, mint például libc, SSL, [[SQLite]], stb.

Részben ezekre épül a *sárga* egységben található *virtuális gép*.
Ez nem kompatibilis a Sun virtuális gépével, teljesen más utasításkészlettel rendelkezik, és más bináris programot futtat. A Java programok nem egy-egy .class állományba kerülnek fordítás után, hanem egy nagyobb, *Dalvik Executable* formátumba, amelynek kiterjesztése .dex, és általában kisebb, mint a forrásul szolgáló .class állományok, mivel a Java fájlban található konstansokat csak egyszer fordítja bele a fordító.

A *zöld* és *kék* színnel jelölt részekben már csak Java forrást találunk. A Java forrást a gép futtatja, ez adja az Android lényegét: látható és tapintható operációs rendszert, és futó programokat.

A virtuális gép akár teljesen elrejti a linux által használt fájlrendszert, és csak az Android Runtime által biztosított fájlrendszert láthatjuk.

---

## Szoftverfejlesztési eszközök Androidra

### Android SDK (Software Development Kit)

- fejlesztő eszközök
- emulátor kezelő
- frissítési lehetőség
- [[java]], [[kotlin]]

---

### Android NDK (Native Development Kit)

- lehetővé teszi natív kód futtatását
- C++

---

### Android ADK (Accessory Development Kit)

- támogatás android kiegészítő eszközök gyártásához (dokkoló, egészségügyi eszközök, időjárás kiegészítő eszközök stb.)
- Android Open Accessory Protocol (USB & Bluetooth)

---

## Fejlesztőeszköz: Android Studio

- 2013 májusában került bemutatásra Google I/O-n
- IntelliJ IDEA alapú fejlesztőkörnyezet
- Windows, OSX, Linux támogatás
- Plugin fejlesztési lehetőség
- Fejlett refaktor képességek
- Teljeskörű támogatás
- Emulátorral

---

## Hello World Androidban

```Java
public class HelloAndroid extends Activity {
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		TextView tv = new TextView(this);
		tv.setText("Hello World!");
		setContentView(tv);
	}

}
```

### ugyanez XAML-el:

```XAML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android=
 "http://schemas.android.com/apk/res/android"
 android:layout_width="match_parent"
 android:layout_height="match_parent"
 android:orientation="vertical" >

	<TextView
		android:id="@+id/tvHello"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:text="@string/hello" />
</LinearLayout>
```

```java
import android.app.Activity
import android.os.Bundle
import android.widget.TextView

public class HelloWorldActivity extends Activity {
	@Override
	public void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.main);
	TextView myTextView = (TextView)findViewById(R.id.tvHello);
	myTextView.append("\n--MODIFIED--"); }
	
}
```
