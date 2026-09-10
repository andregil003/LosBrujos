# PLAN DE TRABAJO — LosBrujos · HACKCREA 2026

> **Proyecto:** MultaClara — "De multa a educación"
> **Reto:** Reto 05 — Multas de tránsito que educan, no que castigan
> **Equipo:** LosBrujos (5 miembros)
> **Hackathon:** 28-30 agosto / 9-11 septiembre 2026
> **Última actualización:** 10 septiembre 2026

---

## QUÉ ES ESTE PROYECTO (resumen para todos)

**MultaClara** es una aplicación web (PWA) que le explica a un ciudadano de Guatemala:
1. **Qué significa** la multa que le llegó (en lenguaje claro, sin jerga legal)
2. **Si es válida** o si la puede pelear (semáforo de legalidad)
3. **Cuánto puede ahorrar** (descuentos, cursos viales, condonaciones)
4. **Cómo impugnar** si tiene derecho (genera un borrador PDF listo para presentar)

**La frase que resume todo:** *"Una multa que nadie entiende no cambia el comportamiento."*

**Por qué importa:** En Guatemala hay 6.7 millones de vehículos, el 53% son motos, y el 59% de los conductores no paga impuesto de circulación porque el sistema es confuso, fragmentado y punitivo. No hay ninguna app que haga lo que MultaClara.

---

## LO QUE YA TENEMOS HECHO

Antes de empezar a programar, ya investigamos un montón. Esto es lo que ya está listo:

| Qué | Estado | Dónde está |
|-----|--------|------------|
| Investigación del problema (1000+ líneas) | COMPLETO | `reto5-multas-transito.md` |
| Análisis de los 3 retos del hackathon | COMPLETO | `analisis-retos.md` |
| Perfiles de hosts y organizadores | COMPLETO | `perfiles.md` |
| Contexto del evento HACKCREA | COMPLETO | `evento.md` |
| Fuentes verificadas | COMPLETO | `fuentes.md` |
| Benchmarking de 8 países | COMPLETO | En reto5-multas-transito.md |
| Análisis competitivo de apps existentes | COMPLETO | En reto5-multas-transito.md |
| 15 infracciones con montos y consejos | COMPLETO | `data/infracciones.json` |
| Plantilla de impugnación | COMPLETO | `data/plantilla-impugnacion.md` |
| 9 diagramas interactivos | COMPLETOS | `diagramas/` (desplegados en GitHub Pages) |
| Spec detallada de la app (306 líneas) | COMPLETA | `app/SPEC.md` |
| Plan estratégico con prioridades | COMPLETO | `PLAN-ESTRATEGICO.md` |
| Datos de entidades emisoras | FALTAN | Necesitamos `data/entidades.json` |
| Código de la app | NO EXISTE | Hay que construirlo desde cero |

**En resumen:** La parte de investigación y planificación está 100%. Lo que falta es **programar la app**.

---

## QUÉ HAY QUE HACER (los deliverables)

Están organizados por prioridad. Los P0 son IMPRESCINDIBLES (sin esto no hay demo). Los P1 son valor añadido. Los P2 son para el roadmap.

### P0 — IMPRESCINDIBLES (Hay que hacer TODOS)

| # | Qué es | Para qué sirve | Complejidad |
|---|--------|----------------|-------------|
| **P0-1** | Estructura del proyecto con Vite | Crear la base: carpetas, archivos, configuración para que todo funcione | Baja |
| **P0-2** | Pantalla de entrada (QR/URL + placa + idioma) | Lo primero que ve el usuario: escanea QR, ingresa placa, elige idioma (ES/K'iche'/Kawchiquel) y tamaño de letra | Baja |
| **P0-2b** | Selección de placa | Múltiples placas en localStorage, sin cuentas. Para gente con varios vehículos | Baja |
| **P0-2c** | Vista de municipalidades | Gris=sin multas, color=con multas, número en esquina superior derecha | Media |
| **P0-3** | Formulario de la boleta | Donde el usuario ingresa los datos de su multa: fecha, entidad, qué infracción, si es propietario, si manejaba | Media |
| **P0-4** | Explicador de multa | EL CORAZÓN: traduce la multa a lenguaje claro, muestra montos, descuentos, prevención, botones "?" info | Media |
| **P0-5** | Semáforo de legalidad | Dice si la multa es válida (verde/amarillo/rojo) con checklist de Decreto 33-2024. **La gente no sabe que puede apelar** | Alta |
| **P0-5b** | ¿Qué hago? | 3 opciones: pagar / oposición / prescripción | Media |
| **P0-6** | Generador de PDF de impugnación | Crea un borrador listo para presentar (oposición o prescripción), con checkbox obligatorio de disclaimer | Media |
| **P0-7** | PWA instalable | Que se pueda instalar como app en el celular y funcione offline | Baja |
| **P0-8** | Catálogo de entidades | JSON con las 11 entidades que multan, sus jurisdicciones y contactos | Baja |
| **P0-9** | Backend demo: Google Sheets + Apps Script | Datos ficticios para demo. Sheets como BD + Apps Script como API | Media |

