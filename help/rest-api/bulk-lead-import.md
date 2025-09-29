---
title: Importação de leads em massa
feature: REST API
description: Crie e monitore importações assíncronas de leads em massa no Marketo com CSV, TSV ou SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---

# Importação de leads em massa

[Referência de Ponto de Extremidade de Importação de Cliente Potencial em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Para grandes quantidades de registros de clientes potenciais, eles podem ser importados de forma assíncrona com a [API em massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Isso permite importar uma lista de registros para o Marketo usando um arquivo simples com os delimitadores (vírgula, tabulação ou ponto e vírgula). O arquivo pode conter qualquer número de registros, desde que o arquivo totalize menos de 10 MB. A operação de registro é somente &quot;inserir ou atualizar&quot;.

## Limites de processamento

Você pode enviar mais de uma solicitação de importação em massa, com limitações. Cada solicitação é adicionada como um trabalho a uma fila FIFO para ser processada. No máximo dois trabalhos são processados ao mesmo tempo. São permitidos no máximo dez trabalhos na fila em um determinado momento (incluindo os 2 que estão sendo processados no momento). Se você exceder o máximo de dez tarefas, um erro &quot;1016, Muitas importações&quot; será retornado.

## Importar arquivo

A primeira linha do arquivo deve ser um cabeçalho que lista os campos da API REST correspondentes para mapear os valores de cada linha. Um arquivo típico seguiria este padrão básico:

```
email,firstName,lastName
test@example.com,John,Doe
```

O campo `externalCompanyId` pode ser usado para vincular o registro de cliente potencial a um registro de empresa. O campo `externalSalesPersonId` pode ser usado para vincular o registro de cliente potencial a um registro de vendedor.

A própria chamada é feita usando o tipo de conteúdo `multipart/form-data`.

Esse tipo de solicitação pode ser difícil de implementar, portanto, é altamente recomendável usar uma implementação de biblioteca existente.

## Criação de um trabalho

Para fazer uma solicitação de importação em massa, defina o cabeçalho de tipo de conteúdo como &quot;multipart/form-data&quot; e inclua pelo menos um parâmetro de arquivo com o conteúdo do arquivo e um parâmetro de formato com o valor &quot;csv&quot;, &quot;tsv&quot; ou &quot;ssv&quot; indicando o formato do arquivo.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

FirstName,LastName,Email,Company
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

Este ponto de extremidade usa [multipart/form-data como o tipo de conteúdo](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Isso pode ser difícil de corrigir, portanto, a prática recomendada é usar uma biblioteca de suporte HTTP para o idioma de sua escolha. Uma maneira simples de fazer isso com cURL a partir da linha de comando é semelhante a:

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Onde o arquivo de importação &quot;lead_data.csv&quot; contém o seguinte:

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Opcionalmente, também é possível incluir os parâmetros `lookupField`, `listId` e `partitionName` em sua solicitação. `lookupField` permite que você selecione um campo específico para desduplicar, assim como Sincronizar clientes em potencial, e o padrão é email. Você pode especificar `id` como `lookupField` para indicar uma operação &quot;somente atualização&quot;. `listId` permite que você selecione uma lista estática para importar a lista de clientes potenciais; isso fará com que os clientes potenciais na lista se tornem membros dessa lista estática, além de quaisquer criações ou atualizações causadas pela importação. `partitionName` seleciona uma partição específica para a qual importar. Consulte a seção Espaços de trabalho e partições para obter mais informações.

Observe, na resposta à nossa chamada, que não há uma lista de sucessos ou falhas como com Clientes potenciais de sincronização, mas um batchId e um campo de status para o registro na matriz de resultados. Isso ocorre porque essa API é assíncrona e pode retornar um status de Enfileirado, Importação ou Falha. Você deve reter o batchId para obter o status do trabalho de importação e para recuperar falhas e/ou avisos após a conclusão. O batchId permanece válido por sete dias.

## Status do trabalho de pesquisa

É prática recomendada pesquisar o trabalho a cada 5-30 segundos, dependendo da latência necessária e das limitações de chamada da API, para ver o status do trabalho de importação. Você pode fazer isso com a API Obter status de lead de importação.

```
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

Esta resposta mostra uma importação concluída, mas o status pode ser um dos seguintes:

- Completado
- Enfileirado
- Importando
- Falha

Se o trabalho tiver sido concluído, você terá uma listagem do número de linhas processadas, com falha, aquelas com avisos. O parâmetro de mensagem também pode fornecer a mensagem de falha se o status for Failed.

## Falhas

As falhas são indicadas pelo atributo &quot;numOfRowsFailed&quot; na resposta Obter Status de Cliente Potencial de Importação. Se &quot;numOfRowsFailed&quot; for maior que zero, esse valor indicará o número de falhas que ocorreram.

Para recuperar os registros e as causas de linhas com falha, você deverá recuperar o arquivo de falha:

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

A API responde com um arquivo indicando quais linhas falharam, juntamente com uma mensagem indicando por que o registro falhou. O formato do arquivo é o mesmo especificado no parâmetro &quot;format&quot; durante a criação do trabalho. Um campo adicional é anexado a cada registro com uma descrição da falha.

## Avisos

Os avisos são indicados pelo atributo &quot;numOfRowsWithWarning&quot; na resposta Obter Status de Cliente Potencial de Importação. Se &quot;numOfRowsWithWarning&quot; for maior que zero, esse valor indicará o número de avisos que ocorreram.

Para recuperar os registros e as causas de linhas de aviso, recupere o arquivo de aviso:

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

A API responde com um arquivo indicando quais linhas produziram avisos, juntamente com uma mensagem indicando por que o registro falhou. O formato do arquivo é o mesmo especificado no parâmetro &quot;format&quot; durante a criação do trabalho. Um campo adicional é anexado a cada registro com uma descrição do aviso.
