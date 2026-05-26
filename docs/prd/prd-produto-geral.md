# PRD — Helptopia: Produto Geral (MVP)

**Versão:** 1.0  
**Data:** 2026-05-26  
**Status:** Documentação do estado atual  
**Autor:** Felipe  

---

## 1. Introdução / Visão Geral

**Helptopia** é um guia web para iniciantes do jogo mobile *Heartopia* — um simulador de vida aconchegante focado em criatividade, personalização e progresso pacífico. O produto oferece recursos de consulta (itens lucrativos, mapa interativo, códigos promocionais) e autenticação de usuários, centralizando em um único lugar as informações mais úteis para novos jogadores.

**Stack:** Node.js + Express 5, EJS, PostgreSQL (Supabase), `pg` nativo, `bcrypt`, `express-session`. CSS vanilla. Sem frameworks front-end.

**URL de acesso local:** `http://localhost:3000`

---

## 2. Objetivos

- Fornecer uma referência rápida sobre itens rentáveis do Heartopia, eliminando a necessidade de busca em wikis externas fragmentadas.
- Disponibilizar o mapa interativo oficial incorporado na plataforma para localização de recursos.
- Centralizar códigos promocionais ativos com indicação de validade e mecanismo de cópia.
- Oferecer autenticação segura (cadastro + login) como base para funcionalidades futuras personalizadas.
- Garantir acessibilidade WCAG 2.1 AA em todas as páginas públicas.

---

## 3. User Stories

### US-001: Consultar itens mais lucrativos

**Descrição:** Como um novo jogador de Heartopia, quero ver uma tabela com os itens artesanais mais rentáveis para que eu saiba onde focar meu tempo de jogo.

**Critérios de Aceitação:**
- [ ] A tabela exibe: nome do item, ingredientes, preço de venda (em ⭐), lucro e nível mínimo requerido.
- [ ] Itens são categorizados visualmente por faixa de lucro (`profit-top`, `profit-high`, `profit-mid`).
- [ ] A tabela é legível em telas de 320px (mobile) sem rolagem horizontal oculta.
- [ ] A página `/guia` é acessível sem login.
- [ ] Verificar no navegador: abrir `/guia`, confirmar que a tabela carrega sem erros de console e que badges de lucro têm cores distintas.

---

### US-002: Visualizar mapa interativo de recursos

**Descrição:** Como um novo jogador, quero acessar um mapa interativo de Heartopia para que eu localize recursos e ingredientes sem sair do site.

**Critérios de Aceitação:**
- [ ] A página `/mapa` exibe o mapa via `<iframe>` com `loading="lazy"` e `title` descritivo.
- [ ] O iframe ocupa a área principal da página sem transbordar o layout.
- [ ] A página é acessível sem login.
- [ ] Verificar no navegador: abrir `/mapa` e confirmar que o iframe carrega corretamente.

---

### US-003: Ver códigos promocionais ativos

**Descrição:** Como jogador, quero ver os códigos promocionais de Heartopia com status e recompensa para que eu possa resgatá-los sem pesquisar em outros lugares.

**Critérios de Aceitação:**
- [ ] A página `/codigos` lista todos os registros da tabela `codigos` ordenados por `status ASC, expira ASC`.
- [ ] Cada card exibe: código, recompensa, ícone, status (`ATIVO` / `EXPIRADO`) e data de expiração quando disponível.
- [ ] Botão "Copiar" copia o código para o clipboard e muda temporariamente o texto para "Copiado!" por 2 segundos.
- [ ] Botão "✓" marca o código como resgatado via `localStorage`; o estado persiste entre recarregamentos da página.
- [ ] Códigos expirados são visualmente distintos (badge diferente) dos ativos.
- [ ] Quando não há códigos no banco, exibe mensagem: "Nenhum código disponível no momento. Volte em breve!".
- [ ] A página é acessível sem login.
- [ ] Verificar no navegador: clicar em "Copiar" e confirmar feedback visual; marcar como resgatado e recarregar a página.

---

### US-004: Criar conta

**Descrição:** Como visitante, quero me cadastrar com nome de usuário, e-mail e senha para que eu possa ter acesso a funcionalidades futuras personalizadas.

