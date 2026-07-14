# SCOPE.md

## App: QPU-01 Form Generator
## Core: Lee un archivo .clinical y asiste visualmente en el diseño y ordenamiento de una encuesta web

### Input
- Archivo .clinical (JSON estructurado) con:
  - project.name, project.specialty, project.date
  - variables array with name, type, description, metadata (question, unit, range, categories)

### Output
- HTML5 semántico con formulario accesible
- CSS vanilla para estilización básica
- JavaScript vanilla para validación y dinámica
- Ordenamiento de campos según la estructura del .clinical

### Users
- Investigadores clínicos que diseñan encuestas para estudios
- Personal médico que captura datos en campo

### Assumptions
- .clinical file follows the schema in CLINICAL-CONTRACTS.md
- Variables types are correctly defined (Nominal Dicotómica, Policotómica, Ordinal, Cuantitativa)
- The form should enforce validation rules based on variable types and ranges
- The form should be accessible (keyboard navigation, ARIA labels)
- No build step required, single HTML file preferred
</parameter>
</function>
<task_progress>
- [x] Fase -1: Descubrimiento - Recibir entrada del usuario (QPU-01 seleccionado)
- [x] Fase -1: Construir mapa inicial (LO QUE ENTENDÍ) - COMPLETADO
- [ ] Fase -1: Iteración (máximo 3 rondas de aclaración) - PENDIENTE
- [ ] Fase -1: Cierre - Generar docs/SCOPE.md, docs/ACCEPTANCE-CRITERIA.md, docs/UX-FLOW.md - PENDIENTE
- [ ] Fase 0 QPU-01: Viabilidad + Stack - PENDIENTE
- [ ] Fase 1 QPU-01: Estructura + Contratos - PENDIENTE
- [ ] Fase 2 QPU-01: UI + CSS - PENDIENTE
- [ ] Fase 3 QPU-01: Lógica Core - PENDIENTE
- [ ] Fase 4 QPU-01: Workers - PENDIENTE
- [ ] Fase 5 QPU-01: Robustez - PENDIENTE
</task_progress>
</write_to_file>
</tool_call>