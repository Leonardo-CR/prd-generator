---
project: TaskFlow CLI
version: 1.0
date: 2026-04-27
status: draft
pipeline_stage: PRD
next_steps: [MVP Definition, User Stories, OpenSpec]
---

# PRD: TaskFlow CLI

## 1. Problem Statement

**Problem:** Desarrolladores que trabajan en terminal no tienen una herramienta de gestión de tareas que se integre naturalmente con su flujo de trabajo sin salir de la línea de comandos.

**Context:** Los desarrolladores pasan la mayor parte de su tiempo en la terminal. Las herramientas de gestión de tareas existentes (Trello, Notion, Jira) requieren cambiar de contexto a un navegador o app separada, interrumpiendo el flujo de trabajo. Las alternativas en CLI existentes son rudimentarias o tienen curvas de aprendizaje elevadas.

**Motivation:** Con el auge del desarrollo asistido por IA y los flujos de trabajo terminal-first, existe una oportunidad clara para una herramienta simple, rápida y scripteable que gestione tareas directamente desde la terminal.

---

## 2. Users

### Primary User
- **Role:** Desarrollador de software individual
- **Technical level:** Developer (cómodo con CLI, git, scripting)
- **Context of use:** Durante sesiones de desarrollo activo, en terminal, generalmente en macOS o Linux
- **Key pain point:** Perder el hilo de sus tareas pendientes al cambiar entre contextos, o usar herramientas demasiado pesadas para tareas simples

### Secondary Users (if any)
- **DevOps / SRE:** Automatización de tareas mediante scripts que consultan o crean tareas

---

## 3. Goals & Non-Goals

### Goals
- Permitir crear, listar, completar y eliminar tareas desde la terminal en menos de 2 comandos
- Persistir tareas localmente sin necesidad de cuenta ni conexión a internet
- Soportar etiquetas y prioridades para organizar tareas
- Ser scripteable (salida parseable por otros comandos UNIX)
- Tiempo de arranque < 100ms

### Non-Goals (explicit out-of-scope)
- No habrá sincronización en la nube ni colaboración multi-usuario
- No habrá interfaz gráfica ni web
- No habrá integración con servicios externos (GitHub Issues, Jira, etc.) en v1
- No habrá notificaciones ni recordatorios

---

## 4. Domain Model

| Entity | Description | Key Attributes |
|--------|-------------|----------------|
| Task | Unidad de trabajo a realizar | id, title, status, priority, tags, created_at, completed_at |
| Tag | Etiqueta para categorizar tareas | name |
| Project | Agrupación opcional de tareas | name, description |

**Relationships:**
- Task pertenece a un Project (opcional)
- Task tiene muchos Tags
- Project tiene muchas Tasks

---

## 5. Functional Requirements

### FR-01: Crear tarea
**Description:** El usuario puede crear una nueva tarea especificando título, prioridad opcional y etiquetas opcionales.
**Actors:** Usuario (CLI)
**Inputs:** Título (string), prioridad (alta/media/baja, default: media), etiquetas (lista de strings)
**Outputs / Outcomes:** Tarea creada con ID único, confirmación en stdout
**Priority:** Must-have

### FR-02: Listar tareas
**Description:** El sistema muestra todas las tareas pendientes, opcionalmente filtradas por etiqueta, prioridad o proyecto.
**Actors:** Usuario (CLI)
**Inputs:** Filtros opcionales (--tag, --priority, --project, --all para incluir completadas)
**Outputs / Outcomes:** Lista formateada en tabla o JSON (según flag --json)
**Priority:** Must-have

### FR-03: Completar tarea
**Description:** El usuario marca una tarea como completada por su ID.
**Actors:** Usuario (CLI)
**Inputs:** ID de tarea
**Outputs / Outcomes:** Tarea actualizada con status=done y completed_at, confirmación en stdout
**Priority:** Must-have

