# Plano: URL da API por variável de ambiente no frontend

## Arquivos e módulos

- `EMPREST.AI/package.json`: adicionar scripts e dependência de build.
- `EMPREST.AI/vite.config.js`: configurar a entrada estática do frontend.
- `EMPREST.AI/index.html`: carregar o script como módulo.
- `EMPREST.AI/script.js`: resolver a URL via `import.meta.env.VITE_API_URL`.
- `EMPREST.AI/.env.example`: manter documentada a variável pública.
- `EMPREST.AI/README.md`: documentar execução e build.

## Abordagem técnica

Adicionar a menor configuração Vite compatível com a estrutura existente,
sem migrar a página para framework. O módulo JavaScript usará
`import.meta.env.VITE_API_URL` com fallback local; todas as requisições já
compartilham a constante `API_URL`.

## Ordem de execução

1. Criar a configuração e os scripts do Vite.
2. Alterar a entrada HTML e a resolução da URL.
3. Atualizar a documentação de execução.
4. Instalar dependências conforme o manifesto alterado.
5. Executar as verificações dos critérios de aceitação.

## Testes e build

- `node --check script.js`;
- `npm run build`;
- inspeção do bundle para confirmar que a URL configurada é incorporada.

## Impacto

- Dados: nenhum.
- Contratos: nenhum.
- Segurança: somente variáveis `VITE_*` públicas são consumidas.
- Operação: deployments devem definir `VITE_API_URL` antes do build.

## Rastreabilidade

- Etapa 1: CA-01 e CA-04.
- Etapa 2: CA-02 e CA-03.
- Etapa 3: CA-04.
- Etapas 4-5: CA-04.
