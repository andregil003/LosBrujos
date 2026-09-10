# SPEC — Prototipo Reto 05 · "Te llegó una multa ¿y ahora?"

> **Equipo:** LosBrujos · HACKCREA 2026
> **Narrativa:** "De multa a educación" — transformar el castigo en comprensión.
> **Constraint del reto:** El prototipo NO es asesoría legal, NO presenta impugnaciones, NO garantiza resultados.
> **Base técnica:** Arquitectura reutilizada de QUIZZO (PWA estática, vanilla JS, sin backend, localStorage, i18n, Cloudflare Pages).

---

## 1. Visión

El ciudadano recibe una boleta de tránsito y no entiende nada: ni qué hizo, ni cuánto debe, ni si puede pelear, ni cuándo se vence. **MultaClara** traduce la jerga legal a lenguaje claro, le dice si la multa es válida (semáforo de legalidad), cuánto puede ahorrar, y le genera un borrador de impugnación si aplica.

**Frase del pitch:** *"Una multa que nadie entiende no cambia el comportamiento."*

---

## 2. Principios de diseño

| Principio | Aplicación |
|---|---|
| **Lenguaje claro** | Cero jerga legal sin traducción. Cada infracción tiene explicación en palabras de la calle + glosario |
| **Accesibilidad universal** | Semáforo con iconografía (no solo color) para daltonismo; tipografía grande; contraste alto; audio (V2) |
| **Offline-first** | PWA instalable, todo funciona sin internet (los datos viajan en JSON local) |
| **Privacidad primero** | Todo queda en el dispositivo (localStorage). Cumple Ley 89-2005. Sin cuentas |
| **No es asesoría legal** | Checkbox obligatorio antes de generar PDF + disclaimer visible en cada documento |
| **Educación, no castigo** | Cada explicación incluye una cápsula de prevención (dato de seguridad vial) |

---

## 3. Arquitectura (reutilizada de QUIZZO)

```
app/
├── index.html          SPA con vistas (patrón #view-* de QUIZZO)
├── sw.js               Service worker: precache offline + banner de updates
├── manifest.json       PWA instalable
├── css/styles.css      Tema oscuro estilo Linear/Notion (tokens de QUIZZO adaptados)
├── js/
│   ├── core.js         Lógica pura y testeable: fechas, prescripción, semáforo, validación
│   ├── i18n.js         Diccionarios: es + skeleton k'iche'/q'eqchi'/garífuna
│   ├── data.js         Carga de infracciones.json + catálogo de entidades
│   └── script.js       UI: navegación entre vistas + render
├── data/
│   ├── infracciones.json   Catálogo de 15 infracciones (ya existe, se expande)
│   └── entidades.json      Entidades emisoras + jurisdicción + contacto
├── icons/              Favicon + PNGs PWA
├── _headers            Cache control para SW/version
└── wrangler.jsonc      Deploy Cloudflare Pages
```

**Decisiones:**
- Vanilla JS, cero frameworks, cero build step (patrón QUIZZO) → demo estable, sin riesgo de build roto.
- Datos en JSON local → cero backend, cero riesgo en demo.
- Deploy: Cloudflare Pages (gratis, CDN, HTTPS).

---

## 4. Flujo de usuario (macro)

```
INICIO
  │
  ├─ Opción A: "Tengo la boleta en la mano" → ingreso manual (número, fecha, entidad)
  ├─ Opción B: "Escanear boleta" (V2, OCR) → foto → autocompleta
  └─ Opción C: "Solo quiero entender una infracción" → buscar por nombre (ej: "semáforo")
  │
  ▼
EXPLICADOR
  • Qué hiciste (lenguaje claro) + código normativo traducido
  • Cuánto cuesta + cuánto pagarías con descuento/curso vial
  • Cápsula de prevención (dato de seguridad vial)
  │
  ▼
SEMÁFORO DE LEGALIDAD
  • Verde: notificación OK, dentro de plazos → guía de pago/descuento
  • Amarillo: algo no cuadra (datos, pruebas, competencia) → sugiere revisar
  • Rojo: vicio grave o prescripción → sugiere impugnar
  │
  ▼
ACCIÓN
  • Generar borrador de impugnación (PDF) — checkbox obligatorio
  • Guía de pago con descuento (cuánto ahorras, dónde, cómo)
  • Recursos: dónde ir, documentos, cursos viales
```

---

## 5. Pantallas detalladas

### P0 — INICIO

**Propósito:** El usuario llega con una boleta (o sin ella) y elige cómo empezar. Cero fricción.

**Elementos:**
- Hero: "¿Te llegó una multa? Te la explicamos" + subtítulo "Entendé qué te cobran, si es válida y qué podés hacer"
- **Botón gigante #1:** "Ingresar datos de mi boleta" (manual) — el camino principal
- **Botón #2:** "Escanear mi boleta" (V2, OCR) — marcado como "pronto" si no está en el build
- **Botón #3 (texto):** "Solo quiero entender una infracción" → buscador del catálogo
- Footer: disclaimer "Herramienta educativa. No es asesoría legal ni representa al gobierno"

**Estados:**
- Primera visita: sin historial → los 3 botones
- Con historial (V2): "Tus últimas consultas" (recientes desde localStorage)

**Validaciones:** ninguna (es navegación).

---

### P1 — DATOS DE LA BOLETA

