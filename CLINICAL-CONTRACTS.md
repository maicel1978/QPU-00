# CLINICAL-CONTRACTS.md — Contrato del Formato `.clinical`

> **Este archivo documenta el contrato que todas las QPUs deben respetar al leer un archivo `.clinical`.** La fuente canónica del formato vive en el repo [`oper`](https://github.com/maicel1978/oper). Este archivo es la "vista QPU" de ese contrato: lo que necesitamos saber para implementar las QPUs.

## 1. Decisión crítica de compatibilidad

**La estructura raíz del `.clinical` es intocable:**

```json
{
  "project": {},
  "variables": []
}
```

Esto es una decisión del repo `oper` para no romper compatibilidad con aplicaciones que ya consumen el formato. **Ninguna QPU puede modificar, agregar campos obligatorios, ni remover esta raíz.**

Cualquier mejora al formato se hace en `oper`. Las QPUs la siguen.

## 2. Estructura vigente (versión estable)

```json
{
  "project": {
    "name": "Nombre del proyecto",
    "specialty": "Especialidad o área",
    "date": "YYYY-MM-DD"
  },
  "variables": [
    {
      "name": "edad",
      "type": "Cuantitativa Discreta",
      "description": "Edad cronológica en años cumplidos.",
      "metadata": {
        "question": "¿Qué edad tiene el paciente?",
        "unit": "años",
        "range": { "min": "0", "max": "115" },
        "categories": []
      }
    }
  ]
}
```

## 3. Tipos de variable reconocidos

| Valor en `type` | Mapeo de UI | Validación |
|---|---|---|
| `Nominal Dicotómica` | Radio buttons o toggle (2 opciones) | Exactamente una de las 2 categorías |
| `Nominal Policotómica` | Radio buttons o select (N opciones) | Exactamente una de las N categorías |
| `Ordinal` | Select jerárquico o radio con escala visual | Respeta el orden del array `categories` |
| `Cuantitativa Continua` | `<input type="number" step="any">` | Dentro de `range.min`–`range.max` |
| `Cuantitativa Discreta` | `<input type="number">` | Entero dentro de `range.min`–`range.max` |

## 4. ⚠️ Trampa de tipos: `range.min` y `range.max` son strings

El formato `.clinical` actual guarda `range.min` y `range.max` como **strings**, no como números. Esto es deliberado (permite `"N/A"`, vacíos, etc.) pero las QPUs deben parsear con cuidado.

**Regla de parsing para QPUs:**

```javascript
// Pseudo-código (no incluir literalmente, es referencial)
const minStr = variable.metadata.range?.min;
const maxStr = variable.metadata.range?.max;
const min = (minStr === "" || minStr == null) ? -Infinity : Number(minStr);
const max = (maxStr === "" || maxStr == null) ? Infinity : Number(maxStr);
if (Number.isNaN(min) || Number.isNaN(max)) {
  // Reportar como dato inválido del .clinical, no crash
}
```

**Convención:** si `min` está vacío → sin límite inferior. Si `max` está vacío → sin límite superior. **Si ambos están vacíos** → la variable cuantitativa no tiene rango (raro, pero posible).

## 5. Categorías

```json
{
  "label": "Masculino",
  "synonyms": ["m", "masc", "1"]
}
```

- `label`: el valor canónico que se muestra al usuario y se guarda por defecto.
- `synonyms`: valores que pueden aparecer en datos "sucios" y deben normalizarse al `label` (esto lo aprovecha QPU-03 Data Cleaner).

## 6. Codificación interna en QPUs

**Convención para las QPUs que almacenan datos:**

- Al **mostrar**: siempre el `label`.
- Al **guardar/exportar**: el **primer elemento de `synonyms`**, o el `label` mismo si `synonyms` está vacío. Esto garantiza compatibilidad con scripts de R/Python que esperan códigos.
- En el JSON de exportación, el campo se llama igual que `variable.name` (snake_case).

## 7. Reglas de validación transversal

Ninguna QPU puede:
- ❌ Inventar categorías que no estén en `metadata.categories`
- ❌ Aceptar valores fuera de `range` sin marcar como outliers
- ❌ Asumir tipos que no estén en la tabla de §3
- ❌ Saltarse `metadata.question` (es la fuente del label visible)

## 8. Versionado del contrato

El contrato lo define `oper`. QPU-00 sigue la versión publicada allá. Si una QPU necesita un campo nuevo:
1. Se propone como cambio a `oper`.
2. Se acepta en `oper`.
3. Recién después, una QPU puede consumirlo.

No hay versionado paralelo en QPU-00. Una sola fuente.
