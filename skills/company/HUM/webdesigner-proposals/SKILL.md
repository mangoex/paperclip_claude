---
name: "webdesigner-proposals"
slug: "webdesigner-proposals"
metadata:
  paperclip:
    slug: "webdesigner-proposals"
    skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/webdesigner-proposals"
  paperclipSkillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/webdesigner-proposals"
  skillKey: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/webdesigner-proposals"
key: "company/7f544ec3-9f4e-4c1b-a124-46ed0792bd9d/webdesigner-proposals"
---

# WebDesigner — Propuestas Web Premium | Humanio Marketing

## Identidad

Eres WebDesigner, el agente de diseño web de Humanio Marketing. Creas propuestas web visualmente impactantes en HTML puro con efectos premium. Cada página debe verse como un sitio de agencia de diseño de primer nivel.

## Proceso completo

### 1. Leer el brief

Del ticket extrae: nombre, giro, ciudad, teléfono, WhatsApp, Instagram, servicios, rating, propuesta Humanio, score, URL de propuesta web si existe.

### 1.5 Solicitar aprobación para crear

```
## Solicitud para crear propuesta web — {NOMBRE_NEGOCIO}

**Negocio:** {NOMBRE_NEGOCIO}
**Giro:** {GIRO} | {CIUDAD}
**Score:** {SCORE}/10
**Paleta:** fondo {COLOR_BG}, acento {COLOR_ACENTO}
**Efectos:** cursor tracking, parallax hero, scroll animations, marquee, counters

¿Apruebas crear esta propuesta?
```

### 2. Definir identidad visual por giro

**Dentistas:** `--accent:#2dd4bf` `--bg:#03080f` `--bg2:#060d17` `--bg3:#0a1628`
**Estéticas:** `--accent:#f472b6` `--bg:#0f0608` `--bg2:#160a0d` `--bg3:#1e0f14`
**Restaurantes:** `--accent:#fb923c` `--bg:#0c0804` `--bg2:#140f06` `--bg3:#1c1508`
**Abogados:** `--accent:#a78bfa` `--bg:#07050f` `--bg2:#0d0a17` `--bg3:#130f1f`
**Inmobiliarias:** `--accent:#34d399` `--bg:#030f0a` `--bg2:#061610` `--bg3:#091e16`

### 3. Descargar imagen hero (Pexels API)

`source.unsplash.com` está deprecado. Usa Pexels API.

```bash
mkdir -p /tmp/proposal-{slug}
GIRO_QUERY=$(echo "{GIRO}" | tr ' ' '+' | sed 's/é/e/g;s/á/a/g;s/í/i/g;s/ó/o/g;s/ú/u/g')

# Buscar foto en Pexels por giro
PEXELS_RESULT=$(curl -s "https://api.pexels.com/v1/search?query=${GIRO_QUERY}+professional+interior&per_page=1&orientation=landscape" \
  -H "Authorization: $PEXELS_API_KEY")

HERO_URL=$(echo "$PEXELS_RESULT" | python3 -c "import json,sys; data=json.load(sys.stdin); print(data['photos'][0]['src']['large2x'])" 2>/dev/null)

if [ -z "$HERO_URL" ]; then
  # Fallback por giro si la API falla
  case "{GIRO}" in
    *dental*|*dentist*) HERO_URL="https://images.pexels.com/photos/3845743/pexels-photo-3845743.jpeg?w=1600&q=80" ;;
    *estética*|*salon*|*belleza*) HERO_URL="https://images.pexels.com/photos/3997379/pexels-photo-3997379.jpeg?w=1600&q=80" ;;
    *restaurante*|*café*|*food*) HERO_URL="https://images.pexels.com/photos/1640777/pexels-photo-1640777.jpeg?w=1600&q=80" ;;
    *abogad*|*legal*|*juridic*) HERO_URL="https://images.pexels.com/photos/5668858/pexels-photo-5668858.jpeg?w=1600&q=80" ;;
    *inmobil*|*real+estate*) HERO_URL="https://images.pexels.com/photos/1396122/pexels-photo-1396122.jpeg?w=1600&q=80" ;;
    *) HERO_URL="https://images.pexels.com/photos/3182812/pexels-photo-3182812.jpeg?w=1600&q=80" ;;
  esac
fi

curl -L "$HERO_URL" -o /tmp/proposal-{slug}/hero.jpg
echo "Imagen hero descargada: $HERO_URL"
```

### 3.5 Usar 21st.dev Magic para componentes premium (opcional pero recomendado)

Si necesitas un componente de UI específico (testimonios animados, galería, stats card, etc.) que mejore el diseño, usa el MCP de 21st.dev para generarlo:

```
# Instrucción al MCP magic:
# "Create a dark-themed [component type] with accent color [--accent value]
#  using vanilla HTML, CSS, and JavaScript only (no React, no npm).
#  Style: premium, minimal, motion-rich. Font: Syne for headings, Inter for body."

# Luego integra el componente generado en el HTML del site.
```

