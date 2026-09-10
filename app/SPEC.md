# SPEC — Prototipo Reto 05 · "Te llegó una multa ¿y ahora?"

> **Equipo:** LosBrujos · HACKCREA 2026
> **Narrativa:** "De multa a educación" — transformar el castigo en comprensión.
> **Constraint del reto:** El prototipo NO es asesoría legal, NO presenta impugnaciones, NO garantiza resultados.
> **Base técnica:** Vite + React + shadcn/ui + Tailwind CSS (PWA instalable, Google Sheets + Apps Script como backend demo, datos ficticios para demo, i18n, Cloudflare Pages).

---

## 1. Visión

El ciudadano recibe una boleta de tránsito y no entiende nada: ni qué hizo, ni cuánto debe, ni si puede pelear, ni cuándo se vence. **MultaClara** traduce la jerga legal a lenguaje claro, le dice si la multa es válida (semáforo de legalidad), cuánto puede ahorrar, y le genera un borrador de impugnación si aplica.

**Frase del pitch:** *"Una multa que nadie entiende no cambia el comportamiento."*

---

## 2. Principios de diseño

| Principio | Aplicación |
|---|---|
| **Lenguaje claro** | Cero jerga legal sin traducción. Cada infracción tiene explicación en palabras de la calle + glosario |
| **Accesibilidad universal** | Semáforo con iconografía (no solo color) para daltonismo; tipografía grande; contraste alto; botones "?" para info contextual |
| **Offline-first** | PWA instalable, funciona sin internet (datos en Google Sheets + cache local) |
| **Privacidad primero** | Datos en localStorage del usuario + Google Sheets solo para demo. Sin cuentas de usuario |
| **Notificación automática** | "Abrir la página = haber sido notificado" (cumple Decreto 33-2024: si buscaste tu placa, ya fuiste notificado) |
| **No es asesoría legal** | Checkbox obligatorio antes de generar PDF + disclaimer visible en cada documento |
| **Educación, no castigo** | Cada explicación incluye una cápsula de prevención (dato de seguridad vial) |

---

## 3. Arquitectura (Vite + React + shadcn + Tailwind)

```
reto-5-brujos/
├── index.html                Entry point (PWA meta tags)
├── vite.config.js            Vite + React + Tailwind + PWA plugin
├── src/
│   ├── main.jsx              Bootstrap de React
│   ├── App.jsx               Router principal (vistas)
│   ├── index.css             Tailwind imports + tema oscuro
│   ├── lib/
│   │   ├── constantes.js     Plazos legales: 15/60/120 días (Lemus)
│   │   ├── core.js           Lógica pura: fechas, prescripción, semáforo, validación
│   │   ├── data.js           Carga de infracciones.json + entidades
│   │   ├── utils.js          cn() helper para shadcn (clsx + tailwind-merge)
│   │   └── i18n/
│   │       ├── es.json       Traducciones español (Uriel)
│   │       ├── kiche.json    Traducciones K'iche' (Uriel)
│   │       └── kawchiquel.json Traducciones Kawchiquel (Uriel)
│   ├── components/
│   │   ├── ui/               Componentes shadcn (Button, Card, Select, etc.)
│   │   └── IdiomaSelector.jsx Selector ES/K'iche'/Kawchiquel + tamaño de letra
│   ├── screens/
│   │   ├── Home.jsx          P0: Landing "¿Te llegó una multa?" + QR/URL
│   │   ├── Placas.jsx        P0.5: Selección de placa guardada (localStorage)
│   │   ├── Municipios.jsx    Vista de municipalidades: gris=sin multas, color=con multas
│   │   ├── Form.jsx          P1: Formulario guiado de la boleta
│   │   ├── Explicador.jsx    P2: Traducción de la multa + botones "?"
│   │   ├── Semaforo.jsx      P3: Semáforo de legalidad
│   │   ├── QueHago.jsx       P3.5: Oposición (15 días) vs prescripción (120 días)
│   │   └── Accion.jsx        P4: PDF / pago con descuento
│   └── hooks/
│       └── useLocalStorage.js Hook para persistencia local (placas guardadas)
├── public/
│   ├── infracciones.json     Catálogo de 15+ infracciones
│   ├── entidades.json        11 entidades emisoras
│   ├── favicon.svg           Icono de la app
│   ├── icon-192.png          PWA icon 192x192
│   ├── icon-512.png          PWA icon 512x512
│   └── robots.txt
├── data/
│   └── plantilla-impugnacion.md Plantilla de impugnación
├── _headers                  Cache control para Cloudflare
└── package.json              Dependencias
```

