# Plano: primeira tela de login

## Arquivos

- `index.html`: estrutura semântica da tela.
- `styles.css`: tokens, layout, responsividade e estados visuais.
- `script.js`: interações locais do formulário e contexto de Operações.
- `docs/specs/login.md`: requisitos e critérios de aceitação.
- `tasks/login.md`: tarefas e verificações.

## Abordagem

1. Recriar a composição estática da seção de login conforme `layout.md`.
2. Usar CSS nativo com os tokens de cor, tipografia, espaçamento e raios definidos no layout.
3. Implementar apenas feedback local de formulário, sem inventar autenticação ou API.
4. Manter os arquivos independentes para facilitar a migração futura para React/Vite.

## Verificação

- Servir `EMPREST.AI` com um servidor HTTP local.
- Confirmar que `index.html`, `styles.css` e `script.js` respondem sem erro.
- Fazer checagem visual em viewport desktop e mobile quando houver ferramenta de navegador disponível.
- Executar `git diff --check` nos repositórios afetados.
- Como não há `package.json` nem scaffold no frontend, não há script de teste ou build frontend disponível nesta etapa.

## Rastreabilidade

- CA-01, CA-02 e CA-06: `index.html` e `styles.css`.
- CA-03, CA-04, CA-05 e CA-07: `index.html` e `script.js`.
