# 🎯 Plan Estratégico — HACKCREA 2026 / LosBrujos / Reto 05

> **Fecha:** 2026-09-10
> **Objetivo:** Definir deliverables críticos, priorizar por factibilidad, mapear sistemas existentes y definir exactamente qué construimos, qué mejoramos y qué dejamos para después.

---

## 1. MAPA DEL ECOSISTEMA — Sistemas Existentes en Guatemala (lo que YA existe)

### 1.1 Entidades que MULTAN (10+)

| Entidad | Jurisdicción | Multa en rutas nacionales | Portal propio |
|---|---|---|---|
| **PNC — Depto. de Tránsito** | Rutas nacionales, departamentales, centroamericanas | ✅ Sí | transito.gob.gt |
| **EMETRA** | Ciudad de Guatemala | ❌ Solo CDMX | muniguate.com/emetra |
| **PMT Villa Nueva** | Villa Nueva | ❌ Solo municipal | — |
| **PMT Emixtra (Mixco)** | Mixco | ❌ Solo municipal | — |
| **PMT Escuintla** | Escuintla | ❌ Solo municipal | — |
| **PMT Antigua Guatemala** | Antigua | ❌ Solo municipal | — |
| **PMT Jutiapa** | Jutiapa | ❌ Solo municipal | — |
| **PMT Palencia** | Palencia | ❌ Solo municipal | — |
| **PMT S.C. Pinula** | S.C. Pinula | ❌ Solo municipal | — |
| **PMT San Lucas** | San Lucas Sacatepéquez | ❌ Solo municipal | — |
| **PMT Amatitlán** | Amatitlán | ❌ Solo municipal | — |

### 1.2 Portales de CONSULTA (fragmentados)

| Portal | Qué cubre | Cómo funciona | Problemas clave |
|---|---|---|---|
| **Portal SAT** (portal.sat.gob.gt/portal/multas) | PNC + EMETRA + algunas munis | Placa + NIT | Muestra "null" en nombres, sin explicaciones, sin impugnación, requiere NIT |
| **PNC Tránsito** (transito.gob.gt) | Solo PNC | Placa + NIT | Solo PNC, sin info de impugnación |
| **EMETRA/MuniGuate** (muniguate.com/emetra) | Solo CDMX | Web básica | Sin plazos, pago solo Banrural |
| **Portales municipales** | Cada muni lo suyo | Varía | Muchos no tienen portal |

### 1.3 Sistemas de PAGO (fragmentados)

| Entidad | Banco/Canal | Canales disponibles |
|---|---|---|
| PNC | Bancos del sistema | Presencial + banca en línea |
| EMETRA | Banrural (exclusivo) | Presencial + banca virtual Banrural |
| Mixco/S.C. Pinula/Fraijanes | Banco Industrial | bi en línea |
| Solvencia multas en línea | transito.gob.gt | Desde agosto 2025 |

**Conclusión:** NO existe app unificada, NO hay APIs públicas, NO hay notificación digital consistente.

### 1.4 Apps Competidoras

| App | Estado | Rating | Limitaciones |
|---|---|---|---|
| **Multas Guate** (Android) | Activa | ⭐ 2.1 | Solo consulta básica, sin explicación, sin OCR, sin idiomas, sin plazos |
| **MuniGuate 2.0** (iOS/Android) | Activa (última actualización 2020) | ⭐ 2.8 | Solo EMETRA, bugs en pago, interfaz vieja |
| **TransitoGT** (Android) | Inactiva (2022) | ⭐ 1.5 | Solo mapas de tráfico, abandonada |

**Brecha:** Ninguna app combina: consulta + explicación + impugnación + idiomas + offline.

### 1.5 Marco Legal vigente

