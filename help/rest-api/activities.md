---
title: Atividades
feature: REST API
description: Use a API REST de atividades do Marketo Engage para listar tipos de atividades, buscar atividades principais com tokens de paginação e lidar com alterações personalizadas e de valores de dados.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
TQID: https://experienceleague.adobe.com/62keaj4uNoxIPCzr9AQzKrIsfuHBvC25knYisZRUvF4
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1758
ht-degree: 0%

---

# Atividades

O Marketo oferece suporte a vários tipos de atividades relacionadas a registros de clientes potenciais. Quase todas as etapas de alteração, ação ou fluxo são registradas no log de atividades de um lead. Você pode recuperar essas atividades por meio da API ou usá-las em filtros e acionadores de Smart List e Smart Campaign.

Cada atividade tem um `id` exclusivo e se conecta a um registro de cliente potencial por meio de `leadId`, que corresponde ao campo de ID do registro. Toda atividade também tem um `activityDate`.

Os tipos de atividades disponíveis variam de acordo com a assinatura e cada tipo tem sua própria definição. O significado de `primaryAttributeValueId` e `primaryAttributeValue` depende do tipo de atividade.

Use a API de metadados de atividades personalizadas para criar Tipos de atividade personalizados. Use a API Adicionar atividades personalizadas para adicionar registros de atividades personalizadas.

A maioria das atividades será removida após algum período.

## Descrever

Use o ponto de extremidade [Obter Tipos de Atividade](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) para recuperar os tipos de atividade disponíveis e suas definições para uma instância.

```
GET /rest/v1/activities/types.json
```

