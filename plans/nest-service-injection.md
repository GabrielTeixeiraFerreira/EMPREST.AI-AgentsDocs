# Plano: Injeção explícita dos serviços Nest

## Arquivos e módulos

- `EMPREST.AI-Back/src/modules/auth/auth.controller.ts`;
- `EMPREST.AI-Back/src/modules/system/system.controller.ts`;
- testes E2E existentes do backend.

## Abordagem técnica

Usar `@Inject(...)` nos parâmetros dos construtores dos controllers para
declarar explicitamente os tokens dos serviços. Isso corrige a resolução sob
`tsx` sem alterar os serviços, módulos ou contratos HTTP.

## Ordem de execução

1. Adicionar a injeção explícita nos dois controllers.
2. Executar testes E2E e build do backend.
3. Verificar `GET /` e o encaminhamento do login em execução local.
4. Conferir o status do Git e atualizar as tasks.

## Impacto

- Dados: nenhum.
- Contratos: nenhum.
- Segurança: nenhuma mudança.
- Frontend: nenhum arquivo alterado.

## Rastreabilidade

- Etapa 1: CA-01 e CA-02.
- Etapas 2-3: CA-03 e CA-04.
