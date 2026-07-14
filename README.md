# QPU Ecosystem Hub (Quantitative & Protocol Utilities)

Bienvenido al repositorio central del ecosistema **QPU**. Este repositorio es la **Especificación Madre** y el *Single Source of Truth* (Única Fuente de Verdad) metodológica y técnica para el desarrollo de herramientas asistidas de investigación clínica y bioestadística.

> 📌 **Estás en un punto estable del repositorio.** Si volvés después de un tiempo, empezá por [`CURRENT-STATUS.md`](./CURRENT-STATUS.md) para ver en qué fase estamos.

## Índice de lectura rápida

| Quiero... | Ir a... |
|---|---|
| Entender la filosofía del ecosistema | [`VISION.md`](./VISION.md) |
| Saber qué stack usamos y por qué | [`ARCHITECTURE.md`](./ARCHITECTURE.md) |
| Entender cómo trabajamos (proceso) | [`METHODOLOGY.md`](./METHODOLOGY.md) |
| Trabajar con un agente de IA | [`AGENT-GUIDE.md`](./AGENT-GUIDE.md) |
| Ver el estado actual del repo | [`CURRENT-STATUS.md`](./CURRENT-STATUS.md) |
| Ver qué cambió | [`CHANGELOG.md`](./CHANGELOG.md) |
| Conocer el contrato `.clinical` | [`CLINICAL-CONTRACTS.md`](./CLINICAL-CONTRACTS.md) |
| Arrancar una QPU nueva | [`templates/`](./templates/) + carpeta `qpu-XX/` |
| Ver un `.clinical` de ejemplo | [`examples/ejemplo-basico.clinical`](./examples/ejemplo-basico.clinical) |

## Filosofía en 30 segundos

El ecosistema QPU no es un software monolítico ni una plataforma en la nube. Es un **sistema modular asistido basado en piezas de Lego**.

El núcleo es el proceso de **Operacionalización de Variables (OPER)**, que produce un archivo `.clinical` (JSON estructurado). A partir de ahí, utilidades independientes, ultraespecíficas y portátiles denominadas **QPUs (Quantitative & Protocol Utilities)** asisten al investigador paso a paso.

Las QPUs no automatizan a ciegas el proceso científico; asisten al investigador eliminando la programación manual, los errores de transcripción y garantizando el rigor metodológico.

## Catálogo de QPUs (estado actual)

| # | QPU | Estado | Carpeta |
|---|---|---|---|
| 01 | Form Generator | 🟡 Definido | [`qpu-01-form-generator/`](./qpu-01-form-generator/) |
| 02 | Data Collector | ⚪ Pendiente | [`qpu-02-data-collector/`](./qpu-02-data-collector/) |
| 03 | Data Cleaner | ⚪ Pendiente | [`qpu-03-data-cleaner/`](./qpu-03-data-cleaner/) |
| 04 | OMR Assistant | ⚪ Pendiente | [`qpu-04-omr-assistant/`](./qpu-04-omr-assistant/) |
| 05 | Exploratory Analysis | ⚪ Pendiente | [`qpu-05-exploratory-analysis/`](./qpu-05-exploratory-analysis/) |
| 06 | Cross-Sectional | ⚪ Pendiente | [`qpu-06-cross-sectional/`](./qpu-06-cross-sectional/) |
| 07 | Case-Control & Longitudinal | ⚪ Pendiente | [`qpu-07-case-control-longitudinal/`](./qpu-07-case-control-longitudinal/) |
| 08 | Cohort Analysis | ⚪ Pendiente | [`qpu-08-cohort-analysis/`](./qpu-08-cohort-analysis/) |

## Compatibilidad con el ecosistema `oper`

El formato `.clinical` se define en el repo [`oper`](https://github.com/maicel1978/oper). Esta es una **decisión externa que respetamos**: la raíz `{ "project": {}, "variables": [] }` es intocable. Si necesitamos un cambio, se discute en `oper` primero.

## Stack (resumen)

**Vanilla puro:** HTML5 + CSS3 + JavaScript ES2022+ Modules. Cero frameworks en runtime, cero CDNs, cero build step para el usuario final. La justificación detallada está en [`ARCHITECTURE.md`](./ARCHITECTURE.md).

## Cómo contribuir / retomar

1. **Nueva sesión con agente:** pegar `METHODOLOGY.md` + `AGENT-GUIDE.md` + `ARCHITECTURE.md` + `CURRENT-STATUS.md` + el archivo a tocar.
2. **Nueva QPU:** copiar `templates/` a `qpu-XX/`, rellenar SCOPE primero, esperar aprobación antes de programar.
3. **Cambio pequeño:** abrir PR con descripción, smoke test, y entrada en el `CHANGELOG.md` correspondiente.

---

> 🧭 **Cuando vuelvas en unos días, este README + `CURRENT-STATUS.md` te dicen exactamente dónde quedaste y cómo continuar.**
