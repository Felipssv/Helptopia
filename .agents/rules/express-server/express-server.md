---
description: Convenções do servidor Express e padrões de rota no Helptopia.
globs: index.js,routes/**/*.js
alwaysApply: false
---

# Express (`index.js`) — Helptopia

## Estrutura & Inicialização
- Ordem habitual: `express.urlencoded({ extended: true })` → `express.static('public')` → rotas → `listen`.
- **Session:** configurar `express-session` com SECRET antes de registrar rotas de autenticação.
- **Banco:** conecte `pg.Pool` uma vez em `db.js`; use a pool em callbacks/promises nas rotas que acessam BD.

## Padrões de Rota & Banco de Dados
- **Auth routes** (`/auth/login`, `/auth/register`, `/auth/logout`) em `routes/authRoutes.js`.
- **Page routes** (`/`, `/guia`, `/mapa`, `/codigos`) em `routes/pageRoutes.js`.
- **Queries:** usar driver nativo `pg` com statements parametrizados (`$1`, `$2`, etc.) — **nunca** concatenar strings em SQL.
- **Modelo:** funções de BD em `models/usuarioModel.js` retornam Promise ou callback; rota chama função e trata resultado.

## Autenticação & Validação
- **Cadastro (`POST /auth/register`)**:
  - Validar `nome`, `email`, `senha` no servidor (não confiar apenas em HTML).
  - Hash `senha` com `bcrypt.hash(senha, 10)` antes de inserir no BD.
  - Tratar erro de email duplicado (constraint `UNIQUE` na tabela `usuarios`).
  - Redirecionar a `/` com sucesso ou retornar `/register` com erro.
  
- **Login (`POST /auth/login`)**:
  - Buscar usuário por email em PostgreSQL.
  - Comparar senha com `bcrypt.compare(inputSenha, senhaHasheada)`.
  - Usar `req.session.userId = user.id` para manter sessão logada.
  - Redirecionar a `/` ou página inicial; erro: retornar `/login` com mensagem.
  
- **Logout (`GET /auth/logout`)**: `req.session.destroy()` e redirecionar a `/login`.

## Erros & Mensagens
- `console.error` no servidor para falhas inesperadas (erro BD, validação, etc.).
- Respostas ao cliente com mensagens **claras e em português** — nunca vazar stack trace.
- Usar `res.render(..., { error: 'Mensagem', user: req.session.userId })` para passar estado entre requisições.

## Segurança
- Validar entrada de usuário sempre (email é email, código tem formato, etc.).
- Queries parametrizadas são **obrigatórias**; sem exceções.
- Session secret em `.env` (não hardcoded).
- Middleware de autenticação: verificar `req.session.userId` antes de servir rotas protegidas.
