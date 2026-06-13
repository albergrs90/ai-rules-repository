# ⚡ Backend AI Rules: .NET Core & C#

Este documento establece los estándares de ingeniería y diseño de software obligatorios para el desarrollo de backends modernos con .NET Core y C#. La IA debe seguir estas pautas sin excepción para garantizar la consistencia, el rendimiento y la mantenibilidad del código.

---

## 1. 🏗️ Arquitectura y Diseño de Software

* **Clean Architecture / DDD:** Estructura las soluciones separando claramente las responsabilidades en capas lógicas: Dominio (*Domain*), Aplicación (*Application*), Infraestructura (*Infrastructure*) y Presentación (*WebAPI*).
* **Inyección de Dependencias (DI):** Registra todos los servicios en el contenedor de inversión de control (IoC) nativo utilizando los ciclos de vida adecuados (`Transient`, `Scoped`, `Singleton`). Queda prohibida la instanciación manual mediante `new` en componentes de lógica.
* **Controladores Delgados (Thin Controllers):** Mantén los controladores del API extremadamente limpios. Toda la lógica de negocio debe delegarse a servicios de aplicación, casos de uso o manejadores de comandos (patrón CQRS con MediatR).

---

## 2. 💻 Estilo de Código y C# Moderno

* **Nomenclatura Estándar de Microsoft:** Usa `PascalCase` para clases, interfaces (anteponiendo la `I`), métodos y propiedades públicas. Usa `camelCase` para parámetros de métodos y variables locales. Usa `_camelCase` con guion bajo para campos privados dentro de una clase.
* **Uso de C# Moderno:** Aprovecha las características más recientes del lenguaje como `record` para DTOs e inmutabilidad de datos, expresiones switch con coincidencia de patrones (*pattern matching*), bloques `using` simplificados y la característica de tipos de referencia nulos habilitados (`#nullable enable`).

### Ejemplo de DTO Inmutable

```csharp
namespace Application.DTOs;

public record UserDto(
    Guid Id, 
    string Email, 
    string FullName, 
    bool IsActive
);
```
## 3. 💾 Acceso a Datos con Entity Framework Core
Consultas Asíncronas: Utiliza siempre las variantes asíncronas para las operaciones de base de datos que involucren I/O (ToListAsync(), FirstOrDefaultAsync(), SaveChangesAsync()).

Optimización de Consultas (No-Tracking): Para operaciones estrictamente de lectura donde los datos no vayan a ser modificados ni persistidos, utiliza siempre .AsNoTracking() para mejorar el rendimiento y reducir el consumo de memoria.

Migraciones Controladas: No generes código que intente aplicar migraciones automáticamente en entornos productivos mediante context.Database.Migrate(). El despliegue de base de datos debe estar completamente desacoplado.

## 4. 🛡️ API Design, Validaciones y Seguridad
Respuestas Consistentes: Todos los endpoints del API deben retornar una estructura de respuesta homogénea, utilizando los códigos de estado HTTP correctos (200 OK, 201 Created, 400 BadRequest, 401 Unauthorized, 404 NotFound, 500 InternalServerError).

Validación de Datos Completa: Implementa la validación de peticiones de forma desacoplada de los controladores utilizando librerías como FluentValidation antes de procesar cualquier lógica de negocio.

## 🛑 Regla de Oro para la IA
Todo código generado en C# debe compilar con cero advertencias (warnings) bajo la configuración de #nullable enable. Nunca utilices métodos bloqueantes como .Result o .Wait() sobre tareas asíncronas, ya que esto puede ocasionar bloqueos de hilos (thread pool starvation). El código asíncrono debe ser asíncrono de principio a fin (async all the way).
