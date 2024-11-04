---
aliases: 
tags:
  - 5semester
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-04 13:35
---

# XPath

Az *XPath* egy standard [[XML]] lekérdező nyelv. Feltételeink alapján lekérdezi a dokumentum egy részét, majd visszatér egy csomóponttal / boolean / szám / szöveg adattal. Szorosan kapcsolódik az [[XSLT]] -hez. 

Példaadat:

![[Pasted image 20241104133906.png]]


| parancs                       | jelentés                                                  |
| ----------------------------- | --------------------------------------------------------- |
| /                             | Gyökértől kezdve                                          |
| //                            | Aktuális csomóponttól kezdve bármelyik leszármazottban    |
| .                             | Aktuális csomópont                                        |
| ..                            | Szülő csomópont                                           |
| @nev                          | Adott nevű attribútum                                     |
| /konyvtar/konyv[1]            | Valahányadik gyerek (1-től indexelt)                      |
| /konyvtar/konyv[last()]       | Utolsó gyerek                                             |
| /konyvtar/konyv[position()<3] | Első kettő gyerek                                         |
| //cim[@nyelv='en']            | Olyan title elem, melynek a nyelv attribútuma 'en' értékű |

>[!example]+ Példák:
>
>### //konyv
>![[Pasted image 20241104134315.png]]
>
>### //cim
>![[Pasted image 20241104134333.png]]
>
>### //@nyelv
>![[Pasted image 20241104134350.png]]
>
>### /konyvtar/konyv[1]
>![[Pasted image 20241104134411.png]]
>
>### /konyvtar/konyv[ar>5000]
>![[Pasted image 20241104134440.png]]
>
>### /konyvtar/konyv/ar[text()]
>![[Pasted image 20241104134510.png]]
>




