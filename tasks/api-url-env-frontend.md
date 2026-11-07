# Tasks: URL da API por variável de ambiente no frontend

## T-01 — Configurar build do frontend

- Descrição: adicionar Vite, scripts de desenvolvimento/build e configuração
  da entrada HTML.
- Arquivos: `EMPREST.AI/package.json`, `EMPREST.AI/vite.config.js`,
  `EMPREST.AI/index.html`.
- Critérios: CA-01, CA-04.
- Verificação: `npm run build`.
- Estado: concluída.

## T-02 — Consumir a URL pública do ambiente

- Descrição: trocar a leitura de `window.EMPRESAI_API_URL` por
  `import.meta.env.VITE_API_URL`, preservando o fallback local.
- Arquivos: `EMPREST.AI/script.js`.
- Critérios: CA-02, CA-03.
- Verificação: `node --check script.js`.
- Estado: concluída.

## T-03 — Documentar configuração e validar

- Descrição: atualizar instruções de execução, instalar dependências do
  manifesto e executar as verificações finais do frontend.
- Arquivos: `EMPREST.AI/README.md`, `EMPREST.AI/package-lock.json`.
- Critérios: CA-04.
- Verificação: `npm run build` e `node --check script.js`.
- Estado: concluída.
