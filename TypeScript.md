---
aliases:
  - TS
  - typescript
tags:
  - uni
  - 5semester
  - web
  - client
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-25 15:51
---
# TypeScript


A *TypeScript* egy ingyenes és open-source programnyelv a Microsoft által fejlesztve, amely a [[JavaScript|JavaScriptet]] egészíti ki típusossággal és valódi osztály alapú objektum-orientáltsággal. Gyakorlatilag típusos JavaScript, ahol a típusok opcionálisak, ezért minden [[JavaScript|JS]] egyben *TS* is, de amint beírunk egy típust valahova, már csak *TS*. 

## típusok

A típust TypeScriptben a változó neve után írjuk.

>[!example]+ Példa
>```ts
>function A(a,b) { // .js vagy .ts fájl is lehetne
>	return a+b;
>}
>
>function A(a: number, b: number) { // csak .ts fájlban lehet
>	return a+b;
>
>}
>```

>[!question]+ Miért fontosak a típusok?
>*Kezdetleges dokumentáció*
>- Sokszor lehet következtetni, hogy mit csinál
>	- névből [[JS]]/TS
>	- Paraméterek nevéből [[JS]]/TS
>	- Paraméterek típusából - csak TS!!
