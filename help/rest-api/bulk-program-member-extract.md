---
title: Extração de membros do programa em massa
feature: REST API
description: Use as APIs REST de extração de membros do programa em massa do Marketo para exportar registros grandes de membros para ETL, data warehouse e arquivamento, com permissões e metadados de campo.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
TQID: https://experienceleague.adobe.com/w4qaVTKSe0EORaSiURB6WbJXi29JUdEgfkb2dnfuVFw
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1081
ht-degree: 5%

---

# Extração de membros do programa em massa

[Referência de Ponto de Extremidade de Extração de Membro de Programa em Massa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members)

As APIs REST de Extração de membro de programa em massa recuperam grandes conjuntos de registros de membros de programa do Marketo. Use essas APIs para troca contínua de dados entre o Marketo e sistemas externos, ETL, data warehouse e arquivamento.

## Permissões

O usuário da API deve ter uma função com a permissão Lead somente leitura, com a permissão Lead de leitura e gravação ou com ambas.

## Descrever

Use [Descrever Membro do Programa](https://developer.adobe.com/marketo-apis/api/mapi#operation/describeProgramMemberUsingGET2) para determinar quais campos estão disponíveis e recuperar seus metadados. O atributo `name` contém o nome do campo REST API.

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

## Filtros

As exportações de membros do programa oferecem suporte a várias opções de filtro. Quando um trabalho especifica vários tipos de filtro, a API os combina com uma operação AND.

Todo trabalho deve especificar `programId` ou `programIds`. Todos os outros filtros são opcionais. O filtro `updatedAt` requer uma infraestrutura que não está disponível em todas as assinaturas.

<table>
  <tbody>
    <tr>
      <td>Tipo de filtro</td>
      <td>Tipo de dados</td>
      <td>Observações</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Inteiro</td>
      <td>Aceita a ID de um programa. As tarefas retornam todos os registros acessíveis que são membros do programa no momento em que a tarefa começa a ser processada.Recupere as IDs de programa usando o ponto de extremidade <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Obter Programas</a>.Não é possível usar com o filtro programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Matriz[Inteiro]</td>
      <td>Aceita uma matriz de até 10 IDs de programa. As tarefas retornam todos os registros acessíveis que são membros dos programas no momento em que a tarefa começa a ser processada.Um campo adicional "programId" é adicionado ao arquivo de exportação como o primeiro campo. Este campo identifica o programa do qual um registro de associação de programa foi extraído.Recupere as IDs de programa usando o ponto de extremidade <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Obter Programas</a>.Não é possível usar com o filtro programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Booleano</td>
      <td>Aceita um booleano usado para filtrar registros de associação de programa para <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">pessoas que esgotaram o conteúdo</a>.</td>
    </tr>
    <tr>
      <td>NurtureCadence</td>
      <td>String</td>
      <td>Aceita uma sequência de caracteres usada para filtrar registros de associação de programa para uma determinada cadência de criação.Os valores permitidos são:
        <ul>
          <li>pausar - a cadência está pausada</li>
          <li>norma - a cadência é normal</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Matriz[Cadeia de caracteres]</td>
      <td>Aceita uma matriz de nomes de status de membros do programa. Vários nomes de status são ORed juntos.As tarefas com este tipo de filtro retornam todos os registros acessíveis cujo status de membro do programa corresponde a qualquer um dos nomes de status especificados. Podem ser usados os nomes de status padrão e definido pelo usuário.Se o filtro statusNames for usado com o filtro "programIds", cada programa será verificado para procurar registros de associação cujo status corresponda a qualquer um dos nomes de status. Se um nome de status não for encontrado em nenhum dos programas, o erro "1003, Dados inválidos" será retornado.
        <table>
          <tbody>
            <tr>
              <td>Participou</td>
              <td>Participação sob demanda</td>
              <td>Devolvido</td>
            </tr>
            <tr>
              <td>Clicado</td>
              <td>Contatado</td>
              <td>Convertido</td>
            </tr>
            <tr>
              <td>Envolvido</td>
              <td>Formulário preenchido</td>
              <td>Influenciado</td>
            </tr>
            <tr>
              <td>Convidado</td>
              <td>Membro</td>
              <td>Não compareceu</td>
            </tr>
            <tr>
              <td>Não está no programa</td>
              <td>Na lista</td>
              <td>Aberto</td>
            </tr>
            <tr>
              <td>Registrado</td>
              <td>Registrando</td>
              <td>Erro de registro</td>
            </tr>
            <tr>
              <td>Enviado</td>
              <td>Inscrito</td>
              <td>Inscrição cancelada</td>
            </tr>
            <tr>
              <td>Visualizado</td>
              <td>Visitado</td>
              <td>Estande visitado</td>
            </tr>
            <tr>
              <td>Em lista de espera</td>
              <td>Conteúdo da Web</td>
              <td></td>
            </tr>
          </tbody>
        </table></td>
    </tr>
    <tr>
      <td>updatedAt*</td>
      <td>Date Range</td>
      <td>Aceita um objeto JSON com os membros startAt e endAt. startAt aceita um datetime que representa a marca d'água inferior e endAt aceita um datetime que representa a marca d'água superior. O intervalo deve ser de 31 dias ou menos. Os datetimes devem estar em um formato ISO-8601, sem milissegundos.Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que foram atualizados mais recentemente dentro do intervalo de datas.</td>
    </tr>
  </tbody>
</table>

Algumas assinaturas não são compatíveis com esse tipo de filtro. Se não estiver disponível, o ponto de extremidade de Trabalho de Criar Programa de Exportação retornará `1035, Unsupported filter type for target subscription`. Entre em contato com o Suporte da Marketo para solicitar essa funcionalidade para sua assinatura.

## Opções

O ponto de extremidade Criar Trabalho do Membro do Programa de Exportação fornece opções para:

- Especifique os campos a serem incluídos no arquivo de exportação.
- Renomeie os cabeçalhos de coluna exportados.
- Especifique o formato do arquivo de exportação.

| Parâmetro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| campos | Matriz[Cadeia de Caracteres] | Sim | O parâmetro fields aceita uma matriz JSON de cadeias de caracteres. Os campos listados são incluídos no arquivo exportado. Os seguintes tipos de campo podem ser exportados:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Especifique um campo usando o nome da API REST que pode ser recuperado usando Descrever lead2 e/ou Descrever endpoints de membros do programa. |
| columnHeaderNames | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| formato | String | Não | Aceita um dos seguintes: CSV, TSV, SSV. O arquivo exportado é renderizado como um arquivo de valores separados por vírgula, valores separados por tabulação ou valores separados por espaço, respectivamente, se definido. O padrão é CSV, caso não esteja definido. |

## Criação de um trabalho

Use o ponto de extremidade [Criar Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportProgramMembersUsingPOST) para definir o trabalho de exportação. Especifique um `filter` que contenha a ID do programa e o `fields` a ser exportado. Você também pode especificar `format` e `columnHeaderNames`.

```http
POST /bulk/v1/program/members/export/create.json
```

```json
{
   "format": "CSV",
   "fields": [
        "firstName",
        "lastName",
        "email",
        "membershipDate",
        "program",
        "statusName",
        "leadId",
        "reachedSuccess",
        "leadCustomField01",
        "leadCustomField02",
        "pMCustomField01",
        "pMCustomField02"
   ],
   "filter": {
      "programId":1044
   }
}
```

```json
{
    "requestId": "4d44#16f92734f6e",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2020-01-11T02:33:48Z"
        }
    ],
    "success": true
}
```

A resposta confirma que o trabalho foi criado, mas a exportação não é iniciada automaticamente. Passe o `exportId` retornado para o ponto de extremidade [Enfileirar Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/enqueueExportProgramMembersUsingPOST) para iniciar o trabalho:

```http
POST /bulk/v1/program/members/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "d70b#16f9273ae32",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z"
        }
    ],
    "success": true
}
```

A resposta de enfileiramento inicialmente retorna um status `Queued`. Quando um slot de exportação se torna disponível, o status muda para `Processing`.

## Status do trabalho de pesquisa

Você pode recuperar o status somente para trabalhos criados pelo mesmo usuário da API.

Como a exportação é executada de forma assíncrona, use o ponto de extremidade [Obter Status do Trabalho do Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportLeadsStatusUsingGET) para sondar seu progresso. O status é atualizado apenas uma vez a cada 60 segundos, portanto, não consulte com mais frequência.

O status pode ser `Created`, `Queued`, `Processing`, `Canceled`, `Completed` ou `Failed`.

```http
GET /bulk/v1/program/members/export/{exportId}/status.json
```

```json
{
    "requestId": "9a40#16f9274d250",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
        }
    ],
    "success": true
}
```

Essa resposta mostra que a tarefa ainda está sendo processada, portanto, o arquivo não está disponível. Quando o status do trabalho mudar para `Completed`, o arquivo estará pronto para ser baixado.

```json
{
    "requestId": "11ad1#16f9ff6da23",
    "result": [
        {
            "exportId": "1118dc83-273b-4d44-becb-4d212fece550",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
            "finishedAt": "2020-01-11T02:36:12Z",
            "numberOfRecords": 13,
            "fileSize": 1752,
            "fileChecksum": "sha256:b3c8e70e6e501cf1025e345a66b409d4fd07364c7da773cfa68a2b68ce1a7212"
        }
    ],
    "success": true
}
```

## Recuperação de dados

Para recuperar uma exportação concluída de membros do programa, passe o `exportId` para o ponto de extremidade [Obter Arquivo de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportProgramMembersFileUsingGET).

O ponto de extremidade retorna o arquivo no formato configurado para o trabalho. Se um campo de membro do programa solicitado não contiver dados, o campo de exportação correspondente conterá `null`.

```http
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```text
firstName,lastName,email,Member Date,Program,Status,Lead Id,Success,leadCustomField01,leadCustomField02,pMCustomField01,pMCustomField02
Meera,Reed,mree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1789,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jon,Umber,jumb@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1790,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Lyanna,Mormont,lmor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1791,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickon,Stark,rsta@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1792,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Hodor,null,hodor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1793,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Osha,null,osha@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1794,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jojen,Reed,Jree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1795,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickard,Karstark,rkar@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1796,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Maester,Luwin,mluw@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1797,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rodrik,Cassel,rcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1798,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jory,Cassel,jcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1799,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Septa,Mordane,smor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1800,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
```

Para recuperação parcial ou retomável, o ponto de extremidade do arquivo dá suporte ao cabeçalho HTTP `Range` opcional com um tipo de intervalo de `bytes`. Se você não definir o cabeçalho, o endpoint retornará o arquivo inteiro. Para obter mais informações, consulte [Extração em massa](bulk-extract.md).

## Cancelar um trabalho

Para cancelar um trabalho configurado incorretamente ou que não é mais necessário, chame o ponto de extremidade [Cancelar Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/cancelExportProgramMembersUsingPOST):

```http
POST /bulk/v1/program/members/export/{exportId}/cancel.json
```

```json
{
    "requestId": "bb4f#16f86727f89",
    "result": [
        {
            "exportId": "f0d3520c-3a60-4568-9e71-2e619d3805a4",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2020-01-07T21:47:35Z"
        }
    ],
    "success": true
}
```

O status da resposta indica que a tarefa foi cancelada.
