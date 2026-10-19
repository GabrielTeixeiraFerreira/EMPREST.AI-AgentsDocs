# Plano: API do backend com endpoint raiz

## Arquivos e módulos

- `EMPREST.AI-Back/package.json`: adição de dependências NestJS, scripts de build, start e test.
- `EMPREST.AI-Back/tsconfig.json` e `tsconfig.build.json`: configuração do compilador TypeScript com suporte a decorators.
- `EMPREST.AI-Back/src/main.ts`: ponto de entrada e bootstrap da aplicação NestJS com Express.
- `EMPREST.AI-Back/src/app.module.ts`: módulo raiz da aplicação.
- `EMPREST.AI-Back/src/modules/system/system.module.ts`: módulo do domínio de sistema/saúde.
- `EMPREST.AI-Back/src/modules/system/system.controller.ts`: controller HTTP que responde a `GET /`.
- `EMPREST.AI-Back/src/modules/system/system.service.ts`: service responsável pela resposta.
- `EMPREST.AI-Back/test/system.e2e-spec.ts`: teste de integração da API usando supertest.
- `EMPREST.AI-Back/test/jest-e2e.json` ou configuração no `package.json`: configuração do Jest para testes e2e.
- `docs/specs/api-root.md`: requisitos e critérios de aceitação.
- `tasks/api-root.md`: lista de tarefas e status.

## Abordagem técnica

1. Manter a arquitetura orientada a domínio conforme ADR-001 (`src/modules/<dominio>/`), criando o domínio `system` para o endpoint raiz / healthcheck.
2. Aplicar princípios SOLID:
   - SRP: `SystemController` trata entrada e saída HTTP; `SystemService` provê a lógica da mensagem;
   - DIP: `SystemController` recebe `SystemService` via injeção de dependência;
   - Clean Architecture: separação nítida de responsabilidades e desacoplamento.
3. Não adicionar Prisma nem alterar schema nesta etapa, focando exclusivamente na inicialização da API HTTP e no endpoint raiz.

## Ordem de execução

1. Configurar `package.json`, `tsconfig.json` e `tsconfig.build.json` e instalar dependências.
2. Implementar `SystemService`, `SystemController`, `SystemModule`, `AppModule` e `main.ts`.
3. Criar os testes com Jest + supertest em `test/system.e2e-spec.ts`.
4. Executar `npm test` e `npm run build`.
5. Realizar smoke test manual (subir a aplicação em porta local e fazer requisição HTTP para verificar retorno 200 com JSON "Hello World").
6. Executar validação de check (`git status --short`, etc.) e atualizar o status das tasks.

## Estratégia de testes e build

- Build: `npm run build` gerando `dist/` sem erros de tipagem TypeScript;
- Testes: `npm test` executando os testes supertest cobrindo `GET /` e confirmando status 200 e payload JSON;
- Smoke test: iniciar o servidor e efetuar chamada HTTP GET em `http://localhost:3000/`.

## Impacto em dados, contratos, segurança e operação

- Sem impacto no banco de dados (nenhuma migration envolvida);
- Sem impacto em autenticação (endpoint raiz público para health check/conectividade);
- Contrato da raiz: `GET /` -> HTTP 200 `{ "message": "Hello World" }`.

## Rastreabilidade com CA-xx

- CA-01 (subida da API): `src/main.ts`, `src/app.module.ts`.
- CA-02 (status HTTP 200) e CA-03 (JSON com Hello World): `src/modules/system/system.controller.ts`, `src/modules/system/system.service.ts`.
- CA-04 (compilação sem erro): `EMPREST.AI-Back/package.json`, `tsconfig.json`, `tsconfig.build.json`.
- CA-05 (teste supertest verde): `test/system.e2e-spec.ts`, `npm test`.

