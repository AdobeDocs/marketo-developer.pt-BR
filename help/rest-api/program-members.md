---
title: "Membros do programa"
feature: REST API
description: "Criar e gerenciar membros do programa."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Membros do programa

[Referência de endpoint de membros do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

O Marketo expõe as APIs para ler, criar, atualizar e excluir registros de membros do programa. Os registros de membros do programa estão relacionados aos registros de cliente potencial por meio do campo de id de cliente potencial. Os registros são compostos de um conjunto de campos padrão e, opcionalmente, até 20 campos personalizados adicionais. Os campos contêm dados específicos do programa para cada membro e podem ser usados em formulários, filtros, acionadores e ações de fluxo. Esses dados podem ser visualizados no [Guia Membros](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) na interface do Marketo Engage.

## Descrever

A variável [Descrever membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) O endpoint segue o padrão padrão para objetos de banco de dados de clientes potenciais. A variável `searchableFields` matriz fornece o conjunto de campos válidos para consulta. A variável `fields` A matriz contém metadados de campo, incluindo nome da API REST, nome de exibição e capacidade de atualização de campo.

```
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Consultar

A variável [Obter membros do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) O endpoint permite recuperar membros de um programa. Exige uma `programId` parâmetro de caminho e `filterType` e `filterValues` parâmetros de consulta.

`programId` é usado para especificar qual programa pesquisar.

`filterType` é usado para especificar qual campo usar como filtro de pesquisa. Ele aceita qualquer campo na lista &quot;searchableFields&quot; retornada pelo [Descrever membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) terminal. Se você especificar um filterType que seja um campo personalizado, o dataType do campo personalizado deve ser &quot;string&quot; ou &quot;integer&quot;. Se você especificar um filterType diferente de &quot;leadId&quot;, um máximo de 100.000 registros de membros de programa poderão ser processados pela solicitação. Dependendo de como sua instância do Marketo está configurada, você receberá um dos seguintes erros:

- Se o número total de membros do programa exceder 100.000, um erro será retornado: &quot;1003, Total subscription size: 100.001 excede o limite permitido de 100.000 para o filtro&quot;.
- Se o número total de membros do programa _que correspondem ao filtro_ exceder 100.000, um erro será retornado: &quot;1003, Tamanho de associação correspondente: 100.001 excede o limite permitido (100.000) para esta api&quot;.

Para consultar um programa cuja contagem de associação excede o limite, use o [API de extração de membros do programa em massa](bulk-program-member-extract.md) em vez disso.

`filterValues` é usado para especificar quais valores pesquisar e aceita até 300 valores em um formato separado por vírgulas. A chamada procura registros em que o campo do membro do programa corresponde a um dos filterValues incluídos.

Como alternativa, você pode filtrar por intervalo de datas especificando `updatedAt` como filterType com `startAt` e `endAt` parâmetros datetime. O intervalo deve ser de sete dias ou menos. Os datetimes devem estar em um formato ISO-8601, sem milissegundos.

O modelo opcional `fields` parâmetro de consulta aceita uma lista separada por vírgulas de nomes de API de campo retornados pelo [Descrever membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) terminal. Quando incluído, cada registro na resposta inclui os campos especificados. Quando omitido, o conjunto padrão de campos retornados é `acquiredBy`, `leadId`, `membershipDate`, `programId`, e `reachedSuccess`.

Por padrão, no máximo 300 registros são retornados. Você pode usar o `batchSize` parâmetro de consulta para reduzir esse número. Se a variável **moreResult** é true, significa que mais resultados estão disponíveis. Continue a chamar esse endpoint até que o atributo moreResult retorne false, o que significa que não há resultados disponíveis. A variável `nextPageToken` retornados por essa API devem sempre ser reutilizados para a próxima iteração dessa chamada.

Se o tamanho total da sua solicitação GET exceder 8KB, será retornado um erro HTTP: &quot;414, URI muito longo&quot; (por [RFC 7231](https://datatracker.ietf.org/doc/html/rfc72316.5.12)). Como solução alternativa, você pode alterar o GET para POST, adicionar `_method=GET` e coloque a string de consulta no corpo da solicitação.

```
GET /rest/v1/programs/{programId}/members.json?filterType=statusName&filterValues=Influenced
```

```json
{
    "requestId": "109da#17915eec072",
    "result": [
        {
            "seq": 0,
            "leadId": 1789,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 1,
            "leadId": 1790,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 2,
            "leadId": 1791,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 3,
            "leadId": 1792,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 4,
            "leadId": 1793,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 5,
            "leadId": 1794,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 6,
            "leadId": 1795,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 7,
            "leadId": 1796,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 8,
            "leadId": 1797,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 9,
            "leadId": 1798,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 10,
            "leadId": 1799,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 11,
            "leadId": 1800,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Criar e atualizar

Há dois endpoints que oferecem suporte à operação de criação/atualização em membros do programa. Uma permite atualizar somente o status dos membros do programa. O outro permite atualizar o conjunto de campos de membro do programa marcados como &quot;atualizáveis&quot;. Ambos os endpoints permitem modificar até 300 registros de membros de programa por chamada.

### Status do membro do programa

A variável [Status do Membro do Programa de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) endpoint é usado para criar ou atualizar o status do programa para um ou mais membros.

O necessário `programId` parâmetro de caminho especifica o programa que contém membros a serem criados ou atualizados.

O necessário `statusName` parâmetro especifica o status do programa a ser aplicado a uma lista de clientes potenciais. O statusName deve corresponder a um status disponível para o canal do programa. Os status válidos podem ser recuperados usando o [Obter canais](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET) terminal. Se o status de um lead tiver um valor de etapa maior que o statusName designado, esse lead será ignorado.

O necessário `input` o parâmetro é uma matriz de `leadId` que correspondem aos membros do programa. É possível enviar até 300 leadIds por chamada. Uma operação de substituição é executada em cada registro. Se o leadId estiver associado a um membro do programa, seu status de associação será atualizado. Caso contrário, um novo registro de membro do programa será criado, o registro será associado ao leadId e o status da associação será atribuído.

O endpoint responde com uma `status` de &quot;atualizado&quot;, &quot;criado&quot; ou &quot;ignorado&quot;. Se ignorado, um `reasons` também será incluído. O endpoint também responderá com uma `seq` campo que é um índice que pode ser usado para correlacionar os registros enviados à ordem da resposta.

Se a chamada for bem-sucedida, uma atividade &quot;Alterar status do programa&quot; será gravada no log de atividades do lead.

```
POST /rest/v1/programs/{programId}/members/status.json
```

```
Content-Type: application/json
```

```json
{
    "statusName":"Influenced",
    "input":[
        {
            "leadId": 1800
        },
        {
            "leadId": 1801
        },
        {
            "leadId": 1235
        }
    ]
}
```

```json
{
    "requestId": "14b2d#17916378ec5",
    "result": [
        {
            "seq": 0,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead skipped because it is already in or past this status"
                }
            ]
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1801
        },
        {
            "seq": 2,
            "status": "created",
            "leadId": 1235
        }
    ],
    "success": true
}
```

### Dados dos membros do programa

A variável [Sincronizar Dados do Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) endpoint é usado para atualizar dados de campo de membro do programa para um ou mais membros. Você pode modificar qualquer campo personalizado ou campos padrão que sejam &quot;atualizáveis&quot; (consulte [Descrever membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) (endpoint).

O necessário `programId` parâmetro de caminho especifica o programa que contém os membros a serem atualizados.

O necessário `input` é uma matriz. Cada elemento de matriz contém um `leadId` e um ou mais campos a serem atualizados (usando o nome da API). Uma operação de atualização é executada em cada registro. O leadId deve estar associado a um membro do programa. Os campos devem ser atualizáveis. É possível enviar até 300 leadIds por chamada.

O endpoint responde com uma `status` de &quot;atualizado&quot; ou &quot;ignorado&quot;. Se ignorado, um `reasons` também será incluído. O endpoint também responderá com uma `seq` campo que é um índice que pode ser usado para correlacionar os registros enviados à ordem da resposta.

Se a chamada for bem-sucedida, uma atividade &quot;Alterar dados do membro do programa&quot; será gravada no log de atividades do lead.

```
POST /rest/v1/programs/{programId}/members.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1789,
            "registrationCode": "dcff5f12-a7c7-11eb-bcbc-0242ac130002"
        },
        {
            "leadId": 1790,
            "registrationCode": "c0404b78-d3fd-47bf-82c4-d16f3852ab3a"
        },
        {
            "leadId": 1003,
            "registrationCode": "aa880c57-75b8-426b-a33a-fbf6302d7cb4"
        }
    ]
}
```

```json
{
    "requestId": "edc3#1791659b8d2",
    "result": [
        {
            "seq": 0,
            "status": "updated",
            "leadId": 1789
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1790
        },
        {
            "seq": 2,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1013",
                    "message": "Membership not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Campos

O objeto do membro do programa contém campos padrão e campos personalizados opcionais. Campos padrão estão presentes em cada assinatura de Marketo Engage, enquanto campos personalizados são criados pelo usuário, conforme necessário. Cada definição de campo é composta de um conjunto de atributos que descrevem o campo. Exemplos de atributos são nome de exibição, nome da API e dataType. Esses atributos são conhecidos coletivamente como metadados.

Os endpoints a seguir permitem consultar, criar e atualizar campos no objeto membro do programa. Essas APIs exigem que o usuário da API proprietária tenha uma função com uma ou as duas **Campo Padrão de Esquema de Leitura/Gravação** ou **Campo Personalizado do Esquema de Leitura-Gravação** permissões.

### Campos de consulta

Consultar campos de membros do programa é simples. Você pode consultar um único campo de membro de programa por nome de API ou consultar o conjunto de todos os campos de membro de programa. Os campos padrão e personalizados podem ser recuperados, dependendo das permissões de função que estão sendo usadas. Campos ocultos também são recuperados.

#### Por nome

A variável [Obter campo de membro do programa por nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) O ponto de extremidade recupera metadados de um único campo no objeto do membro do programa. O necessário `fieldApiName` parâmetro de caminho especifica o nome da API do campo. A resposta é como o endpoint Descrever membro do programa, mas contém metadados adicionais, como o `isCustom` atributo que indica se o campo é um campo personalizado.

```
GET /rest/v1/programs/members/schema/fields/{fieldApiName}.json
```

```json
{
    "requestId": "15416#17e955554de",
    "result": [
        {
            "displayName": "Status",
            "name": "statusName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Navegar

A variável [Obter campos de membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) o ponto de extremidade recupera metadados para todos os campos no objeto do membro do programa. Por padrão, no máximo 300 registros são retornados. Você pode usar o `batchSize` parâmetro de consulta para reduzir esse número. Se a variável `moreResult` é true, significa que mais resultados estão disponíveis. Continue a chamar esse endpoint até que o atributo moreResult retorne false, o que significa que não há resultados disponíveis. A variável `nextPageToken` retornados por essa API devem sempre ser reutilizados para a próxima iteração dessa chamada.

```
GET /rest/v1/programs/members/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "102f6#17e9557f123",
    "result": [
        {
            "displayName": "Acquired By",
            "name": "acquiredBy",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Cadence",
            "name": "nurtureCadence",
            "description": null,
            "dataType": "string",
            "length": 4,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Exhausted",
            "name": "isExhausted",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Member Date",
            "name": "membershipDate",
            "description": null,
            "dataType": "datetime",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Program",
            "name": "program",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "BC7J6EPVLT6T4B5FKUU3APCYN4======",
    "moreResult": true
}
```

### Criar campos

A variável [Criar campos de membros do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) endpoint cria um ou mais campos personalizados no objeto de membro do programa. Esse endpoint fornece funcionalidades comparáveis às do [disponível na interface de usuário do Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). É possível criar até 20 campos personalizados usando esse endpoint.

Considere cuidadosamente cada campo criado na instância de produção do Marketo Engage usando a API. Depois que um campo é criado, não é possível excluí-lo ([você só pode ocultá-la](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). A proliferação de campos não utilizados é uma prática recomendada que causará desordem na sua instância.

O necessário `input` parâmetro é uma matriz de objetos de campo de membros do programa. Cada objeto contém um ou mais atributos. Os atributos necessários são o `displayName`, `name`, e `dataType` que correspondem ao nome de exibição da interface do usuário do campo, ao nome da API do campo e ao tipo de campo, respectivamente. Opcionalmente, você pode especificar `description`, `isHidden`, `isHtmlEncodingInEmail`e `isSensitive`.

Há algumas regras associadas ao `name` e `displayName` nomeação. A variável `name` o atributo deve ser exclusivo, começar com uma letra e conter apenas letras, números ou sublinhado. O *`isplayName` deve ser exclusivo e não pode conter caracteres especiais. Uma convenção de nomenclatura comum deve ser aplicada [camel case](https://en.wikipedia.org/wiki/Camel_case#) para `displayName` para produzir `name`. Por exemplo, uma variável `displayName` de &quot;Meu campo personalizado&quot; produziria uma `name` de &quot;myCustomField&quot;.

```
POST /rest/v1/programs/members/schema/fields.json
```

```json
{
  "input": [
    {
        "displayName": "PMCF Custom Field 03",
        "name": "pMCFCustomField03",
        "description": "My third custom field",
        "dataType": "string"
    }
  ]
}
```

```json
{
    "requestId": "13a7#17e955fcb44",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "created"
        }
    ],
    "success": true
}
```

### Atualizar campo

A variável [Atualizar campo de membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) O endpoint atualiza um único campo personalizado no objeto membro do programa. Geralmente, as operações de atualização de campo executadas usando a interface do usuário do Marketo Engage são viáveis usando a API. Há algumas diferenças resumidas na tabela abaixo.

| Atributo | Atualizável por API? | Atualizável pela interface? | Atualizável por API? | Atualizável pela interface? |
|---|---|---|---|---|
| dataType | não | não | não | sim |
| descrição | sim | sim | sim | sim |
| displayName | não | não | sim | sim |
| isCustom | não | não | não | não |
| isHidden | não | sim | sim (se criado pela API) | sim |
| isHtmlEncodingInEmail | sim | sim | sim | sim |
| isSensitive | sim | sim | sim | sim |
| length | não | não | não | não |
| name | não | não | não | não |

O necessário `fieldApiName` parâmetro de caminho especifica o nome da API do campo a ser atualizado. O necessário `input` O parâmetro é uma matriz que contém um único objeto de campo de cliente potencial. O objeto de campo contém um ou mais atributos.

```
POST /rest/v1/programs/members/schema/fields/pMCFCustomField03.json
```

```json
{
  "input": [
      {
        "displayName": "Lunch Preference",
        "description": "Attendee food preference",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

```json
{
    "requestId": "215f#17e95663955",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Excluir

A variável [Excluir membros do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) o ponto de extremidade é usado para excluir registros de membros do programa. O necessário `programId` parâmetro de caminho especifica o programa que contém membros a serem excluídos. O corpo da solicitação contém um `input` matriz de ids de lead. São permitidas no máximo 300 IDs de cliente potencial por chamada.

O endpoint responde com uma `status` de &quot;excluído&quot; ou &quot;ignorado&quot;. Se ignorado, um `reasons` também será incluído. O endpoint também responderá com uma `seq` campo que é um índice que pode ser usado para correlacionar os registros enviados à ordem da resposta.

```
POST /rest/v1/programs/{programId}/members/delete.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1235
        },
        {
            "leadId": 77
        }
    ]
}
```

```json
{
    "requestId": "302a#17916619417",
    "result": [
        {
            "seq": 0,
            "status": "deleted",
            "leadId": 1235
        },
        {
            "seq": 1,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead not in program"
                }
            ]
        }
    ],
    "success": true
}
```
