# Spec: dados iniciais de produção

## Contexto e problema

O ambiente de produção precisa iniciar com os equipamentos reais da prática e
com uma conta de professor para validação do fluxo de login. A carga deve ser
reexecutável sem duplicar registros.

## Objetivo

Popular o backend com os 12 equipamentos, seus estados e caminhos de imagem,
além do usuário `marcelo.grilo@institutojef.org.br` sem empréstimos.

## Fora de escopo

- Alterações no frontend.
- Tela de catálogo.
- Cadastro de equipamento pela interface.
- Importação de planilha.
- Criação de empréstimos.
- Upload automático para o Storage.

## Usuários e sistemas envolvidos

- Professor de teste: `marcelo.grilo@institutojef.org.br`.
- Seed Prisma do backend.
- Bucket público do Supabase Storage com as imagens já preparadas.

## Comportamento esperado

- Existem exatamente 12 equipamentos identificados de `TI-0001` a `TI-0012`.
- As categorias são 4 notebooks, 3 monitores, 3 cabos e 2 câmeras.
- `TI-0004`, `TI-0007` e `TI-0010` ficam em manutenção; os demais ficam
  disponíveis.
- Cada equipamento possui patrimônio único, nome, categoria, situação e um
  caminho distinto para uma imagem já publicada no bucket público do Supabase;
  os 12 objetos existentes são `800x600`.
- O usuário de teste existe com a senha padrão da turma escrita no arquivo de
  seed e começa sem empréstimos.
- A execução repetida atualiza os registros previstos sem criar duplicatas.

## Critérios de aceitação

- CA-01: O seed cria ou atualiza exatamente os equipamentos `TI-0001` a
  `TI-0012`, com as quatro categorias e os estados previstos, incluindo
  manutenção em `TI-0004`, `TI-0007` e `TI-0010`.
- CA-02: Cada equipamento possui um caminho distinto para uma imagem do bucket
  público do Supabase, e os 12 objetos existentes são `800x600`.
- CA-03: O seed cria ou atualiza o usuário de teste com a senha padrão
  `Inovacao2026`, sem criar empréstimos.
- CA-04: Executar o seed mais de uma vez não aumenta a quantidade de
  equipamentos, usuários ou empréstimos.
- CA-05: O manifesto de imagens preserva créditos, IDs, hashes e não contém
  segredos.
- CA-06: O schema, testes e build do backend passam.

## Riscos, dependências e dúvidas abertas

- O schema atual não possui uma tabela de usuários: a conta é administrada pelo
  Supabase Auth. A criação idempotente exige `SUPABASE_SERVICE_ROLE_KEY`
  disponível somente no ambiente de execução do seed.
- Como as imagens já estão no bucket, a etapa de download/redimensionamento não
  será repetida.
- O escopo confirmado é backend; o seed somente referencia os caminhos do
  bucket e não faz upload.
