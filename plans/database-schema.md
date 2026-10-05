# Plano: schema inicial do banco

## Arquivos

- `EMPREST.AI-Back/package.json`
- `EMPREST.AI-Back/prisma/schema.prisma`
- `EMPREST.AI-Back/prisma/seed.ts`
- migration Prisma gerada pela CLI
- `EMPREST.AI/docs/specs/database-schema.md`

## Ordem

1. Validar o schema Prisma.
2. Gerar uma migration inicial pela CLI.
3. Adicionar índices parciais, RLS e policies na mesma migration.
4. Gerar o client e executar o seed.
5. Resetar banco local, validar migration e conferir Git.

## Segurança e dados

Usuários são referências UUID para `auth.users`, sem duplicar credenciais no
schema da aplicação. Policies restringem registros ao tenant presente na
membership do usuário autenticado. Não existem dados anteriores neste
backend, então não há backfill.

## Verificação

`npm run db:validate`, `npm run db:generate`, `npm run db:reset` e `git status
--short`.
