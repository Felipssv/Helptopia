---
name: prd
description: "Gere um PRD para features do Helptopia: autenticação de usuários, códigos promocionais, guia, mapa, etc. Crie Documentos de Requisitos de Produto detalhados com schema do banco, rotas da API e critérios de aceitação. Use ao planejar uma feature, iniciar uma nova especificação, ou quando pedir um PRD. Triggers: criar prd, escrever prd para, planejar esta feature, requisitos para, especificar."
user-invocable: true
---

# Gerador de PRD

Crie Documentos de Requisitos de Produto (PRD) detalhados, claros, acionáveis e prontos para implementação.

---

## O Trabalho

1. Receba a descrição de uma feature do usuário
2. Faça 3-5 perguntas esclarecedoras essenciais (com opções com letras)
3. Gere um PRD estruturado baseado nas respostas
4. Salve em `docs/prd-[nome-da-feature].md`

**Importante:** NÃO comece a implementar. Apenas crie o PRD.

---

## Passo 1: Perguntas Esclarecedoras

Faça apenas perguntas críticas quando o prompt inicial for ambíguo. Foque em:

- **Problema/Objetivo:** Qual problema isso resolve?
- **Funcionalidade Principal:** Quais são as ações-chave?
- **Escopo/Limites:** O que isso NÃO deve fazer?
- **Critérios de Sucesso:** Como sabemos que está pronto?

### Formate as Perguntas Assim:

```
1. Qual é o objetivo principal desta feature?
   A. Melhorar experiência de onboarding do usuário
   B. Aumentar retenção de usuários
   C. Reduzir carga de suporte
   D. Outro: [especifique]

2. Quem é o usuário-alvo?
   A. Apenas novos usuários
   B. Apenas usuários existentes
   C. Todos os usuários
   D. Apenas usuários admin

3. Qual é o escopo?
   A. Versão mínima viável
   B. Implementação completa
   C. Apenas backend/API
   D. Apenas UI
```

Isso permite que os usuários respondam com "1A, 2C, 3B" para iteração rápida. Lembre-se de indentar as opções.

---

## Passo 2: Estrutura do PRD

Gere o PRD com estas seções:

### 1. Introdução/Visão Geral
Descrição breve da feature e do problema que ela resolve.

### 2. Objetivos
Objetivos específicos e mensuráveis (lista com bullets).

### 3. User Stories
Cada story precisa:
- **Título:** Nome descritivo curto
- **Descrição:** "Como um [usuário], quero [feature] para que [benefício]"
- **Critérios de Aceitação:** Checklist verificável do que significa "pronto"

Cada story deve ser pequena o suficiente para implementar em uma sessão focada.

**Formato:**
```markdown
### US-001: [Título]
**Descrição:** Como um [usuário], quero [feature] para que [benefício].

**Critérios de Aceitação:**
- [ ] Critério específico verificável
- [ ] Outro critério
- [ ] Typecheck/lint passa
- [ ] **[Apenas stories UI]** Verificar no navegador usando skill dev-browser
```

**Importante:** 
- Critérios de aceitação devem ser verificáveis, não vagos. "Funciona corretamente" é ruim. "Botão mostra diálogo de confirmação antes de deletar" é bom.
- **Para qualquer story com mudanças UI:** Sempre inclua "Verificar no navegador usando skill dev-browser" nos critérios de aceitação. Isso garante verificação visual do trabalho frontend.

### 4. Requisitos Funcionais
Lista numerada de funcionalidades específicas:
- "RF-1: O sistema deve permitir que usuários..."
- "RF-2: Quando um usuário clica X, o sistema deve..."

Seja explícito e inequívoco.

### 5. Impacto no Banco de Dados (se aplicável)
- Tabelas afetadas ou criadas
- Migrations necessárias
- Constraints e índices
- Exemplo: `CREATE TABLE usuarios (id SERIAL PRIMARY KEY, email VARCHAR UNIQUE, ...)`

### 6. Rotas & Endpoints da API (se aplicável)
- Endpoints afetados ou criados
- Métodos HTTP e caminhos
- Estrutura de request/response
- Exemplo: `POST /auth/register`, `GET /codigos`, etc.
- Referência: Use queries parametrizadas com driver `pg` (`.agents/rules/helptopia-stack/`)

### 7. Não-Objetivos (Fora do Escopo)
O que essa feature NÃO incluirá. Crítico para gerenciar escopo.

Especificamente para Helptopia:
- Deixe claro o escopo do guia/mapa (features educacionais vs. features do jogo)
- Especifique limites de autenticação (sem single sign-on, OAuth, etc.)
- Defina limites de códigos promocionais (uso único? expiração? etc.)

