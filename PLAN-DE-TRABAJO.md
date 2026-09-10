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
| **P0-2** | Pantalla de inicio (landing) | Lo primero que ve el usuario: "¿Te llegó una multa?" con 3 opciones para empezar | Baja |
| **P0-3** | Formulario de la boleta | Donde el usuario ingresa los datos de su multa: fecha, entidad, qué infracción, si manejaba | Media |
| **P0-4** | Explicador de multa | EL CORAZÓN: traduce la multa a lenguaje claro, muestra montos, descuentos, prevención | Media |
| **P0-5** | Semáforo de legalidad | Dice si la multa es válida (verde/amarillo/rojo) con checklist de Decreto 33-2024 | Alta |
| **P0-6** | Generador de PDF de impugnación | Crea un borrador listo para presentar, con checkbox obligatorio de disclaimer | Media |
| **P0-7** | PWA instalable | Que se pueda instalar como app en el celular y funcione offline | Baja |
| **P0-8** | Catálogo de entidades | JSON con las 11 entidades que multan, sus jurisdicciones y contactos | Baja |

**Resultado P0:** App completa que explica una multa, dice si es impugnable, y genera el PDF. **Esto es el 80% del pitch.**

### P1 — VALOR AÑADIDO (Si sobra tiempo)

| # | Qué es | Para qué sirve | Complejidad |
|---|--------|----------------|-------------|
| **P1-1** | Traducción a K'iche' + Inglés | Inclusión lingüística: 1.27M de k'iche'hablantes + turistas | Media |
| **P1-2** | Buscador de infracciones | El usuario escribe "semáforo" y le aparece la infracción | Baja |
| **P1-3** | Historial de consultas | Guarda las últimas consultas para no tener que repetir | Baja |
| **P1-4** | Guía de estafas | Cómo identificar multas falsas (problema real en Guatemala) | Baja |
| **P1-5** | Audio de frases clave | Para personas que hablan pero no leen su idioma | Media |
| **P1-6** | Guía "¿Dónde impugnar?" | Dirección, horario y documentos según la entidad | Baja |

### P2 — ESCALA / ROADMAP (Post-hackathon)

| # | Qué es | Para qué sirve |
|---|--------|----------------|
| **P2-1** | OCR de boletas | Fotografiar la boleta y que la app lea los datos sola |
| **P2-2** | Cálculo automático de prescripción | Que calcule solo si la multa ya prescribió |
| **P2-3** | Alertas push de plazos | Notificación "queda 1 día para impugnar" |
| **P2-4** | Dashboard de flota B2B | Para empresas de transporte que tienen muchos vehículos |
| **P2-5** | Temas CSS (oscuro/claro) | Que el usuario pueda elegir el tema |

---

## CÓMO FUNCIONA LA APP (flujo del usuario)

Así es como el usuario va a usar MultaClara:

```
PASO 1: El usuario llega a la app
   │
   ├─ "Tengo la boleta en la mano" → va al formulario
   ├─ "Escanear mi boleta" (V2, OCR) → foto de la boleta
   └─ "Solo quiero entender una infracción" → buscador
   │
   ▼
PASO 2: Ingresa los datos de su multa
   • Tipo de papel (boleta, requerimiento, citación)
   • Fecha de la infracción (CRÍTICO para plazos)
   • Entidad que multó (PNC, EMETRA, etc.)
   • Qué infracción le cobran
   • "¿Vos manejabas el vehículo?" (sí/no)
   │
   ▼
PASO 3: Ve la explicación de su multa
   • Qué hizo mal (en lenguaje claro)
   • Cuánto cuesta + cuánto ahorraría con descuento
   • Por qué es peligroso (cápsula de prevención)
   │
   ▼
PASO 4: Ve el semáforo de legalidad
   • 🟢 Verde: "Tu multa parece válida" → guía de pago con descuento
   • 🟡 Amarillo: "Revisá estos puntos" → sugiere verificar
   • 🔴 Rojo: "Podrías impugnar" → checklist + generador de PDF
   │
   ▼
PASO 5: Toma acción
   • Si rojo/amarillo: genera borrador de impugnación (PDF)
   • Si verde: ve cómo pagar con descuento
   • En cualquier caso: ve dónde ir, qué documentos llevar
```

---

## TECH STACK (con qué construimos)

