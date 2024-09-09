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