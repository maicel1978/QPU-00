# SCOPE.md — [QPU-XX — Nombre Descriptivo]

## Resumen ejecutivo
[1-2 frases. Qué hace esta QPU y por qué existe.]

## Input
- **Archivo(s) aceptado(s):** [ej: `.clinical` JSON]
- **Esquema esperado:** [referencia: `CLINICAL-CONTRACTS.md`]
- **Otros inputs:** [ej: datos del usuario, configuraciones]

## Output
- **Producto principal:** [ej: formulario HTML5 interactivo]
- **Exportación:** [ej: JSON con respuestas, CSV para análisis]
- **Persistencia local:** [ej: localStorage con respuestas en progreso]

## Usuarios objetivo
- **Primarios:** [ej: investigador clínico que diseña encuestas]
- **Secundarios:** [ej: personal de campo que captura datos con la QPU-02]

## Assumptions
- [Supuesto sobre el entorno: ej: navegador moderno, sin internet en campo]
- [Supuesto sobre los datos: ej: el `.clinical` viene validado por `oper`]
- [Supuesto sobre el usuario: ej: sin conocimientos técnicos]

## No-goals (crítico listarlos)
- ❌ [Algo que parece relacionado pero NO hace esta QPU]
- ❌ [Otro no-objetivo]

## Dependencias con otras QPUs
- **Lee de:** [ej: `.clinical` producido por `oper`]
- **Produce para:** [ej: QPU-02 puede leer su output si la JSON exportada es compatible]
- **Acoplamiento:** [ninguno / solo por `.clinical` / otro]

## Riesgos identificados
| Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|
| [ej: `.clinical` malformado] | M | A | Validar al cargar, mostrar error claro |