**Resultado P0:** App completa que explica una multa, dice si es impugnable, genera el PDF, y muestra las municipalidades con multas. **Esto es el 80% del pitch.**

### P1 — VALOR AÑADIDO (Si sobra tiempo)

| # | Qué es | Para qué sirve | Complejidad |
|---|--------|----------------|-------------|
| **P1-1** | Traducción a K'iche' + Kawchiquel | Inclusión lingüística: hablantes de idiomas mayas que no entienden la boleta | Media |
| **P1-2** | Buscador de infracciones | El usuario escribe "semáforo" y le aparece la infracción | Baja |
| **P1-3** | Guía de estafas | Cómo identificar multas falsas (problema real en Guatemala) | Baja |
| **P1-4** | Escenario 120 días vencidos | Qué hacer cuando ya prescribió. Guía con abogado o presentación directa | Baja |

### P2 — ESCALA / ROADMAP (Post-hackathon)

| # | Qué es | Para qué sirve |
|---|--------|----------------|
| **P2-1** | OCR de boletas | Fotografiar la boleta y que la app lea los datos sola |
| **P2-2** | Dashboard de flota B2B | Para empresas de transporte que tienen muchos vehículos |
| **P2-3** | Temas CSS (oscuro/claro) | Que el usuario pueda elegir el tema |

---

## CÓMO FUNCIONA LA APP (flujo del usuario)

Así es como el usuario va a usar MultaClara:

```
PASO 1: El usuario llega a la app (QR o URL)
   │
   ▼
PASO 2: Elige idioma (Español / K'iche' / Kawchiquel) + tamaño de letra
   │
   ▼
PASO 3: Selecciona su placa (guardada en localStorage) o ingresa una nueva
   │
   ▼
PASO 4: Ve la vista de municipalidades
   • Gris = sin multas
   • Color = con multas (número en la esquina superior derecha)
   │
   ▼
PASO 5: Elige una multa y ve la explicación
   • Qué hizo mal (en lenguaje claro, con botones "?" para info)
   • Cuánto cuesta + cuánto ahorraría con descuento
   • Por qué es peligroso (cápsula de prevención)
   │
   ▼
PASO 6: Ve el semáforo de legalidad
   • 🟢 Verde: "Tu multa parece válida" → guía de pago con descuento
   • 🟡 Amarillo: "Revisá estos puntos" → sugiere verificar
   • 🔴 Rojo: "Podrías impugnar" → checklist + generador de PDF
   │
   ▼
PASO 7: ¿Qué hago?
   • Opción A: Pagar con descuento
   • Opción B: Desacuerdo por Oposición (form digital)
   • Opción C: Desacuerdo por Prescripción (>120 días)
   │
   ▼
PASO 8: Toma acción
   • Genera borrador de impugnación (PDF) o solicitud de prescripción
   • O ve cómo pagar con descuento
   • En cualquier caso: ve dónde ir, qué documentos llevar
```

---

## TECH STACK (con qué construimos)

| Tecnología | Para qué | Por qué |
|------------|----------|---------|
| **Vite** | Herramienta de desarrollo | Rápido, moderno, hot reload |
| **React** | Framework de componentes | Componentes reutilizables, ecosistema grande |
| **shadcn/ui** | Componentes UI | Button, Card, Select, Input, Checkbox — listos y profesionales |
| **Tailwind CSS** | Estilos | Responsive, dark mode, contraste alto en minutos |
| **JSON** | Datos de infracciones y entidades | Simple, sin backend |
| **Google Sheets + Apps Script** | Backend demo | Datos ficticios para demo. Sheets como BD + Apps Script como API |
| **jsPDF** | Generar PDFs de impugnación | Funciona en el navegador |
| **Service Worker** | Modo offline (PWA) | Que funcione sin internet |
| **Cloudflare Pages** | Desplegar la app | Gratis, rápido, CDN global |

