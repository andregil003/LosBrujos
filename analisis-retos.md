# Análisis Riguroso de Retos — HACKCREA 2026

> **Propósito:** quejas reales documentadas + alcance + stack por reto, pensando en TODA la población guatemalteca (incluyendo pueblos mayas y garífunas).

---

## Reto 05 · Te llegó una multa ¿y ahora?

**Ministerio de Gobernación · Departamento de Tránsito**

### Quejas y problemas reales documentados

| # | Problema | Fuente | Fecha |
|---|---|---|---|
| 1 | **"Multas fantasmas"**: conductores quejándose de que no se les notifica la multa y no pueden ejercer su derecho de defensa dentro de los plazos | Prensa Libre — reformas a Ley de Tránsito (Decreto 33-2024) | Dic 2024 |
| 2 | **Estafas por SMS**: personas reciben mensajes falsos de "multas pendientes" haciéndose pasar por PMT/EMETRA — la gente no sabe distinguir una notificación real de una estafa | Prensa Libre | Feb 2026 |
| 3 | **Fragmentación total**: hay multas de PNC, EMETRA, EMIXTRA, Villa Nueva, Santa Catarina Pinula, Palencia, San Lucas, Amatitlán, Escuintla, Antigua, Jutiapa… cada una con su portal | Portal SAT (multas) | 2025 |
| 4 | **Desconocimiento de beneficios**: MuniGuate condona 50-60% de multas con curso vial, pero casi nadie lo sabe | La Hora / Guatemala.com | Nov 2025 / Mar 2026 |
| 5 | **Lenguaje legal incomprensible**: el papel no dice claro qué hiciste, cuánto pagas ni qué pasa si no estás de acuerdo | El propio reto | — |
| 6 | **Plazos críticos que la gente no conoce**: 15 días para impugnar · 120 días prescripción si no te notifican · 60 días para pagar tras resolución | Decreto 33-2024 | 2024 |

### Datos clave del trámite (para el prototipo)

| Campo | Valor |
|---|---|
| Plazo para impugnar | **15 días** desde la notificación |
| Prescripción (no notificada) | **120 días** |
| Pago tras resolución desfavorable | **60 días** |
| Condonación MuniGuate | 50-60% con curso vial (EMETRA) |
| Consulta de multas | Portal SAT por placa + NIT |
| Instituciones emisoras | PNC, EMETRA, EMIXTRA + 8+ municipalidades |

### Alcance del prototipo

1. **Selector de tipo de papel**: boleta / requerimiento de pago / citación
2. **Explicador en lenguaje simple**: qué te cobran, por qué, con qué pruebas
3. **¿Puedes reclamar?**: árbol de decisión (¿fuiste tú? ¿te notificaron? ¿pasó el plazo?)
4. **Plazos visuales**: semáforo (verde/amarillo/rojo) con días restantes
5. **Detección de estafas**: guía para distinguir notificación real vs SMS falso
6. **Beneficios que existen**: condonaciones, cursos viales

### Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Frontend | Vite + React + shadcn + Tailwind | Rápido, componentes reutilizables, demo estable |
| Datos | JSON local (catálogo + plazos + instituciones) | Funciona sin backend — cero riesgo en demo |
| i18n | JSON por idioma (es, k'iche', q'eqchi', garífuna) | Inclusión lingüística |
| Audio | Grabaciones 🔊 (HTML audio) | Para quienes hablan pero no leen su idioma |
| AI Agent (opcional) | n8n + Claude API | Explicar el texto de la boleta en lenguaje simple |
| Deploy | Cloudflare Pages | Gratis, rápido, CDN |

### Inclusión lingüística — por qué pega aquí

- El reto **menciona explícitamente** "quien no tiene el español como primera lengua" → es el único reto que lo pide.
- Plataformas oficiales (PNC, SAT) son **solo en español** — documentado por Canada.ca.
- Personas mayores mayahablantes que reciben una boleta en lenguaje legal → **no entienden ni el papel ni el plazo**.
- Garífuna: comunidades de Izabal/Livingston donde el español es segunda lengua.

---

## Reto 08 · Licencia sanitaria

**Ministerio de Salud Pública y Asistencia Social (MSPAS)**

### Quejas y problemas reales documentados

