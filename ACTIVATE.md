# ACTIVATE.md — Como Ativar o WebCraft Agent

Copie e cole o bloco abaixo no início de uma conversa com o Claude para ativar o agente completo.

---

## ⚡ Ativação Rápida

```
Você é o WebCraft Agent. Leia e siga as instruções em:
https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/system-prompt.md

Skills disponíveis — consulte conforme necessário:
- Intake:         https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/intake/SKILL.md
- Feedback Loop:  https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/feedback-loop/SKILL.md
- Deploy:         https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/deploy/SKILL.md
- SEO:            https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/seo/SKILL.md
- Acessibilidade: https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/acessibilidade/SKILL.md
- E-commerce:     https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/ecommerce-lite/SKILL.md
- Multilingual:   https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/multilingual/SKILL.md
- Performance:    https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/performance/SKILL.md
- Analytics:      https://raw.githubusercontent.com/thiagoktz/webcraft-agent/main/skills/analytics/SKILL.md
```

> ⚠️ **Substitua `thiagoktz` pelo seu username do GitHub antes de usar.**

---

## 📋 Mapa de Skills — Quando o agente consulta cada um

| Situação | Skill ativado |
|---|---|
| Qualquer geração de UI | `frontend-design` (público) |
| Primeiro briefing do usuário | `intake` |
| Feedback após entrega | `feedback-loop` |
| "Como publico o site?" | `deploy` |
| "Quero aparecer no Google" | `seo` |
| Site para qualquer pessoa | `acessibilidade` |
| "Quero vender produtos" | `ecommerce-lite` |
| "Site em português e inglês" | `multilingual` |
| "Site rápido / Lighthouse" | `performance` |
| "Quero medir os acessos" | `analytics` |

---

## 🧪 Rodar EVALS manualmente

Para testar a qualidade dos skills após mudanças:

```bash
# Clonar o repositório
git clone https://github.com/thiagoktz/webcraft-agent.git
cd webcraft-agent

# Instalar dependências
npm install @anthropic-ai/sdk

# Configurar API key
export ANTHROPIC_API_KEY=sua-chave-aqui

# Executar EVALS
node .github/workflows/run-evals.mjs
```

O relatório será salvo em `eval-report.json`.

---

## 🔄 CI/CD — EVALS automáticos no GitHub

O arquivo `.github/workflows/evals.yml` roda automaticamente quando:
- Um Pull Request altera qualquer arquivo em `skills/`
- Um Push chega à branch `main` com mudanças em skills

**Para configurar:**
1. No GitHub, vá em **Settings → Secrets and variables → Actions**
2. Clique em **New repository secret**
3. Nome: `ANTHROPIC_API_KEY`
4. Valor: sua chave da API Anthropic

A partir daí, todo PR que altere skills receberá um comentário automático com o resultado dos EVALS.

---

## 📁 Estrutura completa do repositório

```
webcraft-agent/
  ├── README.md                         # Visão geral
  ├── ACTIVATE.md                       # Este arquivo
  ├── PRD.md                            # O quê e por quê
  ├── SDD.md                            # Como é construído
  ├── system-prompt.md                  # Identidade do agente
  ├── EVALS.md                          # Critérios de avaliação
  ├── .github/
  │   └── workflows/
  │       └── evals.yml                 # CI/CD automático
  └── skills/
        ├── intake/SKILL.md
        ├── feedback-loop/SKILL.md
        ├── deploy/SKILL.md
        ├── seo/SKILL.md
        ├── acessibilidade/SKILL.md
        ├── ecommerce-lite/SKILL.md
        ├── multilingual/SKILL.md
        ├── performance/SKILL.md
        └── analytics/SKILL.md
```

---

## 🚀 Primeiros passos após ativar

1. Cole o bloco de ativação no Claude
2. Descreva o site que quer criar em linguagem natural
3. O agente vai detectar seu perfil (dev ou PM) e agir automaticamente
4. Itere com feedback em linguagem natural

**Exemplo de primeiro prompt após ativação:**
```
Quero uma landing page para meu serviço de consultoria financeira.
Público: MEIs e pequenas empresas. Tom: sério mas acessível.
```
