Correzione Errori su disponibiltà LGV

![[Pasted image 20240330023919.png]]

Modificare tutti i Value da 6000 a 20000

Per calcolare correttamente i tempi nella

Reporting.ProductionIntervalSummary

```sql
DECLARE @AlarmIdLgvRemoved TABLE (Id bigint)
INSERT INTO @AlarmIdLgvRemoved SELECT CONVERT(bigint, Value) as Value FROM Reporting.ufn_GetSettings (NULL,'AlarmId','AlarmIdRemoved')
INSERT INTO @AlarmIdLgvRemoved SELECT CONVERT(bigint, Value) as Value FROM Reporting.ufn_GetSettings (NULL,'AlarmId','AlarmIdRemovedInMission')
INSERT INTO @AlarmIdLgvRemoved SELECT CONVERT(bigint, Value) as Value FROM Reporting.ufn_GetSettings (NULL,'AlarmId','AlarmIdRemovedNotInMission')
```