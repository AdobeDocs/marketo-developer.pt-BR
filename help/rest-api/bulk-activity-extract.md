---
title: "Extração de atividade em massa"
feature: REST API
description: "Dados de atividade de processamento em lote do Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1381'
ht-degree: 7%

---


# Extração de atividade em massa

[Referência de Ponto de Extremidade de Extração de Atividade em Massa](https://developer.adobe.com/marketo-apis/api/mapi/)

O conjunto de Extração de atividade em massa de APIs REST fornece uma interface programática para recuperar grandes quantidades de dados de atividade do Marketo.  Para casos que não exigem baixa latência e devem transferir volumes significativos de dados de atividades para fora do Marketo, como integração de CRM, ETL, data warehouse e arquivamento de dados.

## Permissões

As APIs de Extração de atividade em massa exigem que o usuário da API tenha as permissões de &quot;Atividade somente leitura&quot; ou &quot;Atividade de leitura e gravação&quot;.

## Filtros

<table>
  <tbody>
    <tr>
      <td>Tipo de filtro</td>
      <td>Tipo de dados</td>
      <td>Obrigatório</td>
      <td>Observações</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>Intervalo de datas</td>
      <td>Sim</td>
      <td>Aceita um objeto JSON com os membros startAt e endAt. startAt aceita um datetime que representa a marca d'água inferior e endAt aceita um datetime que representa a marca d'água superior. O intervalo deve ser de 31 dias ou menos.Trabalhos com esse tipo de filtro retornam todos os registros acessíveis que foram criados dentro do intervalo de datas.Datetimes devem estar em formato ISO-8601, sem milissegundos.</td>
    </tr>
    <tr>
      <td>activityTypeIds</td>
      <td>Matriz[Inteiro]</td>
      <td>Não</td>
      <td>Aceita um objeto JSON com um membro, activityTypeIds. O valor deve ser uma matriz de números inteiros, correspondentes aos tipos de atividade desejados. A atividade "Excluir lead" não é suportada (use <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET">Obter Clientes Potenciais Excluídos</a>(ponto de extremidade). Recupere as IDs do tipo de atividade usando o<a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET">Obter tipos de atividade</a>terminal.</td>
    </tr>
    <tr>
      <td>primaryAttributeValueIds</td>
      <td>Matriz[Inteiro]</td>
      <td>Não</td>
      <td>Aceita um objeto JSON com um membro, primaryAttributeValueIds. O valor é uma matriz de ids que especifica os atributos primários para filtrar. No máximo 50 IDs podem ser especificadas. As IDs são o identificador exclusivo de um campo de cliente potencial ou de um ativo e podem ser recuperadas chamando o ponto de extremidade REST API apropriado. Por exemplo, para filtrar em um formulário específico para a atividade "Preencher formulário", passe o nome do formulário para <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Obter formulário por nome</a> ponto de extremidade para recuperar a ID do formulário. Veja a seguir uma lista de tipos de atividades em que a filtragem de atributo primário é suportada.
        <table>
          <tbody>
            <tr>
              <td>Tipo de atividade</td>
              <td>Id do Valor do Atributo Principal</td>
              <td>Ponto de Extremidade de Recuperação</td>
              <td>Grupo de ativos</td>
            </tr>
            <tr>
              <td>Alterar valor dos dados</td>
              <td>ID do campo de cliente potencial</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrever lead</a></td>
              <td>Nome do atributo</td>
            </tr>
            <tr>
              <td>Alterar pontuação</td>
              <td>ID do campo de cliente potencial</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrever lead</a></td>
              <td>Nome do atributo</td>
            </tr>
            <tr>
              <td>Alteração do status na progressão</td>
              <td>ID do programa</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET">Obter programa por nome</a></td>
              <td>Programa de marketing</td>
            </tr>
            <tr>
              <td>Adicionar à lista</td>
              <td>ID da lista estática</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Obter Lista Estática por Nome</a></td>
              <td>Lista estática</td>
            </tr>
            <tr>
              <td>Remover da lista</td>
              <td>ID da lista estática</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Obter Lista Estática por Nome</a></td>
              <td>Lista estática</td>
            </tr>
            <tr>
              <td>Preenchimento de formulário</td>
              <td>ID do formulário</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Obter formulário por nome</a></td>
              <td>Formulário da Web</td>
            </tr>
          </tbody>
        </table>
        Ao usar primaryAttributeValueIds, o filtro activityTypeIds deve estar presente e conter somente IDs de atividade que correspondam ao grupo de ativos correspondente.Exemplo:Por exemplo, se você estiver filtrando em ativos de Formulários Web, somente a ID de tipo de atividade "Preencher formulário" será permitida em activityTypeIds.Example Corpo de solicitação:{"filter":{"createdAt":{"startAt": "2021-07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValueIds" : [16,102,95,8]}}primaryAttributeValueIds e primaryAttributeValues não podem ser usados juntos.</td>
    </tr>
    <tr>
      <td>primaryAttributeValues</td>
      <td>Matriz[Cadeia de caracteres]</td>
      <td>Não</td>
      <td>Aceita um objeto JSON com um membro, primaryAttributeValues. O valor é uma matriz de nomes que especifica os atributos primários para filtrar. Um máximo de 50 nomes pode ser especificado. Os nomes são o identificador exclusivo de um campo de cliente potencial ou de um ativo e podem ser recuperados chamando o ponto de extremidade REST API apropriado. Por exemplo, para filtrar em um Formulário específico para a atividade "Preencher formulário", transmita a ID do Formulário para <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Obter formulário por ID</a> ponto de extremidade para recuperar o nome do formulário. Veja a seguir uma lista de tipos de atividades em que a filtragem de atributos primários é suportada.
        <table>
          <tbody>
            <tr>
              <td>Tipo de atividade</td>
              <td>Valor do atributo primário</td>
              <td>Ponto de Extremidade de Recuperação</td>
              <td>Grupo de ativos</td>
            </tr>
            <tr>
              <td>Alterar valor dos dados</td>
              <td>Nome para exibição do campo de cliente potencial</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrever lead</a></td>
              <td>Nome do atributo</td>
            </tr>
            <tr>
              <td>Alterar pontuação</td>
              <td>Nome para exibição do campo de cliente potencial</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrever lead</a></td>
              <td>Nome do atributo</td>
            </tr>
            <tr>
              <td>Alteração do status na progressão</td>
              <td>Nome do programa</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET">Obter Programa por Id</a></td>
              <td>Programa de marketing</td>
            </tr>
            <tr>
              <td>Adicionar à lista</td>
              <td>Nome da lista estática</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Obter Lista Estática por Id</a></td>
              <td>Lista estática</td>
            </tr>
            <tr>
              <td>Remover da lista</td>
              <td>Nome da lista estática</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Obter Lista Estática por Id</a></td>
              <td>Lista estática</td>
            </tr>
            <tr>
              <td>Preenchimento de formulário</td>
              <td>Nome do formulário</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Obter formulário por ID</a></td>
              <td>Formulário da Web</td>
            </tr>
          </tbody>
        </table>
        Observe que você deve usar "&lt;<em>programa</em>&gt;.&lt;<em>ativo</em>&gt;" para especificar exclusivamente o nome dos seguintes grupos de ativos: Programa de marketing, Lista estática, Formulário da Web.Exemplo:Por exemplo, um formulário com o nome "Saída MPS" que reside abaixo do programa com o nome "GL_OP_ALL_2021" seria especificado como "GL_OP_ALL_2021.Saída MPS".Exemplo Corpo da solicitação:{"filter":{"createdAt":{"startAt": "2021-07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValues":["GL_OP_ALL_2021.MPS de Saída"]}}Ao usar primaryAttributeValues, o filtro activityTypeIds deve estar presente e conter apenas IDs de atividade que correspondam ao grupo de ativos correspondente. Por exemplo, se você estiver filtrando em ativos de Formulários web, somente a ID do tipo de atividade "Preencher formulário" será permitida em activityTypeIds.primaryAttributeValues e primaryAttributeValueIds não podem ser usados juntos.</td>
    </tr>
  </tbody>
</table>

## Opções

| Parâmetro | Tipo de dados | Obrigatório | Observações |
|---|---|---|---|
| filtro | Matriz[Objeto] | Sim | Aceita uma matriz de filtros. Exatamente um filtro createdAt deve ser incluído na matriz. Um filtro activityTypeIds opcional pode ser incluído. Os filtros são aplicados ao conjunto de atividades acessível e o conjunto de atividades resultante é retornado pelo trabalho de exportação. |
| formato | Sequência de caracteres | Não | Aceita um dos seguintes: CSV, TSV, SSV O arquivo exportado é renderizado como valores separados por vírgula, valores separados por tabulação ou arquivo de valores separados por espaço, respectivamente, se definido. O padrão é CSV, se não definido. |
| columnHeaderNames | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| campos | Matriz[String] | Não | Matriz opcional de cadeias de caracteres que contêm valores de campo. Os campos listados são incluídos no arquivo exportado. Por padrão, os seguintes campos são retornados: `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`,Esse parâmetro pode ser usado para reduzir o número de campos retornados especificando um subconjunto da lista acima.Exemplo:&quot;campos&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Um campo adicional &quot;actionResult&quot; pode ser especificado para incluir a ação da atividade (&quot;bem-sucedido&quot;, &quot;ignorado&quot; ou &quot;falhou&quot;). |


## Criação de um trabalho

Para exportar registros, primeiro defina o trabalho e o conjunto de registros que deseja recuperar.  Crie o trabalho usando o [Criar Trabalho de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) terminal.  Ao exportar atividades, há dois filtros principais que podem ser aplicados: `createdAt`, que é sempre obrigatório, e `activityTypeIds`, que é opcional.  O filtro createdAt é usado para definir um intervalo de datas em que as atividades foram criadas, usando o `startAt` e `endAt` Os parâmetros do, que são campos de data e hora e representam a data de criação mais antiga permitida e a data de criação mais recente permitida, respectivamente.  Opcionalmente, também é possível filtrar apenas por determinados tipos de atividades, usando o `activityTypeIds` filtro.  Isso é útil para remover resultados que não são relevantes para seu caso de uso.

```
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

A tarefa agora tem um status de &quot;Criada&quot;, mas ainda não está na fila de processamento.  Para colocá-lo na fila para que ele possa começar a ser processado, devemos chamar o [Enfileirar tarefa de atividade de exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) endpoint usando a exportId da resposta de status de criação.

```
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

Agora, o status relata que a tarefa foi colocada em fila.  Quando um trabalhador se torna disponível para esse trabalho, o status é alternado para &quot;Processando&quot; e o trabalho começará a agregar registros do Marketo.

## Status do trabalho de pesquisa

O status do trabalho só pode ser recuperado para trabalhos criados pelo mesmo usuário da API.

A Extração de atividade em massa do Marketo é um endpoint assíncrono, portanto, o status do trabalho deve ser sondado para determinar quando o trabalho é concluído.  Consultar usando o [Obter Status do Trabalho de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) do seguinte modo:

```
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

Quando o trabalho for concluído, recupere os dados usando o [Obter arquivo de atividade de exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) terminal.

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

A resposta contém um arquivo formatado da maneira que o trabalho foi configurado. O endpoint responde com o conteúdo do arquivo.

Se um campo de cliente em potencial solicitado estiver vazio (não contiver dados), `then null` é colocado no campo correspondente no arquivo de exportação.  No exemplo abaixo, o campo campaignId da atividade retornada está vazio.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Para oferecer suporte à recuperação parcial e de fácil retomada dos dados extraídos, o endpoint do arquivo oferece suporte ao cabeçalho HTTP como opção `Range` do tipo `bytes`.  Se o cabeçalho não estiver definido, todo o conteúdo será retornado.  Você pode ler mais sobre como usar o cabeçalho Intervalo com o Marketo [Extração em massa](bulk-extract.md).

## Cancelar um trabalho

Se uma tarefa for configurada incorretamente ou se se tornar desnecessária, ela poderá ser facilmente cancelada usando o [Cancelar tarefa de atividade de exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) endpoint:

```
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
