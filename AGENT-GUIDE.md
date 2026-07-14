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

Antes de escribir o refactorizar cualquier archivo de código, el agente debe mostrar:

```
┌─────────────────────────────────────────────┐
│  📝 PRE-CÓDIGO                              │
├─────────────────────────────────────────────┤
│  Archivo   : [path completo]                │
│  Contratos : [de API-CONTRACTS.md]          │
│  Reglas    : [R1, R4, R6... las que         │
│              apliquen a este archivo]       │
│  LOC objetivo: [≤ 35 líneas por cambio]     │
│                                             │
│  Generando código ahora...                  │
└─────────────────────────────────────────────┘
```

**Si no podés listar esto, no generes el código. Preguntá qué falta.**

## 3. Límite duro: ~35 LOC por cambio

Ninguna propuesta de código del agente debe superar las **~35 líneas de código modificadas o nuevas** sin:
- Justificación explícita en el bloque Pre-Código
- Al menos 1 test que valide el cambio
- Actualización de los docs afectados

Si el cambio requiere más, **se parte en commits chicos**, cada uno con su gate.

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
