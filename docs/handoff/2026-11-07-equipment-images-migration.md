# Handoff: migration e upload das imagens de equipamentos

- Data: 2026-11-07
- Escopo: backend
- Status: bloqueada

## Objetivo

Associar as 12 imagens de equipamentos ao modelo `equipment`, criar e aplicar
a migration Prisma para `image_path` e publicar os assets no bucket público do
Supabase Storage.

## O que foi feito

- Adicionado `imagePath String? @map("image_path")` ao modelo `Equipment`.
- Atualizado o seed para criar ou atualizar os equipamentos `TI-0001` a
  `TI-0012` com nome, categoria, situação e caminho da imagem.
- Criada a migration
  `20261106110000_add_equipment_image_path`.
- A migration foi aplicada com `npx prisma migrate deploy` no banco autorizado.
- O status do Prisma confirmou `Database schema is up to date!`.
- Os 12 arquivos foram enviados ao bucket `equipamentos` em
  `images/equipamentos/ti-XXXX.jpg`.
- Os 12 objetos foram verificados por `HeadObject`.
- As 12 URLs públicas retornaram HTTP 200.
- As imagens foram validadas localmente: 12 arquivos, dimensões 800x600,
  hashes e IDs Pixabay sem duplicidade e hashes compatíveis com o manifesto.
- Nenhum commit foi criado.

## Estado atual

As alterações de schema, seed e migration estão implementadas e a migration
está aplicada no banco autorizado. Os assets estão publicados e acessíveis
publicamente no Storage.

A validação completa da tarefa permanece bloqueada: `npm run db:validate` e
`npm run build` passaram, mas `npm test` falhou em
`test/system.e2e-spec.ts` com `TypeError: supertest_1.default is not a
function`. A validação local por `prisma migrate dev`/shadow database também
falhou porque a migration inicial referencia `auth.users` e o shadow database
não possui o schema `auth`.

O working tree contém alterações não relacionadas observadas no encerramento:
`EMPREST.AI`, `EMPREST.AI-Back`, `skills/imagens-equipamentos/SKILL.md` e
`ENTREGA.md`. Não foram revertidas.

## Arquivos relevantes

- `EMPREST.AI-Back/prisma/schema.prisma`: define `Equipment.imagePath`.
- `EMPREST.AI-Back/prisma/seed.ts`: seed idempotente dos 12 equipamentos.
- `EMPREST.AI-Back/prisma/migrations/20261106110000_add_equipment_image_path/migration.sql`:
  adiciona `equipment.image_path`.
- `EMPREST.AI/public/images/equipamentos/manifesto-creditos.json`: manifesto
  e créditos dos assets.
- `docs/specs/equipment-images-migration.md`: spec da alteração.
- `plans/equipment-images-migration.md`: plano da alteração.
- `tasks/equipment-images-migration.md`: tasks e bloqueio de validação.
- `rules/migration.md`: regras de migration e publicação de assets.

## Comandos e verificações

- `npx prisma migrate dev --name add_equipment_image_path`: bloqueado por
  `schema "auth" does not exist` no shadow database.
- `npx prisma migrate deploy`: passou; migration aplicada com sucesso.
- `npx prisma migrate status`: passou; schema atualizado.
- `npm run db:validate`: passou.
- `npm run db:generate`: passou.
- `npm run build`: passou.
- `npm test -- --runInBand`: falhou em 1 teste e 1 suíte por importação do
  `supertest`.
- Validação de assets: passou; 12 imagens, 800x600, hashes e IDs únicos.
- Verificação do Storage: passou; 12 uploads confirmados e 12 URLs HTTP 200.

## Bloqueios e decisões pendentes

- Corrigir ou validar a configuração do `supertest` no teste e2e antes de
  marcar a task T-04 como concluída.
- Disponibilizar um shadow database local com o schema `auth`, ou documentar
  uma estratégia equivalente, caso seja exigida a validação local completa.
- Não executar reset no banco final.
- Não expor nem versionar as credenciais S3 configuradas no `.env`.

## Próximos passos

1. Investigar a falha de importação do `supertest` em
   `EMPREST.AI-Back/test/system.e2e-spec.ts`.
2. Executar novamente `npm test` após a correção aprovada.
3. Atualizar `tasks/equipment-images-migration.md` somente quando T-04 tiver
   todas as validações aprovadas.
4. Conferir o diff e criar commit manualmente, se desejado pelo usuário.
