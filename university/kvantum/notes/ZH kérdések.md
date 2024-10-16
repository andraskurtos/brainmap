---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 20:37
---





# Kvantum ZH kérdések

>[!question]- 1. Hogyan szólnak a kvantummechanika posztulátumai mérnöki interpretációban?
>1. Kvantum bit: Kvantumrendszereket egy állapotvektorokkal írjuk le, amely komplex együtthatókkal rendelkezik.
>2. Logikai kapuk - A kvantum logikai kapuk unitér transzformációkkal működnek, azaz megőrzik az állapotok normáját.
>3. Mérés (Q/C átalakítás): A kvantumrendszerek mérésénél valószínűségi statisztikákat kapunk a mérési operátorok alkalmazásával
>4. Regiszterek (tenzor szorzás): több kvantumbitből álló rendszerek leírására tenzorszorzást alkalmazunk. Egy két qubites rendszer pl így nézhet ki:
>   $$
> \ket{\phi}=\ket{0}\otimes \frac{\ket{0}+\ket{1}}{\sqrt{2}}  
>$$

>[!question]- 2. Mit értünk unitér transzformáció alatt?
>Az unitér transzformáció olyan lineáris transzformációt jelent, amely megőrzi a vektorok hosszát és a belső szorzatokat.

>[!question]- 3. Mit értünk komplex valószínűségi amplitúdó alatt? Mit mutat meg számunkra?
>Ez az amplitúdó határozza meg, hogy egy adott kvantumrendszer egy bizonyos állapotban milyen valószínűséggel található meg. Abszolút érték négyzetük adja meg a teljes mérés eredményét ( a klasszikus bázisban).

>[!question]- 4. Milyen műveletet értünk tenzorszorzás alatt? Számolja ki két megadott vektor tenzorszorzatát!
>A kvantummechanikában tenzorszorzás segítségével több kvantumrendszer együttes állapotát (pl két qubitet) tudjuk leírni. 
> $$
> \ket{\psi}\otimes \ket{\phi}=\begin{bmatrix}
> a_{1} \\
>a_{2}
>\end{bmatrix} \otimes \begin{bmatrix}
> b_{1} \\
>b_{2}
>\end{bmatrix} = \begin{bmatrix}
>a_{1}\cdot b_{1} \\
>a_{1}\cdot b_{2} \\
>a_{2}\cdot b_{1} \\
>a_{2}\cdot b_{2}
\end{bmatrix}
>$$

>[!question] 5. Mit értünk belső szorzat alatt? Számolja ki két megadott vektor belső szorzatát!
>A kvantummechanikában a belső szorzat segítségével számolható ki
 két kvantumállapot közti hasonlóság, vagy az állapotok "vetülete" egymásra. A belső szorzatot gyakran használják vektorok ortogonalitásának vagy normájának meghatározására. A széleskörű alkalmazhatóság kulcsa az a megfigyelés, hogy ha a két összeszorzandó vektor koordinátáival adott:
> $a=[a_{1},a_{2},\dots,a_{n}]$ és $b=[b_{1},b_{2},\dots,b_{n}]$ akkor a skaláris szorzatuk éppen:
> $$
 a\cdot b=\sum_{i=1}^{n}a_{i}b_{i}=a_{1}b_{1}+a_{2}b_{2}+\dots+a_{n}b_{n}
>$$

>[!question]- 6. Miért fontos a belső szorzat meghatározása a kvantuminformatikában?
>A belső szorzat kvantuminformatikában két állapot közti átfedést (használati arányosságot) adja meg. Ha $\ket{\psi}$ és $\ket{\phi}$ két kvantumállapot, akkor a belső szorzat $|\braket{\psi|\phi}|^2$ adja meg annak a valószínűségét, hogy a rendszer az $\ket{\psi}$ állapotból a $\ket{\phi}$ állapotba kerül.

>[!question]- Miért jó, hogy kvantuminformatikában unitér transzformációkat alkalmazunk?
>Az unitér transzformációk alkalmazása kvantuminformatikában azért fontos, mert megőrzik az információt, biztosítják a műveletek fordíthatóságát, és fenntartják a kvantummechanikai szuperpozíciót és interferenciát. Ezáltal lehetővé teszik a kvantumalgoritmusok sikeres végrehajtását, a kvantumbitek manipulációját, és az információ helyes feldolgozását és kiolvasását.