Úsalo especialmente para: hero sections alternativas, cards de servicios con efectos 3D, galería de fotos con lightbox, timeline animado, FAQ accordion premium.

### 4. Crear el HTML completo

Crea `/tmp/proposal-{slug}/index.html` con este template completo. Personaliza TODOS los campos `{...}`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{NOMBRE_NEGOCIO} — {CIUDAD}</title>
<meta name="description" content="{DESCRIPCION_SEO}">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet">
<style>
:root{
  --accent:{COLOR_ACENTO};
  --accent-rgb:{ACCENT_RGB};
  --bg:{COLOR_BG};
  --bg2:{COLOR_BG2};
  --bg3:{COLOR_BG3};
  --text:#e2e8f0;
  --muted:#64748b
}
*{margin:0;padding:0;box-sizing:border-box}
html{scroll-behavior:smooth}
body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--text);overflow-x:hidden;cursor:none}
/* CURSOR */
.cursor{position:fixed;width:32px;height:32px;border:1px solid rgba(var(--accent-rgb),.5);border-radius:50%;pointer-events:none;z-index:9999;transition:transform .1s ease,width .2s,height .2s;top:0;left:0;transform:translate(-50%,-50%)}
.cursor-dot{position:fixed;width:5px;height:5px;background:var(--accent);border-radius:50%;pointer-events:none;z-index:9999;top:0;left:0;transform:translate(-50%,-50%)}
.cursor.hover{width:48px;height:48px;background:rgba(var(--accent-rgb),.1)}
/* NAV */
nav{position:fixed;top:0;left:0;right:0;z-index:100;padding:1.2rem 3rem;display:flex;justify-content:space-between;align-items:center;background:rgba(3,8,15,.85);backdrop-filter:blur(20px);border-bottom:1px solid rgba(var(--accent-rgb),.08);transition:padding .3s}
nav.scrolled{padding:.8rem 3rem}
.logo{font-family:'Syne',sans-serif;font-weight:800;font-size:1.3rem;letter-spacing:-.02em;color:#fff}
.logo span{color:var(--accent)}
.nav-links{display:flex;gap:2rem}
.nav-links a{color:rgba(255,255,255,.5);text-decoration:none;font-size:.85rem;letter-spacing:.06em;text-transform:uppercase;transition:color .2s}
.nav-links a:hover{color:var(--accent)}
.nav-cta{padding:.6rem 1.4rem;background:var(--accent);color:var(--bg);border-radius:100px;font-size:.85rem;font-weight:500;text-decoration:none;transition:opacity .2s}
.nav-cta:hover{opacity:.85}
/* HERO */
.hero{min-height:100vh;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden;padding:8rem 3rem 5rem}
.hero-parallax{position:absolute;inset:-20%;background:url('hero.jpg') center/cover no-repeat;transition:transform .1s linear;will-change:transform}
.hero-overlay{position:absolute;inset:0;background:linear-gradient(to bottom,rgba(3,8,15,.6) 0%,rgba(3,8,15,.8) 60%,var(--bg) 100%)}
.hero-glow{position:absolute;inset:0;background:radial-gradient(ellipse 80% 60% at 50% 30%,rgba(var(--accent-rgb),.12) 0%,transparent 70%)}
.hero-grid{position:absolute;inset:0;background-image:linear-gradient(rgba(var(--accent-rgb),.03) 1px,transparent 1px),linear-gradient(90deg,rgba(var(--accent-rgb),.03) 1px,transparent 1px);background-size:60px 60px}
.hero-content{position:relative;z-index:2;text-align:center;max-width:900px}
.hero-badge{display:inline-flex;align-items:center;gap:.5rem;padding:.4rem 1rem;border:1px solid rgba(var(--accent-rgb),.3);border-radius:100px;font-size:.75rem;letter-spacing:.1em;text-transform:uppercase;color:var(--accent);margin-bottom:2rem}
.badge-dot{width:6px;height:6px;background:var(--accent);border-radius:50%;animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.3}}
h1{font-family:'Syne',sans-serif;font-size:clamp(2.8rem,7vw,5.5rem);font-weight:800;line-height:.95;letter-spacing:-.03em;color:#fff;margin-bottom:1.5rem}
h1 .accent{color:var(--accent)}
h1 .dim{color:rgba(255,255,255,.2)}
.hero-sub{font-size:1.1rem;color:rgba(255,255,255,.45);max-width:520px;margin:0 auto 2.5rem;line-height:1.7}
.hero-btns{display:flex;gap:1rem;justify-content:center;flex-wrap:wrap}
.btn-primary{padding:.9rem 2rem;background:var(--accent);color:var(--bg);border-radius:100px;font-weight:500;text-decoration:none;font-size:.95rem;transition:opacity .2s,transform .2s}
.btn-primary:hover{opacity:.85;transform:translateY(-2px)}
.btn-secondary{padding:.9rem 2rem;border:1px solid rgba(255,255,255,.15);color:rgba(255,255,255,.7);border-radius:100px;font-size:.95rem;text-decoration:none;transition:all .2s}
.btn-secondary:hover{border-color:var(--accent);color:var(--accent)}
/* STATS */
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:1px;background:rgba(255,255,255,.04);border-top:1px solid rgba(255,255,255,.04);border-bottom:1px solid rgba(255,255,255,.04)}
.stat{padding:2.5rem;text-align:center;background:var(--bg)}
.stat-num{font-family:'Syne',sans-serif;font-size:clamp(2rem,4vw,3rem);font-weight:800;color:#fff}
.stat-num .accent-text{color:var(--accent)}
.stat-label{font-size:.8rem;color:var(--muted);letter-spacing:.06em;text-transform:uppercase;margin-top:.3rem}
/* SECTIONS */
.pad{padding:7rem 3rem}
.max{max-width:1100px;margin:0 auto}
.section-tag{font-size:.75rem;letter-spacing:.1em;text-transform:uppercase;color:var(--accent);margin-bottom:1rem}
h2{font-family:'Syne',sans-serif;font-size:clamp(2rem,4vw,3.2rem);font-weight:800;line-height:1.05;letter-spacing:-.02em;color:#fff}
h2 .dim{color:rgba(255,255,255,.2)}
/* SERVICES */
.services-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:1px;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.04);border-radius:16px;overflow:hidden;margin-top:3rem}
.service-card{background:var(--bg2);padding:2.5rem;transition:background .3s;position:relative;overflow:hidden}
.service-card::before{content:'';position:absolute;bottom:0;left:0;width:0;height:2px;background:var(--accent);transition:width .4s ease}
.service-card:hover{background:var(--bg3)}
.service-card:hover::before{width:100%}
.service-icon{width:44px;height:44px;background:rgba(var(--accent-rgb),.1);border:1px solid rgba(var(--accent-rgb),.2);border-radius:10px;display:flex;align-items:center;justify-content:center;margin-bottom:1.5rem}
.service-icon svg{width:20px;height:20px;stroke:var(--accent);fill:none;stroke-width:1.5}
.service-card h3{font-family:'Syne',sans-serif;font-size:1.15rem;font-weight:700;color:#fff;margin-bottom:.75rem}
.service-card p{font-size:.9rem;color:var(--muted);line-height:1.7}
/* ABOUT */
.about-grid{display:grid;grid-template-columns:1fr 1fr;gap:5rem;align-items:center}
.about-img{border-radius:16px;overflow:hidden;background:var(--bg3);min-height:400px;border:1px solid rgba(255,255,255,.06);position:relative}
.about-img img{width:100%;height:100%;min-height:400px;object-fit:cover;display:block}
.about-img::after{content:'';position:absolute;inset:0;background:linear-gradient(135deg,rgba(var(--accent-rgb),.1),transparent)}
.about-badge{display:inline-flex;align-items:center;gap:.4rem;padding:.35rem .9rem;background:rgba(var(--accent-rgb),.1);border:1px solid rgba(var(--accent-rgb),.2);border-radius:100px;font-size:.75rem;color:var(--accent);margin-bottom:1.2rem}
.about-text p{font-size:1rem;color:rgba(255,255,255,.5);line-height:1.8;margin-bottom:1rem}
.detail-row{display:flex;gap:.75rem;align-items:flex-start;padding:.75rem 0;border-bottom:1px solid rgba(255,255,255,.04)}
.detail-row:last-child{border-bottom:none}
.detail-icon{width:32px;height:32px;background:rgba(var(--accent-rgb),.08);border-radius:8px;display:flex;align-items:center;justify-content:center;flex-shrink:0;margin-top:2px}
.detail-icon svg{width:14px;height:14px;stroke:var(--accent);fill:none;stroke-width:1.5}
.detail-text{font-size:.85rem;color:var(--muted)}
.detail-text strong{display:block;color:rgba(255,255,255,.7);font-weight:500;margin-bottom:.15rem}
/* PROCESS */
.tech-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:1rem;margin-top:3rem}
.tech-card{background:var(--bg2);border:1px solid rgba(255,255,255,.04);border-radius:12px;padding:1.75rem;transition:border-color .3s,transform .3s}
.tech-card:hover{border-color:rgba(var(--accent-rgb),.2);transform:translateY(-4px)}
.tech-num{font-family:'Syne',sans-serif;font-size:2.5rem;font-weight:800;color:rgba(var(--accent-rgb),.15);margin-bottom:.75rem;line-height:1}
.tech-card h4{font-size:1rem;font-weight:500;color:#fff;margin-bottom:.5rem}
.tech-card p{font-size:.85rem;color:var(--muted);line-height:1.6}
/* MARQUEE */
.marquee-wrap{overflow:hidden;border-top:1px solid rgba(255,255,255,.04);border-bottom:1px solid rgba(255,255,255,.04);padding:1.5rem 0}
.marquee-track{display:flex;gap:3rem;width:max-content;animation:marquee 25s linear infinite}
.marquee-track:hover{animation-play-state:paused}
@keyframes marquee{from{transform:translateX(0)}to{transform:translateX(-50%)}}
.marquee-item{display:flex;align-items:center;gap:.75rem;white-space:nowrap;color:rgba(255,255,255,.2);font-size:.85rem;font-family:'Syne',sans-serif;font-weight:700;letter-spacing:.08em;text-transform:uppercase;transition:color .2s}
.marquee-item:hover{color:var(--accent)}
.marquee-dot{width:4px;height:4px;background:rgba(var(--accent-rgb),.4);border-radius:50%;flex-shrink:0}
/* CONTACT */
.contact-wrap{display:grid;grid-template-columns:1fr 1fr;gap:4rem}
.contact-info h3{font-family:'Syne',sans-serif;font-size:1.8rem;font-weight:800;color:#fff;margin-bottom:1rem}
.contact-info p{color:var(--muted);line-height:1.7;margin-bottom:2rem}
.contact-badges{display:flex;flex-direction:column;gap:.75rem}
.contact-badge{display:flex;align-items:center;gap:.75rem;font-size:.9rem;color:rgba(255,255,255,.5)}
.contact-badge svg{width:16px;height:16px;stroke:var(--accent);fill:none;stroke-width:1.5;flex-shrink:0}
.contact-form{background:var(--bg2);border:1px solid rgba(255,255,255,.06);border-radius:16px;padding:2rem}
.form-group{margin-bottom:1.25rem}
.form-group label{display:block;font-size:.78rem;letter-spacing:.05em;text-transform:uppercase;color:var(--muted);margin-bottom:.5rem}
.form-group input,.form-group textarea{width:100%;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.08);border-radius:8px;padding:.75rem 1rem;color:#fff;font-size:.9rem;font-family:'Inter',sans-serif;outline:none;transition:border-color .2s}
.form-group input:focus,.form-group textarea:focus{border-color:rgba(var(--accent-rgb),.4)}
.form-group textarea{height:100px;resize:none}
.form-submit{width:100%;padding:.9rem;background:var(--accent);color:var(--bg);border:none;border-radius:100px;font-size:.95rem;font-weight:500;cursor:pointer;font-family:'Inter',sans-serif;transition:opacity .2s,transform .2s}
.form-submit:hover{opacity:.85;transform:translateY(-1px)}
/* WHATSAPP */
.whatsapp-btn{display:inline-flex;align-items:center;gap:.6rem;padding:1rem 2rem;background:#25d366;color:#000;border-radius:100px;font-weight:500;text-decoration:none;font-size:.95rem;transition:opacity .2s,transform .2s;margin-top:1.5rem}
.whatsapp-btn:hover{opacity:.88;transform:translateY(-2px)}
.whatsapp-btn svg{width:20px;height:20px;fill:#000}
/* CTA */
.cta-section{text-align:center;padding:7rem 3rem;border-top:1px solid rgba(255,255,255,.04);position:relative;overflow:hidden}
.cta-glow{position:absolute;width:600px;height:300px;left:50%;top:50%;transform:translate(-50%,-50%);background:radial-gradient(ellipse,rgba(var(--accent-rgb),.08) 0%,transparent 70%);pointer-events:none}
.cta-section p.sub{color:var(--muted);max-width:480px;margin:1rem auto 2.5rem;line-height:1.7}
.cta-features{display:flex;justify-content:center;gap:2rem;flex-wrap:wrap;margin-top:2rem}
.cta-feat{display:flex;align-items:center;gap:.4rem;font-size:.85rem;color:rgba(255,255,255,.3)}
.cta-feat::before{content:'';width:4px;height:4px;background:rgba(var(--accent-rgb),.4);border-radius:50%;flex-shrink:0}
/* FOOTER */
footer{padding:2.5rem 3rem;border-top:1px solid rgba(255,255,255,.04);display:flex;justify-content:space-between;align-items:center}
footer p{font-size:.8rem;color:rgba(255,255,255,.2)}
footer a{color:var(--accent);text-decoration:none}
/* RESPONSIVE */
@media(max-width:768px){
  nav{padding:1rem 1.5rem}
  .nav-links{display:none}
  .hero,.pad{padding-left:1.5rem;padding-right:1.5rem}
  .pad{padding-top:4rem;padding-bottom:4rem}
  .stats{grid-template-columns:1fr}
  .about-grid,.contact-wrap{grid-template-columns:1fr}
  footer{flex-direction:column;gap:.75rem;text-align:center}
  body{cursor:auto}
  .cursor,.cursor-dot{display:none}
}
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-dot" id="cursorDot"></div>

<nav id="navbar">
  <div class="logo">{LOGO_P1}<span>{LOGO_P2}</span></div>
  <div class="nav-links">
    <a href="#servicios">Servicios</a>
    <a href="#nosotros">Nosotros</a>
    <a href="#proceso">Proceso</a>
    <a href="#contacto">Contacto</a>
    <a href="/reporte">Reporte</a>
  </div>
  <a href="https://wa.me/52{TELEFONO_LIMPIO}" class="nav-cta">Agendar cita</a>
</nav>

<section class="hero" id="inicio">
  <div class="hero-parallax" id="parallax"></div>
  <div class="hero-overlay"></div>
  <div class="hero-glow"></div>
  <div class="hero-grid"></div>
  <div class="hero-content">
    <div class="hero-badge" data-aos="fade-down">
      <span class="badge-dot"></span>
      {ESPECIALIDAD_BADGE}
    </div>
    <h1 data-aos="fade-up" data-aos-delay="100">
      {HERO_L1},<br>
      <span class="accent">{HERO_L2}.</span><br>
      <span class="dim">{HERO_L3}.</span>
    </h1>
    <p class="hero-sub" data-aos="fade-up" data-aos-delay="200">{HERO_SUB}</p>
    <div class="hero-btns" data-aos="fade-up" data-aos-delay="300">
      <a href="https://wa.me/52{TELEFONO_LIMPIO}" class="btn-primary">Agendar consulta</a>
      <a href="#servicios" class="btn-secondary">Ver servicios</a>
    </div>
  </div>
</section>

<div class="stats">
  <div class="stat" data-aos="fade-up">
    <div class="stat-num"><span class="cnt" data-target="{STAT1_VAL}">0</span><span class="accent-text">+</span></div>
    <div class="stat-label">{STAT1_LABEL}</div>
  </div>
  <div class="stat" data-aos="fade-up" data-aos-delay="100">
    <div class="stat-num"><span class="cnt" data-target="{STAT2_VAL}">0</span><span class="accent-text">+</span></div>
    <div class="stat-label">{STAT2_LABEL}</div>
  </div>
  <div class="stat" data-aos="fade-up" data-aos-delay="200">
    <div class="stat-num">{STAT3_VAL}<span class="accent-text">{STAT3_SYM}</span></div>
    <div class="stat-label">{STAT3_LABEL}</div>
  </div>
</div>

<section class="pad" id="servicios">
  <div class="max">
    <p class="section-tag" data-aos="fade-up">Lo que hacemos</p>
    <h2 data-aos="fade-up" data-aos-delay="50">{H2_SERVICIOS}<br><span class="dim">{H2_SERVICIOS_DIM}</span></h2>
    <div class="services-grid">
      <div class="service-card" data-aos="fade-up">
        <div class="service-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M12 2v3M12 19v3M2 12h3M19 12h3"/></svg></div>
        <h3>{SVC1_NOMBRE}</h3>
        <p>{SVC1_DESC}</p>
      </div>
      <div class="service-card" data-aos="fade-up" data-aos-delay="100">
        <div class="service-icon"><svg viewBox="0 0 24 24"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg></div>
        <h3>{SVC2_NOMBRE}</h3>
        <p>{SVC2_DESC}</p>
      </div>
      <div class="service-card" data-aos="fade-up" data-aos-delay="200">
        <div class="service-icon"><svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M9 9h6M9 12h6M9 15h4"/></svg></div>
        <h3>{SVC3_NOMBRE}</h3>
        <p>{SVC3_DESC}</p>
      </div>
      <div class="service-card" data-aos="fade-up" data-aos-delay="300">
        <div class="service-icon"><svg viewBox="0 0 24 24"><path d="M20.84 4.61a5.5 5.5 0 00-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 00-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 000-7.78z"/></svg></div>
        <h3>{SVC4_NOMBRE}</h3>
        <p>{SVC4_DESC}</p>
      </div>
    </div>
  </div>
</section>

<section class="pad" id="nosotros" style="background:var(--bg2);border-top:1px solid rgba(255,255,255,.04);border-bottom:1px solid rgba(255,255,255,.04)">
  <div class="max about-grid">
    <div class="about-img" data-aos="fade-right">
      <img src="hero.jpg" alt="{NOMBRE_NEGOCIO}">
    </div>
    <div class="about-text" data-aos="fade-left">
      <div class="about-badge">Sobre nosotros</div>
      <h2 style="margin-bottom:1.5rem">{NOMBRE_NEGOCIO}.<br><span class="dim">{AÑOS}+ años de excelencia.</span></h2>
      <p>{ABOUT_P1}</p>
      <p>{ABOUT_P2}</p>
      <div style="margin-top:1.5rem">
        <div class="detail-row">
          <div class="detail-icon"><svg viewBox="0 0 24 24"><path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7z"/></svg></div>
          <div class="detail-text"><strong>Ubicación</strong>{DIRECCION}</div>
        </div>
        <div class="detail-row">
          <div class="detail-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg></div>
          <div class="detail-text"><strong>Horario</strong>{HORARIO}</div>
        </div>
        <div class="detail-row">
          <div class="detail-icon"><svg viewBox="0 0 24 24"><path d="M12 2l1.09 3.26L16 6l-2.91 2.74.91 3.26L12 10l-2 2 .91-3.26L8 6l2.91-.74L12 2z"/></svg></div>
          <div class="detail-text"><strong>Calificación</strong>{RATING}/5 — {NUM_RESENAS} reseñas verificadas</div>
        </div>
      </div>
    </div>
  </div>
</section>

<section class="pad" id="proceso">
  <div class="max">
    <p class="section-tag" data-aos="fade-up">{TAG_PROCESO}</p>
    <h2 data-aos="fade-up" data-aos-delay="50">{H2_PROCESO}<br><span class="dim">{H2_PROCESO_DIM}</span></h2>
    <div class="tech-grid">
      <div class="tech-card" data-aos="fade-up">
        <div class="tech-num">01</div>
        <h4>{PROC1_TITULO}</h4>
        <p>{PROC1_DESC}</p>
      </div>
      <div class="tech-card" data-aos="fade-up" data-aos-delay="100">
        <div class="tech-num">02</div>
        <h4>{PROC2_TITULO}</h4>
        <p>{PROC2_DESC}</p>
      </div>
      <div class="tech-card" data-aos="fade-up" data-aos-delay="200">
        <div class="tech-num">03</div>
        <h4>{PROC3_TITULO}</h4>
        <p>{PROC3_DESC}</p>
      </div>
      <div class="tech-card" data-aos="fade-up" data-aos-delay="300">
        <div class="tech-num">04</div>
        <h4>{PROC4_TITULO}</h4>
        <p>{PROC4_DESC}</p>
      </div>
    </div>
  </div>
</section>

<div class="marquee-wrap">
  <div class="marquee-track">{MARQUEE_ITEMS}</div>
</div>

<section class="pad" id="contacto">
  <div class="max contact-wrap">
    <div class="contact-info" data-aos="fade-right">
      <p class="section-tag">Contáctanos</p>
      <h3>¿Listo para {CTA_FRASE}?</h3>
      <p>Agenda tu consulta sin costo. Sin compromiso.</p>
      <div class="contact-badges">
        <div class="contact-badge">
          <svg viewBox="0 0 24 24"><path d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"/></svg>
          {TELEFONO_DISPLAY}
        </div>
        <div class="contact-badge">
          <svg viewBox="0 0 24 24"><path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7z"/></svg>
          {DIRECCION_CORTA}
        </div>
        <div class="contact-badge">
          <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg>
          {HORARIO_CORTO}
        </div>
      </div>
      <a href="https://wa.me/52{TELEFONO_LIMPIO}" class="whatsapp-btn">
        <svg viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
        Escribir por WhatsApp
      </a>
    </div>
    <div class="contact-form" data-aos="fade-left">
      <div class="form-group"><label>Nombre completo</label><input type="text" placeholder="Tu nombre"></div>
      <div class="form-group"><label>Teléfono</label><input type="tel" placeholder="+52 667 000 0000"></div>
      <div class="form-group"><label>Correo electrónico</label><input type="email" placeholder="tu@correo.com"></div>
      <div class="form-group"><label>Mensaje</label><textarea placeholder="Cuéntanos sobre tu caso..."></textarea></div>
      <button class="form-submit">Enviar mensaje</button>
    </div>
  </div>
</section>

<section class="cta-section">
  <div class="cta-glow"></div>
  <div style="position:relative;z-index:2">
    <p class="section-tag" data-aos="fade-up">¿Listo para el cambio?</p>
    <h2 data-aos="fade-up" data-aos-delay="50">{CTA_H2}<br><span style="color:var(--accent)">{CTA_H2_ACCENT}</span></h2>
    <p class="sub" data-aos="fade-up" data-aos-delay="100">{CTA_SUB}</p>
    <a href="https://wa.me/52{TELEFONO_LIMPIO}" class="btn-primary" data-aos="fade-up" data-aos-delay="150">Agendar ahora</a>
    <div class="cta-features" data-aos="fade-up" data-aos-delay="200">
      <span class="cta-feat">Consulta inicial gratuita</span>
      <span class="cta-feat">Sin compromiso</span>
      <span class="cta-feat">{AÑOS}+ años de experiencia</span>
    </div>
  </div>
</section>

<footer>
  <p>© 2026 {NOMBRE_NEGOCIO} — Todos los derechos reservados.</p>
  <p>Diseñado por <a href="https://humanio.digital">Humanio Marketing</a></p>
</footer>

<script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vanilla-tilt@1.8.1/dist/vanilla-tilt.min.js"></script>
<script>
// SMOOTH SCROLL — Lenis-style native
document.documentElement.style.scrollBehavior='smooth';

// INIT AOS
AOS.init({duration:800,once:true,offset:60,easing:'ease-out-cubic'});

// CURSOR TRACKING
const cursor=document.getElementById('cursor');
const cursorDot=document.getElementById('cursorDot');
let mx=0,my=0,cx=0,cy=0;
document.addEventListener('mousemove',e=>{
  mx=e.clientX;my=e.clientY;
  cursorDot.style.left=mx+'px';cursorDot.style.top=my+'px';
});
function animCursor(){
  cx+=(mx-cx)*.1;cy+=(my-cy)*.1;
  cursor.style.left=cx+'px';cursor.style.top=cy+'px';
  requestAnimationFrame(animCursor);
}
animCursor();
document.querySelectorAll('a,button,.service-card').forEach(el=>{
  el.addEventListener('mouseenter',()=>cursor.classList.add('hover'));
  el.addEventListener('mouseleave',()=>cursor.classList.remove('hover'));
});

// PARALLAX HERO (throttled)
const parallax=document.getElementById('parallax');
let ticking=false;
window.addEventListener('scroll',()=>{
  if(!ticking){
    requestAnimationFrame(()=>{
      if(parallax) parallax.style.transform='translateY('+(window.pageYOffset*.35)+'px)';
      ticking=false;
    });
    ticking=true;
  }
});

// NAV SCROLL STATE
const navbar=document.getElementById('navbar');
window.addEventListener('scroll',()=>{
  navbar.classList.toggle('scrolled',window.pageYOffset>60);
});

// SCROLL PROGRESS BAR
const progBar=document.createElement('div');
progBar.style.cssText='position:fixed;top:0;left:0;height:2px;background:var(--accent);z-index:9999;transition:width .1s linear;pointer-events:none';
document.body.appendChild(progBar);
window.addEventListener('scroll',()=>{
  const pct=(window.scrollY/(document.body.scrollHeight-window.innerHeight))*100;
  progBar.style.width=pct+'%';
});

// MAGNETIC BUTTONS
document.querySelectorAll('.btn-primary,.nav-cta,.whatsapp-btn').forEach(btn=>{
  btn.addEventListener('mousemove',e=>{
    const r=btn.getBoundingClientRect();
    const x=e.clientX-r.left-r.width/2;
    const y=e.clientY-r.top-r.height/2;
    btn.style.transform=`translate(${x*.15}px,${y*.15}px)`;
  });
  btn.addEventListener('mouseleave',()=>{
    btn.style.transform='';
    btn.style.transition='transform .4s cubic-bezier(.25,.46,.45,.94)';
    setTimeout(()=>btn.style.transition='',400);
  });
});

// 3D TILT on service cards
VanillaTilt.init(document.querySelectorAll('.service-card,.tech-card'),{
  max:8,speed:400,glare:true,'max-glare':.08,perspective:1000
});

// COUNTERS with easing
function easeOut(t){return 1-Math.pow(1-t,3)}
function countUp(el,target,duration){
  const start=performance.now();
  function frame(now){
    const t=Math.min((now-start)/duration,1);
    el.textContent=Math.round(easeOut(t)*target);
    if(t<1)requestAnimationFrame(frame);
    else el.textContent=target;
  }
  requestAnimationFrame(frame);
}
const io=new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(e.isIntersecting){
      e.target.querySelectorAll('.cnt').forEach(el=>{
        countUp(el,parseInt(el.dataset.target),1400);
      });
    }
  });
},{threshold:.4});
document.querySelectorAll('.stats').forEach(s=>io.observe(s));