| Normativa | Impacto en el reto |
|---|---|
| **Decreto 33-2024** | Base principal: notificación obligatoria, 15 días para impugnar, 120 días prescripción, 6 requisitos de notificación |
| **Ley 19-2003 (Idiomas)** Art. 9 | Derecho a info del Estado en idiomas mayas → our app cumple lo que el Estado no |
| **Ley 89-2005 (Datos)** | Todo local (localStorage/IndexedDB) cumple sin problem |
| **Iniciativa 6626** | Interoperabilidad digital — nuestro prototipo es ejemplo vivo |

---

## 2. LO QUE YA TENEMOS (Equipo LosBrujos)

### 2.1 Investigación ✅ COMPLETA
- [x] `reto5-multas-transito.md` — 1000+ líneas de investigación profunda
- [x] `analisis-retos.md` — Comparativa de 3 retos, por qué elegimos el 05
- [x] `gov-digital-gt.md` — Panorama de gobierno digital en Guatemala
- [x] `perfiles.md` — Hosts, organizadores, empresa operativa
- [x] `evento.md` — Contexto del hackathon
- [x] `fuentes.md` — Fuentes verificadas
- [x] Benchmarking de 8 países
- [x] Análisis competitivo de apps existentes
- [x] Voz del usuario (redes sociales, quejas documentadas)

### 2.2 Datos ✅ PARCIAL
- [x] `data/infracciones.json` — 15 infracciones con montos, consejos, emisores
- [x] `data/plantilla-impugnacion.md` — Plantilla con 7 vicios condicionales
- [ ] `data/entidades.json` — Catálogo de entidades + jurisdicción + contacto (FALTA)
- [ ] Datos de descuentos activos por municipalidad (parcial en investigación)
- [ ] Boletas mock para OCR (FALTAN)

### 2.3 Documentación ✅ COMPLETA
- [x] 9 diagramas HTML interactivos (deployados en GitHub Pages)
- [x] `README.md` — Actualizado con links a diagramas

### 2.4 Prototipo ❌ NO EXISTE AÚN
- [ ] Solo hay `app/SPEC.md` (la spec detallada de 306 líneas)
- [ ] NO hay HTML, CSS, JS, ni ningún código ejecutable

---

## 3. DELIVERABLES PRIORIZADOS — ¿QUÉ CONSTRUIMOS?

### 🔴 P0 — IMPRESCINDIBLES (Sin esto no hay demo funcional)

| # | Deliverable | Complejidad | Dependencias | Notas |
|---|---|---|---|---|
| **P0-1** | **Estructura Vite + React + routing SPA** | ⭐ Baja | Ninguna | Scaffold con `npm create vite`, estructura de carpetas, componentes shadcn, routing con React Router. |
| **P0-2** | **P0 — Inicio** (hero + 3 caminos) | ⭐ Baja | P0-1 | Landing page: "¿Te llegó una multa?" + botones de entrada |
| **P0-3** | **P1 — Formulario de boleta** | ⭐⭐ Media | P0-1 | Selector tipo papel, fecha, entidad, infracción. Validación estricta de fechas. **+ pregunta "¿Vos manejabas?" con guía especial si no.** |
| **P0-4** | **P2 — Explicador de multa** | ⭐⭐ Media | P0-3 + JSON | Lenguaje claro + montos + descuentos + prevención. El CORAZÓN del reto. |
| **P0-5** | **P3 — Semáforo de legalidad** | ⭐⭐⭐ Alta | P0-4 + lógica core | Verde/Amarillo/Rojo con iconografía + checklist Decreto 33-2024 + barras de plazo |
| **P0-6** | **P4 — Generador de PDF impugnación** | ⭐⭐ Media | P0-5 + plantilla | jsPDF + checkbox obligatorio + plantilla condicional |
| **P0-7** | **PWA instalable** | ⭐ Baja | P0-1 | `manifest.json` + `sw.js` + icons. Offline-first. |
| **P0-8** | **Datos: `entidades.json`** | ⭐ Baja | Investigación | Catálogo de 11 entidades + jurisdicción + contacto + horario |

