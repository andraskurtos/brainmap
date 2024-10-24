---
aliases: 
tags:
  - 5semester
  - adatvez
  - datadriven
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-24 20:45
---


# Entity Framework

Az *entity framework* egy [[Objektum-relációs Leképezés|ORM]] rendszer, amely lehetővé teszi a *logikai* ([[Adatelérési réteg|adatbázis]]) és a *fogalmi* ([[Üzleti Logika Réteg|üzleti logika]]) modellek szétválasztását. Segítségével függetleníthetjük alkalmazásunkat az adatbázismotortól.

>[!warning]+ Entity Framework vs. Entity Framework Core
>![[Pasted image 20241024204918.png]]

---

## Entitások

A modell által leírt elemek az *entitás típusok*. Ezekhez az adatmodell *tulajdonságokat* rendel, de viselkedésüket nem definiálja. Az entitások más entitásokhoz *relációkkal* kapcsolódhatnak.

---

## Adatbázis

Az EF nem rendelkezik közvetlen ismeretekkel az adatbázismotorról, és annak típusa nincs közvetlen hatással működésére. Az entitásokon végzett műveleteket egy Provider fordítja le a motor műveleteire.

>[!example]+ Néhány támogatott szolgáltató:
>- Microsoft SQL Server, CE, Cosmos DB
>- SQLite, Postgres, IBM Data Servers, MySQL
>- Oracle
>- InMemory adatbázis teszteléshez

>[!summary]+ Leképezési módszerek
>![[Pasted image 20241024205358.png]]

## Database First leképezés

*Database First* leképezés esetén rendelkezünk egy meglévő adatbázissal, melyet tetszőleges DB eszközzel elkészítettünk. A kódot ez alapján generáljuk,