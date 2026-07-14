# ACCEPTANCE-CRITERIA.md — QPU-01 Form Generator

## Criterios funcionales

### CF-01 — Carga del `.clinical`
- **Dado que** el usuario tiene un archivo `.clinical` válido,
- **cuando** lo arrastra o lo selecciona,
- **entonces** el formulario se renderiza con un campo por cada variable del array.

### CF-02 — Validación en tiempo real
- **Dado que** una variable cuantitativa tiene `range.min = 0` y `range.max = 18`,
- **cuando** el usuario intenta salir del campo con valor `-5` o `125`,
- **entonces** se muestra error claro y no se puede enviar el formulario.

### CF-03 — Exportación de datos
- **Dado que** el usuario llenó el formulario completamente,
- **cuando** hace clic en "Exportar",
- **entonces** descarga un JSON y/o CSV con los valores capturados (usando los `synonyms` o `label` como valor guardado).

## Criterios de validación del `.clinical`

### CV-01 — Parsing correcto
- [ ] Carga `examples/ejemplo-basico.clinical` sin errores.
- [ ] Renderiza los 6 tipos de campo correctamente.
- [ ] Muestra error legible ante `.clinical` inválido (no crashea).

### CV-02 — Tipos de variable
- [ ] `Nominal Dicotómica` → radio buttons (2 opciones).
- [ ] `Nominal Policotómica` → select con N opciones.
- [ ] `Ordinal` → select con orden del array respetado.
- [ ] `Cuantitativa Continua` → `<input type="number" step="any">`.
- [ ] `Cuantitativa Discreta` → `<input type="number" step="1">`.

### CV-03 — Parsing del rango
- [ ] `range.min = "0"` y `range.max = "18"` → limita input correctamente.
- [ ] `range.min = ""` → sin límite inferior.
- [ ] `range.max = ""` → sin límite superior.
- [ ] Ambos vacíos → input sin `min`/`max`.

## Criterios de UX

### CX-01 — Accesibilidad
- [ ] Tab navega por todos los campos en orden lógico.
- [ ] Foco visible en todos los controles.
- [ ] Cada input tiene su `<label for="...">` asociado.
- [ ] Mensajes de error anunciados con `aria-live="polite"`.
- [ ] Errores de envío anunciados con `aria-live="assertive"`.

### CX-02 — Robustez visual
- [ ] Responsive desde 360px hasta desktop.
- [ ] Contraste WCAG AA verificado.
- [ ] Estados: vacío, error, éxito, todos bien diseñados.

## Criterios de portabilidad

### CP-01 — Local-first
- [ ] Funciona con doble clic en `index.html` local.
- [ ] No requiere internet en ningún momento del flujo.
- [ ] No carga nada de CDNs externos.

### CP-02 — Sin build
- [ ] El usuario final no necesita `npm`, `node`, ni compilar nada.

## Smoke test manual (reproducible)

1. Abrir `index.html` con doble clic.
2. Arrastrar `../examples/ejemplo-basico.clinical` a la zona de drop.
3. Verificar que aparecen 6 campos con sus labels correctos.
4. Intentar ingresar edad = 25 → debe mostrar error.
5. Ingresar valores válidos en los 6 campos.
6. Exportar → verificar que el JSON/CSV descargado tiene los códigos (synonyms) correctos.

**Resultado esperado:** JSON con 6 pares clave-valor donde las claves coinciden con los `name` del `.clinical` y los valores son los códigos.

## Criterios de tests automatizados

- [ ] Tests unitarios de la función `parseClinical(json)` con casos válidos e inválidos.
- [ ] Tests de la función `validateField(value, variable)` para los 5 tipos.
- [ ] Tests de la función `exportData(formValues, variables)` → JSON y CSV.
