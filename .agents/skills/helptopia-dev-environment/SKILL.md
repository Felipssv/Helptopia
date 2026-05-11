```skill
---
name: helptopia-dev-environment
description: Subir e validar ambiente local do Helptopia (PostgreSQL, .env). Use ao rodar o projeto, depurar conexão com o banco ou orientar setup de desenvolvimento.
---

# Ambiente de desenvolvimento — Helptopia

## Pré-requisitos

- Node.js LTS (v16+)
- PostgreSQL acessível via `DATABASE_URL` (padrão local: `postgresql://usuario:senha@localhost:5432/helptopia`)
- (Opcional) Supabase para ambiente cloud (a configuração é a mesma, mudando apenas o `DATABASE_URL`)

## Passos

1. Na raiz do repositório: `npm install`
2. Copiar `.env.example` para `.env` (se não existir) e ajustar:
   ```
   DATABASE_URL=postgresql://usuario:senha@localhost:5432/helptopia
   SESSION_SECRET=seu_secret_aleatorio_aqui
   NODE_ENV=development
   ```
3. Garantir que PostgreSQL está rodando (verificar porta 5432 por padrão).
4. Subir o servidor:
   - `node index.js` — porta **3000**
   - ou `npm run dev` (se nodemon está configurado em `package.json`)
5. Abrir `http://localhost:3000`

## PostgreSQL Setup

### Criar banco local (exemplo)

```bash
psql -U postgres
CREATE DATABASE helptopia;
\c helptopia
```

### Criar tabelas

```sql
-- Usuários
CREATE TABLE usuarios (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(255) NOT NULL UNIQUE,
  email VARCHAR(255) NOT NULL UNIQUE,
  senha VARCHAR(255) NOT NULL,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Códigos promocionais
CREATE TABLE codigos (
  id SERIAL PRIMARY KEY,
  codigo VARCHAR(50) NOT NULL UNIQUE,
  descricao VARCHAR(255),
  status VARCHAR(50),
  expira DATE,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Verificar tabelas: `\dt` no psql.

## Variáveis de Ambiente

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `DATABASE_URL` | Conexão PostgreSQL | `postgresql://user:pass@localhost:5432/helptopia` |
| `SESSION_SECRET` | Chave de sessão (segura em produção!) | `sua_chave_aleatoria_longa` |
| `NODE_ENV` | Ambiente | `development` ou `production` |
| `PORT` | Porta do servidor (opcional, padrão 3000) | `3000` |

## Falhas comuns

- **`ECONNREFUSED` ou erro de conexão**: PostgreSQL não está rodando ou `DATABASE_URL` incorreto — verificar `psql -l` para confirmar DB.
- **`relation "usuarios" does not exist`**: Tabelas não foram criadas — executar SQL acima.
- **`process.env.SESSION_SECRET is undefined`**: Verificar `.env` e garantir que `SESSION_SECRET` está preenchido.

## Debug

### Ver processo do banco

```bash
# Verificar se PostgreSQL está rodando
ps aux | grep postgres

# Conectar ao psql e checar BD
psql -U postgres -l
```

### Logs do servidor

Ativar logs em `index.js`:
```javascript
console.log('DATABASE_URL:', process.env.DATABASE_URL);
```

## Documentação

- Escopo funcional: `docs/prd/`
- Stack: `.agents/rules/helptopia-stack/`
- Rotas: `routes/` (authRoutes.js, pageRoutes.js)
- Modelos: `models/` (usuarioModel.js)

```
