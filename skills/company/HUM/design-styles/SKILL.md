---
name: "design-styles"
description: "CatÃ¡logo de 10 estilos visuales distintos para el WebDesigner. Cada estilo incluye tipografÃ­a, paleta de colores, modo de fondo (claro/oscuro), efectos CSS, mood board y giros de negocio recomendados. El WebDesigner DEBE seleccionar un estilo diferente para cada prospecto."
slug: "design-styles"
metadata:
  paperclip:
    slug: "design-styles"
    skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/design-styles"
    paperclipSkillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/design-styles"
  skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/design-styles"
  key: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/design-styles"
---

# Design Styles â CatÃ¡logo de Estilos Visuales | Humanio

## PropÃ³sito

Este skill reemplaza el enfoque de "un solo diseÃ±o para todos". El WebDesigner DEBE consultar este catÃ¡logo antes de generar cada sitio y seleccionar el estilo que mejor se adapte al giro, personalidad y mercado del prospecto.

> **REGLA ABSOLUTA:** No repitas el mismo estilo en prospectos consecutivos. Si el Ãºltimo sitio fue "Neon Nights", el siguiente NO puede ser "Neon Nights".

## CÃ³mo seleccionar el estilo

1. Lee el brief del Qualifier (giro, ciudad, audiencia, personalidad del negocio)
2. Consulta la columna "Giros recomendados" de cada estilo
3. Si hay 2+ estilos posibles, elige el que NO se usÃ³ en el Ãºltimo sitio
4. Aplica las variables CSS, fuentes y efectos del estilo elegido
5. Registra el estilo usado en el ticket: `**Estilo aplicado:** {nombre}`

---

## Los 10 estilos

### 1. NEON NIGHTS (Dark + Vibrante)

**Mood:** TecnolÃ³gico, moderno, urbano, energÃ©tico
**Fondo:** Oscuro
**Giros recomendados:** Bares, clubs nocturnos, barbershops, gaming, startups tech, studios de tatuaje

```css
/* Fuentes */
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=DM+Sans:wght@300;400;500&display=swap');

:root {
  --font-heading: 'Space Grotesk', sans-serif;
  --font-body: 'DM Sans', sans-serif;
  --bg: #0a0a0f;
  --bg2: #12121a;
  --bg3: #1a1a2e;
  --text: #e4e4e7;
  --muted: #71717a;
  --accent: #8b5cf6;
  --accent-rgb: 139, 92, 246;
  --accent2: #06b6d4;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Glow suave en borders: `box-shadow: 0 0 20px rgba(var(--accent-rgb), 0.15)`
- Text gradient en headings: `background: var(--gradient); -webkit-background-clip: text;`
- LÃ­neas de grid sutil en fondo (CSS pattern)
- Hover con scale + glow

---

### 2. CLEAN SLATE (Light + Minimalista)

**Mood:** Profesional, limpio, confiable, premium silencioso
**Fondo:** Claro
**Giros recomendados:** Doctores, dentistas, consultorios mÃ©dicos, psicÃ³logos, nutriÃ³logos, contadores

```css
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&family=Inter:wght@300;400;500&display=swap');

