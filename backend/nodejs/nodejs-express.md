# 🟢 Backend AI Rules: Node.js, Express & TypeScript

Este documento establece las directrices obligatorias de arquitectura, seguridad y desarrollo para servicios backend construidos con Node.js y Express utilizando TypeScript. El asistente de IA debe seguir estas pautas estrictamente.

---

## 1. 🏗️ Arquitectura Modular y Estructura

* **Separación de Capas:** Estructura el proyecto siguiendo el patrón de arquitectura de tres capas: Controladores (*Controllers*), Servicios (*Services* / Lógica de negocio) y Acceso a Datos (*Models* / Repositorios).
* **Rutas Desacopladas:** No definas la lógica de los endpoints directamente en el archivo principal (`app.ts` o `server.ts`). Utiliza el enrutador nativo de Express (`express.Router()`) para modularizar las rutas por entidad.
* **Inyección de Configuración:** Lee todas las variables de entorno al arrancar la aplicación en un archivo centralizado de configuración (ej: `config/env.ts`) utilizando librerías como `dotenv`. Valida que las variables requeridas existan antes de levantar el servidor.

---

## 2. 🛡️ Seguridad y Middlewares Obligatorios

* **Middlewares Esenciales:** Todo servidor Express debe inicializarse utilizando los siguientes middlewares de seguridad básicos:
  * `helmet()` para proteger la aplicación de vulnerabilidades web conocidas configurando cabeceras HTTP adecuadas.
  * `cors()` configurado explícitamente con los orígenes permitidos (prohibido usar `*` en producción).
  * `express.json({ limit: '10kb' })` para limitar el tamaño del cuerpo de las peticiones y evitar ataques de denegación de servicio (DoS).
* **Validación de Esquemas:** Valida el cuerpo (`body`), parámetros (`params`) y consultas (`query`) de todas las peticiones entrantes antes de que lleguen al controlador, utilizando librerías como `Zod` o `Joi`.

---

## 3. ⚙️ Manejo de Errores y Asincronía

* **Control de Promesas (Async/Await):** Utiliza funciones asíncronas para operaciones I/O. Todas las funciones controladoras asíncronas deben capturar sus errores de manera que nunca se caiga el proceso de Node.
* **Manejador Global de Errores:** Implementa un middleware centralizado de manejo de errores al final de la pila de Express `(err, req, res, next)`. Los controladores deben pasar los errores capturados a `next(error)`.
* **Clase de Error Personalizada:** Crea una clase que extienda de `Error` (ej: `AppError`) para manejar errores operacionales con códigos de estado HTTP específicos.

### Ejemplo de Controlador y Manejo de Errores

```typescript
import { Request, Response, NextFunction } from 'express';
import { UserService } from '../services/user.service';

export const getUserProfile = async (
  req: Request, 
  res: Response, 
  next: NextFunction
): Promise<void> => {
  try {
    const userId = req.params.id;
    const user = await UserService.findById(userId);
    
    if (!user) {
      res.status(404).json({ 
        status: 'fail', 
        message: 'Usuario no encontrado.' 
      });
      return;
    }

    res.status(200).json({ 
      status: 'success', 
      data: { user } 
    });
  } catch (error) {
    next(error); // Delega al middleware de error global
  }
};
```
## 4. 📝 Rendimiento y Buenas Prácticas
No Bloquear el Event Loop: Evita realizar operaciones síncronas pesadas de CPU (fs.readFileSync, bucles masivos de filtrado) dentro de la ruta de ejecución de una petición.

Tipado Estricto de Express: Utiliza siempre los tipos nativos de TypeScript para Express (Request, Response, NextFunction, RequestHandler) para asegurar un correcto autocompletado y prevenir errores de tipado.

## 🛑 Regla de Oro para la IA
Node.js es monohilo. La IA debe asegurar que ningún error asíncrono quede sin capturar (unhandled rejections), ya que esto puede comprometer la estabilidad del servidor entero. Todo flujo asíncrono debe resolverse con estructuras sólidas de control de excepciones y los recursos abiertos (conexiones a bases de datos o streams) deben cerrarse correctamente.
