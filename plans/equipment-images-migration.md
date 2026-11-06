# Plano: referências das imagens de equipamentos

## Arquivos

- Alterar `EMPREST.AI-Back/prisma/schema.prisma`.
- Alterar `EMPREST.AI-Back/prisma/seed.ts`.
- Gerar uma migration Prisma para `equipment.image_path`.

## Execução

1. Adicionar o campo opcional no modelo Prisma.
2. Tornar o seed idempotente para os 12 itens do manifesto.
3. Gerar a migration com `npx prisma migrate dev`.
4. Validar schema, reset local, testes e build.

## Impacto

Não há remoção nem alteração de dados existentes: a nova coluna aceita
`NULL`. Os arquivos permanecem no frontend e não são enviados ao Storage.

## Verificação

CA-01 e CA-03 serão verificados pelo schema e migration; CA-02 pelo seed;
CA-04 pelo reset; CA-05 por `db:validate`, testes e build.
