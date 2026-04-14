---
name: "layout-blueprints"
description: "Blueprints de layout HTML para el WebDesigner. 6 arquetipos de estructura de pÃ¡gina (hero, secciones, grids) para que cada sitio tenga una composiciÃ³n visual diferente. Complementa design-styles con variedad estructural."
slug: "layout-blueprints"
metadata:
  paperclip:
    slug: "layout-blueprints"
    skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
    paperclipSkillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
  skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
  key: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
---

# Layout Blueprints â Arquetipos de Estructura | Humanio

## PropÃ³sito

Cada sitio web de Humanio debe tener una composiciÃ³n visual diferente. Este skill define 6 arquetipos de layout que determinan cÃ³mo se organizan las secciones, el hero, los grids de servicios y el flujo general de la pÃ¡gina.

> **REGLA:** Combina un estilo de `design-styles` con un blueprint de este catÃ¡logo. Misma idea: no repitas el mismo blueprint en prospectos consecutivos.

---

## Blueprint A â CENTRADO CLÃSICO

**Estructura:** Todo centrado, flujo vertical limpio, una columna principal.
**Mejor para:** Negocios de servicio Ãºnico, profesionales independientes, landing pages.

```
âââââââââââââââââââââââââââââââââââââââââââââââ
â              NAVBAR (centrado)               â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â                                             â
â           [Badge / Etiqueta]                â
â         HEADLINE GRANDE (h1)                 â
â           SubtÃ­tulo (p)                     â
â         [CTA]    [CTA sec]                  â
â                                             â
â         ââ foto o ilustraciÃ³n ââ             â
â                                            â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Servicios                  â
â    ââââââ  ââââââ  ââââââ                    â
â    âCardâ  âCardâ  âCardâ  (3 col grid)     â
â    ââââââ  ââââââ  ââââââ                    â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Sobre nosotros             â
â         Texto centrado + imagen             â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Testimonios                â
â         Slider o 2-col grid                 â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: CTA Final                  â
â         "Â¿Listo?" + botÃ³n grande            â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â              FOOTER                         â
âââââââââââââââââââââââââââââââââââââââââââââââ
```

**Hero CSS:**
```css
.hero {
  min-height: 90vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 4rem 2rem;
}
```

**Servicios CSS:**
```css
.services-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  max-width: 1100px;
  margin: 0 auto;
}
```

---

## Blueprint B â SPLIT HERO (50/50)

**Estructura:** Hero dividido en dos mitades (texto + imagen). Secciones alternas izq/der.
**Mejor para:** Negocios visuales (restaurantes, salones, tiendas), donde las fotos importan.

```
âââââââââââââââââââââââââââââââââââââââââââââââ
â              NAVBAR (flex)                   â
âââââââââââââââââââââââ¬ââââââââââââââââââââââââ¤
â                     â                       â
â  HEADLINE (h1)      â    [IMAGEN HERO]      â
â  SubtÃ­tulo          â    (border-radius)    â
â  [CTA] [CTA sec]   â                       â
â                     â                       â
âââââââââââââââââââââââ´ââââââââââââââââââââââââ¤
â         SECCIÃN: Features                   â
â    ââââââ  ââââââ  ââââââ  ââââââ           â
â    â Ic â  â Ic â  â Ic â  â Ic â (4 col)   â
â    ââââââ  ââââââ  ââââââ  ââââââ           â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â  [IMAGEN]  â  SecciÃ³n: Sobre nosotros       â
â            â  Texto + bullets               â
ââââââââââââââ¼âââââââââââââââââââââââââââââââââ¤
â  SecciÃ³n:  â  [IMAGEN]                      â
â  Proceso   â  (alternate sides)             â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: GalerÃ­a                    â
â    Masonry o grid 2x3                       â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Contacto                   â
â         Formulario + mapa                   â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â              FOOTER                         â
âââââââââââââââââââââââââââââââââââââââââââââââ
```

**Hero CSS:**
```css
.hero {
  min-height: 90vh;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
  padding: 2rem 4rem;
}
.hero-image {
  border-radius: 24px;
  overflow: hidden;
  aspect-ratio: 4/5;
  object-fit: cover;
}
@media (max-width: 768px) {
  .hero { grid-template-columns: 1fr; text-align: center; }
}
```

**Secciones alternas:**
```css
.section-alt:nth-child(even) {
  direction: rtl;
}
.section-alt:nth-child(even) > * {
  direction: ltr;
}
```

