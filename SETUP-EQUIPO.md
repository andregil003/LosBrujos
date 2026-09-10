# SETUP DEL EQUIPO — LosBrujos · HACKCREA 2026

> **Lee esto ANTES de empezar.** Explica todo lo que necesitás para trabajar con el equipo hoy.
> Si tenés una duda, preguntale a André por el grupo.

---

## 1. TUS ROLES (quién hace qué)

| # | Nombre | Rol | Qué hace |
|---|--------|-----|----------|
| 1 | **André** | **Arquitecto** (líder) | Setup del proyecto, lógica core, deploy, decisiones técnicas |
| 2 | **Kevin** | **Frontend** | Construye las pantallas con React + shadcn, responsive, animaciones |
| 3 | **Lemus** | **Lógica de Negocio** | Conecta las piezas: formulario → explicador → semáforo → PDF |
| 4 | **Diego** | **Investigador** | entidades.json, copy de infracciones, traducciones K'iche', guía de estafas |
| 5 | **Uriel** | **Diseñador / QA / Pitch** | Paleta de colores, iconos SVG, tests visuales, slides, pitch |

**Cada uno trabaja en SU rama (branch).** Nadie toca `main` directamente.

---

## 2. REPOSITORIOS

Tenemos **2 repos**. Esto es importante:

