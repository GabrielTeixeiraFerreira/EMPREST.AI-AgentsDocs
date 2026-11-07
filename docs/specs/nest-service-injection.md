# Spec: Injeção explícita dos serviços Nest

## Contexto e problema

O backend inicia, mas os controllers recebem seus serviços como `undefined`.
As requisições para login e para a rota raiz falham com
`Cannot read properties of undefined`.

## Objetivo

Garantir que `AuthController` e `SystemController` recebam suas dependências
quando a aplicação é iniciada pelo comando `start:dev`.

## Fora de escopo

- alterações no contrato da API;
- alterações no banco, migrations ou credenciais;
- mudanças no comportamento de autenticação.

## Sistemas envolvidos

- Backend `EMPREST.AI-Back/`;
- NestJS e o executor TypeScript usado no desenvolvimento.

## Comportamento esperado

- `POST /v1/auth/login` alcança `AuthService.login`.
- `GET /` alcança `SystemService.getRootMessage`, preservando a rota pública
  existente fora do prefixo global.
- A aplicação continua iniciando com `npm run start:dev`.
- Falhas reais do Supabase continuam sendo retornadas explicitamente, sem
  mascaramento.

## Critérios de aceitação

- CA-01: `AuthController` possui `AuthService` injetado e não falha por
  dependência indefinida.
- CA-02: `SystemController` possui `SystemService` injetado e `GET /` retorna
  a resposta existente.
- CA-03: os testes E2E e o build do backend passam.
- CA-04: o frontend não sofre alterações comportamentais.

## Riscos, dependências e dúvidas abertas

- A correção usa injeção explícita compatível com o executor atual, sem
  depender da emissão implícita de metadados de design.
- O login ainda depende das variáveis e do serviço Supabase configurados no
  ambiente.
- Não há dúvidas abertas.
