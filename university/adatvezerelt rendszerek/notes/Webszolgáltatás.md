---
aliases: 
tags:
  - 5semester
  - adatvez
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 14:46
---

# Webszolgáltatások

##### Service Oriented Architecture

>[!summary]+ Service Oriented Architecture
>- Szolgáltatásorientált architektúra
>- Szolgáltatásokból építkezünk
>	- Heterogén környezetben is
>	- Webszolgáltatás
>	- Egy szolgáltatás kliense lehet
>		- egyszerű alkalmazás
>		- másik szolgáltatás
>
>**Alkalmazásszigetek helyett egymással együttműködő szolgáltatások**

![[Pasted image 20241215145105.png]]
##### Webszolgáltatás fogalma

>[!summary]+ Webszolgáltatás fogalma
>1. Művelethalmazt publikál más szolgáltatások/kliensek számára
>2. Hálózati üzenetekkel lehet kommunikálni vele
>3. Szabványos interfész/kommunikáció
>4. Függetlenül telepíthető, verziózható, tetszőleges technológiával megvalósítható egység
>
>![[Pasted image 20241215145528.png]]

## SOA - SOAP és REST

Az SOA/Webszolgáltatások megvalósítására két megközelítés terjedt el, a [[SOAP]] (Simple Object Access Protocol) alapú webszolgáltatások, valamint a [[REST]] megközelítés

Mindkettő esetében kulcsgondolat az "interop" - együttműködés különböző platformok között.

További elterjedt alternatíva a GraphQL, illetve a gPRC.