```sql
DECLARE  @PositionLenght	INT = 1716		-- Position Lenght + StepDistance `
@LgvDropOffset		INT = 700		-- The distance beetween the operation point and the real drop position of pallets 
@DoubleRowOffset	INT = 0		-- Used to space the 2 rows of a locations. Must be 0 if location have only 1 row. 
UPDATE sp 
SET	 
Y = co.y + ROUND(SIN(co.Angle*pi()/180.0), 3 * ((sp.Position - 1) * (sp.Length + 100)) - @LgvDropOffset * ROUND(SIN(co.Angle*pi()/180.0), 3), 
X = co.x + ROUND(COS(co.Angle*pi()/180.0), 3) * ((sp.Position - 1) * (sp.Length + 100)) - @LgvDropOffset * ROUND(COS(co.Angle*pi()/180.0), 3), 
Angle = co.Angle 
FROM 
Lgvcs.StationPositions sp 
JOIN Lgvcs.Stations sl ON sl.StationId = sp.StationId 
JOIN Lgvcs.LgvStations lgv ON lgv.StationId = sl.StationId 
JOIN CC1559_SEContainer_SDM.SEContainer.TempLayoutPoints co ON lgv.LgvStationId = co.Id_Point```