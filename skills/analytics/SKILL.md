---
name: analytics
description: Use este skill quando o usuário mencionar analytics, Google Analytics, GA4, rastreamento, conversões, eventos, funil de vendas ou quiser medir o comportamento dos visitantes. Cobre instalação do GA4, eventos customizados, conversões e boas práticas de privacidade.
---

# Skill: Analytics — GA4 e Eventos de Conversão

Este skill garante integração correta com Google Analytics 4, com rastreamento de eventos relevantes para o negócio e respeito à privacidade dos usuários.

---

## Quando Aplicar

- Usuário menciona "quero saber quantas pessoas acessam"
- Pedido de rastreamento de cliques, formulários ou conversões
- Menção a Google Analytics, GA4, métricas ou funil

---

## 1. Instalação do GA4

### Método recomendado (lazy load após interação):
```html
<script>
  // ID do GA4 — substituir pelo ID real do cliente
  const GA_ID = 'G-XXXXXXXXXX';

  function loadGA() {
    // Evitar carregamento duplo
    if (window.gaLoaded) return;
    window.gaLoaded = true;

    const script = document.createElement('script');
    script.src = `https://www.googletagmanager.com/gtag/js?id=${GA_ID}`;
    script.async = true;
    document.head.appendChild(script);

    window.dataLayer = window.dataLayer || [];
    function gtag(){ dataLayer.push(arguments); }
    window.gtag = gtag;

    gtag('js', new Date());
    gtag('config', GA_ID, {
      // Anonimizar IP (LGPD/GDPR)
      anonymize_ip: true,
      // Não enviar page_view automático (controlar manualmente)
      send_page_view: false
    });

    // Page view manual após consentimento
    gtag('event', 'page_view', {
      page_title: document.title,
      page_location: window.location.href
    });
  }

  // Carregar após primeira interação do usuário
  ['click', 'scroll', 'touchstart', 'keydown'].forEach(event => {
    document.addEventListener(event, loadGA, { once: true });
  });
</script>
```

### Método simples (carregamento imediato):
```html
<!-- No <head>, após meta tags -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){ dataLayer.push(arguments); }
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX', { anonymize_ip: true });
</script>
```

---

## 2. Eventos Padrão a Rastrear

### Formulários:
```javascript
// Envio de formulário de contato
document.querySelector('#form-contato').addEventListener('submit', (e) => {
  gtag('event', 'form_submit', {
    event_category: 'engagement',
    event_label: 'contato',
    form_id: 'form-contato'
  });
});

// Início de preenchimento (interesse)
document.querySelector('#form-contato input:first-child').addEventListener('focus', () => {
  gtag('event', 'form_start', {
    event_category: 'engagement',
    form_id: 'form-contato'
  });
}, { once: true });
```

### CTAs e botões principais:
```javascript
// Rastrear todos os CTAs com data-attribute
document.querySelectorAll('[data-track]').forEach(el => {
  el.addEventListener('click', () => {
    gtag('event', 'cta_click', {
      event_category: 'engagement',
      event_label: el.getAttribute('data-track'),
      element_text: el.textContent.trim()
    });
  });
});

// Uso no HTML:
// <button data-track="hero-cta">Fale conosco</button>
// <a href="/contato" data-track="nav-contato">Contato</a>
```

### WhatsApp (muito comum em sites BR):
```javascript
document.querySelectorAll('a[href*="wa.me"]').forEach(link => {
  link.addEventListener('click', () => {
    gtag('event', 'whatsapp_click', {
      event_category: 'contact',
      event_label: link.closest('section')?.id || 'unknown_section'
    });
  });
});
```

### Scroll depth:
```javascript
const milestones = [25, 50, 75, 90];
const reached = new Set();

window.addEventListener('scroll', () => {
  const scrolled = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;

  milestones.forEach(milestone => {
    if (scrolled >= milestone && !reached.has(milestone)) {
      reached.add(milestone);
      gtag('event', 'scroll_depth', {
        event_category: 'engagement',
        event_label: `${milestone}%`,
        value: milestone
      });
    }
  });
});
```

### Tempo na página:
```javascript
// Engajamento por tempo
[30, 60, 120, 300].forEach(seconds => {
  setTimeout(() => {
    gtag('event', 'time_on_page', {
      event_category: 'engagement',
      event_label: `${seconds}s`,
      value: seconds
    });
  }, seconds * 1000);
});
```

---

## 3. Eventos de Conversão

Marcar como conversão no painel GA4 os eventos mais importantes:

```javascript
// Conversão principal: formulário enviado
function trackConversion(tipo) {
  gtag('event', 'conversion', {
    send_to: 'G-XXXXXXXXXX',
    conversion_type: tipo, // 'form_contact', 'whatsapp', 'purchase_intent'
    value: 1.0,
    currency: 'BRL'
  });
}

