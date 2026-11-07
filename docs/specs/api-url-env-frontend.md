# Spec: URL da API por variável de ambiente no frontend

## Contexto e problema

O frontend mantém `VITE_API_URL` no arquivo `.env`, mas o código executado
no navegador procura `window.EMPRESAI_API_URL`. Como essa propriedade não é
carregada pelo HTML, as requisições usam sempre o fallback local.

## Objetivo

Fazer o frontend usar a URL definida em `VITE_API_URL` durante o build,
preservando o fallback local para desenvolvimento sem configuração.

## Fora de escopo

- alterações no backend;
- mudanças no contrato ou nas rotas da API;
- exposição de variáveis privadas no bundle do frontend.

## Sistemas envolvidos

- Frontend `EMPREST.AI/`;
- ferramenta de build Vite, responsável por disponibilizar variáveis
  `VITE_*` ao código do navegador.

## Comportamento esperado

- O build substitui `import.meta.env.VITE_API_URL` pela URL configurada no
  ambiente correspondente.
- Login e carregamento do catálogo usam a mesma URL resolvida.
- Sem `VITE_API_URL`, o frontend continua usando
  `http://localhost:3000/v1`.
- Variáveis sem o prefixo `VITE_` não são lidas nem incluídas no bundle.

## Critérios de aceitação

- CA-01: o frontend lê `VITE_API_URL` por `import.meta.env` no build.
- CA-02: `POST /auth/login` e `GET /equipment` usam a URL configurada.
- CA-03: a ausência de `VITE_API_URL` mantém o fallback local.
- CA-04: `npm run build` e `node --check` passam no frontend.

## Riscos, dependências e dúvidas abertas

- A configuração passa a depender do build Vite para gerar o bundle de
  produção.
- O valor de `VITE_API_URL` é público por definição e não pode conter
  segredos.
- Não há dúvidas abertas.
