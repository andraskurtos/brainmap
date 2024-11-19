---
aliases: 
tags:
  - quantum
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-19 18:33
---

# Kvantum ZH2 kérdések

![[ZH kérdések]]

>[!question]- 1. Hogyan szólnak a kvantummechanika posztulátumai mérnöki interpretációban?
>1. Kvantumbit - Kvantumrendszereket állapotvektorokkal írjuk le, amely komplex együtthatókkal rendelkezik.
>2. Logikai kapuk - A kvantum logikai kapuk unitér transzformációkkal működik, azaz megőrzik az állapotok normáját
>3. Mérés (Q/C átalakítás) - A kvantumrendszerek mérésénél valószínűségi statisztikákat kapunk a mérési operátorok alkalmazásával
>4. Regiszterek (tenzor szorzás) - több kvantumbitből álló rendszerek leírására tenzorszorzást alkalmazunk. Egy két qubites rendszer pl így nézhet ki:
>	$$
>\ket{\psi} = \ket{0} \otimes \frac{\ket{0} +\ket{1} }{\sqrt{ 2 }}
>$$

>[!question]- 2. Mit értünk unitér transzformáció alatt?
>Az unitér transzformáció olyan lineáris transzformáció, amely megőrzi a vektorok hosszát és a belső szorzatokat.

>[!question]- 3. Mit értünk komplex valószínűségi amplitúdó ala"? Mit mutat meg számunkra?
>Ez az amplitúdó határozza meg, hogy egy adott kvantumrendszer egy bizonyos állapotban milyen valószínűséggel található meg. Abszolútértéknégyzetük adja meg a teljes mérés eredményét.

>[!question]- 4. Milyen műveletet értünk tenzorszorzás alatt? Számolja ki két megadott vektor tenzorszorzatát!
>A kvantuminformatikában a tenzorszorzattal több rendszer együttes állapotát tudjuk leírni.
> $$
>\ket{\psi} \otimes \ket{\phi} =\begin{bmatrix}a_{1}\\a_{2}
\end{bmatrix}\otimes \begin{bmatrix}
b_{1}\\b_{2}
\end{bmatrix} = \begin{bmatrix} a_{1}\cdot b_{1}\\a_{1}\cdot b_{2}\\a_{2}\cdot b_{1}\\a_{2}\cdot b_{2}
\end{bmatrix}
>$$ 

>[!question]- 5. Mit értünk belső szorzat alatt? Számolja ki két megadott vektor belső szorzatát!
>A kvantummechanikában a belső szorzattal számolható ki két kvantumállapot közti hasonlóság, avagy az állapotok vetülete egymásra.
> $$
> a\cdot b=\sum_{i=1}^{n}a_{i}b_{i}=a_{1}b_{1}+a_{2}b_{2}+\dots+a_{n}b_{n}
$$

>[!question]- 6. Miért fontos a belső szorzat meghatározása a kvantuminformatikában?
>Mert ennek segítségével meghatározhatjuk a két kvantumállapot közti hasonlóságot. $|\braket{ \psi | \phi }|^{2} }$

>[!question]- 7. Miért jó, hogy a kvantuminforma0kában unitér transzformációkat alkalmazunk?
>Mert így megmarad a vektorok belső szorzata, a szuperpozíció és interferencia, a műveletek fordíthatóak lesznek

>[!question]- 8. Mikor használhatunk projekJv mérést?
>Projektív mérést alkalmazunk, ha egy kvantumrendszer állapotát mérni szeretnénk, és biztosak akarunk lenni abban, hogy a mérés után jól meghatározott állapotba kerül. A mérési operátorok sajátállapotai megadják a lehetséges kimeneteleket. Alkalmazható kvantumszámításban, összefonódás lemérésében, és általában a kvantumrendszerek diszkrét mérési kimeneteleihez.

>[!question]- 9. Hogyan konstruáljuk a mérési operátorokat projektív mérés esetén?
>A projektív mérései operátorokat az alábbi módokon konstruáljuk:
>- kiválasztjuk a mérési operátort, ami Hermitikus
>- megtaláljuk a mérési operátor sajátállapotait és sajátértékeit
>- minden sajátértékhez projektort rendelünk, amely a megfelelő sajátállapotba vetíti a rendszert
>- a mérés után a rendszer állapotát ezen projektorok segítségével vetítjük, és meghatározzuk a mérési valószínűségeket

>[!question]- 10. Hogyan tudunk előállítani tetszőleges állapotú kvantumbitet? Rajzolja fel az áramkört!
>$$
>\ket{0} \to H \to P(\alpha) \to H \to P(0.5\pi+\beta) \to \ket{\phi}
>$$

