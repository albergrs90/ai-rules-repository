# 🤖 AI Rules Repository

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/albergrs90/ai-rules-repository/graphs/commit-activity)
[![GitHub stars](https://img.shields.io/github/stars/albergrs90/ai-rules-repository.svg)](https://github.com/albergrs90/ai-rules-repository/stargazers)

> **Guías de desarrollo estandarizadas para asistentes de código con IA**
> 
> Mejora la calidad del código generado por Claude Code, Cursor, GitHub Copilot, Windsurf y otros asistentes de IA con reglas probadas y mejores prácticas de la industria.

---

## 🎯 ¿Qué es esto?

Este repositorio contiene un conjunto de **reglas y directrices de desarrollo** organizadas por capas que puedes importar en tus herramientas de IA favoritas para:

✅ **Generar código más seguro** - Prevención de fugas de APIs, validación de inputs  
✅ **Mantener clean code** - Principios SOLID, patrones de diseño, convenciones  
✅ **Optimizar rendimiento** - Índices SQL, patrones de consultas eficientes  
✅ **Documentar mejor** - Comentarios útiles, JSDoc, READMEs profesionales  
✅ **Reducir bugs** - Manejo de errores, validaciones, testing  

**¿El resultado?** Asistentes de IA que generan código de calidad profesional desde el primer intento, ahorrándote horas de revisiones y refactorizaciones.

---

## ⚡ Quick Start (3 pasos)

### 1️⃣ Elige tu stack tecnológico

Identifica las tecnologías de tu proyecto:

# Ejemplo: Proyecto Full-Stack
- Frontend: React + Next.js
- Backend: .NET Core
- Database: SQL Server

## 2️⃣ Selecciona las reglas necesarias
Combina reglas de diferentes capas según tu stack:

✅ global-rules/security-clean-code.md      (SIEMPRE incluir)
✅ global-rules/documentation-style.md      (Recomendado)
✅ frontend/react/react-rules.md            (Si usas React)
✅ frontend/nextjs/nextjs-rules.md          (Si usas Next.js)
✅ backend/dotnet-core/dotnet-core-rules.md (Si usas .NET)
✅ database-rules/sql-server-*.md           (Si usas SQL Server)

## 3️⃣ Importa en tu herramienta de IA
Sigue las instrucciones específicas para tu herramienta en la sección 📚 Cómo Usar.

## 📚 Cómo Usar

## 🔹 Cursor

Crea un archivo .cursorrules en la raíz de tu proyecto:
# En la raíz de tu proyecto
touch .cursorrules

Copia el contenido de las reglas que necesites:
# Ejemplo para proyecto React + .NET + SQL Server
cat ~/ai-rules-repository/global-rules/security-clean-code.md >> .cursorrules
cat ~/ai-rules-repository/frontend/react/react-rules.md >> .cursorrules
cat ~/ai-rules-repository/backend/dotnet-core/dotnet-core-rules.md >> .cursorrules
cat ~/ai-rules-repository/database-rules/sql-server-indexing.md >> .cursorrules

Alternativa: Usa el script automático:

# Descarga el script de conversión
curl -O https://raw.githubusercontent.com/albergrs90/ai-rules-repository/main/scripts/cursor-setup.sh
chmod +x cursor-setup.sh
./cursor-setup.sh react dotnet sql-server

## 🔹 Claude Code

Crea un archivo CLAUDE.md en la raíz de tu proyecto:

touch CLAUDE.md

Añade las reglas con el formato específico de Claude:

# Reglas de Desarrollo

## Seguridad y Clean Code
[Contenido de global-rules/security-clean-code.md]

## React
[Contenido de frontend/react/react-rules.md]

## .NET Core
[Contenido de backend/dotnet-core/dotnet-core-rules.md]

## 🔹 GitHub Copilot

Crea un archivo .github/copilot-instructions.md:

mkdir -p .github
touch .github/copilot-instructions.md

Copia las reglas relevantes en formato markdown.

## 🔹 Windsurf / Otras herramientas
La mayoría de herramientas aceptan archivos .md como contexto. Simplemente:
Copia los archivos .md que necesites
Pégalos en el contexto de tu herramienta
O usa el archivo combinado que generes

## 🎨 Ejemplos de Combinación
Ejemplo 1: E-commerce Full-Stack
Stack: Next.js + .NET Core + SQL Server

# Combinación recomendada
global-rules/security-clean-code.md
global-rules/documentation-style.md
frontend/nextjs/nextjs-rules.md
backend/dotnet-core/dotnet-core-rules.md
database-rules/sql-server-indexing.md
database-rules/sql-server-procedures-functions.md

Resultado: La IA generará:
- Componentes Next.js con Server Components optimizados
- APIs .NET con Clean Architecture
- Consultas SQL con índices apropiados
- Código documentado y seguro
- Ejemplo 2: SaaS con React + Node.js
- Stack: React + Node.js/Express + SQL Server

global-rules/security-clean-code.md
frontend/react/react-rules.md
backend/nodejs/nodejs-express.md
database-rules/sql-server-indexing.md

Ejemplo 3: API REST Pura
Stack: .NET Core + SQL Server (sin frontend)

global-rules/security-clean-code.md
backend/dotnet-core/dotnet-core-rules.md
database-rules/sql-server-indexing.md
database-rules/sql-server-procedures-functions.md

