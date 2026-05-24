---
name: ecommerce-lite
description: Use este skill quando o usuário pedir um catálogo de produtos, loja virtual sem pagamento, vitrine online ou e-commerce simples. Cobre listagem de produtos, filtros, carrinho visual e formulário de pedido por WhatsApp ou e-mail — sem integração com gateway de pagamento.
---

# Skill: E-commerce Lite — Catálogo sem Pagamento Real

Este skill guia a criação de catálogos e vitrines online com experiência de compra completa, exceto pelo processamento de pagamento — que é substituído por WhatsApp, e-mail ou formulário de pedido.

---

## Escopo deste Skill

### Inclui ✅
- Listagem de produtos com foto, nome, descrição e preço
- Filtros por categoria, preço e atributos
- Busca de produtos
- Página de detalhe do produto
- Carrinho de compras (visual/local)
- Formulário de pedido (WhatsApp, e-mail ou formulário)
- Cálculo de total
- Layout responsivo mobile-first

### Não inclui ❌
- Gateway de pagamento (Stripe, PagSeguro, Mercado Pago)
- Autenticação de usuários
- Painel administrativo de produtos
- Gestão de estoque em tempo real
- Integração com ERP ou sistemas externos

---

## 1. Estrutura de Dados dos Produtos

Use um array JavaScript como fonte de dados:

```javascript
const produtos = [
  {
    id: 1,
    nome: "Camiseta Básica Algodão",
    categoria: "Roupas",
    preco: 79.90,
    precoOriginal: 99.90, // opcional, para mostrar desconto
    descricao: "100% algodão, disponível em P, M, G e GG",
    imagem: "https://placehold.co/400x400?text=Camiseta",
    atributos: {
      tamanhos: ["P", "M", "G", "GG"],
      cores: ["Branco", "Preto", "Cinza"]
    },
    destaque: true,
    disponivel: true
  }
];
```

---

## 2. Layout das Páginas

### Página principal (catálogo):
```
[Header com logo + carrinho (ícone + contador)]
[Banner hero opcional]
[Barra de busca]
[Filtros laterais ou horizontais]
[Grid de produtos]
  └── Card: imagem, nome, preço, botão "Adicionar"]
[Paginação ou "Ver mais"]
[Footer]
```

### Card de produto:
```html
<article class="produto-card">
  <div class="produto-imagem">
    <img src="..." alt="[nome do produto]" loading="lazy">
    <span class="badge-desconto" aria-label="20% de desconto">-20%</span>
  </div>
  <div class="produto-info">
    <h3 class="produto-nome">Camiseta Básica</h3>
    <div class="produto-precos">
      <span class="preco-original" aria-label="De R$ 99,90">R$ 99,90</span>
      <span class="preco-atual" aria-label="Por R$ 79,90">R$ 79,90</span>
    </div>
    <button class="btn-adicionar" onclick="adicionarAoCarrinho(1)">
      Adicionar ao carrinho
    </button>
  </div>
</article>
```

---

## 3. Carrinho de Compras

Usar `localStorage` para persistir o carrinho:

```javascript
// Estrutura do carrinho
let carrinho = JSON.parse(localStorage.getItem('carrinho')) || [];

function adicionarAoCarrinho(produtoId, quantidade = 1, atributos = {}) {
  const produto = produtos.find(p => p.id === produtoId);
  const itemExistente = carrinho.find(
    item => item.id === produtoId &&
    JSON.stringify(item.atributos) === JSON.stringify(atributos)
  );

  if (itemExistente) {
    itemExistente.quantidade += quantidade;
  } else {
    carrinho.push({ ...produto, quantidade, atributos });
  }

  localStorage.setItem('carrinho', JSON.stringify(carrinho));
  atualizarContadorCarrinho();
  mostrarFeedbackAdicao();
}

function calcularTotal() {
  return carrinho.reduce((total, item) => total + (item.preco * item.quantidade), 0);
}

function formatarPreco(valor) {
  return valor.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
}
```

---

## 4. Finalização do Pedido

