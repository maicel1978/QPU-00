# CHANGELOG.md — Ecosistema QPU

> **Convención:** este changelog documenta hitos del ecosistema (cambios que afectan a múltiples QPUs, decisiones de stack, breaking changes del contrato). Cada QPU tiene su propio `qpu-XX/CHANGELOG.md` con sus cambios granulares.

Formato basado en [Keep a Changelog](https://keepachangelog.com/), adaptado.

## [No liberado]

### Added
- `METHODOLOGY.md`: metodología PRISMA+ v5.2 adaptada al ecosistema QPU.
- `AGENT-GUIDE.md`: reglas canónicas para agentes de IA.
- `ARCHITECTURE.md`: reescrito como verdad única del stack.
- `CURRENT-STATUS.md`: estado vivo del repositorio.
- `templates/`: plantillas SCOPE, ACCEPTANCE-CRITERIA, UX-FLOW.
- Estructura inicial `qpu-01..08/` con READMEs mínimos.
- `examples/ejemplo-basico.clinical` consolidado.
- Regla R15 (revisada): restricciones por revisabilidad (intención, pantalla, archivos, test), no por LOC arbitrario.
- Regla R16: Ventana de Creatividad para exploración.
- Regla R17: Nudges de creatividad (preferencias declaradas).
- Regla R18: Protocolo de asunción explícita.

### Changed
- Stack unificado: **vanilla puro, sin build step, sin CDNs, sin Tailwind, sin Alpine**.
- `CLINICAL-CONTRACTS.md`: alineado con la decisión de `oper` de no romper la raíz del `.clinical`. Documentada la trampa de tipos en `range.min/max` (vienen como string).
- `AGENT-GUIDE-QPU.md` (duplicado): eliminado. Queda solo `AGENT-GUIDE.md`.
- Archivo `path` (duplicado huérfano): eliminado.
- **R15 revisada**: la regla original "~35 LOC por cambio" se reemplazó por las 4 condiciones de revisabilidad (intención, diff en una pantalla, máx archivos, test obligatorio). Razón: LOC como métrica es arbitraria y no escala bien con el tipo de cambio. Ver `METHODOLOGY.md` R15.
- Bloque `📝 PRE-CÓDIGO` actualizado con campos nuevos: intención, archivos, test, diff estimado, asunciones.

### Removed
- `path` (archivo duplicado mal nombrado).
- `AGENT-GUIDE-QPU.md` (duplicado).
- Basura XML/tool-call en `docs/SCOPE.md` (era residuo de prompt).
- Métrica arbitraria de ~35 LOC (reemplazada por las 4 condiciones de R15).

### Fixed
- Contradicción de stack entre 4 archivos (README, ARCHITECTURE, AGENT-GUIDE-QPU, path) — resuelta a favor de vanilla puro.
- Regla de tamaño de cambio arbitraria — reemplazada por restricciones de revisabilidad.

## Historial previo

- `7f5610e` — "caos incial" (estado previo de referencia, sin formato de changelog).
