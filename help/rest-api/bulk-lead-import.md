---
title: Importação de leads em massa
feature: REST API
description: Crie e monitore importações assíncronas de leads em massa no Marketo com CSV, TSV ou SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
TQID: https://experienceleague.adobe.com/UamXYWis5J1ERqnp5lAnfUf3pFcgfSOLfKRXRB-Yg4I
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 619
ht-degree: 1%

---

# Importação de leads em massa

[Referência de Ponto de Extremidade de Importação de Cliente Potencial em Massa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads)

Use a [API em massa](https://developer.adobe.com/marketo-apis/api/mapi#operation/importLeadUsingPOST) para importar grandes números de registros de cliente potencial de forma assíncrona. Forneça os registros em um arquivo simples delimitado por vírgula, tabulação ou ponto-e-vírgula com menos de 10 MB.

A importação de clientes potenciais em massa dá suporte apenas à operação de registro &quot;inserir ou atualizar&quot;.

## Limites de processamento

Cada solicitação de importação em massa é adicionada como um trabalho a uma fila FIFO (first-in, first-out). São aplicáveis os seguintes limites:

- No máximo dois trabalhos podem ser processados simultaneamente.
- Um máximo de 10 tarefas podem estar na fila, incluindo as duas tarefas que estão sendo processadas.

Se você exceder o máximo de 10 trabalhos, a API retornará um erro `1016, Too many imports`.

## Importar arquivo

A primeira linha do arquivo deve ser um cabeçalho que lista os campos da API REST para os quais os valores em cada mapa de linha. Um arquivo típico segue este padrão:

```csv
email,firstName,lastName
test@example.com,John,Doe
```

Usar `externalCompanyId` para vincular um registro de cliente potencial a um registro de empresa. Use `externalSalesPersonId` para vincular um registro de cliente potencial a um registro de vendedor.

Enviar a solicitação usando o tipo de conteúdo `multipart/form-data`. Use uma implementação de biblioteca existente para criar a solicitação de várias partes.

## Criação de um trabalho

Para criar um trabalho de importação em massa, defina o tipo de conteúdo como `multipart/form-data` e inclua estes parâmetros:

- `file`: O conteúdo do arquivo de importação.
- `format`: O formato de arquivo. Os valores válidos são `csv`, `tsv` e `ssv`.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Este ponto de extremidade usa [multipart/form-data como o tipo de conteúdo](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Use uma biblioteca de suporte HTTP para seu idioma preferido a fim de criar a solicitação corretamente. O exemplo a seguir usa cURL da linha de comando:

```bash
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Neste exemplo, o arquivo de importação `lead_data.csv` contém os seguintes dados:

```text
firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Você também pode incluir estes parâmetros opcionais:

- `lookupField`: seleciona o campo usado para desduplicação e o padrão é `email`. Especifique `id` para executar uma operação &quot;somente atualização&quot;.
- `listId`: seleciona uma lista estática. Os clientes potenciais importados se tornam membros desta lista, além de quaisquer registros criados ou atualizados pela importação.
- `partitionName`: Seleciona a partição para a qual importar. Consulte a seção Espaços de trabalho e partições para obter mais informações.

Como a API é assíncrona, a resposta contém `batchId` e `status` campos em vez de sucessos e falhas individuais. O status pode ser `Queued`, `Importing` ou `Failed`.

Mantenha o `batchId` para verificar o status do trabalho e recuperar falhas ou avisos após a conclusão. O `batchId` permanece válido por sete dias.

## Status do trabalho de pesquisa

Use a API Obter status do lead de importação para pesquisar o trabalho a cada 5-30 segundos, dependendo dos requisitos de latência e das limitações de chamada da API.

```http
GET /bulk/v1/leads/batch/{id}.json
```

```json
{
   "requestId":"8136#146daebc2ed",
   "success":true,
   "result":[
      {
         "batchId":1022,
         "status":"Complete",
         "numOfLeadsProcessed":2,
         "numOfRowsFailed":1,
         "numOfRowsWithWarning":0,
         "message":"Import completed with errors, 2 records imported (2 members), 1 failed"
      }
   ]
}
```

Esta resposta mostra uma importação concluída. O status pode ser um dos seguintes valores:

- Completado
- Enfileirado
- Importando
- Falha

Quando o job for concluído, a resposta listará o número de linhas processadas, com falha e processadas com avisos. O parâmetro `message` também pode fornecer uma mensagem de falha quando o status é `Failed`.

## Falhas

O atributo `numOfRowsFailed` na resposta Obter Status de Cliente Potencial de Importação indica o número de linhas com falha. Um valor maior que zero significa que ocorreram falhas.

Para recuperar os registros com falha e suas causas, solicite o arquivo de falha:

```http
GET /bulk/v1/leads/batch/{id}/failures.json
```

A API retorna um arquivo que identifica cada linha com falha e explica por que o registro falhou. O arquivo usa o formato especificado pelo parâmetro `format` durante a criação do trabalho. Um campo adicional em cada registro descreve a falha.

## Avisos

O atributo `numOfRowsWithWarning` na resposta Obter Status de Cliente Potencial de Importação indica o número de linhas com avisos. Um valor maior que zero significa que ocorreram avisos.

Para recuperar os registros afetados e suas causas, solicite o arquivo de aviso:

```http
GET /bulk/v1/leads/batch/{id}/warnings.json
```

A API retorna um arquivo que identifica cada linha com um aviso e explica por que o aviso ocorreu. O arquivo usa o formato especificado pelo parâmetro `format` durante a criação do trabalho. Um campo adicional em cada registro descreve o aviso.