// SPLIT TEXT REVEAL for h1
const h1=document.querySelector('h1');
if(h1){
  const words=h1.innerHTML.split(' ');
  h1.innerHTML=words.map((w,i)=>`<span style="display:inline-block;opacity:0;transform:translateY(30px);transition:opacity .6s ${i*.08}s ease,transform .6s ${i*.08}s ease">${w}&nbsp;</span>`).join('');
  setTimeout(()=>{
    h1.querySelectorAll('span').forEach(s=>{s.style.opacity='1';s.style.transform='translateY(0)'});
  },200);
}

// SMOOTH ANCHOR SCROLL
document.querySelectorAll('a[href^="#"]').forEach(a=>{
  a.addEventListener('click',e=>{
    const el=document.querySelector(a.getAttribute('href'));
    if(el){e.preventDefault();el.scrollIntoView({behavior:'smooth',block:'start'});}
  });
});

// IMAGE LAZY LOAD with blur-up
document.querySelectorAll('img[data-src]').forEach(img=>{
  const obs=new IntersectionObserver(([e])=>{
    if(e.isIntersecting){
      img.src=img.dataset.src;
      img.style.filter='blur(10px)';
      img.onload=()=>{img.style.transition='filter .6s ease';img.style.filter='none'};
      obs.disconnect();
    }
  });
  obs.observe(img);
});
</script>
</body>
</html>
```

### 5. Variables a reemplazar

| Variable                | Descripción                        |
| ----------------------- | ---------------------------------- |
| `{NOMBRE_NEGOCIO}`      | Nombre completo del negocio        |
| `{CIUDAD}`              | Ciudad                             |
| `{COLOR_ACENTO}`        | Color acento hex                   |
| `{ACCENT_RGB}`          | RGB del acento ej: `45,212,191`    |
| `{COLOR_BG/BG2/BG3}`    | Colores de fondo                   |
| `{LOGO_P1/P2}`          | Nombre partido en dos para el logo |
| `{TELEFONO_LIMPIO}`     | Solo dígitos sin espacios          |
| `{TELEFONO_DISPLAY}`    | Teléfono formateado                |
| `{ESPECIALIDAD_BADGE}`  | Texto del badge del hero           |
| `{HERO_L1/L2/L3}`       | 3 líneas del headline              |
| `{HERO_SUB}`            | Subtítulo del hero                 |
| `{STAT1/2_VAL/LABEL}`   | Estadísticas con contadores        |
| `{STAT3_VAL/SYM/LABEL}` | Tercera estadística                |
| `{H2_SERVICIOS/DIM}`    | Título sección servicios           |
| `{SVC1-4_NOMBRE/DESC}`  | 4 servicios del negocio            |
| `{AÑOS}`                | Años de experiencia                |
| `{ABOUT_P1/P2}`         | Descripción del negocio            |
| `{DIRECCION/CORTA}`     | Dirección completa y corta         |
| `{HORARIO/CORTO}`       | Horarios de atención               |
| `{RATING/NUM_RESENAS}`  | Rating y número de reseñas         |
| `{TAG_PROCESO}`         | Etiqueta sección proceso           |
| `{H2_PROCESO/DIM}`      | Título sección proceso             |
| `{PROC1-4_TITULO/DESC}` | 4 pasos del proceso                |
| `{MARQUEE_ITEMS}`       | Keywords repetidas x2 para loop    |
| `{CTA_FRASE}`           | Texto CTA contacto                 |
| `{CTA_H2/ACCENT/SUB}`   | Sección CTA final                  |

### Formato del marquee

```html
<div class="marquee-item"><span>KEYWORD</span><span class="marquee-dot"></span></div>
```

Repite todos los items exactamente 2 veces para el loop infinito.

### 6. Incluir el diagnóstico SEO (si existe)

Si el ticket de Qualifier incluye el archivo `reporte.html` adjunto, inclúyelo en el site:

```bash
# El Qualifier genera reporte.html y lo adjunta al ticket
# Descarga el attachment y cópialo al directorio del site:
cp /tmp/reporte-{slug}.html /tmp/proposal-{slug}/reporte.html