>[!question]- 8. Adja meg a Pauli-X, Pauli-Y és Pauli-Z kapu mátrixait!
> $$
> X = NOT = \begin{pmatrix}
>0&1 \\
>1&0
>\end{pmatrix}
>$$
>
>$$
>Y = \begin{pmatrix}
>0&-i\\i&0
>\end{pmatrix}
>$$
>
>$$
>Z = \begin{pmatrix}
>1&0\\0&-1
>\end{pmatrix}
>$$ 

>[!question]- 9. Adja meg a fázisforgató kapu mátrixát!
> $$
> Z = \begin{pmatrix}
> 1&0\\0&e^{j\alpha}
>\end{pmatrix}
>$$

>[!question]- 10. Adja meg a Hadamard-kapu mátrixát!
> $$
> H = \frac{1}{\sqrt{ 2 }} \times \begin{pmatrix}
>1&1\\1&-1
>\end{pmatrix}
>$$

>[!question]- 11. Adja meg a CNOT-kapu mátrixát!
> $$
> CNOT = \begin{bmatrix}
> 1&0&0&0 \\ 0&1&0&0 \\ 0&0&0&1 \\ 0&0&1&0
>\end{bmatrix}
>$$

>[!question]- 12. Mire jó a Bloch-gömb?
>Polárkoordinátás geometriai szemléltetés. A két komplex paraméterről áttérhetünk valódi gömbi paraméterezésre, az állapot egyértelműen jellemezhető az $R^3$ -beli gömb felszínének egy pontjával.

>[!question]- 13. Ha egy kvantumbitet két komplex valószínűségi amplitúdóval (ami 4 paramétert jelent a valós és képzetes rész miatt) és a bázisvektorokkal definiálunk, akkor miért elegendő csupán két paraméterrel (két szöggel) megadni a Bloch-gömbön?
>A globális fázis elhagyható, mivel nem befolyásolja a fizikai állapotot.
>Normálási feltétel megszorítja a kvantumbit amplitúdóit.
>Ennek eredményeként a kvantumbit fizikai állapota két szögparaméterrel leírható.

>[!question]- 14. Milyen állapotot kapunk, ha a Hadamard-kaput alkalmazzuk a $\ket{0}$ kvantumbiten? S ha a $\ket{1}$ -en?
> $$
> \ket{0}\to \frac{\ket{0}+\ket{1}}{\sqrt{2}} = \ket{+}  
>$$
>
>$$
>\ket{1}\to \frac{\ket{0} - \ket{1} }{\sqrt{ 2 }}=\ket{-}  
>$$

>[!question]- 15. Az összefonódás milyen kvantummechanikai jelenséget takar?
>Az összefonódás egy olyan kvantummechanikai jelenség, amelyben a részecskék állapotai szorosan összekapcsolódnak, függetlenül attól, milyen távol vannak egymástól. Ez pl. azt jelenti, hogy az első és a második qubit együtt olyan szuperpozícióban van, ahol vagy mintkettő $\ket{0}$ -ban van, vagy mindkettő $\ket{1}$ -ben, de nem lehet megmondani, melyikük van melyik állapotban, amíg mérést nem végzünk.