| # | Problema | Fuente | Fecha |
|---|---|---|---|
| 1 | **"La confusión más costosa"**: emprendedores piensan que basta una licencia y descubren en la aduana que necesitan **tres capas** (licencia de establecimiento + registro por producto + patente) | livinginguatemala.com | May 2026 |
| 2 | **Cadena de 5 permisos obligatorios**: SAT NIT → Registro Mercantil → IGSS → MSPAS → patente municipal — nadie lo explica antes de invertir | livinginguatemala.com | May 2026 |
| 3 | **Requisitos cambiantes**: "Los requisitos están sujetos a cambios según actualizaciones de Normas Técnicas" — la info se desactualiza | Catálogo de Trámites (DRCPFA) | 2026 |
| 4 | **3 departamentos distintos**: DRACES (servicios de salud), DRCPFA (farmacéuticos), DRCA (alimentos) — cada uno con sus formularios y normas | MSPAS | — |
| 5 | **Costos que varían Q90–Q5,400** según tipo de establecimiento — imposible saber cuál te toca sin leer normas técnicas | Catálogo de Trámites | 2026 |
| 6 | **El reto lo dice**: los requisitos se descubren **después** de alquilar y adaptar el local | El propio reto | — |

### Datos clave del trámite (para el prototipo)

| Campo | Valor |
|---|---|
| Costo | Q90 – Q5,400 (según tipo de establecimiento) |
| Vigencia | 5 años |
| Tiempo de respuesta | 10–15 días (DRACES) · 30 días renovación (DRCPFA) |
| Tipos de establecimiento | Farmacia, droguería, clínica, laboratorio, venta de medicina, naturista, distribuidora, restaurante… |
| Documentos típicos | F-AS-f-01, autoinspección F-AS-c-01/02/03, director técnico colegiado, croquis, patente, DPI, RTU |

### Alcance del prototipo

1. **Quiz de 4 preguntas**: ¿qué quieres abrir? (farmacia/clínica/restaurante…) → ¿apertura/renovación/traslado? → ¿dónde? → ¿tienes director técnico?
2. **Resultado**: lista de requisitos exactos + costo + tiempo + a qué departamento ir (DRACES/DRCPFA/DRCA)
3. **Checklist "antes de firmar el contrato"**: qué debe tener el local (croquis, condiciones sanitarias, director técnico)
4. **Mapa de la cadena completa**: SAT → Registro Mercantil → IGSS → MSPAS → patente municipal — para que nadie descubra las capas tarde

### Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Frontend | Vite + React + shadcn + Tailwind | Quiz + checklist, sin backend |
| Datos | JSON local (tipos de establecimiento + requisitos + costos del catálogo) | Datos reales verificables |
| i18n | JSON por idioma | Inclusión lingüística |
| Audio | Grabaciones 🔊 | Emprendedores que no leen su idioma |
| Deploy | Cloudflare Pages | Gratis |

### Inclusión lingüística — por qué pega aquí

- El reto dice **"sobre todo fuera de la capital"** → departamentos con alta población maya (Alta Verapaz, Quiché, Totonicapán, Sololá).
- **Cooperativas y grupos de productores** (Reto 07) son mayormente indígenas — el Reto 08 tiene el mismo público emprendedor.
- Un emprendedor q'eqchi' que quiere abrir su farmacia en Cobán: necesita saber los requisitos **en su idioma** antes de invertir.
- MSPAS tiene una **Unidad de Atención de la Salud de los Pueblos Indígenas e interculturalidad** — hay un precedente institucional que valida el enfoque.

---

## Reto 01 · Antecedentes policiacos

**Ministerio de Gobernación · PNC**

### Quejas y problemas reales documentados

| # | Problema | Fuente | Fecha |
|---|---|---|---|
| 1 | **Sistema caído / irregular**: "Trámite de antecedentes policiacos es irregular por fallas en el sistema" — largas filas, tiempo de espera, gente que llega y no puede hacer el trámite | Prensa Libre | Jul 2019 |
| 2 | **Costo Q30 golpea a quienes buscan empleo**: iniciativa 6696 propone gratuidad — "la mayoría de las personas que los solicitan son ciudadanos de escasos recursos que se encuentran en la búsqueda de empleo" | Congreso de Guatemala | Ene 2026 |
| 3 | **Pago en línea roto**: "En algunos bancos aún aparece como alternativa, aunque el pago no se procesa" — solo funciona Banrural | Prensa Libre | Ago 2025 |
| 4 | **Se exige en TODOS lados**: empleo, alquilar/comprar inmueble, trámites legales, migración (Canadá exige ambos documentos) | AGN / Canada.ca | 2025 |
| 5 | **Vigencia de 6 meses obliga a repetir**: cada constancia cuesta Q30 y dura 6 meses — quien busca trabajo la saca varias veces al año | Acuerdo Gubernativo 149-2020 | 2020 |
| 6 | **Plataforma solo en español**: "Apply online (available only in Spanish)" | Canada.ca | — |