**Critérios de Aceitação:**
- [ ] Formulário em `/register` exige: `username`, `email`, `password`, `confirmPassword`.
- [ ] Todos os campos são obrigatórios; omitir qualquer um exibe "Preencha todos os campos".
- [ ] Senhas divergentes exibem "Senhas não coincidem!".
- [ ] Nome de usuário já existente exibe "Este usuário já existe".
- [ ] E-mail já cadastrado exibe "Este email já está cadastrado".
- [ ] Senha é armazenada como hash bcrypt (fator 10); nunca em texto puro.
- [ ] Cadastro bem-sucedido redireciona para `/login`.
- [ ] Verificar no navegador: tentar cadastrar com e-mail duplicado e confirmar mensagem de erro inline.

---

### US-005: Fazer login e logout

**Descrição:** Como usuário cadastrado, quero fazer login com meu nome de usuário para que a aplicação me reconheça em páginas que requerem autenticação.

**Critérios de Aceitação:**
- [ ] Formulário em `/login` aceita `username` e `password`.
- [ ] Credenciais inválidas exibem "Usuário ou senha incorretos" sem revelar qual campo está errado.
- [ ] Login bem-sucedido cria `req.session.usuario = { id, nome, email }` e redireciona para `/`.
- [ ] `GET /logout` destrói a sessão e redireciona para `/`.
- [ ] Após logout, `res.locals.usuario` é `null` em todas as templates.
- [ ] Verificar no navegador: logar, confirmar que o navbar reflete o estado autenticado; deslogar e confirmar retorno ao estado anônimo.

---

### US-006: Navegar entre páginas com contexto visual

**Descrição:** Como usuário, quero uma barra de navegação que destaque a página atual para que eu saiba onde estou no site.

**Critérios de Aceitação:**
- [ ] O navbar está presente em todas as páginas via `partials/navbar.ejs`.
- [ ] O link da página atual recebe destaque visual (via variável `pagina` passada na rota).
- [ ] Quando autenticado, o navbar exibe o nome do usuário ou link de logout.
- [ ] Verificar no navegador: navegar entre `/`, `/guia`, `/mapa` e `/codigos` confirmando o destaque correto.

---

## 4. Requisitos Funcionais

| ID | Requisito |
|----|-----------|
| RF-1 | O servidor Express deve iniciar na porta 3000 (`index.js`). |
| RF-2 | `res.locals.usuario` deve ser injetado em toda requisição via middleware antes das rotas. |
| RF-3 | Todas as queries ao PostgreSQL devem usar parâmetros posicionais (`$1`, `$2`, …). |
| RF-4 | Senhas devem ser hashadas com `bcrypt.hash(senha, 10)` antes do INSERT. |
| RF-5 | Login deve comparar com `bcrypt.compare`; nunca comparar strings diretas. |
| RF-6 | O segredo da sessão deve vir de `process.env.SESSION_SECRET` (não hardcoded). |
| RF-7 | Stack traces nunca devem ser enviados ao cliente; erros internos devem ser logados com `console.error`. |
| RF-8 | Templates devem usar `<%= %>` para todo conteúdo de banco/usuário; `<%-` somente para partials internas. |
| RF-9 | Arquivos estáticos (CSS, imagens, JS) são servidos de `/public` via `express.static`. |
| RF-10 | O motor de templates EJS deve ser configurado como `view engine` no Express. |
| RF-11 | A rota `/codigos` ordena resultados por `status ASC, expira ASC`. |

---

## 5. Impacto no Banco de Dados

### Tabelas existentes

#### `usuarios`

```sql
CREATE TABLE usuarios (
  id    SERIAL PRIMARY KEY,
  nome  VARCHAR UNIQUE NOT NULL,
  email VARCHAR UNIQUE NOT NULL,
  senha VARCHAR        NOT NULL  -- bcrypt hash
);
```

#### `codigos`

```sql
CREATE TABLE codigos (
  id        SERIAL PRIMARY KEY,
  codigo    VARCHAR     NOT NULL,
  recompensa TEXT       NOT NULL,
  status    VARCHAR     NOT NULL,  -- 'Ativo' | 'Expirado'
  expira    VARCHAR,               -- data de expiração (texto livre)
  icone     VARCHAR                -- emoji do card
);
```

### Queries em uso

