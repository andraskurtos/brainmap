---
created: 20240901 23:53
tags:
  - 5semester
  - ai
  - mi
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# A* keresés

Lényegében az [[Egyenletes költségű keresés]] és a [[Mohó keresés]] egyesítése. Az egyenletes költségű keresés az útvonalakat az útköltség alapján rendezi: visszamenőleges költség - $g(n)$. A mohó keresés pedig a cél közelsége alapján választ: előremutató költség - $h(n)$. Lényegi pontja az [[Heurisztikus keresés#Elfogadható heurisztika|elfogadható heurisztikák]] kialakítása.

---

### **optimalitása**

feltevések:
- "A" az optimális célcsomópont
- "B" a szuboptimális célcsomópont
- h elfogadható
állítás:
- "A" előbb kerül ki a peremből, mint B

![[Pasted image 20240909183910.png]]

Bizonyítás:

- Tegyük fel, hogy "*B*" a peremen található
- Továbbá, hogy "*A*" valamely "*n*" őse szintén a peremen található (vagy maga "*A*" is)
- Állítás: "*n*" kifejtésére előbb kerül sor, mint "*B*" kifejtésére
	1. f(n) <= f(A)
	2. f(A) <= f(B)
	3. "*n*" előbb kerül kifejtésre, mint "*B*"
==> "*A*" minden őse előbb kerül kifejtésre, mint "*B*"
==> "*A*" kifejtésre kerül "*B*" előtt
==> **A* keresés optimális**

---
### **alkalmazásai**

- videójátékok
- útvonalkeresési problémák
- erőforrástervezési problémák
- robotmozgás tervezése
- nyelvi elemzés
- gépi fordítás
- beszédfelismerés
- ...
