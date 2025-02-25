---
aliases:
  - MSSQL
  - Microsoft SQL Server
tags:
  - 5semester
  - adatvez
  - datadriven
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-09-17 13:22
---





# MS SQL

## Microsoft SQL szerver Platform

Az [[Adatbázis|adatbázis]] definíció szerint logikailag összefüggő adatok rendezett gyűjteménye. Az [[Adat|adatok]] ebben a definícióban *ismert tények, melyek számítógépes adattárolón rögzíthetők*. 

> [!example]- 
> Az adatok lehetnek *hagyományosak*: szöveg, dátum, szám,
> *multimédiások*: kép, hang, videó,
> vagy *strukturáltak*: [[XML]], [[JSON]].


Az *MS SQL* a [[T-SQL]] nyelvet használja a [[Adatbázisok Szerveroldali Programozása|szerveroldali programozás]] megvalósítására.

## MS SQL adatszótár

### Information Schema Views

```tsql
INFORMATION_SCHEMA.
	TABLES, VIEWS, COLUMNS, PARAMETERS, TABLE_PRIVILEGES, ...
```

### Catalog Views

Teljeskörű információ a szerverről.

```t-sql
sys.
	databases, database_files, filegroups, messages, schemas,
	objects, tables, columns, foreign_keys, ... 
```

### Dynamic Management Views

Szerver diagnosztikai információk

```t-sql
sys.dm_tran_locks
sys.dm_exec_cached_plans
sys.dm_exec_sessions
```

### Adatszótár példa:
```sql
select * from sys.tables
select * from INFORMATION_SCHEMA.TABLES
select * from sys.objects
select * from INFORMATION_SHEMA.COLUMNS
```