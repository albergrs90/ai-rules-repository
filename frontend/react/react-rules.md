# ⚛️ Frontend AI Rules: React & TypeScript

Este documento define las reglas estrictas de desarrollo para componentes, arquitectura, tipado y gestión de estado en aplicaciones React utilizando TypeScript. El asistente de IA debe asegurar su cumplimiento absoluto.

---

## 1. 📂 Estructura de Componentes y Archivos

* **Un Componente por Archivo:** Cada archivo debe exportar un único componente principal como exportación nominal (`export const MyComponent = ...`). Evita exportaciones por defecto (`export default`).
* **Co-localización:** Mantén los archivos relacionados juntos. Si un componente utiliza subcomponentes específicos, funciones auxiliares o estilos que no se comparten en ningún otro lugar, colócalos en la misma carpeta del componente.
* **Nomenclatura:** Usa `PascalCase` para archivos de componentes (`UserCard.tsx`) y `camelCase` para hooks, utilidades y funciones (`useAuth.ts`).

---

## 2. 🏗️ Estilo de Código y Funcionalidad

* **Componentes Funcionales con Arrow Functions:** Define siempre los componentes utilizando funciones flecha tipadas o infiriendo el tipo de retorno `JSX.Element`.
* **TypeScript Estricto:** Prohibido el uso de `any`. Define interfaces o tipos explícitos para las `props`, estados y valores de retorno de funciones y hooks personalizados.
* **Desestructuración de Props:** Desestructura las propiedades directamente en la firma del componente o en la primera línea del mismo, definiendo valores por defecto de manera limpia.

### Ejemplo de Componente Estructurado

```typescript
interface ButtonProps {
  label: string;
  variant?: 'primary' | 'secondary';
  onClick: () => void;
}

export const Button = ({ 
  label, 
  variant = 'primary', 
  onClick 
}: ButtonProps): JSX.Element => {
  return (
    <button className={`btn btn-${variant}`} onClick={onClick}>
      {label}
    </button>
  );
};
```
## 3. 🎣 Manejo de Estado y Hooks
Reglas de los Hooks: Respeta estrictamente el orden y las reglas de ejecución de los React Hooks (no llamarlos dentro de condicionales o bucles).

Memoización Inteligente: Utiliza useMemo y useCallback únicamente cuando haya un beneficio real de rendimiento medible (por ejemplo, al pasar dependencias a otros hooks o componentes hijos optimizados con React.memo). No los añadas de forma indiscriminada.

Separación de Conceptos: Extrae la lógica compleja de efectos (useEffect) o de negocio pesado a Hooks Personalizados (custom hooks) independientes para mejorar la legibilidad y la capacidad de testeo.

## 4. 🎨 Gestión de Estilos (Tailwind CSS)
Clases Limpias y Legibles: Si se utiliza Tailwind CSS, no generes cadenas kilométricas inline difíciles de leer. Agrupa lógicas de clases condicionales utilizando librerías como clsx o tailwind-merge.

Diseño Responsivo Primero: Diseña siempre pensando en dispositivos móviles primero utilizando los prefijos adecuados (sm:, md:, lg:).

🛑 Regla de Oro para la IA
Al generar o refactorizar interfaces de usuario con React, prioriza siempre la inmutabilidad del estado, la correcta gestión de los ciclos de vida para evitar fugas de memoria (limpieza en useEffect) y un tipado estricto que prevenga fallos en tiempo de ejecución.
