# Spec: Conexão entre frontend e API publicada

## Contexto e problema

O frontend publicado em `https://emprest-ai.vercel.app` continua usando a
URL local da API e a API publicada não tem essa origem configurada no CORS.
Consequentemente, o login falha no navegador com erro de CORS.

## Objetivo

Documentar e preparar a configuração de produção para o frontend chamar a API
publicada em `https://emprestaiback.vercel.app/v1` e para o backend aceitar a
origem do frontend publicado.

## Fora de escopo

- credenciais, secrets ou variáveis privadas;
- alterações no contrato das rotas;
- alteração da política de CORS para aceitar qualquer origem.

## Sistemas envolvidos

- Frontend Vercel: `https://emprest-ai.vercel.app`;
- Backend Vercel: `https://emprestaiback.vercel.app`.

## Comportamento esperado

- O build de produção do frontend usa
  `https://emprestaiback.vercel.app/v1` após normalizar a variável pública,
  mesmo que ela tenha sido cadastrada sem o sufixo `/v1`.
- O backend permite a origem `https://emprest-ai.vercel.app` por
  `CORS_ORIGIN`.
- As origens locais continuam documentadas para desenvolvimento.
- Nenhum segredo é incluído nos arquivos versionados ou no bundle.

## Critérios de aceitação

- CA-01: exemplos e documentação do frontend orientam
  `VITE_API_URL=https://emprestaiback.vercel.app` para produção, e o frontend
  acrescenta `/v1` quando necessário.
- CA-02: exemplos e documentação do backend orientam
  `CORS_ORIGIN` com `https://emprest-ai.vercel.app`.
- CA-03: a política de CORS continua recusando origens não configuradas em
  produção.
- CA-04: build do frontend e build/testes do backend passam.

## Riscos, dependências e dúvidas abertas

- As variáveis precisam ser cadastradas nos painéis da Vercel e um novo
  deploy precisa ser feito; alterar arquivos versionados não altera variáveis
  já salvas nos projetos.
- A API publicada precisa estar acessível publicamente e com suas variáveis
  privadas configuradas.
- Não há dúvidas abertas.
