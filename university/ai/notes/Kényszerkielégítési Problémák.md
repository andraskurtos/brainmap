---
aliases:
  - kényszerkielégítési problémák
  - CSP
  - Constraint Satisfaction Problems
tags:
  - uni
  - ai
  - mi
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-25 18:40
---
	




# Kényszerkielégítési Problémák

A kényszerkielégítési problémák *speciális felfedező módszerek*.

>[!summary]+ Kényszerkielégítési probléma definíciója
>
>- $X$ változók egy halmaza: $\{X_{1},X_{2},\dots,X_{n}\}$
>- $D$ tartományok egy halmaza: $\{D_{1},D_{2},\dots,D_{n}\}$
>- $D_{i}=\{v_{1},v_{2},\dots,v_{k}\}$ az $X_{i}$ változóhoz.
>- $C$ kényszerek egy halmaza:
>	- A változók által felvehető értékek megengedett kombinációi.
>	- $C_{j}= <\text{hatáskör},\text{reláció}>$, ahol
>	  - hatáskör = változók rendezett halmaza (amelyekre a kényszer vonatkozik)
>	   - reláció = az értékek, amelyeket a változók felvehetnek (explicit felsorolás v. implicit függvény)

>[!example]+ I. Példa: Térképszínezés
>![[Pasted image 20240925185442.png]]
>
>**Változók:** $X={WA,NT,Q,NSW,V,SA,T}$
>**Értéktartományok:** $D_{i}=\{\text{vörös},\text{zöld},\text{kék}\}$
>**Kényszerek:** szomszédos területekszíne legyen eltérő
>$C=\{SA\neq WA,SA\neq NT,SA\neq Q\dots,NSW\neq V\}$
>
>**Megoldás:** **teljes** és **konzisztens** változó-érték **hozzárendelés**
>
>![[Pasted image 20240925190202.png]]

>[!example]+ II. Példa: N-királynő probléma
>**Változók:** $X=\{Q_{1},Q_{2},\dots,Q_{N}\}$
>**Értéktartományok:** $D_{i}=\{1,2,\dots,N\}$
>**Kényszerek:**
>Implicit: $\forall\text{ i,j } (Q_{i},Q_{j})$ nem "támadják egymást" - nincsenek egy sorban, illetve egy átlóban egymással
>Explicit:
>	  $(Q_{1},Q_{2})\in \{(1,3),(1,4),\dots\}$
>	  $(Q_{1},Q_{3})\in \{(1,2),(1,4),\dots\}$
>	  $(Q_{1},Q_{4})\in \{(1,2),(1,3),\dots\}$
>	  $\dots$

>[!info]+ Gyakorlati kényszerkielégítési problémák
>- Hozzárendelési problémák, pl. ki, mit, hol tanítson
>- Menetrendi problémák, pl. vasút
>- Szállításütemezés
>- Gyári ütemezés
>- Áramkörtervezés

---

## A CSP problémák típusai

### ***Változók alapján***

*Diszkrét változók:*

- **véges** értéktartományok
	- pl: logikai típusú CSP, Boole-féle kielégítési vizsgálatok (NP-teljes)
- **végtelen** értéktartományok:
	- egész számok, karaktersorozatok, stb.
	- pl: ütemezési problémák, változók a munkaszakaszok kezdete/vége

*Folytonos változók:*

- fizikai állapotváltozók
- pl. Hubble-űrteleszkóp megfigyeléseinek kezdő/vég-időpontja

---
### ***Kényszerek alapján***

*Unáris* kényszer: egyetlen változóra vonatkozik, pl. SA != zöld
*Bináris* kényszer: két változó viszonyára vonatkozik, pl SA != WA
*Magasabb rendű* kényszer: 3 vagy több változó viszonyára vonatkozik (pl. oszloponkénti változó korlátok betűrejtvényekben, Sudoku kényszerei).
*Preferencia*-kényszer: Egy érték jobb(an preferált), mint egy másik, pl. a vörös jobb, mint a zöld
Gyakran változó hozzárendelés költségeként reprezentálják ==> korlátozott optimalizációs problémák
##
>[!info] Kényszergráfok
>A kényszer[[Gráf|gráf]] [[Csomópont|csomópontjai]] a változók, és [[Él|élei]] a bináris kényszerek. Itt csak bináris *CSP*-k: egy-egy kényszer legfeljebb 2 változót köt össze.
>
>![[Pasted image 20240925193424.png]]

## Kényszerkielégítési problémák megoldása


>[!error]+ A probléma megfogalmazása
>
>**Kezdeti állapot**: összes változó-hozzárendelés üres {}
>
>**Operátor**: értéket hozzárendelni egy még nem lekötött változóhoz, úgy, hogy az eddigi hozzárendelésekkel ne ütközzön
>                   -> *kudarc, ha nincs megengedett hozzárendelés*
> 
> **Célállapotteszt**: ha az aktuális hozzárendelés teljes, és minden kényszer teljesül
> 
> **Keresési fa**:
> $n$ db változó esetén minden megoldás $n$ mélységben fekszik
>              --> mi történik, ha a **mélységi keresést** használjuk?
>   Elágazások száma $L$ mélységben $(L=0,1,2,\dots)$, ha minden változó $d$ számú értéket vehet fel (ez a változó tartományának mérete)
>   $b=(n-L)d$, (mert $L$ változó már értéket kapott)
>   azaz $n!*d^n$ levélcsomópont
>   (miközben $d^n$ hozzárendelés van)
>   
>   *(pl 8 betűs betűrejtvény, $n=8,\space d=10, \space\text{levelek száma }4\cdot10^{12}$)*

---
## [[Keresés]]

Itt a változó-hozzárendelés **kommutatív**, azaz például
(WA = **vörös**) majd (NT=**zöld**) $\equiv$ (NT=**zöld**), majd (WA=**vörös**)

Egy-egy csomópontban csakis egy változó hozzárendelése történhet meg:
--> $b=d$ és a fának $d^n$ levele van.

>[!info]+
>Alapvető nem informált algoritmus (keresés CSP) problémák megoldására:
>*[[Visszalépéses Keresés|visszalépéses keresés]]* (backtracking search), azaz:
>**mélységi keresés**
>- minden szinten egyetlen egy változó-hozzárendeléssel
>- ha sérül valamelyik kényszer, visszalép
>(egyszer sérült kényszer mélyebben nem jöhet helyre)

>[!summary]+ [[Visszalépéses Keresés|Visszalépéses keresés]] algoritmus
>
>![[Pasted image 20240926151827.png]]


>[!danger]+ Néhány gyengeség
>
>- Tekintsük az itt látható részleges hozzárendelést
>  
>![[Pasted image 20240926152015.png]]
>
>- A [[Csomópont|csomópontokat]] lentről felfelé, balról jobbra járjuk be
>- Vajon a jelenlegi hozzárendelés kudarcra van ítélve?
>- A naiv [[Visszalépéses Keresés|visszalépéses keresés]] túl későn veszi észre a problémát

>[!success]+ A [[Visszalépéses Keresés|visszalépéses keresés]] hatékonyságának növelése
>
>- Általános, tárgyterület-független heurisztikákkal növelhetjük a hatékonyságot
>- Szűrés:
>	- Ki tudjuk korán szűrni a kudarcra ítélt megoldásokat?
>- Sorrendezés:
>	- **Változók sorrendezése**: Melyik változóhoz rendeljünk értéket a következő lépésben?
>	- **Értékek sorrendezése**: Melyik értéket rendeljük hozzá először a változóhoz?
>- Struktúra:
>	- Ki tudjuk használni a probléma struktúráját?


