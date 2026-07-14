# QPU Ecosystem — Quantitative & Protocol Utilities (QPU-0)

Este repositorio es el **Hub Central** del ecosistema QPU. Define la metodología, arquitectura, contratos de datos y estándares de desarrollo bajo los cuales deben construirse todas las herramientas de soporte a la investigación clínica y bioestadística.

## 1. Filosofía: El Enfoque Modular Asistido (Lego)

El ecosistema QPU rechaza los softwares monolíticos gigantes y cerrados. Cada utilidad (QPU) es un componente independiente, ligero y portátil que asiste al investigador científico de forma quirúrgica en una etapa específica del proceso, tomando como única fuente de verdad el archivo de metadatos clínicos `.clinical` generado en la fase de **Operacionalización (OPER)**.

[ OPER: Operacionalización ]
    │
    └─ Genera .clinical
    │
    ├─ QPU-01 (Form Generator)
    ├─ QPU-02 (Data Collector)
    └─ QPU-03 (Data Cleaner)

## 2. Catálogo Indexado de QPUs

* **QPU-01 (Form Generator):** Lee un archivo `.clinical` y asiste visualmente en el diseño y ordenamiento de una encuesta web.
* **QPU-02 (Data Collector):** Aplicación autónoma local para la entrada de datos con validación en tiempo real de rangos y categorías.
* **QPU-03 (Data Cleaner):** Herramienta asistida de consistencia lógica basada en el esquema del `.clinical` para depuración antes del análisis.
* **QPU-04 (OMR Assistant):** Generador de plantillas de papel y digitalizador automático de encuestas mediante marcas de reconocimiento óptico.
* **QPU-05 (Exploratory Data Analysis):** Reporte automatizado de estadística descriptiva basal (Tablas de frecuencia, distribuciones y análisis exploratorio).
* **QPU-06 (Cross-Sectional Analysis):** Análisis estadístico e indicadores epidemiológicos específicos para estudios transversales.
* **QPU-07 (Case-Control & Longitudinal):** Análisis estadístico para estudios de casos y controles y análisis longitudinal.
* **QPU-08 (Cohort Analysis):** Modelos de supervivencia y cálculo de riesgos en estudio.