# Evals — Testes de Avaliação dos Skills
## WebCraft Agent

**Versão:** 1.0  
**Data:** Maio 2026  
**Objetivo:** Garantir que cada skill produz outputs de qualidade consistente e detectar regressões após atualizações.

---

## Como usar estes evals

1. Para cada skill, use o **prompt de teste** como input para o agente
2. Compare o output com os **critérios de aprovação**
3. Marque cada critério como ✅ aprovado ou ❌ reprovado
4. Se ≥ 80% dos critérios forem aprovados, o skill está saudável
5. Documente falhas e abra issue no repositório

---

## EVAL-01: Skill de Intake

### Teste 1.1 — Brief vago de PM
**Input:**
```
Quero um site para meu negócio de bolos artesanais.
```

**Critérios de aprovação:**
- [ ] Detecta perfil como PM/Designer (não usa jargão técnico)
- [ ] Infere tipo de site (institucional / portfólio / cardápio)
- [ ] Não faz mais de 3 perguntas antes de agir
- [ ] Gera ou inicia geração de uma v1 sem travar esperando respostas
- [ ] Tom e seções inferidas são adequados ao segmento (bolos/alimentação)

---

### Teste 1.2 — Brief técnico de Dev
**Input:**
```
Preciso de uma landing page em React com Tailwind. Hero com CTA, seção de features e footer. Deploy no Vercel.
```

**Critérios de aprovação:**
- [ ] Detecta perfil como Desenvolvedor
- [ ] Usa React + Tailwind conforme solicitado
- [ ] Não pergunta coisas desnecessárias (stack já foi informada)
- [ ] Menciona ou planeja deploy no Vercel
- [ ] Entrega código, não apenas descrição

---

### Teste 1.3 — Detecção de perfil misto
**Input:**
```
Quero um site bonito para minha clínica. Pode usar React se for mais fácil.
```

**Critérios de aprovação:**
- [ ] Não assume perfil Dev só porque "React" foi mencionado casualmente
- [ ] Pergunta ou assume postura de PM como padrão mais seguro
- [ ] Oferece entregar em HTML/CSS/JS se preferir (mais simples)
- [ ] Não expõe código desnecessariamente na resposta inicial

---

## EVAL-02: Skill de Frontend Design

### Teste 2.1 — Identidade visual distintiva
**Input:**
```
Cria uma landing page para um estúdio de tatuagem chamado "Tinta Negra".
```

**Critérios de aprovação:**
- [ ] Não usa paleta genérica (sem purple gradient em fundo branco)
- [ ] Tipografia distintiva (não Arial, Roboto ou Inter)
- [ ] Tem ponto de vista estético claro (dark, editorial, brutal, etc.)
- [ ] Animações ou micro-interações presentes
- [ ] Layout tem elemento memorável (não é template genérico)
- [ ] CSS usa variáveis customizadas (`--color-*`, `--font-*`)

---

### Teste 2.2 — Responsividade
**Input:**
```
Gera uma landing page para um app de meditação chamado "Calma".
```

**Critérios de aprovação:**
- [ ] Mobile-first (estilos base para mobile, media queries para desktop)
- [ ] Funciona em 375px (iPhone SE), 768px (tablet) e 1280px (desktop)
- [ ] Imagens usam `max-width: 100%` ou equivalente
- [ ] Texto legível sem zoom em mobile
- [ ] Navegação adaptada para mobile (hamburger ou stack)

---

### Teste 2.3 — Coerência estética
**Input:**
```
Quero algo minimalista e premium para um escritório de arquitetura.
```

**Critérios de aprovação:**
- [ ] Paleta restrita (máximo 3 cores)
- [ ] Muito espaço negativo
- [ ] Tipografia refinada e bem espaçada
- [ ] Sem animações excessivas ou chamarizes visuais
- [ ] Não contradiz o briefing (ex: não gera algo colorido e cheio de elementos)

---

## EVAL-03: Skill de SEO

### Teste 3.1 — Meta tags completas
**Input:**
```
Gera um site para uma escola de inglês chamada "Speak Up" em Campinas.
```

**Critérios de aprovação:**
- [ ] `<title>` entre 50-60 caracteres com palavra-chave
- [ ] `<meta description>` entre 150-160 caracteres
- [ ] Tags Open Graph presentes (og:title, og:description, og:type, og:url)
- [ ] Twitter Card presente
- [ ] `<link rel="canonical">` presente
- [ ] Favicon referenciado
- [ ] `lang="pt-BR"` no `<html>`

---

### Teste 3.2 — Estrutura de headings
**Input:**
```
Landing page para um coach de carreira chamado "Marcos Duarte".
```

**Critérios de aprovação:**
- [ ] Apenas 1 `<h1>` na página
- [ ] `<h1>` contém palavra-chave relevante (não só o nome)
- [ ] Hierarquia correta (h1 → h2 → h3, sem pulos)
- [ ] Headings descrevem o conteúdo da seção

---

### Teste 3.3 — Performance básica
**Critérios de aprovação (verificar no código gerado):**
- [ ] Scripts externos com `defer` ou `async`
- [ ] Google Fonts com `display=swap`
- [ ] Imagens hero com `loading="eager"`, demais com `loading="lazy"`
- [ ] `width` e `height` definidos nas imagens
- [ ] JSON-LD presente e adequado ao tipo de negócio

---

## EVAL-04: Skill de Acessibilidade

### Teste 4.1 — Estrutura semântica e navegação
**Input:**
```
Site para uma ONG de adoção de animais chamada "Lar Feliz".
```

