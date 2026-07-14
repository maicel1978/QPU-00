# ACCEPTANCE CRITERIA.md — QPU-01 Form Generator

## 1. Funcionalidades Requeridas
- [ ] **Carga de .clinical**: La aplicación debe leer un archivo `.clinical` válido (JSON estructurado según CLINICAL-CONTRACTS.md).
- [ ] **Generación de formulario**: Crear un formulario HTML5 semántico con campos según las variables definidas en `.clinical`.
- [ ] **Validación en tiempo real**: Aplicar reglas de validación según el tipo de variable (Nominal Dicotómica, Ordinal, Cuantitativa) y sus rangos.
- [ ] **Accesibilidad**: 
  - Navegación por teclado completa.
  - Etiquetas ARIA claras para lectores de pantalla.
  - Enfoque visible en campos interactivos.
- [ ] **Exportación de datos**: Al finalizar, permitir descarga de los datos ingresados en formato JSON/CSV.
- [ ] **Sin build step**: La aplicación debe funcionar como un único archivo HTML (sin dependencias externas).

## 2. Reglas de Aceptación
- [ ] El formulario debe respetar estrictamente el esquema del `.clinical` (no se permiten variables adicionales o faltantes).
- [ ] Los mensajes de error deben ser claros y localizados (ej: "El campo 'edad' debe estar entre 0 y 18 años").
- [ ] La aplicación debe funcionar en navegadores modernos sin necesidad de instalación.
- [ ] No se permiten frameworks (React, Vue, etc.) en el runtime.

## 3. MVP Específico
- [ ] Soportar al menos 5 variables de ejemplo (ej: edad, sexo, altura, peso, categoría de BMI).
- [ ] Validación de rangos y categorías definidas en `.clinical`.
- [ ] Estructura de formulario accesible y semántica.