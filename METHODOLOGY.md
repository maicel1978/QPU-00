# METHODOLOGY.md — PRISMA+ v5.2 adaptado al Ecosistema QPU

> **Este archivo es la metodología de trabajo del ecosistema QPU.** Es una adaptación de [PRISMA+ v5.2](https://github.com/maicel1978/oper) (Práctica, Resiliente, Iterativa, Supervisada, Modular, Auditable) a la realidad de las QPUs: aplicaciones chicas, de una sola responsabilidad, con `.clinical` como contrato único.

> Si trabajás con un agente de IA, **pegá este archivo junto con `AGENT-GUIDE.md` y `CURRENT-STATUS.md`**.

---

## 1. Roles simultáneos

En cada Gate Check, quien trabaja (humano o agente) reporta desde tres perspectivas:

- 🏛️ **ARQUITECTO**: estructura, decisiones técnicas, trade-offs
- 🛠️ **DESARROLLADOR**: código limpio, modular, defensivo
- 🧪 **QA**: fallos, casos edge, problemas de UX antes de que ocurran

---

## 2. Definiciones clave

```
Vanilla runtime    : en producción, archivos estáticos sin framework.
                     Sin React/Vue/Svelte/Angular/Alpine en runtime.

Tooling dev        : ESLint/Prettier permitidos como devDependencies.
                     Nunca afectan el runtime.

Build step opt-in  : permitido si el output final sigue siendo
                     estático y legible. Sin build step es el default.
```

---

## 3. Reglas inquebrantables

### R1 — Runtime siempre vanilla
HTML5 + CSS3 + Vanilla JS ES2022+ Modules. Sin excepciones. (Reforzado en `ARCHITECTURE.md`.)

### R2 — Tooling de desarrollo opt-in
- Sin build step (default).
- Vite opt-in, **solo si se justifica en el `qpu-XX/ARCHITECTURE.md` específico** y el output sigue siendo estático y portable.
- ESLint + Prettier opcionales como devDependencies, nunca en runtime.

### R3 — CDNs
**Prohibidas por defecto** en el ecosistema QPU (refuerza offline-first). Si una QPU necesita una, se justifica individualmente y se documenta.

Excepción justificada posible: librerías de procesamiento pesado (Chart.js para QPU-05, etc.) si se demuestra que no se puede lograr offline. **Esto requiere aprobación explícita.**

### R4 — Web Workers cuando aplique
```
Regla base  : operaciones > 200ms van en Worker
Datasets    : > 10k filas en Worker
Audio/Imagen: SIEMPRE en Worker
ML/IA       : SIEMPRE en Worker
```

### R5 — Progreso y cancelación
Si una tarea puede tardar > 1s:
```javascript
// Progreso obligatorio
worker.postMessage({ type: 'PROCESS', id, payload, reportEvery: 1000 });

// Cancelación obligatoria
const controller = { cancelled: false };
// Worker verifica controller.cancelled periódicamente
```

### R6 — Programación defensiva
- Validar tipo/formato/tamaño del input antes de procesar.
- `try-catch` con mensajes útiles al usuario.
- Graceful degradation: si algo falla, no romper toda la app.
- Límite de memoria antes de procesar datasets grandes.

### R7 — Contratos entre módulos
- UI → Core: funciones exportadas, tipos de entrada/salida, errores.
- Core → Worker: mensajes `{ type, id, payload }`.
- Worker → Core: respuestas `{ type, id, payload }` o errores `{ type: 'error', id, message, details }`.

**En QPU-00:** el contrato externo principal es `.clinical`. No se rompe nunca.

### R8 — Privacidad por defecto
Todo procesamiento local. **Sin red salvo que el usuario lo pida explícitamente y esté documentado.** En el ecosistema QPU, esto es especialmente crítico: se manejan datos clínicos.

### R9 — Accesibilidad mínima obligatoria
- Navegación por teclado completa.
- Foco visible en todos los controles.
- Labels asociados a inputs.
- `aria-live` para estados de progreso y errores.
- Contraste mínimo WCAG AA.

### R10 — Límite de archivos
```
Recomendado  : ≤ 200 líneas por archivo (cohesión)
Máximo       : 350 líneas con justificación en Gate Check
Prohibido    : > 350 líneas sin división lógica
```

### R11 — Protocolo de Pausa (CRÍTICO)
Al terminar cada fase, mostrar EXACTAMENTE este bloque:

```
┌─────────────────────────────────────────┐
│  ⏸️  FASE [N] COMPLETADA                │
│                                         │
│  Para continuar escribe:                │
│  → "APROBAR"            (sin cambios)   │
│  → "CORREGIR: [qué]"   (hay cambios)   │
│  → "EXPLICAR: [qué]"   (tengo dudas)   │
│                                         │
│  Cualquier otra respuesta no es válida. │
│  Te recordaré estos tres comandos.      │
└─────────────────────────────────────────┘
```

### R12 — Gestión de cambios y mejoras
Si se detecta un error propio → Protocolo de Errores (§8).
Si se detecta una mejora no solicitada:
```
┌─────────────────────────────────────────┐
│  💡 MEJORA DETECTADA                    │
├─────────────────────────────────────────┤
│  Actual  : [cómo está ahora]            │
│  Mejora  : [qué cambiaría]              │
│  Ventaja : [qué se gana]                │
│  Riesgo  : [qué puede romperse]         │
│                                         │
│  ¿Aplico? APROBAR / IGNORAR             │
└─────────────────────────────────────────┘
```
**NUNCA optimizar silenciosamente.**

### R13 — Anti-hallucination para código
Antes de generar cualquier archivo de código, **siempre** mostrar el bloque `📝 PRE-CÓDIGO` (definido en `AGENT-GUIDE.md` §2). Si no podés listarlo, no generes el código.

### R14 — Recordatorio vanilla en código
Todo archivo `.js` debe empezar con el comentario estándar (definido en `AGENT-GUIDE.md` §6).

### R15 — Tamaño y forma de los cambios del agente (regla QPU-00)

**Por qué esta regla existe:** los agentes de IA pueden generar diffs grandes rápido. Eso es un riesgo para la calidad y para la auditoría humana, no un beneficio. La restricción correcta no es por líneas de código (eso es arbitrario), sino por **revisabilidad y trazabilidad**.

Un cambio del agente debe cumplir **las cuatro condiciones** siguientes. Si no las cumple, se parte en cambios más chicos.

**R15.1 — Una sola intención por cambio**
El commit resuelve una sola cosa, declarable en una frase. Si el mensaje del commit necesita "y", son dos cambios.

```
✅ "Validar range con string vacío en QPU-01"
✅ "Agregar export CSV a QPU-01"
❌ "Refactor de validaciones + cambiar tema + agregar export"
```

**R15.2 — Diff revisable en una pantalla**
El diff completo debe entrar en una pantalla de editor (≈ 50 líneas visibles). Si no entra, se parte.

**Excepciones legítimas** (documentar en el commit):
- Archivos generados (ej: `examples/*.clinical` con muchas variables).
- Tablas o assets estáticos donde el LOC no es señal de complejidad.
- Migraciones de formato completas (de un esquema a otro).

**R15.3 — Máximo de archivos tocados por cambio**
- **Lógica (carpeta `core/`, `workers/`, `utils/`):** **1 archivo + sus tests**.
- **UI (HTML/CSS/JS de UI):** hasta **2 archivos** (el acoplamiento visual lo justifica).
- **Docs:** pueden tocar varios archivos (es su naturaleza).
- **Prohibido:** un commit que toca lógica de `core/`, `workers/` y `ui/` a la vez. Son 3 commits.

**R15.4 — Test que falla antes y pasa después**
Cada cambio tiene un test que verifica el comportamiento:

| Tipo de cambio | Test esperado |
|---|---|
| Lógica pura (funciones, parsers, validadores) | Test unitario automatizado |
| Lógica con efectos (DOM, storage) | Test de integración o smoke test manual documentado |
| UI sin lógica | Smoke test manual con pasos numerados |
| Fix de bug | Test de regresión que reproduce el bug original |
| Refactor (sin cambio de comportamiento) | Tests existentes deben seguir pasando + 1 test que verifique que el comportamiento no cambió |

**Si no podés escribir el test, es una señal de que el cambio no está bien delimitado.** Volvé a R15.1.

**Anti-pattern explícito:** el agente no puede argumentar "este cambio es chico, no necesita test". El test no se negocia, se escribe.

### R16 — Ventana de creatividad (modo exploración)

**Cuándo aplica:** cuando vos pedís algo abierto tipo *"explorá cómo harías X"*, *"proponé opciones"*, *"pensá alternativas"*, o cuando el problema no está bien definido.

**Cómo funciona:**
1. El agente responde con **2 a 3 enfoques diferentes**, sin Pre-Código ni código todavía.
2. Cada enfoque se presenta como una propuesta breve (qué hace, qué tradeoff tiene, qué complejidad implica).
3. Vos elegís uno (o decís "combiná 1 y 2", o "ninguno, repensá").
4. Recién después de la elección, arranca el flujo normal: Pre-Código → código → tests → gate.

**Por qué:** mata la rigidez del Pre-Código para exploración y preserva la creatividad del agente sin sacrificar el rigor. La restricción se aplica **después** de que haya claridad sobre qué hacer.

### R17 — Nudges de creatividad (no restricciones, recomendaciones)

En vez de restringir al agente en cosas que no rompen el sistema, lo **empujamos hacia lo bueno**. Son preferencias declaradas, no reglas inquebrantables:

- Preferir **funciones puras** sobre estado mutable cuando es posible.
- Preferir **composición** sobre herencia.
- Preferir **datos explícitos** sobre implícitos (un objeto bien definido > un Map mágico).
- Si el agente encuentra una forma **más simple** mientras trabaja, debe reportarla con el bloque `💡 MEJORA DETECTADA` antes de aplicarla.
- Si una decisión depende de algo que el agente no sabe, **preguntá** en vez de asumir.
- **Nudges anti-complejidad:**
  - Si una función pasa de 30 líneas, probablemente hace 2 cosas. Partila.
  - Si un archivo pasa de 200 líneas, probablemente tiene 2 responsabilidades. Partilo.
  - Si una variable tiene 3 niveles de anidación, reconsiderá la estructura.
- **Nudges pro-simplicidad:**
  - Si podés resolver con HTML semántico nativo, no agregues JS.
  - Si podés resolver con CSS vanilla, no agregues JS.
  - Si podés resolver con 5 líneas, no escribas 20.

**Cómo se distinguen de R1–R15:** las R1–R15 son **inquebrantables** (rompen el sistema). Las nudges de R17 son **preferencias** que el agente debe seguir salvo justificación explícita. Si las rompe, debe explicar por qué.

### R18 — Protocolo de asunción explícita

Cuando el agente **asume algo** (aunque sea obvio para él), debe declararlo explícitamente en una línea visible. Esto evita el clásico "el agente inventó 3 cosas que no le pediste y están mal".

**Cuándo aplica:**
- Antes del Pre-Código: ¿qué estás asumiendo del contexto que no está escrito?
- Durante la implementación: si tomás una decisión no pedida, marcala.
- Al final del cambio: ¿qué quedó asumido y conviene revisar?

**Formato:**
```
📌 Asunciones de este cambio:
   - El .clinical usa siempre minúsculas en `name`.
   - El usuario tiene un navegador moderno (Chrome 100+).
   - El localStorage está disponible.
   - [etc.]
```

Si alguna de esas asunciones es falsa en el contexto real, vos la marcás y se corrige **antes** de mergear.

---

## 4. Arquitectura de procesamiento

```
┌────────────────────────────────────────────────┐
│                UI (vanilla)                    │
│   HTML5 semántico + CSS vanilla + JS ligero    │
└──────────────────┬─────────────────────────────┘
                   │ llama funciones puras
                   ▼
┌────────────────────────────────────────────────┐
│              Core (vanilla)                    │
│   Lógica testeable, sin DOM, sin I/O pesado    │
└──────────────────┬─────────────────────────────┘
                   │ mensajes tipados
                   ▼
┌────────────────────────────────────────────────┐
│            Workers (cuando aplique)            │
│   MessageChannel directo, sin Comlink          │
└────────────────────────────────────────────────┘
```

**QPU-01 (Form Generator)** probablemente no necesita Worker — la operación es parsear JSON y renderizar HTML, no hay > 200ms.
**QPU-05 (Exploratory Analysis)** sí va a necesitarlo — datasets grandes.
**QPU-08 (Cohort)** casi seguro.

---

## 5. Fases de ejecución

Una fase por respuesta del agente. Sin excepciones. Las fases son:

| Fase | Nombre | Entregable principal |
|---|---|---|
| -1 | Descubrimiento | Mapa de lo entendido + preguntas |
| 0 | Viabilidad + Stack | Decisión documentada |
| 1 | Estructura + Contratos | Árbol + `API-CONTRACTS.md` |
| 2 | UI + CSS | HTML semántico + CSS |
| 3 | Lógica Core | Módulos con tests |
| 4 | Workers | Si aplica |
| 5 | Robustez + Pulido | Tests de estrés + accesibilidad |

**El ecosistema QPU no entra directo a Fase 0.** Primero hay una "Fase de Inicialización" del repo (la que estamos cerrando ahora) y después cada QPU arranca en su propia Fase -1.

---

## 6. Tests (criterio QPU-00)

El ecosistema QPU busca simplicidad, pero los tests son obligatorios. Stack sugerido:

- **Unit tests de lógica core**: framework vanilla liviano (opciones a evaluar: `vitest` en modo browser, o un mini-runner de ~50 líneas propio).
- **Tests de validación de `.clinical`**: cargar archivos de ejemplo y verificar parseo.
- **Tests de UI**: smoke tests manuales reproducibles (no automatizados al inicio, por portabilidad).

Cada test se documenta en `qpu-XX/ACCEPTANCE-CRITERIA.md` con un smoke test manual.

---

## 7. Gate Check (antes de cada pausa)

```
□ ¿Límite de archivos respetado? (R10)
□ ¿UI no bloqueada? (R4/R5 si aplica)
□ ¿Validaciones y errores claros? (R6)
□ ¿Contratos respetados? (R7)
□ ¿Docs actualizadas?
□ ¿Runtime sigue siendo vanilla? (R1)
□ ¿Pre-código mostrado? (R13)
□ ¿Comentario vanilla en archivos? (R14)
□ ¿Una sola intención? (R15.1)
□ ¿Diff entra en una pantalla? (R15.2)
□ ¿Archivos tocados dentro del máximo? (R15.3)
□ ¿Test escrito que verifica el cambio? (R15.4)
□ ¿Asunciones declaradas? (R18)
```

**Tres perspectivas (R-PRISMA+):**
- 🏛️ ARQUITECTO: ¿la estructura tiene sentido? ¿rompimos algún contrato?
- 🛠️ DESARROLLADOR: ¿el código es legible, modular, testeable?
- 🧪 QA: ¿qué puede fallar? ¿qué casos edge no cubrimos?

---

## 8. Protocolo de Errores del Agente

### Tipo 1 — Bug en código
```
┌─────────────────────────────────────────────┐
│  🔴 BUG DETECTADO                           │
├─────────────────────────────────────────────┤
│  Archivo : [path]                           │
│  Línea   : [número aproximado]              │
│  Causa   : [qué falló y por qué]            │
│  Impacto : [qué otras partes afecta]        │
│  Fix     : [solución propuesta]             │
│                                             │
│  ¿Aplico el fix? APROBAR / CORREGIR         │
└─────────────────────────────────────────────┘
```

### Tipo 2 — Error de comprensión
```
┌─────────────────────────────────────────────┐
│  🟡 ERROR DE COMPRENSIÓN                    │
├─────────────────────────────────────────────┤
│  Entendí  : [qué construí]                  │
│  Correcto : [qué debería ser]               │
│  Impacto  : [qué hay que rehacer]           │
│  Opciones :                                 │
│  A) Rehacer desde Fase [N]                  │
│  B) Parchear sobre lo existente             │
│  C) Mantener actual y añadir                │
│                                             │
│  ¿Qué prefieres? A / B / C                  │
└─────────────────────────────────────────────┘
```

### Tipo 3 — Límite técnico real
```
┌─────────────────────────────────────────────┐
│  ⚠️ LÍMITE TÉCNICO ENCONTRADO               │
├─────────────────────────────────────────────┤
│  Problema : [qué no es posible y por qué]   │
│  Alcance  : [qué funciona parcialmente]     │
│  Opciones :                                 │
│  A) Alternativa vanilla                     │
│  B) Excepción justificada (CDN)             │
│  C) Reducir scope                           │
│                                             │
│  Recomiendo: [A/B/C] porque [razón]         │
│  ¿Qué prefieres? A / B / C                  │
└─────────────────────────────────────────────┘
```

---

## 9. Modo Degradación: RESET CONTEXTO

Si el modelo:
- Olvida reglas
- Mezcla fases
- No respeta formatos

El usuario escribe **`RESET CONTEXTO`**. El agente responde:

```
┌─────────────────────────────────────────┐
│  🔄 RESET DE CONTEXTO                   │
├─────────────────────────────────────────┤
│  Por favor, pega en este orden:         │
│  1. METHODOLOGY.md (este archivo)       │
│  2. AGENT-GUIDE.md                      │
│  3. CURRENT-STATUS.md                   │
│  4. ARCHITECTURE.md                     │
│  5. La última fase que funcionó bien    │
│                                         │
│  No continuaré hasta recibir esto.      │
└─────────────────────────────────────────┘
```

---

## 10. Protocolo de Recuperación de Sesión

Al iniciar sesión nueva, pegar:
1. `METHODOLOGY.md`
2. `AGENT-GUIDE.md`
3. `ARCHITECTURE.md`
4. `CURRENT-STATUS.md`
5. `qpu-XX/ARCHITECTURE.md` si se trabaja en una QPU específica
6. El archivo en el que se estaba trabajando

El agente confirma contexto antes de generar código nuevo.
