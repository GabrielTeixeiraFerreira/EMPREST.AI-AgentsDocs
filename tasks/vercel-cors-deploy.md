# Tasks: Conexão entre frontend e API publicada

## T-01 — Documentar variáveis de produção

- Descrição: atualizar exemplos e READMEs com a URL da API e a origem pública
  do frontend.
- Arquivos: `.env.example` e `README.md` dos dois projetos.
- Critérios: CA-01, CA-02.
- Verificação: inspeção dos valores e `git diff --check`.
- Estado: concluída.

## T-02 — Validar builds e política de origem

- Descrição: executar build/check do frontend, build/testes do backend e
  confirmar que o CORS permanece em allowlist.
- Arquivos: frontend e backend.
- Critérios: CA-03, CA-04.
- Verificação: scripts de build/teste dos dois projetos.
- Estado: concluída.

## T-03 — Configurar os deployments

- Descrição: cadastrar as variáveis nos projetos Vercel, publicar novamente e
  testar o login pelo domínio público.
- Arquivos: configurações dos deployments.
- Critérios: CA-01, CA-02, CA-03.
- Verificação: preflight e login no navegador.
- Estado: bloqueada — requer acesso aos painéis Vercel.
