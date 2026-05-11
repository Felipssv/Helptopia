# Helptopia

Plataforma interativa de guia para iniciantes do jogo **Heartopia**! Um site educativo que oferece informações essenciais, mapas do jogo, dicas de códigos promocionais e um sistema de autenticação para personalizar a experiência do usuário.

## Sobre o Projeto

Helptopia é uma aplicação web desenvolvida com **Node.js** e **Express.js** que serve como guia completo para novos jogadores de Heartopia. A plataforma oferece um ponto centralizado de aprendizado com funcionalidades de autenticação, consulta de informações e gerenciamento de códigos promocionais.

## Funcionalidades Principais

- **Autenticação Segura**: Sistema completo de login e registro com criptografia de senha (bcrypt)
- **Guia do Jogo**: Página dedicada com instruções e dicas para iniciantes querendo enriquecer dentro do jogo
- **Mapa Interativo**: Visualização do mapa do jogo Heartopia
- **Códigos Promocionais**: Lista de códigos válidos e expirados com data de validade
- **Sessão de Usuário**: Sistema de sessão para manter usuários autenticados
- **Interface Responsiva**: Design simples e intuitivo em CSS vanilla

## Banco de Dados

O projeto utiliza **PostgreSQL** hospedado no **Supabase** com as seguintes tabelas:

### Tabela: `usuarios`
Armazena informações dos usuários registrados:

```sql
CREATE TABLE usuarios (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(100) NOT NULL UNIQUE,
  email VARCHAR(100) NOT NULL UNIQUE,
  senha VARCHAR(255) NOT NULL,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Campos:**
- `id`: Identificador único do usuário
- `nome`: Nome de usuário (deve ser único)
- `email`: Email do usuário (deve ser único)
- `senha`: Senha criptografada com bcrypt
- `criado_em`: Data de criação da conta

### Tabela: `codigos`
Armazena os códigos promocionais disponíveis:

```sql
CREATE TABLE codigos (
  id SERIAL PRIMARY KEY,
  codigo VARCHAR(50) NOT NULL UNIQUE,
  descricao VARCHAR(255),
  status VARCHAR(20) DEFAULT 'ativo',
  expira DATE,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Campos:**
- `id`: Identificador único do código
- `codigo`: Código promocional (deve ser único)
- `descricao`: Descrição da recompensa/benefício
- `status`: Estado do código (ativo ou expirado)
- `expira`: Data de expiração do código
- `criado_em`: Data de criação do código

## Stack Tecnológico

- **Backend**: Node.js + Express.js
- **Frontend**: EJS (Template Engine)
- **Banco de Dados**: PostgreSQL (Supabase)
- **Autenticação**: bcrypt para hash de senhas, express-session para gerenciamento de sessões
- **Utilitários**: dotenv para variáveis de ambiente

## Pré-requisitos

- Node.js (versão 14 ou superior)
- npm ou yarn
- Conexão com banco de dados PostgreSQL (Supabase recomendado)

## Instalação e Execução

### 1. Clone o repositório
```bash
git clone https://github.com/Felipssv/Helptopia.git
cd Helptopia
```

### 2. Instale as dependências
```bash
npm install
```

### 3. Configure as variáveis de ambiente
Crie um arquivo `.env` na raiz do projeto:
```
DATABASE_URL=sua_url_de_conexao_postgresql
```

### 4. Execução

**Modo produção:**
```bash
npm start
```

**Modo desenvolvimento (com nodemon para recarregamento automático):**
```bash
npm run dev
```

O aplicativo será executado em `http://localhost:3000`. Abra seu navegador e acesse essa URL.

## Estrutura do Projeto

```
Helptopia/
├── index.js                  # Arquivo principal do servidor Express
├── db.js                     # Configuração da conexão com PostgreSQL
├── package.json              # Dependências e scripts do projeto
├── models/
│   └── usuarioModel.js       # Funções de banco de dados para usuários
├── routes/
│   ├── authRoutes.js         # Rotas de autenticação (login, register, logout)
│   └── pageRoutes.js         # Rotas de páginas (home, guia, mapa, códigos)
├── views/
│   ├── home.ejs              # Página inicial
│   ├── login.ejs             # Página de login
│   ├── register.ejs          # Página de registro
│   ├── guia.ejs              # Página do guia do jogo
│   ├── mapa.ejs              # Página do mapa interativo
│   ├── codigos.ejs           # Página de códigos promocionais
│   └── partials/             # Templates reutilizáveis
│       ├── header.ejs        # Cabeçalho
│       ├── navbar.ejs        # Barra de navegação
│       └── footer.ejs        # Rodapé
├── public/
│   ├── css/
│   │   └── style.css         # Estilos CSS
│   └── img/                  # Imagens do projeto
└── docs/
    └── prd/                  # Documentação e especificações
```

## Fluxo de Autenticação

1. **Registro**: Usuário preenche formulário com nome, email, senha e confirmação
2. **Validação**: Sistema verifica se usuário/email já existem e se senhas coincidem
3. **Hash**: Senha é criptografada com bcrypt antes de armazenar no banco
4. **Login**: Usuário autentica com nome de usuário ou email + senha
5. **Sessão**: Após login bem-sucedido, sessão é criada e mantém usuário autenticado
6. **Logout**: Sessão é destruída e usuário desconectado

## 🎯 Rotas Disponíveis

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/` | Página inicial |
| GET | `/login` | Página de login |
| POST | `/login` | Processa login |
| GET | `/register` | Página de registro |
| POST | `/register` | Processa registro |
| GET | `/logout` | Faz logout do usuário |
| GET | `/guia` | Guia para iniciantes |
| GET | `/mapa` | Mapa do jogo |
| GET | `/codigos` | Lista de códigos promocionais |

## Autores

- Felipe Sorrentino
- Erin Sarmento