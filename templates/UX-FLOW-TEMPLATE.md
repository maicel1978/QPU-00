# UX-FLOW.md — [QPU-XX — Nombre]

## Flujo de usuario (alto nivel)

```
[Pantalla 1: ...]
   ↓ (acción del usuario)
[Pantalla 2: ...]
   ↓ ...
[Estado final: ...]
```

## Pantallas / estados

### Estado 0 — Inicial
- **Qué ve el usuario:** [descripción]
- **Acciones posibles:** [lista]
- **Empty state:** [qué pasa si no hay datos]

### Estado 1 — [Nombre]
- **Qué ve el usuario:** [descripción]
- **Acciones posibles:** [lista]

### Estado 2 — Error
- **Qué ve el usuario:** [descripción]
- **Acciones posibles:** [volver, reintentar, reportar]

### Estado 3 — Éxito / Final
- **Qué ve el usuario:** [descripción]
- **Output disponible:** [descargar, copiar, etc.]

## Micro-interacciones clave

- **Carga de archivo:** drag & drop + botón. Feedback visual inmediato.
- **Validación de campo:** en tiempo real al perder foco (`blur`).
- **Envío:** confirmación con resumen antes de exportar.
- **Cancelación:** [si aplica, cómo se cancela un proceso largo]

## Estados de error contemplados

| Error | Origen | UX |
|---|---|---|
| Archivo malformado | Input inválido | Mensaje claro + opción de reintentar |
| Variable fuera de rango | Validación de `.clinical` | Marcar el campo específico, no enviar |
| Sin permisos locales | Browser bloquea persistencia | Aviso y modo "solo sesión" |
| Worker falló | Error de procesamiento | Fallback a main thread si es viable |

## Diagrama ASCII (opcional)

```
┌─────────────┐
│  Pantalla 1 │
└──────┬──────┘
       │ click
       ▼
┌─────────────┐    error    ┌─────────────┐
│  Pantalla 2 │ ───────────▶│  Pantalla E │
└──────┬──────┘             └──────┬──────┘
       │ éxito                     │ volver
       ▼                            ▼
┌─────────────┐             ┌─────────────┐
│  Pantalla 3 │             │  Pantalla 1 │
└─────────────┘             └─────────────┘
```
