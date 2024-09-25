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
# EGYENLETES KÖLTSÉGŰ [[Keresés|KERESÉS]]


![[Pasted image 20240909173903.png]]


Stratégia: a legkisebb költségű csomópont kifejtése. A perem prioritásos sor, ahol a prioritás a kumulált költségnek felel meg. Kifejti az összes olyan csomópontot, ami *kevesebb költséggel elérhető, mint a legkisebb költségű megoldás*.

Ha a megoldás $C^*$ költségű, és az élek legalább $\epsilon$ költségűek, akkor az effektív mélység megközelítőleg: $C^* /\epsilon$

![[Pasted image 20240909174638.png]]

A szükséges idő $O(b^{C^*/\epsilon})$ (az effektív mélységben exponencionális). A perem tárhelyigénye megközelítőleg ezzel arányos.

Az algoritmus teljes, feltéve, hogy a legjobb megoldás véges költségű, és a minimum élköltség pozitív.
Optimális.

---

### Problémái:

- egyre növekvő költségű kontúrokat fejt ki
- jó hír: *teljes és optimális*
- rossz hír: 
	- minden irányban feltár megoldási opciókat
	- nem rendelkezik információval a célállapot helyzetéről

