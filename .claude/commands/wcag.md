Execute uma auditoria de acessibilidade WCAG 2.1 nível AA no código frontend indicado (views EJS ou CSS em `public/`).

## Critérios Obrigatórios

| Área | WCAG | O que verificar |
|------|------|----------------|
| Contraste | 1.4.3 | Texto normal ≥ **4.5:1**; texto grande (≥18pt ou 14pt bold) ≥ **3:1** |
| Teclado | 2.1.1 | Toda ação acessível sem mouse (Tab, Shift+Tab, Enter, Space, Escape) |
| Imagens | 1.1.1 | `alt` descritivo em imagens informativas; `alt=""` se decorativas |
| Formulários | 1.3.1 / 3.3.2 | `<label for>` em cada input; erros via `aria-describedby`; campo obrigatório indicado por texto, não só cor |
| Foco visível | 2.4.7 | `:focus` / `:focus-visible` nunca removidos sem equivalente perceptível |
| Estrutura | 1.3.1 | Um único `<h1>` por página; hierarquia `h1→h2→h3` coerente; `<main>`, `<nav>`, `<header>`, `<footer>` presentes |
| Layout | 1.4.4 / 2.5.5 | Usável com zoom até 200%; alvos clicáveis com área suficiente |
| Movimento | 2.3.3 | `prefers-reduced-motion` onde houver animação relevante |

## Checklist Rápido (antes de concluir qualquer mudança em views/ ou public/)

1. Todo `<img>` tem `alt` descritivo (ou `alt=""` se decorativa)?
2. Links e botões têm foco visível (`:focus`)?
3. Cada `<input>` tem `<label for="id">` correspondente?
4. Hierarquia de headings (`h1 → h2 → h3`) é coerente e sem pulos?
5. Razão de contraste do texto ≥ 4.5:1 versus o fundo?
6. Mensagens de erro/campo obrigatório NÃO usam só cor para transmitir informação?
7. Tags semânticas (`<main>`, `<nav>`, `<header>`, `<footer>`) estão presentes?
8. Zoom até 200% não quebra o layout?

## Como Executar a Auditoria

1. Ler os arquivos EJS ou CSS indicados.
2. Verificar cada item do checklist acima.
3. Para contraste: calcular ou estimar a razão usando os valores de cor presentes no CSS (`public/css/style.css`).
4. Reportar: para cada problema encontrado, indicar o critério WCAG violado, o trecho de código, e a correção recomendada.
5. Ao final, listar itens aprovados e itens que precisam de correção.

## Ferramentas de Referência (para sugerir ao usuário)

- Contraste: https://webaim.org/resources/contrastchecker/
- Audit completo: Lighthouse (aba Accessibility no DevTools)
- Leitor de tela para teste manual: NVDA (Windows) ou Orca (Linux)
