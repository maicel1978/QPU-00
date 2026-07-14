# CURRENT-STATUS.md — Estado Vivo del Repositorio

> **Pegá este archivo al inicio de cada sesión con un agente.** Es la foto actual del repo: qué fase global estamos, qué QPUs están en qué estado, qué quedó pendiente.

## Estado global

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ESTADO DEL ECOSISTEMA QPU
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Fase global          : 0 → 1 (transición)
Fase de inicialización: CERRADA (cimientos estables)
Siguiente foco       : QPU-01 Form Generator (Fase -1)
Stack                : vanilla puro, sin build step, sin CDNs
Tooling              : ninguno todavía (pendiente decisión)
Tests                : pendiente (definir stack de tests en QPU-01)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Estado por QPU

| QPU | Estado | Fase | Notas |
|---|---|---|---|
| QPU-01 Form Generator | 🟡 Definido | -1 → 0 | Tiene docs viejos (SCOPE/ACCEPTANCE/UX en `docs/`) que se van a migrar a `qpu-01/` |
| QPU-02 Data Collector | ⚪ Sin iniciar | — | |
| QPU-03 Data Cleaner | ⚪ Sin iniciar | — | |
| QPU-04 OMR Assistant | ⚪ Sin iniciar | — | |
| QPU-05 Exploratory Analysis | ⚪ Sin iniciar | — | Probablemente la primera en necesitar Workers |
| QPU-06 Cross-Sectional | ⚪ Sin iniciar | — | |
| QPU-07 Case-Control & Longitudinal | ⚪ Sin iniciar | — | |
| QPU-08 Cohort Analysis | ⚪ Sin iniciar | — | |

**Leyenda:** ⚪ pendiente · 🟡 en curso · 🟢 completo · 🔴 bloqueado

## Archivos clave del repositorio

| Archivo | Rol |
|---|---|
| `README.md` | Punto de entrada |
| `VISION.md` | Filosofía del ecosistema |
| `ARCHITECTURE.md` | Verdad única del stack |
| `METHODOLOGY.md` | Cómo trabajamos (PRISMA+ v5.2 adaptado) |
| `AGENT-GUIDE.md` | Reglas para agentes de IA |
| `CLINICAL-CONTRACTS.md` | Contrato del formato `.clinical` |
| `CURRENT-STATUS.md` | Este archivo |
| `CHANGELOG.md` | Historia de cambios del ecosistema |
| `templates/` | Plantillas para SCOPE/ACCEPTANCE/UX |
| `qpu-XX/` | Una carpeta por QPU |
| `examples/` | Archivos `.clinical` de referencia |

## Decisiones tomadas (cerradas)

- ✅ **Stack**: vanilla puro, sin Tailwind, sin Alpine, sin CDNs. Justificación en `ARCHITECTURE.md`.
- ✅ **Compatibilidad `.clinical`**: se respeta la raíz `{project, variables}` del repo `oper`. No se rompe.
- ✅ **Rango de `range.min/max`**: vienen como string, hay que parsear con cuidado. Documentado en `CLINICAL-CONTRACTS.md` §4.
- ✅ **Carpeta por QPU**: cada QPU vive en `qpu-XX/`. Los docs viejos en `docs/` se migran cuando la QPU arranque.
- ✅ **Gobernanza de docs**: mínimo. Sin ADRs, sin changelog global obligatorio por commit. CHANGELOG por QPU y CHANGELOG raíz solo para hitos.
- ✅ **Restricciones de cambio del agente (R15)**: NO por LOC. Por: una intención, diff en una pantalla, máx archivos, test que falla antes y pasa después. Las 4 son obligatorias.
- ✅ **Ventana de creatividad (R16)**: para exploración, el agente propone 2-3 enfoques sin código, vos elegís, y recién ahí arranca el Pre-Código. La creatividad se preserva sin perder rigor.
- ✅ **Nudges de creatividad (R17)**: preferencias declaradas (no reglas), anti-complejidad y pro-simplicidad. El agente las sigue salvo justificación.
- ✅ **Asunciones explícitas (R18)**: cada cambio declara sus asunciones. Las revisás antes de mergear.

## Decisiones pendientes

- ❓ **Stack de tests**: ¿framework externo (vitest) o mini-runner propio? Decidir en QPU-01 Fase 0.
- ❓ **Convención de exports**: ¿cjs-style o ES modules con import/export puro? Decidir en QPU-01 Fase 1.
- ❓ **Naming de archivos**: `kebab-case` vs `camelCase` para `.js`. Decidir en QPU-01 Fase 1.
- ❓ **Versión del formato `.clinical`**: ¿seguimos la v7.0 de `oper` o etiquetamos? Por ahora, sin versión: respetar lo que esté en `oper`.

## Próximos pasos (orden sugerido)

1. **Crear `qpu-01-form-generator/`** con SCOPE, ACCEPTANCE, UX migrados desde `docs/`.
2. **QPU-01 Fase 0**: decisión de stack específica, viabilidad.
3. **QPU-01 Fase 1**: estructura + contratos.
4. **Recién después** decidir el stack de tests definitivo y propagarlo al resto.
