---
name: deploy
description: Use este skill quando o usuário solicitar publicação ou deploy do website gerado pelo WebCraft Agent. Cobre empacotamento, Vercel, Netlify e instruções manuais.
---

# Skill: Deploy

Este skill guia o processo de empacotamento e publicação do website gerado.

---

## Objetivo

Garantir que o site gerado seja entregue de forma que o usuário consiga publicá-lo — seja via plataforma automatizada ou manualmente.

---

## Opções de Deploy

### Opção 1: Vercel (recomendado para React/Next.js)
**Quando usar:** Stack React, Next.js ou quando o usuário mencionar Vercel.

**Passos:**
1. Confirme que o projeto tem `package.json` com script de build
2. Instrua o usuário a:
   ```bash
   npm install -g vercel
   vercel login
   vercel --prod
   ```
3. Para projetos Next.js, o Vercel detecta automaticamente

**Para usuário PM:** "Você pode publicar gratuitamente no Vercel. Basta criar uma conta em vercel.com e arrastar a pasta do projeto."

---

### Opção 2: Netlify (recomendado para HTML/CSS/JS)
**Quando usar:** Stack HTML/CSS/JS puro, sem build step.

**Passos para Dev:**
1. Acesse netlify.com
2. Arraste a pasta do projeto para o painel "Sites"
3. O site estará no ar em segundos com URL gerada automaticamente

**Passos via CLI:**
```bash
npm install -g netlify-cli
netlify login
netlify deploy --prod --dir .
```

**Para usuário PM:** "É só acessar netlify.com, criar uma conta grátis e arrastar a pasta do site. Em menos de 1 minuto seu site estará no ar."

---

### Opção 3: GitHub Pages (para projetos versionados)
**Quando usar:** Usuário já usa GitHub ou quer versionamento.

**Passos:**
1. Suba o projeto para um repositório GitHub
2. Vá em Settings → Pages
3. Selecione a branch `main` e pasta `/root`
4. O site estará disponível em `username.github.io/repositório`

---

### Opção 4: Entrega de arquivos (sem deploy)
**Quando usar:** Usuário quer apenas os arquivos para deploy próprio ou entrega a um dev.

**Empacotamento:**
1. Garanta estrutura limpa de arquivos
2. Remova arquivos temporários e comentários de desenvolvimento
3. Minifique CSS e JS se solicitado
4. Entregue via `present_files`

---

## Checklist Pré-Deploy

Antes de qualquer deploy, verifique:

- [ ] Site abre corretamente no browser
- [ ] Responsivo em mobile (375px), tablet (768px) e desktop (1280px)
- [ ] Sem links quebrados ou imagens faltando
- [ ] Meta tags básicas presentes (title, description, og:image)
- [ ] Favicon configurado (ou placeholder)
- [ ] Formulários têm action configurado (ou instrução de configuração)

---

## Meta Tags Mínimas Obrigatórias

Todo site deve ter no `<head>`:
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="[descrição do site]">
<meta property="og:title" content="[título]">
<meta property="og:description" content="[descrição]">
<title>[Título do Site]</title>
```

---

## Comunicação por Perfil

### Para Dev:
- Forneça comandos exatos de CLI
- Explique variáveis de ambiente necessárias
- Documente configurações específicas do projeto

### Para PM:
- Use linguagem simples e visual
- Recomende sempre Netlify drag-and-drop como primeira opção
- Ofereça fazer o deploy junto passo a passo

---

## Domínio Customizado

Se o usuário perguntar sobre domínio próprio:
1. Compre o domínio em registros como Registro.br, GoDaddy ou Namecheap
2. No painel do Vercel/Netlify, vá em "Domain Settings"
3. Adicione o domínio e configure o DNS conforme instruções da plataforma
4. Propagação leva entre 15 minutos e 48 horas