>[!question]- 16. Hány darab Bell-állapot van? Írja le ezeket!
> $$
>\ket{\beta_{00}}=\frac{\ket{00}+\ket{11}}{\sqrt{ 2 }}  
>$$
>
>$$
>\ket{\beta_{01}}=\frac{\ket{01}+\ket{10}}{\sqrt{ 2 }}
>$$
>
>$$
>\ket{\beta_{10}}=\frac{\ket{00}-\ket{11}}{\sqrt{ 2 }} 
>$$
>
>$$
>\ket{\beta_{11}}=\frac{\ket{01}-\ket{10}}{\sqrt{ 2 }}
>$$
>

 >[!question]- 17. Hogyan állítjuk elő a Bell-állapotokat?
 >A Hadamard kapu és a CNOT kapu segítségével egy egyszerű kezdeti állapotból (például $\ket{00}$) könnyen előállíthatóak Bell-állapotok.
 >1. Kezdőállapot: Kezdjük két qubit $\ket{00}$ állapotban.
 >2. Hadamarad kapu alkalmazása az első qubitre: A Hadamard kapu szuperpozíciót hoz létre az első qubiten.
 >3. CNOT kapu alkalmazása: A CNOT kapu a másoik qubitet feltételesen (az első qubit állapotától függően) átalakítja.
 
 >[!question]- 18. Kvantuminformatika és kvantumkommunikációs szemszögből milyen előnyei és hátrányai vannak az összefonódásnak?
 >**Előnyök:** kvantumkriptográfia, biztonság, kvantumteleportáció, a kvantumszámítógépek több számítási műveletet hajtanak végre párhuzamosan
 
 >[!question]- 19. Milyen kapcsolat van a |0⟩ és a |+⟩ állapot között?
 >A Hadamard kapu az, ami átalakítja a $\ket{0}$ állapotot $\ket{+}$ állapottá, és fordítva. A Hadamard kapu szuperpozícióba hozza a bitet.
 
 >[!question]- 20. Milyen kapcsolat van a $\frac{\ket{00}+\ket{11}}{\sqrt{ 2 }}$ és a $\frac{\ket{++}+\ket{--}}{\sqrt{ 2 }}$ állapotok között?
 >Ha mindkét qubitre alkalmazzuk a Hadamard trafót, akkor a bázistrafó a következőképpen alakul:
 > $$
> H\otimes H(\frac{\ket{00}+\ket{11}  }{\sqrt{2}})= \frac{\ket{++}+\ket{--}  }{\sqrt{ 2 }}
>$$
>Tehát a két állapot egymásba alakítható Hadamard kapukkal. Ez azt jelenti, hogy az első állapot a $\ket{00}$ és a $\ket{11}$ bázisállapotok szuperpozíciója, míg a második állapot ugyanez az összefonódott állapot a $\ket{+}$ és $\ket{-}$ bázisában kifejezve.

>[!question]- 23. Hogy néz ki a mérés posztulátuma projektív mérés esetén?
>A mérés posztulátuma azt írja le, hogyan változik egy kvantumrendszer állapota méréskor. és milyen valószínűséggel kapjuk meg a mérési eredményeket. A Hermitikus operátor sajátállapotaihoz tartoznak projektorok $P_{i}$. Ezek az operátorok az egyes mérési kimenetelekhez tartozó projektív mérésekhez tartoznak.
>
>Annak a valószínűsége, hogy a kvantumállapot $\ket{\psi}$ a $\ket{m_{i}}$ sajátállapotra projektálódik:
> $$
> P(m_{i}|\psi)=\braket{ \psi|P_{i} | \psi } = |\braket{  m_{i}|\psi}|^{2}
>$$ 

>[!question]- 24. Mikor használhatunk projektív mérést?
>Projektív mérést alkalmazunk, mikor egy kvantumrendszer állapotát mérni szeretnénk, és biztosak akarunk lenni abban, hogy a mérés után jól meghatározott állapotba kerül. A mérési operátor sajátállapotai adják meg a lehetséges kimeneteket. Alkalmazható kvantumszámításban, összefonódás mérésében, és általában a kvantumrendszerek diszkrét mérési kimeneteleihez.

>[!question]- 25. Hogyan konstruáljuk a mérési operátorokat projektív mérés esetén?
>A projektív mérési operátorokat az alábbi módon konstruáljuk:
>- kiválasztjuk a mérési operátort, ami Hermitikus
>- megtaláljuk a mérési operátor sajátállapotait és sajátértékeit.
>- minden sajátértékhez projektort rendelünk, amely a megfelelő sajátállapotba vetíti a rendszert.
>- A mérés után a rendszer állapotát ezen projektorok segítségével vetítjük, és meghatározzuk a mérési valószínűségeket.

>[!question]- 26. . Miért lehet elegendő a projektív mérés elvégzése akkor is, ha nem ortogonálisak a megmérendő vektorok?
>