# ANEFFY — Filtros Industriales B2B
> Landing page B2B de alta conversión para la línea de filtros industriales **ANEFFY** de Anaerobia.

[![Astro](https://img.shields.io/badge/Astro-6.x-FF5D01?logo=astro&logoColor=white)](https://astro.build)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38BDF8?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Output: Static](https://img.shields.io/badge/Output-Static-4CAF50)](https://docs.astro.build/en/guides/deploy/)

---

## Tabla de Contenidos

- [Descripción del Proyecto](#descripción-del-proyecto)
- [Stack Tecnológico](#stack-tecnológico)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Inicio Rápido](#inicio-rápido)
- [Variables de Entorno](#variables-de-entorno)
- [Comandos Disponibles](#comandos-disponibles)
- [Sistema de Diseño](#sistema-de-diseño)
- [Deploy](#deploy)
- [Roadmap](#roadmap)

---

## Descripción del Proyecto

ANEFFY es la línea de refacciones y filtros industriales originales de **Anaerobia**. Este sitio es una landing page B2B orientada a conversión, dirigida a responsables de compras, ingenieros de planta y gerentes de mantenimiento en la industria manufacturera mexicana.

**Propuesta de valor principal:**
- Filtros OEM 100% compatibles con equipos Anaerobia
- Producción nacional → sin aranceles de importación
- Stock disponible → entrega 24-48h en territorio nacional

---

## Stack Tecnológico

| Tecnología | Versión | Propósito |
|---|---|---|
| [Astro](https://astro.build) | `^6.3.1` | Framework principal (output estático) |
| [Tailwind CSS](https://tailwindcss.com) | `^4.3.0` | Estilos vía plugin Vite |
| [@tailwindcss/vite](https://tailwindcss.com/docs/installation/framework-guides/astro) | `^4.3.0` | Integración Tailwind v4 con Vite |
| [GSAP](https://gsap.com) | `3.12.5` | Animaciones (cargado vía CDN) |
| [ScrollTrigger](https://gsap.com/docs/v3/Plugins/ScrollTrigger/) | `3.12.5` | Animaciones al hacer scroll (CDN) |
| [Phosphor Icons](https://phosphoricons.com) | `latest` | Iconografía (cargado vía CDN) |
| Google Fonts | — | Saira Condensed + Roboto Condensed |

---

## Estructura del Proyecto

```
anaerobia-filtros/
├── public/
│   └── favicon.svg              # Favicon SVG con colores ANEFFY
├── src/
│   ├── layouts/
│   │   └── Layout.astro         # Layout base: Navbar + Footer + Scripts globales
│   ├── pages/
│   │   └── index.astro          # Página principal (landing completa)
│   └── styles/
│       └── global.css           # Sistema de diseño: tokens @theme + utilidades
├── .env.example                 # Template de variables de entorno (seguro para Git)
├── .gitignore                   # Excluye node_modules, dist, .env, .astro
├── astro.config.mjs             # Configuración Astro: output static + Tailwind Vite
├── package.json
├── tsconfig.json
└── docs/
    ├── ARCHITECTURE.md          # Decisiones técnicas y arquitectura
    ├── DESIGN_SYSTEM.md         # Tokens de diseño, colores, tipografía
    ├── CONTRIBUTING.md          # Guía para colaboradores
    └── ROADMAP.md               # Secciones pendientes y backlog
```

---

## Inicio Rápido

### 1. Clonar e instalar

```bash
git clone <url-del-repositorio> anaerobia-filtros
cd anaerobia-filtros
npm install
```

### 2. Configurar variables de entorno

```bash
cp .env.example .env
# Edita .env con los datos SMTP reales
```

### 3. Iniciar servidor de desarrollo

```bash
npm run dev
# → http://localhost:4321
```

---

## Variables de Entorno

Copia `.env.example` como `.env` (nunca subas `.env` al repositorio):

```env
CONTACT_EMAIL_TO=ventas@anaerobia.com
CONTACT_EMAIL_FROM=noreply@anaerobia.com
SMTP_HOST=smtp-relay.brevo.com
SMTP_PORT=587
SMTP_USER=tu_usuario
SMTP_PASS=tu_contraseña
BREVO_API_KEY=xkeysib-...
PUBLIC_SITE_URL=https://www.aneffy.com
```

Ver [`docs/CONTRIBUTING.md`](./docs/CONTRIBUTING.md) para detalles completos.

---

## Comandos Disponibles

| Comando | Acción |
|---|---|
| `npm run dev` | Inicia servidor de desarrollo en `localhost:4321` |
| `npm run build` | Genera build de producción en `./dist/` |
| `npm run preview` | Vista previa del build local |

---

## Sistema de Diseño

Ver [`docs/DESIGN_SYSTEM.md`](./docs/DESIGN_SYSTEM.md) para la referencia completa de tokens, colores y componentes.

**Paleta rápida:**
- `--color-navy: #003141` — Fondo principal
- `--color-lime: #9ac31c` — Acento / CTA
- `--color-azure: #0c5b8c` — Acento secundario

---

## Deploy

El sitio genera **HTML estático** (`output: 'static'`) compatible con cualquier hosting tradicional:

- **cPanel / Hosting compartido**: Sube el contenido de `./dist/` vía FTP
- **Vercel / Netlify**: Conecta el repositorio, build command `npm run build`, output dir `dist`
- **VPS / Apache / Nginx**: Apunta el document root a `./dist/`

> ⚠️ Si en el futuro se requiere el formulario de cotización con envío de email, el proyecto deberá migrar a `output: 'hybrid'` o `'server'` para habilitar endpoints API. Ver [`docs/ROADMAP.md`](./docs/ROADMAP.md).

---

## Roadmap

Ver [`docs/ROADMAP.md`](./docs/ROADMAP.md) para el backlog completo de funcionalidades.

---

**Desarrollado por el equipo Anaerobia · 2026**
