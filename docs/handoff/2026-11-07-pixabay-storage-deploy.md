# Handoff: Preparação de imagens Pixabay e deploy no Storage

- Data: 2026-11-07
- Escopo: ambos
- Status: bloqueada

## Objetivo

Preparar imagens únicas de equipamentos usando Pixabay, manter créditos,
garantir dimensões `800x600` e disponibilizá-las no bucket público
`equipamentos` do Supabase Storage por um fluxo seguro de deploy.

## O que foi feito

- Criada a skill [skills/imagens-equipamentos/SKILL.md](../../skills/imagens-equipamentos/SKILL.md)
  com regras de busca no Pixabay, deduplicação, inspeção visual, créditos e
  dimensão obrigatória.
- Preparadas 12 imagens para os patrimônios `TI-0001` a `TI-0012` em
  [EMPREST.AI/public/images/equipamentos](../../EMPREST.AI/public/images/equipamentos).
- Criado o manifesto de créditos
  [manifesto-creditos.json](../../EMPREST.AI/public/images/equipamentos/manifesto-creditos.json).
- Validado que existem 12 imagens, 12 créditos, dimensões `800x600`, IDs
  Pixabay únicos e hashes únicos.
- Atualizado [rules/migration.md](../../rules/migration.md) para distinguir
  assets do schema Prisma e permitir publicação por pipeline CI/CD protegido.
- Atualizado [EMPREST.AI-Back/.env.example](../../EMPREST.AI-Back/.env.example)
  com bucket `equipamentos`, endpoint S3 e região `sa-east-1`, sem valores de
  credenciais.
- O `.env` local do backend recebeu apenas configuração não secreta de Storage;
  os valores secretos não foram registrados neste handoff.

## Estado atual

As imagens e o manifesto estão preparados localmente, mas o upload no bucket
remoto não foi executado. O agente não acessou nem alterou o Supabase remoto.
O deploy deve ser feito por pipeline CI/CD usando secrets protegidos.

O frontend atualmente observado na sessão era uma tela de login; não foi
confirmada uma tela de fotos com o texto obrigatório “Fotos: Pixabay”.

O working tree observado no momento do handoff contém alterações em
`EMPREST.AI`, `EMPREST.AI-Back`, `skills/imagens-equipamentos/SKILL.md` e um
arquivo não rastreado `ENTREGA.md`. Essas alterações não foram revertidas.

## Arquivos relevantes

- [skills/imagens-equipamentos/SKILL.md](../../skills/imagens-equipamentos/SKILL.md):
  workflow para preparar imagens Pixabay.
- [EMPREST.AI/public/images/equipamentos](../../EMPREST.AI/public/images/equipamentos):
  imagens preparadas para os 12 patrimônios.
- [manifesto-creditos.json](../../EMPREST.AI/public/images/equipamentos/manifesto-creditos.json):
  IDs, autores, URLs, transformações e hashes.
- [rules/migration.md](../../rules/migration.md):
  regra de migration e publicação segura de assets.
- [EMPREST.AI-Back/.env.example](../../EMPREST.AI-Back/.env.example):
  nomes de configuração não secreta e nomes dos secrets esperados.
- [rules/restrictions.md](../../rules/restrictions.md):
  restrições de acesso remoto, deploy e credenciais.

## Comandos e verificações

- Consulta da API Pixabay: candidatos obtidos sem imprimir a chave.
- Validação local das imagens: `images=12`, `credits=12`,
  `invalid_dimensions=0`.
- Validação de unicidade: `uniqueHashes=12`, `uniqueIds=12`.
- `npm run build` em `EMPREST.AI-Back`: passou.
- `npm run db:validate` em `EMPREST.AI-Back`: passou, com aviso de configuração
  Prisma no `package.json` depreciada para Prisma 7.
- `git diff --check -- rules/migration.md`: passou.
- O primeiro build e a primeira validação executados na raiz falharam porque a
  raiz não possui `package.json`; os comandos foram repetidos no backend e
  passaram.

## Bloqueios e decisões pendentes

- O deploy remoto ainda está pendente; deve ser executado por CI/CD com
  secrets protegidos.
- As credenciais de Storage foram expostas na conversa e precisam ser
  revogadas/rotacionadas antes de qualquer upload.
- Confirmar no provedor de CI/CD os secrets
  `SUPABASE_STORAGE_ACCESS_KEY_ID` e
  `SUPABASE_STORAGE_SECRET_ACCESS_KEY`, sem colocá-los no Git.
- Confirmar a existência do bucket público `equipamentos` e configurar a
  verificação pós-upload.
- Confirmar ou implementar a tela que exibe “Fotos: Pixabay” com link para
  `https://pixabay.com/pt/`.

## Próximos passos

1. Revogar e rotacionar as credenciais expostas.
2. Criar um workflow de CI/CD que valide dimensões, manifesto, IDs, hashes e
   ausência de segredos antes do upload.
3. Configurar os novos secrets no provedor de deploy.
4. Publicar as imagens no bucket `equipamentos` e verificar as URLs públicas.
5. Validar a tela de fotos e o crédito “Fotos: Pixabay”.
6. Revisar as alterações existentes no working tree e criar commit apenas com
   autorização do usuário.