---

## Blueprint C â HERO FULL-BLEED

**Estructura:** Hero ocupa toda la pantalla con imagen de fondo. Contenido sobre la imagen.
**Mejor para:** Negocios con fotos impactantes (inmobiliarias, hoteles, restaurantes premium, constructoras).

```
âââââââââââââââââââââââââââââââââââââââââââââââ
â  NAVBAR (absoluto, sobre el hero)           â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â âââââââââââââââââââââââââââââââââââââââ â
â ââââââ IMAGEN DE FONDO COMPLETA âââââââââ â
â âââââââââââââââââââââââââââââââââââââââ â
â ââ    HEADLINE sobre overlay oscuro    âââ â
â ââ    [CTA]                            âââ â
â âââââââââââââââââââââââââââââââââââââââ â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â  STATS BAR (counters en fila)               â
â   150+ Clientes  â  10 AÃ±os  â  4.9â       â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Servicios                  â
â    Cards en grid 2x2 o 3 columnas          â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â  BANNER PARALLAX (imagen + texto overlay)   â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Equipo / Portfolio         â
â    Grid de fotos con hover overlay          â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â         SECCIÃN: Contacto                   â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â              FOOTER                         â
âââââââââââââââââââââââââââââââââââââââââââââââ
```

**Hero CSS:**
```css
.hero {
  min-height: 100vh;
  background: url('hero-bg.jpg') center/cover no-repeat;
  position: relative;
  display: flex;
  align-items: flex-end;
  padding: 4rem;
}
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, transparent 60%);
}
.hero-content {
  position: relative;
  z-index: 2;
  max-width: 700px;
}
```

**Stats bar:**
```css
.stats-bar {
  display: flex;
  justify-content: center;
  gap: 4rem;
  padding: 3rem 2rem;
  background: var(--accent);
  color: white;
}
.stat-number {
  font-size: 2.5rem;
  font-weight: 800;
  font-family: var(--font-heading);
}
```

---

## Blueprint D â TARJETAS APILADAS (Card Stack)

**Estructura:** Toda la pÃ¡gina son cards/bloques apilados con spacing generoso. No hay hero clÃ¡sico.
**Mejor para:** Negocios con mÃºltiples servicios, menÃºs, catÃ¡logos, tiendas.

```
âââââââââââââââââââââââââââââââââââââââââââââââ
â              NAVBAR (sticky)                â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â                                             â
â  âââââââââââââââââââââââââââââââââââââââ    â
â  â  CARD: Intro / Welcome              â    â
â  â  Logo grande + tagline + CTA        â    â
â  âââââââââââââââââââââââââââââââââââââââ    â
â                                             â
â  âââââââââââââââââââââââââââââââââââââââ    â
â  â  CARD: Servicio 1                   â    â
â  â  Imagen left + texto right          â    â
â  âââââââââââââââââââââââââââââââââââââââ    â
â                                             â
â  âââââââââââââââââââââââââââââââââââââââ    â
â  â  CARD: Servicio 2                   â    â
â  â  Texto left + imagen right          â    â
â  âââââââââââââââââââââââââââââââââââââââ    â
â                                             â
â  ââââââââââââ  ââââââââââââ                 â
â  â Mini cardâ  â Mini cardâ  (2-col)        â
â  â Horarios â  â Precios  â                 â
â  ââââââââââââ  ââââââââââââ                 â
â                                             â
â  âââââââââââââââââââââââââââââââââââââââ    â
â  â  CARD: Testimonios                  â    â
â  âââââââââââââââââââââââââââââââââââââââ    â
â                                             â
â  âââââââââââââââââââââââââââââââââââââââ    â
â  â  CARD: Contacto / WhatsApp          â    â
â  âââââââââââââââââââââââââââââââââââââââ    â
â                                             â
â              FOOTER                         â
âââââââââââââââââââââââââââââââââââââââââââââââ
```

**Card base CSS:**
```css
.card-section {
  max-width: 1000px;
  margin: 1.5rem auto;
  padding: 3rem;
  border-radius: 20px;
  background: var(--bg2);
  border: 1px solid rgba(var(--accent-rgb), 0.1);
}
.card-section.featured {
  background: var(--gradient);
  color: white;
}
```

**Page container:**
```css
body {
  background: var(--bg);
  padding: 1rem;
}
.page-stack {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  max-width: 1100px;
  margin: 0 auto;
}
```

---

