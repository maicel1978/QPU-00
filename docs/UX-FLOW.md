# UX-FLOW.md — QPU-01 Form Generator

## Flujo de Usuario
1. **Inicio**: La aplicación carga y muestra una interfaz para cargar o ingresar manualmente el archivo `.clinical`.
2. **Procesamiento**: Al cargar el archivo, se analiza su estructura y se genera el formulario dinámicamente.
3. **Relleno del formulario**: El usuario completa los campos según las variables definidas.
4. **Validación**: Se aplican reglas de validación en tiempo real (ej: rangos, tipos de datos).
5. **Finalización**: Al enviar el formulario, se valida todo el contenido y se permite la descarga de los datos en JSON/CSV.

## Diagrama de Flujo
```
[Inicio] 
  ↓
[Cargar/Ingresar .clinical]
  ↓
[Generar formulario dinámico]
  ↓
[Rellenar formulario con validación en tiempo real]
  ↓
[Enviar formulario → Validación final → Descarga de datos]
```

## Requisitos de UX
- [ ] Interfaz intuitiva para cargar el archivo `.clinical` (botón de "Elegir archivo" o campo de texto para pegar JSON).
- [ ] Formulario bien organizado por secciones (ej: datos demográficos, medidas antropométricas).
- [ ] Feedback visual claro para errores (colores, mensajes en tiempo real).
- [ ] Opción de descargar datos sin necesidad de conexión a internet.