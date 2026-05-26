Guie o usuário para subir e validar o ambiente local do Helptopia. Siga os passos abaixo:

## Pré-requisitos

- Node.js LTS (v16+)
- PostgreSQL acessível via `DATABASE_URL`
- (Opcional) Supabase para ambiente cloud

## Passos

1. Na raiz do repositório: `npm install`
2. Criar `.env` (se não existir) com:
   ```
   DATABASE_URL=postgresql://usuario:senha@localhost:5432/helptopia
   SESSION_SECRET=seu_secret_aleatorio_aqui
   NODE_ENV=development
   ```
3. Garantir que PostgreSQL está rodando (porta 5432 por padrão).
4. Subir o servidor: `npm run dev` (porta **3000**)
5. Abrir `http://localhost:3000`

## Criar banco local (se ainda não existir)

```bash
psql -U postgres
CREATE DATABASE helptopia;
\c helptopia
```

```sql
CREATE TABLE usuarios (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(255) NOT NULL UNIQUE,
  email VARCHAR(255) NOT NULL UNIQUE,
  senha VARCHAR(255) NOT NULL,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE codigos (
  id SERIAL PRIMARY KEY,
  codigo VARCHAR(50) NOT NULL UNIQUE,
  descricao VARCHAR(255),
  status VARCHAR(50),
  expira DATE,
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Variáveis de Ambiente

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `DATABASE_URL` | Conexão PostgreSQL | `postgresql://user:pass@localhost:5432/helptopia` |
| `SESSION_SECRET` | Chave de sessão | `chave_aleatoria_longa` |
| `NODE_ENV` | Ambiente | `development` ou `production` |
| `PORT` | Porta (opcional, padrão 3000) | `3000` |

## Falhas Comuns

- **`ECONNREFUSED`**: PostgreSQL não está rodando — verificar com `psql -l`
- **`relation "usuarios" does not exist`**: Tabelas não foram criadas — executar SQL acima
- **`SESSION_SECRET is undefined`**: Verificar se `.env` está preenchido

## Debug

```bash
# Verificar se PostgreSQL está rodando
ps aux | grep postgres

# Conectar ao banco e checar tabelas
psql -U postgres -d helptopia -c "\dt"
```