**Componentes shadcn que usamos:**
- `Button` → CTAs de la landing, "Explicar mi multa", "Generar PDF"
- `Card` → Tarjetas del explicador, semáforo, entidades
- `Select` → Selector de entidad, tipo de infracción
- `Input` → Fecha de infracción, fecha de notificación, número de boleta
- `Checkbox` → "¿Vos manejabas?" + disclaimer obligatorio
- `Badge` → Estados del semáforo, prioridades
- `Dialog` → Confirmación antes de generar PDF
- `Tabs` → Las 3 opciones de la landing
- `Progress` → Barras de plazos (días restantes)

**Decisiones:**
- React + shadcn → componentes reutilizables, diseño profesional, responsive con Tailwind.
- Tailwind CSS → dark mode + contraste alto + responsive en minutos.
- Datos en JSON local → cero backend, cero riesgo en demo.
- PWA con vite-plugin-pwa → service worker automático, offline-first.
- Deploy: Cloudflare Pages (gratis, CDN, HTTPS).

---

## 4. Flujo de usuario (macro)

```
ENTRADA (QR o URL)
  │  Escanea QR desde la boleta, o ingresa la URL directamente
  │  (PWA accesible desde navegador — no es app descargable)
  │
  ▼
SELECCIÓN DE PLACA
  │  El usuario selecciona una placa de su lista (guardada en localStorage)
  │  O ingresa una placa nueva
  │  (Múltiples placas: para gente con varios vehículos)
  │
  ▼
VISTA DE MUNICIPALIDADES
  │  Mapa/lista de todas las municipalidades registradas
  │  En gris donde NO hay multas → en color donde SÍ hay multas
  │  Números en la esquina superior derecha como indicador visual
  │
  ▼
EXPLICADOR
  │  Traduce lenguaje jurídico a lenguaje claro
  │  Muestra: qué hiciste, cuánto cuesta, cómo ahorrar
  │  Botones "?" cerca de cada texto para info adicional
  │
  ▼
SEMÁFORO DE LEGALIDAD
  │  Verde: pagar con descuento
  │  Amarillo: revisar datos, pedir notificación formal
  │  Rojo: impugnar
  │
  ▼
¿QUÉ HAGO?
  │  Opción A: Desacuerdo por Oposición (form digital)
  │  Opción B: Desacuerdo por Prescripción (>120 días)
  │
  ▼
APELACIÓN
  • Formulario digital listo para descargar e imprimir
  • O llevar a mano a ventanilla (escaneado y subido digital)
```

---

## 5. Pantallas detalladas

### P0 — ENTRADA (QR / URL)

**Propósito:** Acceso rápido desde la boleta de tránsito. Sin app, sin cuenta, sin fricción.

**Elementos:**
- Pantalla de bienvenida con logo + título "MultaClara"
- **Campo de entrada:** número de placa (texto, formato GUATEMALA: 3 letras + 3-4 números)
- **Botón:** "Buscar multas" → busca en localStorage/Google Sheets
- **Si no hay multas registradas:** "No encontramos multas para esta placa. Podés registrar los datos de tu boleta para analizarla"
- **Si hay multas:** Lista de multas encontradas con estado (verde/amarillo/rojo)
- **Footer:** disclaimer "Herramienta educativa. No es asesoría legal"

**Idioma:** Selector visible al tope (Español / K'iche' / Kawchiquel)
**Accesibilidad:** Control de tamaño de letra (A+, A-) + botones "?" para info contextual

---

### P0.5 — SELECCIÓN DE PLACA (nuevo)

**Propósito:** La gente tiene varios vehículos. Guardar placas facilita la búsqueda.

**Elementos:**
- **Lista de placas guardadas** (localStorage)
- **Botón:** "Agregar placa nueva"
- **Cada placa muestra:** número de placa + cantidad de multas pendientes
- **Si hay muchas placas:** scroll vertical, búsqueda por texto

