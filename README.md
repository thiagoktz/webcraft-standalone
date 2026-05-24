# WebCraft Agent — Documentação Completa

Agente de desenvolvimento de websites com skills especializados, documentação estruturada e CI/CD automático via GitHub Actions.

---

## Estrutura

```
webcraft-agent/
  ├── README.md
  ├── ACTIVATE.md                       # Como ativar o agente no Claude
  ├── PRD.md                            # O quê e por quê
  ├── SDD.md                            # Como é construído
  ├── system-prompt.md                  # Identidade e comportamento do agente
  ├── EVALS.md                          # Critérios de avaliação (95 critérios)
  ├── .github/
  │   └── workflows/
  │       └── evals.yml                 # CI/CD: roda EVALS a cada PR
  └── skills/
        ├── intake/SKILL.md             # Interpretação de briefings
        ├── feedback-loop/SKILL.md      # Processamento de revisões
        ├── deploy/SKILL.md             # Publicação do site
        ├── seo/SKILL.md                # Meta tags e estrutura SEO
        ├── acessibilidade/SKILL.md     # Checklist WCAG 2.1 AA
        ├── ecommerce-lite/SKILL.md     # Catálogos sem pagamento real
        ├── multilingual/SKILL.md       # Sites em múltiplos idiomas
        ├── performance/SKILL.md        # Core Web Vitals avançado
        └── analytics/SKILL.md          # GA4 e eventos de conversão
```

> O skill `frontend-design` é público e reutilizado do repositório central de skills.

---

## Ativação rápida

Veja o arquivo `ACTIVATE.md` para o bloco pronto para colar no Claude.

---

## Mapa de Skills

| Situação | Skill |
|---|---|
| Toda geração de UI | `frontend-design` (público) |
| Primeiro briefing | `intake` |
| Revisão após entrega | `feedback-loop` |
| Publicação do site | `deploy` |
| Otimização para Google | `seo` |
| Conformidade de acessibilidade | `acessibilidade` |
| Catálogo de produtos | `ecommerce-lite` |
| Site em múltiplos idiomas | `multilingual` |
| Performance / Lighthouse | `performance` |
| Rastreamento de métricas | `analytics` |

---

## CI/CD — EVALS automáticos

Todo PR que alterar arquivos em `skills/` dispara automaticamente:

1. **Validação estrutural** — verifica arquivos obrigatórios e frontmatter
2. **EVALS via Claude API** — executa casos de teste reais e avalia critérios
3. **Comentário no PR** — posta resultado com tabela de aprovação/reprovação

**Configuração necessária:**
- Adicionar `ANTHROPIC_API_KEY` em **Settings → Secrets → Actions** do repositório

---

## Como evoluir o agente

```
Edita um SKILL.md
      ↓
Abre Pull Request
      ↓
CI roda EVALS automaticamente
      ↓
Resultado comentado no PR
      ↓
Aprovado? → Merge para main
      ↓
Claude usa versão atualizada na próxima conversa
```

---

## Versão e Histórico

| Versão | Data | Mudança |
|---|---|---|
| 1.0 | Mai 2026 | Versão inicial (intake, feedback-loop, deploy) |
| 1.1 | Mai 2026 | Skills: seo, acessibilidade, ecommerce-lite + EVALS |
| 1.2 | Mai 2026 | Skills: multilingual, performance, analytics + CI/CD + ACTIVATE.md |
