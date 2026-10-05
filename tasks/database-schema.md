# Tasks: schema inicial do banco

- [x] T-01 | Criar schema Prisma e seed idempotente | `EMPREST.AI-Back/prisma` | CA-01, CA-05 | `npm run db:validate` e `npm run db:generate` | concluída
- [x] T-02 | Gerar migration com constraints e índices | `EMPREST.AI-Back/prisma/migrations` | CA-02, CA-04 | `npx prisma migrate dev --name initial_database_schema --create-only` | concluída
- [x] T-03 | Adicionar RLS e policies por tenant | migration inicial | CA-03 | inspeção da migration e `npm run db:validate` | concluída
- [x] T-04 | Verificar aplicação no Supabase e escopo do diff | backend | CA-06 | `npx prisma migrate status`, `npm run db:seed` e `git status --short` | concluída no banco configurado
