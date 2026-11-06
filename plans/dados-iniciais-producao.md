# Plano: dados iniciais de produção

## Arquivos ou módulos

- Inspecionar o modelo atual de autenticação e adaptar o seed ao contrato
  existente.
- Alterar `EMPREST.AI-Back/prisma/seed.ts`.
- Confirmar os objetos já existentes no bucket público do Supabase e preservar
  o manifesto de créditos, sem fazer upload ou adicionar `sharp`.

## Abordagem técnica

Usar `upsert` com chaves naturais existentes para tenant e equipamento. Usar a
Supabase Auth Admin API para localizar ou criar o usuário pelo e-mail, sem
persistir a chave de serviço. Preservar empréstimos existentes e não criar
nenhum no seed. Referenciar os caminhos já publicados no bucket.

## Ordem de execução

1. Verificar o modelo de usuário e o mecanismo de senha do backend.
2. Confirmar os 12 caminhos distintos dos objetos já publicados.
3. Atualizar o seed e adicionar a criação idempotente do usuário via Supabase
   Auth Admin API.
4. Executar o seed repetidamente e verificar contagens e estados.
5. Executar validação, testes, build e os checks finais.

## Testes e build

- `npm run db:validate`
- `npm run db:seed` duas vezes em banco local autorizado
- `npm test`
- `npm run build`
- `git status --short`

## Impacto

Não há nova migration prevista. A alteração de dados é idempotente e não cria
empréstimos. A senha aparece somente no arquivo de seed conforme solicitado;
nenhuma chave de API ou credencial de Storage será adicionada aos arquivos
versionáveis.

## Rastreabilidade

- CA-01 e CA-04: seed idempotente e verificação de contagens.
- CA-02 e CA-05: caminhos do bucket e manifesto de créditos existente.
- CA-03: upsert do usuário e ausência de empréstimos.
- CA-06: validação, testes e build.
