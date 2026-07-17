# HUMAN-GUIDE.md — Tu hoja de ruta (simple, sin vueltas)

> **Para vos. Solo lo que tenés que hacer. Nada de teoría.**

---

## 🎯 LA REGLA DE ORO

> **Todo se hace en esta carpeta. No copiés nada a otro lado. No crees otros repos. Acá se desarrolla, acá se guarda, acá se sube.**

---

## 📋 TU DÍA A DÍA (5 pasos, siempre igual)

### 1. ACTUALIZÁ
```bash
git pull
```

### 2. MIRÁ QUÉ TOCA HOY
Abrí **`CURRENT-STATUS.md`** → Buscá **"Próxima acción atómica"**
- Ahí tenés **comandos listos para copiar/pegar/ejecutar**
- No pensés. Solo copiá → pegá en terminal → Enter

### 3. COMPLETÁ LO QUE FALTA
- Se te abrieron archivos `.md` en VS Code
- Son **checklists y tablas**. Marcá casillas, completá lo que falte
- No escribás essays. Solo: ✅ sí / ❌ no / "texto corto"

### 4. PEDILE AL AGENTE
- Abrí el chat (ChatGPT / Claude / Gemini)
- **Pegá exactamente lo que dice la tabla de abajo** (según qué estés haciendo)
- Esperá a que el agente responda: `✅ CONTEXTO CONFIRMADO`
- Escribí: **"Confirmado"**

### 5. GUARDÁ Y SUBÍ
```bash
git add -A && git commit -m "descripción corta" && git push
```

---

## 📋 QUÉ PEGAR AL AGENTE (según qué hagas hoy)

| Si hoy vas a... | Pegá ESTO en el chat (en orden) |
|---|---|
| **Empezar QPU-01 (Fase 0)** | 1. `METHODOLOGY.md` 2. `AGENT-GUIDE.md` 3. `ARCHITECTURE.md` 4. `CURRENT-STATUS.md` 5. **Todos los `.md` de `qpu-01-form-generator/`** 6. El archivo que vayas a tocar |
| **Seguir QPU-01 (Fase 1 en adelante)** | Lo de arriba + `qpu-01-form-generator/API-CONTRACTS.md` |
| **Evaluar un prototipo tuyo (OMR, calculadoras, etc.)** | 1. `METHODOLOGY.md` 2. `AGENT-GUIDE.md` 3. `ARCHITECTURE.md` 4. `CURRENT-STATUS.md` 5. El prototipo (URL / HTML / carpeta) 6. Prompt: *"Evaluá esto para QPU-XX: qué se reusa, qué se adapta, qué se descarta. Informe corto, sin código."* |
| **Revisar si está todo bien** | 1. `METHODOLOGY.md` 2. `AGENT-GUIDE.md` 3. `ARCHITECTURE.md` 4. `CURRENT-STATUS.md` 5. `CHANGELOG.md` 6. La carpeta `qpu-XX/` a revisar |

> **Regla simple**: Un chat = una QPU. No pegues cosas de QPU-03 si estás en QPU-01.

---

## 🚫 LO QUE NUNCA HACÉS

| No hagas esto | Por qué |
|---|---|
| Copiar carpetas a otro lado | Pierde sincronía. Trabajá **acá**. |
| Crear otro repo en GitHub | Este repo **es** el central. Los otros se crean **solo al final** para Netlify. |
| Pegar docs de otra QPU | El agente se confunde. **Un chat = una QPU**. |
| Saltarte `CURRENT-STATUS.md` | Es tu brújula. Siempre abrílo primero. |
| Escribir código vos mismo | Para eso está el agente. Vos decidís, él codea. |

---

## 🆘 SI ALGO FALLA

| Problema | Solución (copiá y pegá) |
|---|---|
| Agente dice tonterías / se olvida reglas | Escribí: `RESET CONTEXTO` → volvé a pegar el pack completo |
| Netlify falla al hacer deploy | Ejecutá: `node tools/audit-vanilla.js qpu-01-form-generator` → arreglá lo que diga |
| Git te dice "rejected" | `git pull --rebase` → resolvé si pide → `git push` |
| No sabés en qué fase estás | Abrí `CURRENT-STATUS.md` → mirá "Estado por QPU" |

---

## 📁 ARCHIVOS QUE EXISTEN PARA VOS (solo estos 5)

| Archivo | Para qué sirve | Cuándo lo abrís |
|---|---|---|
| **`CURRENT-STATUS.md`** | **Tu brújula. Estado + próximos comandos.** | **SIEMPRE PRIMERO** |
| `HUMAN-GUIDE.md` | Esta hoja. Paso a paso. | Cuando dudás "¿qué hago ahora?" |
| `AGENT-INTERACTION-MANUAL.md` | Tabla de qué pegar al agente. | Cuando vas a abrir un chat |
| `METHODOLOGY.md` (solo §3, §5, §7) | Reglas, fases, gate check. | Cuando tenés que decidir/validar |
| `README.md` | Mapa general. | Primera vez / recordatorio |

> **El resto de archivos son para el agente, no para vos.** No los leas salvo que el agente te lo pida.

---

## ✅ CHECKLIST MENTAL (antes de cerrar el día)

- [ ] `git pull` hecho
- [ ] `CURRENT-STATUS.md` leído
- [ ] Comandos de