# Añade también un botón en el index.html que apunte al reporte.
# Busca la sección del footer o nav y añade:
# <a href="./reporte">Ver mi reporte SEO gratuito →</a>
```

Si no hay diagnóstico disponible aún, omite este paso — el Qualifier lo generará y podrá actualizarse después del deploy.

### 7. Solicitar aprobación antes de publicar

```
## Propuesta lista para publicar — {NOMBRE_NEGOCIO}

**Efectos incluidos:**
- Cursor personalizado con magnetic pull en botones
- Hero parallax (scroll suave throttled)
- Scroll progress bar
- Scroll animations (AOS con easing cúbico)
- Contadores animados (easing easeOut)
- 3D tilt en service cards (vanilla-tilt)
- Split-text reveal en h1
- Blur-up lazy loading de imágenes
- Marquee infinito pausable

**Páginas:**
- `/` — Propuesta web del negocio
- `/reporte` — Reporte SEO gratuito (si disponible)

**URL destino:** https://humanio-{slug}.netlify.app

¿Apruebas publicar?
```

### 8. Publicar en Netlify

```bash
SLUG=$(echo "{NOMBRE_NEGOCIO}" | tr '[:upper:]' '[:lower:]' | tr ' ' '-' | sed 's/[^a-z0-9-]//g')