**Resultado P0:** App completa que explica una multa, dice si es impugnable, y genera el PDF. **Esto es el 80% del pitch.**

### 🟡 P1 — VALOR AÑADIDO (Si sobra tiempo)

| # | Deliverable | Complejidad | Dependencias | Notas |
|---|---|---|---|---|
| **P1-1** | **i18n: K'iche' + Inglés** | ⭐⭐ Media | P0 completo | UI + glosario. K'iche' = inclusión (Ley 19-2003). Inglés = turistas/expats. |
| **P1-2** | **Buscador de infracciones** | ⭐ Baja | P0-3 | "semáforo", "casco", "placa" → filtra el catálogo |
| **P1-3** | **Historial de consultas** | ⭐ Baja | P0-4 | localStorage, "Tus últimas consultas" |
| **P1-4** | **Guía de estafas** | ⭐ Baja | Investigación | Notificación real vs SMS falso. Contenido editorial. |
| **P1-5** | **Audio 🔊 frases clave** | ⭐⭐ Media | P1-1 | HTML audio para personas que hablan pero no leen su idioma |
| **P1-6** | **Guía "¿Dónde impugnar?"** | ⭐ Baja | P0-8 | Mapa/dirección/horario según entidad |

**Resultado P1:** App con inclusión lingüística + funcionalidades de valor. **Para el pitch: "3 idiomas: español + k'iche' + inglés — incluimos al 1.27M de k'iche'hablantes y a los extranjeros que manejan en Guatemala".**

### 🟢 P2 — ESCALA / POST-HACKATHON (Solo si sobra MUCHO tiempo o para el roadmap)

| # | Deliverable | Complejidad | Dependencias | Notas |
|---|---|---|---|---|
| **P2-1** | **OCR de boletas** (Tesseract.js) | ⭐⭐⭐ Alta | Mock boletas PNG | 3-4 boletas mock. Complejo de demostrar bien en poco tiempo. |
| **P2-2** | **Cálculo automático de prescripción** | ⭐ Baja | P0-3 | Ya está en la lógica, solo UI extra |
| **P2-3** | **Alertas push de plazos** | ⭐⭐⭐ Alta | Service Worker + permisos | Notificación "queda 1 día para impugnar" |
| **P2-4** | **Dashboard de flota B2B** | ⭐⭐⭐ Alta | Auth + datos | Para empresas de transporte. Solo pitch. |
| **P2-5** | **Temas CSS (oscuro/claro)** | ⭐ Baja | P0-1 | Tema oscuro estilo Linear/Notion |

---

## 4. ANÁLISIS POR SISTEMA — ¿QUÉ HACEMOS vs QUÉ YA EXISTE?

### 4.0 INSIGHT DE CAMPO: "La multa es del carro, no del conductor" 🚗💨

**Hallazgo de André (sept 2026):** Para ver multas hoy, entrás a cada municipalidad, ponés la placa y ves las de ESA municipalidad. No hay forma de ver todas tus multas globalmente. Y el problema más común: **el vehículo se presta** (familia, hijos, amigos) y las multas llegan al DUEÑO, no al conductor real.

**Implicaciones para MultaClara:**

| Aspecto | Sistema actual | MultaClara |
|---|---|---|
| Consulta | Por municipalidad, una por una | Un solo lugar (manual por ahora, placa en V2) |
| Responsable | Siempre el dueño paga | Pregunta "¿Vos manejabas?" → guía según respuesta |
| Dueño no manejaba | Sin opciones, paga | Guía: el conductor real responde O impugnar señalando que no era el dueño |
| Notificación | Al dueño del vehículo | Explica que el Decreto 33-2024 exige notificar al conductor o dueño, y que si no eras vos, hay argumento |

