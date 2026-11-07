# Handoff: Associar equipamentos ao usuário

- Data: 2026-11-07
- Escopo: backend
- Status: bloqueada

## Objetivo

Associar todos os equipamentos já existentes no banco ao usuário de UID
`cb10204d-f92d-4d28-9ae2-45e7e748cf68`.

## O que foi feito

- O usuário confirmou que o escopo é somente o backend/banco.
- Foi revisado o modelo Prisma e confirmado que equipamentos pertencem a um
  `tenant`; usuários são associados aos tenants por `memberships`.
- Foi concluído que não deve ser criada uma relação direta ou coluna de usuário
  em `equipment`.
- Foi proposta uma operação idempotente para inserir ou atualizar a membership
  do usuário em cada tenant que possui equipamentos.
- Não foram alterados código, schema, migrations ou dados.
- Nenhum commit foi criado.

## Estado atual

A operação está aguardando aprovação explícita do usuário antes de qualquer
alteração. A proposta atual é:

```sql
INSERT INTO memberships (user_id, tenant_id, role)
SELECT 'cb10204d-f92d-4d28-9ae2-45e7e748cf68'::uuid, tenant_id, 'collaborator'
FROM equipment
GROUP BY tenant_id
ON CONFLICT (user_id, tenant_id)
DO UPDATE SET role = EXCLUDED.role;
```

Essa operação associa o usuário a todos os tenants que possuem equipamentos e
não duplica memberships existentes.

## Arquivos relevantes

- `EMPREST.AI-Back/prisma/schema.prisma`: define `Tenant`, `Membership` e
  `Equipment`, incluindo o relacionamento indireto.
- `EMPREST.AI-Back/prisma/migrations/20261001115558_initial_database_schema/migration.sql`:
  define as tabelas, constraints e policies de memberships e equipamentos.
- `EMPREST.AI-Back/prisma/seed.ts`: demonstra a associação de usuário a tenant
  usando `membership.upsert`.
- `EMPREST.AI-Back/src/modules/equipment/equipment.service.ts`: utiliza
  memberships para filtrar equipamentos por usuário.
- `rules/spec-flow.md`: exige spec, plano, tasks, aprovação e verificação para
  mudanças de comportamento.
- `rules/migration.md`: exige aprovação prévia do DDL para mudanças de schema.
- `rules/check.md`: define testes, build e verificação final.

## Comandos e verificações

- `git status --short --branch`: branch `master`; havia alteração pré-existente
  em `EMPREST.AI`, fora do escopo desta tarefa.
- Leitura de `prisma/schema.prisma`, migrations, seed, regras e
  `package.json`: concluída.
- Testes, build e validações do backend: não executados, pois nenhuma alteração
  foi autorizada ou aplicada.

## Bloqueios e decisões pendentes

- Aprovação explícita para executar a associação proposta no banco.
- Confirmar se o papel do usuário deve ser `collaborator` ou `operations`;
  a proposta usa `collaborator`, seguindo o padrão do seed existente.
- Não foi verificado se o UID existe no `auth.users`, pois a operação ainda não
  foi executada.

## Próximos passos

1. Obter a aprovação do usuário para a operação de associação e para o papel
   `collaborator`.
2. Definir o mecanismo autorizado de execução no banco; não executar SQL
   diretamente em banco remoto sem confirmação e sem seguir o fluxo de
   migration quando aplicável.
3. Executar a operação idempotente no ambiente autorizado.
4. Verificar a quantidade de tenants/equipamentos associados e validar o
   backend com os comandos definidos em `EMPREST.AI-Back/package.json`.
5. Atualizar a spec, plano e tasks do SDD caso a alteração seja implementada.
