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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 1564
ht-degree: 6%

---

# Extração de atividade em massa

[Referência de Ponto de Extremidade de Extração de Atividade em Massa](https://developer.adobe.com/marketo-apis/api/mapi)

O conjunto de Extração de atividade em massa de APIs REST fornece uma interface programática para recuperar grandes quantidades de dados de atividade do Marketo.  Para casos que não exigem baixa latência e devem transferir volumes significativos de dados de atividades para fora do Marketo, como integração de CRM, ETL, data warehouse e arquivamento de dados.

## Permissões

As APIs de Extração de atividade em massa exigem que o usuário da API tenha as permissões de &quot;Atividade somente leitura&quot; ou &quot;Atividade de leitura e gravação&quot;.

## Filtros

| Tipo de filtro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| `createdAt` | Date Range | Sim | Aceita um objeto JSON com os membros `startAt` e `endAt`. `startAt` aceita um datetime que representa a marca d&#39;água inferior e `endAt` aceita um datetime que representa a marca d&#39;água superior. O intervalo deve ser de 31 dias ou menos. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis criados dentro do intervalo de datas. Os datetimes devem estar em um formato ISO-8601, sem milissegundos. |
| `activityTypeIds` | Matriz\[Inteiro\] | Não | Aceita um objeto JSON com um membro, `activityTypeIds`. O valor deve ser uma matriz de números inteiros, correspondentes aos tipos de atividade desejados. A atividade &quot;Excluir Cliente Potencial&quot; não é suportada (use o ponto de extremidade [Obter Clientes Potenciais Excluídos](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET)). Recupere as IDs de tipo de atividade usando o [Ponto de extremidade Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [`primaryAttributeValueIds`](#primaryattributevalueids-options) | Matriz\[Inteiro\] | Não | Aceita um objeto JSON com um membro, `primaryAttributeValueIds`. O valor é uma matriz de IDs que especifica os atributos primários para filtrar. É possível especificar no máximo 50 ids. As IDs são o identificador exclusivo de um campo de cliente potencial ou de um ativo, e podem ser recuperadas chamando o endpoint da API REST apropriado. Por exemplo, para filtrar em um Formulário específico para a atividade &quot;Preencher Formulário&quot;, passe o nome do Formulário para o ponto de extremidade [Obter Formulário por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) para recuperar a ID do Formulário. Veja a seguir uma lista de tipos de atividades em que a filtragem de atributos primários é suportada. |
| [`primaryAttributeValues`](#primaryattributevalues-options) | Matriz\[Cadeia\] | Não | Aceita um objeto JSON com um membro, `primaryAttributeValues`. O valor é uma matriz de nomes que especifica os atributos primários para filtrar. É possível especificar no máximo 50 nomes. Os nomes são o identificador exclusivo de um campo de cliente potencial ou de um ativo, e podem ser recuperados chamando o endpoint da API REST apropriado. Por exemplo, para filtrar em um Formulário específico para a atividade &quot;Preencher Formulário&quot;, passe a ID do Formulário para o ponto de extremidade [Obter Formulário por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) para recuperar o nome do Formulário. Veja a seguir uma lista de tipos de atividades em que a filtragem de atributos primários é suportada. |

### opções de primaryAttributeValueIds {#primaryattributevalueids-options}

| Tipo de atividade | Id do Valor do Atributo Principal | Ponto de Extremidade de Recuperação | Grupo de ativos |
| --- | --- | --- | --- |
| Alterar valor dos dados | ID do campo de cliente potencial | [Descrever cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome do atributo |
| Alterar pontuação | ID do campo de cliente potencial | [Descrever cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome do atributo |
| Alteração do status na progressão | ID do programa | [Obter Programa por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) | Programa de marketing |
| Adicionar à lista | ID da lista estática | [Obter Lista Estática por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Remover da lista | ID da lista estática | [Obter Lista Estática por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Preenchimento de formulário | ID do formulário | [Obter Formulário por Nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) | Formulário da Web |

Ao usar `primaryAttributeValueIds`, o filtro `activityTypeIds` deve estar presente e conter somente IDs de atividade que correspondam ao grupo de ativos correspondente. Por exemplo, se você estiver filtrando ativos de Formulários da Web, somente a ID de tipo de atividade &quot;Preencher Formulário&quot; será permitida em `activityTypeIds`.

Exemplo de corpo da solicitação:

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
| Preenchimento de formulário | Nome do formulário | [Obter formulário por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) | Formulário da Web |

Observe que você deve usar a notação `&lt;program&gt;.&lt;asset&gt;` para especificar o nome dos seguintes grupos de ativos: Programa de marketing, Lista estática, Formulário web. Por exemplo, um formulário com o nome &quot;Saída MPS&quot; que reside abaixo de um programa com o nome &quot;GL_OP_ALL_2021&quot; seria especificado como &quot;GL_OP_ALL_2021.Saída MPS&quot;.

Exemplo de corpo da solicitação:

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

Ao usar `primaryAttributeValues`, o filtro `activityTypeIds` deve estar presente e conter somente IDs de atividade que correspondam ao grupo de ativos correspondente. Por exemplo, se você estiver filtrando ativos de Formulários da Web, somente a ID de tipo de atividade &quot;Preencher Formulário&quot; será permitida em `activityTypeIds`. `primaryAttributeValues` e `primaryAttributeValueIds` não podem ser usados juntos.

## Opções

| Parâmetro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| `filter` | Matriz\[Objeto\] | Sim | Aceita uma matriz de filtros. Exatamente um filtro `createdAt` deve ser incluído na matriz. Um filtro `activityTypeIds` opcional pode ser incluído. Os filtros são aplicados ao conjunto de atividades acessível e o conjunto de atividades resultante é retornado pelo trabalho de exportação. |
| `format` | String | Não | Aceita um dos seguintes: CSV, TSV, SSV. O arquivo exportado é renderizado como um arquivo de valores separados por vírgula, valores separados por tabulação ou valores separados por espaço, respectivamente, se definido. O padrão é CSV, caso não esteja definido. |
| `columnHeaderNames` | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| `fields` | Matriz\[Cadeia\] | Não | Matriz opcional de cadeias de caracteres que contêm valores de campo. Os campos listados são incluídos no arquivo exportado. Por padrão, os seguintes campos são retornados: `marketoGUID`, `leadId`, `activityDate`, `activityTypeId`, `campaignId`, `primaryAttributeValueId`, `primaryAttributeValue` e `attributes`. Esse parâmetro pode ser usado para reduzir o número de campos retornados especificando um subconjunto da lista acima: `"fields": ["leadId", "activityDate", "activityTypeId"]`. Um campo adicional `actionResult` pode ser especificado para incluir a ação da atividade: `("succeeded", "skipped", or "failed")`. |

## Criação de um trabalho

Para exportar registros, primeiro defina o trabalho e o conjunto de registros que deseja recuperar.  Crie o trabalho usando o ponto de extremidade [Criar Trabalho de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).  Ao exportar atividades, há dois filtros principais que podem ser aplicados: `createdAt`, que é sempre obrigatório, e `activityTypeIds`, que é opcional.  O filtro `createdAt` é usado para definir um intervalo de datas em que as atividades foram criadas, usando os parâmetros `startAt` e `endAt`, que são campos de data e hora, e representam a data de criação mais antiga permitida e a data de criação mais recente permitida, respectivamente.  Você também pode filtrar apenas alguns tipos de atividades, usando o filtro `activityTypeIds`.  Isso é útil para remover resultados que não são relevantes para seu caso de uso.

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

A tarefa agora tem um status de &quot;Criada&quot;, mas ainda não está na fila de processamento.  Para colocá-lo na fila para que ele possa começar a ser processado, chame o ponto de extremidade [Enfileirar Trabalho de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) usando a exportId da resposta de status de criação.

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

Agora, o status é informando que a tarefa foi colocada em fila.  Quando um trabalhador se torna disponível para esse trabalho, o status é alternado para &quot;Processando&quot; e o trabalho começa a agregar registros do Marketo.

## Status do trabalho de pesquisa

O status do trabalho só pode ser recuperado para trabalhos criados pelo mesmo usuário da API.

A Extração de atividade em massa do Marketo é um endpoint assíncrono, portanto, o status do trabalho deve ser sondado para determinar quando o trabalho é concluído.  Consulte usando o ponto de extremidade [Obter Status do Trabalho da Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) da seguinte maneira:

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

O campo de status pode responder com um dos seguintes valores:

- Criado
- Enfileirado
- Processamento
- Cancelado
- Concluído
- Falha

## Recuperação de dados

Quando o trabalho for concluído, recupere seus dados usando o ponto de extremidade [Obter Arquivo de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET).

```http
GET /bulk/v1/activities/export/{exportId}/file.json
```

A resposta contém um arquivo formatado da maneira que o trabalho foi configurado. O endpoint responde com o conteúdo do arquivo.

Se um campo de cliente em potencial solicitado estiver vazio (não contiver dados), `then null` será colocado no campo correspondente no arquivo de exportação.  No exemplo abaixo, o campo `campaignId` da atividade retornada está vazio.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Para oferecer suporte à recuperação parcial e de fácil retomada de dados extraídos, o ponto de extremidade do arquivo oferece suporte opcionalmente ao cabeçalho HTTP `Range` do tipo `bytes`.  Se o cabeçalho não estiver definido, todo o conteúdo será retornado.  Você pode ler mais sobre como usar o cabeçalho Intervalo com a [Extração em massa](bulk-extract.md) do Marketo.

## Cancelar um trabalho

Se um trabalho for configurado incorretamente ou se se tornar desnecessário, ele poderá ser facilmente cancelado usando o ponto de extremidade [Cancelar Trabalho da Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

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

Essa resposta tem um status que indica que o trabalho foi cancelado.