**Flujo nuevo en P1 (formulario):**
1. Después de ingresar los datos de la boleta → pregunta: **"¿Vos manejabas el vehículo?"**
2. **Sí** → continúa normal (explicador + semáforo)
3. **No** → muestra guía especial:
   - "La multa es del conductor, no del dueño"
   - "Si el conductor es familiar/conocido: coordiná con él para pagar o impugnar"
   - "Si no sabés quién manejaba: la notificación debe cumplir el Decreto 33-2024; si no te notificaron correctamente, hay argumento"
   - Opción de generar borrador de impugnación señalando que el notificado no era el conductor

**Por qué pega con el jurado:** Es un caso de uso REAL y cotidiano que ningún portal resuelve. El 53% de las multas son a motos (vehículos que se prestan mucho en familia). "¿Te prestaron el carro y te llegó la multa? Te decimos qué hacer."

---

### 4.1 CONSULTA DE MULTAS

| Aspecto | Sistema actual | Nuestro prototipo | Mejora |
|---|---|---|---|
| **Consulta** | Portal SAT: placa + NIT → solo muestra monto | Manual: tipo papel + fecha + entidad + infracción → explicación completa | ❌ No resolvemos la consulta por placa (no hay API). Sí resolvemos la EXPLICACIÓN. |
| **Cobertura** | Fragmentada: 4+ portales | Unificado: 11 entidades en un solo lugar | ✅ Un solo punto de entrada para entender |
| **Resultado** | Monto + nada más | Lenguaje claro + montos + descuentos + prevención | ✅ Transformamos datos en conocimiento |

**Decisión:** NO intentamos hacer scraping/consulta por placa. No hay API pública, el riesgo de demo rota es altísimo. En su lugar, el usuario ingresa los datos de su boleta manualmente (lo que cualquiera puede hacer con la boleta en la mano).

### 4.2 EXPLICACIÓN DE MULTAS

| Aspecto | Sistema actual | Nuestro prototipo | Mejora |
|---|---|---|---|
| **Lenguaje** | Jurídico ("Art. 90 del Reglamento") | Claro ("Pasaste con luz roja") | ✅ + Glosario expandible |
| **Montos** | Solo el monto | Monto + descuento curso vial + condonación activa | ✅ El ciudadano ve cuánto AHORRA |
| **Prevención** | Ninguna | Cápsula de prevención por infracción | ✅ "De multa a educación" |
| **Accesibilidad** | Solo español desktop | Multilingual + audio + móvil | ✅ Cumple Ley 19-2003 |

**Decisión:** ESTO es nuestro core. El explicador en lenguaje claro es el deliverable #1.

### 4.3 LEGALIDAD / IMPUGNACIÓN

| Aspecto | Sistema actual | Nuestro prototipo | Mejora |
|---|---|---|---|
| **Evaluación** | Ninguna | Semáforo de legalidad (3 estados) | ✅ Primera herramienta que evalúa si la multa es válida |
| **Plazos** | No se muestran | Barras de progreso visuales | ✅ "Quedan 12 de 15 días para impugnar" |
| **Impugnación** | No hay guía | Generador de borrador PDF | ✅ Checklist + plantilla + disclaimer |
| **Notificación** | No se valida | Checklist de 6 requisitos Decreto 33-2024 | ✅ El ciudadano sabe si su notificación es válida |

**Decisión:** El semáforo + generador PDF es nuestro diferenciador vs SAT/EMETRA.

### 4.4 INCLUSIÓN LINGÜÍSTICA

| Aspecto | Sistema actual | Nuestro prototipo | Mejora |
|---|---|---|---|
| **Idiomas** | Solo español | Español + K'iche' + Q'eqchi' + Garífuna | ✅ Primera app de tránsito multilingüe |
| **Audio** | Ninguno | 🔊 Frases clave en idioma local | ✅ Para analfabetas funcionales |
- **Base legal:** Ley 19-2003 Art. 9 (obligatorio, el Estado no lo cumple)

**Decisión:** El i18n es el factor diferenciador más fuerte para el jurado. Priorizar español + K'iche' al menos.

