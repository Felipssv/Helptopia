---
description: Stack e objetivos do Helptopia (Express, EJS, PostgreSQL). Use em qualquer mudança no repositório.
alwaysApply: true
---

# Helptopia — contexto do projeto

## Stack & Arquitetura
- **Backend:** Node.js, **Express.js**, **EJS** (`views/`), **PostgreSQL** (driver nativo `pg`), **bcrypt** para senhas, **express-session** para autenticação, **dotenv** para configuração.
- **Banco de dados:** PostgreSQL (Supabase); tabelas `usuarios` e `codigos`; variável `DATABASE_URL` via `.env`.
- **Escopo produto:** plataforma interativa de guia para iniciantes do **Heartopia** com autenticação, gerenciamento de dados, códigos promocionais e exibição de mapa/guia.
- **Idioma da interface:** mensagens visíveis ao usuário em **português** (Brasil).

## Dados & Estrutura
- **Tabela `usuarios`:** `id`, `nome`, `email`, `senha` (hasheada com bcrypt), `criado_em`.
- **Tabela `codigos`:** `id`, `codigo`, `descricao`, `status`, `expira`, `criado_em`.
- **Queries:** usar driver nativo `pg` com statements parametrizados (`$1`, `$2`, etc.) para evitar SQL injection (pattern em `models/usuarioModel.js`).
- **Autenticação:** bcrypt para hash de senhas (nunca armazenar plain text); sessions para manter usuário logado.

## Padrões & Boas Práticas
- **Rotas:** organizar em `routes/authRoutes.js` (login/register/logout) e `routes/pageRoutes.js` (páginas).
- **Modelos:** lógica de BD em `models/` (ex: `usuarioModel.js`) com funções reutilizáveis.
- **Templates:** EJS em `views/` com partials reutilizáveis em `views/partials/`.
- **Estático:** CSS vanilla em `public/css/` (sem frameworks CSS desnecessários); imagens em `public/img/`.
- **Código:** comentado e legível para nível iniciante-intermediário; explicar conceitos de arquitetura cliente-servidor, HTTP, banco de dados.

## Restrições & Preferences
- **Não introduzir** ORM (Sequelize, TypeORM), ORMs complexas ou frameworks frontend **salvo pedido explícito** — manter driver nativo `pg`.
- **Sem novos pacotes pesados** sem justificativa: validação/parsing deve ser simples e compreensível.
- **Error handling:** sempre tratar erros de BD com try/catch e retornar mensagens apropriadas ao usuário.
- **Segurança:** queries parametrizadas, bcrypt obrigatório, CSRF protection em formulários POST.