>[!question]- Hogyan tudunk előállítani tetszőleges állapotú kvantumbitet? Vezesse le lépésről-lépésre az előállítást! (A klasszikus trigonometrikus azonosságokat nem kell bizonyítani/levezetni)
> $\ket{\phi_{0}}=\ket{0}$
>$\ket{\phi_{1}}=H\ket{0}=\frac{\ket{0} + \ket{1} }{\sqrt{ 2 }}$
>$\ket{\phi_{2}}=P(\alpha)\ket{\phi_{1}}=\frac{\ket{0}+e^{j\alpha}}{\sqrt{ 2 }}$
> $\ket{\phi_{3}}=H\ket{\phi_{2}}=\frac{\frac{\ket{0}+\ket{1}}{\sqrt{ 2 }}+e^{j\alpha}\frac{\ket{0}-\ket{1}}{\sqrt{ 2 }}}{\sqrt{ 2 }}=\frac{1+e^{ja}}{\sqrt{ 2 }}\ket{0}+\frac{1-e^{j\alpha}}{2}\ket{1}$

>[!question]- 12. Mi a szupersűrű tömörítés alapgondolata?
>Klasszikus információ továbbítása kvantumcsatornán át, összefonódás használatával.
>1. Összefonódott állapot előállítása:
>   Alice és Bob megosztanak egy Bell-párt
>2. Információ kódolása:
>   Alice egy klasszikus kétbitnyi információt kvantumműveletekkel (Pauli kapukkal) kódol az általa birtokolt qubitbe.
>3. Alice elküldi a kódolt qubitet Bobnak
>4. Bob, aki az összefonódott állapot másik qubitjével rendelkezik, elvégzi a megfelelő Bell-mérést, és visszanyeri a két klasszikus bit információt.

>[!question]- 13. Rajzolja fel a szupersűrű tömörítés blokkvázlatát, és mutassa be röviden a működését.
>1. egy kvantumkapu segítségével Bell-párt generálunk
>2. Pauli kapu alkalmazásával kódolunk
>3. Qubitet elküldjük egy klasszikus csatornán
>4. Bell-mérést végzünk
>![[Pasted image 20241119204543.png]]


>[!question]- 14. Milyen hátránya van a szupersűrűségű tömörítésnek?
>Használatához szükség van egy hagyományos csatornára, így nem használható ki az elméleti hatékonysága.

>[!question]- 18. Mit jelent a QKD rövidítés és mit értünk alatta?
>Quantum Key Distribution - kvantum kulcsszétosztás
>Szimmetrikus kulcsú titkosítás esetén eljuttathatjuk kvantumos módszerekkel a kulcsot egyik féltől a másikhoz. Ily módon a kulcsokat úgy egyeztethetjük, hogy mindenképpen észrevesszük, ha valaki hallgatózik.

>[!question]- 19. Hogyan működik a BB84 QKD protokoll?
![](https://www.youtube.com/embed/8hNQyTdNil4?si=XD5CW_agN94COpod)
> Fotonok polarizációs állapota a kvantumbit, ezt beszéljük meg utána hagyományos csatornán --> kulcs
> 
> 1. Alíz kulcsbiteket és bázisokat sorsol
> 2. A kulcsbiteket a bázisoban továbbítja
> 3. Bob mérési bázisokat sorsol
> 4. Bob mérési fázisokkal mér, és kap egy mérési eredményt
> 5. Ahol a bázisok megegyeznek, ott a küldött és mért bitértékek megegyeznek
> 6. Alíz és Bob nyilvános csatornán egyeztetik a bázisokat. Ahol ugyanazt a bázist használták, megtartják a bitértékeket.

>[!question]- 20. Mi garantálja azt, hogy észrevesszük a támadó jelenlétét a BB84 protokoll használata esetén?
>Támadó jelenléte esetén ő is bázist választ, majd a mért eredményeket továbbítja. Ha Alíz és Bob bázisa megegyezik, de Éváé nem, akkor a mért bitek nem fognak egyezni.

>[!question]- 21. Hogyan működik a B92 QKD protokoll?
>Bob és Alíz bitértéket beszéli meg nyivánosan, és a bázist tartják meg, így mindig tudják, ha le lettek hallgatva. A bázis szolgálhat kulcsként.

>[!question]- 22. Hogyan működik az E91 összefonódáson alapuló protokoll?
>

>[!question]- 23. Mik azok a DiVincenzo kritériumok?
>Megszabják, mit kell tudnia egy kvantumszámítógépnek
>1. Skálázható kvantumrendszer jól definiált qubitekkel
>2. Qubitek inicializálásának képessége
>3. Hosszú koherenciaidő
>4. Egyetemes kvantumkapuk megvalósítása
>5. Qubitek hatékony mérhetősége
>6. Qubitek összefonódott állapotának előállítása
>7. Qubitek átvitele