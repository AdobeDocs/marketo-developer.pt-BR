---
title: Atividades
feature: REST API
description: Use a API REST de atividades do Marketo Engage para listar tipos de atividades, buscar atividades principais com tokens de paginação e lidar com alterações personalizadas e de valores de dados.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '2046'
ht-degree: 0%

---

# Atividades

O Marketo permite uma grande variedade de tipos de atividades relacionadas a registros de clientes potenciais.  Quase todas as alterações, ações ou etapas de fluxo são registradas no log de atividades de um lead e podem ser recuperadas por meio da API ou aproveitadas nos filtros e acionadores da Smart List e do Smart Campaign.  As atividades são sempre relacionadas ao registro de lead por meio do leadId, correspondente ao campo Id do registro, e também têm uma ID exclusiva própria.

Há um número muito grande de tipos de atividades potenciais, que podem variar de assinatura para assinatura, e têm definições exclusivas para cada um. Embora cada atividade tenha seu próprio e exclusivo `id`, `leadId` e `activityDate`, os valores `primaryAttributeValueId` e `primaryAttributeValue` variam em seu significado.

O Marketo também permite a criação de Tipos de atividade personalizados por meio da API de metadados de atividades personalizadas. A adição de atividades personalizadas é feita por meio da API Adicionar atividades personalizadas.

A maioria das atividades será removida após algum período.

## Descrever

Para recuperar uma lista de tipos disponíveis e suas definições para uma instância, você pode usar o ponto de extremidade [Obter Tipos de Atividade](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET).

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

As respostas do mundo real incluem muito mais definições. Neste exemplo, o tipo mostrado é um &quot;Formulário de preenchimento&quot;, que tem um atributo principal de &quot;ID de formulário da Web&quot;, que se refere à Marketo ID do formulário que foi preenchido e pode ser usado para se relacionar a esse ativo específico no Marketo. Além disso, há definições para cada um dos atributos possíveis de um registro de atividade específico desse tipo e seus tipos de dados. Observe que, se o campo estiver vazio, esse atributo específico será omitido de um registro de atividade individual.

## Consultar

Para recuperar atividades do Marketo, chame o ponto de extremidade [Obter atividades de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET). Primeiro, é necessário recuperar um token de paginação para o datetime a partir do qual você deseja começar a recuperar atividades. Em seguida, você passa o token de paginação no parâmetro de consulta `nextPageToken`. Além disso, você passa até dez IDs de tipo de atividade no parâmetro de consulta `activityTypeIds` como uma lista separada por vírgulas.

Opcionalmente, você pode incluir um parâmetro de consulta listId para restringir sua pesquisa apenas aos registros incluídos em uma lista estática específica, ou um parâmetro de consulta leadIds e pesquisar atividades somente de um conjunto especificado de leads. Você pode passar até 30 leadIds como uma lista separada por vírgulas.

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

Para a primeira chamada, use a API Obter Token de Paginação para obter `nextPageToken`. Para chamadas subsequentes a este ponto de extremidade, use o `nextPageToken returned` da resposta. Este ponto de extremidade sempre retorna `the nextPageToken`.

Se o atributo `moreResult` for true, significa que mais resultados estarão disponíveis. Continue a chamar este ponto de extremidade até que o atributo `moreResult` retorne falso, o que significa que não há resultados disponíveis. O `nextPageToken` retornado desta API deve sempre ser reutilizado para a próxima iteração desta chamada.

Em alguns casos, essa API pode responder com menos de 300 itens de atividade, mas também pode ter o atributo `moreResult` definido como verdadeiro.  Isso indica que há mais atividades que podem ser retornadas e que o ponto de extremidade pode ser consultado para atividades mais recentes incluindo a `nextPageToken` retornada em uma chamada subsequente.

Observe que em cada item da matriz de resultados, o atributo inteiro `id` está sendo substituído pelo atributo da cadeia de caracteres `marketoGUID` como identificador exclusivo.

### Alterações no valor dos dados

Para atividades de Alteração do valor de dados, é fornecida uma versão especializada da API de atividades. O ponto de extremidade [Obter Alterações de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) retorna somente atividades de registros de Alteração de Valor de Dados para campos de cliente potencial. A interface é a mesma da API Obter atividades principais, com duas diferenças:

* Não há um parâmetro `activityTypeIds`, pois o ponto de extremidade retorna apenas as atividades Alteração do Valor dos Dados e Novo Cliente Potencial.
* O parâmetro de consulta `fields` é obrigatório, no qual você pode passar uma lista de campos separada por vírgulas para indicar para quais campos deseja recuperar alterações.

```
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

Cada atividade na resposta tem uma matriz de campos, incluindo uma lista de alterações na atividade, que especificará os `id` e `name` do campo alterados, bem como os valores novos e antigos relativos à alteração.

Observe que em cada item da matriz de resultados, o atributo inteiro `id` está sendo substituído pelo atributo da cadeia de caracteres `marketoGUID` como identificador exclusivo.

### Clientes potenciais excluídos

Também há um ponto de extremidade especial [Obter clientes em potencial excluídos](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) para recuperar atividades excluídas da Marketo.

```
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

