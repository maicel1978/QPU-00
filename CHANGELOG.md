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
- Regla R15: límite de ~35 LOC por cambio del agente.

### Changed
- Stack unificado: **vanilla puro, sin build step, sin CDNs, sin Tailwind, sin Alpine**.
- `CLINICAL-CONTRACTS.md`: alineado con la decisión de `oper` de no romper la raíz del `.clinical`. Documentada la trampa de tipos en `range.min/max` (vienen como string).
- `AGENT-GUIDE-QPU.md` (duplicado): eliminado. Queda solo `AGENT-GUIDE.md`.
- Archivo `path` (duplicado huérfano): eliminado.

### Removed
- `path` (archivo duplicado mal nombrado).
- `AGENT-GUIDE-QPU.md` (duplicado).
- Basura XML/tool-call en `docs/SCOPE.md` (era residuo de prompt).

### Fixed
- Contradicción de stack entre 4 archivos (README, ARCHITECTURE, AGENT-GUIDE-QPU, path) — resuelta a favor de vanilla puro.

## Historial previo

- `7f5610e` — "caos incial" (estado previo de referencia, sin formato de changelog).
