---
title: Importação de Membros do Programa em Massa
feature: REST API
description: Saiba como importar membros do programa em massa por meio da API REST do Marketo usando arquivos CSV TSV ou SSV com menos de 10 MB, limites de fila, parâmetros necessários e status do trabalho de sondagem.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
TQID: https://experienceleague.adobe.com/T1PAzLN1mnp38kJ0jwh6kPv6r1Uvxc7-o9zeTHetIV0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 771
ht-degree: 0%

---

# Importação de Membros do Programa em Massa

[Referência de Ponto de Extremidade de Importação de Membro do Programa em Massa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members)

Use a [API em massa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members) para importar grandes números de registros de membros de programas de forma assíncrona. Forneça os registros em um arquivo simples delimitado por vírgula, tabulação ou ponto-e-vírgula com menos de 10 MB.

A importação de membros do programa em massa oferece suporte somente à operação de registro &quot;inserir ou atualizar&quot;.

## Limites de processamento

Cada solicitação de importação em massa é adicionada como um trabalho a uma fila FIFO (first-in, first-out). São aplicáveis os seguintes limites:

- No máximo dois trabalhos podem ser processados simultaneamente.
- Um máximo de 10 tarefas podem estar na fila, incluindo as duas tarefas que estão sendo processadas.

Se você exceder o máximo de 10 trabalhos, a API retornará um erro `1016, Too many imports`.

## Importar arquivo

A primeira linha do arquivo deve ser um cabeçalho que lista os nomes de campo da API REST para os quais os valores em cada mapa de linha. Recupere esses nomes usando os pontos de extremidade [Descrever lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) e [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeProgramMemberUsingGET).

Os registros podem conter campos de cliente potencial, campos de cliente potencial personalizados e campos de membro de programa personalizado.

Um arquivo típico segue este padrão:

```text
email,firstName,lastName
test@example.com,John,Doe
```

Enviar a solicitação usando o tipo de conteúdo `multipart/form-data`. Use uma implementação de biblioteca existente para criar a solicitação de várias partes.

## Criação de um trabalho

O ponto de extremidade [Importar Membros do Programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lê os registros de membros do programa de um arquivo e os adiciona a um programa com status especificado. Os registros podem conter campos de cliente potencial e campos de membro de programa personalizado.

Todos os registros devem incluir o campo de email, que é usado para desduplicação.

O parâmetro de caminho `programId` especifica o programa ao qual os membros são adicionados.

A solicitação requer três parâmetros de consulta:

- `format`: O formato de arquivo de importação (`CSV`, `TSV` ou `SSV`).
- `programMemberStatus`: o status do programa atribuído aos membros importados.
- `file`: o nome do arquivo que contém os registros de membros do programa.

```http
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```text
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```text
----------------------------118046853683028616211319
Content-Disposition: form-data; name="file"; filename="Lead-House-Lannister.csv"
Content-Type: text/csv

firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0

----------------------------118046853683028616211319--
```

```json
{
    "requestId": "17f4a#16f87f87325",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Como o ponto de extremidade é assíncrono, a resposta contém `batchId` e `status` campos. O status pode ser `Queued`, `Importing` ou `Failed`.

Mantenha o `batchId` para verificar o status da importação e recuperar falhas ou avisos após a conclusão. O `batchId` permanece válido por sete dias.

A seguinte solicitação cURL de linha de comando envia o exemplo de trabalho:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Neste exemplo, o arquivo de importação `Lead-House-Lannister.csv` contém os seguintes dados:

```text
firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0
```

## Status do trabalho de pesquisa

Depois de criar o trabalho de importação, sonde-o a cada 5-30 segundos. Passe o parâmetro de caminho `batchId` para o ponto de extremidade [Obter Status de Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "e0cb#16f87f8b177",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Complete",
            "numOfLeadsProcessed": 8,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 8 records imported (8 members)"
        }
    ],
    "success": true
}
```

Esta resposta mostra uma importação concluída. O status pode ser `Complete`, `Queued`, `Importing` ou `Failed`.

Quando o job for concluído, a resposta listará o número de linhas processadas, com falha e processadas com avisos. O parâmetro `message` também pode fornecer uma mensagem de falha quando o status é `Failed`.

## Falhas

O atributo `numOfRowsFailed` na resposta [Obter Status de Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indica o número de linhas com falha. Um valor maior que zero significa que ocorreram falhas.

Passe o parâmetro de caminho `batchId` para o ponto de extremidade Obter Falhas de Membro do Programa de Importação para recuperar os registros com falha e suas causas.

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

O ponto de extremidade retorna um arquivo que identifica cada linha com falha e explica por que o registro falhou. O arquivo usa o formato especificado pelo parâmetro `format` durante a criação do trabalho. Um campo adicional em cada registro descreve a falha.

Por exemplo, suponha que você importe o seguinte arquivo com uma pontuação de lead inválida:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

O status do trabalho retorna `numOfRowsFailed` como 1, indicando que ocorreu uma falha:

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "4c2d#16f8b32c8ef",
    "result": [
        {
            "batchId": 1046,
            "importId": "1046",
            "status": "Complete",
            "numOfLeadsProcessed": 0,
            "numOfRowsFailed": 1,
            "numOfRowsWithWarning": 0,
            "message": "Import completed with errors, 0 records imported (0 members), 1 failed"
        }
    ],
    "success": true
}
```

Recupere o arquivo de falha para obter mais informações:

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avisos

O atributo `numOfRowsWithWarning` na resposta [Obter Status do Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indica o número de linhas com avisos. Um valor maior que zero significa que ocorreram avisos.

Passe o parâmetro de caminho `batchId` para o ponto de extremidade [Obter Avisos do Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) para recuperar os registros afetados e suas causas.

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

O endpoint retorna um arquivo que identifica cada linha com um aviso e explica por que o aviso ocorreu. O arquivo usa o formato especificado pelo parâmetro `format` durante a criação do trabalho. Um campo adicional em cada registro descreve o aviso.

Por exemplo, suponha que você importe o seguinte arquivo com um endereço de email inválido:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

O status do trabalho retorna `numOfRowsWithWarning` como 1, indicando que ocorreu um aviso:

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
   "requestId":"4ca1#16f883c2003",
   "result":[
      {
         "batchId":1041,
         "importId":"1041",
         "status":"Complete",
         "numOfLeadsProcessed":1,
         "numOfRowsFailed":0,
         "numOfRowsWithWarning":1,
         "message":"Import succeeded, 1 records imported (1 members), 1 warning."
      }
   ],
   "success":true
}
```

Recuperar o arquivo de aviso para obter mais informações:

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
