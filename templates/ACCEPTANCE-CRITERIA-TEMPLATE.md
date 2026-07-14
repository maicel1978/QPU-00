# ACCEPTANCE-CRITERIA.md — [QPU-XX — Nombre]

## Criterios funcionales (qué tiene que hacer)

### CF-01 — [Nombre del criterio]
- **Dado que** [contexto/estado inicial],
- **cuando** [acción del usuario o evento],
- **entonces** [resultado esperado].

### CF-02 — [Otro criterio]
- **Dado que** ...
- **cuando** ...
- **entonces** ...

## Criterios de validación del `.clinical`

### CV-01 — Parsing correcto
- [ ] Carga un `.clinical` válido y muestra todos los campos.
- [ ] Reporta error claro con archivo malformado (no crashea).
- [ ] Acepta el archivo canónico de `examples/`.

### CV-02 — Tipos de variable
- [ ] Renderiza correctamente los 5 tipos: Nominal Dicotómica, Nominal Policotómica, Ordinal, Cuantitativa Continua, Cuantitativa Discreta.
- [ ] Respeta `range.min/max` parseando correctamente el string.

## Criterios de UX

### CX-01 — Accesibilidad
- [ ] Navegación completa por teclado (tab/enter/esc/arrow).
- [ ] Foco visible en todos los controles.
- [ ] Labels asociados a inputs.
- [ ] Mensajes de error anunciados con `aria-live`.

### CX-02 — Robustez visual
- [ ] Responsive en pantallas desde 360px.
- [ ] Contraste WCAG AA.
- [ ] Estados vacíos y de error bien diseñados.

## Criterios de portabilidad

### CP-01 — Local-first
- [ ] Funciona con doble clic en el `.html` local.
- [ ] No requiere internet en ningún momento.
- [ ] No carga nada de CDNs externos.

### CP-02 — Sin build
- [ ] El usuario final no necesita `npm`, `node`, ni compilar nada.

## Smoke test manual (reproducible)

1. Abrir `qpu-XX/index.html` con doble clic.
2. Cargar `examples/ejemplo-basico.clinical`.
3. Verificar que aparecen todos los campos del ejemplo.
4. [Pasos específicos de esta QPU]
5. Verificar export con datos de prueba.

**Resultado esperado:** [descripción del output esperado]

## Criterios de tests automatizados (si aplica)

- [ ] Tests unitarios del core (parsing, validación).
- [ ] Tests de integración del flujo principal.
- [ ] Cobertura mínima: [% objetivo]
