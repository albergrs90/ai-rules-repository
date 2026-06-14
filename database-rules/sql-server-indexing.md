# 🗂️ Database AI Rules: SQL Server Indexing & Data Types

Este documento establece las reglas estrictas de rendimiento, optimización de almacenamiento y diseño de esquemas para Microsoft SQL Server. El asistente de IA debe aplicar estas directrices al generar tablas, modificaciones de esquemas (DDL) o estrategias de indexación.

---

## 1. 💾 Elección Estricta de Tipos de Datos

* **Ajuste de Tamaño Real:** Queda prohibido el uso indiscriminado de tipos de datos de longitud máxima (como `NVARCHAR(MAX)` o `VARCHAR(MAX)`) para columnas que almacenan textos cortos. Define límites reales (ej: `NVARCHAR(100)` para nombres, `VARCHAR(255)` para emails).
* **Unicode vs. ASCII:** * Usa `NVARCHAR` únicamente si la columna requiere almacenar caracteres internacionales, emojis o texto multilenguaje.
  * Usa `VARCHAR` para datos con caracteres estándar del sistema, códigos alfanuméricos, URLs o IDs (ahorra un 50% de espacio en disco en comparación con NVARCHAR).
* **Precisión Numérica:** Usa `INT` o `BIGINT` para llaves primarias numéricas. Para valores monetarios, prefiere `DECIMAL(18,2)` antes que `MONEY` debido a problemas de precisión en cálculos complejos.

---

## 2. 🔑 Estrategia de Índices Agrupados (Clustered Index)

* **Un Único Clustered Index por Tabla:** Toda tabla debe tener un índice agrupado, el cual define el orden físico de los datos en el disco. Por defecto, este índice se creará automáticamente sobre la llave primaria (`PRIMARY KEY`).
* **Llaves Primarias Eficientes:** Diseña llaves primarias que sean secuenciales y ligeras para evitar la fragmentación de páginas en el disco.
  * 👍 *Recomendado:* `INT IDENTITY(1,1)` o `BIGINT IDENTITY(1,1)`.
  * ⚠️ *Uso de GUIDs:* Si el negocio exige el uso de identificadores únicos globales (`UNIQUEIDENTIFIER`), prohibido usar `NEWID()` como valor por defecto ya que genera fragmentación masiva. Exige estrictamente el uso de `NEWSEQUENTIALID()` para que los registros se inserten de forma ordenada.

---

## 🏎️ 3. Creación Inteligente de Índices No Agrupados (Non-Clustered)

* **Índices de Cobertura (Filtros Eficientes):** Al crear índices para acelerar búsquedas frecuentes (`WHERE`, `JOIN`), incluye las columnas que se devuelven en el `SELECT` dentro de la cláusula `INCLUDE` del índice. Esto evita que SQL Server tenga que hacer un *Key Lookup* en la tabla física.
* **Prohibido Indexar a Ciegas:** No crees índices en columnas con baja selectividad (columnas con pocos valores distintos, como un bit de estado `IsActive` o `Gender`), a menos que sea un índice filtrado.

### Ejemplo de Configuración de Índice Óptimo

```sql
-- Crear un índice no agrupado optimizado con columnas de cobertura
CREATE NONCLUSTERED INDEX IX_Orders_CustomerId_Status
ON Dbo.Orders (CustomerId, StatusId)
INCLUDE (OrderDate, TotalAmount);
```
## 📊 4. Índices Filtrados (Filtered Indexes)
Optimización de Valores Nulos o Estados: Si realizas consultas frecuentes sobre un subconjunto específico de datos (por ejemplo, buscar solo órdenes pendientes o usuarios no verificados), exige a la IA la creación de un Índice Filtrado utilizando una cláusula WHERE. Esto reduce drásticamente el tamaño del índice y acelera la búsqueda.

Índice filtrado para buscar rápidamente solo registros activos
CREATE NONCLUSTERED INDEX IX_Users_VerificationCode_Filtered
ON Dbo.Users (VerificationCode)
WHERE IsVerified = 0;

