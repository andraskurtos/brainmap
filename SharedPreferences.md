---
aliases:
  - datastore
  - sharedpreferences
  - DataStore
  - Shared Preferences
  - shared preferences
  - Shared preferences
tags:
  - 5semester
  - mobweb
  - android
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 16:54
---


# SharedPreferences / Datastore


## SharedPreferences

A *SharedPreferences* alaptípusok tárolására alkalmas kulcs-érték páronként. Fájlban tárolódik, de ezt elfedi az [[Android]] operációs rendszer. Létrehozáskor beállítható a láthatósága:
- ***MODE_PRIVATE***: csak a saját alkalmazásunk érheti el
- ***MODE_WORLD_READABLE***: csak a saját alkalmazásunk írhatja, bárki olvashatja
- ***MODE_WORLD_WRITABLE***: bárki írhatja és olvashatja
A SharedPreferences megőrzi tartalmát az alkalmazás vagy a telefon újraindítása esetén is. Ideális olyan adatok tárolására, melyek primitív típussal könnyen reprezentálhatók ()

