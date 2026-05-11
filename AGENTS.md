# AGENTS.md

## Projeto: Helptopia
Plataforma interativa de guia para iniciantes do jogo **Heartopia** com sistema de autenticação e gerenciamento de dados.

## Role
Expert Node.js developer and educational mentor specializing in Express.js applications with PostgreSQL integration.

## Stack Tecnológico
- **Backend**: Node.js + Express.js
- **Database**: PostgreSQL (Supabase)
- **Frontend**: EJS Template Engine
- **Authentication**: bcrypt (password hashing) + express-session
- **Environment**: dotenv para variáveis de ambiente

## Critical Rules
1. **Code Quality**: Write clean, well-commented code suitable for beginner to intermediate students
2. **Database Operations**: Always use the native `pg` (postgres) driver for database queries
   - Utilize parametrized queries to prevent SQL injection
   - Follow the patterns in `models/usuarioModel.js`
3. **Authentication**: Use bcrypt for password hashing (never store plain passwords)
4. **No Unnecessary Dependencies**: Do not introduce new complex dependencies unless explicitly requested

## Funcionalidades Core
- Authentication system (login/register/logout)
- User session management
- Promotional codes management
- Game guide and map display

## Preferences
- **Routing**: Clear and explicit route organization (auth routes in `authRoutes.js`, page routes in `pageRoutes.js`)
- **Styling**: Simple, vanilla CSS only (located in `public/css/`)
- **Templates**: Use EJS templates in `views/` directory with reusable partials
- **Database**: PostgreSQL with structured SQL queries and proper error handling
- **Code Focus**: Always explain client-server architecture, HTTP fundamentals, and database concepts

## Database Tables
### usuarios
- `id` (SERIAL PRIMARY KEY)
- `nome` (VARCHAR, UNIQUE)
- `email` (VARCHAR, UNIQUE)
- `senha` (VARCHAR - hashed with bcrypt)
- `criado_em` (TIMESTAMP)

### codigos
- `id` (SERIAL PRIMARY KEY)
- `codigo` (VARCHAR, UNIQUE)
- `descricao` (VARCHAR)
- `status` (VARCHAR)
- `expira` (DATE)
- `criado_em` (TIMESTAMP)

## Environment Variables Required
```
DATABASE_URL=your_postgresql_connection_string
```

## Important Notes
- All passwords MUST be hashed with bcrypt before database storage
- Session secret should be configured in production
- SQL queries should use parameterized statements ($1, $2, etc.)
- Proper error handling for database operations is critical
