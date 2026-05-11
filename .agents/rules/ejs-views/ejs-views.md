---
description: Padrões das views EJS e partials do Helptopia.
globs: views/**/*.ejs
alwaysApply: false
---

# Views EJS — Helptopia

## Partials & Reutilização
- Reutilizar **partials**: `views/partials/navbar.ejs`, `header.ejs`, `footer.ejs` via `<%- include(...) %>`.
- Manter estrutura consistente: header com navegação, footer com links, navbar responsiva.
- Passar ao `res.render(...)` apenas as **locals** necessárias (ex.: `user`, `guias`, `mapa`, `codigos`, `error`, `mensagem`) — evitar globais ou contexto desnecessário.

## Segurança & Escaping
- Conteúdo dinâmico do usuário ou banco: usar escaping padrão do EJS (`<%= %>`) — nunca `<%-` a menos que seja HTML confiável do sistema.
- Exibir nome do usuário logado, descrições de código, etc.: sempre com `<%= %>` para evitar XSS.

## Formulários
- **POST routes**: `method="POST"` e `action="/auth/login"`, `/auth/register"`, `/codigos"` etc.
- Nomes de campos alinhados a `req.body` no servidor (ex: `nome`, `email`, `senha`, `codigo`).
- Mensagens de erro: exibir `<%= error %>` quando houver validação no servidor.
- Icons/imagens: usar `<img alt="...">` sempre com texto alternativo descritivo (acessibilidade).

## HTML Semântico & Styling
- Usar `<main>`, `<nav>`, `<header>`, `<footer>` onde couber (não só `<div>`).
- Classes CSS alinhadas a `public/css/style.css` — não criar CSS novo sem necessidade.
- Tabelas para dados tabelados (ex: lista de códigos); `<form>` para qualquer entrada de usuário.
- Hierarquia de headings: uma única `<h1>` por página, depois `<h2>`, `<h3>` em ordem lógica.

## Dados & Contexto
- Exibir resultados da BD (usuários, códigos, mapa, guia) em estruturas simples: listas, cards ou tabelas.
- Uso de flags booleanas: `<% if (user) %>` para exibir menu autenticado; `<% if (message) %>` para sucesso/erro.
- Pages estáticas: `home.ejs`, `guia.ejs`, `mapa.ejs`; pages dinâmicas: `login.ejs`, `register.ejs`, `codigos.ejs`.