**Reglas:**
- Las placas se guardan automáticamente al buscar
- El usuario puede eliminar placas de la lista
- No hay cuenta de usuario — todo en el dispositivo

---

### P0.6 — VISTA DE MUNICIPALIDADES (nuevo)

**Propósito:** Ver todas las municipalidades registradas y dónde hay multas.

**Elementos:**
- **Lista/grid de municipalidades** (todas las que aparecen en el catálogo)
- **Cada municipalidad muestra:**
  - En gris: sin multas registradas
  - En color: con multas pendientes
  - **Número en la esquina superior derecha:** cantidad de multas pendientes
- **Click en una municipalidad:** filtra las multas de esa entidad

**Reglas:**
- Las municipalidades vienen de `entidades.json`
- El usuario puede buscar por nombre
- Solo muestra municipalidades relevantes para las placas guardadas

---

### P1 — DATOS DE LA BOLETA

**Propósito:** Capturar los datos mínimos para explicar la multa. **La fecha es crítica**: la calculadora de prescripción (120 días) y el semáforo dependen de ella.

**Flujo:** El usuario ya ingresó su placa en P0. Ahora ingresa los datos de la multa específica.

**Elementos (formulario guiado):**
1. **Número de placa** (ya capturado, editable)
2. **¿Sos el propietario del vehículo?** (sí/no) — **PREGUNTA CLAVE**:
   - **Sí** → continúa normal
   - **No** → "¿Quién manejaba?" (si es familiar/conocido, coordinar con él. Si no sabés quién manejaba, la notificación debe cumplir el Decreto 33-2024)
3. **Tipo de papel** (selector de 3 chips): Boleta de tránsito / Requerimiento de pago / Citación
4. **Número de boleta/remisión** (texto, opcional — para el PDF)
5. **Entidad que multó** (select con las 11 entidades: PNC, EMETRA, EMIXTRA, PMT Villa Nueva, Mixco, Escuintla, Antigua, Jutiapa, Palencia, S.C. Pinula, S. Lucas, Amatitlán)
6. **Fecha de la infracción** (input date) — **validación estricta**:
   - Requerida. Si falta → error "Necesitamos la fecha para calcular tus plazos"
   - No puede ser futura → error "La fecha no puede ser en el futuro"
   - No puede ser anterior a 5 años → error "Esta multa es muy antigua; verificá si ya prescribió"
7. **Fecha de notificación** (input date, opcional pero recomendada):
   - Si se ingresa y es posterior a infracción + 120 días → **alerta inmediata**: "⚠️ Esta multa podría haber prescrito"
   - Si no se ingresa → el semáforo usa el escenario "sin notificación confirmada"
8. **¿Qué infracción te cobran?** (select del catálogo de 15, con búsqueda) — o "No sé / no dice" → muestra el catálogo completo para explorar
9. **¿Vos manejabas el vehículo?** (sí/no) — **ya cubierto arriba**

**Botón:** "Explicar mi multa" (deshabilitado hasta que fecha + infracción estén válidas)

**Validaciones (resumen):**

| Campo | Regla | Error |
|---|---|---|
| Placa | requerida, formato válido | "Ingresá el número de placa" |
| ¿Sos el propietario? | requerida (sí/no) | "Necesitamos saber si sos el dueño del carro" |
| Fecha infracción | requerida, ≤ hoy, ≥ hoy-5años | mensajes específicos |
| Fecha notificación | opcional, ≥ fecha infracción | "La notificación no puede ser antes de la infracción" |
| Infracción | requerida (select o búsqueda) | "Elegí qué infracción te cobran" |
| ¿Vos manejabas? | requerida (sí/no) | "Necesitamos saber si eras vos quien manejaba" |

**Reglas de negocio (localización):**
- "Abrir la página = haber sido notificado" (Decreto 33-2024: si buscaste tu placa, ya fuiste notificado)
- "No cuenta como notificado hasta que busque su placa o te paren"
- "El año pasado salió una ley que dice que no te pueden cobrar hasta que te notifiquen"
- "Si te paran, sí cuenta como notificado"
- "Depende del poli si te explica cómo y cuándo pagar"
- "La multa no tiene suficiente información, no es clara, ni hay educación para entender la multa"

**Backend (demo):**
- Google Sheets como base de datos (datos ficticios para demo)
- Apps Script como API intermedia
- Datos de ejemplo: municipalidades, multas ficticias, estados