**Decisión clave:** Usamos React + shadcn/ui + Tailwind. Componentes listos = menos código = menos bugs en el hackathon.

---

## CRONOGRAMA — HOY (10 septiembre, hackathon presencial)

**Todo sale HOY.** El día está dividido en 3 bloques. Cada persona trabaja en su rol desde el minuto 1.

### MAÑANA (9:00 – 12:30) — CIMIENTOS + ESTRUCTURA
| Bloque | Tarea | Responsable | Estado |
|--------|-------|-------------|--------|
| 9:00 | Setup Vite + React + shadcn + Tailwind + estructura de carpetas | **Arquitecto** | |
| 9:00 | Paleta de colores, tipografía, componentes SVG (semáforo, check, warning) | **Diseñador** | |
| 9:00 | Crear `entidades.json` (11 entidades, jurisdicciones, contactos, horarios) | **Investigador** | |
| 9:30 | Service Worker + manifest.json (PWA instalable + offline) | **Arquitecto** | |
| 10:00 | Pantalla de entrada — QR/URL + placa + idioma (ES/K'iche'/Kawchiquel) + tamaño letra | **Frontend** | |
| 10:00 | Empezar copy de las 15 infracciones en lenguaje claro | **Investigador** | |
| 10:30 | Lógica core: cálculo de prescripción (120 días), validación de fechas | **Lógica** | |
| 11:00 | Selección de placa (localStorage, múltiples placas) | **Frontend** | |
| 11:00 | Vista de municipalidades (gris=sin multas, color=con multas, números) | **Frontend + Lógica** | |
| 11:30 | Formulario de boleta (P1) con validación estricta + "¿Sos propietario?" + "¿Vos manejabas?" | **Frontend** | |
| 12:00 | Revisión rápida: ¿la app carga? ¿Tailwind se ve? ¿la PWA instala? | **Todos** | |

### TARDE (1:30 – 6:00) — CORE FUNCIONAL
| Bloque | Tarea | Responsable | Estado |
|--------|-------|-------------|--------|
| 1:30 | Explicador de multa: lenguaje claro + montos + descuentos + prevención + botones "?" | **Frontend + Lógica** | |
| 2:00 | Lógica del semáforo: leer datos → decidir verde/amarillo/rojo | **Lógica** | |
| 2:30 | Semaforo UI: tarjeta gigante + iconografía + checklist Decreto 33-2024 | **Frontend + Diseñador** | |
| 3:00 | ¿Qué hago? — 3 opciones: pagar / oposición / prescripción | **Frontend + Lógica** | |
| 3:00 | Generador de PDF de impugnación (jsPDF + checkbox obligatorio + plantilla) | **Lógica** | |
| 3:30 | Estructura i18n + traducciones a K'iche' + Kawchiquel (al menos UI + frases clave) | **Investigador** | |
| 4:00 | Backend demo: Google Sheets + Apps Script (datos ficticios) | **Arquitecto** | |
| 4:00 | Guía de estafas (notificación real vs SMS falso) | **Investigador** | |
| 4:30 | Escenario 120 días vencidos (qué hacer cuando ya prescribió) | **Lógica** | |
| 5:00 | QA visual: probar en celular, tablet, desktop. Lista de bugs | **Diseñador** | |
| 5:30 | Fix de bugs + pulido visual + responsive final | **Frontend + Arquitecto** | |

### NOCHE (6:00 – 9:00) — PITCH + DEPLOY FINAL
| Bloque | Tarea | Responsable | Estado |
|--------|-------|-------------|--------|
| 6:00 | Deploy final a Cloudflare Pages (versión pulida) | **Arquitecto** | |
| 6:00 | Slides de presentación (5 min) | **Diseñador + Investigador** | |
| 6:30 | Ensayo de pitch #1 | **Todos** | |
| 7:00 | Ajustes según feedback del ensayo | **Responsable de cada area** | |
| 7:30 | Ensayo de pitch #2 (final) | **Todos** | |
| 8:00 | Verificación final: app carga, PDF genera, offline funciona, K'iche' visible | **Todos** | |
| 8:30 | Submit / presentación | **Líder de pitch** | |

