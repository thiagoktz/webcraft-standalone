---
name: seo
description: Use este skill em toda geração de website pelo WebCraft Agent. Garante estrutura semântica correta, meta tags completas e boas práticas de SEO on-page antes da entrega.
---

# Skill: SEO — Otimização de Meta Tags e Estrutura

Este skill garante que todo site gerado tenha uma base sólida de SEO on-page, aumentando suas chances de ser encontrado nos mecanismos de busca.

---

## Quando Aplicar

- Em toda geração de site (aplicar sempre por padrão)
- Ao processar feedback que mencione "aparecer no Google", "ser encontrado", "SEO"
- Antes de qualquer deploy

---

## 1. Meta Tags Obrigatórias

Todo `<head>` deve conter no mínimo:

```html
<!-- Base -->
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="[150-160 caracteres descrevendo a página]">
<meta name="robots" content="index, follow">

<!-- Open Graph (compartilhamento em redes sociais) -->
<meta property="og:title" content="[Título da página]">
<meta property="og:description" content="[Descrição com até 160 caracteres]">
<meta property="og:type" content="website">
<meta property="og:url" content="[URL canônica da página]">
<meta property="og:image" content="[URL de imagem 1200x630px]">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="[Título]">
<meta name="twitter:description" content="[Descrição]">
<meta name="twitter:image" content="[URL da imagem]">

<!-- Canônica -->
<link rel="canonical" href="[URL principal da página]">

<!-- Favicon -->
<link rel="icon" type="image/png" href="/favicon.png">

<title>[Palavra-chave principal] | [Nome da marca]</title>
```

---

## 2. Estrutura Semântica de Headings

### Regras obrigatórias:
- **Apenas 1 `<h1>` por página** — deve conter a palavra-chave principal
- `<h2>` para seções principais
- `<h3>` para subseções dentro das seções
- Nunca pule níveis (h1 → h3 sem h2)

### Exemplo correto:
```html
<h1>Fisioterapia Especializada em São Paulo</h1>  <!-- 1 por página -->
  <h2>Nossos Serviços</h2>
    <h3>Fisioterapia Ortopédica</h3>
    <h3>Fisioterapia Neurológica</h3>
  <h2>Sobre a Clínica</h2>
  <h2>Depoimentos</h2>
  <h2>Como Chegar</h2>
```

---

## 3. Imagens

Toda `<img>` deve ter:
```html
<img
  src="foto-equipe.jpg"
  alt="Equipe de fisioterapeutas da Clínica Saúde em movimento"
  width="800"
  height="600"
  loading="lazy"
>
```

- `alt` deve ser descritivo e contextual (não "imagem1.jpg")
- `width` e `height` evitam layout shift (CLS)
- `loading="lazy"` para imagens abaixo do fold
- `loading="eager"` para imagens no hero (above the fold)

---

## 4. Performance (Core Web Vitals)

### LCP (Largest Contentful Paint) — meta: < 2.5s
- Pré-carregue a imagem hero:
  ```html
  <link rel="preload" as="image" href="hero.jpg">
  ```
- Use fontes com `font-display: swap`:
  ```css
  @import url('https://fonts.googleapis.com/css2?family=...&display=swap');
  ```

### CLS (Cumulative Layout Shift) — meta: < 0.1
- Sempre defina `width` e `height` em imagens
- Reserve espaço para elementos carregados dinamicamente

### FID / INP (Interatividade) — meta: < 200ms
- Evite JavaScript bloqueante no `<head>`
- Use `defer` ou `async` em scripts externos:
  ```html
  <script src="script.js" defer></script>
  ```

---

## 5. Dados Estruturados (Schema.org)

Adicione JSON-LD conforme o tipo de site:

### Para negócio local (clínica, restaurante, loja):
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Nome do Negócio",
  "description": "Descrição",
  "url": "https://www.site.com.br",
  "telephone": "+55 11 99999-9999",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Rua Exemplo, 123",
    "addressLocality": "São Paulo",
    "addressRegion": "SP",
    "postalCode": "01310-100",
    "addressCountry": "BR"
  }
}
</script>
```

### Para produto/SaaS:
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Nome do Produto",
  "applicationCategory": "BusinessApplication",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "BRL"
  }
}
</script>
```

---

## 6. Checklist SEO Pré-Entrega

- [ ] `<title>` único e com palavra-chave principal (50-60 caracteres)
- [ ] `<meta description>` descritiva e com CTA (150-160 caracteres)
- [ ] Apenas 1 `<h1>` por página
- [ ] Hierarquia de headings sem pulos
- [ ] Todas as imagens com `alt` descritivo
- [ ] `width` e `height` definidos em todas as imagens
- [ ] Tags Open Graph completas
- [ ] `<link rel="canonical">` presente
- [ ] Scripts com `defer` ou `async`
- [ ] Fonte carregada com `display=swap`
- [ ] JSON-LD adequado ao tipo de negócio
- [ ] Favicon configurado

---

## Comunicação por Perfil

### Para PM/Designer:
"Adicionei as configurações que ajudam o site a aparecer no Google — título otimizado, descrição da página e configuração para compartilhamento em redes sociais."

### Para Dev:
"Implementei meta tags completas (OG, Twitter Card, canônica), schema.org LocalBusiness, preload do hero image e scripts com defer para otimizar LCP e CLS."
