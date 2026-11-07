# Tasks: Injeção explícita dos serviços Nest

## T-01 — Declarar dependências dos controllers

- Descrição: adicionar `@Inject(AuthService)` e `@Inject(SystemService)` nos
  construtores correspondentes.
- Arquivos: controllers de auth e system do backend.
- Critérios: CA-01, CA-02.
- Verificação: teste E2E e build.
- Estado: concluída.

## T-02 — Validar inicialização e regressão

- Descrição: executar testes, build e uma chamada local à rota raiz; conferir
  que o frontend permanece inalterado.
- Arquivos: backend e status do workspace.
- Critérios: CA-03, CA-04.
- Verificação: `npm test`, `npm run build` e `git status --short`.
- Estado: concluída.