---

## LOS 5 ROLES DEL EQUIPO

Cada persona tiene un rol claro. Nadie se pisa, todos saben qué hacer.

### ROL 1: ARQUITECTO (Desarrollador Principal)
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Crea la estructura del proyecto (carpetas, archivos, configuración)
- Configura Vite + React + shadcn + Tailwind
- Configura el Service Worker y el manifest.json (PWA)
- Escribe la lógica core: cálculo de prescripción, semáforo, validación de fechas
- Configura Google Sheets + Apps Script (backend demo con datos ficticios)
- Despliega a Cloudflare Pages
- Decide las decisiones técnicas (qué librería usar, cómo organizar el código)

**Herramientas que usa:** Vite, React, shadcn/ui, Tailwind CSS, Service Worker, Google Sheets + Apps Script, Cloudflare Pages

**Entregables:**
- Proyecto funcionando en `npm run dev`
- Estructura de carpetas limpia
- Service Worker activo (offline)
- Backend demo conectado (Sheets + Apps Script)
- App desplegada en Cloudflare Pages

**Depende de:** Nadie (es el primero en empezar)

---

### ROL 2: FRONTEND (Constructor de Interfaces)
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Construye las pantallas: entrada, selección de placa, municipalidades, formulario, explicador, semáforo, ¿qué hago?, PDF
- Crea componentes React de cada vista (Home, Placas, Municipios, Form, Explicador, Semaforo, QueHago, Accion)
- Usa componentes de shadcn (Button, Card, Select, Input, Checkbox, Tabs, Tooltip)
- Implementa selector de idioma (ES/K'iche'/Kawchiquel) + tamaño de letra
- Implementa botones "?" para info contextual
- Aplica Tailwind para responsive y tema oscuro
- Hace que todo se vea bien en celular y en desktop

**Herramientas que usa:** React, shadcn/ui, Tailwind CSS, SVGs

**Entregables:**
- Pantalla de entrada con QR/URL + placa + idioma
- Selección de placa (múltiples placas en localStorage)
- Vista de municipalidades (gris=sin multas, color=con multas, números)
- Formulario de boleta con validación visual + "¿Sos propietario?"
- Tarjeta de explicador con montos, prevención y botones "?"
- Tarjeta de semáforo con iconografía + barras de plazo
- Pantalla "¿Qué hago?" con 3 opciones
- Todo responsive (mobile-first)

**Depende de:** El Arquitecto le entrega la estructura base (Día 1)

---

### ROL 3: LÓGICA DE NEGOCIO (Motor de la App)
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Conecta el formulario con el explicador (si el usuario pone fecha X y entidad Y, muestra Z)
- Implementa la lógica del semáforo: lee los datos de la boleta y decide verde/amarillo/rojo
- Maneja el catálogo de infracciones (lectura del JSON, búsqueda, filtrado)
- Implementa el "¿Sos propietario?" + "¿Vos manejabas?" y la guía especial
- Maneja localStorage para las placas guardadas
- Implementa la lógica de prescripción (120 días) y oposición (15 días)
- Conecta con Google Sheets + Apps Script para datos demo

**Herramientas que usa:** JavaScript, JSON, localStorage, Apps Script

**Entregables:**
- Función de semáforo (verde/amarillo/rojo según datos)
- Función de prescripción (120 días) y oposición (15 días)
- Conexión formulario → explicador → semáforo → ¿qué hago? → PDF
- Búsqueda de infracciones por palabra
- Placas guardadas en localStorage
- Conexión con datos demo (Sheets + Apps Script)

**Depende de:** El Arquitecto le entrega `core.js` y `data.js` (Día 1-2)

---

### ROL 4: INVESTIGADOR / CONTENIDO
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Crea `entidades.json` con las 11 entidades, jurisdicciones, contactos, horarios
- Escribe el copy en lenguaje claro para las 15 infracciones
- Prepara las traducciones a K'iche' y Kawchiquel (UI + frases clave)
- Crea la guía de estafas (notificación real vs SMS falso)
- Crea el escenario 120 días vencidos (qué hacer cuando ya prescribió)
- Investigación adicional si el jurado pregunta por algo específico
- Prepara el guion del pitch