### 4.5 OFFLINE

| Aspecto | Sistema actual | Nuestro prototipo | Mejora |
|---|---|---|---|
| **Conectividad** | Requiere internet | PWA offline-first | ✅ 39% de Guatemala sin internet |
| **Datos** | Query al servidor | JSON local + localStorage | ✅ Cero dependencia de backend |
| **Instalación** | No es app | PWA instalable | ✅ Se instala desde el navegador |

---

## 5. CRONOGRAMA SUGERIDO (Hackathon)

### Día 1 — Cimientos (P0)
- [ ] Scaffold Vite + React + shadcn + Tailwind
- [ ] Tailwind (tema oscuro, responsive)
- [ ] P0 Inicio (hero + 3 caminos)
- [ ] Datos: `entidades.json`
- [ ] PWA: manifest + sw.js + icons

### Día 2 — Core (P0)
- [ ] P1 Formulario con validación de fechas
- [ ] P2 Explicador con datos del JSON
- [ ] Lógica core: semáforo + prescripción + plazos
- [ ] i18n: estructura + español completo

### Día 3 — Diferenciadores (P0 + P1)
- [ ] P3 Semáforo de legalidad (UI + lógica)
- [ ] P4 Generador PDF impugnación
- [ ] P1: K'iche' (al menos UI + frases clave)
- [ ] Deploy Cloudflare Pages
- [ ] Tests manuales + fix

### Día 4 — Pitch + Pulido
- [ ] P1: Q'eqchi' + Garífuna (parcial)
- [ ] Guía de estafas
- [ ] Historial localStorage
- [ ] Audio 🔊 (si hay tiempo)
- [ ] Pulido visual + responsive final
- [ ] Ensayo de pitch

---

## 6. RIESGOS Y MITIGACIONES

| Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|
| Tiempo insuficiente para P0 completo | Alta | Crítico | Priorizar P0-1 a P0-6; PWA puede ser mínimo |
| PDF no genera bien | Media | Alto | Usar jsPDF probado; tener fallback "copiar texto" |
| i18n consume mucho tiempo | Media | Medio | Empezar con español completo; K'iche' solo UI |
| Montos incorrectos en JSON | Baja | Alto | Los null ya están marcados; mostrar "verificar en boleta" |
| Deploy falla | Baja | Alto | Cloudflare Pages es straightforward; testear temprano |
| Jurado pregunta por OCR/demo | Media | Medio | Tener mock listo; explicar que es V2, enfocar en P0 |

---

## 7. MÉTRICAS DE ÉXITO (Para el pitch)

| Métrica | Benchmark | Nuestro objetivo |
|---|---|---|
| Tiempo de carga | <3s | <2s (PWA offline) |
| Cobertura de infracciones | — | 15 infracciones explicadas |
| Idiomas | — | 3 (español + k'iche' + inglés) |
| Funciona offline | — | ✅ PWA |
| PDF generado | — | ✅ Borrador impugnación |
| Cumple Decreto 33-2024 | — | ✅ 6 requisitos checklist |
| Usuarios potenciales | — | 6.7M+ vehículos |

---

## 8. DECISIONES TOMADAS ✅

| # | Pregunta | Decisión |
|---|---|---|
| 1 | ¿OCR en el MVP? | **NO** → V2. El usuario ingresa datos a mano de la boleta. |
| 2 | ¿Idiomas? | **Español + K'iche' + Inglés** (3 idiomas). Inglés para turistas/expats. |
| 3 | ¿Framework? | **Vite + React + shadcn + Tailwind** (componentes reutilizables, diseño profesional, responsive con Tailwind). Módulos ES, hot reload, build moderno. |
| 4 | ¿Tema visual? | **Oscuro** (estilo Linear/Notion). |
| 5 | ¿Deploy? | **Cloudflare Pages** (ya tienen wrangler config en SPEC). |
