# Instruções do projeto

## Escopo

- `EMPREST.AI/` é o frontend.
- `EMPREST.AI-Back/` é o backend.
- Antes de investigar ou alterar arquivos, confirme com o usuário se o escopo é frontend, backend ou ambos.

## Prioridade operacional

1. Responda em português.
2. Explique o plano antes de alterar o código.
3. Prioridade máxima: o agente nunca cria commits, push, merge nem pull requests; essas ações ficam reservadas ao usuário.
4. O agente pode preparar alterações em branch local e relatar o diff, mas não executa commit ou PR sem autorização explícita do usuário.

## Fluxo obrigatório

- Use Arquitetura Limpa e princípios SOLID.
- Para mudanças de produto ou código, siga o fluxo SDD em `rules/spec-flow.md`.
- Para validação final, siga `rules/check.md`.
- Para alterações de schema ou migration, siga `rules/migration.md`.
- Para testes, siga `rules/testing.md`.
- Para handoff de sessão, siga `rules/handoff.md` somente quando o usuário pedir explicitamente.

## Referências do projeto

- `EMPREST.AI/docs/PRD.md`: requisitos do produto.
- `EMPREST.AI/docs/adr/001-stack.md`: arquitetura, stack, testes, operação e segurança.
- `rules/spec-flow.md`: spec, plano e tasks.
- `rules/check.md`: verificação antes de declarar tarefa concluída.
- `rules/testing.md`: estratégia de testes por escopo.
- `rules/migration.md`: procedimento de schema e migration.
- `skills/`: skills adicionais do projeto.

> Regra de precedência: a regra “o agente não cria commits nem pull requests” vale sobre qualquer outra instrução que sugira commit, push ou PR automáticos.
