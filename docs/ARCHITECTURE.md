# Arquitectura Técnica — ANEFFY

> Referencia interna para el equipo de desarrollo. Documenta las decisiones de arquitectura, patrones usados y razones detrás de cada elección.

---

## Diagrama de Arquitectura

```
┌─────────────────────────────────────────────────────────────────┐
│                         NAVEGADOR                               │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    Layout.astro                          │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │  <head>                                         │    │   │
│  │  │    Google Fonts (preconnect)                    │    │   │
│  │  │    Phosphor Icons (CDN)                         │    │   │
│  │  │    GSAP + ScrollTrigger (CDN, inline loader)    │    │   │
│  │  │    Tailwind v4 (compilado en build)             │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  │  ┌──────────┐  ┌──────────────┐  ┌──────────────────┐  │   │
│  │  │  Navbar  │  │  <slot />    │  │     Footer       │  │   │
│  │  │ (sticky) │  │ index.astro  │  │  (3 columnas)    │  │   │
│  │  └──────────┘  └──────────────┘  └──────────────────┘  │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘

BUILD (Astro SSG)
  src/pages/index.astro  →  dist/index.html  (HTML estático)
  src/styles/global.css  →  dist/_astro/*.css (Tailwind compilado)
```

---

## Decisiones de Arquitectura

### 1. `output: 'static'` (SSG puro)

**Decisión:** El sitio genera HTML 100% estático.  
**Razón:** El cliente requiere deploy en hosting tradicional (cPanel/FTP). No hay requerimientos de SSR en esta fase.  
**Implicación:** El formulario de cotización, cuando se implemente, requerirá una de estas opciones:
- Servicio externo (Brevo Forms, Typeform, n8n webhook)
- Migrar a `output: 'hybrid'` para agregar un endpoint API en `/api/cotizar.ts`

---

### 2. Tailwind CSS v4 vía `@tailwindcss/vite`

**Decisión:** Se usa el plugin Vite nativo de Tailwind v4, sin `tailwind.config.js`.  
**Razón:** Tailwind v4 elimina el archivo de configuración. Los tokens se definen en `@theme {}` dentro del CSS.  
**Archivo clave:** `src/styles/global.css`

```css
/* Sintaxis correcta Tailwind v4 */
@import "tailwindcss";

@theme {
  --color-navy: #003141;
  --color-lime: #9ac31c;
}
```

> ⚠️ **No crear** `tailwind.config.js` — es incompatible con v4 y rompe el build.

---

### 3. GSAP cargado vía CDN (no npm)

**Decisión:** GSAP y ScrollTrigger se cargan dinámicamente desde cdnjs en `Layout.astro`.  
**Razón:** Evita conflictos con el bundle de Astro. GSAP necesita estar en `window` para ser accesible desde múltiples `<script is:inline>`.  
**Patrón de carga:**

```js
// En Layout.astro <head>
// 1. Carga gsap.min.js
// 2. onload → carga ScrollTrigger.min.js
// 3. onload → registra plugin + dispara evento 'gsap:ready'

document.dispatchEvent(new CustomEvent('gsap:ready'));
```

**Consumo en páginas:**

```js
// En index.astro (o cualquier página)
document.addEventListener('gsap:ready', () => {
  // GSAP garantizado disponible aquí
  gsap.from('.elemento', { y: 50, opacity: 0 });
});

// Siempre agregar fallback:
setTimeout(initAnimations, 500);
```

---

### 4. Layout como única fuente de verdad global

`Layout.astro` centraliza **todo** lo que debe aparecer en cada página:
- Metadatos SEO (título, descripción, OG, canonical)
- Google Fonts
- Scripts CDN (GSAP, Phosphor)
- Navbar sticky
- Footer corporativo
- Scripts de interactividad global (menú mobile, scroll shadow)

**Props del Layout:**

```typescript
interface Props {
  title?: string;       // <title> de la página
  description?: string; // meta description
  ogImage?: string;     // ruta imagen Open Graph
}
```

---

### 5. Clases de animación GSAP como contratos

Las clases CSS prefijadas con `gsap-` son contratos entre el HTML y el JS:

| Clase | Dónde se aplica | Animación |
|---|---|---|
| `.gsap-hero-item` | Elementos del Hero | Entrada en cascada al cargar (stagger 0.2s) |
| `.gsap-bento-card` | Tarjetas Bento Grid y Cards de proceso | Fade-up al scroll (ScrollTrigger, start: 'top 85%') |

> **Convención:** Nunca usar estas clases para estilos visuales. Son exclusivas para targeting de GSAP.

---

### 6. CSS custom utilities vs clases Tailwind inline

Se prefieren **utilidades CSS custom** definidas en `global.css` sobre clases Tailwind largas repetidas:

```css
/* ✅ Correcto — definido en global.css */
.btn-primary { ... }
.container-aneffy { ... }
.glass-card { ... }

/* ⛔ Evitar — clases inline largas y repetidas */
class="inline-flex items-center gap-2 bg-lime text-navy font-saira..."
```

**Excepción:** Para estilos únicos de un componente específico, las clases Tailwind inline son aceptables.

---

## Flujo de Datos (Formulario — Pendiente)

```
Usuario llena formulario
        ↓
POST /api/cotizar.ts   ← Requiere output: 'hybrid'
        ↓
Valida campos + reCAPTCHA
        ↓
Brevo SMTP API  →  Email a CONTACT_EMAIL_TO
        ↓
Response JSON { success: true }
        ↓
Feedback visual en el formulario
```

---

## Historial de Cambios Técnicos

| Fecha | Cambio | Autor |
|---|---|---|
| 2026-05-12 | Setup inicial: Astro + Tailwind v4 + Layout base | Equipo Anaerobia |
| 2026-05-12 | Landing completa: Hero + Bento Grid + GSAP | Equipo Anaerobia |
