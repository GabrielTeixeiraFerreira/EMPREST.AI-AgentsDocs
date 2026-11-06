# Plano: login e catálogo inicial

## Arquivos

- `docs/specs/login-catalogo.md`, `plans/login-catalogo.md`,
  `tasks/login-catalogo.md`: rastreabilidade SDD.
- `EMPREST.AI-Back/prisma/migrations/...`: carga idempotente do catálogo.
- `EMPREST.AI-Back/prisma/seed.ts`: usuário de teste e membership.
- `EMPREST.AI-Back/src/modules/auth/*`: login e validação de sessão.
- `EMPREST.AI-Back/src/modules/equipment/*`: catálogo protegido.
- `EMPREST.AI-Back/src/main.ts`: prefixo, CORS e JSON.
- `EMPREST.AI/index.html`, `script.js`, `styles.css`: tela pós-login.

## Abordagem

Usar Supabase Auth como provedor de credenciais e sessão, sem duplicar senha
no banco local. O backend valida a sessão no endpoint `/auth/v1/user` e usa
Prisma para consultar equipamentos por tenant. O frontend permanece estático,
com uma transição de formulário para catálogo e armazenamento da sessão apenas
durante a aba.

## Ordem

1. Criar migration e atualizar seed.
2. Implementar módulos, guards e testes de API.
3. Integrar login e catálogo no frontend.
4. Rodar reset, testes, builds e checks finais.

## Testes e impacto

Validar `prisma validate`, `prisma migrate reset`, `npm test`, `npm run build`
e `node --check script.js`. A migration é idempotente e não remove
empréstimos existentes.

## Rastreabilidade

- CA-01/CA-02: migration e seed.
- CA-03/CA-04: módulos de autenticação e equipamentos.
- CA-05: frontend.
- CA-06: suíte e verificações finais.
