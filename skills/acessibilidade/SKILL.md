---
name: acessibilidade
description: Use este skill em toda geração de website pelo WebCraft Agent. Garante conformidade com WCAG 2.1 nível AA, tornando o site acessível para pessoas com deficiências visuais, motoras, auditivas e cognitivas.
---

# Skill: Acessibilidade — WCAG 2.1 AA

Este skill garante que todo site gerado seja acessível ao maior número possível de pessoas, seguindo as diretrizes WCAG 2.1 no nível AA.

---

## Quando Aplicar

- Em toda geração de site (aplicar sempre por padrão)
- Ao processar feedback relacionado a acessibilidade
- Antes de qualquer deploy

---

## 1. Estrutura e Semântica

### HTML semântico obrigatório:
```html
<!-- Landmarks para navegação por leitores de tela -->
<header role="banner">...</header>
<nav role="navigation" aria-label="Menu principal">...</nav>
<main role="main">...</main>
<aside role="complementary">...</aside>
<footer role="contentinfo">...</footer>
```

### Skip link (primeiro elemento do body):
```html
<a href="#main-content" class="skip-link">Pular para o conteúdo principal</a>

<style>
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px 16px;
  z-index: 9999;
}
.skip-link:focus {
  top: 0;
}
</style>
```

---

## 2. Contraste de Cores

### Requisitos WCAG AA:
| Elemento | Ratio mínimo |
|---|---|
| Texto normal (< 18pt) | 4.5:1 |
| Texto grande (≥ 18pt ou 14pt bold) | 3:1 |
| Componentes de UI e gráficos | 3:1 |

### Combinações seguras de exemplo:
```css
/* Fundo escuro */
--bg: #0f172a;
--text: #f8fafc;      /* ratio 17:1 ✅ */
--accent: #38bdf8;    /* ratio 4.6:1 ✅ */

/* Fundo claro */
--bg: #ffffff;
--text: #1e293b;      /* ratio 16:1 ✅ */
--accent: #1d4ed8;    /* ratio 5.9:1 ✅ */
```

### Nunca use só cor para transmitir informação:
```html
<!-- ❌ Errado: só cor indica erro -->
<input style="border-color: red">

<!-- ✅ Correto: cor + ícone + texto -->
<input aria-describedby="email-error" aria-invalid="true">
<span id="email-error">⚠ E-mail inválido</span>
```

---

## 3. Teclado e Foco

### Todo elemento interativo deve ser acessível por teclado:
```css
/* Nunca remova o outline sem substituir */
:focus {
  outline: 3px solid #2563eb;
  outline-offset: 2px;
}

/* Para design mais refinado */
:focus-visible {
  outline: 3px solid #2563eb;
  outline-offset: 2px;
  border-radius: 4px;
}
```

### Ordem de tabulação lógica:
- Deve seguir a ordem visual da página
- Nunca use `tabindex` maior que 0
- Use `tabindex="0"` para tornar elementos não-interativos focáveis
- Use `tabindex="-1"` para remover do fluxo de tabulação

### Armadilhas de foco (modais e dropdowns):
```javascript
// Ao abrir modal, prender foco dentro dele
const focusableElements = modal.querySelectorAll(
  'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
);
// Implementar ciclo de foco entre primeiro e último elemento
```

---

## 4. Imagens e Mídia

### Imagens:
```html
<!-- Imagem informativa -->
<img src="grafico.png" alt="Gráfico mostrando crescimento de 40% nas vendas em 2025">

<!-- Imagem decorativa (não adiciona informação) -->
<img src="fundo-decorativo.png" alt="" role="presentation">

<!-- Ícone com texto adjacente -->
<img src="icone-email.svg" alt=""> <!-- alt vazio quando texto já descreve -->
<span>contato@empresa.com</span>
```

### Vídeos:
- Sempre oferecer legendas (CC)
- Não iniciar com autoplay + som
- Fornecer transcrição para conteúdo importante

---

## 5. Formulários

