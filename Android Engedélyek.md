---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 17:48
---





# [[Android]] Engedélyek

Veszélyes/kritikus/személyes adatokat érintő műveletekhez a felhasználó engedélyére lehet szükség. Az új permission modellel a 6-os androidtól felfele lehetőség van futási időben engedélyt kérni, a [[Java]] kódból.

>[!example]+ A permission meglétének ellenőrzése:
>
> ```java
> int permissionCheck =
> 	ContextCompat.checkSelfPermission(thisActivity,
> 	Manifest.permission.WRITE_CALENDAR);
> ```

>[!hint]+ Permissionkérés Activity-ben:
>
>1. Megvizsgálni, hogy van-e engedély?
>2. Ha szükséges, elmagyarázni a felhasználónak, miért kérjük, majd újrakérni.
>3. Permission kérés eredményének kezelése egyesével: 
>	- Engedélyezés
>	- Tiltás

Az engedélyeknek két típusa van, a *Normal* és a *Dangerous permissions*.  A felhasználó az adott engedélyeket bármikor visszavonhatja a beállításokban.

![[Pasted image 20241015175518.png]]



