## **daily routine tasks**:
```dataview
TASK
FROM #daily 
WHERE date(file.name) = date(today)
AND !completed
```
