# ARQUITECTURA DE DATOS Y CONVENCIONES — LosBrujos

> **Lee esto ANTES de escribir código.** Define cómo se llaman las variables, dónde vive cada dato y cómo trabajamos con Git.
> Si tenés una duda, preguntale a André por el grupo.

---

## 1. PRINCIPIO RECTOR: UNA SOLA FUENTE DE VERDAD

**Cada dato vive en UN solo lugar.** Si cambia algo, se cambia en ESE lugar y se propaga a toda la app.

- ❌ **NUNCA** copies datos entre archivos
- ❌ **NUNCA** pongas un valor fijo (hardcodeado) dentro de un componente
- ✅ Si necesitás un dato, lo **importás** desde su archivo fuente

**Ejemplo:** el plazo para impugnar (15 días) vive en `constantes.js`. Si mañana la ley cambia a 20 días, se cambia en UN archivo y toda la app lo usa actualizado. Nada más.

---

## 2. DÓNDE VIVE CADA DATO (fuentes de verdad)

| Dato | Archivo fuente | Quién lo mantiene |
|---|---|---|
| Catálogo de infracciones (15+) | `public/infracciones.json` | Diego (copy) + Lemus (estructura) |
| Entidades emisoras (11) | `public/entidades.json` | Diego |
| Plazos legales (15 / 60 / 120 días) | `src/lib/constantes.js` | Lemus |
| Traducciones (ES / K'iche' / Kawchiquel) | `src/lib/i18n/*.json` | Uriel |
| Plantilla de impugnación | `data/plantilla-impugnacion.md` | Lemus |
| Multas demo (por placa) | Google Sheets + Apps Script | Equipo |

**Regla:** si un dato no está en su fuente, NO lo inventes. Preguntá al dueño del dato.

---

## 2.5 ESTRUCTURA DE CARPETAS Y ARCHIVOS (qué creamos)

Esta es la estructura completa del repo de código `reto-5-brujos`. **Cada archivo de la lista hay que crearlo** (los que ya existen, mantenerlos así).

```
reto-5-brujos/
├── index.html                    Entry point (PWA meta tags)
├── vite.config.js                Vite + React + Tailwind + PWA plugin
├── package.json                  Dependencias
├── _headers                      Cache control para Cloudflare
├── public/
│   ├── infracciones.json         Catálogo de 15+ infracciones (Diego/Lemus)
│   ├── entidades.json            11 entidades emisoras (Diego)
│   ├── favicon.svg               Icono de la app
│   ├── icon-192.png              PWA icon 192x192
│   ├── icon-512.png              PWA icon 512x512
│   └── robots.txt
├── data/
│   └── plantilla-impugnacion.md  Plantilla de impugnación (Lemus)
└── src/
    ├── main.jsx                  Bootstrap de React
    ├── App.jsx                   Router principal (vistas)
    ├── index.css                 Tailwind imports + tema oscuro
    ├── lib/
    │   ├── constantes.js         Plazos legales: 15/60/120 días (Lemus)
    │   ├── core.js               Lógica pura: fechas, prescripción, semáforo, validación (Lemus)
    │   ├── data.js               Carga de infracciones.json + entidades (Lemus)
    │   ├── utils.js              cn() helper para shadcn (clsx + tailwind-merge)
    │   └── i18n/
    │       ├── es.json           Traducciones español (Uriel)
    │       ├── kiche.json        Traducciones K'iche' (Uriel)
    │       └── kawchiquel.json   Traducciones Kawchiquel (Uriel)
    ├── hooks/
    │   └── useLocalStorage.js    Persistencia local (placas guardadas)
    ├── components/
    │   ├── ui/                   Componentes shadcn (Button, Card, Select, Tabs, Tooltip…)
    │   └── IdiomaSelector.jsx    Selector ES/K'iche'/Kawchiquel + tamaño de letra (Kevin)
    └── screens/
        ├── Home.jsx              P0: Landing "¿Te llegó una multa?" + QR/URL (Kevin)
        ├── Placas.jsx            P0.5: Selección de placa guardada (localStorage) (Kevin)
        ├── Municipios.jsx        Vista de municipalidades: gris=sin multas, color=con multas (Kevin)
        ├── Form.jsx              P1: Formulario guiado de la boleta (Kevin)
        ├── Explicador.jsx        P2: Traducción de la multa + botones "?" (Kevin)
        ├── Semaforo.jsx          P3: Semáforo de legalidad (verde/amarillo/rojo) (Kevin)
        ├── QueHago.jsx           P3.5: Oposición (15 días) vs prescripción (120 días) (Kevin)
        └── Accion.jsx            P4: PDF / pago con descuento (Kevin)
```

**Notas:**
- `components/ui/` se genera con el CLI de shadcn (`npx shadcn@latest init` + `add`). No lo crees a mano.
- `screens/` = una pantalla por archivo, en PascalCase.
- `lib/` = lógica pura, sin JSX. `core.js` y `constantes.js` no importan React.
- `i18n/` = solo JSON, las mismas claves en los 3 archivos.
- El backend demo (Google Sheets + Apps Script) vive FUERA de este repo — es un script aparte que Lemus conecta.

---

## 3. REGLAS DE NOMBRES (camelCase y amigos)

