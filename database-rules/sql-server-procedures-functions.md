# 🛠️ Database AI Rules: SQL Server Stored Procedures & Functions

Este documento establece las directrices obligatorias para la creación, optimización y uso de Procedimientos Almacenados (Stored Procedures) y Funciones (UDFs) en Microsoft SQL Server. El asistente de IA debe aplicar estas reglas para asegurar la seguridad, la reutilización de código y evitar bloqueos de rendimiento.

---

## 1. ⚙️ Procedimientos Almacenados (Stored Procedures)

* **Uso Obligatorio para Lógica Compleja:** Toda operación que involucre múltiples pasos, mutaciones de datos (Inserts/Updates correlacionados) o lógica de negocio en la base de datos debe encapsularse en un *Stored Procedure* parametrizado.
* **Control de Errores Estricto:** Todo procedimiento que modifique datos debe implementar obligatoriamente bloques `BEGIN TRY...END TRY` y `BEGIN CATCH...END CATCH`, asegurando que cualquier fallo revierta los cambios mediante un `ROLLBACK`.
* **Configuración del Entorno:** Incluye siempre la directiva `SET NOCOUNT ON;` al inicio del procedimiento. Esto evita que SQL Server envíe mensajes de conteo de filas afectadas al cliente por cada instrucción, reduciendo el tráfico de red y mejorando la velocidad de ejecución.

### Ejemplo de Procedimiento Almacenado Estructurado

```sql
CREATE PROCEDURE Dbo.UpsertUserActivity
    @UserId INT,
    @ActivityDescription NVARCHAR(255)
AS
BEGIN
    SET NOCOUNT ON;
    
    BEGIN TRY
        BEGIN TRANSACTION;

        -- Lógica de negocio encapsulada
        IF EXISTS (SELECT 1 FROM Dbo.Users WHERE Id = @UserId)
        BEGIN
            INSERT INTO Dbo.UserLogs (UserId, Description, LoggedAt)
            VALUES (@UserId, @ActivityDescription, GETUTCDATE());
        END
        ELSE
        BEGIN
            THROW 51000, 'El usuario especificado no existe.', 1;
        END

        COMMIT TRANSACTION;
    </BEGIN TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;

        -- Propagar el error de forma segura
        THROW;
    END CATCH
END;
```
## 2. ❌ Prohibición de Funciones Escalares en Consultas Masivas
El Problema del Rendimiento (RBAR): Queda estrictamente prohibido el uso de Funciones Escalares definidas por el usuario (CREATE FUNCTION ... RETURNS <tipo_dato>) dentro de las cláusulas SELECT o WHERE en consultas que procesen grandes volúmenes de datos. SQL Server las ejecuta fila por fila (Row-By-Agonizing-Row), lo que destruye el paralelismo y degrada el rendimiento del procesador.

Alternativa Obligatoria: Si se necesita reutilizar lógica de consulta o cálculo, la IA debe transformarla obligatoriamente en una Función de Tabla en Línea (Inline Table-Valued Function o iTVF). SQL Server puede integrar estas funciones directamente en el plan de ejecución general como si fuesen una vista dinámica.

## Ejemplo de Alternativa Correcta (iTVF)

-- ❌ Evitar: Función escalar que calcula el neto por cada fila de forma aislada
-- ✔️ Correcto: Función de tabla en línea (Inline TVF)

CREATE FUNCTION Dbo.Fn_GetNetBalanceInline (@TaxRate DECIMAL(4,2))
RETURNS TABLE
AS
RETURN
(
    SELECT 
        Id,
        TotalAmount,
        (TotalAmount * (1 - @TaxRate)) AS NetAmount
    FROM Dbo.Orders
);

## 3. 🛡️ Prevención de Inyección SQL Dinámica
Parametrización Obligatoria: Prioriza siempre las consultas estáticas parametrizadas. Si la naturaleza del requerimiento obliga a la IA a construir consultas dinámicas (por ejemplo, filtros de búsqueda altamente variables), queda terminantemente prohibido concatenar variables directamente en cadenas de texto (SET @Sql = 'SELECT * FROM Tbl WHERE Id = ' + @UserParam).

Uso Obligatorio de Sp_executesql: Todo SQL dinámico permitido debe ejecutarse utilizando estrictamente el procedimiento del sistema sp_executesql, declarando y pasando los parámetros de forma segura y tipada.

## 🛑 Regla de Oro para la IA
Los procedimientos almacenados en SQL Server reutilizan los "Planes de Ejecución" basados en los primeros parámetros que reciben (Parameter Sniffing). La IA debe estructurar las consultas internas evitando el uso de variables locales intermedias que oculten los valores reales al optimizador de consultas, a menos que se requiera explícitamente un hint como OPTION (RECOMPILE) para mitigar un problema específico de distribución de datos.
