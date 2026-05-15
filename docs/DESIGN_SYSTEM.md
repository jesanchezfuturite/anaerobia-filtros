# Sistema de Diseño — ANEFFY

> Referencia de tokens, colores, tipografía y componentes del proyecto.

---

## Paleta de Colores

Definidos en `src/styles/global.css` dentro de `@theme {}`.

| Token | Hex | Uso |
|---|---|---|
| `--color-navy` | `#003141` | Fondo principal |
| `--color-lime` | `#9ac31c` | Acento primario, CTAs |
| `--color-azure` | `#0c5b8c` | Acento secundario, bordes |
| `--color-gray-muted` | `#8da8b8` | Texto secundario |
| `--color-navy-90` | `#00364a` | Variante navy clara |
| `--color-lime-90` | `#a8d31f` | Hover del lime |

### Gradientes de Sección

```css
/* Hero / impares */
background: linear-gradient(135deg, #003141 0%, #00243a 55%, #001820 100%);
/* CTA Final */
background: linear-gradient(135deg, #003141 0%, #0c5b8c 100%);
/* Footer */
background: #001e2e;
/* Clase utilitaria */
.section-gradient { background: linear-gradient(160deg, var(--color-navy) 0%, #00243a 60%, #001e2e 100%); }
```

---

## Tipografía

| Token | Fuente | Pesos | Uso |
|---|---|---|---|
| `--font-saira` | Saira Condensed | 600, 700 | Títulos, CTAs, labels |
| `--font-roboto` | Roboto Condensed | 300, 400, 700 | Cuerpo, párrafos, UI |

### Escala Hero (clamp responsivo)
```html
<!-- H1 -->
<h1 style="font-size: clamp(2.8rem, 7vw, 5.5rem); line-height: 1.0;">
<!-- H2 sección -->
<h2 style="font-size: clamp(2rem, 4vw, 3.25rem);">
```

---

## Componentes

### `.btn-primary`
Fondo lime, texto navy, `hover:scale(1.05)` + glow.
```html
<a href="#cotizar" class="btn-primary text-lg px-8 py-4">
  <i class="ph ph-package" aria-hidden="true"></i>
  Cotizar Refacciones
</a>
```

### `.btn-secondary`
Outline lime, `hover:bg-lime hover:text-navy`.
```html
<a href="#specs" class="btn-secondary text-lg px-8 py-4">
  Ver Especificaciones
</a>
```

### `.container-aneffy`
`max-width: 1280px`, padding `1.5rem` mobile / `2.5rem` desktop.

### Eyebrow / Badge de Sección
```html
<div class="flex items-center gap-3">
  <span class="w-8 h-px" style="background:#9ac31c;" aria-hidden="true"></span>
  <span class="font-roboto text-xs tracking-[0.25em] uppercase font-semibold" style="color:#9ac31c;">
    Etiqueta
  </span>
</div>
```

### Glassmorphism Card (Bento Grid)
```html
<article style="background:rgba(255,255,255,0.04); backdrop-filter:blur(12px); border:1px solid rgba(255,255,255,0.09);" class="rounded-2xl p-8">
```

---

## Clases GSAP (contratos JS)

> No usar para estilos visuales.

| Clase | Animación |
|---|---|
| `.gsap-hero-item` | Entrada en cascada al cargar (stagger 0.2s, `power3.out`) |
| `.gsap-bento-card` | Fade-up al scroll (ScrollTrigger, `start: 'top 85%'`) |

---

## Iconografía — Phosphor Icons (CDN)

```html
<i class="ph ph-[nombre]" aria-hidden="true"></i>
```

Íconos usados: `ph-chat-circle-dots`, `ph-package`, `ph-file-text`, `ph-magnifying-glass`, `ph-truck`, `ph-envelope-simple`, `ph-phone`, `ph-map-pin`, `ph-lightning`, `ph-shield-check`, `ph-whatsapp-logo`.

---

## Bento Grid — Estructura Desktop

```
┌──────────────────────────┬─────────────┐
│ Libre de Aranceles       │ Stock       │
│ (col-span-2, 280px)      │ Inmediato   │
├──────────────────────────┴─────────────┤
│ Rendimiento OEM Garantizado (col-span-3)│
│ [texto] ──────── [tags de especificación]│
└────────────────────────────────────────┘
```

---

## Sombras y Tokens

```css
--shadow-glass: 0 8px 32px rgba(0, 0, 0, 0.25);
--shadow-glow:  0 0 24px rgba(154, 195, 28, 0.35);
--radius-sm: 2px;  --radius-md: 6px;  --radius-lg: 12px;
```