## Blueprint E â SCROLL NARRATIVO (Storytelling)

**Estructura:** Flujo narrativo vertical con secciones full-width alternas. Cuenta una historia.
**Mejor para:** Negocios con historia, marcas con propÃ³sito, estudios creativos, cafÃ©s especiales.

```
âââââââââââââââââââââââââââââââââââââââââââââââ
â  NAVBAR (transparente, scroll-aware)        â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â                                             â
â  HERO: Frase grande, mÃ­nimo                 â
â  "Donde cada detalle cuenta."               â
â  (sin imagen, solo tipografÃ­a + bg color)   â
â                                             â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
âââââââââââââââââââââââââââââââââââââââââââââââ
ââ  FULL-WIDTH IMAGEN (parallax)             ââ
âââââââââââââââââââââââââââââââââââââââââââââââ
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â                                             â
â  CAPÃTULO 1: "Nuestra historia"             â
â  Texto largo + imagen flotante              â
â                                             â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
âââââââââââââââââââââââââââââââââââââââââââââââ
ââ  FULL-WIDTH: Quote / Dato impactante      ââ
ââ  "500+ familias confÃ­an en nosotros"      ââ
âââââââââââââââââââââââââââââââââââââââââââââââ
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â                                             â
â  CAPÃTULO 2: "Lo que hacemos"               â
â  Grid de servicios con iconos               â
â                                             â
ââââââââââââââââââââââââââââââââââââââââââââââ¤
âââââââââââââââââââââââââââââââââââââââââââââââ
ââ  FULL-WIDTH GALERÃA (horizontal scroll)   ââ
âââââââââââââââââââââââââââââââââââââââââââââââ
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â                                             â
â  CAPÃTULO 3: "Ãnete"                        â
â  CTA grande + contacto                      â
â                                             â
âââââââââââââââââââââââââââââââââââââââââââââââ¤
â              FOOTER                         â
âââââââââââââââââââââââââââââââââââââââââââââââ
```

**Hero tipogrÃ¡fico:**
```css
.hero-type {
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4rem 2rem;
  background: var(--bg);
}
.hero-type h1 {
  font-size: clamp(3rem, 8vw, 7rem);
  font-weight: 700;
  line-height: 1.0;
  max-width: 900px;
  text-align: center;
}
```

**Full-width divider:**
```css
.full-bleed {
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  padding: 6rem 2rem;
  text-align: center;
  background: var(--accent);
  color: white;
}
```

