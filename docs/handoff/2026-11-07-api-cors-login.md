# Handoff: Correção de login e conexão com a API publicada

- Data: 2026-11-07
- Escopo: ambos
- Status: bloqueada

## Objetivo

Corrigir a falha de login do frontend publicado em
`https://emprest-ai.vercel.app`, garantindo que a API seja chamada com o
prefixo `/v1` e que a origem publicada seja aceita pelo CORS do backend.

## O que foi feito

- O frontend passou a ler `VITE_API_URL` via `import.meta.env`.
- A URL da API é normalizada para adicionar `/v1` exatamente uma vez.
- A configuração documentada para produção é
  `https://emprestaiback.vercel.app`.
- O backend mantém uma allowlist explícita de origens via `CORS_ORIGIN`.
- A origem `https://emprest-ai.vercel.app` foi adicionada aos exemplos e à
  documentação de produção.
- A injeção explícita de `AuthService` e `SystemService` foi corrigida nos
  controllers Nest.
- Nenhum commit foi criado.

## Estado atual

O código local está preparado, mas o frontend publicado ainda precisa de novo
deploy. A imagem mais recente mostrava a chamada incorreta
`https://emprestaiback.vercel.app/auth/login`; o bundle local corrigido gera
`https://emprestaiback.vercel.app/v1/auth/login`.

O preflight público da API passou a responder com a origem autorizada durante
a validação. O login público ainda não foi validado após o redeploy do
frontend. A task T-03 permanece bloqueada por exigir acesso aos painéis da
Vercel.

## Arquivos relevantes

- `EMPREST.AI/script.js`: normaliza `VITE_API_URL` e monta as chamadas da API.
- `EMPREST.AI/.env.example`: documenta a URL pública do backend.
- `EMPREST.AI/README.md`: documenta a variável de produção e o redeploy.
- `EMPREST.AI-Back/src/main.ts`: implementa a allowlist de CORS.
- `EMPREST.AI-Back/.env.example`: inclui a origem pública do frontend.
- `EMPREST.AI-Back/README.md`: documenta `CORS_ORIGIN` na Vercel.
- `docs/specs/vercel-cors-deploy.md`: spec da conexão entre deployments.
- `plans/vercel-cors-deploy.md`: plano da alteração.
- `tasks/vercel-cors-deploy.md`: T-01 e T-02 concluídas; T-03 bloqueada.
- `docs/specs/api-url-env-frontend.md`: spec anterior da leitura de env no
  frontend.
- `docs/specs/nest-service-injection.md`: spec da injeção dos serviços Nest.

## Comandos e verificações

- `node --check .\script.js`: passou.
- `npm run build` em `EMPREST.AI`: passou.
- `npm run build` em `EMPREST.AI-Back`: passou.
- `npm test` em `EMPREST.AI-Back`: passou; 1 suíte e 1 teste.
- `git diff --check`: passou.
- Preflight público em
  `https://emprestaiback.vercel.app/v1/auth/login`: rota alcançada; a
  validação pública confirmou a política de origem.
- Teste com payload inválido na rota correta retornou `400`, indicando que a
  requisição chegou ao backend; não foi usado nenhum segredo ou senha real.

## Bloqueios e decisões pendentes

- Bloqueio: acesso aos projetos/painéis da Vercel para salvar:
  `VITE_API_URL=https://emprestaiback.vercel.app` no frontend e
  `CORS_ORIGIN=https://emprest-ai.vercel.app` no backend, seguido de
  redeploy.
- Pendente: validar no navegador que o request final é
  `POST https://emprestaiback.vercel.app/v1/auth/login` e concluir o login
  com o usuário criado pelo responsável.
- Não alterar a política para `*`; manter allowlist explícita.

## Próximos passos

1. Configurar as duas variáveis nos projetos correspondentes da Vercel.
2. Fazer redeploy do backend e do frontend.
3. Abrir `https://emprest-ai.vercel.app` e confirmar no Network a URL com
   `/v1/auth/login`.
4. Testar o login com o novo usuário.
5. Se o endpoint retornar `401` após a URL estar correta, investigar o
   usuário no Supabase/Auth; isso será um problema de credencial, não de CORS
   ou rota.
6. Atualizar T-03 para `concluída` somente após o teste público passar.
