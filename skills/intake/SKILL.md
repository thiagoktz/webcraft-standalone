---
name: intake
description: Use este skill ao interpretar o briefing inicial do usuário no WebCraft Agent. Garante extração consistente de informações e detecção correta do perfil do usuário antes de qualquer geração de código.
---

# Skill: Intake de Briefing

Este skill guia a interpretação do briefing inicial do usuário e a geração do brief técnico interno antes de qualquer execução.

---

## Objetivo

Transformar linguagem natural (às vezes vaga) em um brief estruturado que permita ao agente agir com precisão e sem perguntas desnecessárias.

---

## Informações a Extrair

### Obrigatórias (sem estas, pergunte):
- **Tipo de site:** landing page, portfólio, institucional, dashboard...
- **Propósito:** o que o site deve comunicar ou fazer?
- **Público-alvo:** quem vai acessar?

### Desejáveis (assuma padrão se ausente):
- **Tom visual:** moderno, sóbrio, colorido, minimalista... (padrão: moderno e limpo)
- **Stack:** HTML/CSS/JS, React, Next.js... (padrão: HTML/CSS/JS)
- **Seções:** hero, features, depoimentos, CTA, footer... (padrão: as mais comuns para o tipo)

### Inferíveis (nunca pergunte):
- **Perfil do usuário:** detecte por sinais linguísticos (veja abaixo)
- **Paleta de cores:** derive do tom e do segmento de negócio

---

## Detecção de Perfil do Usuário

### Sinais de Desenvolvedor:
- Menciona stack tecnológica ("quero em React", "usa CSS Grid")
- Usa jargão técnico ("componente", "API", "deploy", "responsivo")
- Pede estrutura de arquivos ou comentários no código
- Faz perguntas sobre implementação

### Sinais de Designer / PM:
- Descreve o negócio, não a tecnologia
- Usa linguagem de resultado ("quero que pareça...", "preciso que o cliente...")
- Não menciona stack ou menciona ferramentas de design (Figma, etc.)
- Foca em visual e conteúdo, não em como é feito

### Quando incerto:
- Assuma PM/Designer como padrão (mais seguro)
- Ajuste se o usuário usar linguagem técnica nas próximas mensagens

---

## Regras de Clarificação

- Faça **no máximo 3 perguntas** por vez
- Priorize as informações **obrigatórias**
- Se tiver dúvida entre 2 caminhos, escolha um e informe ao usuário ("vou assumir X — me avise se preferir diferente")
- **Nunca bloqueie a execução** esperando respostas — entregue uma v1 e itere

---

## Output: Brief Técnico Interno

Ao final do intake, gere mentalmente este JSON antes de agir:

```json
{
  "tipo": "landing page",
  "produto": "descrição do produto/serviço",
  "publico": "quem vai acessar",
  "tom": "moderno, profissional",
  "stack": "HTML/CSS/JS",
  "perfil_usuario": "PM",
  "secoes": ["hero", "features", "depoimentos", "CTA", "footer"],
  "restricoes": [],
  "duvidas_pendentes": []
}
```

---

## Exemplo de Aplicação

**Usuário:** "Quero um site para minha clínica de fisioterapia. Algo que passe confiança e seja fácil de atualizar."

**Extração:**
- Tipo: site institucional
- Produto: clínica de fisioterapia
- Público: pacientes em busca de tratamento
- Tom: confiável, acolhedor, profissional
- Stack: HTML/CSS/JS (padrão — usuário não mencionou)
- Perfil: PM/Designer (fala em resultado, não em tecnologia)
- Seções assumidas: hero, serviços, equipe, depoimentos, contato, footer
- Dúvidas pendentes: nenhuma crítica — pode executar