Observe que em cada item da matriz de resultados, o atributo inteiro `id` está sendo substituído pelo atributo da cadeia de caracteres `marketoGUID` como identificador exclusivo.

### Página pelos resultados

Por padrão, os endpoints mencionados nesta seção retornam 300 itens de atividade por vez.  Se o atributo `moreResult` for true, mais resultados estarão disponíveis. Chame o ponto de extremidade até que o atributo `moreResult` retorne false, o que significa que não há mais resultados disponíveis. O `nextPageToken` retornado deste ponto de extremidade deve sempre ser reutilizado para a próxima iteração desta chamada.

Em alguns casos, esse ponto de extremidade pode responder com menos de 300 itens de atividade, mas também pode ter o atributo `moreResult` definido como verdadeiro.  Isso indica que há atividades adicionais que podem ser retornadas e que o ponto de extremidade pode ser consultado para atividades mais recentes incluindo a `nextPageToken` retornada em uma chamada subsequente. Observe que `nextPageToken` precisa ser Codificado por URL na solicitação.

## Tipos de atividades personalizadas

As Atividades personalizadas funcionam como atividades padrão, exceto que o esquema é gerenciado por terceiros e não pelo Marketo. As instâncias de atividades personalizadas estão vinculadas aos registros de clientes potenciais por meio do `leadId` da mesma forma que as atividades padrão, mas os atributos primário e secundário são definidos arbitrariamente. Quando um tipo de atividade personalizado é aprovado, um acionador e um filtro de Smart List correspondentes são criados, para que os clientes potenciais possam ser processados com base nos dados de atividade personalizados atuais ou históricos.

* Número máximo de atividades personalizadas: 10
* Número máximo de atributos por atividade personalizada: 20

A recuperação de dados de atividades personalizadas é feita da mesma forma que as atividades padrão, por meio da API [Obter atividades de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET).

## Tipos de consulta

Além do ponto de extremidade padrão Obter Tipos de Atividade, os pontos de extremidade [Obter Tipos de Atividade Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) e [Descrever Tipo de Atividade Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) retornam detalhes sobre os tipos de atividade provisionados na instância do Marketo e metadados relativos aos atributos de determinado tipo. O [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) normal ainda retorna metadados sobre atividades personalizadas, mas não indica se um determinado tipo é personalizado.

### Obter tipos

```
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

Para descrições de tipo, você deve passar `apiName` como parâmetro de caminho. Por padrão, você obtém a versão aprovada da atividade. Opcionalmente, você pode passar o parâmetro `draft=true` para recuperar a versão de rascunho da atividade.

```
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

Cada tipo de atividade personalizado requer um nome de exibição, nome da API, nome do acionador, nome do filtro e atributo principal.

Para garantir a consistência de seus tipos com as convenções do Marketo e evitar colisões, é importante seguir algumas diretrizes ao criar seus tipos:

**Nome para Exibição:** O nome para exibição do tipo de atividade deve descrever resumidamente o que um registro de atividade representa, como &quot;Enviar Email&quot; ou &quot;Alterar Valor dos Dados&quot;. Normalmente, esses nomes devem estar no formato infinitivo, ou seja, &quot;Participar do evento&quot;.  Os nomes de exibição aceitam caracteres alfanuméricos, espaços e sublinhados. Os nomes para exibição devem conter pelo menos uma letra.

**Nome da API:** O nome da API é composto de caracteres alfanuméricos (tamanho máximo de 255). Se você for um parceiro do LaunchPoint, deve anexar um namespace representativo aos nomes da API do tipo de atividade. Isso evita colisões com tipos provisionados pelo cliente.  A convenção é usar todas as minúsculas ou camelCase para ajudar a distinguir entre outras strings de texto.

**Descrição:** Para atividades que podem ter comportamento não óbvio, deve incluir uma descrição do que o tipo de atividade representa em relação ao cliente potencial.

**Nome do Gatilho:** cada tipo de atividade deve ter um nome de gatilho exclusivo e legível. Os nomes dos acionadores devem estar no tempo presente de terceira pessoa, como &quot;Participa de um evento&quot;. Os parceiros do LaunchPoint devem incluir o nome da empresa na atividade, como &quot;Webinário de participantes - Empresa Acme&quot;.

**Nome do Filtro:**  Cada tipo de atividade deve ter um nome de filtro exclusivo e legível. Os nomes dos filtros devem estar no pretérito da terceira pessoa, como &quot;Participou de um evento&quot;. Os parceiros do LaunchPoint devem incluir o nome da empresa na atividade, ou seja, &quot;Webinário assistido - Acme Company&quot;.

