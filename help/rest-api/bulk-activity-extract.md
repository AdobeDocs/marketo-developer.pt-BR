---
title: Extração de atividade em massa
feature: REST API
description: API REST de extração de atividade em massa do Marketo para exportar dados de atividade em alto volume usando um intervalo de datas de 31 dias, atividade e filtros de atributo principal para ETL e CRM.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
TQID: https://experienceleague.adobe.com/lIlXNjatN-F77Dv3xsVkQ3hAWwLZ4wlSW0zKNkFJFMA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1268
ht-degree: 7%

---

# Extração de atividade em massa

[Referência de Ponto de Extremidade de Extração de Atividade em Massa](https://developer.adobe.com/marketo-apis/api/mapi)

As APIs REST de extração de atividade em massa recuperam grandes volumes de dados de atividade do Marketo. Use essas APIs para processos que não exigem baixa latência, como integração de CRM, ETL, data warehouse e arquivamento de dados.

## Permissões

O usuário da API deve ter a permissão &quot;Atividade somente leitura&quot; ou &quot;Atividade de leitura e gravação&quot;.

## Filtros

| Tipo de filtro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| `createdAt` | Date Range | Sim | Um objeto JSON que contém `startAt` e `endAt`. `startAt` é o datetime de marca d&#39;água baixa e `endAt` é o datetime de marca d&#39;água alta. O intervalo deve ser de 31 dias ou menos. A tarefa retorna todos os registros acessíveis criados dentro do intervalo de datas. Use valores datetime ISO-8601 sem milissegundos. |
| `activityTypeIds` | Matriz\[Inteiro\] | Não | Uma matriz de números inteiros para os tipos de atividade solicitados. A atividade &quot;Excluir cliente em potencial&quot; não é compatível. Em vez disso, use o ponto de extremidade [Obter clientes em potencial excluídos](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). Recupere as IDs de tipo de atividade com o [ponto de extremidade Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [`primaryAttributeValueIds`](#primaryattributevalueids-options) | Matriz\[Inteiro\] | Não | Uma matriz que aceita no máximo 50 ids para atributos primários. Cada ID identifica exclusivamente um campo ou ativo de cliente potencial. Recupere as IDs chamando o ponto de extremidade da API REST apropriado. Por exemplo, para filtrar em um Formulário específico para a atividade &quot;Preencher Formulário&quot;, passe o nome do Formulário para o ponto de extremidade [Obter Formulário por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) para recuperar a ID do Formulário. Consulte [opções de primaryAttributeValueIds](#primaryattributevalueids-options) para obter os tipos de atividades compatíveis. |
| [`primaryAttributeValues`](#primaryattributevalues-options) | Matriz\[Cadeia\] | Não | Uma matriz que aceita no máximo 50 nomes para atributos primários. Cada nome identifica exclusivamente um campo ou ativo de cliente potencial. Recupere nomes chamando o endpoint da API REST apropriado. Por exemplo, para filtrar em um Formulário específico para a atividade &quot;Preencher Formulário&quot;, passe a ID do Formulário para o ponto de extremidade [Obter Formulário por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) para recuperar o nome do Formulário. Consulte [opções de primaryAttributeValues](#primaryattributevalues-options) para obter os tipos de atividades compatíveis. |

### opções de primaryAttributeValueIds {#primaryattributevalueids-options}

| Tipo de atividade | Id do Valor do Atributo Principal | Ponto de Extremidade de Recuperação | Grupo de ativos |
| --- | --- | --- | --- |
| Alterar valor dos dados | ID do campo de cliente potencial | [Descrever cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome do atributo |
| Alterar pontuação | ID do campo de cliente potencial | [Descrever cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome do atributo |
| Alteração do status na progressão | ID do programa | [Obter Programa por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) | Programa de marketing |
| Adicionar à lista | ID da lista estática | [Obter Lista Estática por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Remover da lista | ID da lista estática | [Obter Lista Estática por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Preenchimento de formulário | ID do formulário | [Obter Formulário por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) | Formulário da Web |

Ao usar `primaryAttributeValueIds`, você também deve incluir o filtro `activityTypeIds`. Este filtro pode conter somente IDs de atividade que correspondam ao grupo de ativos correspondente. Por exemplo, ao filtrar ativos de Formulários da Web, `activityTypeIds` pode conter somente a ID de tipo de atividade &quot;Preencher Formulário&quot;.

A solicitação a seguir inclui o filtro `primaryAttributeValueIds`:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValueIds": [
      16,102,95,8
    ]
  }
}
```

`primaryAttributeValueIds` e `primaryAttributeValues` não podem ser usados juntos.

### opções de primaryAttributeValues {#primaryattributevalues-options}

| Tipo de atividade | Valor do atributo primário | Ponto de Extremidade de Recuperação | Grupo de ativos |
| --- | --- | --- | --- |
| Alterar valor dos dados | Nome para exibição do campo de cliente potencial | [Descrever cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome do atributo |
| Alterar pontuação | Nome para exibição do campo de cliente potencial | [Descrever cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome do atributo |
| Alteração do status na progressão | Nome do programa | [Obter Programa por Id](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) | Programa de marketing |
| Adicionar à lista | Nome da lista estática | [Obter Lista Estática por Id](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Lista estática |
| Remover da lista | Nome da lista estática | [Obter Lista Estática por Id](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Lista estática |
| Preenchimento de formulário | Nome do formulário | [Obter formulário por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) | Formulário da Web |

Use a notação `&lt;program&gt;.&lt;asset&gt;` para especificar nomes para os grupos de ativos Programa de marketing, Lista estática e Formulário web. Por exemplo, especifique o form &quot;Saída MPS&quot; no programa &quot;GL_OP_ALL_2021&quot; como &quot;GL_OP_ALL_2021.MPS de Saída&quot;.

A solicitação a seguir inclui o filtro `primaryAttributeValues`:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValues": [
      "GL_OP_ALL_2021.MPS Outbound"
    ]
  }
}
```

Ao usar `primaryAttributeValues`, você também deve incluir o filtro `activityTypeIds`. Este filtro pode conter somente IDs de atividade que correspondam ao grupo de ativos correspondente. Por exemplo, ao filtrar ativos de Formulários da Web, `activityTypeIds` pode conter somente a ID de tipo de atividade &quot;Preencher Formulário&quot;.

`primaryAttributeValues` e `primaryAttributeValueIds` não podem ser usados juntos.

## Opções

| Parâmetro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| `filter` | Objeto | Sim | Um objeto que contém filtros que se aplicam ao conjunto de atividades acessível. Incluir exatamente um filtro `createdAt`. Você também pode incluir um filtro `activityTypeIds`. O trabalho de exportação retorna o conjunto de atividades resultante. |
| `format` | String | Não | O formato do arquivo de exportação: CSV, TSV ou SSV. Esses valores produzem valores separados por vírgula, tabulação ou espaço, respectivamente. O padrão é CSV. |
| `columnHeaderNames` | Objeto | Não | Um objeto JSON de pares de valores-chave de campo e cabeçalho de coluna. Cada chave deve nomear um campo incluído no trabalho de exportação. Seu valor define o cabeçalho de coluna exportado para esse campo. |
| `fields` | Matriz\[Cadeia\] | Não | Uma matriz de campos a serem incluídos no arquivo de exportação. Por padrão, a resposta inclui `marketoGUID`, `leadId`, `activityDate`, `activityTypeId`, `campaignId`, `primaryAttributeValueId`, `primaryAttributeValue` e `attributes`. Para retornar um subconjunto, especifique os campos desta lista, como `"fields": ["leadId", "activityDate", "activityTypeId"]`. Você também pode especificar `actionResult` para incluir a ação da atividade: `("succeeded", "skipped", or "failed")`. |

## Criação de um trabalho

Crie um trabalho de exportação para definir os registros a serem recuperados. Use o ponto de extremidade [Criar Trabalho de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).

Todo trabalho requer um filtro `createdAt`. Seus parâmetros de datetime `startAt` e `endAt` definem as datas de criação de atividade mais antigas e mais recentes permitidas. Para excluir tipos de atividades que não são relevantes, inclua também o filtro `activityTypeIds` opcional.

A solicitação a seguir cria um trabalho de exportação de CSV para tipos de atividades selecionadas em um intervalo de datas:

```http
POST /bulk/v1/activities/export/create.json
```

```json
{
   "format": "CSV",
   "filter": {
      "createdAt": {
         "startAt": "2017-07-01T23:59:59-00:00",
         "endAt": "2017-07-31T23:59:59-00:00"
      },
      "activityTypeIds": [
         1,
         12,
         13
      ]
   }
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

A resposta retorna um `exportId` e um status de &quot;Criado&quot;. Uma tarefa criada ainda não está na fila de processamento.

Para adicionar o trabalho à fila, chame o ponto de extremidade [Enfileirar Trabalho de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) com o `exportId` da resposta de criação.

```http
POST /bulk/v1/activities/export/{exportId}/enqueue.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Queued",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

O status da resposta agora é &quot;Em fila&quot;. Quando um trabalhador se torna disponível, o status muda para &quot;Processando&quot; e o trabalho começa a agregar registros do Marketo.

## Status do trabalho de pesquisa

O status do trabalho só pode ser recuperado para trabalhos criados pelo mesmo usuário da API.

A Extração de atividade em massa processa trabalhos de forma assíncrona. Sonde o ponto de extremidade [Obter Status do Trabalho da Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) para determinar quando um trabalho é concluído:

```http
GET /bulk/v1/activities/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 15423,
         "fileSize": 12342,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
   ]
}
```

O campo `status` retorna um dos seguintes valores:

- `Created`
- `Queued`
- `Processing`
- `Canceled`
- `Completed`
- `Failed`

## Recuperação de dados

Quando o status do trabalho for &quot;Concluído&quot;, recupere os dados exportados com o ponto de extremidade [Obter Arquivo de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET):

```http
GET /bulk/v1/activities/export/{exportId}/file.json
```

O corpo da resposta contém o arquivo no formato configurado para o trabalho.

Se um campo de atividade solicitado não contiver dados, `null` aparecerá no campo de arquivo de exportação correspondente. O exemplo a seguir mostra dados de atividade exportados:

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Para recuperação parcial ou retomável, o ponto de extremidade do arquivo dá suporte ao cabeçalho HTTP `Range` opcional com um intervalo `bytes`. Se você omitir esse cabeçalho, o endpoint retornará o arquivo inteiro. Para obter mais informações sobre como usar o cabeçalho `Range`, consulte [Extração em Massa](bulk-extract.md).

## Cancelar um trabalho

Para parar um trabalho desnecessário ou configurado incorretamente, chame o ponto de extremidade [Cancelar Trabalho da Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

```http
POST /bulk/v1/activities/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

O status da resposta indica que a tarefa foi cancelada.
