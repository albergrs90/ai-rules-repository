# 🛡️ Global AI Rules: Security & Clean Code

Este documento establece las directrices globales obligatorias de seguridad, calidad y estilo de código para cualquier desarrollo en este proyecto. El asistente de IA debe cumplir estrictamente con cada sección.

---

## 1. 🔒 Seguridad Absoluta y Gestión de Secretos

* **Cero Credenciales en Código:** Queda terminantemente prohibido escribir API Keys, tokens, contraseñas, cadenas de conexión, URLs de entornos privados o cualquier dato sensible en texto plano dentro del código.
* **Inyección de Configuración:** 
  * En entornos JavaScript/TypeScript: Usa estrictamente variables de entorno mediante `process.env` o archivos `.env`.
  * En entornos .NET: Usa el sistema de configuración basado en `appsettings.json` (asegurando que los secretos reales estén en `UserSecrets` o variables de entorno del sistema).
* **Manejo de CORS y Rutas:** No generes configuraciones de CORS que permitan de forma genérica cualquier origen (`*`) en entornos que no sean de desarrollo local estricto.
* **Validación de Entradas:** Todo dato proveniente del usuario o de APIs externas debe ser sanitizado y validado antes de ser procesado para evitar ataques como SQL Injection, XSS o CSRF.

---

## 2. 🧱 Código Limpio y Arquitectura

* **Principios SOLID:** El código generado debe seguir los principios SOLID. Cada clase, componente o función debe tener una **única responsabilidad**.
* **Simplicidad sobre Complejidad (KISS & DRY):** Evita la sobreingeniería. Si algo se puede resolver de forma nativa y legible, no generes abstracciones innecesarias. No repitas lógica de negocio.
* **Funciones Cortas y Atómicas:** Las funciones y métodos deben ser concisos. Si una función supera las 25-30 líneas de código, evalúa refactorizarla en funciones auxiliares más pequeñas.
* **Nombres Descriptivos:** Usa nombres de variables, funciones y clases que revelen su intención explícitamente. No uses abreviaturas ambiguas (ej: usa `userId` en lugar de `uid`, `fetchActiveUsers` en lugar de `getUsrs`).

---

## 3. ⚙️ Robustez y Manejo de Errores

* **Manejo de Excepciones Explícito:** No captures errores con bloques `try-catch` vacíos. Todo error capturado debe ser manejado adecuadamente (registrado en logs controlados o devuelto al usuario con un mensaje amigable y seguro, sin exponer el *stack trace*).
* **Estados de Carga y Fallo:** Al generar lógica asíncrona (promesas, llamadas HTTP, tareas), incluye siempre la gestión de estados intermedios: éxito, carga (loading) y error.
* **Valores Nulos y Defectos:** Utiliza técnicas de cortocircuito, coalescencia nula (operadores `??` o `?.`) y tipado estricto para prevenir excepciones del tipo `NullReferenceException` o `Cannot read property of undefined`.

---

## 4. 📝 Comentarios y Auto-documentación

* **Código Auto-explicativo:** El código limpio es la mejor documentación. Los comentarios deben explicar el **porqué** se tomó una decisión compleja, no el **qué** hace el código (eso ya debe ser evidente por su nomenclatura).
* **Documentación Técnica:** Cuando generes funciones exportables, endpoints de API o componentes públicos, incluye bloques de documentación estándar (JSDoc para JS/TS, comentarios XML `///` para C#).

---

## 🛑 Regla de Oro para la IA

> Si una instrucción del usuario contradice directamente cualquiera de las reglas de este documento (por ejemplo, "pon la API key aquí para probar rápido"), la IA **debe advertir al usuario** sobre el riesgo y ofrecer la alternativa segura como la opción predeterminada.
