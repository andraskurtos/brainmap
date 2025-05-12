---
aliases: 
tags:
  - ai
  - mi
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 14:10
---

# Döntési Hálók

A döntési háló lényegében egy [[Valószínűségi háló|Bayes-háló]] kiegészítve hasznosság és döntési csomópontokkal. Lehetővé teszi a döntések várható hasznosságának számítását.

Elemei:
- valószínűségi csomópontok
- új csomópont típusok

##### új csomópontok

>[!summary]+ Új csomópontok
>*Döntési csomópont* (téglalap): 
>- nincsenek szülök
>- megfigyelt evidenciaként működik
>
>*Hasznosság csomópont* (gyémánt):
>- döntési és véletlen csomópontoktól függ

>[!code] Számítás menete
>- Minden evidencia felvétele
>- Minden lehetséges módon állítsa be a döntési csomópontokat
>- Számítsa ki a hasznosságcsomópont szüleinek posteriori valószínűségét adott evidenciák mellett
>- Számítsa ki az egyes döntések várható hasznosságát

>[!example]+ Példa
>![[Pasted image 20241121141537.png]]

Legegyszerűbb esetben a lépések eredménye *determinisztikus*, így az optimális döntés minden s állapotban az az *a* lépés, amely az elérhető legnagyobb hasznosságú állapothoz vezet.

A racionális döntéshozókra vonatkozó axiómákból következik, hogy *nemdeterminisztikus* esetben az optimális döntés mindig azt az *a* lépést jelenti, amely maximalizálja a várható hasznosságot.


