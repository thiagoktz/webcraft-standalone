---
name: performance
description: Use este skill quando o usuário mencionar performance, velocidade, Core Web Vitals, Lighthouse, LCP, CLS, INP ou quando o site gerado tiver muitas imagens, animações ou scripts externos. Aplique também como checklist final em qualquer entrega de produção.
---

# Skill: Performance — Otimizações Avançadas de Core Web Vitals

Este skill garante que o site gerado atinja pontuação alta no Lighthouse e métricas saudáveis de Core Web Vitals, resultando em melhor UX e SEO.

---

## Metas de Core Web Vitals (Google)

| Métrica | Boa | Precisa melhorar | Ruim |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | ≤ 2.5s | 2.5s–4s | > 4s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | 200–500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | 0.1–0.25 | > 0.25 |

---

## 1. LCP — Carregamento do Maior Elemento

### Identificar o elemento LCP
Geralmente é a imagem hero ou o H1 principal.

### Otimizações obrigatórias:

```html
<!-- 1. Preload da imagem hero -->
<link rel="preload" as="image" href="hero.webp" fetchpriority="high">

<!-- 2. Imagem hero sem lazy loading -->
<img
  src="hero.webp"
  alt="..."
  width="1440"
  height="600"
  fetchpriority="high"
  decoding="async"
>

<!-- 3. Fontes com preconnect e display swap -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=...&display=swap" rel="stylesheet">

<!-- 4. DNS prefetch para domínios de terceiros -->
<link rel="dns-prefetch" href="https://www.googletagmanager.com">
```

### CSS crítico inline (above the fold):
```html
<head>
  <style>
    /* CSS mínimo para renderizar o hero sem bloquear */
    body { margin: 0; font-family: var(--font-body); }
    .hero { min-height: 100vh; display: flex; align-items: center; }
    .hero h1 { font-size: clamp(2rem, 5vw, 4rem); }
  </style>
  <!-- CSS completo carregado de forma não-bloqueante -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
```

---

## 2. CLS — Estabilidade Visual

### Causas comuns e soluções:

```html
<!-- ✅ Sempre defina dimensões em imagens -->
<img src="foto.webp" width="800" height="600" alt="...">

<!-- ✅ Reservar espaço para embeds e iframes -->
<div style="aspect-ratio: 16/9; width: 100%;">
  <iframe src="..." style="width:100%;height:100%;border:0;" loading="lazy"></iframe>
</div>

<!-- ✅ Fontes com font-display: swap (evita FOUT agressivo) -->
<!-- ✅ Skeleton screens para conteúdo dinâmico -->
```

```css
/* Evitar mudanças de layout ao carregar fontes */
:root {
  --font-body: 'MinhaFonte', Georgia, serif; /* fallback similar */
}

/* Reservar espaço para anúncios ou banners dinâmicos */
.ad-slot {
  min-height: 250px;
  background: #f5f5f5;
}

/* Transformações não causam CLS (usam compositor) */
.card:hover {
  transform: translateY(-4px); /* ✅ sem reflow */
  /* top: -4px; ❌ causa reflow */
}
```

---

## 3. INP — Responsividade a Interações

### Scripts com defer e async:
```html
<!-- Scripts não-críticos: defer (executa após parse do HTML) -->
<script src="analytics.js" defer></script>

<!-- Scripts independentes: async (executa assim que carrega) -->
<script src="widget-externo.js" async></script>

<!-- Scripts críticos: inline no final do body -->
<script>
  // Apenas lógica essencial para first interaction
</script>
```

### Dividir tarefas longas de JavaScript:
```javascript
// ❌ Bloqueia a thread principal
function processarLista(items) {
  items.forEach(item => processarItem(item)); // 500+ itens = jank
}

// ✅ Ceder controle ao browser entre chunks
async function processarListaAsync(items) {
  const CHUNK_SIZE = 50;
  for (let i = 0; i < items.length; i += CHUNK_SIZE) {
    const chunk = items.slice(i, i + CHUNK_SIZE);
    chunk.forEach(item => processarItem(item));
    await new Promise(resolve => setTimeout(resolve, 0)); // yield
  }
}
```

### Event listeners eficientes:
```javascript
// ❌ Listener pesado no scroll
window.addEventListener('scroll', () => {
  calcularPosicaoDeTodosOsElementos(); // caro
});

// ✅ Throttle com requestAnimationFrame
let rafPending = false;
window.addEventListener('scroll', () => {
  if (!rafPending) {
    rafPending = true;
    requestAnimationFrame(() => {
      calcularPosicao();
      rafPending = false;
    });
  }
});
```

---

## 4. Imagens — Otimização Completa