**Propósito:** Capturar los datos mínimos para explicar la multa. **La fecha es crítica**: la calculadora de prescripción (120 días) y el semáforo dependen de ella.

**Elementos (formulario guiado):**
1. **Tipo de papel** (selector de 3 chips): Boleta de tránsito / Requerimiento de pago / Citación
2. **Número de boleta/remisión** (texto, opcional — para el PDF)
3. **Entidad que multó** (select con las 11 entidades: PNC, EMETRA, EMIXTRA, PMT Villa Nueva, Mixco, Escuintla, Antigua, Jutiapa, Palencia, S.C. Pinula, S. Lucas, Amatitlán)
4. **Fecha de la infracción** (input date) — **validación estricta**:
   - Requerida. Si falta → error "Necesitamos la fecha para calcular tus plazos"
   - No puede ser futura → error "La fecha no puede ser en el futuro"
   - No puede ser anterior a 5 años → error "Esta multa es muy antigua; verificá si ya prescribió"
5. **Fecha de notificación** (input date, opcional pero recomendada):
   - Si se ingresa y es posterior a infracción + 120 días → **alerta inmediata**: "⚠️ Esta multa podría haber prescrito"
   - Si no se ingresa → el semáforo usa el escenario "sin notificación confirmada"
6. **¿Qué infracción te cobran?** (select del catálogo de 15, con búsqueda) — o "No sé / no dice" → muestra el catálogo completo para explorar

**Botón:** "Explicar mi multa" (deshabilitado hasta que fecha + infracción estén válidas)

**Validaciones (resumen):**

| Campo | Regla | Error |
|---|---|---|
| Fecha infracción | requerida, ≤ hoy, ≥ hoy-5años | mensajes específicos |
| Fecha notificación | opcional, ≥ fecha infracción | "La notificación no puede ser antes de la infracción" |
| Infracción | requerida (select o búsqueda) | "Elegí qué infracción te cobran" |

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

### P4 — ACCIÓN

**Propósito:** Convertir la información en acción. Dos caminos según el semáforo.

#### Camino A: Impugnar (rojo/amarillo)

**Elementos:**
1. **Checkbox obligatorio** (bloquea el botón hasta marcarlo):
   > ☐ "Entiendo que este es un borrador automatizado, no sustituye la asesoría de un abogado, y que presentarlo no garantiza resultados."
2. Formulario de datos personales (nombre, CUI, domicilio, teléfono, correo) — **todo opcional excepto nombre**, se puede generar el PDF con datos parciales
3. **Botón:** "Generar borrador (PDF)" → usa `plantilla-impugnacion.md` con los condicionales activados según los vicios detectados en P3
4. Vista previa del documento en pantalla (texto) antes de descargar
5. Descarga PDF (jsPDF) + botón "Copiar texto" (para quienes no pueden abrir PDF)
6. **Guía de presentación:** "¿Dónde lo llevás?" — entidad, dirección, horario, documentos a acompañar (del catálogo de entidades)
7. Disclaimer permanente: "Este borrador es orientativo. Consulte con un abogado para asesoría legal específica."

#### Camino B: Pagar con descuento (verde)

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
| 1 | P0 Inicio con 3 caminos | P0 |
| 2 | P1 Formulario con validación estricta de fechas | P0 |
| 3 | P2 Explicador (lenguaje claro + montos + descuentos + prevención) | P0 |
| 4 | P3 Semáforo de legalidad (iconografía + checklist + plazos) | P0 |
| 5 | P4 Generador de borrador PDF con checkbox obligatorio | P0 |
| 6 | Catálogo de 15 infracciones con copy claro | P0 |
| 7 | PWA instalable + offline | P0 |

### CAPA 2 — Valor (si sobra tiempo mañana / viernes temprano)
| # | Funcionalidad | Prioridad |
|---|---|---|
| 8 | OCR de boletas (Tesseract.js) con 3-4 boletas mock PNG | P1 |
| 9 | i18n: k'iche' + q'eqchi' + garífuna (UI + audio de frases clave) | P1 |
| 10 | Historial de consultas (localStorage) | P1 |
| 11 | Buscador de infracciones por palabra ("semáforo", "casco", "placa") | P1 |
| 12 | Detección de estafas (guía: notificación real vs SMS falso) | P1 |

### CAPA 3 — Escala (post-hackathon / slides de pitch)
| # | Funcionalidad | Prioridad |
|---|---|---|
| 13 | Dashboard de flota B2B (empresas de transporte) | P2 (pitch) |
| 14 | Alertas push de plazos (Service Worker + ntfy) | P2 |
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
| Accesibilidad | Solo español, desktop | Multilingüe + audio + móvil-first |
| Confianza | "null" en nombres, bugs | Copy cuidado, disclaimers claros |
| Estética | Portales anticuados | Tema oscuro estilo Linear/Notion (de QUIZZO) |
| Offline | Requiere internet | PWA offline-first |

**Detalles visuales:**
- Tema oscuro con acento verde/ámbar/rojo según semáforo
- Tipografía legible (Space Grotesk de QUIZZO o similar)
- Tarjetas con glass blur (patrón QUIZZO, blur bajo para rendimiento)
- Iconografía SVG inline (sin dependencias)
- Animaciones sutiles de entrada (fade/slide, patrón QUIZZO)

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