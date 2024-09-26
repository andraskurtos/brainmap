---
aliases:
  - visszalépéses keresés
tags:
  - uni
  - 5semester
  - ai
  - mi
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-26 15:32
---





# Visszalépéses Keresés


A *visszalépéses keresés* egy [[Neminformált keresés|neminformált keresés]]i algoritmus [[Kényszerkielégítési Problémák|kényszerkielégítési problémák]] megoldására. Gyakorlatilag a [[Mélységi keresés|mélységi keresés]] egy speciális esete, minden szinten *egy-egy változóhozzárendeléssel*, és valamely *kényszer sérülése esetén visszalép.*

>[!summary]+ A Visszalépéses Keresés algoritmusa
>
>![[Pasted image 20240926151827.png]]

>[!success]+ A [[Visszalépéses Keresés|visszalépéses keresés]] hatékonyságának növelése
>
>- Általános, tárgyterület-független heurisztikákkal növelhetjük a hatékonyságot
>- [[Visszalépéses Keresés#Szűrés|Szűrés]]:
>	- Ki tudjuk korán szűrni a kudarcra ítélt megoldásokat?
>- Sorrendezés:
>	- **Változók sorrendezése**: Melyik változóhoz rendeljünk értéket a következő lépésben?
>	- **Értékek sorrendezése**: Melyik értéket rendeljük hozzá először a változóhoz?
>- Struktúra:
>	- Ki tudjuk használni a probléma struktúráját?

---

## Szűrés

### előretekintő ellenőrzés


Az **előretekintő ellenőrzés** minden egyes alkalommal, amikor egy X változó értéket kap, minden, az X-hez kényszerrel kapcsolt, lekötetlen Y-t megvizsgál, és Y tartományából *törli* az X számára választott értékkel *inkonzisztens értékeket*.

>[!warning]+ Korlátai
>- Az előretekintő ellenőrzés ugyan sok inkonzisztenciát észrevesz, de *nem mindet*.
>- Ráadásul nem látja jól előre a kudarcokat.
