---
title: Campanhas inteligentes
feature: REST API, Smart Campaigns
description: Saiba como usar as APIs REST do Marketo para campanhas inteligentes, incluindo query por id ou nome, filtros de navegação, criar exclusão de clone e acionadores de agendamento ou solicitação
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
TQID: https://experienceleague.adobe.com/iysRjtqd9plkreyIMuNjAF3YVFHtDUIrc-GInB4V8mg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 978
ht-degree: 1%

---

# Campanhas inteligentes

[Referência de ponto de extremidade de campanhas inteligentes (Ativo)](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns)

[Referência De Endpoint De Campanhas (Clientes Potenciais)](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

Use as APIs REST do Smart Campaign para consultar, criar, clonar e excluir campanhas inteligentes. Você também pode agendar campanhas em lote, solicitar campanhas de acionador e gerenciar a ativação de campanhas.

## Consultar

Consultar campanhas inteligentes [por ID](#by_id), [por nome](#by_name) ou por [navegação](#browse).

### Por ID

O ponto de extremidade [Obter Campanha Inteligente por ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartCampaignByIdUsingGET) usa uma única campanha inteligente `id` como parâmetro de caminho e retorna um único registro de campanha inteligente.

```http
GET /rest/asset/v1/smartCampaign/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "7883#169838a32f0",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        }
    ]
}
```

O ponto de extremidade retorna um registro na primeira posição da matriz `result`.

### Por nome

O ponto de extremidade [Obter Campanha Inteligente por Nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getSmartCampaignByNameUsingGET) usa uma única campanha inteligente `name` como parâmetro e retorna um único registro de campanha inteligente.

```http
GET /rest/asset/v1/smartCampaign/byName.json?name=Test Trigger Campaign
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14494#16c886ffa44",
    "warnings": [],
    "result": [
        {
            "id": 1069,
            "name": "Test Trigger Campaign",
            "description": "",
            "createdAt": "2018-02-16T01:34:39Z+0000",
            "updatedAt": "2019-08-13T00:45:21Z+0000",
            "folder": {
                "id": 327,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 2747,
            "flowId": 1088,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1069A1"
        }
    ]
}
```

O ponto de extremidade retorna um registro na primeira posição da matriz `result`.

### Procurar

O ponto de extremidade [Obter Campanhas Inteligentes](https://developer.adobe.com/marketo-apis/api/asset#operation/getAllSmartCampaignsGET) dá suporte a parâmetros de consulta opcionais para filtragem e paginação.

Os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` aceitam `datetimes` no formato ISO-8601 (sem milissegundos). Se ambos forem definidos, o valor de antigoupdateAt deverá preceder o de latestUpdatedAt.

O parâmetro `folder` especifica a pasta pai a ser procurada. Passe-o como um objeto JSON contendo `id` e `type`.

O inteiro `maxReturn` especifica o número máximo de entradas. O padrão é 20, e o máximo é 200.

O inteiro `offset` especifica onde começar a recuperar entradas. Use com `maxReturn`. O padrão é 0.

Defina o parâmetro Booleano `isActive` para retornar apenas campanhas de gatilho ativas.

```http
GET /rest/asset/v1/smartCampaigns.json?earliestUpdatedAt=2016-09-10T23:15:00-00:00&latestUpdatedAt=2016-09-10T23:17:00-00:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "626#16983a92965",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        },
        {
            "id": 1002,
            "name": "Process Unsubscribes",
            "description": "System smart campaign for processing unsubscribe events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1002,
            "flowId": 1002,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1002A1"
        }
    ]
}
```

O ponto de extremidade retorna um ou mais registros na matriz `result`.

## Criar

Envie uma solicitação POST `application/x-www-form-urlencoded` para o ponto de extremidade [Criar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/createSmartCampaignUsingPOST). Os parâmetros `name` e `folder` são obrigatórios. Passe `folder` como um objeto JSON contendo `id` e `type`.

Como opção, você pode descrever a campanha inteligente usando o parâmetro `description` (máximo de 2.000 caracteres).

```http
POST /rest/asset/v1/smartCampaigns.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Smart Campaign 02&folder={"type": "folder","id": 640}&description=This is a smart campaign creation test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "25bc#16c9138f148",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02",
            "description": "This is a smart campaign creation test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T17:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Atualização

Envie uma solicitação POST `application/x-www-form-urlencoded` para o ponto de extremidade [Atualizar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset). O parâmetro de caminho `id` da campanha inteligente é necessário. Use `name` para alterar o nome ou `description` para alterar a descrição.