---

### P2 — EXPLICADOR

**Propósito:** Traducir la multa. El corazón del reto: lenguaje claro + monto + prevención.

**Elementos (tarjeta principal):**
- **Header:** "Te multaron por: [nombre en lenguaje claro]" + código normativo en pequeño (ej: "Art. 90 Reglamento 273-98")
- **"¿Qué hiciste?"** — descripción en palabras de la calle (del catálogo)
- **"¿Cuánto te cuesta?"** — tabla de montos:
  - Monto original: Q400
  - Con curso vial (25%): Q300
  - Con condonación activa (si la entidad tiene, ej: MuniGuate 50-60%): Q200 / Q160
  - **Ahorro total** destacado: "Podés ahorrar hasta Q240"
- **"¿Por qué es peligroso?"** — cápsula de prevención con dato real (ej: "El exceso de velocidad causa ~40% de los accidentes en Guatemala")
- **Botones "?" contextuales:** cerca de cada texto (montos, plazos, términos legales) → abren tooltip/popover con explicación extra
- **Glosario expandible:** "¿Qué significa 'Art. 90'?" → explica el código en lenguaje claro
- **Botón:** "Revisar si mi multa es válida" → P3

**Reglas:**
- Los montos salen de `infracciones.json` (nunca inventar; los `null` se muestran como "verificar en la boleta")
- El descuento de curso vial es 25% según cartilla; si la entidad tiene condonación activa (MuniGuate 50-60%), se muestra aparte con su fuente/fecha
- Si la infracción tiene `verificar: true` (monto null) → se muestra "Monto no confirmado — verificá en tu boleta" en vez de inventar

---

### P3 — SEMÁFORO DE LEGALIDAD

**Propósito:** Decirle al ciudadano si su multa es defendible. **Con iconografía obligatoria** (no solo color) para daltonismo.

**Lógica del semáforo (core.js, función pura):**

| Estado | Condición | Icono | Color | Acción sugerida |
|---|---|---|---|---|
| 🟢 **Verde** | Notificación OK (fecha ≤ 120 días) + datos coherentes + entidad con competencia | ✓ cheque | verde | Pagar con descuento o curso vial |
| 🟡 **Amarillo** | Sin fecha de notificación confirmada, o datos incompletos, o entidad dudosa | ⚠️ triángulo | amarillo | Revisar la boleta / pedir notificación formal |
| 🔴 **Rojo** | Prescripción (>120 días sin notificar) O vicio grave (sin pruebas, sin base legal, sin competencia territorial) | ⛔ octágono | rojo | Impugnar (generar borrador) |

**Elementos:**
- **Tarjeta de semáforo gigante** con icono + color + título ("Tu multa parece válida" / "Revisá estos puntos" / "Tu multa podría ser impugnable")
- **Checklist de legalidad** (basado en Decreto 33-2024, Art. 2 — los 6 requisitos de la notificación):
  1. ✅/❌ Fecha, hora y lugar exacto
  2. ✅/❌ Datos del vehículo correctos
  3. ✅/❌ Pruebas de respaldo (fotos/videos)
  4. ✅/❌ Base normativa citada + monto
  5. ✅/❌ Procedimiento de impugnación indicado
  6. ✅/❌ Notificada dentro de 120 días
- **Plazos visuales** (cuenta regresiva):
  - "Días para impugnar: 12 de 15" (barra de progreso)
  - "Días desde la infracción: 45 de 120" (barra de prescripción)
- **Detección de competencia territorial:** si la entidad es municipal y el lugar parece ruta nacional/centroamericana → alerta amarilla "Las municipalidades no pueden multar en rutas nacionales sin convenio con la PNC"
- **Botón contextual:** "Generar borrador de impugnación" (si rojo/amarillo) o "Ver cómo pagar con descuento" (si verde)

**Reglas de negocio (del Decreto 33-2024):**
- Plazo para impugnar: **15 días** desde la notificación
- Prescripción: **120 días** sin notificar
- Pago tras resolución desfavorable: **60 días**
- Sin notificación → no se puede exigir pago
- Prohibido retener licencia (salvo 5 multas notificadas sin pagar)

---

### P3.5 — ¿QUÉ HAGO? (nuevo)