| Qué es | Convención | Ejemplo |
|---|---|---|
| Variables y funciones | **camelCase** | `fechaInfraccion`, `calcularPrescripcion()` |
| Constantes (valores fijos) | **UPPER_SNAKE_CASE** | `PLAZO_IMPUTACION_DIAS = 15` |
| Componentes React | **PascalCase** | `SemaforoLegalidad.jsx` |
| Archivos JSON | **kebab-case** | `infracciones.json`, `entidades.json` |
| IDs de datos | **kebab-case** | `"semaforo-rojo"` |
| Claves de i18n | **camelCase** | `"botonExplicarMulta"` |

**Prohibido:**
- ❌ `snake_case` para variables (`fecha_infraccion`)
- ❌ `PascalCase` para variables (`FechaInfraccion`)
- ❌ Nombres con tildes, espacios o caracteres raros (`fecha infracción`)
- ❌ Nombres en mayúsculas para variables normales (`FECHA`)

---

## 4. FORMATOS DE DATOS

### 4.1 `infracciones.json` — catálogo de infracciones

```json
{
  "id": "semaforo-rojo",
  "codigo": "Art. 90",
  "nombre": "No respetar semáforo en rojo",
  "monto": 300,
  "descuentoCursoVial": 0.5,
  "categoria": "semaforos",
  "emisores": ["EMETRA", "PNC"],
  "consejoPrevencion": "Detente siempre en rojo..."
}
```

- `monto` es número (no string). Si no se sabe: `null` → la UI muestra "verificar en boleta"
- `descuentoCursoVial` es decimal (0.5 = 50%)
- `emisores` es un array de IDs de entidades (referencia a `entidades.json`)

### 4.2 `entidades.json` — entidades emisoras

```json
{
  "id": "emetra",
  "nombre": "EMETRA",
  "jurisdiccion": "Ciudad de Guatemala",
  "portal": "https://www.muniguate.com/emetra",
  "contacto": "1234-5678",
  "dondePagar": "Banrural"
}
```

### 4.3 `src/lib/i18n/*.json` — traducciones (Uriel)

Tres archivos con **las mismas claves**:

```
src/lib/i18n/
├── es.json          → { "botonExplicarMulta": "Explicar mi multa" }
├── kiche.json       → { "botonExplicarMulta": "..." }
└── kawchiquel.json  → { "botonExplicarMulta": "..." }
```

- La UI **nunca** tiene texto directo: siempre lee de i18n
- Si agregás una clave en `es.json`, agregala en los otros dos (aunque sea placeholder)

### 4.4 `src/lib/constantes.js` — plazos legales (Lemus)

```js
export const PLAZO_IMPUTACION_DIAS = 15;
export const PLAZO_PAGO_DIAS = 60;
export const PLAZO_PRESCRIPCION_DIAS = 120;
```

---

## 5. REGLA DE ORO: SI CAMBIA ALGO, SE CAMBIA EN TODO

1. **Plazos legales** → viven en `constantes.js`. La calculadora, el semáforo y el PDF los importan de ahí.
2. **Montos** → viven en `infracciones.json`. Ningún componente pone montos a mano.
3. **Textos** → viven en i18n. Ningún componente tiene texto hardcodeado.
4. **Entidades** → viven en `entidades.json`. La vista de municipalidades los lee de ahí.

**Test mental:** si mañana cambia el monto de una multa, ¿cuántos archivos toco? Si la respuesta es más de 1, está mal hecho.

---

## 6. FLUJO DE TRABAJO CON GIT (IMPORTANTE — leelo siempre)

### 6.1 Clonar el repo (una sola vez)

```
git clone https://github.com/ithackathonnacionalgt/reto-5-brujos.git
cd reto-5-brujos
```

### 6.2 Crear TU rama (una sola vez)

```
git checkout -b kevin-frontend        ← usá tu nombre-rol
```

| Quién | Rama |
|---|---|
| Kevin | `kevin-frontend` |
| Lemus | `lemus-logica` |
| Diego | `diego-investigador` |
| Uriel | `uriel-disenador` |

### 6.3 Trabajar y guardar (todo el día)

```
git status                            ← ver qué cambió
git add .                             ← preparar cambios
git commit -m "feat: hice tal cosa"   ← guardar con mensaje claro
git push origin kevin-frontend        ← subir TU rama a GitHub
```

### 6.4 Reglas de oro de Git

- ✅ **Cada uno trabaja SOLO en su rama**
- ✅ **Cada uno hace push SOLO a su rama** (`git push origin tu-rama`)
- ❌ **NUNCA push directo a `main`** — main solo lo toca André (merge)
- ✅ Al empezar el día: `git pull origin main` para traer lo último
- ✅ Mensajes de commit claros: `feat:`, `fix:`, `refactor:`, `docs:`

### 6.5 Ejemplo de mensajes de commit

```
feat: pantalla de municipalidades con números por entidad
fix: selector de idioma no guardaba la preferencia
refactor: mover plazos legales a constantes.js
docs: actualizar arquitectura de datos
```

---

## 7. CHECKLIST ANTES DE AVISAR QUE TERMINASTE

- [ ] Trabajé en MI rama (no en main)
- [ ] No hardcodeé valores que ya existen en JSON/constantes
- [ ] Mis variables usan camelCase
- [ ] Mis componentes usan PascalCase
- [ ] No dejé texto suelto en la UI (todo pasa por i18n)
- [ ] Hice commit con mensaje claro
- [ ] Hice push a MI rama
- [ ] Avisé por ntfy que terminé