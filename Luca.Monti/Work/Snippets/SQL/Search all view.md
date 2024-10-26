```sql
CREATE PROCEDURE dbo.SearchAllViewsAndOrTables
  @SearchTerm     nvarchar(255) = NULL,
  @DatabaseList   nvarchar(128) = NULL,
  @SearchTables   bit = 1,
  @SearchViews    bit = 1
AS
BEGIN
  SET NOCOUNT ON;
  SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; 

  IF @SearchTerm IS NULL OR @SearchTerm NOT LIKE N'%[^%^_]%'
  BEGIN
    RAISERROR(N'Please enter a valid search term.', 11, 1);
    RETURN;
  END 

  CREATE TABLE #results
  (
    [database]     sysname,
    [schema]       sysname,
    [object]       sysname, 
    [column]       sysname,
    ArbitraryValue nvarchar(1000)
  );

  DECLARE @DatabaseCommands  nvarchar(max) = N'', 
          @ColumnCommands    nvarchar(max) = N'';

  SELECT @DatabaseCommands = @DatabaseCommands + N'
    EXEC ' + QUOTENAME(name) + '.sys.sp_executesql 
        @ColumnCommands, N''@SearchTerm nvarchar(255)'', @SearchTerm;'
    FROM sys.databases AS d
    WHERE database_id  > 4  -- non-system databases  
      AND [state]      = 0  -- online 
      AND user_access  = 0  -- multi-user
      AND -- database list is empty or database is in the list
      (
        (LEN(COALESCE(@DatabaseList,'')) = 0)
        OR
        (
          EXISTS
          (
            SELECT 1 FROM STRING_SPLIT(@DatabaseList, N',') AS f
              WHERE LOWER(d.name) = LOWER(LTRIM(RTRIM(f.value)))
          )
        )
      );

  DECLARE @q char(1) = char(39);
  
  SET @ColumnCommands = N'DECLARE @q nchar(1) = nchar(39),
      @SearchCommands nvarchar(max);
      
    SET @SearchCommands = N''DECLARE @VSearchTerm varchar(255) = @SearchTerm;'';
    
    SELECT @SearchCommands = @SearchCommands + char(13) + char(10) + N''
      SELECT TOP (1)
        [db]     = DB_NAME(),
        [schema] = N'' + @q + s.name + @q + '', 
        [table]  = N'' + @q + t.name + @q + '',
        [column] = N'' + @q + c.name + @q + '',
        ArbitraryValue = LEFT(CONVERT(nvarchar(max), '' + QUOTENAME(c.name) + ''), 1000) 
      FROM '' + QUOTENAME(s.name) + ''.'' + QUOTENAME(t.name) + ''
      WHERE '' + QUOTENAME(c.name) + N'' LIKE @'' + CASE 
        WHEN c.system_type_id IN (35, 167, 175) THEN ''V'' 
        ELSE SPACE(0) END + ''SearchTerm;''  
    FROM sys.schemas AS s
    INNER JOIN sys.objects AS t
    ON s.[schema_id] = t.[schema_id]
    INNER JOIN sys.columns AS c
    ON t.[object_id] = c.[object_id]
    WHERE c.system_type_id IN (35, 99, 167, 175, 231, 239)
      AND c.max_length >= LEN(@SearchTerm)
      AND t.type IN (' + QUOTENAME(LEFT('U', @SearchTables), @q)
                 + ',' + QUOTENAME(LEFT('V', @SearchViews),  @q) + N');

    PRINT @SearchCommands; -- for debugging later

    EXEC sys.sp_executesql @SearchCommands, 
      N''@SearchTerm nvarchar(255)'', @SearchTerm;';

  PRINT @ColumnCommands;   -- for debugging now

  INSERT #Results
  (
    [database],
    [schema],
    [object],
    [column],
    ArbitraryValue
  )
  EXEC sys.sp_executesql @DatabaseCommands, 
    N'@ColumnCommands nvarchar(max), @SearchTerm nvarchar(255)', 
    @ColumnCommands, @SearchTerm;
    
  SELECT [Searched for] = @SearchTerm;

  SELECT [database],[schema],[object],[column],ArbitraryValue 
    FROM #Results 
    ORDER BY [database],[schema],[object],[column];
END
GO
```