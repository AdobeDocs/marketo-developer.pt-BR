---
title: Importação de Membros do Programa em Massa
feature: REST API
description: Importação em lote de dados do membro.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 8a785b0719e08544ed1a87772faf90bd9dda3077
workflow-type: tm+mt
source-wordcount: '837'
ht-degree: 0%

---

# Importação de Membros do Programa em Massa

[Referência de Ponto de Extremidade de Importação de Membro de Programa em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Para grandes quantidades de registros de membros de programas, os membros de programas podem ser importados de forma assíncrona com a [API em massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Isso permite importar uma lista de registros para o Marketo usando um arquivo simples com os delimitadores (vírgula, tabulação ou ponto e vírgula). O arquivo pode conter qualquer número de registros, desde que o arquivo totalize menos de 10 MB. A operação de registro é somente &quot;inserir ou atualizar&quot;.

## Limites de processamento

Você pode enviar mais de uma solicitação de importação em massa, com limitações. Cada solicitação é adicionada como um trabalho a uma fila FIFO para ser processada. No máximo dois trabalhos são processados ao mesmo tempo. São permitidos no máximo dez trabalhos na fila em um determinado momento (incluindo os 2 que estão sendo processados no momento). Se você exceder o máximo de dez tarefas, um erro &quot;1016, Muitas importações&quot; será retornado.

## Importar arquivo

A primeira linha do arquivo deve ser um cabeçalho que lista os nomes da API REST correspondentes como campos para mapear os valores de cada linha. Os nomes da API REST podem ser recuperados usando os pontos de extremidade [Descrever Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) e/ou [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET). Os registros podem conter campos de cliente potencial, campos de cliente potencial personalizados e campos de membro de programa personalizado.

Um arquivo típico seguiria este padrão básico:

```
email,firstName,lastName
test@example.com,John,Doe
```

A própria chamada é feita usando o tipo de conteúdo `multipart/form-data`.

Esse tipo de solicitação pode ser difícil de implementar, portanto, é altamente recomendável usar uma implementação de biblioteca existente.

## Criação de um trabalho

O ponto de extremidade [Importar Membros do Programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lê um arquivo contendo registros de membros do programa e os adiciona a um programa com determinado status. Os registros podem conter campos de cliente potencial e campos personalizados de membros do programa. Todos os registros devem incluir o campo de email, que é usado para fins de desduplicação.

O parâmetro de caminho `programId` especifica o programa ao qual os membros são adicionados.

Há três parâmetros de consulta necessários. O parâmetro `format` especifica o formato do arquivo de importação (CSV, TSV ou SSV), o parâmetro `programMemberStatus` especifica o status do programa para os membros que estão sendo adicionados ao programa e o parâmetro `file` contém o nome do arquivo de importação que contém registros de membros do programa.

```
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```
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

Observe na resposta à nossa chamada que há um campo `batchId` e um campo `status` para o registro na matriz de resultados. Como esse endpoint é assíncrono, ele pode retornar um status de Enfileirado, Importação ou Falha. Você deve reter o `batchId` para obter o status do trabalho de importação e para recuperar falhas e/ou avisos após a conclusão. O `batchId` permanece válido por sete dias.

Usando o exemplo acima, uma maneira simples de chamar o endpoint é usar cURL a partir da linha de comando:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Onde o arquivo de importação &quot;Lead-House-Lannister.csv&quot; contém o seguinte:

```
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

Após criar o trabalho de importação, você deve consultar seu status. É prática recomendada pesquisar o trabalho de importação a cada 5-30 segundos. Faça isso passando o parâmetro de caminho `batchId` para o ponto de extremidade [Obter Status de Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```
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

Esta resposta mostra uma importação concluída. O status pode ser um destes: Concluído, Em fila, Importação, Falha.

Se a tarefa tiver sido concluída, você terá uma lista do número de linhas processadas, com falha ou com avisos. O parâmetro de mensagem também pode fornecer a mensagem de falha se o status for Failed.

## Falhas

As falhas são indicadas pelo atributo `numOfRowsFailed` na resposta [Obter Status de Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Se numOfRowsFailed for maior que zero, esse valor indicará o número de falhas que ocorreram.

Use o ponto de extremidade Obter Falhas de Membro do Programa de Importação para recuperar registros e causas de linhas com falha transmitindo o parâmetro de caminho `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

O endpoint responde com um arquivo indicando quais linhas falharam, juntamente com uma mensagem indicando por que o registro falhou. O formato do arquivo é igual ao especificado no parâmetro `format` durante a criação do trabalho. Um campo adicional é anexado a cada registro com uma descrição da falha.

Por exemplo, suponha que você importe o seguinte arquivo com uma pontuação de lead inválida:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Ao verificar o status do trabalho, você vê `numOfRowsFailed` é 1, o que indica que ocorreu uma falha:

```
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

Em seguida, recupere o arquivo de falhas para obter detalhes adicionais sobre a falha:

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avisos

Os avisos são indicados pelo atributo `numOfRowsWithWarning` na resposta [Obter Status do Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Se `numOfRowsWithWarning` for maior que zero, esse valor indicará o número de avisos que ocorreram.

Use o ponto de extremidade [Obter Avisos do Membro do Programa de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) para recuperar registros e causas de linhas de aviso transmitindo o parâmetro de caminho `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

O endpoint responde com um arquivo indicando quais linhas produziram avisos, juntamente com uma mensagem indicando por que o registro produziu um aviso. O formato do arquivo é igual ao especificado no parâmetro `format` durante a criação do trabalho. Um campo adicional é anexado a cada registro com uma descrição do aviso.

Por exemplo, suponha que você importe o seguinte arquivo com um endereço de email inválido:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Ao verificar o status do trabalho, você verá `numOfRowsWithWarning` is 1, que indica que ocorreu um aviso:

```
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

Em seguida, recupere o arquivo de avisos para obter detalhes adicionais sobre o aviso:

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
