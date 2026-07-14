# UX-FLOW.md — QPU-01 Form Generator

## Flujo de usuario (alto nivel)

```
[Estado 0: Drop zone]
       ↓ (carga .clinical)
[Estado 1: Formulario renderizado]
       ↓ (usuario llena)
[Estado 1: Validación en tiempo real]
       ↓ (submit válido)
[Estado 2: Confirmación + Exportar]
       ↓ (click)
[Estado 3: Descarga de JSON/CSV]
```

## Pantallas / estados

### Estado 0 — Drop zone (inicial)
- **Qué ve el usuario:** zona de drop grande con texto "Arrastrá tu archivo `.clinical` acá" + botón "Elegir archivo" + opción de pegar JSON.
- **Empty state:** solo la drop zone.
- **Feedback:** highlight visual al arrastrar encima.

### Estado 1 — Formulario renderizado
- **Qué ve el usuario:** título del proyecto (de `project.name`), subtítulo de la especialidad, y un `<fieldset>` por cada sección lógica (en MVP, todos los campos en una sola sección).
- **Cada campo tiene:**
  - Label con la pregunta.
  - Input según el tipo.
  - Mensaje de error inline (vacío al inicio).
  - Indicador de requerido (`*` o `aria-required="true"`).

### Estado 2 — Validación
- **Por campo:** al perder foco (`blur`), se valida y muestra error si corresponde.
- **Al enviar:** si hay errores, se focus-ea al primer campo con error y se anuncia con `aria-live`.
- **Si todo OK:** transición a Estado 3.

### Estado 3 — Confirmación y exportación
- **Qué ve el usuario:** resumen de respuestas + botones "Exportar JSON" y "Exportar CSV".
- **Tras exportar:** mensaje de éxito + opción de "Volver al formulario" o "Empezar de nuevo".

## Micro-interacciones clave

- **Drag & drop:** highlight con borde de color al pasar el archivo encima.
- **Validación on-blur:** feedback inmediato, no esperar al submit.
- **Submit:** spinner o deshabilitar botón mientras "procesa" (aunque el procesamiento sea local e instantáneo, da feedback).
- **Exportación:** usar `Blob` + `URL.createObjectURL` + `<a download>`.

## Estados de error contemplados

| Error | Origen | UX |
|---|---|---|
| `.clinical` no es JSON válido | Parseo | "El archivo no es un JSON válido. Verificá que sea un `.clinical` exportado por oper." |
| Falta `project` o `variables` | Estructura | "El archivo no tiene la estructura esperada de un `.clinical`." |
| `variables` no es array | Estructura | Mismo mensaje genérico + detalle técnico en consola. |
| `type` desconocido | Variable individual | Saltar la variable con warning en consola, no romper render. |
| Valor fuera de rango | Validación de campo | Marcar el campo, mensaje específico, no enviar. |

## Diagrama ASCII

```
┌──────────────┐
│  Drop zone   │
└──────┬───────┘
       │ carga .clinical
       ▼
┌──────────────┐
│ Formulario   │◀──┐
│  (campos)    │   │ corregir
└──────┬───────┘   │
       │ submit    │
       ▼           │
┌──────────────┐  │
│ Validación   │──┘
└──────┬───────┘
       │ OK
       ▼
┌──────────────┐
│ Confirmación │
│ + Exportar   │
└──────────────┘
```
