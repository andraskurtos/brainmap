---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 20:11
---


# A Kvantummechanika Posztulátumai

## 1. Posztulátum: kvantumbit (Hilbert-tér)

>[!summary]+ Állapotleírás
>Zárt fizikai rendszer aktuális állapota olyan állapotvektorral írható le, amely komplex együtthatókkal rendelkezik, egységnyi hosszú a *H* Hilbert térben (egy komplex lineáris vektortérben, amelyben értelmezve van a belső szorzat). 

Ez a kvantumbit (Qubit).
$$
\ket{\phi}= a\ket{0} + b\ket{1}; \space a,b \in C \space és \space|a|^2+|b|^2 =1 
$$
A kvantumbit mindkét klasszikus (bázis) állapotot tartalmazza egyidőben: szuperpozíció.
$$
\ket{\phi} = a\ket{0}+b\ket{1} = \begin{bmatrix}
0 \\
1
\end{bmatrix}  + b \begin{bmatrix}
0 \\
1
\end{bmatrix} = \begin{bmatrix}
a \\
b
\end{bmatrix}
$$
ahol a és b komplex valószínűségi amplitúdók. Abszolútérték négyzetük adja meg a mérés eredményét (a klasszikus bázisban.)
$$
|a|^2+|b|^2=1
$$
>[!summary]+ Műveletek:
>
>Belső szorzat: $\braket{\cdot|\cdot}$
>A széleskörű alkalmazhatóság kulcsa az a megfigyelés, hogy ha a két összeszorzandó vektor koordinátáival adott:
> $a=[a_{1},a_{2},\dots,a_{n}]$ és $b=[b_{1},b_{2},\dots,b_{n}]$ akkor a skaláris szorzatuk éppen:
> $$
 a\cdot b=\sum_{i=1}^{n}a_{i}b_{i}=a_{1}b_{1}+a_{2}b_{2}+\dots+a_{n}b_{n}
>$$
>
>Külső szorzat: $\ket{\cdot}\bra{\cdot}$
>![[Pasted image 20241015203454.png]]


## 2. Posztulátum: logikai kapuk (Unitér transzformáció, elemi kvantum logikai kapuk)
## 3. Posztulátum: Q/C átalakítás (mérési statisztika, mérés utáni állapot)
## 4. Posztulátum: regiszterek (tenzor-szorzás)