:root {
  --font-heading: 'Plus Jakarta Sans', sans-serif;
  --font-body: 'Inter', sans-serif;
  --bg: #ffffff;
  --bg2: #f8fafc;
  --bg3: #f1f5f9;
  --text: #1e293b;
  --muted: #94a3b8;
  --accent: #2563eb;
  --accent-rgb: 37, 99, 235;
  --accent2: #0ea5e9;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Sombras suaves multi-capa: `box-shadow: 0 1px 3px rgba(0,0,0,0.04), 0 6px 16px rgba(0,0,0,0.06)`
- Sin animaciones agresivas â solo fade-in suave
- Bordes redondeados generosos (16px)
- Mucho espacio blanco (padding 6rem+)
- Iconos con lÃ­nea delgada (Lucide/Phosphor)

---

### 3. TIERRA VIVA (Warm + OrgÃ¡nico)

**Mood:** Natural, artesanal, acogedor, local, autÃ©ntico
**Fondo:** CÃ¡lido claro
**Giros recomendados:** CafeterÃ­as, panaderÃ­as, restaurantes orgÃ¡nicos, floristerÃ­as, tiendas naturistas, veterinarias, yoga studios

```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Nunito:wght@300;400;500;600&display=swap');

:root {
  --font-heading: 'Playfair Display', serif;
  --font-body: 'Nunito', sans-serif;
  --bg: #fefdf8;
  --bg2: #faf5eb;
  --bg3: #f3ead6;
  --text: #3d2c1e;
  --muted: #8b7355;
  --accent: #c2703e;
  --accent-rgb: 194, 112, 62;
  --accent2: #7c9a5e;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Bordes orgÃ¡nicos (border-radius variados: 24px 4px 24px 4px)
- Texturas sutiles de papel/lino como background-image
- TipografÃ­a serif elegante en headings
- Fotos con filter: `saturate(0.85) contrast(1.05)` para tono cÃ¡lido
- Decoraciones SVG: hojas, curvas orgÃ¡nicas

---

### 4. ACERO AZUL (Corporate + Serio)

**Mood:** Institucional, serio, confiable, estructurado
**Fondo:** Claro con acentos oscuros
**Giros recomendados:** Abogados, despachos contables, inmobiliarias, aseguradoras, constructoras, consultorÃ­as

```css
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;600;700&family=Source+Sans+3:wght@300;400;500;600&display=swap');

:root {
  --font-heading: 'Outfit', sans-serif;
  --font-body: 'Source Sans 3', sans-serif;
  --bg: #ffffff;
  --bg2: #f0f4f8;
  --bg3: #e2e8f0;
  --text: #0f172a;
  --muted: #64748b;
  --accent: #1e40af;
  --accent-rgb: 30, 64, 175;
  --accent2: #0369a1;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Layout asimÃ©trico con grid CSS (60/40, 70/30)
- LÃ­neas horizontales divisoras (1px solid #e2e8f0)
- Counters/estadÃ­sticas prominentes
- TipografÃ­a pesada en nÃºmeros
- Cards con borde izquierdo de color (border-left: 4px solid var(--accent))
- Sin animaciones llamativas â transiciones de 0.2s

---

### 5. CORAL POP (Bold + Divertido)

**Mood:** Juvenil, energÃ©tico, divertido, fresco, social
**Fondo:** Claro con colores vibrantes
**Giros recomendados:** HeladerÃ­as, salones de belleza, tiendas de ropa juvenil, gimnasios, escuelas de idiomas, pet shops

```css
@import url('https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap');

:root {
  --font-heading: 'Bricolage Grotesque', sans-serif;
  --font-body: 'DM Sans', sans-serif;
  --bg: #fffbf5;
  --bg2: #fff0e6;
  --bg3: #ffe4d1;
  --text: #1a1a2e;
  --muted: #6b7280;
  --accent: #f43f5e;
  --accent-rgb: 244, 63, 94;
  --accent2: #f97316;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Border-radius extra redondeados (24px, pill buttons 999px)
- Blob shapes como decoraciÃ³n (SVG o clip-path)
- Sombras de color: `box-shadow: 0 8px 24px rgba(var(--accent-rgb), 0.2)`
- Hover con bounce (transform: scale(1.05) + transition con ease-out)
- Emojis como decoraciÃ³n visual (usados con moderaciÃ³n)
- Stickers / badges rotados ligeramente (transform: rotate(-3deg))

---

### 6. MIDNIGHT LUXE (Dark + Elegante)

**Mood:** Premium, exclusivo, sofisticado, lujo accesible
**Fondo:** Oscuro elegante
**Giros recomendados:** Spas, joyerÃ­as, relojerÃ­as, restaurantes finos, wine bars, boutiques de moda, estudios fotogrÃ¡ficos

```css
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600;700&family=Montserrat:wght@300;400;500&display=swap');

:root {
  --font-heading: 'Cormorant Garamond', serif;
  --font-body: 'Montserrat', sans-serif;
  --bg: #0c0a09;
  --bg2: #1c1917;
  --bg3: #292524;
  --text: #fafaf9;
  --muted: #a8a29e;
  --accent: #d4a574;
  --accent-rgb: 212, 165, 116;
  --accent2: #a78bfa;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- TipografÃ­a serif grande y espaciada (letter-spacing: 0.05em en headings)
- LÃ­neas doradas decorativas (1px solid var(--accent), opacity: 0.3)
- Transiciones lentas y suaves (0.6s ease)
- ImÃ¡genes con overlay oscuro sutil
- Layout centrado con max-width estrecho (800px para texto)
- Efectos de revelaciÃ³n al scroll (clip-path animation)

---

### 7. VERDE DIGITAL (Eco + Tech)

**Mood:** Sustentable, innovador, fresco, consciente
**Fondo:** Oscuro con verdes
**Giros recomendados:** Tiendas ecolÃ³gicas, consultorÃ­a ambiental, energÃ­a solar, agro-tech, comida vegana, terapias alternativas

```css
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&family=Inter:wght@300;400;500&display=swap');

:root {
  --font-heading: 'Manrope', sans-serif;
  --font-body: 'Inter', sans-serif;
  --bg: #022c22;
  --bg2: #064e3b;
  --bg3: #065f46;
  --text: #ecfdf5;
  --muted: #6ee7b7;
  --accent: #34d399;
  --accent-rgb: 52, 211, 153;
  --accent2: #a3e635;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Iconos de lÃ­nea verde (stroke animations en SVG)
- Mesh gradient de fondo con greens
- Cards con glassmorphism sutil (backdrop-filter: blur(8px))
- NÃºmeros y datos resaltados en accent
- Pattern de puntos o grid en secciones alternas

---

### 8. PASTEL DREAM (Soft + Femenino)

**Mood:** Delicado, calmado, elegante femenino, confiable
**Fondo:** Claro pastel
**Giros recomendados:** EstÃ©ticas, salones de uÃ±as, boutiques de novia, coaching personal, terapia infantil, doulas, fotografÃ­a de bebÃ©s

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display&family=Poppins:wght@300;400;500;600&display=swap');

:root {
  --font-heading: 'DM Serif Display', serif;
  --font-body: 'Poppins', sans-serif;
  --bg: #fdf4ff;
  --bg2: #fae8ff;
  --bg3: #f0abfc33;
  --text: #3b0764;
  --muted: #a855f7;
  --accent: #d946ef;
  --accent-rgb: 217, 70, 239;
  --accent2: #ec4899;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Bordes suaves (border-radius: 20px)
- Sombras difusas de color (box-shadow con accent al 10%)
- Formas abstractas SVG en fondo (blobs, arcos)
- Animaciones suaves y lentas (0.5s ease-in-out)
- ImÃ¡genes con border-radius grandes o clip-path: circle()
- Espaciado generoso, sensaciÃ³n airy

---

### 9. CONCRETO URBANO (Industrial + Raw)

**Mood:** Raw, directo, urbano, sin pretensiones, honesto
**Fondo:** Gris neutro
**Giros recomendados:** Talleres mecÃ¡nicos, gimnasios crossfit, taquerÃ­as, food trucks, constructoras, ferreterÃ­as, imprentas

```css
@import url('https://fonts.googleapis.com/css2?family=Albert+Sans:wght@400;600;700;900&family=IBM+Plex+Sans:wght@300;400;500&display=swap');

:root {
  --font-heading: 'Albert Sans', sans-serif;
  --font-body: 'IBM Plex Sans', sans-serif;
  --bg: #fafafa;
  --bg2: #f4f4f5;
  --bg3: #e4e4e7;
  --text: #18181b;
  --muted: #71717a;
  --accent: #ef4444;
  --accent-rgb: 239, 68, 68;
  --accent2: #f59e0b;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- TipografÃ­a pesada y grande (font-weight: 900 en h1)
- Sin border-radius (bordes rectos, 0px o 2px max)
- Contraste alto (negro sobre blanco, acento rojo)
- Grid visible / layout tipo magazine
- Hover brutales (background swap instantÃ¡neo, no transition)
- Texto uppercase en labels y CTAs
- Bordes gruesos (2-3px solid)

---

### 10. CIELO ABIERTO (Fresh + Airy)

**Mood:** Abierto, luminoso, respirado, moderno suave
**Fondo:** Claro azul
**Giros recomendados:** Agencias de viajes, inmobiliarias de playa, escuelas, guarderÃ­as, coworkings, terapia ocupacional, clÃ­nicas dentales modernas

```css
@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@300;400;500;600;700&family=Nunito+Sans:wght@300;400;500;600&display=swap');

:root {
  --font-heading: 'Lexend', sans-serif;
  --font-body: 'Nunito Sans', sans-serif;
  --bg: #f8fbff;
  --bg2: #eff6ff;
  --bg3: #dbeafe;
  --text: #1e3a5f;
  --muted: #64748b;
  --accent: #3b82f6;
  --accent-rgb: 59, 130, 246;
  --accent2: #06b6d4;
  --gradient: linear-gradient(135deg, var(--accent), var(--accent2));
}
```

**Efectos distintivos:**
- Layout amplio con max-width 1400px
- ImÃ¡genes grandes con border-radius: 24px
- Floating cards con sombra suave elevada
- Scroll animations suaves (solo fade + translateY 20px)
- Ilustraciones o iconos en estilo outlined
- Secciones con padding vertical generoso (8rem)
- Botones con border visible + fill on hover

---

## Tabla de referencia rÃ¡pida

| # | Estilo | Fondo | Fuente heading | Fuente body | Accent | Mood |
|---|--------|-------|---------------|-------------|--------|------|
| 1 | Neon Nights | Oscuro | Space Grotesk | DM Sans | #8b5cf6 | Tech vibrante |
| 2 | Clean Slate | Claro | Plus Jakarta Sans | Inter | #2563eb | Profesional limpio |
| 3 | Tierra Viva | CÃ¡lido | Playfair Display | Nunito | #c2703e | OrgÃ¡nico cÃ¡lido |
| 4 | Acero Azul | Claro | Outfit | Source Sans 3 | #1e40af | Corporate serio |
| 5 | Coral Pop | Claro vibrante | Bricolage Grotesque | DM Sans | #f43f5e | Juvenil divertido |
| 6 | Midnight Luxe | Oscuro | Cormorant Garamond | Montserrat | #d4a574 | Premium exclusivo |
| 7 | Verde Digital | Oscuro verde | Manrope | Inter | #34d399 | Eco tech |
| 8 | Pastel Dream | Claro pastel | DM Serif Display | Poppins | #d946ef | Suave femenino |
| 9 | Concreto Urbano | Gris neutro | Albert Sans | IBM Plex Sans | #ef4444 | Industrial raw |
| 10 | Cielo Abierto | Claro azul | Lexend | Nunito Sans | #3b82f6 | Fresh luminoso |

## Reglas

- **NUNCA** uses Syne + Inter como default automÃ¡tico â selecciona del catÃ¡logo
- **SIEMPRE** registra el estilo elegido en el ticket del prospecto
- **NUNCA** repitas el mismo estilo en 2 prospectos consecutivos
- Si el brief del Qualifier indica una personalidad especÃ­fica, Ãºsala como guÃ­a
- Si no hay indicaciÃ³n clara, elige basÃ¡ndote en el giro del negocio
- Puedes combinar elementos de 2 estilos si tiene sentido, pero documÃ©ntalo
- Los colores de accent pueden ajustarse ligeramente al branding existente del negocio
- Si el negocio ya tiene colores de marca, integra esos colores en el estilo elegido
