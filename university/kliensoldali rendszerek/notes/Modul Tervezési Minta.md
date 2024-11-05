---
aliases:
  - modul tervezési minta
  - module design pattern
  - Module Design Pattern
  - Module Pattern
  - module pattern
tags:
  - 5semester
  - uni
  - web
  - client
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-25 15:09
---





# Modul Tervezési Minta

A *modul tervezési minta* segítségével ([[JavaScript]]) kódunkat egy minden mástól független névtérbe csomagolhatjuk, elkerülve a globális névtér szennyezését.

>[!example]+ Példa
>```JavaScript
>var N = (function () {
>	var priValt = 3;
>	var priFV = function () {alert('Privát!');};
>	return {
>		pubValt: 5,
>		pubFv: function () {alert('Publikus!');}
>	};
>})();
>
>N.pubFv();
>alert(N.pubValt);
>```

A [[konstruktor]] függvény segítségével a modulnak adhatunk bemeneti paramétereket (*import*), a return kulcsszó után pedig azt határozhatjuk meg, hogy kívülről mi látszik a modulból (*export*). 

>[!tip]+ Akár több fájlba is darabolhatjuk:
>Az *import-export* lehetőségnek köszönhetően a modulunk kódját akár több fájlba is tehetjük, a publikus tagok el fogják érni egymást.
>
>```JavaScript
>var N = (function (my) {
>	my.pubFv = function () { alert( 'Publikus' ); };
>}( N || {} ));
>
>// Másik fájl
>var N = ( function (my) {
>	my.pubValt = 5;
>	return my;
>}( N || {} ));
>
>// Felhasználás
>N.pubFv();
>alert(N.pubValt);
>```