```sql
-- Buscar usuário por nome (login)
SELECT * FROM usuarios WHERE nome = $1;

-- Buscar usuário por email (checagem de duplicata)
SELECT * FROM usuarios WHERE email = $1;

-- Criar usuário
INSERT INTO usuarios (nome, email, senha) VALUES ($1, $2, $3) RETURNING id, nome, email;

-- Listar códigos
SELECT * FROM codigos ORDER BY status ASC, expira ASC;
```

---

## 6. Rotas & Endpoints da API

| Método | Rota        | Auth | Descrição                                      | Arquivo              |
|--------|-------------|------|------------------------------------------------|----------------------|
| GET    | `/`         | Não  | Renderiza home                                 | `pageRoutes.js`      |
| GET    | `/guia`     | Não  | Renderiza guia de itens lucrativos             | `pageRoutes.js`      |
| GET    | `/mapa`     | Não  | Renderiza página com mapa interativo (iframe)  | `pageRoutes.js`      |
| GET    | `/codigos`  | Não  | Lista códigos do banco e renderiza a página    | `pageRoutes.js`      |
| GET    | `/login`    | Não  | Renderiza formulário de login                  | `pageRoutes.js`      |
| GET    | `/register` | Não  | Renderiza formulário de cadastro               | `pageRoutes.js`      |
| POST   | `/login`    | Não  | Autentica usuário, cria sessão                 | `authRoutes.js`      |
| POST   | `/register` | Não  | Cria novo usuário com senha hashada            | `authRoutes.js`      |
| GET    | `/logout`   | Sim  | Destrói sessão e redireciona para `/`          | `authRoutes.js`      |

**Formato de resposta:** todas as rotas respondem com `res.render()` (SSR). Não há endpoints JSON nesta versão MVP.

**Sessão:** `req.session.usuario = { id: Number, nome: String, email: String }`

---

## 7. Não-Objetivos

Esta versão MVP **não inclui**:

- Painel de administração para gerenciar códigos ou usuários.
- API REST / JSON para consumo externo.
- Perfil de usuário editável (foto, bio, preferências).
- Sistema de favoritos, anotações ou progresso personalizado.
- Notificações (e-mail, push) sobre novos códigos.
- Testes automatizados (unitários ou de integração).
- Internacionalização (i18n) — o produto é exclusivamente em pt-BR.
- Autenticação via OAuth / redes sociais.
- Rate limiting, CAPTCHA ou proteção contra força bruta no login.

---

## 8. Considerações de Design (UI/UX & WCAG 2.1 AA)

### Convenções visuais atuais

- Layout em cards (`class="card"`) como unidade visual primária.
- Grid de features na home (`features-grid`).
- Tabela responsiva com badges de lucro coloridos no guia.
- Cards de código com badge de status (ativo/expirado) e botão de cópia.
- CSS vanilla em `public/css/style.css`; sem frameworks.

### Checklist de acessibilidade obrigatório (toda mudança em `views/` ou `public/`)

1. Todo `<img>` tem `alt` descritivo (ou `alt=""` se decorativo)?
2. Todos os links e botões têm foco visível (`:focus` / `:focus-visible`)?
3. Cada `<input>` tem `<label for="id">` correspondente?
4. Uma única `<h1>` por página; hierarquia sem saltos (`h1→h2→h3`)?
5. Contraste de texto ≥ 4.5:1 sobre o fundo?
6. Indicadores de erro/campo obrigatório usam texto — nunca apenas cor?
7. Tags semânticas presentes: `<main>`, `<nav>`, `<header>`, `<footer>`?
8. Layout funciona a 200% de zoom?

### Dívidas de acessibilidade conhecidas

- O mapa via `<iframe>` externo pode não ser totalmente navegável por teclado (dependência de terceiro).
- Botões de cópia e "resgatado" em `codigos.ejs` não têm `aria-label` descritivo.

---

## 9. Considerações Técnicas

### Dependências principais

| Pacote            | Versão   | Uso                                 |
|-------------------|----------|-------------------------------------|
| `express`         | ^5.2.1   | Framework HTTP                      |
| `ejs`             | ^5.0.1   | Motor de templates SSR              |
| `pg`              | ^8.20.0  | Driver PostgreSQL nativo            |
| `bcrypt`          | ^6.0.0   | Hash de senhas                      |
| `express-session` | ^1.19.0  | Gerenciamento de sessão             |
| `dotenv`          | ^17.4.2  | Carregamento de variáveis de ambiente |
| `nodemon`         | ^3.1.14  | Auto-reload em desenvolvimento      |

