---
title: Campanhas inteligentes
feature: REST API, Smart Campaigns
description: Visão geral da campanha inteligente
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 1%

---

# Campanhas inteligentes

[Referência de Ponto de Extremidade de Campanhas Inteligentes (Ativo)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Referência de Ponto de Extremidade de Campanhas (Clientes Potenciais)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

O Marketo oferece um conjunto de APIs REST para executar operações em campanhas inteligentes. Essas APIs seguem o padrão de interface padrão para APIs de ativos, fornecendo opções de consulta, criação, clonagem e exclusão. Além disso, você pode gerenciar a execução inteligente da campanha agendando campanhas em lote ou solicitando campanhas de acionador.

## Consultar

A consulta de campanhas inteligentes segue os tipos de consulta padrão para ativos de [por id](#by_id), [por nome](#by_name) e [navegação](#browse).

### Por ID

O ponto de extremidade [Obter Campanha Inteligente por ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) usa uma única campanha inteligente `id` como parâmetro de caminho e retorna um único registro de campanha inteligente.

```
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

Com este ponto de extremidade, sempre haverá um único registro na primeira posição da matriz `result`.

### Por nome

O ponto de extremidade [Obter Campanha Inteligente por Nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) usa uma única campanha inteligente `name` como parâmetro e retorna um único registro de campanha inteligente.

```
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

Com este ponto de extremidade, sempre haverá um único registro na primeira posição da matriz `result`.

### Navegar

O ponto de extremidade [Obter Campanhas Inteligentes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) funciona como outros pontos de extremidade de navegação da API de ativos e permite que vários parâmetros de consulta opcionais especifiquem critérios de filtragem.

Os parâmetros `earliestUpdatedAt` e `latestUpdatedAt` aceitam `datetimes` no formato ISO-8601 (sem milissegundos). Se ambos forem definidos, o valor de antigoupdateAt deverá preceder o de latestUpdatedAt.

O parâmetro `folder` especifica a pasta pai na qual procurar. O formato é um bloco JSON contendo `id` e `type` atributos.

O parâmetro `maxReturn` é um número inteiro que especifica o número máximo de entradas a serem retornadas. O padrão é 20. O máximo é 200.

O parâmetro `offset` é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado em conjunto com `maxReturn`. O padrão é 0.

O parâmetro `isActive` é um booliano que especifica o retorno de apenas campanhas de Gatilho ativas.

```
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

Com este ponto de extremidade, haverá um ou mais registros na matriz `result`.

## Criar

O ponto de extremidade [Criar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) é executado com um POST aplicativo/x-www-form-urlencoded com dois parâmetros obrigatórios. O parâmetro `name` especifica o nome da campanha inteligente a ser criada. O parâmetro `folder` especifica a pasta principal onde a campanha inteligente é criada. O formato é um bloco JSON contendo `id` e `type` atributos.

Como opção, você pode descrever a campanha inteligente usando o parâmetro `description` (máximo de 2.000 caracteres).

```
POST /rest/asset/v1/smartCampaigns.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O ponto de extremidade [Atualizar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/) é executado com um POST application/x-www-form-urlencoded. Uma única campanha inteligente `id` é necessária como parâmetro de caminho. Você pode usar o parâmetro `name` para atualizar o nome da campanha inteligente ou o parâmetro `description` para atualizar a descrição da campanha inteligente.

```
POST /rest/asset/v1/smartCampaign/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O ponto de extremidade [Clone Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) é executado com um POST application/x-www-form-urlencoded com três parâmetros obrigatórios. É necessário um parâmetro `id` que especifica a campanha inteligente a ser clonada, um parâmetro `name` que especifica o nome da nova campanha inteligente e um parâmetro `folder` para especificar a pasta principal onde a nova campanha inteligente é criada. O formato é um bloco JSON contendo `id` e `type` atributos.

Como opção, você pode descrever a campanha inteligente usando o parâmetro `description` (máximo de 2.000 caracteres).

```
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

O ponto de extremidade [Excluir Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) usa uma única campanha inteligente `id` como parâmetro de caminho.

```
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

As campanhas inteligentes em lote são iniciadas em um horário específico e afetam um conjunto específico de clientes potenciais de uma só vez.

## Agendar

Use o ponto de extremidade [Agendar Campanha](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) para agendar uma campanha em lote para execução imediata ou em uma data futura. A campanha `id` é um parâmetro de caminho necessário. Os parâmetros opcionais são `tokens`, `runAt` e `cloneToProgram` que são passados no corpo da solicitação como application/json.

O parâmetro de matriz de tokens é uma matriz de Meus tokens que substitui os tokens de programa existentes. Após a execução da campanha, os tokens são descartados.  Cada item da matriz de token contém pares de nome/valor. O nome do token deve ser formatado como &quot;{{my.name}}&quot;.

O parâmetro datetime runAt especifica quando executar a campanha. Se não especificado, a campanha será executada 5 minutos após o endpoint ser chamado. O valor datetime não pode ser posterior a dois anos.

As campanhas programadas por meio dessa API sempre aguardam no mínimo cinco minutos antes de serem executadas.

O parâmetro da cadeia de caracteres `cloneToProgram` contém o nome de um programa resultante.  Quando definido, faz com que a campanha, o programa principal e todos os seus ativos sejam criados com o novo nome resultante. O programa principal é clonado e a campanha recém-criada será agendada. O programa resultante é criado abaixo do pai. Programas com trechos, notificações por push, mensagens no aplicativo, listas estáticas, relatórios e ativos sociais não podem ser clonados dessa maneira. Quando usado, esse endpoint é limitado a 20 chamadas por dia. O ponto de extremidade do [programa clone](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) é a alternativa recomendada.

```
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

## Gatilho

Os acionadores de campanhas inteligentes afetam uma pessoa de cada vez com base em um evento acionado.

### Solicitar

Use o ponto de extremidade [Solicitar Campanha](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) para passar um conjunto de clientes em potencial para uma campanha de acionador para executar pelo fluxo da campanha. A campanha deve ter um acionador &quot;Campanha solicitada&quot; com &quot;API de serviço da Web&quot; como a origem.

Este ponto de extremidade requer uma campanha `id` como parâmetro de caminho e um parâmetro de matriz de inteiros `leads` contendo ids de cliente potencial. É permitido um máximo de 100 clientes em potencial por chamada.

Opcionalmente, o parâmetro de matriz `tokens` pode ser usado para substituir Meus Tokens locais no programa pai da campanha. `tokens` aceita no máximo 100 tokens. Cada item da matriz `tokens` contém um par nome/valor. O nome do token deve ser formatado como &quot;{{my.name}}&quot;. Se você usar [Adicionar um Token do Sistema como um Link em uma abordagem de Email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) para adicionar o token do sistema &quot;viewAsWebpageLink&quot;, não será possível substituí-lo usando `tokens`. Em vez disso, use [Adicionar uma Exibição como Link da Página da Web para uma abordagem de Email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) que permite substituir &quot;viewAsWebPageLink&quot; usando `tokens`.

Os parâmetros `leads` e `tokens` são passados no corpo da solicitação como application/json.

```
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

O ponto de extremidade [Ativar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) é simples. Um parâmetro de caminho `id` é necessário. Para que a ativação seja bem-sucedida, o seguinte deve ser verdadeiro para a campanha:

- Deve ser desativado
- Deve ter pelo menos um acionador e uma etapa de fluxo
- Deve ter acionadores, filtros e etapas de fluxo sem erros

```
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

A opção [Desativar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) é simples. Um parâmetro de caminho `id` é necessário. Para que a desativação seja bem-sucedida, a campanha deve ser ativada.

```
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
