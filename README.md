# 🤖 AI Rules Repository

¡Bienvenido a tu repositorio centralizado de directrices de desarrollo para Inteligencia Artificial! Este espacio está diseñado para almacenar, organizar y mantener plantillas de **System Prompts**, **AI Rules** y **archivos de contexto (.md)** que guían a asistentes de código (Claude Code, Cursor, GitHub Copilot, Windsurf, etc.) a generar código seguro, limpio y alineado con las mejores prácticas de la industria.

---

## 📂 Estructura del Repositorio

El repositorio está organizado por capas de abstracción para facilitar la combinación de reglas según el stack tecnológico de cada proyecto:

```text
📂 ai-rules-repository
 ┣ 📂 global-rules/              # Directrices universales (Seguridad, Filosofía, Git)
 ┃ ┣ 📜 security-clean-code.md   # Prevención de fugas de APIs, principios SOLID, etc.
 ┃ ┗ 📜 documentation-style.md   # Estilo de comentarios, JSDoc y formato de READMEs.
 ┃
 ┣ 📂 frontend/                  # Reglas específicas para tecnologías Frontend
 ┃ ┣ 📂 react/
 ┃ ┃ ┗ 📜 react-rules.md         # Buenas prácticas en React, Hooks, TypeScript, etc.
 ┃ ┗ 📂 nextjs/
 ┃   ┗ 📜 nextjs-rules.md        # Estructura de App Router, Server Components.
 ┃
 ┣ 📂 backend/                   # Reglas específicas para tecnologías Backend
 ┃ ┣ 📂 dotnet-core/
 ┃ ┃ ┗ 📜 dotnet-core-rules.md   # Clean Architecture, Inyección de dependencias, C#.
 ┃ ┗ 📂 nodejs/
 ┃   ┗ 📜 nodejs-express.md      # Arquitectura de Middlewares, manejo de errores.
 ┃
 ┣ 📂 database-rules/            # Gestión de persistencia y almacenamiento
 ┃ ┣ 📜 sql-server-indexing.md   # Estrategia de índices y tipos de datos en SQL Server.
 ┃ ┗ 📜 sql-server-procedures-functions.md # Uso óptimo de Stored Procedures y iTVFs.
 ┃
 ┗ 📜 README.md                  # Esta guía de uso.
