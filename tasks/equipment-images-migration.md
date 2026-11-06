# Tasks: referências das imagens de equipamentos

- T-01 — [concluída] Adicionar `imagePath` opcional ao modelo `Equipment`.
- T-02 — [concluída] Fazer o seed inserir/atualizar os 12 equipamentos do
  manifesto de forma idempotente.
- T-03 — [concluída] Gerar e aplicar a migration Prisma no banco autorizado.
  A validação local via `migrate dev` continua limitada pelo schema `auth`
  ausente no shadow database.
- T-04 — [bloqueada] Executar validação, testes, build e conferir o status do
  repositório; o schema/build passaram, mas o teste e2e falhou em `supertest`.
