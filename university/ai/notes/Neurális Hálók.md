---
aliases: 
tags:
  - uni
  - 5semester
  - ai
  - mi
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-10 13:47
---
yaa# Neurális hálók

## Regressziós modellek

>[!summary]+ Lineáris modell
>- A bemenetek **változó** értékek
>- Minden változónak van egy súlya
>- A kimenet az összegzett eredmény (**lineáris kombináció**)
> $y=\sum_{i}w_{i}\cdot f_{i}(x)=w\cdot f(x)$

![[Pasted image 20241210135029.png]]


Hiba:
$$
total\space error= \sum_{i}\left( y_{i}-\sum_{k}w_{k}f_{k}(x_{i}) \right)^2
$$

Hiba minimalizálása:
tfh csak egy **x** pontunk van, **f(x)** jellemzőkkel, **y** célértékkel és **w** súlyokkal

$$
error(w)=\frac{1}{2}\left( y-\sum_{k}w_{k}f_{k}(x) \right)^2
$$
$$
\frac{\nabla error(w)}{\nabla w_{m}}=\left( y-\sum_{k}w_{k}f_{k}(x) \right)f_{m}(x)
$$
$$
w_{m} \leftarrow w_{m} + \alpha \left( y-\sum_{k}w_{k}f_{k}(x) \right)f_{m}(x)
$$

## Lineáris osztályozók

- A bemenetek **változó** értékek
- Minden változónak van egy **súlya**
- Az összetett eredmény (lineáris kombináció) az **aktivácó**

$$
activation_{w}(x) = \sum_{i} w_{i}\cdot f_{i}(x) = w\cdot f(x)
$$
Kimenet az aktiváció szerint:
- pozitív, output: +1
- negatív, output: -1

## Döntési szabályok

Bináris döntési szabály

A jellemzővektorok terében
- a minták a pontok
- bármely súlyvektor egy hipersík
- egyik oldal megfelel Y=+1-nek
- a másik Y=-1-nek

### Bináris perceptron

Kezdetben: Súlyok = 0
Minden tanítási mintához:
- Osztályozás az aktuális súlyok alapján
$$
y = \{ +1 \space if \space w \cdot f(x) \geq 0
$$
$$
y = \{ -1 if w\cdot f(x) < 0
$$
- Ha helyes (azaz y=y*), nincs változás!
- Ha rossz: állítsa be a súlyvektort
$$
w=w+y^*\cdot f
$$
### Többosztályos

Ha több osztályunk van:
- Minden osztály súlyvektora $w_{y}$
- Az y osztály (aktivációja): $w_{y}\cdot f(x)$
- Az az előrejelzés nyer, amelyiknek legmagasabb az aktivációja
      $y = \arg \max_{y} w_{y} \cdot f(x)$

>[!summary]+ A perceptronok tulajdonságai
>- Elválaszthatóság: igaz, ha egyes paraméterek alapján a modell tökéletesen szeparálja a mintákat
>- Konvergencia: ha a tanító minta elválaszható, a perceptron végül konvergál (bináris esetben)

>[!warning]+ Problémák a perceptronnal
>- Zaj: ha az adatok nem különíthetők el, a súlyok "*kidőlhetnek*"
>- Középszerű általánosítás: "alig" elválasztható megoldást talál
>- Túltanulás: a teszt/validációs adathalmazon a modell pontossága általában emelkedik, majd csökken (egyfajta túlilleszkedés)


## Valószínűségi döntések

ha nem elválasztható

Aktiváció: $z =w\cdot f(x)$
	Ha $z =w\cdot f(x)$ pozitív -> a valószínűség 1-re közelítsen
	Ha $z =w\cdot f(x)$ negatív -> a valószínűség 0-ra közelítsen

Szigmoid függvény:
$$
\phi(z)=\frac{1}{1+e^{-z}}
$$
Legjobb w?
Maximum likelihood becslés:

$$
\max_{w} ll(w) = \max_{w} \sum_{i}\log P(y^{(i)}|x^{(i)};w)
$$
$$
P(y^{(i)}=+1|x^{(i)};w) = \frac{1}{1+e^{-w\cdot f(x^{(i)})}}
$$
$$
P(y^{(i)}=-1|x^{(i)};w) = 1-\frac{1}{1+e^{-w\cdot f(x^{(i)})}}
$$