**Herramientas que usa:** Markdown, JSON, Google Translate / hablantes nativos

**Entregables:**
- `data/entidades.json` (11 entidades completas)
- Copy de las 15 infracciones en lenguaje claro
- Archivos de i18n: español, K'iche', Kawchiquel
- Guía de estafas (contenido editorial)
- Guía de prescripción (120 días vencidos)
- Guion de pitch de 5 minutos

**Depende de:** Nadie (puede empezar desde el Día 1)

---

### ROL 5: DISEÑADOR / QA / PITCH
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Define la paleta de colores, tipografía, espaciados en Tailwind
- Diseña los componentes: tarjetas, botones, iconos SVG
- Revisa que todo se vea bien (QA visual)
- Testea la app en diferentes pantallas (celular, tablet, desktop)
- Prepara las slides de la presentación
- Ensaya el pitch con el equipo
- Hace el deploy final y verificación

**Herramientas que usa:** Tailwind CSS, SVG, Figma (si lo usan), presentaciones

**Entregables:**
- Paleta de colores definida (oscuro + acentos verde/ámbar/rojo)
- Componentes SVG (iconos de semáforo, check, warning)
- Lista de bugs visuales para que los developes arreglen
- Slides de presentación
- Checklist post-deploy

**Depende de:** El Arquitecto le entrega la base visual (Día 1)

---

## MATRIZ DE RESPONSABILIDADES (quién hace qué)

| Tarea | Arquitecto | Frontend | Lógica | Investigador | Diseñador/QA |
|-------|:----------:|:--------:|:------:|:------------:|:------------:|
| Setup Vite + React + shadcn | **LÍDER** | Ayuda | — | — | — |
| Service Worker / PWA | **LÍDER** | — | — | — | Verifica |
| Tailwind / Estilos base | Ayuda | **LÍDER** | — | — | **LÍDER** |
| Pantalla entrada (QR/URL + placa + idioma) | — | **LÍDER** | — | — | Diseña |
| Selección de placa (localStorage) | — | **LÍDER** | **LÍDER** | — | Revisa |
| Vista de municipalidades | — | **LÍDER** | **LÍDER** | — | Diseña |
| Formulario boleta (+ propietario) | — | **LÍDER** | Conecta | — | Revisa |
| Explicador multa (+ botones "?") | — | **LÍDER** | **LÍDER** | Copy | Diseña |
| Semáforo legalidad | Ayuda | UI | **LÍDER** | — | Iconos |
| ¿Qué hago? (3 opciones) | — | **LÍDER** | **LÍDER** | — | Diseña |
| Generador PDF | — | Ayuda | **LÍDER** | — | Verifica |
| entidades.json | — | — | — | **LÍDER** | — |
| i18n (K'iche' + Kawchiquel) | — | Conecta | — | **LÍDER** | — |
| Guía estafas | — | — | — | **LÍDER** | Diseña |
| Escenario 120 días | — | — | **LÍDER** | Ayuda | — |
| Backend demo (Sheets + Apps Script) | **LÍDER** | — | Ayuda | — | — |
| Buscador infracciones | — | Ayuda | **LÍDER** | — | — |
| Deploy Cloudflare | **LÍDER** | — | — | — | Verifica |
| Pitch / Slides | — | — | — | Ayuda | **LÍDER** |
| QA / Tests manuales | Verifica | Revisa | Revisa | — | **LÍDER** |

---

## LO QUE EL JURADO VA A BUSCAR (y cómo lo cubrimos)

| Qué busca el jurado | Cómo lo mostramos | Quién lo prepara |
|---------------------|-------------------|------------------|
| **UX excepcional** | Tema oscuro, responsive, lenguaje claro, botones "?" info | Diseñador + Frontend |
| **Tecnología aplicada** | PWA offline, PDF generation, i18n, Google Sheets + Apps Script | Arquitecto + Lógica |
| **Impacto medible** | 6.7M+ vehículos, 53% motos, 4,233 accidentes | Investigador |
| **Inclusión** | K'iche' + Kawchiquel, tamaño de letra accesible | Investigador |
| **Viabilidad** | Open source, Cloudflare Pages gratis, Sheets como BD demo | Arquitecto |
| **Cumplimiento legal** | Decreto 33-2024, Ley 19-2003, Ley 89-2005 | Investigador |
| **Storytelling** | "De multa a educación" — narrativa clara | Líder de pitch |

