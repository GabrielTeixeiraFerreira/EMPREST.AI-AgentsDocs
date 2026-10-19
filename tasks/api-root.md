# Tasks: API do backend com endpoint raiz

- [x] T-01 | Configurar `package.json` (scripts e dependências) e tsconfig | `EMPREST.AI-Back/package.json`, `tsconfig.json`, `tsconfig.build.json` | CA-04 | `npm run build` sem erro | concluída
- [x] T-02 | Implementar bootstrap, módulo raiz e endpoint `GET /` | `src/main.ts`, `src/app.module.ts`, `src/modules/system/*` | CA-01, CA-02, CA-03 | `npm run start` + `GET /` retorna 200 e JSON | concluída
- [/] T-03 | Criar teste supertest de `GET /` | `test/system.e2e-spec.ts` | CA-05 | `npm test` verde | em andamento
- [ ] T-04 | Executar validação final da tarefa | backend | CA-01 a CA-05 | `npm test`, `npm run build`, smoke de `GET /`, `git status --short` | pendente
