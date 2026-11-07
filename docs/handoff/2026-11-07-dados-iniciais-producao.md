# Handoff: dados iniciais de produção

- Data: 2026-11-07
- Escopo: backend
- Status: bloqueada

## Objetivo

Preparar o backend para popular os 12 equipamentos reais, associar as imagens
já existentes no bucket Supabase, criar o usuário de teste de forma idempotente
e definir os estados corretos dos equipamentos.

## O que foi feito

- Confirmado o escopo backend com o usuário.
- Atualizado o seed para os equipamentos `TI-0001` a `TI-0012`.
- Definidos `TI-0004`, `TI-0007` e `TI-0010` como `maintenance`; os demais
  ficam `available`.
- Definidos os caminhos de imagem `ti-0001.jpg` a `ti-0012.jpg`.
- Implementado upsert idempotente do tenant, equipamentos e membership.
- Implementada criação/atualização do usuário
  `marcelo.grilo@institutojef.org.br` via Supabase Auth Admin API.
- Alterado o seed para usar `SUPABASE_SECRET_KEY`, sem registrar o valor da
  chave no repositório ou neste handoff.
- Criada a migration estrutural
  `20261106110000_add_equipment_image_path`.
- Criada a migration de dados
  `20261106112000_seed_initial_equipment_data`, para aplicação manual pelo
  usuário.
- Corrigido o import do `supertest` no teste E2E.
- Criados os artefatos SDD:
  `docs/specs/dados-iniciais-producao.md`,
  `plans/dados-iniciais-producao.md` e
  `tasks/dados-iniciais-producao.md`.
- Nenhum commit foi criado.

## Estado atual

O código e a migration estão preparados. O usuário informou que aplicará a
migration manualmente. A execução real do seed ainda não foi validada nesta
sessão porque o ambiente disponível aponta para banco remoto e a regra do
projeto impede testar contra o banco remoto pelo agente.

A task T-04 está concluída. A task T-05 permanece bloqueada até executar o
seed duas vezes em ambiente autorizado e conferir contagens, estados, imagens,
usuário e zero empréstimos.

## Arquivos relevantes

- `EMPREST.AI-Back/prisma/seed.ts`: seed idempotente, usuário de teste e
  estados dos equipamentos.
- `EMPREST.AI-Back/prisma/schema.prisma`: modelo `Equipment` com
  `imagePath`.
- `EMPREST.AI-Back/prisma/migrations/20261106110000_add_equipment_image_path/migration.sql`:
  adiciona `equipment.image_path`.
- `EMPREST.AI-Back/prisma/migrations/20261106112000_seed_initial_equipment_data/migration.sql`:
  atualiza estados e caminhos das imagens.
- `EMPREST.AI-Back/.env.example`: documenta `SUPABASE_SECRET_KEY`.
- `docs/specs/dados-iniciais-producao.md`: especificação e critérios CA-01 a
  CA-06.
- `plans/dados-iniciais-producao.md`: plano técnico.
- `tasks/dados-iniciais-producao.md`: rastreabilidade das tasks e bloqueio da
  execução real.

## Comandos e verificações

- `npm run db:validate` em `EMPREST.AI-Back`: passou; houve apenas aviso de
  configuração Prisma depreciada no `package.json`.
- `npm test` em `EMPREST.AI-Back`: passou; 1 suíte e 1 teste.
- `npm run build` em `EMPREST.AI-Back`: passou.
- `git diff --check`: passou.
- A variável `SUPABASE_SECRET_KEY` foi confirmada no `.env` sem exibir seu
  valor.
- `npx prisma migrate deploy`: não executado pelo agente nesta etapa.
- `npm run db:seed`: não executado pelo agente nesta etapa.

## Bloqueios e decisões pendentes

- Aplicar manualmente as migrations no ambiente autorizado.
- Executar o seed com `SUPABASE_SECRET_KEY` configurada.
- Confirmar que os 12 objetos do bucket correspondem aos caminhos
  `ti-0001.jpg` até `ti-0012.jpg`.
- Validar que o seed repetido não duplica equipamentos, usuário, membership ou
  empréstimos.
- Não registrar a senha do usuário, tokens ou chaves neste handoff.
- O status raiz observado contém alterações fora desta sessão em `EMPREST.AI`,
  `EMPREST.AI-Back`, `skills/imagens-equipamentos/SKILL.md` e um arquivo
  `ENTREGA.md`; não foram revertidas.

## Próximos passos

1. Aplicar `npx prisma migrate deploy` no ambiente autorizado.
2. Executar `npm run db:seed` com a chave secreta disponível somente no
   ambiente.
3. Executar o seed novamente.
4. Conferir no banco as quantidades, estados, caminhos, membership e ausência de
   empréstimos.
5. Atualizar T-05 para concluída somente após essa verificação.
6. Revisar o diff e criar commit manualmente, se desejado pelo usuário.
