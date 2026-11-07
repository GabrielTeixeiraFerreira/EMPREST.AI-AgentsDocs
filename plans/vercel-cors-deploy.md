# Plano: Conexão entre frontend e API publicada

## Arquivos e módulos

- `EMPREST.AI/.env.example`: documentar URL pública de produção.
- `EMPREST.AI/README.md`: documentar configuração na Vercel.
- `EMPREST.AI-Back/.env.example`: documentar origem pública permitida.
- `EMPREST.AI-Back/README.md`: documentar configuração na Vercel.
- `docs/specs/vercel-cors-deploy.md`: rastreabilidade da mudança.

## Abordagem técnica

Usar as variáveis já existentes: `VITE_API_URL` no build público e
`CORS_ORIGIN` no runtime da API. Normalizar `VITE_API_URL` para garantir o
prefixo `/v1` exatamente uma vez. Manter a função de CORS com lista explícita
de origens, sem wildcard e sem fallback permissivo em produção.

## Ordem de execução

1. Atualizar exemplos e instruções de deploy.
2. Validar frontend e backend.
3. Cadastrar as variáveis nos projetos Vercel e realizar novos deploys.
4. Conferir o preflight CORS da URL pública.

## Impacto

- Dados: nenhum.
- Contratos: nenhum.
- Segurança: mantém allowlist explícita.
- Operação: exige atualização das variáveis de ambiente na Vercel.

## Rastreabilidade

- Etapa 1: CA-01 e CA-02.
- Etapa 2: CA-04.
- Etapas 3-4: CA-03 e CA-04.
