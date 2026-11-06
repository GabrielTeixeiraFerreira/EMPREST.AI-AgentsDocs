# Spec: referências das imagens de equipamentos

## Contexto e objetivo

Os 12 assets preparados em `EMPREST.AI/public/images/equipamentos` precisam
ser associados aos equipamentos persistidos pelo backend por meio do
patrimônio.

## Fora de escopo

- upload dos arquivos para o Supabase Storage;
- alteração do frontend;
- criação de novas políticas ou tabelas.

## Comportamento esperado

O modelo `equipment` deve aceitar o caminho público opcional da imagem. O
seed deve criar ou atualizar os 12 equipamentos do manifesto de forma
idempotente, preservando os estados e caminhos correspondentes.

## Critérios de aceitação

- CA-01: `equipment.image_path` existe como coluna opcional.
- CA-02: O seed cria ou atualiza exatamente os equipamentos `TI-0001` a
  `TI-0012` com seus caminhos `/images/equipamentos/ti-XXXX.jpg`.
- CA-03: Registros existentes sem imagem permanecem válidos com `NULL`.
- CA-04: A migration aplica em banco local limpo sem intervenção manual.
- CA-05: O schema Prisma, build e testes do backend passam.

## Riscos e dependências

A aplicação local da migration depende de um PostgreSQL configurado pelas
variáveis de ambiente do backend. A publicação dos assets continua sendo uma
etapa separada.