```http
POST /rest/asset/v1/smartCampaign/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
name=Smart Campaign 02 Update&description=This is a smart campaign update test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14b6a#16c924b992f",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02 Update",
            "description": "This is a smart campaign update test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T22:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Clonar

Envie uma solicitação POST `application/x-www-form-urlencoded` para o ponto de extremidade [Clonar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/cloneSmartCampaignUsingPOST). Os parâmetros `id`, `name` e `folder` são obrigatórios. Eles especificam a campanha de origem, o nome da nova campanha e a pasta principal. Passe `folder` como um objeto JSON contendo `id` e `type`.

Como opção, você pode descrever a campanha inteligente usando o parâmetro `description` (máximo de 2.000 caracteres).

```http
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Test Trigger Campaign Clone&folder={"type": "folder","id": 640}&description=This is a smart campaign clone test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "681d#16c9339499b",
    "warnings": [],
    "result": [
        {
            "id": 1077,
            "name": "Test Trigger Campaign Clone",
            "description": "This is a smart campaign clone test.",
            "createdAt": "2019-08-15T03:01:41Z+0000",
            "updatedAt": "2019-08-15T03:01:41Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5135,
            "flowId": 1096,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1077A1"
        }
    ]
}
```

## Excluir

O ponto de extremidade [Excluir Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteSmartCampaignUsingPOST) usa uma única campanha inteligente `id` como parâmetro de caminho.

```http
POST /rest/asset/v1/smartCampaign/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d757#16c934216ac",
    "warnings": [],
    "result": [
        {
            "id": 1077
        }
    ]
}
```

## Lote

As campanhas inteligentes em lote são executadas em um horário especificado e processam um conjunto definido de clientes potenciais juntos.

## Programação

Use [Agendar Campanha](https://developer.adobe.com/marketo-apis/api/mapi#operation/scheduleCampaignUsingPOST) para agendar uma campanha em lote. O parâmetro de caminho `id` da campanha é obrigatório. Passe os parâmetros `tokens`, `runAt` e `cloneToProgram` opcionais no corpo da solicitação JSON.

A matriz `tokens` substitui Meus Tokens de programa existentes para esta execução. O Marketo descarta as sobreposições após a execução da campanha. Cada item contém um par nome/valor, e o nome do token deve usar o formato `{{my.name}}`.

O parâmetro date-time `runAt` especifica quando executar a campanha. Se omitida, a campanha será executada cinco minutos após a solicitação. O valor não pode ser superior a dois anos no futuro.

As campanhas programadas por meio dessa API sempre aguardam no mínimo cinco minutos antes de serem executadas.

O parâmetro da cadeia de caracteres `cloneToProgram` contém o nome de um programa resultante.  Quando definido, faz com que a campanha, o programa principal e todos os seus ativos sejam criados com o novo nome resultante. O programa principal é clonado e a campanha recém-criada será agendada. O programa resultante é criado abaixo do pai. Programas com trechos, notificações por push, mensagens no aplicativo, listas estáticas, relatórios e ativos sociais não podem ser clonados dessa maneira. Quando usado, esse endpoint é limitado a 20 chamadas por dia. O ponto de extremidade do [programa clone](https://developer.adobe.com/marketo-apis/api/asset#operation/cloneProgramUsingPOST) é a alternativa recomendada.

```http
POST /rest/v1/campaigns/{id}/schedule.json
```

```json
{
   "input":
      {
         "runAt": "2018-03-28T18:05:00+0000",
         "tokens": [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
          ]
      }
}
```

```json
{
    "requestId": "52b#161d90e1743",
    "result": [
        {
            "id": 3713
        }
    ],
    "success": true
}
```

## Acionador

Acionar campanhas inteligentes processa uma pessoa de cada vez em resposta a um evento.

### Solicitação

Use [Solicitar Campanha](https://developer.adobe.com/marketo-apis/api/mapi#operation/triggerCampaignUsingPOST) para passar clientes potenciais pelo fluxo de uma campanha de acionador. A campanha deve usar um acionador Campaign is Requested com a API de serviço da Web como origem.

O parâmetro de caminho `id` da campanha e uma matriz de inteiros `leads` de IDs de clientes potenciais são necessários. Cada chamada aceita no máximo 100 clientes em potencial.

Opcionalmente, o parâmetro de matriz `tokens` pode ser usado para substituir Meus Tokens locais no programa pai da campanha. `tokens` aceita no máximo 100 tokens. Cada item da matriz `tokens` contém um par nome/valor. O nome do token deve ser formatado como &quot;`{{my.name}}`&quot;. Se você usar [Adicionar um Token do Sistema como um Link em uma abordagem de Email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) para adicionar o token do sistema &quot;viewAsWebpageLink&quot;, não será possível substituí-lo usando `tokens`. Em vez disso, use [Adicionar uma Exibição como Link da Página da Web para uma abordagem de Email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) que permite substituir &quot;viewAsWebPageLink&quot; usando `tokens`.

Passe os parâmetros `leads` e `tokens` no corpo da solicitação JSON.

```http
POST /rest/v1/campaigns/{id}/trigger.json
```

```json
{
   "input":
      {
         "leads" : [
            {
               "id" : 318592
            },
            {
               "id" : 318593
            }
         ],
         "tokens" : [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
         ]
      }
}
```

```json
{
    "requestId": "9e01#161d922f1aa",
    "result": [
        {
            "id": 3712
        }
    ],
    "success": true
}
```

### Ativar

O ponto de extremidade [Ativar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/activateSmartCampaignUsingPOST) é simples. Um parâmetro de caminho `id` é necessário. Para que a ativação seja bem-sucedida, o seguinte deve ser verdadeiro para a campanha:

- A campanha está desativada.
- A campanha tem pelo menos um acionador e uma etapa de fluxo.
- A campanha tem acionadores, filtros e etapas de fluxo sem erros.

```http
POST /rest/asset/v1/smartCampaign/{id}/activate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a33a#161d9c0dcf3",
    "result": [
        {
            "id": 1069
        }
    ]
}
```

### Desativar

A opção [Desativar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset#operation/deactivateSmartCampaignUsingPOST) é simples. Um parâmetro de caminho `id` é necessário. Para que a desativação seja bem-sucedida, a campanha deve ser ativada.

```http
POST /rest/asset/v1/smartCampaign/{id}/deactivate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6228#161d9c29fbf",
    "result": [
        {
            "id": 1069
        }
    ]
}
```