### 8. Considerações de Design (Opcional)
- Requisitos de UI/UX
- Link para mockups se disponível
- Componentes existentes relevantes para reutilizar
- Requisitos de acessibilidade (WCAG 2.1 AA por `.agents/rules/frontend-wcag-accessibility`)

### 9. Considerações Técnicas (Opcional)
- Constraints ou dependências conhecidas
- Pontos de integração com sistemas existentes
- Requisitos de performance
- Para Helptopia: Referência a padrões do stack (Express, PostgreSQL, bcrypt, sessions, templates EJS)

### 10. Métricas de Sucesso
Como o sucesso será medido?
- "Reduzir tempo para completar X em 50%"
- "Aumentar taxa de conversão em 10%"

### 11. Estratégia de Testes (se aplicável)
- Como testar essa feature (Jest, testes manuais, testes de integração)
- Para backend: Validação de queries usando PostgreSQL
- Para frontend: Testes no navegador usando skill dev-browser

### 12. Perguntas Abertas
Perguntas restantes ou áreas que precisam de esclarecimento.

---

## Escrevendo para Desenvolvedores Junior

O leitor do PRD pode ser um desenvolvedor junior ou um agente de IA. Portanto:

- Seja explícito e inequívoco
- Evite jargão ou explique-o
- Forneça detalhes suficientes para entender propósito e lógica principal
- Numere requisitos para fácil referência
- Use exemplos concretos quando apropriado

---

## Saída

- **Formato:** Markdown (`.md`)
- **Localização:** `docs/prd/`
- **Nome do arquivo:** `prd-[nome-da-feature].md` (kebab-case)
- **Exemplo:** `docs/prd/prd-autenticacao-usuario.md`, `docs/prd/prd-codigos-promocionais.md`

---

## Exemplo de PRD (Genérico)

```markdown
# PRD: Sistema de Prioridade de Tarefas

Adicione níveis de prioridade às tarefas para que os usuários possam se concentrar no que mais importa. As tarefas podem ser marcadas como prioridade alta, média ou baixa, com indicadores visuais e filtragem para ajudar os usuários a gerenciar sua carga de trabalho.

## Objetivos

- Permitir atribuição de prioridade (alta/média/baixa) a qualquer tarefa
- Fornecer diferenciação visual clara entre níveis de prioridade
- Permitir filtragem e ordenação por prioridade
- Padrão de novas tarefas como prioridade média

## User Stories

### US-001: Adicionar campo de prioridade ao banco
**Descrição:** Como desenvolvedor, preciso armazenar prioridade de tarefa para que persista entre sessões.

**Critérios de Aceitação:**
- [ ] Adicionar coluna de prioridade à tabela de tarefas: 'alta' | 'média' | 'baixa' (padrão 'média')
- [ ] Gerar e executar migration com sucesso
- [ ] Typecheck passa

### US-002: Exibir indicador de prioridade em cards de tarefas
**Descrição:** Como usuário, quero ver a prioridade da tarefa à primeira vista para saber o que precisa de atenção primeiro.

**Critérios de Aceitação:**
- [ ] Cada card de tarefa mostra badge colorida de prioridade (vermelho=alta, amarelo=média, cinza=baixa)
- [ ] Prioridade visível sem passar o mouse ou clicar
- [ ] Typecheck passa
- [ ] Verificar no navegador usando skill dev-browser

### US-003: Adicionar seletor de prioridade à edição de tarefa
**Descrição:** Como usuário, quero mudar a prioridade de uma tarefa ao editá-la.

**Critérios de Aceitação:**
- [ ] Dropdown de prioridade no modal de edição de tarefa
- [ ] Mostra prioridade atual como selecionada
- [ ] Salva imediatamente na mudança de seleção
- [ ] Typecheck passa
- [ ] Verificar no navegador usando skill dev-browser

### US-004: Filtrar tarefas por prioridade
**Descrição:** Como usuário, quero filtrar a lista de tarefas para ver apenas itens de alta prioridade quando estou focado.

**Critérios de Aceitação:**
- [ ] Dropdown de filtro com opções: Todas | Alta | Média | Baixa
- [ ] Filtro persiste em parâmetros de URL
- [ ] Mensagem de estado vazio quando nenhuma tarefa corresponde ao filtro
- [ ] Typecheck passa
- [ ] Verificar no navegador usando skill dev-browser

## Requisitos Funcionais

- RF-1: Adicionar campo `prioridade` à tabela de tarefas ('alta' | 'média' | 'baixa', padrão 'média')
- RF-2: Exibir badge colorida de prioridade em cada card de tarefa
- RF-3: Incluir seletor de prioridade no modal de edição de tarefa
- RF-4: Adicionar dropdown de filtro de prioridade ao cabeçalho da lista
- RF-5: Ordenar por prioridade dentro de cada coluna de status (alta para média para baixa)

## Não-Objetivos

- Nenhuma notificação ou lembrete baseado em prioridade
- Nenhuma atribuição automática de prioridade baseada em data de vencimento
- Nenhuma herança de prioridade para subtarefas

## Considerações Técnicas

- Reutilizar componente de badge existente com variantes de cor
- Estado de filtro gerenciado via parâmetros de URL
- Prioridade armazenada no banco, não computada

## Métricas de Sucesso

- Usuários conseguem mudar prioridade em menos de 2 cliques
- Tarefas de alta prioridade imediatamente visíveis no topo das listas
- Nenhuma regressão na performance da lista de tarefas

## Perguntas Abertas

- A prioridade deve afetar a ordenação de tarefas dentro de uma coluna?
- Devemos adicionar atalhos de teclado para mudanças de prioridade?
```

