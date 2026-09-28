# Regra de testes

> leitor: agente

## Fonte da regra

Esta regra segue as decisões de testes registradas em
`EMPREST.AI/docs/adr/001-stack.md`. A ADR é a fonte da escolha das
ferramentas; os scripts definidos nos `package.json` de cada repositório são a
fonte dos comandos executáveis.

## Matriz obrigatória

### Backend

- use Jest para testes unitários;
- teste services sem subir a aplicação inteira, substituindo dependências por
  doubles quando isso não esconder o comportamento sob teste;
- teste repositories e comportamento de persistência com Testcontainers e um
  Postgres real;
- use supertest sobre a aplicação Nest para testar a API, incluindo guards,
  validação de entrada e filtro de exceções;
- cubra o isolamento entre tenants em pelo menos um caso por endpoint: o tenant
  A não pode enxergar dados do tenant B.

Não use Prisma mockado como substituto dos testes de repository. Mockar o ORM
não verifica constraint, cascade, transação ou policy de RLS.

### Frontend

- use Vitest para testes unitários;
- teste componentes, validações e lógica de estado sem depender do navegador
  quando o comportamento não exigir integração visual;
- use Playwright para os fluxos E2E que travam a operação, incluindo login,
  abertura de chamado e cadastro de ativo quando esses fluxos existirem;
- mantenha os testes compatíveis com React, Vite e TypeScript em modo `strict`.

### Contrato entre repositórios

- a API publica `openapi.json` como artefato do CI;
- o frontend regenera o cliente por script a partir desse contrato;
- o CI do frontend deve falhar quando o cliente gerado divergir do
  `openapi.json` publicado;
- uma alteração de contrato deve incluir a regeneração e a verificação do
  cliente no escopo da tarefa.

## Caminhos críticos

Não existe meta percentual de cobertura. A aprovação depende de testar os
caminhos críticos do comportamento alterado, incluindo, quando aplicável:

- autenticação e proteção de rotas;
- autorização por papel e tenant;
- criação, atualização e leitura dos recursos alterados;
- validação de entrada e formato RFC 9457 dos erros;
- transações, constraints e cascades relevantes;
- isolamento de dados entre tenants;
- estados de carregamento, sucesso e erro no frontend;
- fluxos operacionais cobertos por E2E.

## Execução por escopo

Antes de testar:

1. identifique se a mudança afeta frontend, backend ou ambos;
2. leia os scripts do `package.json` no repositório afetado;
3. execute o teste mais estreito que cubra a mudança;
4. execute a suíte do repositório quando a alteração cruzar módulos ou
   contratos;
5. execute o build correspondente conforme `rules/check.md`.

Para mudanças que afetam frontend e backend, valide os dois lados. Para
mudanças de banco, use também `rules/migration.md`; os testes de integração
devem usar banco local e nunca o banco remoto.

## Critérios de conclusão

Uma tarefa de código só pode ser considerada validada quando:

- os testes aplicáveis terminarem sem falha;
- novos caminhos críticos tiverem testes ou uma justificativa explícita de
  por que não são testáveis nesta etapa;
- o teste de isolamento tiver sido criado ou preservado para cada endpoint
  afetado;
- a verificação de contrato tiver passado quando houver mudança no OpenAPI;
- o build correspondente tiver passado;
- os resultados e a última linha de cada comando relevante forem registrados
  conforme `rules/check.md`.

Se um teste falhar, pare, explique a causa e o impacto e aguarde aprovação
antes de alterar o código. Não masque falhas removendo assertions, reduzindo o
escopo do teste ou substituindo uma dependência real por mock sem justificar a
mudança.