---

## FRASES CLAVE PARA EL PITCH

Usá estas frases en la presentación:

- **"Una multa que nadie entiende no cambia el comportamiento"**
- **"De multa a educación: transformamos punitivo en preventivo"**
- **"Offline-first porque el 39% de Guatemala no tiene internet confiable"**
- **"Interfaz en K'iche', Q'eqchi' y Garífuna — la primera app de tránsito multilingüe"**
- **"Ejemplo vivo de la Interoperabilidad Digital que la Iniciativa 6626 propone"**

---

## RESTRICCIONES DEL RETO (lo que NO podemos hacer)

| Restricción | Cómo la cumplimos |
|-------------|-------------------|
| "NO es asesoría legal" | Checkbox obligatorio + disclaimer en cada PDF |
| "NO presenta impugnaciones" | Genera BORRADORES, no documentos legales |
| "NO garantiza resultados" | Mensaje explícito: "Consulte con un abogado" |
| "NO representa a MINGOB" | Disclaimer: "Proyecto independiente" |

---

## RIESGOS Y CÓMO LOS MANEJAMOS

| Riesgo | Qué tan probable | Qué tan grave | Cómo lo evitamos |
|--------|:----------------:|:-------------:|------------------|
| No alcanzamos a terminar el P0 | Alta | Crítico | Priorizamos P0-1 a P0-6; PWA puede ser mínimo |
| El PDF no genera bien | Media | Alto | Usamos jsPDF probado; fallback "copiar texto" |
| Las traducciones consumen tiempo | Media | Medio | Empezamos con español completo; K'iche' solo UI |
| Los montos están mal en el JSON | Baja | Alto | Los `null` ya están marcados; mostramos "verificar" |
| El deploy falla | Baja | Alto | Cloudflare Pages es straightforward; testear temprano |
| El jurado pregunta por OCR | Media | Medio | Tenemos mock listo; explicamos que es V2 |

---

## ESTADÍSTICAS PARA EL PITCH (datos que debemos memorizar)

| Dato | Cifra | Fuente |
|------|-------|--------|
| Vehículos registrados en Guatemala | 6,700,000+ | Departamento de Tránsito |
| % que son motos | 53% | SAT |
| % que no paga impuesto de circulación | 59.56% | Prensa Libre |
| Accidentes de tránsito (primer semestre 2026) | 4,233 | PNC |
| Muertes viales (primer semestre 2026) | 1,135 | PNC |
| Aumento de multas 2026 vs 2025 | +38% | PNC |
| Brecha digital (sin internet) | 39% | Prensa Libre |
| Acceso internet en comunidades rurales | <22% | PIIED |
| Mortalidad vial vs promedio LATAM | 35% superior | OMS |

---

## COMANDOS ÚTILES (para el equipo técnico)

```bash
# Clonar el repo (si alguien no lo tiene)
git clone https://github.com/andregil003/LosBrujos.git

# Instalar dependencias
npm install

# Correr en desarrollo
npm run dev

# Build para producción
npm run build

# Deploy a Cloudflare Pages
npx wrangler pages deploy dist
```

---

## CHECKLIST PRE-PITCH (revisar antes de presentar)

- [ ] La app carga en menos de 3 segundos
- [ ] Funciona en celular (responsive)
- [ ] Funciona offline (PWA instalable)
- [ ] El formulario valida fechas correctamente
- [ ] El explicador muestra montos y descuentos
- [ ] El semáforo funciona (verde/amarillo/rojo)
- [ ] El PDF se genera y se puede descargar
- [ ] El disclaimer es visible en cada paso
- [ ] Las traducciones a K'iche' están (al menos UI)
- [ ] El pitch dura 5 minutos o menos
- [ ] Todos los team members pueden explicar su rol

---

> **Recuerden:** Capa 1 completa y pulida > Capa 1 + Capa 2 a medias. El jurado premia lo que funciona, no lo que promete. 🧙‍♂️