| Tecnología | Para qué | Por qué |
|------------|----------|---------|
| **Vite** | Herramienta de desarrollo | Rápido, moderno, hot reload |
| **HTML + CSS + JavaScript puro** | La app en sí | Sin frameworks complicados, estable |
| **Tailwind CSS** | Estilos | Rápido de diseñar, responsive |
| **JSON** | Datos de infracciones y entidades | Simple, sin backend |
| **jsPDF** | Generar PDFs de impugnación | Funciona en el navegador |
| **Service Worker** | Modo offline (PWA) | Que funcione sin internet |
| **Cloudflare Pages** | Desplegar la app | Gratis, rápido, CDN global |

**Decisión clave:** NO usamos React, Vue, ni Angular. Es JavaScript puro. Esto es intencional: menos cosas que se rompan durante el hackathon.

---

## CRONOGRAMA (cómo repartimos el tiempo)

### DÍA 1 — CIMIENTOS
| Hora | Tarea | Responsable |
|------|-------|-------------|
| Mañana | Setup del proyecto Vite + estructura de carpetas + CSS base | Desarrollador 1 |
| Mañana | Crear `entidades.json` (catálogo de 11 entidades) | Investigador |
| Tarde | Pantalla de inicio (landing con 3 caminos) | Desarrollador 2 |
| Tarde | Service Worker + manifest.json (PWA) | Desarrollador 1 |
| Todo el día | Infraestructura de diseño: paleta de colores, tipografía, componentes | Diseñador |

### DÍA 2 — CORE
| Hora | Tarea | Responsable |
|------|-------|-------------|
| Mañana | Formulario de boleta con validación de fechas | Desarrollador 2 |
| Mañana | Cargar infracciones.json + lógica de datos | Desarrollador 1 |
| Tarde | Explicador de multa (lenguaje claro + montos + prevención) | Desarrollador 1 |
| Tarde | Lógica del semáforo de legalidad | Desarrollador 2 |
| Tarde | i18n: estructura + español completo | Investigador |

### DÍA 3 — DIFERENCIADORES
| Hora | Tarea | Responsable |
|------|-------|-------------|
| Mañana | Semáforo UI (tarjeta + checklist + barras de plazo) | Desarrollador 1 + Diseñador |
| Mañana | Generador de PDF de impugnación | Desarrollador 2 |
| Tarde | K'iche' (al menos UI + frases clave) | Investigador |
| Tarde | Deploy a Cloudflare Pages | Desarrollador 1 |
| Tarde | Tests manuales + fix de bugs | QA / Todos |

### DÍA 4 — PITCH + PULIDO
| Hora | Tarea | Responsable |
|------|-------|-------------|
| Mañana | Pulido visual + responsive final | Diseñador + Desarrolladores |
| Mañana | Guía de estafas + historial localStorage | Investigador + Desarrollador 2 |
| Tarde | Ensayo de pitch (5 min) | Líder de pitch |
| Tarde | Deploy final + verificación | Desarrollador 1 |
| Tarde | Preparar slides de presentación | Líder de pitch |

---

## LOS 5 ROLES DEL EQUIPO

Cada persona tiene un rol claro. Nadie se pisa, todos saben qué hacer.

### ROL 1: ARQUITECTO (Desarrollador Principal)
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Crea la estructura del proyecto (carpetas, archivos, configuración)
- Configura Vite, el Service Worker, el manifest.json
- Escribe la lógica core: cálculo de prescripción, semáforo, validación de fechas
- Despliega a Cloudflare Pages
- Decide las decisiones técnicas (qué librería usar, cómo organizar el código)

**Herramientas que usa:** Vite, JavaScript puro, Service Worker, Cloudflare Pages

**Entregables:**
- Proyecto funcionando en `npm run dev`
- Estructura de carpetas limpia
- Service Worker activo (offline)
- App desplegada en Cloudflare Pages

**Depende de:** Nadie (es el primero en empezar)

---

### ROL 2: FRONTEND (Constructor de Interfaces)
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Construye las pantallas: inicio, formulario, explicador, semáforo, PDF
- Escribe el HTML de cada vista
- Aplica los estilos CSS (tema oscuro, responsive, tarjetas)
- Hace que todo se vea bien en celular y en desktop
- Implementa las animaciones y transiciones

**Herramientas que usa:** HTML, CSS, Tailwind, SVGs

**Entregables:**
- Pantalla de inicio con 3 botones
- Formulario de boleta con validación visual
- Tarjeta de explicador con montos y prevención
- Tarjeta de semáforo con iconografía + barras de plazo
- Todo responsive (mobile-first)