```html
<!-- Todo input precisa de label associado -->
<label for="nome">Nome completo</label>
<input
  id="nome"
  type="text"
  name="nome"
  autocomplete="name"
  required
  aria-required="true"
>

<!-- Mensagem de erro acessível -->
<input
  id="email"
  type="email"
  aria-describedby="email-hint email-error"
  aria-invalid="true"
>
<span id="email-hint">Formato: nome@exemplo.com</span>
<span id="email-error" role="alert">Por favor, insira um e-mail válido</span>

<!-- Grupo de opções -->
<fieldset>
  <legend>Forma de contato preferida</legend>
  <input type="radio" id="whatsapp" name="contato" value="whatsapp">
  <label for="whatsapp">WhatsApp</label>
  <input type="radio" id="email-opt" name="contato" value="email">
  <label for="email-opt">E-mail</label>
</fieldset>
```

---

## 6. ARIA — Uso Correto

### Use ARIA apenas quando HTML semântico não basta:
```html
<!-- Botão que expande/colapsa -->
<button aria-expanded="false" aria-controls="menu-mobile">
  Menu
</button>

<!-- Região com conteúdo dinâmico -->
<div role="alert" aria-live="polite">
  Mensagem enviada com sucesso!
</div>

<!-- Navegação com múltiplos navs -->
<nav aria-label="Menu principal">...</nav>
<nav aria-label="Breadcrumb">...</nav>
<nav aria-label="Rodapé">...</nav>
```

### Nunca use ARIA incorretamente:
```html
<!-- ❌ Errado -->
<div role="button" onclick="...">Clique aqui</div>

<!-- ✅ Correto -->
<button onclick="...">Clique aqui</button>
```

---

## 7. Tipografia e Legibilidade

```css
/* Tamanho mínimo de fonte */
body { font-size: 1rem; } /* = 16px, nunca menos */

/* Espaçamento para leitura */
p {
  line-height: 1.5;      /* mínimo WCAG */
  letter-spacing: 0.12em; /* opcional mas recomendado */
  max-width: 75ch;        /* linha confortável */
}

/* Nunca use px fixo para fonte — permite zoom do usuário */
/* ❌ */ font-size: 14px;
/* ✅ */ font-size: 0.875rem;

/* Texto não deve desaparecer ao zoom 200% */
/* Use unidades relativas (rem, em, %) */
```

---

## 8. Movimento e Animações

```css
/* Respeitar preferência do usuário por menos movimento */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

- Nunca use flashes acima de 3 por segundo (risco de convulsão)
- Animações em loop devem ter opção de pausa

---

## 9. Checklist WCAG AA Pré-Entrega

### Percebível
- [ ] Todas as imagens têm `alt` adequado
- [ ] Contraste de texto ≥ 4.5:1 (normal) ou 3:1 (grande)
- [ ] Contraste de componentes UI ≥ 3:1
- [ ] Informação não transmitida apenas por cor
- [ ] Vídeos têm legendas

### Operável
- [ ] Todo conteúdo acessível por teclado
- [ ] Foco visível em todos os elementos interativos
- [ ] Skip link presente e funcional
- [ ] Sem armadilhas de teclado
- [ ] `prefers-reduced-motion` implementado

### Compreensível
- [ ] `lang` definido no `<html>` (`lang="pt-BR"`)
- [ ] Labels em todos os campos de formulário
- [ ] Erros de formulário identificados e descritos
- [ ] Ordem de leitura lógica

### Robusto
- [ ] HTML válido (sem erros críticos)
- [ ] ARIA usado corretamente
- [ ] Funciona com leitores de tela (VoiceOver/NVDA)
- [ ] Funciona sem JavaScript para conteúdo essencial

---

## Comunicação por Perfil

### Para PM/Designer:
"O site foi construído para funcionar para todas as pessoas, incluindo quem usa leitores de tela ou navega só pelo teclado. Isso também melhora o SEO e é exigido por lei em alguns contextos."

### Para Dev:
"Implementei WCAG 2.1 AA: landmarks semânticos, skip link, contraste verificado (mínimo 4.5:1), aria-labels em elementos interativos, prefers-reduced-motion e formulários com labels e erros acessíveis."
