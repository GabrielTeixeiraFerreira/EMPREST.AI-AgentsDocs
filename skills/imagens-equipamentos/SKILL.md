---
name: imagens-equipamentos
description: "Prepare imagens de equipamentos para o sistema usando a API do Pixabay. Use quando for buscar, selecionar, recortar, validar, catalogar ou preparar fotos para o Supabase Storage, exigindo 800x600, uma foto única por equipamento, sem pessoas, marcas ou logos legíveis, com créditos preservados."
argument-hint: "Informe os equipamentos e, se disponível, a pasta de saída."
---

# Imagens de equipamentos do Pixabay

## Objetivo

Preparar um conjunto de fotos para os equipamentos do sistema, mantendo uma
imagem diferente para cada equipamento e um registro auditável da origem.
Esta skill pode preparar os arquivos e o manifesto local, mas não faz upload no
Supabase Storage do deploy.

## Restrições obrigatórias

- Usar a API oficial do Pixabay (`https://pixabay.com/api/pt/` ou o endpoint
  documentado pela conta) para localizar as imagens.
- Nunca colocar a chave da API em código, arquivos gerados, logs, manifesto,
  prompt, commit ou repositório. Ler a chave somente de variável de ambiente ou
  segredo já configurado; se não estiver disponível, parar e informar o
  bloqueio.
- Usar somente ferramentas, bibliotecas e comandos permitidos pelas rules e
  pelos ADRs do projeto. Não instalar dependências novas sem autorização.
- Aceitar somente fotos em que não haja pessoas visíveis, marcas identificáveis
  ou logos/textos de marca legíveis. `safesearch` não substitui a inspeção
  visual. Rejeitar uma imagem em caso de dúvida.
- Cada arquivo final deve ter exatamente `800x600` pixels. Fazer o recorte
  central ou focal e redimensionar com uma ferramenta já permitida; não
  distorcer o equipamento.
- Usar exatamente uma foto por equipamento e não reutilizar a mesma imagem.
  Conferir o `id` do Pixabay e, quando possível, um hash do arquivo contra o
  manifesto inteiro.
- Preservar os créditos de cada imagem: equipamento, nome do arquivo, `id` do
  Pixabay, página da imagem, autor, URL usada, data de coleta e transformações.
- A interface que exibe as fotos deve mostrar **“Fotos: Pixabay”** como link
  para `https://pixabay.com/pt/`, conforme as diretrizes da API.
- O bucket de destino é público no Supabase Storage, mas a skill não tenta
  autenticar, configurar o bucket ou enviar arquivos para o Supabase do deploy.

## Procedimento

1. Confirmar o escopo (frontend, backend ou ambos), listar os equipamentos e
   localizar a spec, ADR e rules relevantes antes de editar qualquer código.
2. Confirmar a pasta de saída e verificar se existe um manifesto anterior.
   Carregar os IDs e hashes já usados para impedir repetição.
3. Para cada equipamento, consultar o Pixabay com termos específicos, usando
   `image_type=photo`, orientação horizontal quando útil e `safesearch=true`.
   Buscar candidatos adicionais quando os primeiros forem rejeitados.
4. Abrir a página de cada candidato e inspecionar a imagem em tamanho
   suficiente. Rejeitar pessoas, partes de pessoas, marcas, logos, embalagens
   ou qualquer texto de marca legível; registrar o motivo da rejeição sem
   salvar conteúdo desnecessário.
5. Escolher um candidato ainda não usado. Baixar a imagem sem expor a chave e
   produzir um arquivo final `800x600`, preservando o enquadramento do
   equipamento.
6. Validar o arquivo final: formato suportado pelo projeto, dimensões exatas,
   leitura bem-sucedida e ausência de repetição por ID e hash. Se qualquer
   verificação falhar, substituir o candidato.
7. Salvar o manifesto de créditos junto dos arquivos. O manifesto não deve
   conter a chave da API. Usar um formato já adotado pelo projeto; se não
   houver um, preferir JSON com uma entrada por equipamento.
8. Entregar a pasta preparada para upload manual no bucket público, incluindo
   uma lista clara de caminhos/URLs esperados após o upload. Não declarar que
   o upload foi concluído.
9. Se houver alteração de tela ou contrato, aplicar o fluxo SDD do projeto e
   garantir o texto **“Fotos: Pixabay”** com link visível na tela de fotos.

## Estrutura mínima do manifesto

Cada entrada deve conter:

```json
{
  "equipment": "nome-do-equipamento",
  "filename": "nome-do-equipamento.webp",
  "pixabayId": 123456,
  "pixabayPageUrl": "https://pixabay.com/pt/photos/...",
  "author": "autor informado pelo Pixabay",
  "sourceUrl": "URL de download usada",
  "collectedAt": "data ISO-8601",
  "width": 800,
  "height": 600,
  "sha256": "hash do arquivo final",
  "transformations": "recorte e redimensionamento aplicados"
}
```

## Verificação antes de concluir

- Todos os equipamentos solicitados têm exatamente uma imagem.
- Todos os arquivos medem `800x600`.
- Nenhum `pixabayId` ou `sha256` se repete.
- A inspeção visual não encontrou pessoas, marcas ou logos legíveis.
- O manifesto de créditos existe e não contém segredos.
- A tela exibe “Fotos: Pixabay” com link.
- O resultado distingue claramente arquivos preparados de arquivos enviados;
  sem acesso ao deploy, o upload fica pendente para o responsável.
