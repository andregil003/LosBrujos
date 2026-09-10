# ENTREGABLES — LosBrujos · HACKCREA 2026

> **Este es el checklist maestro.** Cada vez que alguien termina algo, marca la casilla `[x]` y actualiza el contador de arriba.
> **Cómo marcar:** cambiar `- [ ]` por `- [x]` en la línea del entregable.
> **Última actualización:** 10 septiembre 2026

---

## 📊 PROGRESO GENERAL

**P0 (imprescindible):** `[x]` 1 / 12
**P1 (valor añadido):** `[ ]` 0 / 4
**P2 (roadmap):** `[ ]` 0 / 3
**Investigación/planificación:** `[x]` 12 / 12 ✅
**Pitch + deploy:** `[ ]` 0 / 5

**Total:** 13 / 33

---

## ✅ YA COMPLETADO (investigación y planificación)

- [x] Investigación del problema (1000+ líneas) — `reto5-multas-transito.md`
- [x] Análisis de los 3 retos del hackathon — `analisis-retos.md`
- [x] Perfiles de hosts y organizadores — `perfiles.md`
- [x] Contexto del evento HACKCREA — `evento.md`
- [x] Fuentes verificadas — `fuentes.md`
- [x] Benchmarking de 8 países — en reto5-multas-transito.md
- [x] Análisis competitivo de apps existentes — en reto5-multas-transito.md
- [x] 15 infracciones con montos y consejos — `data/infracciones.json`
- [x] Plantilla de impugnación — `data/plantilla-impugnacion.md`
- [x] 9 diagramas interactivos — `diagramas/` (GitHub Pages)
- [x] Spec detallada de la app — `app/SPEC.md`
- [x] Plan estratégico con prioridades — `PLAN-ESTRATEGICO.md`

---

## 🔴 P0 — IMPRESCINDIBLES (sin esto no hay demo)

| # | Entregable | Responsable | Estado |
|---|-----------|-------------|--------|
| P0-1 | Estructura del proyecto con Vite + React + shadcn + Tailwind | Arquitecto | `[x]` |
| P0-2 | Pantalla de entrada (QR/URL + placa + idioma + tamaño letra) | Frontend | `[ ]` |
| P0-2b | Selección de placa (múltiples placas en localStorage) | Frontend + Lógica | `[ ]` |
| P0-2c | Vista de municipalidades (gris=sin multas, color=con multas, número) | Frontend + Lógica | `[ ]` |
| P0-3 | Formulario de la boleta (+ "¿Sos propietario?" + "¿Vos manejabas?") | Frontend | `[ ]` |
| P0-4 | Explicador de multa (lenguaje claro + montos + descuentos + botones "?") | Frontend + Lógica | `[ ]` |
| P0-5 | Semáforo de legalidad (verde/amarillo/rojo + checklist Decreto 33-2024) | Lógica | `[ ]` |
| P0-5b | ¿Qué hago? (pagar / oposición 15 días / prescripción 120 días) | Frontend + Lógica | `[ ]` |
| P0-6 | Generador de PDF de impugnación (jsPDF + checkbox disclaimer) | Lógica | `[ ]` |
| P0-7 | PWA instalable (Service Worker + manifest + offline) | Arquitecto | `[ ]` |
| P0-8 | Catálogo de entidades — `entidades.json` (11 entidades) | Investigador | `[ ]` |
| P0-9 | Backend demo: Google Sheets + Apps Script (datos ficticios) | Arquitecto | `[ ]` |

**P0 completados:** 1 / 12

---

## 🟡 P1 — VALOR AÑADIDO (si sobra tiempo)

| # | Entregable | Responsable | Estado |
|---|-----------|-------------|--------|
| P1-1 | Traducciones i18n: K'iche' + Kawchiquel (UI + frases clave) | Uriel | `[ ]` |
| P1-2 | Buscador de infracciones (escribís "semáforo" y aparece) | Lógica | `[ ]` |
| P1-3 | Guía de estafas (notificación real vs SMS falso) | Investigador | `[ ]` |
| P1-4 | Escenario 120 días vencidos (qué hacer cuando ya prescribió) | Lógica | `[ ]` |

