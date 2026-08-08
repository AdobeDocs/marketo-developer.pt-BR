---
title: Membros do programa
feature: REST API
description: Use a API REST do Marketo para ler, criar, atualizar e excluir membros do programa, gerenciar campos padrão e personalizados e consultar usando campos pesquisáveis.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
TQID: https://experienceleague.adobe.com/scEHyXYq9C7cCS1kIX810wG7ahT9fsa448NwIfBmzQM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1632
ht-degree: 2%

---

# Membros do programa

[Referência de endpoint de membros do programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members)

O Marketo fornece APIs para ler, criar, atualizar e excluir registros de membros do programa. O campo de id de cliente potencial relaciona registros de membro de programa a registros de cliente potencial.

Cada registro contém campos padrão e pode conter até 20 campos personalizados. Esses campos armazenam dados do membro específicos do programa para uso em formulários, filtros, acionadores e ações de fluxo. Você pode visualizar esses dados na [Guia Membros](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) do programa na interface do usuário do Marketo Engage.

## Descrever

O ponto de extremidade [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeProgramMemberUsingGET2) segue o padrão padrão para objetos de Banco de Dados de Cliente Potencial.

- A matriz `searchableFields` identifica campos que são válidos para consultas.
- A matriz `fields` contém metadados como o nome da API REST, o nome de exibição e se o campo é atualizável.

```http
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

Use o ponto de extremidade [Obter Membros do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/getProgramMembersUsingGET) para recuperar membros de um programa. A solicitação requer um parâmetro de caminho `programId` e parâmetros de consulta `filterType` e `filterValues`.

`programId` especifica o programa a pesquisar.

`filterType` especifica o campo a ser usado como filtro de pesquisa. Ele aceita qualquer campo na lista &quot;searchableFields&quot; retornada pelo ponto de extremidade [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeProgramMemberUsingGET2). Para um campo personalizado, o dataType deve ser &quot;string&quot; ou &quot;integer&quot;.

Quando filterType não for &quot;leadId&quot;, a solicitação poderá processar no máximo 100.000 registros de membros do programa. Dependendo da configuração da instância do Marketo, você receberá um destes erros:

- Se o número total de membros do programa exceder 100.000, um erro será retornado: &quot;1003, Total subscription size: 100.001 excede o limite permitido de 100.000 para o filtro&quot;.
- Se o número total de membros do programa _que correspondem ao filtro_ exceder 100.000, um erro será retornado: &quot;1003, Tamanho de associação correspondente: 100.001 excede o limite permitido (100.000) para esta api&quot;.

Para consultar um programa cuja contagem de associação excede o limite, use a [API de Extração de Membro de Programa em Massa](bulk-program-member-extract.md).

`filterValues` especifica os valores a serem procurados e aceita até 300 valores separados por vírgula. A chamada procura registros em que o campo de membro do programa corresponde a um dos filterValues incluídos.

Como alternativa, filtre por intervalo de datas especificando `updatedAt` como filterType e fornecendo os parâmetros datetime `startAt` e `endAt`. O intervalo deve ser de sete dias ou menos. Use o formato ISO-8601 sem milissegundos para valores datetime.

O parâmetro de consulta `fields` opcional aceita uma lista separada por vírgulas de nomes de API de campos retornados pelo ponto de extremidade [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeProgramMemberUsingGET2). Quando incluído, cada registro de resposta contém os campos especificados. Quando omitida, a resposta retorna `acquiredBy`, `leadId`, `membershipDate`, `programId` e `reachedSuccess` por padrão.

Por padrão, o endpoint retorna no máximo 300 registros. Use o parâmetro de consulta `batchSize` para reduzir esse número.

Se o atributo **moreResult** for true, mais resultados estarão disponíveis. Continue chamando o ponto de extremidade com o `nextPageToken` retornado até que moreResult seja false.

Se o comprimento total da solicitação GET exceder 8KB, o endpoint retornará o erro HTTP &quot;414, URI muito longo&quot;. Para contornar esse limite, altere a solicitação de GET para POST, adicione o parâmetro `_method=GET` e coloque a sequência de consulta no corpo da solicitação.

```http
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

