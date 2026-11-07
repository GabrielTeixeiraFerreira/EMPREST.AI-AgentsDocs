# Spec: solicitar empréstimo

## Contexto e problema

O colaborador precisa consultar o catálogo de equipamentos e retirar um item
disponível sem aprovação manual ou fila de espera. A solicitação deve criar o
empréstimo imediatamente e manter o equipamento indisponível enquanto estiver
emprestado.

## Objetivo

Entregar o fluxo de catálogo, solicitação imediata e consulta dos empréstimos
em aberto do colaborador autenticado.

## Fora de escopo

- aprovação de Operações;
- fila de espera;
- devolução;
- tela de Operações;
- reserva com data futura;
- notificação por e-mail;
- renovação.

## Usuários ou sistemas envolvidos

- Colaborador autenticado: consulta equipamentos, solicita um item e consulta
  seus empréstimos em aberto;
- API: valida as regras de negócio, calcula o vencimento e persiste o
  empréstimo;
- Banco de dados: mantém a consistência de solicitações concorrentes.

## Comportamento esperado

1. O colaborador autenticado vê no catálogo a foto, o nome, a categoria e a
   situação de cada equipamento.
2. Apenas equipamentos disponíveis podem ser solicitados.
3. A solicitação cria o empréstimo na mesma operação, com devolução prevista
   para 14 dias úteis depois, e o equipamento passa a ficar emprestado.
4. O colaborador não pode solicitar se possuir 5 empréstimos em aberto.
5. O colaborador não pode solicitar enquanto tiver qualquer empréstimo em
   atraso.
6. Equipamentos em manutenção ou já emprestados não podem ser solicitados.
7. Solicitações simultâneas para o mesmo equipamento são serializadas; no
   máximo uma cria empréstimo.
8. O colaborador vê somente seus empréstimos em aberto e suas datas previstas.
9. O catálogo não exibe rodapé, texto de crédito ou informação além dos dados
   do equipamento necessários ao fluxo.

## Regras de negócio e estados

- `available`: pode ser solicitado;
- `maintenance`: não pode ser solicitado;
- equipamento com empréstimo sem `returnedAt`: emprestado e não pode ser
  solicitado;
- empréstimo em aberto: `returnedAt` nulo;
- empréstimo em atraso: empréstimo em aberto cuja data atual ultrapassa
  `dueAt`;
- o limite de 5 considera somente empréstimos em aberto do colaborador;
- dias úteis consideram segunda a sexta-feira; feriados não fazem parte desta
  versão.

## Critérios de aceitação

- **CA-01:** Após autenticar, o colaborador vê no catálogo a imagem, o nome, a
  categoria e a situação de cada equipamento, sem rodapé ou crédito de foto.
- **CA-02:** Um equipamento disponível pode ser solicitado; a API cria o
  empréstimo imediatamente, calcula `dueAt` para 14 dias úteis e o catálogo
  passa a indicar o item como emprestado.
- **CA-03:** A API rejeita solicitação de equipamento em manutenção ou com
  empréstimo em aberto.
- **CA-04:** A API rejeita nova solicitação quando o colaborador possui 5
  empréstimos em aberto.
- **CA-05:** A API rejeita nova solicitação quando o colaborador possui
  empréstimo em atraso.
- **CA-06:** Em duas solicitações concorrentes para o mesmo equipamento,
  exatamente uma é criada e a outra recebe erro explícito.
- **CA-07:** O colaborador vê somente seus empréstimos em aberto, incluindo a
  data de devolução prevista.
- **CA-08:** O frontend exibe estados de carregamento, sucesso e erro da
  solicitação e atualiza o catálogo e a lista de empréstimos após sucesso.

## Riscos, dependências e dúvidas abertas

- O fluxo depende de uma sessão Supabase válida e da associação do usuário ao
  tenant `suporte_ti`.
- A consistência concorrente será garantida por transação e bloqueio da linha
  do equipamento no PostgreSQL, sem alteração de schema nesta tarefa.
- A divergência entre o PRD anterior (limite 3 e prazo em dias corridos) e este
  briefing é resolvida por esta spec: limite 5 e 14 dias úteis.
- Não há feriados configurados nesta versão.
