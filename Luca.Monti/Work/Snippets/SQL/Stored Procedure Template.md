```sql
USE [CC1641_Coster2_SDM]
GO
/****** Object:  StoredProcedure [Coster].[usp_CreatePalletFromOldSystem]    Script Date: 6/1/2022 9:09:21 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

ALTER PROCEDURE [Coster].[usp_StoredProcedure]
	@Parameter              NVARCHAR(50),
	@ResultCode				INT OUT, 
    @ResultText		        NVARCHAR(MAX) OUT
AS
BEGIN
	DECLARE @actProcName	NVARCHAR(255) = OBJECT_NAME(@@PROCID), 
			@trancount		INT = @@trancount ,
	
			@xstate			INT 

	DECLARE @sup TABLE(lpn	nvarchar(50) NOT null,Quantity INT NOT null)
	DECLARE @i INT
        
	SET @ResultCode = 0

	BEGIN TRY  
 
		
        -- Check parameters
        -- may return

		IF (@trancount = 0) 
			BEGIN TRANSACTION 
		ELSE                          
			SAVE TRANSACTION @actProcName; 

        -- do something

		
		if @ResultCode = 0 
		begin
			IF @trancount = 0  
				COMMIT; 
		end
		else
		begin
			set @xstate = XACT_STATE() 
			IF @xstate = -1 
				ROLLBACK; 
			IF @xstate = 1 AND @trancount = 0 
				ROLLBACK; 
			IF @xstate = 1 AND @trancount > 0 
				ROLLBACK TRANSACTION @actProcName; 
		end
		
		RETURN 0
	END   try

	BEGIN CATCH 
        DECLARE @Error_number INT = ERROR_NUMBER(), @Error_Procedure NVARCHAR(255) = ERROR_PROCEDURE() 
        DECLARE @Error_Message NVARCHAR(4000) = ERROR_MESSAGE()
		set @xstate = XACT_STATE() 
        IF @xstate = -1 
              ROLLBACK; 
        IF @xstate = 1 AND @trancount = 0 
              ROLLBACK; 
        IF @xstate = 1 AND @trancount > 0 
              ROLLBACK TRANSACTION @actProcName; 
        EXEC [Common].[usp_LogProcedureError] 
		RAISERROR ('%s: %d: %s', 16, 1, @Error_Procedure, @Error_number, @Error_Message) ;   
	END CATCH 
	
END
```
