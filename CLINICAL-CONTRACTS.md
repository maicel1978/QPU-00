# CLINICAL CONTRACTS — Especificación del Formato `.clinical`

Cualquier utilidad del ecosistema QPU debe ser capaz de parsear y respetar estrictamente el formato `.clinical` de entrada.

## 1. Esquema JSON de Validación (`.clinical`)

El archivo `.clinical` es un JSON estructurado que define la operacionalización del protocolo clínico. 

Ejemplo de parseo basado en `operacionalizacion (3).clinical`:

```json
{
  "project": {
    "name": "operacionalizacion",
    "specialty": "pediatria",
    "date": "2026-06-06"
  },
  "variables": [
    {
      "name": "sexo",
      "type": "Nominal Dicotómica",
      "description": "Sexo biológico del paciente",
      "metadata": {
        "question": "¿Cuál es el sexo biológico?",
        "unit": "",
        "range": { "min": "", "max": "" },
        "categories": [
          { "label": "Masculino", "synonyms": ["m", "1"] },
          { "label": "Femenino", "synonyms": ["f", "2"] }
        ]
      }
    }
  ]
}
```

## 2. Reglas de Mapeo de Variables a UI (QPU-01 & QPU-02)

Al leer una variable del array, la QPU de captura de datos debe renderizar la interfaz según la siguiente lógica metodológica:

| Tipo en `.clinical` | Mapeo de UI Recomendado | Reglas de Validación |
|---------------------|------------------------|---------------------|
| **Nominal Dicotómica** | Grupo de botones excluyentes (Radio Buttons o Toggle). | Debe obligar a seleccionar una de las 2 categorías definidas. |
| **Nominal Policotómica** | Lista de botones de opción o desplegable simple. | No permite entradas libres. Muestra las etiquetas (label). |
| **Ordinal** | Lista jerárquica con escala visual o menús secuenciales. | Respeta el orden declarado en el array de categorías. |
| **Cuantitativa Continua o Discreta** | Input numérico (`<input type="number">`). | Aplica de forma estricta los atributos min y max definidos en range. No permite guardar si está fuera de límites. |

## 3. Manejo de Sinónimos y Codificación

Durante la captura en QPU-02, aunque el usuario final ve y selecciona la etiqueta legible (label como "Masculino"), la aplicación debe almacenar internamente el primer elemento del array de sinónimos (synonyms como "1") para garantizar que la exportación sea compatible con scripts bioestadísticos directos en R.

---

## 4. ACCEPTANCE CRITERIA — Ecosistema QPU (La Experiencia de Usuario)

*Este archivo define los criterios de UX y accesibilidad. Al igual que con APU, queremos interfaces que médicos sin conocimientos técnicos puedan usar con teclado y lectores de pantalla en zonas rurales sin conexión.*

### Criterios Funcionales Transversales

**AC-01 — Robustez en el punto de captura (Offline-First)**
- **Dado que** el personal médico está recopilando datos en un área sin cobertura telefónica,
- **cuando** ingresa información en **QPU-02**,
- **entonces** la app debe validar y almacenar localmente las respuestas en el navegador, permitiendo la descarga segura en formato JSON/CSV una vez finalizado el proceso.

**AC-02 — Bloqueo defensivo de datos fuera de rango**
- **Dado que** una variable cuantitativa (ej: `edad`) tiene un rango definido de `0` a `120`,
- **cuando** el capturador intenta escribir `-5` o `125`,
- **entonces** el formulario debe mostrar un feedback visual claro de error en tiempo real e impedir el envío del formulario.

**AC-03 — Portabilidad sin fricción**
- **Dado que** se distribuyen los módulos QPU,
- **cuando** el usuario descarga el archivo HTML de la utilidad,
- **entonces** la aplicación debe funcionar con un simple doble clic local, sin necesidad de realizar instalaciones de dependencias.

### Criterios de Accesibilidad (Basados en APU)

- **AC-04 — Operación con teclado:** El diseño de formularios en QPU-01 y la entrada de datos en QPU-02 deben poder realizarse enteramente con navegación por teclado.
- **AC-05 — Foco visible y claro:** Todos los controles interactivos y de opción múltiple deben tener indicadores de foco visibles y claros.