```json
  "requestId": "6e78#148ad3b76f1",
  "success": true,
  "result": [
    {
      "id": 2,
      "name": "Fill Out Form",
      "description": "User fills out and submits form on web page",
      "primaryAttribute": {
        "name": "Webform ID",
        "dataType": "integer"
      },
      "attributes": [
        {
          "name": "Client IP Address",
          "dataType": "string"
        },
        {
          "name": "Form Fields",
          "dataType": "text"
        },
        {
          "name": "Query Parameters",
          "dataType": "string"
        },
        {
          "name": "Referrer URL",
          "dataType": "string"
        },
        {
          "name": "User Agent",
          "dataType": "string"
        },
        {
          "name": "Webpage ID",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

As respostas reais incluem mais definições. Este exemplo mostra o tipo de atividade &quot;Preencher formulário&quot;. Seu atributo principal, &quot;ID de formulário da Web&quot;, se refere à Marketo ID do formulário enviado e vincula a atividade a esse ativo.

A resposta também define cada atributo possível para o tipo de atividade e seu tipo de dados. Se um campo estiver vazio, esse atributo será omitido do registro de atividade individual.

## Consultar

Use o ponto de extremidade [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) para recuperar atividades. Primeiro, recupere um token de paginação para a data e hora em que a recuperação da atividade deve começar. Passe esse token no parâmetro de consulta `nextPageToken`.

Transmita até dez IDs de tipo de atividade como uma lista separada por vírgulas no parâmetro de consulta `activityTypeIds`.

Opcionalmente, restrinja a consulta a um destes parâmetros:

- `listId` limita os resultados a registros em uma lista estática específica.
- `leadIds` limita os resultados a atividades para até 30 clientes potenciais, fornecidos em uma lista separada por vírgulas.

>[!CAUTION]
>
>A partir de 30/12/2026, as chamadas para os pontos de extremidade `Get Lead Activities` e `Get Lead Changes` que incluem o parâmetro `listId` falharão (código de erro 1003) se as listas de destino contiverem 10.000 ou mais clientes potenciais. Para evitar interrupções do serviço, verifique se as chamadas têm o escopo correto para evitar esse limite. Consulte o [Guia de migração](migration.md).

```
GET /rest/v1/activities.json?activityTypeIds=1&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3I3LCWXH3Y6IIZ7YSGQLXHCPVE5Q====
```

```json
{
  "requestId": "24fd#15188a88d7f",
  "result": [
    {
      "id": 102988,
      "marketoGUID": "102988",
      "leadId": 1,
      "activityDate": "2023-01-16T23:32:19Z",
      "activityTypeId": 1,
      "primaryAttributeValueId": 71,
      "primaryAttributeValue": "localhost/munchkintest2.html",
      "attributes": [
        {
          "name": "Client IP Address",
          "value": "10.0.19.252"
        },
        {
          "name": "Query Parameters",
          "value": ""
        },
        {
          "name": "Referrer URL",
          "value": ""
        },
        {
          "name": "User Agent",
          "value": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
        },
        {
          "name": "Webpage URL",
          "value": "/munchkintest2.html"
        }
      ]
    }
  ],
  "success": true,
  "nextPageToken": "WQV2VQVPPCKHC6AQYVK7JDSA3J62DUSJ3EXJGDPTKPEBFW3SAVUA====",
  "moreResult": false
}
```

Para a primeira chamada, use a API Obter Token de Paginação para obter `nextPageToken`. Para cada chamada subsequente, passe o `nextPageToken` retornado pela resposta anterior. Este ponto de extremidade sempre retorna `nextPageToken`.

Se `moreResult` for verdadeiro, mais resultados estarão disponíveis. Continue chamando o ponto de extremidade com o `nextPageToken` retornado até que `moreResult` seja falso.

A API pode retornar menos de 300 itens de atividade ao configurar `moreResult` como verdadeiro. Nesse caso, inclua o `nextPageToken` retornado em outra chamada para recuperar atividades mais recentes.

Em cada item da matriz de resultados, o atributo de cadeia de caracteres `marketoGUID` substitui o atributo inteiro `id` como identificador exclusivo.

### Alterações no valor dos dados

Use o ponto de extremidade [Obter Alterações de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) para recuperar registros de Alteração de Valor de Dados para campos de cliente potencial. Sua interface difere da API Obter atividades de lead de duas maneiras:

- O ponto de extremidade não tem parâmetro `activityTypeIds` porque retorna somente a Alteração do Valor dos Dados e novas atividades de cliente potencial.
- O parâmetro de consulta `fields` necessário aceita uma lista de campos separada por vírgulas cujas alterações você deseja recuperar.

>[!CAUTION]
>
>A partir de 30/12/2026, as chamadas para os pontos de extremidade `Get Lead Activities` e `Get Lead Changes` que incluem o parâmetro `listId` falharão (código de erro 1003) se as listas de destino contiverem 10.000 ou mais clientes potenciais. Para evitar interrupções do serviço, verifique se as chamadas têm o escopo correto para evitar esse limite. Consulte o [Guia de migração](migration.md).

```http
GET /rest/v1/activities/leadchanges.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&fields=firstName,lastName,department
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 1078,
      "marketoGUID": "1078",
      "leadId": 775,
      "activityDate": "2014-09-17T22:31:49+0000",
      "activityTypeId": 13,
      "fields": [
        {
          "id": 48,
          "name": "firstName",
          "newValue": "FirstName_6176",
          "oldValue": "FirstName_4914"
        }
      ],
      "attributes": [
        {
          "name": "Reason",
          "value": "Web service API"
        },
        {
          "name": "Source",
          "value": "Web service API"
        },
        {
          "name": "Lead ID",
          "value": 775
        }
      ]
    }
  ]
}
```

Cada atividade na resposta tem uma matriz de campos que lista suas alterações. Cada alteração especifica os `id` e `name` do campo, juntamente com os valores novos e antigos.

Em cada item da matriz de resultados, o atributo de cadeia de caracteres `marketoGUID` substitui o atributo inteiro `id` como identificador exclusivo.

### Clientes potenciais excluídos

Use o ponto de extremidade [Obter Clientes Potenciais Excluídos](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET) para recuperar atividades de cliente potencial excluídas da Marketo.

```http
GET /rest/v1/activities/deletedleads.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 2,
      "marketoGUID": "2",
      "leadId": 6,
      "activityDate": "2013-09-26T06:56:35+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 6,
      "primaryAttributeValue": "Owyliphys Iledil",
      "attributes": []
    },
    {
      "id": 3,
      "marketoGUID": "3",
      "leadId": 9,
      "activityDate": "2013-12-28T00:39:45+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 4,
      "primaryAttributeValue": "First Last",
      "attributes": []
    }
  ]
}
```

Em cada item da matriz de resultados, o atributo de cadeia de caracteres `marketoGUID` substitui o atributo inteiro `id` como identificador exclusivo.

### Página pelos resultados

Por padrão, os endpoints nesta seção retornam 300 itens de atividade de cada vez. Se `moreResult` for verdadeiro, mais resultados estarão disponíveis. Passar o `nextPageToken` retornado em cada chamada subsequente até `moreResult` ser falso.

Um ponto de extremidade pode retornar menos de 300 itens de atividade ao configurar `moreResult` como verdadeiro. Nesse caso, inclua o `nextPageToken` retornado em outra chamada para recuperar atividades mais recentes. Codificação de URL `nextPageToken` na solicitação.

## Tipos de atividades personalizadas

As Atividades personalizadas funcionam como atividades padrão, mas terceiros gerenciam seus esquemas. Os registros de atividade personalizados são vinculados aos registros de clientes potenciais por meio de `leadId`, e seus atributos primário e secundário são definidos pelo usuário.

Quando um tipo de atividade personalizada é aprovado, o Marketo cria um acionador e filtro de Smart List correspondente. Em seguida, você pode processar clientes em potencial com base nos dados de atividade personalizados atuais ou históricos.

- Máximo de atividades personalizadas: 10
- Máximo de atributos por atividade personalizada: 20

Recupere dados de atividades personalizadas por meio da API [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), da mesma forma que recupera atividades padrão.

## Tipos de consulta

Use [Obter Tipos de Atividade Personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getCustomActivityTypeUsingGET) para recuperar detalhes sobre os tipos provisionados em uma instância do Marketo. Use [Descrever Tipo de Atividade Personalizada](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/describeCustomActivityTypeUsingGET) para recuperar metadados de atributo para um tipo específico.

O ponto de extremidade padrão [Obter Tipos de Atividade](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) também retorna metadados de atividade personalizados, mas não identifica se um tipo é personalizado.

### Obter tipos

```http
GET /rest/v1/activities/external/types.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved"
    }
  ]
}
```

### Descrever tipos

Para descrever um tipo, passe `apiName` como um parâmetro de caminho. Por padrão, o endpoint retorna a versão aprovada da atividade. Para recuperar a versão de rascunho, passe o parâmetro `draft=true` opcional.

```http
GET /rest/v1/activities/external/type/{apiName}/describe.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

