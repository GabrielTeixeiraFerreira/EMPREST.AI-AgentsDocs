# Tasks: solicitar empréstimo

## T-01 — Criar contrato e regras de empréstimo

- **Descrição:** Implementar módulo, controlador e serviço backend para
  solicitar empréstimo e listar empréstimos em aberto do colaborador.
- **Área:** `EMPREST.AI-Back/src/`.
- **Critérios:** CA-02, CA-03, CA-04, CA-05, CA-06, CA-07.
- **Verificação:** testes unitários/API e `npm run build`.
- **Estado:** concluída.

## T-02 — Testar regras e isolamento do backend

- **Descrição:** Cobrir sucesso, bloqueios, dias úteis, concorrência e
  isolamento por usuário/tenant sem usar banco remoto.
- **Área:** `EMPREST.AI-Back/test/` e testes dos serviços.
- **Critérios:** CA-02 a CA-07.
- **Verificação:** `npm test` e `npm run db:validate`.
- **Estado:** concluída.

## T-03 — Integrar solicitação no frontend

- **Descrição:** Exibir catálogo limpo, botão apenas para itens disponíveis,
  lista de empréstimos em aberto e estados de carregamento, sucesso e erro.
- **Área:** `EMPREST.AI/index.html`, `script.js`, `styles.css`.
- **Critérios:** CA-01, CA-08.
- **Verificação:** `npm run build`.
- **Estado:** concluída.

## T-04 — Verificar entrega completa

- **Descrição:** Executar validações dos dois repositórios, conferir cada
  critério e garantir que o worktree contenha somente alterações da tarefa.
- **Área:** repositório inteiro.
- **Critérios:** CA-01 a CA-08.
- **Verificação:** regras de `rules/check.md` e `git status --short`.
- **Estado:** concluída.