**Propósito:** La gente no sabe que se puede apelar. Esta pantalla clarifica las opciones y muestra que SÍ hay alternativas.

**Elementos:**
- **Título:** "¿Qué podés hacer?"
- **Tarjeta 1: Pagar con descuento** (si semáforo verde)
  - Monto con descuento aplicado
  - Pasos para obtener descuento
  - Botón "Ver opciones de pago"
- **Tarjeta 2: Desacuerdo por Oposición** (si semáforo amarillo/rojo)
  - "No estoy de acuerdo con la multa"
  - Qué es: impugnar dentro de 15 días de notificación
  - Requisitos: boleta, datos, pruebas
  - Botón "Generar formulario de oposición"
- **Tarjeta 3: Desacuerdo por Prescripción** (si aplica)
  - "Ya pasaron 120 días y no me notificaron"
  - Qué es: la multa prescribe si no te notificaron en 120 días
  - Escenario: "pasaron 120 días, no sé nada sobre mi multa vencida"
  - Botón "Generar solicitud de prescripción"
- **Tarjeta 4: Información general** (siempre visible)
  - "¿Qué es una notificación?"
  - "¿Qué pasa si me paran?"
  - "¿Necesito un abogado?"
  - Link a guía de estafas

**Reglas de negocio:**
- "No cuenta como notificado hasta que busque su placa o te paren"
- "Si te paran, sí cuenta como notificado"
- "El año pasado salió una ley que dice que no te pueden cobrar hasta que te notifiquen"
- "Depende del poli si te explica cómo y cuándo pagar"

---

### P4 — APELACIÓN

**Propósito:** Generar el documento correcto según el camino elegido.

#### Camino A: Oposición (form digital)

**Elementos:**
1. **Checkbox obligatorio** (bloquea el botón hasta marcarlo):
   > ☐ "Entiendo que este es un borrador automatizado, no sustituye la asesoría de un abogado, y que presentarlo no garantiza resultados."
2. Formulario de datos personales (nombre, CUI, domicilio, teléfono, correo) — **todo opcional excepto nombre**, se puede generar el PDF con datos parciales
3. **Botón:** "Generar borrador (PDF)" → usa `plantilla-impugnacion.md` con los condicionales activados según los vicios detectados en P3
4. Vista previa del documento en pantalla (texto) antes de descargar
5. Descarga PDF (jsPDF) + botón "Copiar texto" (para quienes no pueden abrir PDF)
6. **Guía de presentación:** "¿Dónde lo llevás?" — entidad, dirección, horario, documentos a acompañar (del catálogo de entidades)
7. Disclaimer permanente: "Este borrador es orientativo. Consulte con un abogado para asesoría legal específica."

#### Camino B: Prescripción (>120 días sin notificar)

**Elementos:**
1. **Checkbox obligatorio:** similar a oposición
2. Formulario简化: solo nombre + placa + fecha de infracción
3. **Genera solicitud de prescripción** (PDF)
4. **Guía:** "Pasaron 120 días, no sé nada de mi multa. Qué hacer cuando ya se vencieron"
5. Opción de llevar con abogado o presentar directamente

#### Camino C: Pagar con descuento (verde)

**Elementos:**
1. "Cuánto pagarías hoy" — monto con descuento aplicado + ahorro
2. "Cómo obtener el descuento" — pasos (curso vial, condonación activa con fecha límite)
3. "Dónde pagar" — banco/portal según entidad (del catálogo)
4. "Después de pagar" — guardar recibo, verificar solvencia en 48h
5. Cápsula de prevención final: "La multa ya está, pero la próxima se evita así…"

---

## 6. Capas y fases del MVP ambicioso

André pidió un alcance **ambicioso por capas y fases**. Así lo estructuro:

### CAPA 1 — Core (imprescindible, mañana jueves)
| # | Funcionalidad | Prioridad |
|---|---|---|
| 1 | P0 Entrada: búsqueda por placa + selector de idioma (ES/K'iche'/Kawchiquel) + tamaño de letra | P0 |
| 2 | P0.5 Selección de placa (múltiples placas en localStorage) | P0 |
| 3 | P0.6 Vista de municipalidades (gris=sin multas, color=con multas, números) | P0 |
| 4 | P1 Formulario de datos de boleta (con placa pre-capturada) — **preguntar si es el propietario** | P0 |
| 5 | P2 Explicador (lenguaje claro + montos + descuentos + prevención + botones "?" info) | P0 |
| 6 | P3 Semáforo de legalidad (iconografía + checklist + plazos) — **la gente no sabe que puede apelar** | P0 |
| 7 | P3.5 ¿Qué hago? — 3 opciones: pagar / oposición / prescripción | P0 |
| 8 | P4 Generador de borrador PDF (oposición o prescripción) con checkbox obligatorio | P0 |
| 9 | Catálogo de 15 infracciones con copy claro | P0 |
| 10 | PWA instalable + offline + QR como entrada | P0 |
| 11 | Datos demo: Google Sheets + Apps Script (datos ficticios para demo) | P0 |

### CAPA 2 — Valor (si sobra tiempo mañana / viernes temprano)
| # | Funcionalidad | Prioridad |
|---|---|---|
| 12 | Guía de estafas (notificación real vs SMS falso) | P1 |
| 13 | Escenario 120 días vencidos: qué hacer cuando ya prescribió | P1 |

### CAPA 3 — Escala (post-hackathon / slides de pitch)
| # | Funcionalidad | Prioridad |
|---|---|---|
| 14 | OCR de boletas (Tesseract.js) con 3-4 boletas mock PNG | P2 |
| 15 | Dashboard de flota B2B (empresas de transporte) | P2 (pitch) |
| 15 | API real (convenio SAT/EMETRA) | P2 (roadmap) |
| 16 | Expansión regional (El Salvador, Honduras) | P2 (roadmap) |

**Regla de oro:** Capa 1 completa y pulida > Capa 1 + Capa 2 a medias. El jurado premia lo que funciona, no lo que promete.

---

## 7. Mejoras visuales (respecto a portales existentes)

| Aspecto | Portales actuales (SAT, EMETRA) | MultaClara |
|---|---|---|
| Lenguaje | Jerga legal ("Art. 90") | Lenguaje claro + glosario |
| Montos | Solo el monto | Monto + descuentos + ahorro visible |
| Plazos | No se muestran | Barras de progreso visuales |
| Legalidad | No se evalúa | Semáforo con iconografía |
| Accesibilidad | Solo español, desktop | Multilingüe (ES + K'iche' + Kawchiquel) + tamaño de letra + botones "?" + móvil-first |
| Confianza | "null" en nombres, bugs | Copy cuidado, disclaimers claros |
| Estética | Portales anticuados | Tema oscuro con Tailwind + shadcn (profesional, responsive) |
| Offline | Requiere internet | PWA offline-first |

**Detalles visuales:**
- Tema oscuro con acento verde/ámbar/rojo según semáforo (Tailwind: `emerald`, `amber`, `red`)
- Tipografía legible (Inter para UI, JetBrains Mono para código normativo)
- Tarjetas con glass blur (Tailwind: `backdrop-blur` + `bg-white/5`)
- Iconografía con Lucide React (shadcn default)
- Animaciones sutiles de entrada (fade/slide, CSS transitions de Tailwind)

---

## 8. Nombres sugeridos para el manifest

| Nombre | Por qué | Veredicto |
|---|---|---|
| **MultaClara** | Corto, describe la función, funciona en español neutro | ⭐ Recomendado |
| MultaEduca | Fusiona "multa" + "educa" (la narrativa) | Bueno |
| TuMultaGT | Local, identifica Guatemala | Bueno |
| EntendéTuMulta | Voseo guatemalteco, muy cercano | Largo para icono |
| MultaFácil | Simple, directo | Genérico |

**Recomendación:** `MultaClara` para el manifest (corto, se lee bajo el icono) + subtítulo en el pitch: *"MultaClara — de multa a educación"*.

---

## 9. Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Montos no verificados (null en catálogo) | Mostrar "verificar en boleta", nunca inventar |
| Fecha mal ingresada → semáforo erróneo | Validación estricta en P1 + doble confirmación |
| Demo sin internet | PWA offline-first, datos locales |
| Jurado duda del constraint legal | Checkbox obligatorio + disclaimers visibles |
| OCR falla en demo | Camino manual gigante como opción principal |
| Feature creep | Capas: Capa 1 completa primero, siempre |