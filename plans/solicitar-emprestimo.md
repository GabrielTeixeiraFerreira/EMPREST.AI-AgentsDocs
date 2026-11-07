# Plano: solicitar empréstimo

## Arquivos ou módulos

- `EMPREST.AI-Back/src/modules/loan/`: criar serviço, controlador e módulo
  para solicitação e listagem de empréstimos do colaborador.
- `EMPREST.AI-Back/src/app.module.ts`: registrar o módulo de empréstimos.
- `EMPREST.AI-Back/src/modules/equipment/`: ampliar o contrato do catálogo para
  expor o estado necessário ao frontend.
- `EMPREST.AI-Back/test/`: adicionar testes de regras, autorização e
  concorrência observável pela API.
- `EMPREST.AI/index.html`, `script.js` e `styles.css`: adicionar ação de
  solicitar, área de empréstimos em aberto e estados visuais, mantendo o
  catálogo sem rodapé/crédito.
- `EMPREST.AI/docs/specs/` e documentação de contrato: atualizar somente se o
  contrato público existente exigir.

## Abordagem técnica

1. Reutilizar o modelo `Loan` existente, sem migration.
2. Obter o usuário pelo token Bearer e validar a associação ao tenant em todas
   as consultas e comandos.
3. Criar uma transação que bloqueie a linha de `Equipment`, valide status e
   empréstimo ativo, conte empréstimos do colaborador, verifique atraso,
   calcule 14 dias úteis e crie o empréstimo.
4. Tratar a perda de concorrência com erro de conflito explícito, sem
   fallback silencioso.
5. Reaproveitar o cliente HTTP do frontend e atualizar o estado local apenas
   após resposta bem-sucedida.

## Ordem de execução

1. Implementar o contrato backend e as regras de negócio.
2. Criar testes backend para regras, tenant e concorrência.
3. Integrar catálogo, solicitação e empréstimos em aberto no frontend.
4. Validar build e testes dos dois repositórios.
5. Conferir os critérios de aceitação e o status do worktree.

## Estratégia de testes e build

- Backend: `npm test`, `npm run build` e `npm run db:validate`.
- Frontend: `npm run build`.
- Testar pelo menos: sucesso, manutenção, item emprestado, limite 5, atraso,
  cálculo de dias úteis, concorrência, isolamento do usuário e estados de
  erro/sucesso no frontend.

## Impacto em dados, contratos, segurança e operação

- Não há alteração de schema ou migration.
- Serão adicionados endpoints de empréstimos e seus contratos de resposta.
- O token nunca será persistido fora da sessão atual nem incluído em logs.
- A operação é atômica no banco e não acessa banco remoto durante os testes.

## Relação com critérios

- Etapas 1 e 2: CA-02 a CA-07.
- Etapa 3: CA-01 e CA-08.
- Etapas 4 e 5: todos os critérios.