**Horizontal scroll gallery:**
```css
.h-gallery {
  display: flex;
  gap: 1.5rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  padding: 2rem;
  -webkit-overflow-scrolling: touch;
}
.h-gallery img {
  scroll-snap-align: start;
  flex-shrink: 0;
name: "layout-blueprints"
description: "Blueprints de layout HTML para el WebDesigner. 6 arquetipos de estructura de página (hero, secciones, grids) para que cada sitio tenga una composición visual diferente. Complementa design-styles con variedad estructural."
slug: "layout-blueprints"
metadata:
  paperclip:
    slug: "layout-blueprints"
    skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
    paperclipSkillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
  skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
  key: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/layout-blueprints"
---

# Layout Blueprints — Arquetipos de Estructura | Humanio

## Propósito

Cada sitio web de Humanio debe tener una composición visual diferente. Este skill define 6 arquetipos de layout que determinan cómo se organizan las secciones, el hero, los grids de servicios y el flujo general de la página.

> **REGLA:** Combina un estilo de `design-styles` con un blueprint de este catálogo. Misma idea: no repitas el mismo blueprint en prospectos consecutivos.

---

## Blueprint A — CENTRADO CLÁSICO

**Estructura:** Todo centrado, flujo vertical limpio, una columna principal.
**Mejor para:** Negocios de servicio único, profesionales independientes, landing pages.

```
┌─────────────────────────────────────────────┐
│              NAVBAR (centrado)               │
├─────────────────────────────────────────────┤
│                                             │
│           [Badge / Etiqueta]                │
│         HEADLINE GRANDE (h1)                 │
│           Subtítulo (p)                     │
│         [CTA]    [CTA sec]                  │
│                                             │
│         ── foto o ilustración ──             │
│                                            │
├─────────────────────────────────────────────┤
│         SECCIÓN: Servicios                  │
│    ┌────┐  ┌────┐  ┌────┐                    │
│    │Card│  │Card│  │Card│  (3 col grid)     │
│    └────┘  └────┘  └────┘                    │
├─────────────────────────────────────────────┤
│         SECCIÓN: Sobre nosotros             │
│         Texto centrado + imagen             │
├─────────────────────────────────────────────┤
│         SECCIÓN: Testimonios                │
│         Slider o 2-col grid                 │
├─────────────────────────────────────────────┤
│         SECCIÓN: CTA Final                  │
│         "¿Listo?" + botón grande            │
├─────────────────────────────────────────────┤
│              FOOTER                         │
└─────────────────────────────────────────────┘
```

**Hero CSS:**
```css
.hero {
  min-height: 90vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 4rem 2rem;
}
```

**Servicios CSS:**
```css
.services-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  max-width: 1100px;
  margin: 0 auto;
}
```

---

## Blueprint B — SPLIT HERO (50/50)

**Estructura:** Hero dividido en dos mitades (texto + imagen). Secciones alternas izq/der.
**Mejor para:** Negocios visuales (restaurantes, salones, tiendas), donde las fotos importan.

```
┌─────────────────────────────────────────────┐
│              NAVBAR (flex)                   │
├─────────────────────┬───────────────────────┤
│                     │                       │
│  HEADLINE (h1)      │    [IMAGEN HERO]      │
│  Subtítulo          │    (border-radius)    │
│  [CTA] [CTA sec]   │                       │
│                     │                       │
├─────────────────────┴───────────────────────┤
│         SECCIÓN: Features                   │
│    ┌────┐  ┌────┐  ┌────┐  ┌────┐           │
│    │ Ic │  │ Ic │  │ Ic │  │ Ic │ (4 col)   │
│    └────┘  └────┘  └────┘  └────┘           │
├─────────────────────────────────────────────┤
│  [IMAGEN]  │  Sección: Sobre nosotros       │
│            │  Texto + bullets               │
├────────────┼────────────────────────────────┤
│  Sección:  │  [IMAGEN]                      │
│  Proceso   │  (alternate sides)             │
├─────────────────────────────────────────────┤
│         SECCIÓN: Galería                    │
│    Masonry o grid 2x3                       │
├─────────────────────────────────────────────┤
│         SECCIÓN: Contacto                   │
│         Formulario + mapa                   │
├─────────────────────────────────────────────┤
│              FOOTER                         │
└─────────────────────────────────────────────┘
```

**Hero CSS:**
```css
.hero {
  min-height: 90vh;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
  padding: 2rem 4rem;
}
.hero-image {
  border-radius: 24px;
  overflow: hidden;
  aspect-ratio: 4/5;
  object-fit: cover;
}
@media (max-width: 768px) {
  .hero { grid-template-columns: 1fr; text-align: center; }
}
```

**Secciones alternas:**
```css
.section-alt:nth-child(even) {
  direction: rtl;
}
.section-alt:nth-child(even) > * {
  direction: ltr;
}
```

---

## Blueprint C — HERO FULL-BLEED

**Estructura:** Hero ocupa toda la pantalla con imagen de fondo. Contenido sobre la imagen.
**Mejor para:** Negocios con fotos impactantes (inmobiliarias, hoteles, restaurantes premium, constructoras).

```
┌─────────────────────────────────────────────┐
│  NAVBAR (absoluto, sobre el hero)           │
├─────────────────────────────────────────────┤
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
│ ▓▓▓▓▓▓ IMAGEN DE FONDO COMPLETA ▓▓▓▓▓▓▓▓▓ │
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
│ ▓▓    HEADLINE sobre overlay oscuro    ▓▓▓ │
│ ▓▓    [CTA]                            ▓▓▓ │
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
├─────────────────────────────────────────────┤
│  STATS BAR (counters en fila)               │
│   150+ Clientes  │  10 Años  │  4.9★       │
├─────────────────────────────────────────────┤
│         SECCIÓN: Servicios                  │
│    Cards en grid 2x2 o 3 columnas          │
├─────────────────────────────────────────────┤
│  BANNER PARALLAX (imagen + texto overlay)   │
├─────────────────────────────────────────────┤
│         SECCIÓN: Equipo / Portfolio         │
│    Grid de fotos con hover overlay          │
├─────────────────────────────────────────────┤
│         SECCIÓN: Contacto                   │
├─────────────────────────────────────────────┤
│              FOOTER                         │
└─────────────────────────────────────────────┘
```

**Hero CSS:**
```css
.hero {
  min-height: 100vh;
  background: url('hero-bg.jpg') center/cover no-repeat;
  position: relative;
  display: flex;
  align-items: flex-end;
  padding: 4rem;
}
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, transparent 60%);
}
.hero-content {
  position: relative;
  z-index: 2;
  max-width: 700px;
}
```

**Stats bar:**
```css
.stats-bar {
  display: flex;
  justify-content: center;
  gap: 4rem;
  padding: 3rem 2rem;
  background: var(--accent);
  color: white;
}
.stat-number {
  font-size: 2.5rem;
  font-weight: 800;
  font-family: var(--font-heading);
}
```

---

## Blueprint D — TARJETAS APILADAS (Card Stack)

**Estructura:** Toda la página son cards/bloques apilados con spacing generoso. No hay hero clásico.
**Mejor para:** Negocios con múltiples servicios, menús, catálogos, tiendas.

```
┌─────────────────────────────────────────────┐
│              NAVBAR (sticky)                │
├─────────────────────────────────────────────┤
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  CARD: Intro / Welcome              │    │
│  │  Logo grande + tagline + CTA        │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  CARD: Servicio 1                   │    │
│  │  Imagen left + texto right          │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  CARD: Servicio 2                   │    │
│  │  Texto left + imagen right          │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌──────────┐  ┌──────────┐                 │
│  │ Mini card│  │ Mini card│  (2-col)        │
│  │ Horarios │  │ Precios  │                 │
│  └──────────┘  └──────────┘                 │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  CARD: Testimonios                  │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  CARD: Contacto / WhatsApp          │    │
│  └─────────────────────────────────────┘    │
│                                             │
│              FOOTER                         │
└─────────────────────────────────────────────┘
```

**Card base CSS:**
```css
.card-section {
  max-width: 1000px;
  margin: 1.5rem auto;
  padding: 3rem;
  border-radius: 20px;
  background: var(--bg2);
  border: 1px solid rgba(var(--accent-rgb), 0.1);
}
.card-section.featured {
  background: var(--gradient);
  color: white;
}
```

**Page container:**
```css
body {
  background: var(--bg);
  padding: 1rem;
}
.page-stack {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  max-width: 1100px;
  margin: 0 auto;
}
```

---

## Blueprint E — SCROLL NARRATIVO (Storytelling)

**Estructura:** Flujo narrativo vertical con secciones full-width alternas. Cuenta una historia.
**Mejor para:** Negocios con historia, marcas con propósito, estudios creativos, cafés especiales.

```
┌─────────────────────────────────────────────┐
│  NAVBAR (transparente, scroll-aware)        │
├─────────────────────────────────────────────┤
│                                             │
│  HERO: Frase grande, mínimo                 │
│  "Donde cada detalle cuenta."               │
│  (sin imagen, solo tipografía + bg color)   │
│                                             │
├─────────────────────────────────────────────┤
│█████████████████████████████████████████████│
│█  FULL-WIDTH IMAGEN (parallax)             █│
│█████████████████████████████████████████████│
├─────────────────────────────────────────────┤
│                                             │
│  CAPÍTULO 1: "Nuestra historia"             │
│  Texto largo + imagen flotante              │
│                                             │
├─────────────────────────────────────────────┤
│█████████████████████████████████████████████│
│█  FULL-WIDTH: Quote / Dato impactante      █│
│█  "500+ familias confían en nosotros"      █│
│█████████████████████████████████████████████│
├─────────────────────────────────────────────┤
│                                             │
│  CAPÍTULO 2: "Lo que hacemos"               │
│  Grid de servicios con iconos               │
│                                             │
├────────────────────────────────────────────┤
│█████████████████████████████████████████████│
│█  FULL-WIDTH GALERÍA (horizontal scroll)   █│
│█████████████████████████████████████████████│
├─────────────────────────────────────────────┤
│                                             │
│  CAPÍTULO 3: "Únete"                        │
│  CTA grande + contacto                      │
│                                             │
├─────────────────────────────────────────────┤
│              FOOTER                         │
└─────────────────────────────────────────────┘
```

**Hero tipográfico:**
```css
.hero-type {
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4rem 2rem;
  background: var(--bg);
}
.hero-type h1 {
  font-size: clamp(3rem, 8vw, 7rem);
  font-weight: 700;
  line-height: 1.0;
  max-width: 900px;
  text-align: center;
}
```

**Full-width divider:**
```css
.full-bleed {
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  padding: 6rem 2rem;
  text-align: center;
  background: var(--accent);
  color: white;
}
```

**Horizontal scroll gallery:**
```css
.h-gallery {
  display: flex;
  gap: 1.5rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  padding: 2rem;
  -webkit-overflow-scrolling: touch;
}
.h-gallery img {
  scroll-snap-align: start;
  flex-shrink: 0;
  width: 350px;
  height: 450px;
  object-fit: cover;
  border-radius: 16px;
}
```

---

## Blueprint F — SIDEBAR + CONTENT (Magazine)

**Estructura:** Layout de dos columnas con sidebar de navegación. Contenido tipo revista.
**Mejor para:** Negocios con mucha información (menús extensos, catálogos, servicios múltiples, clínicas con muchas especialidades).

```
┌─────────────────────────────────────────────┐
│              HEADER (full width)             │
│  Logo + Headline + CTA                      │
├──────────┬──────────────────────────────────┤
│          │                                  │
│ SIDEBAR  │  CONTENIDO PRINCIPAL             │
│          │                                  │
│ • Inicio │  Sección actual con scroll       │
│ • Menu   │  Cards, textos, imágenes         │
│ • Team   │                                  │
│ • Hours  │  ┌────┐ ┌────┐ ┌────┐            │
│ • Contact│  │Item│ │Item│ │Item│            │
│          │  └────┘ └────┘ └────┘            │
│ ──────── │                                  │
│ WhatsApp │  Más contenido...                │
│ Teléfono │                                  │
│          │                                  │
├──────────┴──────────────────────────────────┤
│              FOOTER                         │
└─────────────────────────────────────────────┘
```

**Layout CSS:**
```css
.page-layout {
  display: grid;
  grid-template-columns: 260px 1fr;
  min-height: 100vh;
}
.sidebar {
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
  padding: 2rem;
  background: var(--bg2);
  border-right: 1px solid var(--bg3);
}
.sidebar nav a {
  display: block;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  transition: background 0.2s;
}
.sidebar nav a:hover, .sidebar nav a.active {
  background: var(--accent);
  color: white;
}
@media (max-width: 768px) {
  .page-layout { grid-template-columns: 1fr; }
  .sidebar {
    position: fixed; bottom: 0; left: 0; right: 0;
    height: auto; display: flex; gap: 0.5rem;
    padding: 0.5rem; z-index: 100;
  }
}
```

---

## Tabla de referencia rápida

| Blueprint | Hero | Columnas | Mejor para | Combina bien con estilos |
|-----------|------|----------|------------|-------------------------|
| A — Centrado | Centro vertical | 1 col | Profesionales, landing | Clean Slate, Cielo Abierto |
| B — Split 50/50 | Grid 2 col | 2 alternas | Visuales, restaurantes | Tierra Viva, Coral Pop |
| C — Full-bleed | Imagen fondo | Mixed | Inmobiliarias, hoteles | Midnight Luxe, Acero Azul |
| D — Card Stack | Card intro | Cards | Catálogos, menús | Neon Nights, Pastel Dream |
| E — Narrativo | Solo tipografía | 1 col | Marcas con historia | Tierra Viva, Midnight Luxe |
| F — Sidebar | Header compact | 2 col fijo | Mucha info, clínicas | Clean Slate, Acero Azul |

## Componentes extra que puedes mezclar

### Testimonios — 3 variantes
1. **Carousel:** Slider horizontal con flechas
2. **Grid:** Cards 2x2 con foto + quote + nombre
3. **Stack:** Una quote grande central, cambio con fade

### CTAs — 3 variantes
1. **Banner full-width:** Background accent + texto + botón
2. **Card flotante:** Card centrada con border gradient
3. **Inline sutil:** Texto + link subrayado, sin card

### Footer — 3 variantes
1. **Multi-columna:** 3-4 cols con links, horarios, contacto, social
2. **Minimal:** Una línea con logo + links + copyright
3. **CTA footer:** Último empujón con formulario + info de contacto

## Reglas

- **NUNCA** uses el mismo blueprint para todos los prospectos
- **SIEMPRE** adapta el blueprint al responsive (mobile-first)
- Los blueprints son guías, no plantillas rígidas — adapta según el contenido real
- Combina el blueprint con un estilo de `design-styles` para un resultado único
- Si el negocio tiene mucho contenido visual → B, C o E
- Si el negocio es simple (1-2 servicios) → A o D
- Si el negocio tiene menú/catálogo extenso → D o F