Dois endpoints oferecem suporte para operações de criação e atualização em membros do programa:

- Um endpoint atualiza somente o status de membro do programa.
- Um endpoint atualiza campos de membros do programa marcados como &quot;atualizáveis&quot;.

Cada endpoint pode modificar até 300 registros de membros de programa por chamada.

### Status do membro do programa

Use o ponto de extremidade [Status do Membro do Programa de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncProgramMemberStatusUsingPOST) para criar ou atualizar o status do programa para um ou mais membros.

Os parâmetros necessários são:

- `programId`: um parâmetro de caminho que especifica o programa que contém membros a serem criados ou atualizados.
- `statusName`: especifica o status do programa a ser aplicado a uma lista de clientes potenciais. O statusName deve corresponder a um status disponível para o canal do programa. Recupere status válidos com o ponto de extremidade [Obter Canais](https://developer.adobe.com/marketo-apis/api/asset#operation/getAllChannelsUsingGET). Se o status de um lead tiver um valor de etapa maior que o statusName designado, a solicitação ignorará esse lead.
- `input`: Uma matriz de `leadId` valores que correspondem aos membros do programa. É possível enviar até 300 leadIds por chamada.

O endpoint executa uma substituição em cada registro. Se o leadId estiver associado a um membro do programa, o endpoint atualizará seu status de associação. Caso contrário, ele cria um registro de membro do programa, associa o registro ao leadId e atribui o status de associação.

A resposta inclui um `status` de &quot;atualizado&quot;, &quot;criado&quot; ou &quot;ignorado&quot;. Um resultado ignorado também inclui uma matriz `reasons`. O campo `seq` é um índice que correlaciona cada registro enviado com a ordem de resposta.

Se a chamada for bem-sucedida, uma atividade &quot;Alterar status do programa&quot; será gravada no log de atividades do lead.

```http
POST /rest/v1/programs/{programId}/members/status.json
```

```text
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

Use o ponto de extremidade [Dados do Membro do Programa de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi#operation/syncProgramMemberDataUsingPOST) para atualizar os dados de campo do membro do programa para um ou mais membros. Você pode modificar qualquer campo personalizado ou qualquer campo padrão marcado como &quot;atualizável&quot; pelo ponto de extremidade [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeProgramMemberUsingGET2).

Os parâmetros necessários são:

- `programId`: um parâmetro de caminho que especifica o programa que contém membros a serem atualizados.
- `input`: uma matriz cujos elementos contêm um `leadId` e um ou mais campos para atualizar pelo nome da API. É possível enviar até 300 leadIds por chamada.

O endpoint atualiza cada registro. O leadId deve ser associado a um membro do programa, e cada campo deve ser atualizável.

A resposta inclui um `status` de &quot;atualizado&quot; ou &quot;ignorado&quot;. Um resultado ignorado também inclui uma matriz `reasons`. O campo `seq` é um índice que correlaciona cada registro enviado com a ordem de resposta.

Se a chamada for bem-sucedida, uma atividade &quot;Alterar dados do membro do programa&quot; será gravada no log de atividades do lead.

```http
POST /rest/v1/programs/{programId}/members.json
```

```text
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

O objeto do membro do programa contém campos padrão e campos personalizados opcionais. Campos padrão estão presentes em todas as assinaturas do Marketo Engage, enquanto usuários criam campos personalizados conforme necessário.

Cada campo é definido por atributos, como nome de exibição, nome da API e dataType. Juntos, esses atributos são chamados de metadados.

Os pontos de extremidade a seguir consultam, criam e atualizam campos no objeto de membro de programa. O usuário da API deve ter uma função com a permissão **Campo Padrão de Esquema de Leitura-Gravação**, o **Campo Personalizado de Esquema de Leitura-Gravação** ou ambos.

### Campos de consulta

Consulte um campo de membro de programa por nome de API ou recupere todos os campos de membro de programa. As permissões de função determinam se a resposta pode incluir campos padrão, campos personalizados ou ambos. A resposta também inclui campos ocultos.

#### Por nome

O ponto de extremidade [Obter Campo de Membro do Programa por Nome](https://developer.adobe.com/marketo-apis/api/mapi#operation/getProgramMemberFieldByNameUsingGET) recupera metadados de um campo no objeto de membro do programa. O parâmetro de caminho `fieldApiName` necessário especifica o nome da API do campo.

A resposta é semelhante à resposta Descrever membro do programa, mas inclui metadados adicionais. Por exemplo, o atributo `isCustom` indica se o campo é personalizado.

```http
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

#### Procurar

O ponto de extremidade [Obter Campos de Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/getProgramMemberFieldsUsingGET) recupera metadados para todos os campos no objeto de membro do programa. Por padrão, retorna no máximo 300 registros. Use o parâmetro de consulta `batchSize` para reduzir esse número.

Se o atributo `moreResult` for true, mais resultados estarão disponíveis. Continue chamando o ponto de extremidade com o `nextPageToken` retornado até que moreResult seja false.

```http
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

O ponto de extremidade [Criar Campos de Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/createProgramMemberFieldUsingPOST) cria campos personalizados no objeto de membro do programa. Ela fornece funcionalidades comparáveis à [interface do Marketo Engage](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Você pode criar até 20 campos personalizados com esse endpoint.

Considere cuidadosamente cada campo antes de criá-lo em uma instância de produção do Marketo Engage. Após criar um campo, você não pode excluí-lo; [você só pode ocultá-lo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo). Campos não utilizados adicionam desordem à instância.

O parâmetro `input` necessário é uma matriz de objetos de campo de membros do programa. Cada objeto contém um ou mais atributos.

- Os atributos necessários são `displayName`, `name` e `dataType`. Eles correspondem ao nome de exibição da interface, ao nome da API e ao tipo de campo, respectivamente.
- Os atributos opcionais são `description`, `isHidden`, `isHtmlEncodingInEmail` e `isSensitive`.

Os atributos `name` e `displayName` têm estas regras de nomenclatura:

- O atributo `name` deve ser exclusivo, começar com uma letra e conter apenas letras, números ou sublinhados.
- O *`isplayName` deve ser exclusivo e não pode conter caracteres especiais.

Uma convenção comum é aplicar o [camel case](https://en.wikipedia.org/wiki/Camel_case#) a `displayName` para produzir `name`. Por exemplo, um `displayName` de &quot;Meu campo personalizado&quot; produz um `name` de &quot;myCustomField&quot;.

```http
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

O ponto de extremidade [Atualizar Campo de Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/updateProgramMemberFieldUsingPOST) atualiza um campo personalizado no objeto de membro do programa. A maioria das atualizações de campo disponíveis na interface do usuário do Marketo Engage também está disponível por meio da API. A tabela a seguir resume as diferenças.

| Atributo | Atualizável por API? | Atualizável pela interface? | Atualizável por API? | Atualizável pela interface? |
| --- | --- | --- | --- | --- |
| dataType | não | não | não | sim |
| descrição | sim | sim | sim | sim |
| displayName | não | não | sim | sim |
| isCustom | não | não | não | não |
| isHidden | não | sim | sim (se criado pela API) | sim |
| isHtmlEncodingInEmail | sim | sim | sim | sim |
| isSensitive | sim | sim | sim | sim |
| length | não | não | não | não |
| name | não | não | não | não |

A solicitação exige estes parâmetros:

- `fieldApiName`: um parâmetro de caminho que especifica o nome da API do campo a ser atualizado.
- `input`: uma matriz que contém um objeto de campo de cliente potencial com um ou mais atributos.

```http
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

Use o ponto de extremidade [Excluir Membros do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/deleteProgramMemberUsingPOST) para excluir registros de membros do programa. O parâmetro de caminho `programId` necessário especifica o programa que contém os membros a serem excluídos.

O corpo da solicitação contém uma matriz `input` de IDs de clientes em potencial. Cada chamada permite no máximo 300 IDs de lead.

A resposta inclui um `status` de &quot;excluído&quot; ou &quot;ignorado&quot;. Um resultado ignorado também inclui uma matriz `reasons`. O campo `seq` é um índice que correlaciona cada registro enviado com a ordem de resposta.

```http
POST /rest/v1/programs/{programId}/members/delete.json
```

```text
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
