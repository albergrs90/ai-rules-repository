# 📝 Global AI Rules: Documentation Style

Este documento define los estándares obligatorios de documentación, comentarios en el código y formato de archivos Markdown. El asistente de IA debe asegurar que cualquier texto explicativo o estructura de documentación cumpla estrictamente con estas reglas.

---

## 1. 💻 Comentarios en el Código: Calidad sobre Cantidad

* **El código limpio se auto-documenta:** No comentes código obvio. Los comentarios deben explicar el **porqué** (la intención, decisiones arquitectónicas o lógica de negocio compleja) y no el **qué** (la sintaxis del lenguaje).
  * ❌ *Evitar:* `// Incrementa el contador en 1` -> `count++;`
  * *Permitir:* `// Compensación temporal por el desfase horaria de la API externa` -> `const adjustedTime = serverTime + 3600;`
* **Prohibido el Código Muerto:** Nunca dejes bloques de código comentados en el archivo final. Si una lógica ya no se usa, debe eliminarse por completo. El control de versiones (Git) ya se encarga de mantener el historial.

---

## 2. 📑 Estándares de Documentación Técnica

Cuando generes componentes públicos, clases, funciones exportables o endpoints, utiliza los formatos de documentación estándar del lenguaje.

### A. Entornos JavaScript / TypeScript (JSDoc)

Usa bloques `/** ... */` detallando el propósito, parámetros con sus tipos y el valor de retorno.

```typescript
/**
 * Calcula el total del carrito aplicando impuestos y descuentos acumulables.
 * * @param items - Listado de productos activos en el carrito.
 * @param discountCode - Cupón de descuento opcional a validar.
 * @returns Un objeto con el desglose del precio final.
 * @throws {ValidationError} Si el código de descuento ha expirado.
 */
export function calculateCartTotal(items: CartItem[], discountCode?: string): CartSummary { ... }
```
## B. Entornos .NET / C# (Comentarios XML)
Usa el formato triple barra /// para generar metadatos limpios que reconozca el IDE.

Procesa el pago de una orden mediante la pasarela integrada.

<param name="orderId">Identificador único de la orden activa.</param>
<param name="paymentDetails">Información cifrada del método de pago.</param>
<returns>Un resultado que indica el éxito o la razón del fallo de la transacción.</returns>
public async Task<PaymentResult> ProcessPaymentAsync(Guid orderId, PaymentDetails paymentDetails) { ... }

## 3. 🌐 Idioma y Localización
Código e Interfaces Técnicas: Todo el código fuente, nombres de variables, funciones, clases, tablas de bases de datos y bloques de documentación técnica (JSDoc, XML) deben escribirse estrictamente en Inglés.

Documentación de Cara al Usuario / READMEs: Las descripciones de los proyectos, guías de instalación rápida, manuales y archivos README deben estar disponibles en 3 idiomas (Español, Inglés y un tercer idioma relevante para el proyecto si se especifica; por defecto, Francés o Portugués) o estructurados de forma que el usuario pueda alternar fácilmente entre ellos.

## 4. 🗂️ Estructura de Archivos Markdown (.md)
Cuando crees o actualices archivos Markdown (como el README.md del proyecto final), mantén siempre esta estructura de formato:

Títulos Claros: Usa una jerarquía lógica de encabezados (#, ##, ###).

Reglas Horizontales: Separa las secciones principales usando --- para mejorar la legibilidad visual.

Énfasis Judicioso: Usa negritas para resaltar palabras clave, comandos o restricciones importantes. No abuses de ellas.

Listas y Tablas: Para comparar configuraciones, detallar variables de entorno o listar prerrequisitos, prioriza el uso de tablas limpias y listas con viñetas en lugar de bloques densos de texto.

## 🛑 Regla de Oro para la IA
Si el usuario te pide añadir un comentario descriptivo en el código, asegúrate de que use el formato correcto según el stack. Si el usuario te pide documentar un módulo entero, genera un archivo README.md o MIGRATION.md complementario en la carpeta correspondiente en lugar de saturar el archivo de código con texto instructivo.