NETLIFY_AUTH_TOKEN=$NETLIFY_AUTH_TOKEN netlify deploy \
  --dir=/tmp/proposal-$SLUG \
  --prod \
  --site-name="humanio-$SLUG" \
  --message="Propuesta {NOMBRE_NEGOCIO} — Humanio Marketing"

echo "Sitio publicado: https://humanio-${SLUG}.netlify.app"
echo "Reporte: https://humanio-${SLUG}.netlify.app/reporte"
```

Si el deploy falla con error de site name ocupado, añade un sufijo numérico:
```bash
--site-name="humanio-${SLUG}-2"
```

### 9. Notificar al CEO

```
## Propuesta web publicada ✅

**Cliente:** {NOMBRE_NEGOCIO}
**URL propuesta:** https://humanio-{slug}.netlify.app
**URL reporte:** https://humanio-{slug}.netlify.app/reporte
**Efectos:** parallax, magnetic buttons, 3D tilt, split-text, scroll progress, AOS cúbico, counters easeOut, blur-up images

**Siguiente paso:** Outreach tiene ambas URLs para incluir en la propuesta comercial
```

## Reglas de calidad

* SIEMPRE dark theme — nunca fondo blanco
* SIEMPRE cursor personalizado
* SIEMPRE parallax en el hero
* SIEMPRE marquee con keywords del giro x2
* SIEMPRE pedir aprobación antes de crear Y antes de publicar
* El slug debe ser solo letras minúsculas y guiones
* `{TELEFONO_LIMPIO}` solo dígitos sin espacios ni guiones
* Las imágenes de Unsplash pueden tardar — si falla usar color de fondo sólido
* Si Netlify falla con el nombre, agregar `-mx` al slug y reintentar

