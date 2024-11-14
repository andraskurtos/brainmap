---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-14 14:24
---
# Lekérdezés Optimalizálás
## Válaszidőt befolyásoló tényezők

- I/O költség
	- Adatbázisokban meghatározó
	- Moore törvény nem igaz rá
	- Speciális kezelési módok
	- Írás, olvasás, pozicionálás (seek)
- CPU használat
	- Komplex lekérdezések
	- Összetett számítások
- Memóriahasználat
	- Cache hatás

## Optimalizálás alapelvek

- Statisztikák alapján értékel
	- Költség = Válaszidő (CPU + I/O idő)
- Triviális terv
	- Egyszerűbb lekérdezéshez egyértelműen generálható
	- Szabály alapú
- Ha nem készíthető triviális terv
	- Összetett lekérdezések
	- 3 fázisú optimalizáció

## Háromfázisú optimalizáció

0. fázis:
- Egyszerű átalakítások
- Preferált hash join
- Ha a költség < X -> végrehajtás

1. Fázis
- Kibővített átalakítások
- Ha a költség < Y -> végrehajtás

2. Fázis
- Párhuzamos végrehajtás vizsgálata

## Lekérdezés feldolgozás menete

- Elemző
	- Lekérdezés fordítása
	- Logikai terv készítése
- Optimalizáló
	- Fizikai terv elkészítése
	- Táblák bejárása
	- Táblák összekapcsolása
- Sorfordító
	- Fizikai terv leképezése I/O műveletek
- Végrehajtó
	- Műveletek végrehajtása


# [[MS SQL]] szerver logikai végrehajtási elv

## Elemző fa

- relációk (levél elemek)
- műveletek (csomópontok)
- adatok áramlása (lentről fölfelé)

## Relációs algebra műveletek

- Descartes-szorzat (RxS)
- Projekció ($\pi_{L}(R)$)
- Szelekció, kiválasztás
- Összekapcsolás
- Ismétlődés szűrése
- Csoportosítás
- Rendezés