### FR-04: Eliminar tarea
**Description:** El usuario elimina permanentemente una tarea por su ID.
**Actors:** Usuario (CLI)
**Inputs:** ID de tarea, confirmación interactiva (o flag --force para scripts)
**Outputs / Outcomes:** Tarea eliminada, confirmación en stdout
**Priority:** Must-have

### FR-05: Editar tarea
**Description:** El usuario puede modificar el título, prioridad o etiquetas de una tarea existente.
**Actors:** Usuario (CLI)
**Inputs:** ID de tarea, campos a modificar
**Outputs / Outcomes:** Tarea actualizada, confirmación en stdout
**Priority:** Should-have

### FR-06: Gestión de proyectos
**Description:** El usuario puede crear proyectos y asignar tareas a ellos para organizar el trabajo.
**Actors:** Usuario (CLI)
**Inputs:** Nombre de proyecto, descripción opcional
**Outputs / Outcomes:** Proyecto creado; las tareas pueden referenciarlo con --project
**Priority:** Should-have

---

## 6. Non-Functional Requirements

| Category | Requirement | Notes |
|----------|-------------|-------|
| Performance | Tiempo de arranque < 100ms | Crítico para no interrumpir flujo |
| Performance | Tiempo de respuesta < 200ms para cualquier operación | |
| Usability | Comandos intuitivos sin consultar documentación | Inspirado en la API de git |
| Portabilidad | Funciona en macOS, Linux (Ubuntu/Debian). Windows nice-to-have | |
| Persistencia | Datos almacenados localmente en archivo JSON o SQLite | Sin dependencias de red |
| Scriptabilidad | Flag --json disponible en todos los comandos de lectura | Para pipes y scripts |

---

## 7. Technical Constraints

- **Platform:** CLI (terminal)
- **Stack:** Python 3.10+ o Go 1.21+ (decisión pendiente — ver Open Questions)
- **Integrations:** Ninguna en v1
- **Deployment:** Instalable via pip, brew, o binario descargable
- **Regulatory / Business constraints:** Todos los datos permanecen locales; no hay telemetría

---

## 8. Acceptance Criteria

- [ ] `task add "Revisar PR de Juan" --priority alta --tag work` crea una tarea y muestra su ID
- [ ] `task list` muestra solo tareas pendientes por defecto
- [ ] `task list --all` incluye tareas completadas
- [ ] `task list --json` produce JSON válido parseable por `jq`
- [ ] `task done 3` marca la tarea con ID 3 como completada
- [ ] `task rm 3` pide confirmación antes de eliminar (excepto con --force)
- [ ] El tiempo de arranque medido con `time task list` es < 100ms en hardware moderno
- [ ] Ejecutar 1000 operaciones consecutivas no produce corrupción de datos

---

## 9. Open Questions

| # | Question | Impact | Owner |
|---|----------|--------|-------|
| 1 | ¿Python o Go? Go da binarios rápidos sin dependencias; Python es más fácil de contribuir | Afecta instalación, performance y portabilidad | Tech Lead |
| 2 | ¿Almacenamiento en JSON plano o SQLite? JSON es simple; SQLite escala mejor con muchas tareas | Afecta queries complejos y performance con listas grandes | Tech Lead |
| 3 | ¿El ID de tarea es numérico secuencial o UUID? Numérico es más cómodo en CLI; UUID es más seguro para futura sync | Afecta UX y extensibilidad futura | Product |

---

## 10. Glossary

| Term | Definition |
|------|------------|
| Task | Unidad de trabajo individual con título, estado y metadatos |
| Tag | Etiqueta de texto libre para categorizar tareas (ej: "work", "personal", "bug") |
| Project | Agrupación nombrada de tareas relacionadas |
| Status | Estado de una tarea: `pending` o `done` |
| Priority | Nivel de urgencia de una tarea: `high`, `medium`, `low` |
| Scriptable | Que puede usarse en scripts de shell gracias a salidas predecibles y flag --json |
