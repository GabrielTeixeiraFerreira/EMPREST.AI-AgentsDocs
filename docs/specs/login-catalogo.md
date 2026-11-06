# Spec: login e catálogo inicial

Status: implementação concluída; validação completa bloqueada pelo ambiente
de banco remoto e pela ausência de um teste autenticado executável localmente.

## Contexto e problema

O frontend possui apenas uma simulação de login e o backend ainda não oferece
autenticação nem leitura do catálogo inicial.

## Objetivo

Permitir que `userTeste` entre com a senha `123456` e veja o catálogo de
equipamentos do tenant `suporte_ti`.

## Fora de escopo

- criação de empréstimos;
- tela de operações;
- cadastro manual de equipamentos;
- upload de imagens.

## Comportamento esperado

- O catálogo contém os 12 equipamentos informados, com patrimônio, nome,
  categoria, situação e imagem.
- O login aceita `userTeste` e `123456`, cria/atualiza a conta Supabase
  `userTeste@emprest.ai` pelo seed e retorna uma sessão.
- Credenciais inválidas recebem erro explícito sem revelar qual campo falhou.
- O catálogo exige token Bearer válido e retorna somente itens do tenant
  `suporte_ti`.
- Após login bem-sucedido, a página substitui o formulário pelo catálogo.

## Critérios de aceitação

- CA-01: migration idempotente cria/atualiza o tenant e os 12 equipamentos.
- CA-02: seed idempotente cria/atualiza `userTeste@emprest.ai` com senha
  `123456` e sua membership no tenant.
- CA-03: `POST /v1/auth/login` autentica credenciais válidas e rejeita
  credenciais inválidas.
- CA-04: `GET /v1/equipment` exige autenticação e retorna os 12 campos do
  catálogo do tenant correto.
- CA-05: o frontend envia login ao backend e renderiza o catálogo após sucesso.
- CA-06: testes, migration reset e builds do backend passam; o JavaScript do
  frontend passa em `node --check`.

## Riscos e dependências

- O Supabase Auth é externo ao Prisma; a conta é criada pelo seed via Admin
  API e a sessão é validada pela API Auth do Supabase.
- O backend depende de `DATABASE_URL`, `DIRECT_URL`, `SUPABASE_URL` e
  `SUPABASE_SECRET_KEY`.