## Criar tipo

Cada tipo de atividade personalizado requer um nome de exibição, nome da API, nome do acionador, nome do filtro e atributo principal. Use as diretrizes a seguir para manter os tipos consistentes com as convenções do Marketo e evitar colisões de nomes:

- **Nome para Exibição:** Descreva brevemente o que um registro de atividade representa, como &quot;Enviar Email&quot; ou &quot;Alterar Valor dos Dados&quot;. Use o formulário infinitivo, como &quot;Participar do evento&quot;. Os nomes de exibição aceitam caracteres alfanuméricos, espaços e sublinhados e devem conter pelo menos uma letra.

- **Nome da API:** Use caracteres alfanuméricos, com um comprimento máximo de 255. Se você for um parceiro do LaunchPoint, inclua um namespace representativo nos nomes da API do tipo de atividade para evitar colisões com os tipos provisionados pelo cliente. Use minúsculas ou camelCase para distinguir nomes de API de outras strings.

- **Descrição:** Para atividades com comportamento não óbvio, explique o que o tipo de atividade representa em relação ao cliente potencial.

- **Nome do Acionador:** Forneça um nome exclusivo e legível no tempo presente de terceira pessoa, como &quot;Participa de um Evento&quot;. Os parceiros do LaunchPoint devem incluir o nome da empresa, como &quot;Participa do webinário - Acme Company&quot;.

- **Nome do Filtro:** Forneça um nome exclusivo e legível no pretérito de terceiros, como &quot;Participou de um Evento&quot;. Os parceiros do LaunchPoint devem incluir o nome da empresa, como &quot;Webinário assistido - Acme Company&quot;.

- **Atributo principal:** selecione o campo mais significativo para o tipo de atividade. Para uma atividade &quot;Evento assistido&quot;, esse campo é o nome do evento. O atributo primário é exibido por padrão como um parâmetro em cada acionador ou filtro do tipo de atividade. Seu valor também aparece no registro de atividades de uma pessoa sem exigir detalhamento na atividade.

Um novo tipo de atividade personalizado é criado como rascunho. Aprove o tipo antes de adicionar registros de atividade desse tipo. As atualizações se aplicam à versão de rascunho e devem ser aprovadas antes de aparecerem na versão ao vivo. Depois que um tipo de atividade personalizado é aprovado e está em uso, os campos anteriores não podem ser alterados.

Ao criar um tipo, o parâmetro de descrição é opcional. Os parâmetros necessários são `apiName`, `name`, `triggerName`, `filterName` e `primaryAttribute`.

```http
POST /rest/v1/activities/external/type.json
```