| Repo | Para qué | Quién lo usa |
|------|----------|--------------|
| **[andregil003/LosBrujos](https://github.com/andregil003/LosBrujos)** | Investigación, datos, plan de trabajo, diagramas | Todo el equipo (lee) |
| **[ithackathonnacionalgt/reto-5-brujos](https://github.com/ithackathonnacionalgt/reto-5-brujos)** | Código de la app (React, componentes, entregables) | Todo el equipo (escribe) |

**REGLA:** La info de investigación se queda en `LosBrujos`. El código de la app va en `reto-5-brujos`.

---

## 3. INSTALAR ntfy (notificaciones del equipo)

ntfy es una app gratis que nos avisa cuando alguien termina una tarea o necesita algo. **No necesitás saber programar para usarla.**

### Paso 1: Descargá la app

| Plataforma | Link |
|------------|------|
| **Android** | https://play.google.com/store/apps/details?id=io.heckel.ntfy |
| **iPhone** | https://apps.apple.com/app/ntfy/id1625396347 |

### Paso 2: Suscribite al tópico

1. Abrí la app
2. Tocá el botón **+** (arriba a la derecha)
3. Escribí este nombre exacto: **`losbrujos-hackcrea-2026`**
4. Tocá **Subscribe**

Listo. Ya vas a recibir notificaciones cuando André mande tareas.

### Paso 3: Probá que funciona

André va a mandar un mensaje de prueba al grupo. Si lo ves en la app, está todo bien.

---

## 4. INSTALAR GIT

Git es el programa que nos permite trabajar en el mismo código sin pisarnos.

### Windows

1. Entrá a https://git-scm.com/download/win
2. Descargá el instalador (64-bit)
3. Ejecutalo — podés darle "Next" a todo (dejá las opciones por defecto)
4. Para verificar que se instaló: abrí **PowerShell** y escribí:
   ```
   git --version
   ```
   Si te dice algo como `git version 2.xx.x`, está bien.

### Mac

1. Abrí la **Terminal** ( Spotlight → escribe "Terminal")
2. Escribí:
   ```
   git --version
   ```
3. Si te pide instalar las "Command Line Tools", dale **Install** y esperá
4. Si ya lo tenías, te va a mostrar la versión

### Configurá tu nombre (una sola vez)

En la terminal / PowerShell, escribí (reemplazá con tu nombre real):

**Windows:**
```
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

**Mac:**
```
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

---

## 5. CLONEAR EL REPO DE ENTREGABLES

Este es el repo donde vamos a poner TODO el código de la app. **Hacé esto una sola vez.**

### Windows (PowerShell)

```
cd C:\Users\[tu-usuario]\Documents
git clone https://github.com/ithackathonnacionalgt/reto-5-brujos.git
cd reto-5-brujos
npm install
```

### Mac (Terminal)

```
cd ~/Documents
git clone https://github.com/ithackathonnacionalgt/reto-5-brujos.git
cd reto-5-brujos
npm install
```

**Listo.** Ya tenés el repo en tu computadora con todas las dependencias instaladas.

### Probar que todo funciona

```
npm run dev
```

Abrí el link que te muestre (algo como `http://localhost:5173`) en tu navegador. Si ves la app, está todo bien.

---

## 6. CREAR TU RAMA (branch) — MUY IMPORTANTE

Cada uno trabaja en una rama separada para no pisar el trabajo del otro. **Tu rama se llama con tu nombre y tu rol.**

### Windows (PowerShell)

```
git checkout -b [tu-nombre]-[tu-rol]
```

### Mac (Terminal)

```
git checkout -b [tu-nombre]-[tu-rol]
```

### Ejemplos exactos:

| Nombre | Comando exacto |
|--------|----------------|
| **Kevin** (Frontend) | `git checkout -b kevin-frontend` |
| **Lemus** (Lógica) | `git checkout -b lemus-logica` |
| **Diego** (Investigador) | `git checkout -b diego-investigador` |
| **Uriel** (Diseñador) | `git checkout -b uriel-disenador` |

**Si hacés esto bien, tu rama se va a llamar algo como `kevin-frontend`.** Verificalo con:

```
git branch
```

Debe aparecer con un asterisco (*) tu rama.

---

## 7. INSTALAR NODE.JS (necesario para la app)

La app usa Vite, que necesita Node.js.

### Windows

1. Entrá a https://nodejs.org
2. Descargá la versión **LTS** (la que dice "Recommended")
3. Ejecutá el instalador — Next a todo
4. Verificá en PowerShell:
   ```
   node --version
   npm --version
   ```

### Mac

```
brew install node
```

O descargalo de https://nodejs.org (versión LTS).

Verificá:
```
node --version
npm --version
```

---

## 8. INSTALAR OPENDCODE (opcional — para trabajar con IA)

opencode es una herramienta de IA que te ayuda a programar. **No es obligatorio**, pero te va a ahorrar mucho tiempo.

### Windows

1. Descargá de https://opencode.ai (o instalá con npm):
   ```
   npm install -g opencode
   ```
2. Configurá tu API key (pedile a André la key):
   ```
   opencode config
   ```

### Mac

```
npm install -g opencode
opencode config
```

### Configuración para el proyecto

Una vez que tengas opencode instalado y el repo clonado, entrá a la carpeta del proyecto y abrí opencode:

```
cd reto-5-brujos
opencode
```

opencode va a leer el proyecto y te va a poder ayudar con tus tareas.

---

## 9. FLUJO DE TRABAJO (cómo trabajamos juntos)

Así es como funciona el equipo durante el hackathon:

```
1. André manda la tarea por ntfy
   ↓
2. Vos la recibís en tu celular
   ↓
3. Trabajás en TU rama (ej: kevin-frontend)
   ↓
4. Cuando terminás, hacés commit:
   git add .
   git commit -m "feat: hice tal cosa"
   ↓
5. Avisás por ntfy que terminaste
   ↓
6. André revisa y hace merge a main
```

### Comandos que vas a usar todo el día

**Ver qué cambios tenés sin commitear:**
```
git status
```

**Guardar tus cambios (commit):**
```
git add .
git commit -m "feat: descripción breve de lo que hiciste"
```

**Subir tu rama a GitHub:**
```
git push origin [tu-rama]
```
Ejemplo: `git push origin kevin-frontend`

**Bajar los cambios que otros subieron:**
```
git pull origin main
```

**Cambiar entre ramas:**
```
git checkout [nombre-de-la-rama]
```

---

## 10. QUÉ HACE CADA ROL (resumen rápido)

### Kevin — Frontend
- Creá los componentes React de cada pantalla (Home, Form, Explicador, Semaforo, Accion)
- Usá los componentes de shadcn (Button, Card, Select, Input, Checkbox, Tabs)
- Aplicá Tailwind para responsive y tema oscuro
- Hacé que se vea bien en celular
- **Tu rama:** `kevin-frontend`

### Lemus — Lógica de Negocio
- Conectá el formulario con el explicador (props de React)
- Implementá la lógica del semáforo (verde/amarillo/rojo) en `core.js`
- Manejá el JSON de infracciones
- Generá el PDF con jsPDF
- **Tu rama:** `lemus-logica`

### Diego — Investigador
- Creá `entidades.json` con las 11 entidades
- Escribí el copy en lenguaje claro para cada infracción
- Prepará las traducciones a K'iche'
- Creá la guía de estafas
- **Tu rama:** `diego-investigador`

### Uriel — Diseñador / QA / Pitch
- Definí la paleta de colores en Tailwind (theme en `tailwind.config.js`)
- Creá los iconos SVG (semáforo, check, warning)
- Testeá la app en diferentes pantallas
- Prepará las slides y ensayá el pitch
- **Tu rama:** `uriel-disenador`

---

## 11. CHECKLIST RÁPIDO (¿estás listo?)

- [ ] Descargué ntfy y me suscribí a `losbrujos-hackcrea-2026`
- [ ] Instalé Git
- [ ] Cloné el repo `reto-5-brujos`
- [ ] Corrí `npm install` para instalar dependencias
- [ ] Corrí `npm run dev` y vi la app en el navegador
- [ ] Creé mi rama con mi nombre y rol
- [ ] Instalé Node.js
- [ ] (Opcional) Instalé opencode
- [ ] Hice un commit de prueba y lo subí

**Si todo esto está checked, ya podés empezar a chambear.** 🧙‍♂️

---

## 12. DUDAS COMUNES

**"¿Y si me equivoqué en la rama?"**
No pasa nada. Pedile ayuda a André.

**"¿Puedo editar archivos directo en GitHub?"**
Sí, pero no es ideal. Mejor trabajá en tu computadora y subí con git.

**"¿Qué pasa si hago push a main sin querer?"**
André lo va a ver. No es el fin del mundo, pero tratá de trabajar en tu rama.

**"¿Necesito internet para todo esto?"**
- Para clonar el repo: sí
- Para hacer push/pull: sí
- Para trabajar en la app: no (una vez que la tengas clonada)

---

> **Recordatorio:** El repo `LosBrujos` es solo de referencia (investigación, datos, plan). Todo el código de la app va en `reto-5-brujos`. Si necesitás un dato de la investigación, abrí `LosBrujos` en GitHub y copialo.
>
> ¡A chambear! 🧙‍♂️
