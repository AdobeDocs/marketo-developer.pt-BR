---
title: "Extração de membros do programa em massa"
feature: REST API
description: "Processamento em lote da extração de dados do Membro."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 4%

---


# Extração de membros do programa em massa

[Referência de Ponto de Extremidade de Extração de Membro de Programa em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

O conjunto de Extração de membro de programa em massa de APIs REST fornece uma interface programática para recuperar grandes conjuntos de registros de membro de programa do Marketo. Essa é a interface recomendada para casos de uso que exigem intercâmbio contínuo de dados entre o Marketo e um ou mais sistemas externos, para fins de ETL, data warehouse e arquivamento.

## Permissões

As APIs de Extração de membros do programa em massa exigem que o usuário da API responsável tenha uma função com uma ou ambas as permissões de Lead somente leitura ou Lead de leitura e gravação.

## Descrever

[Descrever membro do programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) O é a principal fonte da verdade para determinar se os campos estão disponíveis para uso e os metadados sobre esses campos. A variável `name` O atributo contém o nome da API REST.

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

## Filtros

Os membros do programa oferecem suporte a várias opções de filtro. Vários tipos de filtros podem ser especificados para um trabalho, nesse caso, eles são ANDed juntos. Você deve especificar o `programId` ou o `programIds` filtro. Todos os outros filtros são opcionais. A variável `updatedAt` O filtro exige componentes de infraestrutura adicionais que ainda não foram implantados em todas as assinaturas.

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
      <td>Aceita a ID de um programa. As ordens de produção retornam todos os registros acessíveis que são membros do programa no momento em que a ordem de produção começa a ser processada. Recupere as IDs do programa usando <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Obter Programas</a> endpoint.Não pode ser usado com o filtro programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Matriz[Inteiro]</td>
      <td>Aceita uma matriz de até 10 IDs de programa. As tarefas retornam todos os registros acessíveis que são membros dos programas no momento em que a tarefa começa a ser processada. Um campo adicional "programId" é adicionado ao arquivo de exportação como o primeiro campo. Este campo identifica o programa do qual um registro de associação de programa foi extraído. Recupere as IDs de programa usando a <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Obter Programas</a> endpoint.Não pode ser usado com o filtro programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Booleano</td>
      <td>Aceita um booleano usado para filtrar registros de associação de programa para <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">pessoas que esgotaram o conteúdo</a>.</td>
    </tr>
    <tr>
      <td>NurtureCadence</td>
      <td>Sequência de caracteres</td>
      <td>Aceita uma sequência de caracteres usada para filtrar registros de associação de programa para uma determinada cadência de criação. Os valores permitidos são:
        <ul>
          <li>pausar - a cadência está pausada</li>
          <li>norma - a cadência é normal</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Matriz[Cadeia de caracteres]</td>
      <td>Aceita uma matriz de nomes de status de membros do programa. Vários nomes de status são ORed juntos.Trabalhos com esse tipo de filtro retornam todos os registros acessíveis cujo status de membro do programa corresponde a qualquer um dos nomes de status especificados. Nomes de status padrão e definidos pelo usuário podem ser usados. Se o filtro statusNames for usado com o filtro "programIds", cada programa será verificado em busca de registros de associação cujo status corresponda a qualquer um dos nomes de status. Se um nome de status não for encontrado em nenhum dos programas, o erro "1003, Dados inválidos" será retornado.
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
      <td>Intervalo de datas</td>
      <td>Aceita um objeto JSON com os membros startAt e endAt. startAt aceita um datetime que representa a marca d'água inferior e endAt aceita um datetime que representa a marca d'água superior. O intervalo deve ser de 31 dias ou menos. Os datetimes devem estar em um formato ISO-8601, sem milissegundos. Trabalhos com esse tipo de filtro retornam todos os registros acessíveis que foram atualizados mais recentemente dentro do intervalo de datas.</td>
    </tr>
  </tbody>
</table>

O tipo de filtro não está disponível para algumas assinaturas. Se não estiver disponível para sua assinatura, você receberá um erro ao chamar o endpoint Criar trabalho de membro do programa de exportação (&quot;1035, Tipo de filtro não suportado para assinatura de destino&quot;). Os clientes podem entrar em contato com o Suporte da Marketo para ativar essa funcionalidade em suas assinaturas.

## Opções

O ponto de extremidade Criar Trabalho do Membro do Programa de Exportação fornece várias opções de formatação. Essas opções oferecem ao usuário a capacidade de:

- Especificar os campos a serem incluídos no arquivo exportado
- Renomear cabeçalhos de coluna desses campos
- Especificar o formato do arquivo exportado

| Parâmetro | Tipo de dados | Obrigatório | Observações |
|---|---|---|---|
| campos | Matriz[String] | Sim | O parâmetro fields aceita uma matriz JSON de cadeias de caracteres. Os campos listados são incluídos no arquivo exportado. Os seguintes tipos de campo podem ser exportados:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Especifique um campo usando seu nome de API REST, que pode ser recuperado usando Descrever lead2 e/ou Descrever endpoints de membros do programa. |
| columnHeaderNames | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| formato | Sequência de caracteres | Não | Aceita um dos seguintes: CSV, TSV, SSV. O arquivo exportado é renderizado como um arquivo de valores separados por vírgula, valores separados por tabulação ou valores separados por espaço, respectivamente, se definido. O padrão é CSV, caso não esteja definido. |


## Criação de um trabalho

Os parâmetros do processo são definidos antes do início da exportação usando o [Criar Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) terminal. Precisamos definir o `filter` contendo a id do programa e a variável `fields` que são necessários para a exportação. Opcionalmente, podemos definir a variável `format` do arquivo e o `columnHeaderNames`.

```
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

Isso retorna uma resposta de status indicando que o processo foi criado. A tarefa foi definida e criada, mas ainda não foi iniciada. Para isso, a [Enfileirar Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) o ponto de extremidade deve ser chamado usando o `exportId` na resposta do status de criação:

```
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

Isso responderá com uma primeira `status` de &quot;Em fila&quot;, após o que será definido como &quot;Processando&quot; quando houver um slot de exportação disponível.

## Status do trabalho de pesquisa

Observação: o status só pode ser recuperado para trabalhos criados pelo mesmo usuário da API.

Como esse é um endpoint assíncrono, depois de criar o trabalho, devemos sondar seu status para determinar seu progresso. Consultar usando o [Obter Status de Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) terminal. O status só é atualizado uma vez a cada 60 segundos, portanto, uma frequência de polling inferior a essa não é recomendada e, em quase todos os casos, ainda é excessiva. O campo de status pode responder com qualquer um dos seguintes: Criado, Em fila, Processando, Cancelado, Concluído, Falha.

```
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

O endpoint de status responde indicando que o trabalho ainda está sendo processado, portanto, o arquivo ainda não está disponível para recuperação. Uma vez que o trabalho `status` as alterações para &quot;Concluído&quot; estão disponíveis para download.

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

Para recuperar o arquivo de uma exportação concluída de membros do programa, basta chamar o [Obter arquivo de membro do programa de exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET) terminal com seu `exportId`.

A resposta contém um arquivo formatado da maneira que o trabalho foi configurado. O endpoint responde com o conteúdo do arquivo. Se um campo do membro do programa solicitado estiver vazio (não contiver dados), `null` é colocado no campo correspondente no arquivo de exportação.

```
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```
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

Para oferecer suporte à recuperação parcial e de fácil retomada dos dados extraídos, o endpoint do arquivo oferece suporte opcional ao Intervalo do cabeçalho HTTP do tipo bytes. Se o cabeçalho não estiver definido, todo o conteúdo será retornado. Você pode ler mais sobre como usar o cabeçalho Intervalo com o Marketo [Extração em massa](bulk-extract.md).

## Cancelar um trabalho

Se uma tarefa for configurada incorretamente ou se se tornar desnecessária, ela poderá ser facilmente cancelada usando o [Cancelar Trabalho de Membro do Programa de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) endpoint:

```
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

Isso responde com uma `status` indicando que o trabalho foi cancelado.