```json
{
  "apiName": "attendConference",
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attends Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Tipo de atualização

Para atualizar um tipo, passe o apiName necessário como um parâmetro de caminho. Outros campos podem ser fornecidos no corpo da solicitação.

```http
POST /rest/v1/activities/external/type/{apiName}.json
```

```json
{
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attend Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Aprovar tipo

Gerencie tipos com Aprovar tipo de atividade personalizado, Descartar rascunho do tipo de atividade personalizado e Excluir tipo de atividade personalizado, como você faria com os ativos padrão do Marketo.

## Atributos de tipo de atividade personalizados

Cada tipo de atividade personalizada pode ter de 0 a 20 atributos secundários. Um atributo secundário pode usar qualquer tipo de campo Marketo válido. Adicionar, atualizar e remover atributos secundários separadamente do tipo pai.

É possível editar atributos enquanto um tipo de atividade está em uso e, em seguida, aprovar as alterações. As atividades criadas após a aprovação usam o novo conjunto de atributos secundário. As alterações não se aplicam retroativamente a atividades existentes desse tipo.

A remoção de atributos também remove sua disponibilidade nos filtros correspondentes.

As atualizações na lista de atributos secundária usam cada nome de API do atributo como a chave primária. Para alterar um Nome de API, exclua o atributo e adicione-o novamente com o nome de API desejado.

Os tipos de dados válidos para atributos são: string, boolean, integer, float, link, email, currency, date, datetime, phone, text.

Antes de alterar o atributo primário de um tipo de atividade, rebaixe o atributo primário existente definindo `isPrimary` como falso.

### Criar atributos

Para criar um atributo, passe o parâmetro de caminho `apiName` necessário. Os parâmetros `name` e `dataType` também são obrigatórios. A descrição e os parâmetros `isPrimary` são opcionais.

```http
POST /rest/v1/activities/external/type/{apiName}/attributes/create.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendees",
      "name": "Number of Attendees",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Atualizar atributos

Ao atualizar atributos, o atributo `apiName` é a chave primária e já deve existir. Você não pode alterar `apiName` com uma atualização.

```http
POST /rest/v1/activities/external/type/{apiName}/attributes/update.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendee",
      "name": "Number of Attendee",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendee",
          "name": "Number of Attendee",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Excluir atributos

Para excluir um atributo, passe o parâmetro de caminho `apiName` necessário para a atividade personalizada. Além disso, passe o parâmetro de atributo obrigatório como uma matriz de objetos de atributo. Cada objeto deve conter um parâmetro `apiName` para o tipo de atividade personalizado.

```http
POST /rest/v1/activities/external/type/{apiName}/attributes/delete.json
```

```json
{ "attributes":[ { "apiName":"conferenceDate" }, { "apiName":"numberOfAttendees" } ] }
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Adicionar atividades personalizadas

As atividades personalizadas são registros de atividades históricas (write-once) para registros individuais de pessoas. Os administradores do Marketo podem gerenciar o esquema no Marketo ou uma integração de API pode gerenciá-lo remotamente.

Use o ponto de extremidade [Adicionar atividades personalizadas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/addCustomActivityUsingPOST) para adicionar atividades personalizadas a registros de clientes potenciais. O campo `leadId` associa cada atividade a um cliente potencial. Exibir atividades personalizadas no log de atividades do cliente potencial ou recuperá-las por meio de Obter atividades do cliente potencial especificando a ID do tipo de atividade personalizada.

Use atividades personalizadas para dados relacionados a uma pessoa que não precisam ser atualizados ou substituídos. Por exemplo, registre a participação no evento como uma atividade &quot;Evento assistido&quot;.

Use objetos personalizados para registros relacionados à pessoa que podem ser alterados, como inscrição de aluno. Objetos personalizados podem ser atualizados, mas atividades personalizadas não podem.

O membro de entrada é uma matriz de objetos de atividade. É possível enviar no máximo 300 registros de atividade por vez.

Os membros `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` e atributos são obrigatórios. A matriz de atributos deve conter o atributo não primário. Especifique-o com name (nome do campo) ou apiName (nome da API), e o valor para o valor a ser definido.

```http
POST /rest/v1/activities/external.json
```

```json
{
  "input": [
    {
      "leadId": 1001,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 1200,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 3000,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Contest Form",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 50,
      "marketoGUID": "50",
      "status": "added"
    },
    {
      "id": 51,
      "marketoGUID": "51",
      "status": "added"
    },
    {
      "status": "skipped",
      "errors": [
        {
          "code": "1004",
          "message": "Lead not found"
        }
      ]
    }
  ]
}
```

## Tempos limite

Os endpoints de atividades têm um tempo limite de 30 s, exceto para os seguintes endpoints:

- Obter token de paginação: 300s
- Adicionar atividade personalizada: 90s
