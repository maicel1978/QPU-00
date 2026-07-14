# SCOPE.md — QPU-01 Form Generator

## Resumen ejecutivo
Lee un archivo `.clinical` (JSON) y genera dinámicamente un formulario HTML5 accesible, con validación en tiempo real y exportación de datos. Es la **puerta de entrada al ecosistema QPU**: convierte la operacionalización abstracta en algo que se puede probar y usar.

## Input
- **Archivo principal:** `.clinical` (JSON estructurado, ver `CLINICAL-CONTRACTS.md`).
- **Interacción:** drag & drop o selector de archivo + pegado opcional de JSON.
- **Configuración:** orden de campos, visibilidad de variables (futuro).

## Output
- **Producto principal:** formulario HTML5 interactivo renderizado en el navegador.
- **Exportación:** JSON con respuestas, CSV con respuestas (encabezados = `variable.name`).
- **Persistencia local:** localStorage con respuestas en progreso (recuperación ante cierre accidental).

## Usuarios objetivo
- **Primarios:** investigador clínico que diseña la encuesta y la prueba localmente.
- **Secundarios:** programador que adapta la UI; personal de campo que usa la versión final (que probablemente sea QPU-02).

## Assumptions
- El `.clinical` viene validado por `oper` y respeta la raíz `{project, variables}`.
- `range.min` y `range.max` vienen como string (ver `CLINICAL-CONTRACTS.md` §4).
- El usuario tiene un navegador moderno (Chrome/Firefox/Safari/Edge recientes).
- Sin conexión a internet en el momento de uso.

## No-goals
- ❌ **No captura datos en campo de forma robusta** (eso es QPU-02).
- ❌ **No analiza datos** (eso son QPU-05/06/07/08).
- ❌ **No edita el `.clinical`** (eso es `oper`).
- ❌ **No requiere backend** ni autenticación.
- ❌ **No usa frameworks de UI** ni CDNs (refuerza offline-first).

## Dependencias con otras QPUs
- **Lee de:** `.clinical` producido por `oper`.
- **Produce para:** QPU-02 puede consumir el JSON exportado (estandarizar formato si se confirma).
- **Acoplamiento:** solo por el contrato `.clinical`. Sin código compartido.

## Riesgos identificados
| Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|
| `.clinical` malformado | M | A | Validar al cargar, mostrar error específico, no crashear |
| `range` con strings no numéricos | M | M | Try/catch + fallback a "sin límite" + log |
| Categorías con sinónimos vacíos | B | B | Convención: usar label como código si no hay synonyms |
| Archivo `.clinical` enorme (cientos de variables) | B | M | Render progresivo + warning si > 50 variables |
