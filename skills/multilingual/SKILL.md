---
name: multilingual
description: Use este skill quando o usuário pedir um site em mais de um idioma, mencionar internacionalização, i18n, tradução de conteúdo ou público em diferentes países. Cobre estrutura de URLs, atributos hreflang, sistema de traduções em JSON e alternância de idioma sem reload.
---

# Skill: Multilingual — Sites em Mais de Um Idioma

Este skill garante que sites com múltiplos idiomas sejam implementados corretamente, tanto para usuários quanto para mecanismos de busca.

---

## Quando Aplicar

- Usuário menciona "dois idiomas", "inglês e português", "site internacional"
- Público-alvo em mais de um país ou idioma
- Usuário menciona i18n, internacionalização ou localização

---

## 1. Estrutura de URLs

Escolha uma estratégia e mantenha consistência:

| Estratégia | Exemplo | Quando usar |
|---|---|---|
| Subdiretório | `/pt/`, `/en/` | Mais comum, fácil de implementar |
| Subdomínio | `pt.site.com`, `en.site.com` | Grandes portais |
| Domínio separado | `site.com.br`, `site.com` | Marcas distintas por país |

**Recomendação padrão:** subdiretório (`/pt/`, `/en/`).

---

## 2. Atributos hreflang (SEO Multilingual)

Todo `<head>` deve declarar todas as versões de idioma:

```html
<!-- Para site PT + EN -->
<link rel="alternate" hreflang="pt-BR" href="https://site.com/pt/">
<link rel="alternate" hreflang="en-US" href="https://site.com/en/">
<link rel="alternate" hreflang="x-default" href="https://site.com/en/">

<!-- x-default = idioma padrão quando nenhum corresponde ao usuário -->
```

**Regras:**
- Cada página deve referenciar TODAS as versões (inclusive ela mesma)
- Use códigos completos: `pt-BR`, `en-US`, `es-ES` (não só `pt` ou `en`)
- `x-default` deve apontar para a versão mais universal (geralmente inglês)

---

## 3. Atributo lang no HTML

```html
<!-- Português brasileiro -->
<html lang="pt-BR">

<!-- Inglês americano -->
<html lang="en-US">

<!-- Espanhol da Espanha -->
<html lang="es-ES">
```

Para trechos em idioma diferente dentro da página:
```html
<p>Nossa empresa é referência em <span lang="en">machine learning</span> no Brasil.</p>
```

---

## 4. Sistema de Traduções em JSON

Organize traduções em arquivos separados por idioma:

```javascript
// translations/pt-BR.js
const pt = {
  nav: {
    home: "Início",
    about: "Sobre",
    services: "Serviços",
    contact: "Contato"
  },
  hero: {
    title: "Transforme seu negócio com tecnologia",
    subtitle: "Soluções sob medida para empresas que querem crescer",
    cta: "Fale conosco"
  },
  footer: {
    rights: "Todos os direitos reservados"
  }
};

// translations/en-US.js
const en = {
  nav: {
    home: "Home",
    about: "About",
    services: "Services",
    contact: "Contact"
  },
  hero: {
    title: "Transform your business with technology",
    subtitle: "Tailored solutions for companies ready to grow",
    cta: "Get in touch"
  },
  footer: {
    rights: "All rights reserved"
  }
};
```

---

## 5. Engine de i18n (Vanilla JS)

```javascript
// i18n.js
const translations = { 'pt-BR': pt, 'en-US': en };

function detectLanguage() {
  // 1. Verificar URL (/pt/ ou /en/)
  const path = window.location.pathname;
  if (path.startsWith('/pt')) return 'pt-BR';
  if (path.startsWith('/en')) return 'en-US';

  // 2. Verificar localStorage (preferência salva)
  const saved = localStorage.getItem('lang');
  if (saved && translations[saved]) return saved;

  // 3. Usar idioma do browser
  const browser = navigator.language;
  if (browser.startsWith('pt')) return 'pt-BR';

  // 4. Fallback
  return 'en-US';
}

function t(key) {
  const lang = detectLanguage();
  const keys = key.split('.');
  let value = translations[lang];
  for (const k of keys) {
    value = value?.[k];
  }
  return value || key;
}

function applyTranslations() {
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.getAttribute('data-i18n');
    el.textContent = t(key);
  });

  // Atualizar lang do HTML
  document.documentElement.lang = detectLanguage();
}

function switchLanguage(lang) {
  localStorage.setItem('lang', lang);
  // Redirecionar para versão correta da URL
  const newPath = window.location.pathname.replace(/^\/(pt|en)/, `/${lang.slice(0, 2)}`);
  window.location.href = newPath || `/${lang.slice(0, 2)}/`;
}

// Inicializar
document.addEventListener('DOMContentLoaded', applyTranslations);
```

### Uso no HTML:
```html
<nav>
  <a href="#" data-i18n="nav.home"></a>
  <a href="#" data-i18n="nav.about"></a>
</nav>

<h1 data-i18n="hero.title"></h1>
<p data-i18n="hero.subtitle"></p>
```

---

## 6. Seletor de Idioma

```html
<div class="language-switcher" role="navigation" aria-label="Selecionar idioma">
  <button
    onclick="switchLanguage('pt-BR')"
    aria-label="Mudar para Português"
    aria-pressed="true"
    lang="pt"
  >PT</button>
  <button
    onclick="switchLanguage('en-US')"
    aria-label="Switch to English"
    aria-pressed="false"
    lang="en"
  >EN</button>
</div>
```

---

## 7. Formatação Regional

Datas, moedas e números variam por localidade:

```javascript
const lang = detectLanguage();

// Datas
const data = new Date();
data.toLocaleDateString('pt-BR'); // "23/05/2026"
data.toLocaleDateString('en-US'); // "5/23/2026"

// Moeda
(1990.5).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' }); // R$ 1.990,50
(1990.5).toLocaleString('en-US', { style: 'currency', currency: 'USD' }); // $1,990.50

// Números
(1234567.89).toLocaleString('pt-BR'); // "1.234.567,89"
(1234567.89).toLocaleString('en-US'); // "1,234,567.89"
```

---

## 8. Checklist Multilingual

- [ ] Estratégia de URL definida e consistente (`/pt/`, `/en/`)
- [ ] `hreflang` em todas as páginas apontando para todas as versões
- [ ] `x-default` configurado
- [ ] `lang` correto no `<html>` de cada versão
- [ ] Arquivos de tradução separados por idioma
- [ ] Nenhum texto hardcoded no HTML (tudo via `data-i18n`)
- [ ] Seletor de idioma acessível com `aria-label`
- [ ] Idioma persistido em localStorage
- [ ] Datas e moedas formatadas com `toLocaleString`
- [ ] Fonte suporta caracteres especiais dos idiomas alvo
- [ ] Meta tags (`title`, `description`, OG) traduzidas por versão

---

## Comunicação por Perfil

### Para PM/Designer:
"O site detecta automaticamente o idioma do visitante e permite troca manual. Cada versão tem seu próprio endereço e está configurada para aparecer nos resultados de busca do país correto."

### Para Dev:
"Implementei i18n com JSON de traduções, detecção por URL/localStorage/navigator.language, hreflang completo com x-default, e formatação regional via toLocaleString. Sistema extensível para novos idiomas adicionando um arquivo de tradução."
