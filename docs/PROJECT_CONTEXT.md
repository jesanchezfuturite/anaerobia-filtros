# Contexto del Proyecto ANEFFY (Guía para AI)

Este documento ha sido creado para facilitar el entendimiento de la arquitectura, decisiones de diseño y estado actual del proyecto, permitiendo a futuras sesiones de IA tener el contexto necesario rápidamente sin tener que analizar todo el código.

## 1. Stack Tecnológico Principal
- **Framework:** Astro `^6.3.1` (Output: `static`).
- **Estilos:** Tailwind CSS `^4.3.0` integrado mediante `@tailwindcss/vite` directamente en Vite, sin archivos `tailwind.config.js`. Las variables y utilidades base están definidas en `src/styles/global.css` usando `@theme`.
- **Iconografía:** Phosphor Icons cargados por CDN en el `<head>`.
- **Animaciones:** GSAP y ScrollTrigger cargados por CDN en `Layout.astro` con inicialización segura que difiere la carga para no bloquear el hilo principal.

## 2. Decisiones de Diseño y UI (Sistema de Diseño B2B)
La landing page utiliza un diseño muy orientado al impacto visual "premium" y a la alta conversión.
- **Colores principales:**
  - Navy (Fondo profundo): `#003141` a `#001820`.
  - Lime (Acento llamativo, para botones, iconos y badges): `#9ac31c`.
  - Azure (Acento secundario o orbes): `#0c5b8c`.
- **Tipografías:** `Saira Condensed` (Headings en mayúsculas, negrita, alto impacto) y `Roboto Condensed` (Párrafos, descripciones y labels, para legibilidad limpia).
- **Glassmorphism & Bento Grids:** El diseño usa extensivamente tarjetas estilo "Bento box" (en `#ventajas` y `#especificaciones`) con fondos translúcidos `rgba(255,255,255,0.04)`, `backdrop-filter: blur(12px)` y bordes muy sutiles, dándoles una apariencia cristalina y moderna sobre los fondos oscuros.
- **Utilidades de Layout:** En lugar de reescribir clases, todo el contenido principal está contenido dentro de `.container-aneffy`, que tiene anchos máximos y paddings predefinidos en `global.css`.

## 3. Funcionalidades Clave y Scripts Vanilla

Debido a que Astro renderiza estáticamente y queremos máxima velocidad sin hidratar frameworks como React/Vue, se usa Vanilla JavaScript en línea (`<script is:inline>`) para la interactividad:

### 3.1. Slider / Galería de Imágenes
En la sección `#filtro-info`, hay una galería de 8 imágenes que transicionan mediante `transform: translateX(...)`.
- **Script:** Se encuentra directamente después del marcado del slider. Maneja auto-play (3.5s), pausa al `mouseenter`, e incluye controles de `prev`/`next` y puntos de navegación dinámicos.

### 3.2. Formulario de Contacto y Webhook
- En `#cotizar`, se recopilan datos B2B: Nombre, Correo, Teléfono, Empresa, y Mensaje.
- **Validación:** El frontend tiene validación en Vanilla JS (verificando longitud mínima, regex de email y teléfono).
- **Procesamiento:** Usa `fetch` para enviar un POST al webhook de **n8n** en `https://n8n.ongoing.mx/webhook/80c2332e-35dd-420a-a2d0-869bae808e11` con el header de seguridad `'aneffy': 'Huo0lpaw.'`.
- **Flujo final:** Tras recibir éxito (`response.ok`), redirige al usuario a la página `/gracias` (`gracias.astro`).

### 3.3. Animaciones GSAP
- Se utiliza la clase `gsap-hero-item` para elementos iniciales (Hero) que caen en cascada (Stagger).
- Se utiliza la clase `gsap-bento-card` con `ScrollTrigger` para revelar en scroll la cuadrícula de ventajas y las tarjetas de pasos.
- La navbar en `Layout.astro` tiene una animación en el primer load y añade box-shadow estáticamente cuando se hace scroll hacia abajo.

### 3.4. Chatbot (Conversia)
- En `Layout.astro` se inyecta el script de Conversia `CHATKODEX` con una inicialización atada al evento `load` de la ventana, usando la clave de acceso `IfcGlX07JF6cE`.

## 4. Notas sobre el Rendimiento
- **Imágenes:** Se usa `loading="eager"` solo para las imágenes en el *above the fold* (logo, hero image). El resto (slider) tiene `loading="lazy"`.
- **Scripts de GSAP:** GSAP está envuelto en una IIFE asíncrona que despacha un evento `gsap:ready` al completarse, para asegurar que los scripts de las páginas inicien las animaciones solo cuando los plugins están listos y no arrojen errores de referencia en Astro.

## 5. Cómo realizar futuros cambios
- **Si agregas nuevas secciones:** Usa `<section class="section-pad">` y el contenedor interior `<div class="container-aneffy">`.
- **Si agregas botones:** Usa la clase `.btn-primary` o `.btn-secondary` que ya están pre-estiladas en `global.css`.
- **Si modificas el formulario:** Recuerda que cualquier campo nuevo en el HTML debe ser recogido por `FormData` e integrado en la validación antes de hacer el `fetch` al webhook n8n.
- **No uses framework components (React/Vue/Svelte):** El proyecto sigue una filosofía "Zero-JS framework" aprovechando al máximo Astro + HTML + JS puro. No instales React a menos que sea un requerimiento forzoso de alta interactividad.