---

## Exemplo de PRD — Helptopia: Sistema de Autenticação de Usuários

```markdown
# PRD: Sistema de Autenticação de Usuários

## Introdução

Implementar sistema de autenticação seguro para Helptopia, permitindo que novos usuários se registrem e façam login na plataforma de guia do Heartopia. O sistema deve usar bcrypt para hashing de senhas e express-session para manter sessões seguras.

## Objetivos

- Permitir registro de novos usuários com email e senha
- Autenticar usuários com email e senha no login
- Manter sessão do usuário logado
- Proteger rotas de conteúdo exclusivo para usuários autenticados
- Permitir logout seguro

## User Stories

### US-001: Criar tabela de usuários no banco
**Descrição:** Como desenvolvedor, preciso armazenar dados de usuários no PostgreSQL para que possam se registrar e fazer login.

**Critérios de Aceitação:**
- [ ] Tabela `usuarios` criada com coluna `id` (PRIMARY KEY), `nome` (UNIQUE), `email` (UNIQUE), `senha` (hashed), `criado_em`
- [ ] Índice único em `email` garantido
- [ ] Conexão com `pg.Pool` testada em `db.js`

### US-002: Criar formulário de registro
**Descrição:** Como novo usuário, quero me registrar fornecendo nome, email e senha na página `/register`.

**Critérios de Aceitação:**
- [ ] View EJS em `views/register.ejs` com campos: nome, email, senha, confirmação
- [ ] Validação client-side (HTML5) e server-side
- [ ] Mensagens de erro exibidas (email duplicado, senhas não conferem)
- [ ] Teclado navegável (Tab, Enter)
- [ ] Contraste WCAG 2.1 AA (≥4.5:1)
- [ ] Verificar no navegador usando skill dev-browser

### US-003: Hash de senha e inserção no banco
**Descrição:** Como desenvolvedor, preciso safe-guardar senhas com bcrypt antes de armazenar no BD.

**Critérios de Aceitação:**
- [ ] Função em `models/usuarioModel.js` para inserir usuário com `bcrypt.hash(senha, 10)`
- [ ] Query parametrizada com `pg` (`INSERT INTO usuarios ... VALUES ($1, $2, $3, $4)`)
- [ ] Erro de email duplicado capturado (constraint UNIQUE)
- [ ] Teste manual: inserir usuário e verificar MD5 da senha NÃO é o plaintext

### US-004: Criar formulário de login
**Descrição:** Como usuário registrado, quero fazer login com email e senha.

**Critérios de Aceitação:**
- [ ] View EJS em `views/login.ejs` com campos: email, senha
- [ ] Validação server-side (email existe, senha correta)
- [ ] Mensagem "Usuário ou senha incorretos" (genérica por segurança)
- [ ] Teclado navegável
- [ ] Contraste WCAG AA
- [ ] Verificar no navegador usando skill dev-browser

### US-005: Setup de session com bcrypt.compare
**Descrição:** Como desenvolvedor, preciso validar senha com bcrypt.compare e iniciar sessão.

**Critérios de Aceitação:**
- [ ] Rota `POST /auth/login` compara `bcrypt.compare(inputSenha, senhaHasheada)`
- [ ] Query parametrizada busca usuário por email: `SELECT * FROM usuarios WHERE email = $1`
- [ ] Session criada com `req.session.userId = user.id`
- [ ] Redirect a `/` ou página home após sucesso
- [ ] Teste manual: login válido e inválido

### US-006: Criar rota de logout
**Descrição:** Como usuário, quero fazer logout da plataforma.

**Critérios de Aceitação:**
- [ ] Rota `GET /auth/logout` executa `req.session.destroy()`
- [ ] Redirect a `/login` após logout
- [ ] Botão "Sair" na navbar redirecionado a `/auth/logout`
- [ ] Sessão realmente destruída verificada no navegador

### US-007: Middleware de autenticação
**Descrição:** Como desenvolvedor, preciso proteger rotas de pages exclusivas para logados.

**Critérios de Aceitação:**
- [ ] Middleware `requireAuth(req, res, next)` verifica `req.session.userId`
- [ ] Se não autenticado, redirect a `/login`
- [ ] Aplicado em rotas sensíveis: `/guia`, `/mapa`, `/codigos`
- [ ] Teste manual: tentar acessar `/codigos` sem login redireciona

## Impacto no Banco de Dados

**Nova Tabela: `usuarios`**
```sql
CREATE TABLE usuarios (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(255) NOT NULL UNIQUE,
  email VARCHAR(255) NOT NULL UNIQUE,
  senha VARCHAR(255) NOT NULL,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_usuario_email ON usuarios(email);
```

**Migration necessária em `db.js`:**
- Executar criação de tabela na primeira inicialização, ou via script SQL manual

## Rotas & Endpoints da API

- **POST /auth/register** — Registra novo usuário
  - Body: `{ nome, email, senha, senhaConfirm }`
  - Response: Redirect `/login` ou erro com status 400

- **POST /auth/login** — Faz login e cria sessão
  - Body: `{ email, senha }`
  - Response: Redirect `/` ou erro com status 401

- **GET /auth/logout** — Destrói sessão
  - Response: Redirect `/login`

**Padrão de segurança:** Todas as queries usam parametrização com `pg` (`$1`, `$2`); nunca string concatenation. Senhas sempre hashadas com bcrypt.

## Requisitos Funcionais

- RF-1: Usuário pode registrar com nome, email, senha
- RF-2: Email deve ser único no banco
- RF-3: Senha é hasheada com bcrypt antes de armazenar
- RF-4: Usuário pode fazer login com email e senha
- RF-5: Session mantém usuário logado entre requisições
- RF-6: Usuário pode fazer logout
- RF-7: Rotas protegidas redirecionam usuários não autenticados para `/login`
- RF-8: Mensagens de erro claras (sem vazar dados sensíveis)

## Não-Objetivos (Helptopia)

- Não implementar single sign-on, OAuth, ou third-party auth
- Não adicionar "esqueci minha senha" ou reset por email
- Não adicionar two-factor authentication
- Não implementar permissões de admin (apenas autenticado vs. não autenticado)

## Considerações de Design

- Navbar mostra nome do usuário logado (de `req.session.userId`)
- Link "Sair" aparece apenas se autenticado
- Formulários seguem padrão da skill `wcag-21-aa-testing`
- Reuse de `views/partials/header.ejs` e `footer.ejs`

## Considerações Técnicas

- Stack: Express.js, PostgreSQL (`pg` driver), bcrypt, express-session
- Senhas hasheadas com `bcrypt.hash(senha, 10)`
- Queries parametrizadas obrigatórias (`.agents/rules/helptopia-stack/`)
- SESSION_SECRET em `.env` (gerado aleatoriamente)
- Error handling: try/catch nas rotas, nunca vazar stack trace

## Estratégia de Testes

- **Backend:**
  - Teste inserção de usuário sem bcrypt (deve falhar ao comparar senha)
  - Teste login com email inexistente (erro genérico)
  - Teste logout destruindo sessão e tentando acessar rota protegida
  - Teste middleware `requireAuth` com e sem sessão

- **Frontend:**
  - Testar navegação via teclado (Tab, Enter, Escape)
  - Testar contraste de cores (Lighthouse Accessibility)
  - Testar em diferentes resoluções (zoom 200%)

- **Manual:**
  - Registrar novo usuário → verificar no psql que senha é hashed
  - Fazer login com senha errada → erro genérico
  - Fazer login válido → redireciona, navbar mostra nome
  - Logout → redireciona, session destruída

## Métricas de Sucesso

- Novo usuário consegue registrar em menos de 30 segundos
- Login inválido trata erro sem vazar dados
- Session persiste entre page refreshes
- Acesso a rota protegida sem login redireciona imediatamente
- Nenhuma senha em plaintext no banco

## Perguntas Abertas

- Qual deve ser o requisito mínimo de caracteres para senha?
- Enviar confirmação de email ou apenas registrar?
- Mostrar indicador de força de senha?
```

---

## Checklist

Antes de salvar o PRD:

- [ ] Fez perguntas esclarecedoras com opções com letras
- [ ] Incorporou respostas do usuário
- [ ] User stories são pequenas e específicas
- [ ] Requisitos funcionais são numerados e inequívocos
- [ ] Seção de não-objetivos define limites claros
- [ ] Seção de Impacto no BD inclui estrutura de tabelas (se aplicável)
- [ ] Seção de Rotas da API descreve endpoints e métodos HTTP (se aplicável)
- [ ] Estratégia de Testes inclui abordagem para backend e frontend
- [ ] Salvo em `docs/prd-[nome-da-feature].md`
