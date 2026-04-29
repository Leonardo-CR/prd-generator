---
project: Gestión de Alumnos para Profesores
version: 1.0
date: 2026-04-27
status: draft
pipeline_stage: PRD
next_steps: [MVP Definition, User Stories, OpenSpec]
---

# PRD: Gestión de Alumnos para Profesores

## 1. Problem Statement

**Problem:** Los profesores necesitan una herramienta digital extremadamente simple para gestionar la información básica de sus alumnos sin enfrentarse a interfaces complejas o funcionalidades innecesarias.

**Context:** Actualmente, muchos docentes con poca experiencia tecnológica dependen de hojas de papel o archivos de Excel complejos que dificultan la edición rápida y el acceso desde dispositivos móviles. Este sistema busca ser un ejemplo de uso de un CRUD (Create, Read, Update, Delete) aplicado a este entorno escolar.

**Motivation:** Facilitar la adopción de herramientas digitales mediante una interfaz minimalista que elimine la fricción técnica y permita la gestión de datos en tiempo real desde cualquier lugar.

---

## 2. Users

### Primary User
- **Role:** Profesor de escuela.
- **Technical level:** Bajo / Principiante (poca experiencia con aplicaciones).
- **Context of use:** Salones de clase o desde casa, accediendo tanto desde computadoras de escritorio como desde teléfonos móviles.
- **Key pain point:** Interfaces saturadas de botones y procesos de registro complicados que generan confusión.

---

## 3. Goals & Non-Goals

### Goals
- Proveer una interfaz limpia y minimalista que no requiera capacitación previa.
- Asegurar que la aplicación sea 100% responsiva (optimizado para móviles).
- Implementar un sistema de almacenamiento ligero y eficiente usando SQLite.

### Non-Goals (explicit out-of-scope)
- Gestión de calificaciones o actividades académicas.
- Sistema de asistencia o calendario.
- Comunicación directa (mensajería) entre alumnos y profesores.

---

## 4. Domain Model

> Entidades principales y sus relaciones.

| Entity | Description | Key Attributes |
|--------|-------------|----------------|
| Alumno | Representa a un estudiante inscrito en la institución. | id, nombre_completo, matricula, carrera |

**Relationships:**
- La entidad es independiente en esta fase inicial (No hay relaciones con otras entidades como Grupos o Materias aún).

---

## 5. Functional Requirements

### FR-01: Registro de Alumno (Alta)
**Description:** El sistema debe permitir al profesor ingresar un nuevo alumno.
**Actors:** Profesor.
**Inputs:** Nombre completo, matrícula, carrera.
**Outputs / Outcomes:** Nuevo registro creado en la base de datos SQLite.
**Priority:** Must-have.

### FR-02: Listado de Alumnos (Consulta)
**Description:** Visualización de todos los alumnos registrados en una tabla o lista clara.
**Actors:** Profesor.
**Inputs:** Carga de la página principal.
**Outputs / Outcomes:** Lista de alumnos con sus datos básicos.
**Priority:** Must-have.

### FR-03: Edición de Alumno (Cambios)
**Description:** El profesor debe poder modificar la información de un alumno existente.
**Actors:** Profesor.
**Inputs:** Selección de alumno, nuevos datos.
**Outputs / Outcomes:** Registro actualizado en la base de datos.
**Priority:** Must-have.

### FR-04: Eliminación de Alumno (Baja)
**Description:** Opción para borrar permanentemente a un alumno del sistema.
**Actors:** Profesor.
**Inputs:** Confirmación de eliminación sobre un registro.
**Outputs / Outcomes:** Eliminación del registro en SQLite.
**Priority:** Must-have.

---

## 6. Non-Functional Requirements

| Category | Requirement | Notes |
|----------|-------------|-------|
| Usability | Interfaz minimalista con navegación intuitiva (pocos clics). | Crítico para el perfil del usuario. |
| Compatibility | Diseño responsivo para teléfonos (Mobile-First). | Uso de media queries CSS. |
| Aesthetics | Paleta de colores en tonos azules con diseño limpio. | Estilo institucional y profesional. |
| Persistence | Uso de SQLite para almacenamiento local/ligero. | No requiere servidor de BD complejo. |

---

## 7. Technical Constraints

- **Platform:** Aplicación Web.
- **Stack:** Python (Back-end), HTML/CSS (Front-end), SQLite (Base de datos).
- **Integrations:** Ninguna requerida.
- **Deployment:** Ejecución local o hosting sencillo compatible con Python.

---

## 8. Acceptance Criteria

- [ ] El sistema permite crear, leer, editar y borrar alumnos sin errores.
- [ ] La interfaz se ajusta correctamente al ancho de pantalla de un smartphone.
- [ ] Los datos persisten después de reiniciar la aplicación (SQLite).
- [ ] El diseño visual sigue el esquema de tonos azules y minimalismo solicitado.

---

## 9. Open Questions

| # | Question | Impact | Owner |
|---|----------|--------|-------|
| 1 | ¿Se requiere algún tipo de login/autenticación para los profesores? | Seguridad de los datos. | Usuario |
| 2 | ¿La matrícula debe tener un formato específico (solo números, longitud)? | Validación de datos. | Usuario |

---

## 10. Glossary

| Term | Definition |
|------|------------|
| CRUD | Acrónimo de Create, Read, Update, Delete (Crear, Leer, Actualizar, Borrar). |
| SQLite | Motor de base de datos SQL ligero que no requiere un servidor independiente. |
| Responsivo | Diseño web que se adapta al tamaño de la pantalla del dispositivo. |