**P1 completados:** 0 / 4

---

## 🟢 P2 — ESCALA / ROADMAP (post-hackathon)

| # | Entregable | Estado |
|---|-----------|--------|
| P2-1 | OCR de boletas (fotografiar y que lea los datos sola) | `[ ]` |
| P2-2 | Dashboard de flota B2B (empresas con muchos vehículos) | `[ ]` |
| P2-3 | Temas CSS (oscuro/claro) | `[ ]` |

**P2 completados:** 0 / 3

---

## 🎨 ENTREGABLES POR ROL

### Arquitecto (André)
- [ ] Setup Vite + React + shadcn + Tailwind + estructura de carpetas
- [ ] Service Worker + manifest.json (PWA instalable + offline)
- [ ] Backend demo: Google Sheets + Apps Script conectado
- [ ] Deploy a Cloudflare Pages

### Frontend (Kevin)
- [ ] Pantalla de entrada — QR/URL + placa + idioma + tamaño letra
- [ ] Selección de placa (localStorage, múltiples placas)
- [ ] Vista de municipalidades (gris/color + números)
- [ ] Formulario de boleta con validación + "¿Sos propietario?" + "¿Vos manejabas?"
- [ ] Explicador de multa (montos, descuentos, prevención, botones "?")
- [ ] Tarjeta de semáforo (iconografía + barras de plazo)
- [ ] Pantalla "¿Qué hago?" (3 opciones)
- [ ] Todo responsive (mobile-first)

### Lógica de Negocio (Lemus)
- [ ] `constantes.js` — plazos legales (15/60/120 días)
- [ ] `core.js` — función de semáforo (verde/amarillo/rojo)
- [ ] `core.js` — función de prescripción (120 días) y oposición (15 días)
- [ ] `data.js` — carga de infracciones.json + entidades.json
- [ ] Conexión formulario → explicador → semáforo → ¿qué hago? → PDF
- [ ] Búsqueda de infracciones por palabra
- [ ] Placas guardadas en localStorage
- [ ] Conexión con datos demo (Sheets + Apps Script)
- [ ] Generador de PDF (jsPDF + plantilla + disclaimer)

### Investigador (Diego)
- [ ] `entidades.json` — 11 entidades (jurisdicciones, contactos, horarios)
- [ ] Copy de las 15 infracciones en lenguaje claro
- [ ] Guía de estafas (contenido editorial)
- [ ] Escenario 120 días vencidos (guía)
- [ ] Guion de pitch de 5 minutos

### Diseñador / QA / Pitch + i18n (Uriel)
- [ ] Paleta de colores + tipografía + espaciados en Tailwind
- [ ] Componentes SVG (semáforo, check, warning)
- [ ] `i18n/es.json` — traducciones español
- [ ] `i18n/kiche.json` — traducciones K'iche'
- [ ] `i18n/kawchiquel.json` — traducciones Kawchiquel
- [ ] QA visual: probar en celular, tablet, desktop
- [ ] Lista de bugs visuales para los desarrolladores
- [ ] Slides de presentación
- [ ] Checklist post-deploy

---

## 🎤 PITCH + DEPLOY

- [ ] Deploy final a Cloudflare Pages (versión pulida)
- [ ] Slides de presentación (5 min)
- [ ] Ensayo de pitch #1
- [ ] Ensayo de pitch #2 (final)
- [ ] Verificación final: app carga, PDF genera, offline funciona, K'iche' visible
- [ ] Submit / presentación

---

## 📝 CÓMO USAR ESTE ARCHIVO

1. **Al terminar algo:** cambiá `- [ ]` por `- [x]` en la línea correspondiente
2. **Actualizá el contador:** subí el número en "PROGRESO GENERAL" (ej: `[x] 3 / 9`)
3. **Commit + push a TU rama:**
   ```
   git add ENTREGABLES.md
   git commit -m "docs: marco entregable X"
   git push origin tu-rama
   ```
4. **Avisá por ntfy** que actualizaste el checklist

> **Nota:** este archivo vive en el repo `LosBrujos` (docs). El código de la app vive en `reto-5-brujos`. Cuando alguien termine un entregable de código, también marca la casilla acá.