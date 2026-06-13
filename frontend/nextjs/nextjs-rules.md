# 🚀 Frontend AI Rules: Next.js (App Router) & TypeScript

Este documento establece las directrices obligatorias de arquitectura, rendimiento y desarrollo para proyectos construidos con Next.js utilizando el App Router y TypeScript. El asistente de IA debe seguir estas reglas rigurosamente.

---

## 1. 📂 Arquitectura de Carpetas y Enrutado

* **App Router Estricto:** Toda la lógica de enrutado debe residir dentro de la carpeta `app/`. Usa grupos de rutas (`(marketing)`, `(dashboard)`) para organizar secciones sin alterar la URL si es necesario.
* **Componentes Fuera de `app/`:** La carpeta `app/` solo debe contener archivos de enrutado (`page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`). Los componentes reutilizables de UI deben almacenarse en una carpeta externa raíz (ej: `components/`, `ui/`).
* **Nomenclatura de Rutas:** Usa `kebab-case` para los nombres de las carpetas que representan segmentos de ruta (ej: `app/user-profile/page.tsx`).

---

## 2. 🖥️ Server vs. Client Components

* **Server Components por Defecto:** Todos los componentes son React Server Components (RSC) por defecto. Mantén la lógica de obtención de datos (`fetch`) y operaciones pesadas en el servidor para reducir el bundle de JavaScript enviado al cliente.
* **Uso Explicito de `'use client'`:** Añade la directiva `'use client'` en la primera línea del archivo **únicamente** si el componente requiere:
  * Hooks de estado o ciclo de vida (`useState`, `useEffect`, `useReducer`).
  * Hooks de enrutado del cliente (`useRouter`, `usePathname`).
  * Listeners de eventos interactivos nativos (`onClick`, `onChange`).
* **Hojas de Cliente Atómicas:** No transformes una página entera en componente de cliente. Si solo un botón o un formulario necesita interactividad, extrae esa lógica a un componente hijo marcado con `'use client'`.

---

## 3. 🔄 Obtención de Datos y Server Actions

* **Async Server Components:** Realiza las peticiones de datos directamente en los componentes de servidor usando funciones asíncronas (`async/await`).
* **Server Actions para Mutaciones:** Usa Server Actions (`'use server'`) para manejar envíos de formularios, mutaciones de bases de datos o llamadas protegidas a APIs externas. Nunca expongas endpoints de mutación si pueden resolverse con una acción nativa.

### Ejemplo de Estructura de Server Action

```typescript
// app/actions/todo.ts
'use server';

import { revalidatePath } from 'next/cache';

export async function createTodoAction(formData: FormData) {
  const title = formData.get('title');
  
  if (!title || typeof title !== 'string') {
    return { error: 'El título es obligatorio.' };
  }

  // Lógica para guardar en base de datos o API externa
  // await db.todo.create({ data: { title } });

  // Revalidar la caché de la página para mostrar los cambios
  revalidatePath('/kanban');
  return { success: true };
}
```
## 4. ⚡ Optimización de Rendimiento y SEO
Componente Image Obligatorio: Queda prohibido usar la etiqueta nativa <img>. Usa siempre next/image configurando propiedades width, height, alt descriptivos y priority para imágenes del Above the Fold (LCP).

Fuentes Optimizadas: Usa next/font/google para cargar fuentes personalizadas de manera local y automática, evitando layouts shifts (CLS).

Metadatos Dinámicos: Configura el SEO exportando objetos metadata estáticos o utilizando la función generateMetadata para rutas dinámicas.

## 🛑 Regla de Oro para la IA
Next.js gestiona de forma agresiva el almacenamiento en caché. Al generar rutas o componentes, la IA debe evaluar correctamente si la página debe ser Estática (SSG), Dinámica (SSR) o utilizar Revalidación (ISR), aplicando force-dynamic o revalidate según los requerimientos de tiempo real de los datos del usuario.