// Para e-commerce lite: intenção de compra
function trackPurchaseIntent(produto, valor) {
  gtag('event', 'begin_checkout', {
    currency: 'BRL',
    value: valor,
    items: [{
      item_name: produto.nome,
      item_category: produto.categoria,
      price: produto.preco,
      quantity: 1
    }]
  });
}

// Clique em WhatsApp de pedido = conversão
function trackWhatsAppOrder(total) {
  gtag('event', 'purchase', {
    transaction_id: `WA-${Date.now()}`,
    currency: 'BRL',
    value: total
  });
}
```

---

## 4. Banner de Consentimento (LGPD)

Obrigatório para sites brasileiros que coletam dados:

```html
<div id="cookie-banner" class="cookie-banner" role="alertdialog" aria-labelledby="cookie-title" hidden>
  <div class="cookie-content">
    <h3 id="cookie-title">Cookies e Privacidade</h3>
    <p>
      Usamos cookies para analisar o tráfego e melhorar sua experiência.
      Ao continuar, você concorda com nossa
      <a href="/privacidade">Política de Privacidade</a>.
    </p>
    <div class="cookie-actions">
      <button onclick="aceitarCookies()" class="btn-aceitar">Aceitar</button>
      <button onclick="recusarCookies()" class="btn-recusar">Recusar</button>
    </div>
  </div>
</div>

<script>
  function verificarConsentimento() {
    const consentimento = localStorage.getItem('cookie-consent');
    if (!consentimento) {
      document.getElementById('cookie-banner').hidden = false;
    } else if (consentimento === 'accepted') {
      loadGA(); // só carrega analytics se aceito
    }
  }

  function aceitarCookies() {
    localStorage.setItem('cookie-consent', 'accepted');
    document.getElementById('cookie-banner').hidden = true;
    loadGA();
  }

  function recusarCookies() {
    localStorage.setItem('cookie-consent', 'rejected');
    document.getElementById('cookie-banner').hidden = true;
    // GA não é carregado
  }

  document.addEventListener('DOMContentLoaded', verificarConsentimento);
</script>
```

```css
.cookie-banner {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #1e293b;
  color: #f8fafc;
  padding: 1.5rem;
  z-index: 9999;
  box-shadow: 0 -4px 24px rgba(0,0,0,0.2);
}

.cookie-actions {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.btn-aceitar {
  background: #2563eb;
  color: white;
  border: none;
  padding: 0.5rem 1.5rem;
  border-radius: 6px;
  cursor: pointer;
}

.btn-recusar {
  background: transparent;
  color: #94a3b8;
  border: 1px solid #475569;
  padding: 0.5rem 1.5rem;
  border-radius: 6px;
  cursor: pointer;
}
```

---

## 5. Eventos por Tipo de Site

### Site institucional / landing page:
| Evento | Quando rastrear |
|---|---|
| `page_view` | Carregamento da página |
| `form_start` | Primeiro campo focado |
| `form_submit` | Formulário enviado |
| `cta_click` | Clique em qualquer CTA |
| `whatsapp_click` | Clique no link do WhatsApp |
| `scroll_depth` | 25%, 50%, 75%, 90% |

### E-commerce lite:
| Evento | Quando rastrear |
|---|---|
| `view_item` | Produto visualizado |
| `add_to_cart` | Item adicionado ao carrinho |
| `view_cart` | Carrinho aberto |
| `begin_checkout` | Início da finalização |
| `purchase` | Pedido enviado (WhatsApp/form) |

---

## 6. Checklist Analytics

- [ ] ID do GA4 configurado (substituir `G-XXXXXXXXXX`)
- [ ] `anonymize_ip: true` ativado (LGPD)
- [ ] Banner de consentimento presente e funcional
- [ ] GA carregado somente após consentimento
- [ ] Evento `page_view` rastreado
- [ ] Formulários com `form_start` e `form_submit`
- [ ] CTAs com `data-track` e listener configurado
- [ ] Links de WhatsApp rastreados
- [ ] Scroll depth configurado
- [ ] Conversões principais identificadas
- [ ] Nenhum dado pessoal enviado para o GA (nomes, e-mails, CPF)

---

## Comunicação por Perfil

### Para PM/Designer:
"Configurei o Google Analytics para registrar as ações mais importantes: quando alguém preenche o formulário, clica no WhatsApp ou passa mais de 2 minutos no site. Você vai ver essas métricas no painel do GA4. Também adicionei o aviso de cookies conforme a LGPD."

### Para Dev:
"GA4 com lazy loading pós-interação, consentimento LGPD via localStorage, anonymize_ip, eventos customizados com data-track attribute pattern, scroll depth em 4 milestones e eventos de e-commerce (view_item, add_to_cart, purchase) para o catálogo."
