# Plano: sistema completo EMPREST.AI

## Arquivos e módulos

- `EMPREST.AI/index.html`
- `EMPREST.AI/styles.css`
- `EMPREST.AI/script.js`
- `EMPREST.AI-Back/package.json`
- `EMPREST.AI-Back/src/server.ts`
- `EMPREST.AI-Back/src/mock-data.ts`
- `EMPREST.AI-Back/src/types.ts`

## Abordagem técnica

- manter a estrutura atual do repositório com frontend estático e backend em Node/TypeScript;
- implementar API REST em Express com dados em memória simulando o banco;
- consumir o backend no front por fetch com CORS habilitado;
- preservar o design da tela de login e expandir para as telas internas do PRD.

## Ordem de execução

1. Definir estado inicial, usuários e equipamentos de demonstração.
2. Implementar endpoints da API para autenticação, catálogo e empréstimos.
3. Construir o frontend multi-tela com navegação interna e interação das regras de negócio.
4. Validar fluxos de login, empréstimo, devolução e cadastro.
5. Executar build/testes e revisar diff final.

## Estratégia de testes e build

- verificar `npm install` e `npm run build` no backend;
- validar fluxo principal com testes rápidos em Node e com execução do servidor local;
- conferir que a UI responde a estados de sucesso, erro e bloqueio por atraso.

## Impacto em dados e operação

- a persistência é local e em memória, adequada ao ambiente de demonstração;
- a API opera sem acesso externo a banco ou credenciais reais;
- as regras de negócio estão centralizadas em funções do backend para manter coerência.

## Rastreabilidade com CA-xx

- CA-01 a CA-08 mapeados para a autenticação, catálogo, empréstimo, devolução, cadastro, bloqueio por atraso, e integração front-back.