### Opção A — WhatsApp (mais comum no Brasil):
```javascript
function finalizarPorWhatsApp() {
  const telefone = '5511999999999'; // número da loja
  const itens = carrinho.map(item =>
    `• ${item.nome} (${item.quantidade}x) — ${formatarPreco(item.preco * item.quantidade)}`
  ).join('\n');

  const mensagem = encodeURIComponent(
    `Olá! Gostaria de fazer um pedido:\n\n${itens}\n\nTotal: ${formatarPreco(calcularTotal())}`
  );

  window.open(`https://wa.me/${telefone}?text=${mensagem}`, '_blank');
}
```

### Opção B — Formulário de pedido por e-mail:
```html
<form id="form-pedido" onsubmit="enviarPedido(event)">
  <label for="nome-cliente">Nome completo</label>
  <input id="nome-cliente" type="text" required autocomplete="name">

  <label for="email-cliente">E-mail</label>
  <input id="email-cliente" type="email" required autocomplete="email">

  <label for="telefone-cliente">Telefone / WhatsApp</label>
  <input id="telefone-cliente" type="tel" autocomplete="tel">

  <label for="endereco">Endereço de entrega</label>
  <textarea id="endereco" rows="3"></textarea>

  <label for="observacoes">Observações</label>
  <textarea id="observacoes" rows="2" placeholder="Variações, dúvidas..."></textarea>

  <div class="resumo-pedido" id="resumo-pedido">
    <!-- Preenchido via JS com os itens do carrinho -->
  </div>

  <button type="submit">Enviar pedido</button>
</form>
```

---

## 5. Filtros e Busca

```javascript
function filtrarProdutos({ categoria, precoMax, busca }) {
  return produtos.filter(produto => {
    const matchCategoria = !categoria || produto.categoria === categoria;
    const matchPreco = !precoMax || produto.preco <= precoMax;
    const matchBusca = !busca ||
      produto.nome.toLowerCase().includes(busca.toLowerCase()) ||
      produto.descricao.toLowerCase().includes(busca.toLowerCase());

    return matchCategoria && matchPreco && matchBusca && produto.disponivel;
  });
}
```

---

## 6. Boas Práticas de UX para Catálogo

- **Imagens:** sempre quadradas (1:1) ou consistentes — use `object-fit: cover`
- **Grid responsivo:** 1 coluna mobile, 2 tablet, 3-4 desktop
- **Feedback imediato:** ao adicionar ao carrinho, mostrar animação ou toast
- **Produto indisponível:** mostrar com opacidade reduzida e botão desabilitado
- **Preço:** sempre em destaque, desconto em cor diferente
- **Frete:** ser transparente — informar "frete a combinar" se não calculado

```css
.produto-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 1.5rem;
}

.produto-imagem img {
  width: 100%;
  aspect-ratio: 1;
  object-fit: cover;
}
```

---

## 7. Checklist E-commerce Lite

- [ ] Array de produtos estruturado e tipado
- [ ] Imagens com `alt` descritivo e `aspect-ratio` consistente
- [ ] Filtros por categoria funcionando
- [ ] Busca funcionando
- [ ] Adicionar ao carrinho com feedback visual
- [ ] Contador do carrinho atualizado em tempo real
- [ ] Cálculo de total correto
- [ ] Formatação de preço em pt-BR (R$)
- [ ] Finalização por WhatsApp ou formulário
- [ ] Mensagem de WhatsApp pré-formatada com itens
- [ ] Produtos indisponíveis tratados visualmente
- [ ] Carrinho persistido em localStorage
- [ ] Layout responsivo (mobile-first)

---

## Comunicação por Perfil

### Para PM/Designer:
"Criei uma vitrine completa onde seus clientes podem navegar pelos produtos, filtrar por categoria e finalizar o pedido direto pelo WhatsApp — sem precisar de sistema de pagamento."

### Para Dev:
"Implementei catálogo com array de dados local, filtros e busca client-side, carrinho persistido em localStorage e finalização via deep link WhatsApp com mensagem pré-formatada. Fácil de migrar para API REST depois."
