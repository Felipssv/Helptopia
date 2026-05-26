Gere um PRD (Documento de Requisitos de Produto) para uma feature do Helptopia.

**Importante:** NÃO comece a implementar. Apenas crie o PRD.

## Passo 1 — Perguntas Esclarecedoras

Se o prompt for ambíguo, faça 3–5 perguntas com opções com letras para o usuário responder rapidamente (ex: "1A, 2C, 3B"):

```
1. Qual é o objetivo principal desta feature?
   A. Melhorar experiência do usuário
   B. Adicionar nova funcionalidade
   C. Corrigir problema existente
   D. Outro: [especifique]

2. Quem é o usuário-alvo?
   A. Apenas novos usuários
   B. Apenas usuários autenticados
   C. Todos os usuários
   D. Apenas admin

3. Qual é o escopo?
   A. Versão mínima viável
   B. Implementação completa
   C. Apenas backend/API
   D. Apenas UI/frontend
```

## Passo 2 — Estrutura do PRD

Gere o documento com estas seções:

### 1. Introdução/Visão Geral
### 2. Objetivos (bullets mensuráveis)
### 3. User Stories

Formato obrigatório por story:
```markdown
### US-001: [Título]
**Descrição:** Como um [usuário], quero [feature] para que [benefício].

**Critérios de Aceitação:**
- [ ] Critério verificável e específico
- [ ] Outro critério
- [ ] [Para stories com UI] Verificar no navegador
```

### 4. Requisitos Funcionais (numerados: RF-1, RF-2…)
### 5. Impacto no Banco de Dados (tabelas, SQL, constraints)
### 6. Rotas & Endpoints da API (método, path, request/response)
### 7. Não-Objetivos (o que esta feature NÃO fará)
### 8. Considerações de Design (UI/UX, WCAG 2.1 AA)
### 9. Considerações Técnicas (Express, pg, bcrypt, sessions, EJS)
### 10. Métricas de Sucesso
### 11. Estratégia de Testes
### 12. Perguntas Abertas

## Regras do PRD

- Critérios de aceitação devem ser verificáveis, não vagos. "Funciona corretamente" é ruim. "Botão mostra confirmação antes de deletar" é bom.
- Para qualquer story com mudanças de UI: sempre incluir critério de verificação no navegador.
- Queries SQL sempre parametrizadas (`$1`, `$2`) — referência: `models/usuarioModel.js`.
- Senhas sempre hashadas com bcrypt — nunca armazenar plaintext.
- Mensagens ao usuário em português (Brasil).
- Acessibilidade WCAG 2.1 AA obrigatória para mudanças em `views/` ou `public/`.

## Saída

- Formato: Markdown
- Local: `docs/prd/`
- Nome: `prd-[nome-da-feature].md` (kebab-case)
- Exemplos: `docs/prd/prd-codigos-promocionais.md`, `docs/prd/prd-perfil-usuario.md`