**Atributo principal:** O atributo principal de uma atividade personalizada deve ser o campo mais significativo para o tipo de atividade. Por exemplo, para uma atividade &quot;Evento assistido&quot;, esse seria o nome do evento. Os atributos primários são incluídos como parâmetros por padrão em cada instância de um acionador ou filtro para esse tipo de atividade, e o valor é exibido no log de atividades de um registro de pessoa sem exigir drill-down para a atividade.

Quando uma atividade personalizada é criada, ela é criada como um rascunho e deve ser aprovada antes de ser usada para adicionar registros de atividade desse tipo. Todas as atualizações são aplicadas implicitamente à versão de rascunho do tipo. Para refletir as alterações na versão ao vivo do tipo, ele deve ser aprovado. Quando um tipo de atividade personalizado é aprovado e está em uso, nenhuma alteração nos campos acima pode ser feita.

Ao criar um tipo, o parâmetro de descrição é opcional, enquanto todos os parâmetros a seguir são obrigatórios: `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute`.

```
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

A atualização de um tipo é muito semelhante, exceto que apiName é o único parâmetro obrigatório como parâmetro de caminho.

```
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

Os tipos podem ser gerenciados com Aprovar tipo de atividade personalizado, Descartar rascunho do tipo de atividade personalizado e Excluir tipo de atividade personalizado, da mesma forma que os ativos padrão do Marketo.

## Atributos de tipo de atividade personalizados

Cada tipo de atividade personalizado pode ter de 0 a 20 atributos secundários. Os atributos secundários podem ter qualquer tipo de campo válido para um campo do Marketo. Elas são adicionadas, atualizadas e removidas separadamente do tipo principal, mas podem ser editadas enquanto um tipo de atividade estiver em uso e depois aprovadas. Quando os campos são editados em um tipo em tempo real, todas as atividades desse tipo criadas após a aprovação têm o novo atributo secundário definido. As alterações não serão aplicadas retroativamente a atividades existentes que compartilhem esse tipo.

Tenha cuidado com a remoção de atributos, pois isso afetará sua disponibilidade para uso nos filtros correspondentes.

As atualizações feitas na lista de atributos secundária usam o nome da API de cada atributo como uma chave primária. O Nome da API de um atributo não pode ser alterado. Ele deve ser excluído e adicionado novamente com o nome da API desejado.

Os tipos de dados válidos para atributos são: string, boolean, integer, float, link, email, currency, date, datetime, phone, text.

Ao alterar o atributo primário de um tipo de atividade, qualquer atributo primário existente deve ser rebaixado, definindo-se primeiro `isPrimary` como falso.

### Criar atributos

A criação de um atributo usa um parâmetro de caminho `apiName` necessário. Também são necessários os parâmetros `name` e `dataType`.`The description and` `isPrimary` parâmetros são opcionais.

```
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

Ao executar atualizações de atributos, o `apiName` do atributo é a chave primária. O parâmetro `apiName` deve existir para que a atualização seja bem-sucedida (ou seja, você não pode alterar o parâmetro `apiName` usando update).

```
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

A exclusão de um atributo usa um parâmetro de caminho `apiName` necessário, que é o nome da API de atividade personalizada.  Também é necessário um parâmetro de atributo que seja uma matriz de objetos de atributo.  Cada objeto deve conter um parâmetro `apiName` que seja o nome da API do tipo de atividade personalizada.

```
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

As atividades personalizadas são registros gravados uma vez de atividades históricas relacionadas a registros individuais de pessoas no Marketo. Essas atividades têm um esquema gerenciado por administradores do Marketo ou remotamente por meio de uma integração de API. As atividades personalizadas são adicionadas aos registros de cliente potencial por meio do ponto de extremidade [Adicionar Atividades Personalizadas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) e relacionadas a cada registro de cliente potencial por meio de seu campo `leadId`. As atividades personalizadas podem ser visualizadas na interface do usuário por meio do log de atividades do cliente potencial ou recuperadas pelo endpoint Obter atividades do cliente potencial especificando a ID do tipo de atividade personalizada.

As atividades personalizadas são apropriadas para registrar dados relacionados a um único registro pessoal e que não precisam ser atualizados ou substituídos. Um exemplo seria gravar uma pessoa participando de um evento como uma atividade &quot;Evento assistido&quot;. Para registros relacionados a uma pessoa que pode mudar, como inscrição de aluno, objetos personalizados devem ser usados, pois podem ser atualizados, o que não ocorre com atividades personalizadas.

O membro de entrada é uma matriz de objetos de atividade. Um máximo de 300 registros de atividade podem ser enviados por vez.

Os membros `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` e atributos são obrigatórios. A matriz de atributos deve conter o atributo não primário. Isso pode ser especificado usando name (nome do campo) ou apiName (nome da API) e o valor que corresponde ao valor que você está definindo.

```
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

Os endpoints de atividades têm um tempo limite de 30 s, a menos que indicado abaixo.

* Obter token de paginação: 300s
* Adicionar atividade personalizada: 90s
