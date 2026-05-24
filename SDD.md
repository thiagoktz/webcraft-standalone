# SDD — Software Design Document
## WebCraft Agent

**Versão:** 1.0  
**Data:** Maio 2026  
**Status:** Draft

---

## 1. Visão Técnica

O WebCraft Agent é um agente de IA baseado em Claude que opera em um loop de execução estruturado em 4 camadas: Intake → Skills → Execução → Entrega. Cada camada tem responsabilidades bem definidas e ferramentas específicas.

---

## 2. Arquitetura em Camadas

```
┌─────────────────────────────────────────────┐
│              USUÁRIO (Dev ou PM)            │
└───────────────────┬─────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│           CAMADA 1 — INTAKE                 │
│  Interpreta o briefing                      │
│  Detecta perfil do usuário                  │
│  Gera brief técnico interno                 │
└───────────────────┬─────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│           CAMADA 2 — SKILLS                 │
│  Consulta skills relevantes                 │
│  frontend-design / deploy / intake          │
└───────────────────┬─────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│           CAMADA 3 — EXECUÇÃO               │
│  Gera estrutura → Design → Funcionalidades  │
│  Valida responsividade e acessibilidade     │
└───────────────────┬─────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│           CAMADA 4 — ENTREGA                │
│  Bifurca output por perfil                  │
│  Dev: código + docs técnicas                │
│  PM: preview + descrição em linguagem simples│
└─────────────────────────────────────────────┘
```

---

## 3. Detalhamento das Camadas

### 3.1 Camada de Intake

**Responsabilidade:** Transformar linguagem natural em brief técnico estruturado.

**Inputs:**
- Texto livre do usuário
- Histórico da conversa (se houver)

**Processo:**
1. Extrair tipo de site, público-alvo e tom visual
2. Detectar perfil do usuário (dev ou PM) por sinais linguísticos
3. Identificar stack preferida ou assumir padrão (HTML/CSS/JS)
4. Gerar JSON interno de brief

**Output (brief interno):**
```json
{
  "tipo": "landing page",
  "produto": "SaaS de gestão financeira",
  "publico": "PMEs",
  "tom": "profissional, moderno",
  "stack": "HTML/CSS/JS",
  "perfil_usuario": "PM",
  "funcionalidades": ["hero", "features", "CTA", "footer"]
}
```

**Sinais para detectar perfil:**
- Dev: menciona stack, pede código, usa termos técnicos
- PM: fala em "quero um site que...", descreve negócio, não menciona tecnologia

---

### 3.2 Camada de Skills

**Responsabilidade:** Garantir qualidade e consistência na execução.

| Situação | Skill consultado |
|---|---|
| Toda geração de UI | `frontend-design/SKILL.md` |
| Deploy solicitado | `deploy/SKILL.md` |
| Primeiro contato com usuário | `intake/SKILL.md` |
| Feedback recebido | `feedback-loop/SKILL.md` |

---

### 3.3 Camada de Execução

**Responsabilidade:** Gerar o website em etapas incrementais.

**Sequência:**
1. Estrutura semântica (HTML)
2. Estilização e identidade visual (CSS)
3. Responsividade (media queries / mobile-first)
4. Interações e animações (JS / CSS animations)
5. Acessibilidade (alt texts, aria-labels, contraste)
6. Validação final

**Ferramentas utilizadas:**
- `create_file` → gerar arquivos do site
- `bash` → validar build, rodar linters
- `web_search` → buscar referências e bibliotecas atuais
- `present_files` → entregar output ao usuário

**Stack padrão:**
```
HTML5 semântico
CSS3 com variáveis customizadas
JavaScript vanilla (ES6+)
Google Fonts para tipografia
```

**Stack alternativa (quando solicitado):**
```
React + Tailwind CSS
Next.js para sites com múltiplas páginas
```

---

### 3.4 Camada de Entrega

**Responsabilidade:** Bifurcar o output conforme o perfil do usuário.

**Para Desenvolvedores:**
- Código completo com comentários explicativos
- Decisões técnicas documentadas inline
- Instruções de deploy
- Sugestões de melhorias futuras

**Para Designers / PMs:**
- Preview visual descrito em linguagem simples
- Explicação de cada seção ("o hero apresenta sua proposta de valor...")
- Opções de variação ("posso deixar mais escuro ou mais colorido")
- Sem exposição desnecessária de código

---

## 4. Loop de Feedback

```
Entrega v1
    ↓
Usuário avalia
    ↓
Agente classifica feedback:
  - Visual → ajusta CSS/design
  - Conteúdo → ajusta copy/estrutura
  - Funcional → ajusta JS/comportamento
    ↓
Gera nova versão (mantém contexto)
    ↓
(repete até aprovação ou limite de 5 iterações)
```

---

## 5. Tratamento de Erros

| Erro | Comportamento |
|---|---|
| Brief muito vago | Faz até 3 perguntas de clarificação |
| Stack não suportada | Informa limitação e sugere alternativa |
| Site muito complexo | Divide em entregas incrementais |
| Falha no build | Tenta corrigir automaticamente 1x, então reporta |

---

## 6. Estrutura de Arquivos de Output

```
/output
  ├── index.html
  ├── styles.css
  ├── script.js
  └── /assets
        └── (imagens, ícones, fontes locais)
```

---

## 7. Limitações Conhecidas

- Não gera imagens reais (usa placeholders descritivos)
- Deploy automático requer credenciais do Vercel/Netlify configuradas
- Sites com mais de 5 páginas devem ser divididos em sessões separadas