### Datos clave del trámite (para el prototipo)

| Campo | Valor |
|---|---|
| Costo | Q30.00 |
| Vigencia | **6 meses** |
| Emisor | PNC (policiales) + OJ (penales) — dos constancias distintas |
| En línea | policiales.pnc.gob.gt (solo español) |
| Pago | Banrural (en línea) / Banrural, BI, Banco de los Trabajadores (presencial) |
| Validación existente | Código QR en policiales.pnc.gob.gt/validacion |
| Se exige para | Empleo, alquiler/compra de inmueble, trámites legales, migración |

### Alcance del prototipo

1. **Registrar la constancia**: la persona ingresa datos de su constancia ya obtenida (número, fecha de emisión)
2. **Recibir un código**: código único de verificación (hash)
3. **Verificar vigencia**: cualquiera puede comprobar que la constancia existe y sigue vigente (6 meses) sin ver el contenido
4. **Billetera de constancias**: el usuario guarda sus constancias y ve cuáles están vigentes / cuáles vencen pronto
5. **Lista de trámites que la exigen**: cuántos trámites del catálogo piden antecedentes como requisito (dato del reto)

### Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Frontend | Vite + React + shadcn + Tailwind | Registro + verificación |
| Lógica de códigos | JS puro (hash + fecha de vigencia) | Sin backend — el código se genera y verifica localmente |
| Persistencia | localStorage | El usuario guarda sus constancias en su dispositivo |
| Datos | JSON local (costo, vigencia, sedes, trámites que lo exigen) | Datos reales del catálogo |
| i18n | JSON por idioma | Inclusión lingüística |
| Audio | Grabaciones 🔊 | Personas que no leen su idioma |
| Deploy | Cloudflare Pages | Gratis |

### Inclusión lingüística — por qué pega aquí

- La plataforma PNC es **solo en español** (documentado por Canada.ca) → una persona mayahablante que busca empleo no puede usar el portal en línea y tiene que ir presencial (fila + día perdido).
- El costo Q30 + fila + repetir cada 6 meses golpea más a quienes buscan empleo en comunidades rurales.
- El código de verificación es **universal** (no depende del idioma) → la verificación funciona para todos, pero la UI de registro necesita idiomas + audio.

---

## Comparativa final

| Criterio | 05 Multas | 08 Licencia sanitaria | 01 Antecedentes |
|---|---|---|---|
| Quejas documentadas | 🔥 6 fuertes | 🔥 6 fuertes | 🔥 6 fuertes |
| Dolor universal | Alto (todos manejan) | Medio (emprendedores) | Alto (todos buscan empleo) |
| Inclusión lingüística | 🔥 Explícita en el reto | Alta (fuera de capital) | Alta (portal solo español) |
| Complejidad técnica | Baja-media | Baja | Media |
| Demo ante jurado | 🔥 Instantánea | ✅ Clara | ✅ Técnica |
| Datos verificables | ✅ (Decreto 33-2024) | ✅ (catálogo MSPAS) | ✅ (Q30, 6 meses) |
| Riesgo de demo | Bajo (sin backend) | Bajo (sin backend) | Bajo (localStorage) |

## Stack común (los 3 retos)

```
Frontend Vite + React + shadcn + Tailwind
├── JSON local (datos del catálogo por reto)
├── i18n: es / k'iche' / q'eqchi' / garífuna
├── Audio 🔊 (grabaciones de frases clave)
├── Iconografía universal + semáforo de plazos
└── Deploy: Cloudflare Pages

Opcional (solo Reto 05):
└── n8n + Claude API → AI Agent "explica mi boleta"
```

**Prioridad de idiomas:** K'iche' (~1.27M) y Q'eqchi' (~1.37M) son los más hablados; garífuna es el símbolo de la costa caribe (Izabal/Livingston). Prototipo: español + k'iche' + q'eqchi' + garífuna.

**Fuente legal para el ángulo lingüístico:** Decreto 19-2003 (Ley de Idiomas Nacionales) Art. 9 — "las leyes, instituciones, avisos, disposiciones… deberán traducirse y divulgarse en los idiomas Mayas, Garífuna y Xinka". Nuestro prototipo **cumple la ley que el Estado no cumple** → argumento potente ante el jurado.