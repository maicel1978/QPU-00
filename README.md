# QPU Ecosystem Hub (Quantitative & Protocol Utilities) - QPU-0

Bienvenido al repositorio central del ecosistema **QPU**. Este repositorio actúa como la **Especificación Madre** y el *Single Source of Truth* (Única Fuente de Verdad) metodológica y técnica para el desarrollo de herramientas asistidas de investigación clínica y bioestadística.

## 1. Declaración de Filosofía y Propósito
El ecosistema QPU no es un software monolítico ni una plataforma en la nube (SaaS). Es un **sistema modular asistido basado en piezas de Lego**.
El núcleo del ecosistema es el proceso de **Operacionalización de Variables (OPER)**. Cuando una investigación define sus variables, genera un archivo de metadatos con extensión `.clinical` (JSON estructurado). A partir de ahí, el flujo de trabajo es asistido por utilidades independientes, ultraespecíficas y portátiles denominadas **QPUs (Quantitative & Protocol Utilities)**. 
Las QPUs no automatizan a ciegas el proceso científico; asisten al investigador eliminando la programación manual, los errores de transcripción y garantizando el rigor metodológico desde el diseño hasta el análisis avanzado.

## 2. Principios de Arquitectura Innegociables (Core Architectural Rules)
Cualquier código escrito para una QPU debe cumplir con los siguientes cuatro pilares:
1. **Local-First / Serverless Estricto:** Toda la lógica, procesamiento y persistencia temporal ocurren en el cliente (navegador web). No se permiten bases de datos centralizadas, servicios en la nube (Firebase, AWS, Supabase) ni backend activo.
2. **Monolitos Portables (Single-File Architecture):** Cada QPU debe ser, preferiblemente, una aplicación autocontenida en un único archivo (`.html`) que incluya su propio CSS y JS, o un conjunto mínimo de archivos estáticos sin necesidad de compilación pesada. Debe poder ejecutarse haciendo doble clic localmente en cualquier navegador.
3. **Fidelidad al Metadato (.clinical):** El archivo `.clinical` es la ley. Ninguna QPU puede inventar estructuras de datos o saltarse las restricciones de tipo (Nominal, Ordinal, Cuantitativa), rangos, unidades o sinónimos definidos en la operacionalización de origen.
4. **Modularidad Quirúrgica:** Una QPU hace **una sola cosa** de forma brillante. No se permite el *scope creep* (adición de funciones de otros módulos). Si una QPU genera formularios, no procesa estadísticas.

## 3. Pila Tecnológica Estándar (Tech Stack)
Para garantizar la portabilidad y ligereza del ecosistema, las herramientas permitidas son:
* **Estructura:** HTML5 semántico.
* **Estilos:** CSS vanilla (sin frameworks ni CDNs).
* **Reactividad/Lógica:** Vanilla JS o Alpine.js (con justificación en ARCHITECTURE.md).
* **Persistencia Local:** LocalStorage / IndexedDB (para almacenamiento temporal de datos de captura o configuraciones).

## 4. El Conector Universal: Formato `.clinical` (Esquema Base)
Todas las QPUs deben ser capaces de parsear e interpretar la estructura del archivo `.clinical`. A continuación se muestra el contrato de datos JSON de referencia que toda utilidad debe soportar:
...