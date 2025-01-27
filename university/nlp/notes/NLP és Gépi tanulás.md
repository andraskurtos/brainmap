---
aliases: 
tags:
  - nlp
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-26 13:00
---

# [[Természetesnyelv-feldolgozás|NLP]] és Gépi Tanulás

>[!summary]+ [[Gépi tanulás]]
>Olyan módszerek összefogó neve, melyek adatokból képesek önállóan modellt tanulni, amit korábban nem látott adatok esetében is sikeresen tudnak alkalmazni különféle feladatokban.
>
>Az így tanított modellt egy alkalmazásba építve számos területen használhatjuk: gépi látás, beszédfelismerés, döntéstámogatás, **természetesnyelv-feldolgozás**

## Amazon Alexa

Az Amazon Alexa egy az Amazon által fejlesztett asszisztens alkalmazás.

### Skill

A skill az Alexában egy képesség valamire, például tények lekérdezésére, otthonvezérlésre, időjárás lekérdezésére, stb.
Számos beépített képességgel rendelkezik, és továbbiakkal bővíthető az alkalmazásboltból. Saját képességeket is fejleszthetünk.

**Trigger word/Invocation name**: a képességet aktiváló *hívószó*
"Alexa" - az általános hívószó, ami aktiválja a rendszert
"radio" - internetes rádióállomások lejátszása

### Intent

Az intent a felhasználó *szándéka*: mit szeretne végrehajtani, milyen backend API funkciót aktiváljon a rendszer?

**Sample utterances**: azon kifejezések, amelyekkel felismerhető az Intent
"Alexa, turn on the radio", "Alexa, play last radio station"

**Slot**: az Intent *argumentuma*, amely szerepelhet a kifejezésekben
pl: melyik rádióállomást szeretné hallgatni a felhasználó
"Alexa, play {radio station}"

![[Pasted image 20241026131227.png]]

---

## Az eddig megismert nyelvi modellek

### Statisztikai (**korpuszalapú**) modellek

Szavak és kategóriák gyakorisága alapján, megfigyelésekből állítunk fel valószínűségi modelleket.

>[!warning]+ Problémák
>- rengeteg szó nagyon ritka, [[Zipf törvény]]
>- kevés megjelenés -> pontatlan modell
>- **szavak nem függetlenek**

>[!success]+ Lehetséges megoldások
>- tulajdonságtér-bővítés
>- szótári információk
>- morfológiai elemzés
>- kapcsolatbővítés
>- ontológiák
>- rejtett szemantika felderítése

### Tudásalapú modellek

A *tudásalapú modellek*et nyelvészeti és tárgyterületi szakértők készítik, nyelvtani szabályokkal és szemantikai bővítésekkel.

>[!warning]+ Problémák
>- a nyelv változékony (időben és térben)
>- elemzési algoritmusok hatékonysága és korlátai
>- **a modellépítés manuális**

>[!success]+ Lehetséges megoldások
>- alkalmazási terület korlátozása
>- kontrollált nyelv: szűk szókincs és szabálykészlet
>- szabályozott bemenet

---
## Gépi tanulásos NLP

A *Spacy* egy gépi tanulásra épülő NLP eszköz, ami sokféle NLP feladat megoldására használható: teljes NLP feldolgozóláncot építhetünk, entitásfelismerést, szófaji címkézést, klasszifikációt, függőség-elemzést valósíthatunk meg vele. Angol nyelven előre tanított modellekkel rendelkezik, továbbtanítása is lehetséges. Nyílt forráskódú alkalmazás, részletes dokumentációval.

### Formális modellek tanulása

> [!summary]+ "Treebank":
> - szintaktikailag és szemantikailag elemzett korpusz
> - Penn Treebank: 100000+ annotált elemzési fa

> [!summary]+ Gyakoriság-->Nyelvi modell:
> 
> - az egyes szimbólumok előfordulásai
> 	  pl ha 100 S csomópont alatt 60 *NP VP* található, akkor ezt tanuljuk: *S->NP VP \[0.6\]*
> - tömörítés, általánosítás
> 	  hasonló szerkezetű szabályok összevonása
> - az alacsony számosságú esetek is finomítandók

> [!warning] Nincs elég adat
> - nem ellenőrzött tanulás
>   pl: tananyagtalnulás, inkrementális nehezítés kétszavas-->háromszavas-->...
> - félig ellenőrzött tanulás
>   kis annotált adathalmaz-->nyelvtan-->kiterjesztés
> - más annotációk (pl HTML) alkalmazása

A szó kontextusa fontos: milyen más szavak fordulnak elő a környezetében?

>[!question]+ Hogyan építsünk erre nyelvi modellt?
> - Szó + kontextus --> nyelvi modell
> - Mekkora kontextus? Hogyan reprezentáljuk?
> - Milyen nyelvi modell? Mi a feladat?
> - Mekkora lesz a modellméret?
> - Karakteralapú kontextus és modell? Elszabadulhat a modellméret
> - Ismerjük a szavakat (szótár) <-> sokféle szóalak van (előfeldolgozás)

---

## Szóbeágyazások

### Hogyan állítsuk elő a modellt?

A modellt egyedi szó- vagy n-gram vektorokkal állítjuk elő. Nagy szókincs extrém ritka, hatalmas vektorokat eredményez.
	
Például  $n\space=\space?$  pl $10^5$ szó és 5-gram esetén $10^{25}$ dimenzió.

![[Pasted image 20241026134631.png]]

![[Pasted image 20241026134701.png]]


> [!summary] Elvárások és célok:
> - hasonló jelentésű szavak -> hasonló vektorok (rejtett szemantika megragadása)
> - keresési minta -> találati lista hasonlóság (információtartalom)
> - adott szósorozat után következő szó (mondatbefejezés, válaszgenerálás)
> - adott szósorozat hiányzó szava (helyesírás-ellenőrzés)
> - angol szósorozat->magyar szósorozat (gépi fordítás)
> - szavak jellemzői: szófaj, entitás, szintaktikai szerep (címkézés)

