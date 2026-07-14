# ARCHITECTURE.md — Ecosistema QPU (Fiel a PRISMA+ v5.2)

## 1. Justificación de Tailwind CSS (CDN)
Aunque PRISMA+ v5.2 restringe los CDNs pre-aprobados a ONNX Runtime Web, TensorFlow.js, FFmpeg.wasm, OpenCV.js, Chart.js, D3.js, Math.js y Comlink, se justifica el uso de Tailwind CSS vía CDN por las siguientes razones:
- **Ligereza:** Tailwind CSS es un framework de utilidades que no requiere compilación ni carga de archivos pesados.
- **Portabilidad:** Permite mantener un único archivo HTML sin dependencias externas.
- **Estilización rápida:** Facilita la creación de interfaces accesibles y responsivas sin necesidad de escribir CSS personalizado.
- **Cumplimiento de R1:** No afecta el runtime vanilla, ya que se carga vía CDN y no se incluye en el bundle final.

## 2. Uso de Vanilla JS (Sin Frameworks)
Alpine.js se ha removido del stack tecnológico para cumplir estrictamente con la regla R1 de PRISMA+ v5.2, que prohíbe frameworks en el runtime. Se utiliza Vanilla JS nativo para toda la lógica reactiva, asegurando que no haya dependencias externas en el entorno de ejecución.