### Variáveis de ambiente (`.env`)

```
DATABASE_URL=postgresql://...   # Connection string Supabase / PostgreSQL local
SESSION_SECRET=...              # String aleatória; jamais hardcoded
```

### Dívida técnica conhecida

| Item | Localização | Impacto |
|------|-------------|---------|
| `SESSION_SECRET` hardcoded como `"segredo"` | `index.js:11` | **Segurança crítica** — deve ser movido para `process.env.SESSION_SECRET` |
| `saveUninitialized: false` sem `cookie.secure` | `index.js:13` | Sessão transmitida em HTTP plano em produção |
| Query em `pageRoutes.js` sem model layer | `pageRoutes.js:27` | Aceitável para MVP; extrair para `models/codigoModel.js` quando queries crescerem |

### Convenções a seguir

- Nova lógica de banco de dados de usuários → `models/usuarioModel.js`.
- Novas entidades → novos arquivos de model (`models/[entidade]Model.js`).
- Queries simples de `codigos` podem ficar em `pageRoutes.js`; extrair ao crescerem.
- Rotas de mutação (POST/PUT/DELETE) → `authRoutes.js` ou novo arquivo de rotas.
- Rotas de visualização (GET de páginas) → `pageRoutes.js`.

---

## 10. Métricas de Sucesso

| Métrica | Meta MVP |
|---------|----------|
| Cadastro completo sem erros de formulário | 100% dos campos validados inline |
| Login bem-sucedido cria sessão | Redireciona para `/` em < 1s |
| Página `/codigos` carrega dados do banco | 0 erros 500 em produção |
| Cópia de código funciona | Feedback visual em 100% dos cliques |
| Acessibilidade | Checklist WCAG AA aprovado antes de qualquer merge em `views/` |
| Segurança | Nenhuma senha em texto puro no banco de dados |

---

## 11. Estratégia de Testes

> **Nota:** Não há suite de testes automatizados configurada no MVP atual (`npm test` retorna erro). Os testes são manuais.

### Fluxos críticos a validar manualmente

| Fluxo | Passos | Resultado esperado |
|-------|--------|--------------------|
| Cadastro novo usuário | Preencher `/register` com dados válidos | Redireciona para `/login` |
| Cadastro duplicado | Repetir mesmo `username` ou `email` | Exibe mensagem de erro inline |
| Login válido | Credenciais corretas em `/login` | Redireciona para `/`, navbar mostra usuário |
| Login inválido | Senha errada | Exibe "Usuário ou senha incorretos", sem revelar qual campo falhou |
| Logout | Clicar em logout | Sessão destruída, retorna para `/` como anônimo |
| Ver códigos | Acessar `/codigos` | Lista carregada do banco; botão "Copiar" funciona |
| Marcar como resgatado | Clicar "✓" e recarregar | Estado persiste no `localStorage` |
| Guia de itens | Acessar `/guia` | Tabela renderizada com badges de lucro |
| Mapa | Acessar `/mapa` | Iframe carregado sem erros de console |

---

## 12. Perguntas Abertas

1. **Gerenciamento de códigos:** Quem insere/atualiza/expira os registros na tabela `codigos`? Hoje é manual via SQL. Um painel admin está previsto?
2. **Controle de acesso:** Alguma das páginas atuais deve ser protegida por login no futuro (ex.: `/codigos` somente para usuários autenticados)?
3. **Persistência de "resgatado":** O estado de código resgatado hoje vive no `localStorage` do navegador. Há interesse em sincronizar isso com a conta do usuário no banco?
4. **Segurança da sessão:** O `SESSION_SECRET` hardcoded em `index.js:11` precisa ser corrigido antes de qualquer deploy em produção. Quando isso será endereçado?
5. **Guia de itens:** Os dados da tabela `/guia` estão hard-coded no EJS. Há plano de migrar para o banco de dados para facilitar atualizações?
6. **Mapa:** O iframe aponta para `https://webresources.cc/heartopia/en.html` (recurso externo). Há SLA ou plano de contingência caso o recurso saia do ar?