**Depende de:** El Arquitecto le entrega la estructura base (Día 1)

---

### ROL 3: LÓGICA DE NEGOCIO (Motor de la App)
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Conecta el formulario con el explicador (si el usuario pone fecha X y entidad Y, muestra Z)
- Implementa la lógica del semáforo: lee los datos de la boleta y decide verde/amarillo/rojo
- Maneja el catálogo de infracciones (lectura del JSON, búsqueda, filtrado)
- Implementa el "¿Vos manejabas?" y la guía especial
- Maneja localStorage para el historial

**Herramientas que usa:** JavaScript, JSON, localStorage

**Entregables:**
- Función de semáforo (verde/amarillo/rojo según datos)
- Función de prescripción (120 días)
- Conexión formulario → explicador → semáforo → PDF
- Búsqueda de infracciones por palabra
- Historial en localStorage

**Depende de:** El Arquitecto le entrega `core.js` y `data.js` (Día 1-2)

---

### ROL 4: INVESTIGADOR / CONTENIDO
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Crea `entidades.json` con las 11 entidades, jurisdicciones, contactos, horarios
- Escribe el copy en lenguaje claro para las 15 infracciones
- Prepara las traducciones a K'iche' e Inglés (UI + frases clave)
- Crea la guía de estafas (notificación real vs SMS falso)
- Investigación adicional si el jurado pregunta por algo específico
- Prepara el guion del pitch

**Herramientas que usa:** Markdown, JSON, Google Translate / hablantes nativos

**Entregables:**
- `data/entidades.json` (11 entidades completas)
- Copy de las 15 infracciones en lenguaje claro
- Archivos de i18n: español, K'iche', Inglés
- Guía de estafas (contenido editorial)
- Guion de pitch de 5 minutos

**Depende de:** Nadie (puede empezar desde el Día 1)

---

### ROL 5: DISEÑADOR / QA / PITCH
**Quién lo lleva:** [Nombre del compañero]

**Qué hace:**
- Define la paleta de colores, tipografía, espaciados
- Diseña los componentes: tarjetas, botones, iconos SVG
- Revisa que todo se vea bien ( QA visual)
- Testea la app en diferentes pantallas (celular, tablet, desktop)
- Prepara las slides de la presentación
- Ensaya el pitch con el equipo
- Hace el deploy final y verificación

**Herramientas que usa:** CSS, SVG, Figma (si lo usan), presentaciones

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
| Setup proyecto Vite | **LÍDER** | Ayuda | — | — | — |
| Service Worker / PWA | **LÍDER** | — | — | — | Verifica |
| CSS / Estilos base | Ayuda | **LÍDER** | — | — | **LÍDER** |
| Pantalla inicio | — | **LÍDER** | — | — | Diseña |
| Formulario boleta | — | **LÍDER** | Conecta | — | Revisa |
| Explicador multa | — | **LÍDER** | **LÍDER** | Copy | Diseña |
| Semáforo legalidad | Ayuda | UI | **LÍDER** | — | Iconos |
| Generador PDF | — | Ayuda | **LÍDER** | — | Verifica |
| entidades.json | — | — | — | **LÍDER** | — |
| i18n (traducciones) | — | Conecta | — | **LÍDER** | — |
| Guía estafas | — | — | — | **LÍDER** | Diseña |
| Buscador infracciones | — | Ayuda | **LÍDER** | — | — |
| Historial localStorage | — | — | **LÍDER** | — | — |
| Deploy Cloudflare | **LÍDER** | — | — | — | Verifica |
| Pitch / Slides | — | — | — | Ayuda | **LÍDER** |
| QA / Tests manuales | Verifica | Revisa | Revisa | — | **LÍDER** |

---

## LO QUE EL JURADO VA A BUSCAR (y cómo lo cubrimos)

| Qué busca el jurado | Cómo lo mostramos | Quién lo prepara |
|---------------------|-------------------|------------------|
| **UX excepcional** | Tema oscuro, responsive, lenguaje claro | Diseñador + Frontend |
| **Tecnología aplicada** | PWA offline, PDF generation, i18n | Arquitecto + Lógica |
| **Impacto medible** | 6.7M+ vehículos, 53% motos, 4,233 accidentes | Investigador |
| **Inclusión** | K'iche' + Inglés, audio (V2) | Investigador |
| **Viabilidad** | Open source, Cloudflare Pages gratis, sin backend | Arquitecto |
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
