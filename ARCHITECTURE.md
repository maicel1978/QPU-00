# ARCHITECTURE.md — Ecosistema QPU

> **Este archivo es la fuente única de verdad sobre el stack técnico del ecosistema QPU.** Cualquier otra mención de stack en otros archivos debe ser consistente con este. Si hay contradicción, gana este archivo.

## 1. Filosofía de stack

El ecosistema QPU busca **máxima portabilidad y offline-first**. Las QPUs son aplicaciones que un investigador descarga, abre con doble clic en su navegador, y usa en zonas sin conectividad. Esto fuerza tres consecuencias sobre el stack:

1. **Cero dependencias de red en runtime.** Sin CDNs, sin fuentes web, sin imágenes externas.
2. **Un solo archivo (o conjunto mínimo de archivos estáticos).** Sin paso de compilación. Sin `npm install` para el usuario final.
3. **Runtime siempre vanilla.** HTML5 + CSS3 + JavaScript ES2022+ Modules nativos.

## 2. Stack canónico (regla base)

| Capa | Tecnología | Notas |
|---|---|---|
| Estructura | HTML5 semántico | Landmarks, headings jerárquicos, labels asociados. |
| Estilos | CSS vanilla puro | Sin Tailwind, sin frameworks, sin preprocesadores. |
| Lógica | Vanilla JS ES2022+ Modules | Sin React, Vue, Angular, Svelte, Alpine. |
| Persistencia local | `localStorage` / `IndexedDB` | Solo almacenamiento temporal del usuario. |
| Worker pattern | `MessageChannel` directo | Sin Comlink (requeriría build step). |

### 2.1 Lo que está prohibido en runtime
- ❌ Frameworks de UI (React, Vue, Svelte, Angular, Alpine, etc.)
- ❌ Frameworks de CSS (Tailwind, Bootstrap, Bulma, etc.)
- ❌ CDNs de cualquier tipo (jsDelivr, unpkg, Google Fonts, etc.)
- ❌ Llamadas a APIs externas sin consentimiento explícito del usuario (R8 de PRISMA+)
- ❌ Build step obligatorio (Vite/Webpack/etc.) para el usuario final

### 2.2 Lo que está permitido como tooling de desarrollo
- ✅ ESLint y Prettier como `devDependencies` (no afectan el runtime)
- ✅ Vite solo si se justifica explícitamente en `qpu-XX/ARCHITECTURE.md` y el output sigue siendo estático
- ✅ Tests con framework vanilla (ver `METHODOLOGY.md` §6)

## 3. Decisión de stack por defecto: vanilla puro, sin build step

**Justificación:**
- El catálogo actual (8 QPUs) son aplicaciones pequeñas, de una sola responsabilidad.
- El usuario final es un investigador que descarga un `.html` y lo abre. Cualquier fricción (instalar Node, compilar) es inaceptable.
- El offline-first es la propuesta de valor diferencial del ecosistema.
- No hay evidencia actual de que necesitemos tree-shaking o HMR para QPUs pequeñas.

**Si en el futuro una QPU requiere Vite u otro build step:**
1. Justificar en el `qpu-XX/ARCHITECTURE.md` específico.
2. Mantener el output final como archivos estáticos servibles sin servidor.
3. Documentar el comando de build y verificar que `npm run build` produce un bundle portable.

## 4. Arquitectura de procesamiento (resumen)

- **Operaciones < 200ms**: main thread, asincrónicas con `async/await`.
- **Operaciones ≥ 200ms o datasets > 10k filas**: Web Worker con `MessageChannel` directo.
- **Procesamiento intensivo (ML, audio, imagen)**: Web Worker obligatorio.

Detalle completo en `METHODOLOGY.md` §4.

## 5. Compatibilidad con `.clinical`

El formato `.clinical` es **intocable en su raíz**: `{ "project": {}, "variables": [] }`. Esta decisión viene del repo `oper` y se respeta en todo el ecosistema QPU.

Ver detalle de campos en `CLINICAL-CONTRACTS.md` (que apunta a la fuente canónica en `oper`).

## 6. Versionado

- **Stack del ecosistema**: versión en este archivo. Cambios incompatibles → bump mayor.
- **Contrato `.clinical`**: versionado en `oper`. QPU-00 sigue la versión de `oper`.
- **Cada QPU**: versiona su propio `qpu-XX/ARCHITECTURE.md` y su `CHANGELOG.md` interno.