**Critérios de aprovação:**
- [ ] Skip link presente como primeiro elemento do body
- [ ] Landmarks semânticos: `<header>`, `<nav>`, `<main>`, `<footer>`
- [ ] `aria-label` em navegações múltiplas
- [ ] `lang="pt-BR"` no `<html>`
- [ ] Ordem de tabulação lógica

---

### Teste 4.2 — Contraste e cor
**Critérios de aprovação (verificar no CSS gerado):**
- [ ] Nenhuma combinação de texto/fundo abaixo de 4.5:1 (texto normal)
- [ ] Informação nunca transmitida só por cor
- [ ] Foco visível em elementos interativos (`:focus` ou `:focus-visible` definido)
- [ ] `outline: none` não usado sem alternativa

---

### Teste 4.3 — Formulários acessíveis
**Input:**
```
Landing page com formulário de contato para um dentista.
```

**Critérios de aprovação:**
- [ ] Todo `<input>` tem `<label>` associado por `for`/`id`
- [ ] Campos obrigatórios com `required` e `aria-required="true"`
- [ ] Erros identificados com `aria-describedby`
- [ ] Autocomplete configurado nos campos relevantes
- [ ] Botão de submit tem texto descritivo (não só "Enviar")

---

### Teste 4.4 — Movimento e animações
**Critérios de aprovação:**
- [ ] `@media (prefers-reduced-motion: reduce)` implementado
- [ ] Nenhuma animação em loop sem opção de pausa
- [ ] Sem flashes rápidos

---

## EVAL-05: Skill de Feedback Loop

### Teste 5.1 — Feedback visual de PM
**Input:**
```
[Após entrega de landing page]
Ficou bom mas está muito colorido. Quero algo mais sério e corporativo.
```

**Critérios de aprovação:**
- [ ] Classifica corretamente como feedback Visual
- [ ] Não altera estrutura de seções não mencionadas
- [ ] Reduz saturação ou migra para paleta mais neutra
- [ ] Comunica mudanças em linguagem de negócio (não técnica)
- [ ] Oferece variação alternativa

---

### Teste 5.2 — Feedback técnico de Dev
**Input:**
```
[Após entrega]
Troca o flexbox do hero para CSS Grid. E aumenta o gap entre os cards para 2rem.
```

**Critérios de aprovação:**
- [ ] Aplica exatamente o que foi pedido (Grid + gap)
- [ ] Não altera outros elementos
- [ ] Documenta as mudanças realizadas
- [ ] Não "traduz" para linguagem simples desnecessariamente

---

### Teste 5.3 — Limite de iterações
**Contexto:** Usuário fez 3 iterações sem aprovar nenhuma versão.

**Critérios de aprovação:**
- [ ] Na 3ª iteração, agente sugere nova abordagem
- [ ] Não continua gerando variações infinitamente sem questionar
- [ ] Tom permanece colaborativo, não defensivo

---

## EVAL-06: Skill de E-commerce Lite

### Teste 6.1 — Catálogo funcional
**Input:**
```
Quero uma loja online para vender cosméticos naturais. Finalização pelo WhatsApp.
```

**Critérios de aprovação:**
- [ ] Array de produtos com estrutura completa (id, nome, preço, categoria, imagem)
- [ ] Grid responsivo de produtos
- [ ] Filtro por categoria funcionando
- [ ] Botão "Adicionar ao carrinho" funcional
- [ ] Contador do carrinho atualizado
- [ ] Preços formatados em pt-BR (R$)

---

### Teste 6.2 — Finalização por WhatsApp
**Critérios de aprovação:**
- [ ] Deep link `wa.me` gerado corretamente
- [ ] Mensagem pré-formatada contém todos os itens do carrinho
- [ ] Mensagem inclui quantidades e valores
- [ ] Total calculado corretamente
- [ ] Link abre WhatsApp em nova aba

---

### Teste 6.3 — UX do carrinho
**Critérios de aprovação:**
- [ ] Feedback visual ao adicionar produto (toast, animação ou mudança no botão)
- [ ] Produto indisponível tratado (botão desabilitado, visual diferenciado)
- [ ] Carrinho persistido em localStorage
- [ ] Possível remover itens do carrinho
- [ ] Carrinho vazio tem estado visual adequado

---

## EVAL-07: Skill de Deploy

### Teste 7.1 — Orientação para PM
**Input:**
```
Como faço para publicar o site que você criou?
```
*(perfil PM/Designer)*

**Critérios de aprovação:**
- [ ] Recomenda Netlify drag-and-drop como primeira opção
- [ ] Instruções em linguagem simples, sem CLI obrigatória
- [ ] Menciona que é gratuito
- [ ] Não expõe comandos técnicos desnecessariamente
- [ ] Oferece guiar passo a passo

---

### Teste 7.2 — Orientação para Dev
**Input:**
```
Quero fazer deploy no Vercel via CLI.
```

**Critérios de aprovação:**
- [ ] Fornece comandos exatos e corretos
- [ ] Menciona `vercel --prod` para produção
- [ ] Cobre autenticação (`vercel login`)
- [ ] Menciona variáveis de ambiente se o projeto as usar
- [ ] Checklist pré-deploy aplicado antes das instruções

---

## Resultado Consolidado

| Skill | Total de critérios | Mínimo para aprovação (80%) |
|---|---|---|
| Intake | 12 | 10 |
| Frontend Design | 15 | 12 |
| SEO | 15 | 12 |
| Acessibilidade | 17 | 14 |
| Feedback Loop | 11 | 9 |
| E-commerce Lite | 15 | 12 |
| Deploy | 10 | 8 |
| **Total** | **95** | **77** |

---

## Registro de Execução

| Data | Versão | Aprovados | Reprovados | Status |
|---|---|---|---|---|
| Mai 2026 | 1.0 | — | — | Aguardando execução |
