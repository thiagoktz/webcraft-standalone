# PRD — Agente de Desenvolvimento de Websites

## 1. Visão Geral

**Nome do agente:** WebCraft Agent  
**Versão:** 1.0  
**Data:** Maio 2026  
**Owner:** Time de Produto

---

## 2. Problema

Desenvolver websites de qualidade exige conhecimento técnico especializado e tempo considerável. Designers e PMs frequentemente dependem de devs para traduzir suas ideias em código, criando gargalos no processo. Devs, por sua vez, perdem tempo em decisões repetitivas de estrutura e setup.

---

## 3. Objetivo

Criar um agente de IA capaz de desenvolver websites completos a partir de um briefing em linguagem natural, adaptando sua interface e outputs conforme o perfil do usuário (desenvolvedor técnico ou designer/PM não-técnico).

---

## 4. Público-Alvo

| Perfil | Característica | O que espera do agente |
|---|---|---|
| Desenvolvedor | Conhece código, quer velocidade | Código limpo, comentado, estrutura sólida |
| Designer / PM | Não conhece código | Preview visual, linguagem simples, variações |

---

## 5. Casos de Uso Principais

### UC-01: Landing Page a partir de briefing
> Usuário descreve o produto e público. Agente gera landing page completa e responsiva.

### UC-02: Ajuste iterativo por feedback
> Usuário pede mudanças em linguagem natural. Agente interpreta e aplica sem perder contexto.

### UC-03: Entrega diferenciada por perfil
> Dev recebe código comentado + decisões técnicas. PM recebe preview + descrição das seções.

### UC-04: Geração de variações visuais
> Usuário pede "versão mais sóbria" ou "mais colorida". Agente gera alternativas mantendo estrutura.

---

## 6. Escopo

### Dentro do escopo ✅
- Landing pages
- Sites institucionais
- Portfólios
- Dashboards simples
- Sites responsivos (mobile-first)
- Deploy via Vercel/Netlify

### Fora do escopo ❌
- E-commerce com pagamento real
- Sistemas com autenticação complexa
- Backends e bancos de dados
- Apps mobile nativos

---

## 7. Métricas de Sucesso

| Métrica | Meta |
|---|---|
| Tempo até primeira entrega | < 3 minutos |
| Taxa de aprovação sem revisão | > 40% |
| Satisfação por perfil (NPS) | > 70 |
| Iterações até aprovação final | ≤ 3 |

---

## 8. Restrições e Premissas

- O agente assume HTML/CSS/JS como stack padrão, com React quando solicitado
- O agente não pergunta mais de 3 perguntas antes de entregar uma primeira versão
- Toda entrega deve ser responsiva por padrão
- Acessibilidade (WCAG AA) deve ser considerada em toda geração
