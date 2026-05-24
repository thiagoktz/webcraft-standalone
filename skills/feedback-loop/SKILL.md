---
name: feedback-loop
description: Use este skill ao processar qualquer feedback do usuário após uma entrega do WebCraft Agent. Garante interpretação correta do pedido de revisão e manutenção do contexto entre iterações.
---

# Skill: Feedback Loop

Este skill guia o processamento de revisões e iterações após a primeira entrega do site.

---

## Objetivo

Interpretar o feedback do usuário — técnico ou em linguagem de negócio — e aplicar as mudanças corretas sem perder o contexto do que já foi construído.

---

## Classificação do Feedback

Antes de agir, classifique o feedback em uma destas categorias:

### 🎨 Visual
Afeta aparência, cores, tipografia, espaçamento, layout.
- "Quero mais escuro"
- "A fonte está muito grande"
- "Prefiro algo mais minimalista"
- "As cores não combinam com minha marca"

### 📝 Conteúdo
Afeta textos, imagens, seções, ordem dos elementos.
- "Troca o título do hero"
- "Remove a seção de depoimentos"
- "Adiciona uma seção de preços"
- "O CTA deveria ficar antes do footer"

### ⚙️ Funcional
Afeta comportamento, interações, JavaScript, formulários.
- "O menu não fecha no mobile"
- "Quero que o botão role para a próxima seção"
- "Adiciona um formulário de contato funcional"

### 🏗️ Estrutural
Afeta arquitetura do código, stack, organização de arquivos.
- "Converte para React"
- "Separa o CSS em componentes"
- "Adiciona uma segunda página"

---

## Regras de Interpretação

### Para usuários PM/Designer:
- Traduza linguagem de negócio em ações técnicas **silenciosamente**
- "Parece muito frio" → ajuste paleta para tons mais quentes
- "Quero que pareça mais premium" → refinamento tipográfico, mais espaço negativo, paleta monocromática
- "Não parece meu brand" → pergunte sobre cores e valores da marca

### Para usuários Dev:
- Aceite instruções técnicas diretamente
- Confirme o entendimento antes de aplicar mudanças grandes
- Documente as mudanças realizadas

---

## Preservação de Contexto

A cada iteração, preserve:
- Tipo e propósito do site
- Perfil do usuário
- Decisões de design já aprovadas
- Seções que não foram mencionadas no feedback (não altere o que não foi pedido)

**Regra de ouro:** Se o usuário não mencionou uma seção no feedback, ela não deve mudar.

---

## Limite de Iterações

- Até **5 iterações** por sessão sem necessidade de reiniciar
- Na 3ª iteração sem aprovação, pergunte: "Posso sugerir uma abordagem diferente para o que você está buscando?"
- Na 5ª iteração, proponha recomeçar com um brief mais detalhado

---

## Comunicação por Perfil

### PM/Designer:
- Descreva o que mudou em linguagem simples
- "Escureci o fundo e usei uma tipografia mais refinada para passar mais sofisticação"
- Ofereça alternativas: "Posso também tentar uma versão com tons de azul — prefere ver?"

### Dev:
- Liste as mudanças técnicas realizadas
- "Alterei `--color-bg` de `#fff` para `#0f0f0f`, ajustei `font-size` do h1 para `clamp(2rem, 5vw, 4rem)` e adicionei `letter-spacing: -0.02em`"

---

## Exemplo de Aplicação

**Feedback do usuário (PM):** "Gostei da estrutura mas ficou muito colorido. Quero algo mais sério."

**Classificação:** Visual

**Ações:**
1. Reduzir saturação da paleta
2. Migrar para esquema mais monocromático ou neutro
3. Revisar uso de gradientes (simplificar ou remover)
4. Manter estrutura de seções intacta

**Comunicação:** "Refinei a paleta para tons mais neutros e removi os gradientes — o resultado agora tem um visual mais sóbrio e profissional. O que acha?"
