# Mudança de schema

> leitor: agente

## Quando

Qualquer tarefa que precise criar, alterar ou remover tabela, coluna,
índice, constraint, política de acesso ou seed que afete o contrato do
banco — inclusive quando a mudança parecer trivial.

## Procedimento

1. Antes de gerar qualquer coisa: escreva o DDL ou a alteração
   pretendida na resposta e PARE. Eu aprovo ou corrijo.
2. Gere a migration pela CLI do projeto: `npx prisma migrate dev --name
<nome>`. Uma migration por tarefa. Se a mudança exigir SQL bruto para
   RLS ou policies, mantenha esse SQL dentro da mesma migration.
3. Toda tabela nova nasce com Row Level Security habilitado e pelo
   menos uma policy explícita na mesma migration. Tabela sem policy não
   entra no repositório.
4. Se a tabela já tem dado, diga o que acontece com as linhas
   existentes. Coluna obrigatória nova precisa de default ou de um passo
   de preenchimento.
5. Aplique localmente com `npx prisma migrate reset` e rode
   `<COMANDO-TESTES>`.

## Verificação

`npx prisma migrate reset` numa máquina limpa aplica todas as migrations
sem erro e sem nenhum passo manual.

## Não faça

- Não altere schema pelo Studio nem por SQL avulso fora do fluxo de
  migration. O que não está em migration não existe.
- Não edite migration que já foi para o repositório remoto. Escreva a
  próxima.
- Não altere o projeto Supabase de database remoto manualmente pelo agente. O deploy de
  assets/imagens pode ocorrer manualmente usando a migration.
  secrets protegidos do provedor.
- Não use `supabase ...` como dono do schema do projeto; o Prisma Migrate
  é o dono único do schema conforme o ADR.

## Assets no Supabase Storage

Imagens, arquivos estáticos e seus créditos no Supabase Storage não são
mudanças de schema e não geram migration Prisma. A publicação desses arquivos
deve ocorrer em uma etapa separada de deploy, usando um bucket já provisionado.

- O bucket público usado pelo projeto é configurado por
  `SUPABASE_STORAGE_BUCKET`.
- O endpoint S3, a região e o bucket podem ser versionados somente como
  configuração não secreta.
- `SUPABASE_STORAGE_ACCESS_KEY_ID` e
  `SUPABASE_STORAGE_SECRET_ACCESS_KEY` devem existir apenas no secret manager
  do ambiente de deploy. Nunca devem aparecer em código, `.env.example`,
  manifesto de créditos, logs ou Git.
- A etapa de publicação deve validar que cada imagem tem `800x600`, possui
  crédito no manifesto, não repete ID/hash e usa o caminho do patrimônio.
- O agente deve preparar e validar os arquivos localmente e configurar o
  pipeline, mas não deve ler, imprimir ou armazenar credenciais. O pipeline
  pode publicar os assets automaticamente no bucket somente após as
  validações passarem.
- O pipeline deve usar `SUPABASE_STORAGE_ACCESS_KEY_ID` e
  `SUPABASE_STORAGE_SECRET_ACCESS_KEY` exclusivamente como secrets protegidos,
  nunca como valores versionados.
- O pipeline deve falhar se houver imagem fora de `800x600`, crédito ausente,
  `pixabayId` ou hash repetido, segredo em arquivo versionável ou bucket
  diferente de `SUPABASE_STORAGE_BUCKET`.
- Após o upload, o pipeline deve verificar a existência dos arquivos no
  bucket público e registrar o resultado no log do job, sem expor secrets.
