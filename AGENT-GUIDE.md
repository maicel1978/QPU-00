# AGENT-GUIDE.md — Reglas para Agentes de IA en el Ecosistema QPU

> **Este archivo es la guía canónica para cualquier agente de IA** (Claude, GPT, Gemini, etc.) que trabaje en el ecosistema QPU. Si hay contradicción con otros documentos, gana `METHODOLOGY.md` para reglas de proceso y `ARCHITECTURE.md` para reglas de stack.

## 1. Antes de tocar nada: leer el pack de contexto

Un agente nuevo **debe recibir este pack antes de generar código**:

1. El prompt PRISMA+ v5.2 (metodología general de trabajo)
2. `METHODOLOGY.md` (reglas adaptadas al ecosistema QPU)
3. `ARCHITECTURE.md` (verdades de stack)
4. `CURRENT-STATUS.md` (en qué fase estamos)
5. `qpu-XX/ARCHITECTURE.md` si se va a trabajar en una QPU específica
6. El archivo concreto en el que se va a trabajar

Si no recibís todo esto, pedilo. **No improvises contexto.**

## 2. Protocolo de Pre-Código (obligatorio)

Antes de escribir o refactorizar cualquier archivo de código, el agente debe mostrar este bloque. Si falta cualquiera de los campos, **no genera código**: pregunta qué falta.

```
┌─────────────────────────────────────────────┐
│  📝 PRE-CÓDIGO                              │
├─────────────────────────────────────────────┤
│  Intención    : [una frase, ¿qué resuelve?] │
│  Archivo      : [path completo]             │
│  Otros archivos: [si aplica, máx 1-2]       │
│  Contratos    : [de API-CONTRACTS.md]       │
│  Reglas       : [R1, R4, R6... aplicables]  │
│  Test         : [qué test verifica el cambio│
│                 y cómo se ejecuta]          │
│  Diff estimado: [¿entra en 1 pantalla? S/N] │
│                                             │
│  📌 Asunciones (R18):                       │
│  - [supuesto 1 que no está en el contexto]  │
│  - [supuesto 2]                             │
│                                             │
│  Generando código ahora...                  │
└─────────────────────────────────────────────┘
```

**Excepción — Ventana de Creatividad (R16):** si la petición es exploratoria ("proponé opciones", "explorá cómo harías X"), el agente **no** usa Pre-Código todavía. Primero presenta 2-3 enfoques sin código, vos elegís uno, y recién ahí arranca el flujo normal.

**Excepción — Nudges de creatividad (R17):** durante la implementación, si el agente encuentra una forma más simple, debe reportarla con el bloque `💡 MEJORA DETECTADA` antes de aplicarla (ver `METHODOLOGY.md` R17).

**Si no podés llenar el bloque, no generes código.** Preguntá qué falta.

## 3. Tamaño y forma del cambio (R15)

Un cambio del agente debe cumplir **las cuatro condiciones de R15**:

1. **Una sola intención** (declarable en una frase, sin "y").
2. **Diff revisable en una pantalla** (≈ 50 líneas visibles).
3. **Máximo de archivos tocados**: 1 archivo de lógica (+ tests), hasta 2 de UI, varios en docs.
4. **Test que falla antes y pasa después** (unitario, integración o smoke test manual según el caso).

Si no se cumplen las cuatro, **se parte en cambios más chicos**. Esta restricción no es negociable, pero no es por LOC: es por **revisabilidad**.

**Anti-pattern:** el agente no puede argumentar "este cambio es chico, no necesita test". El test se escribe siempre.

Ver `METHODOLOGY.md` R15 para el detalle completo y las excepciones legítimas.

## 4. Tests obligatorios

Cada cambio que toque lógica debe incluir:
- Al menos 1 test que cubra el caso feliz
- Al menos 1 test que cubra el caso de error o borde
- El test debe ser ejecutable localmente (ver `METHODOLOGY.md` §6)

## 5. Documentación obligatoria

Cada cambio debe mantener sincronizados:
- El `qpu-XX/SCOPE.md` si cambia el alcance
- El `qpu-XX/ACCEPTANCE-CRITERIA.md` si cambia el comportamiento esperado
- El `qpu-XX/CHANGELOG.md` con la entrada del cambio
- `CURRENT-STATUS.md` en la raíz si cambia la fase global

**Degradación cero:** si tocás lógica y no actualizás los docs, el cambio se rechaza.

## 6. Comentario vanilla obligatorio

Todo archivo `.js` debe empezar con:

```javascript
/**
 * PRISMA+ v5.2 adaptado a QPU-00 — Vanilla JS ES2022+ Modules
 * Runtime: NO frameworks (R1)
 * [Descripción breve del archivo]
 */
```

## 7. Lo que el agente NO debe hacer

- ❌ Saltarse el Pre-Código
- ❌ Generar archivos de más de 350 líneas sin justificación
- ❌ Inventar campos del `.clinical` que no estén documentados
- ❌ Agregar dependencias de runtime (frameworks, CDNs)
- ❌ Optimizar silenciosamente sin reportar la mejora detectada
- ❌ Romper compatibilidad con `.clinical` sin discutirlo en una ADR mental primero

## 8. Cómo reportar problemas

- **Bug en código propio** → bloque `🔴 BUG DETECTADO` (ver `METHODOLOGY.md` §8)
- **Error de comprensión** → bloque `🟡 ERROR DE COMPRENSIÓN`
- **Límite técnico encontrado** → bloque `⚠️ LÍMITE TÉCNICO`
- **Mejora detectada no solicitada** → bloque `💡 MEJORA DETECTADA` y esperar APROBAR/IGNORAR

## 9. Comando de reset

Si el agente pierde el rumbo, el usuario puede escribir **`RESET CONTEXTO`**. El agente debe responder con el bloque de reset y pedir el pack completo. **No continuar hasta recibirlo.**