### Formatos modernos com fallback:
```html
<picture>
  <source srcset="hero.avif" type="image/avif">
  <source srcset="hero.webp" type="image/webp">
  <img src="hero.jpg" alt="..." width="1440" height="600">
</picture>
```

### Imagens responsivas:
```html
<img
  src="foto-800.webp"
  srcset="
    foto-400.webp 400w,
    foto-800.webp 800w,
    foto-1200.webp 1200w
  "
  sizes="
    (max-width: 768px) 100vw,
    (max-width: 1200px) 50vw,
    800px
  "
  alt="..."
  width="800"
  height="600"
  loading="lazy"
>
```

### CSS para imagens sem distorção:
```css
img {
  max-width: 100%;
  height: auto;
  display: block;
}

.card-imagem img {
  width: 100%;
  aspect-ratio: 16 / 9;
  object-fit: cover;
  object-position: center;
}
```

---

## 5. CSS — Reduzir Bloqueio de Renderização

```css
/* Usar contain para isolar repintura */
.card {
  contain: layout style; /* browser não recalcula o resto */
}

/* Promover para GPU apenas quando necessário */
.animacao-complexa {
  will-change: transform; /* usar com moderação */
}

/* Remover will-change após animação */
elemento.addEventListener('animationend', () => {
  elemento.style.willChange = 'auto';
});

/* Prefer gap sobre margin para layouts Flex/Grid (menos reflow) */
.grid {
  display: grid;
  gap: 1.5rem; /* ✅ */
}
```

---

## 6. Carregamento de Recursos de Terceiros

```html
<!-- Carregar Google Analytics de forma assíncrona e tardia -->
<script>
  // Aguarda interação do usuário para carregar analytics
  const loadAnalytics = () => {
    const script = document.createElement('script');
    script.src = 'https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXX';
    script.async = true;
    document.head.appendChild(script);
  };

  // Carregar após primeira interação (click, scroll, touch)
  ['click', 'scroll', 'touchstart'].forEach(event => {
    document.addEventListener(event, loadAnalytics, { once: true });
  });
</script>

<!-- Embed do YouTube sem bloquear -->
<!-- Usar lite-youtube-embed em vez de iframe completo -->
<lite-youtube videoid="XXXXXXX" playlabel="Assistir vídeo"></lite-youtube>
```

---

## 7. Cache e Headers (para configuração no servidor)

```
# Netlify _headers ou Vercel vercel.json

# Assets com hash no nome: cache longo
/assets/*
  Cache-Control: public, max-age=31536000, immutable

# HTML: sem cache (sempre fresco)
/*.html
  Cache-Control: no-cache, no-store, must-revalidate

# Fontes: cache longo
/fonts/*
  Cache-Control: public, max-age=31536000, immutable
```

---

## 8. Checklist de Performance Pré-Entrega

### LCP
- [ ] Imagem hero com `fetchpriority="high"` e sem `loading="lazy"`
- [ ] `<link rel="preload">` para hero image e fonte principal
- [ ] `<link rel="preconnect">` para Google Fonts e domínios externos
- [ ] CSS crítico inline no `<head>`
- [ ] CSS não-crítico carregado de forma não-bloqueante

### CLS
- [ ] `width` e `height` em todas as imagens
- [ ] `aspect-ratio` em containers de embed/video
- [ ] Fontes com `font-display: swap`
- [ ] Animações usando `transform` e `opacity` (não `top`, `left`, `width`)

### INP
- [ ] Scripts com `defer` ou `async`
- [ ] Nenhum script inline pesado no `<head>`
- [ ] Event listeners de scroll com `requestAnimationFrame`
- [ ] Tarefas JS longas divididas em chunks

### Imagens
- [ ] Formato WebP (com fallback JPG/PNG)
- [ ] `srcset` e `sizes` para imagens responsivas
- [ ] `loading="lazy"` em imagens abaixo do fold
- [ ] `object-fit: cover` para manter proporção

### Geral
- [ ] Lighthouse score ≥ 90 em Performance (mobile)
- [ ] Recursos de terceiros carregados de forma lazy
- [ ] Total de JS < 200KB (gzipped)
- [ ] Total de CSS < 50KB (gzipped)

---

## Comunicação por Perfil

### Para PM/Designer:
"O site foi otimizado para carregar rapidamente mesmo em conexões lentas — isso melhora a experiência dos usuários e o posicionamento no Google."

### Para Dev:
"Implementei: preload do hero com fetchpriority=high, CSS crítico inline, scripts com defer, imagens WebP com srcset/sizes, contain: layout nas cards e RAF throttling nos scroll listeners. Pronto para Lighthouse ≥ 90."
