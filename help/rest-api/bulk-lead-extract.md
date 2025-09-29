---
title: Extração de chumbo em massa
feature: REST API
description: Saiba como usar as APIs REST de extração de lead em massa do Marketo para exportar leads em massa com filtros de data, lista e lista inteligente, campos personalizados e formatos CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 2%

---

# Extração de chumbo em massa

[Referência de Ponto de Extremidade de Extração de Cliente Potencial em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

O conjunto de Extração de lead em massa de APIs REST fornece uma interface programática para recuperar grandes conjuntos de registros de lead/pessoa do Marketo. Além disso, ele pode ser usado para recuperar clientes potenciais de forma incremental com base na data de criação do registro, na atualização mais recente, na associação de lista estática ou na associação de lista inteligente. A interface recomendada para casos de uso que exigem intercâmbio contínuo de dados entre o Marketo e um ou mais sistemas externos, para fins de ETL, data warehouse e arquivamento.

## Permissões

As APIs de Extração de lead em massa exigem que o usuário da API responsável tenha uma função com uma ou ambas as permissões de Lead somente leitura ou Lead de leitura e gravação.

## Filtros

Os clientes em potencial são compatíveis com várias opções de filtro. Determinados filtros, incluindo `updatedAt`, `smartListName` e `smartListId`, exigem componentes de infraestrutura adicionais que ainda não foram implementados em todas as assinaturas. Somente um tipo de filtro pode ser especificado por trabalho de exportação.

| Tipo de filtro | Tipo de dados | Observações |
|---|---|---|
| createdAt | Date Range | Aceita um objeto JSON com os membros `startAt` e `endAt`. `startAt` aceita um datetime que representa a marca d&#39;água inferior e `endAt` aceita um datetime que representa a marca d&#39;água superior. O intervalo deve ser de 31 dias ou menos. Os datetimes devem estar em um formato ISO-8601, sem milissegundos. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que foram criados dentro do intervalo de datas. |
| updatedAt* | Date Range | Aceita um objeto JSON com os membros `startAt` e `endAt`. `startAt` aceita um datetime que representa a marca d&#39;água inferior e `endAt` aceita um datetime que representa a marca d&#39;água superior. O intervalo deve ser de 31 dias ou menos. Os datetimes devem estar em um formato ISO-8601, sem milissegundos. Observação: esse filtro não filtra no campo &quot;updatedAt&quot; visível, que reflete somente atualizações em campos padrão. Ela filtra com base em quando a atualização de campo mais recente foi feita em um registro de cliente potencialJobs com esse tipo de filtro retorna todos os registros acessíveis que foram atualizados mais recentemente dentro do intervalo de datas. |
| staticListName | String | Aceita o nome de uma lista estática. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros da lista estática no momento em que o trabalho começa a ser processado. Recupere nomes de lista estáticos usando o ponto de extremidade Get Lists. |
| staticListId | Inteiro | Aceita a id de uma lista estática. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros da lista estática no momento em que o trabalho começa a ser processado. Recupere ids de lista estáticas usando o ponto de extremidade Get Lists. |
| smartListName* | String | Aceita o nome de uma lista inteligente. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros das smart lists no momento em que o trabalho começa a ser processado. Recupere nomes de listas inteligentes usando o ponto de extremidade Obter Smart Lists. |
| smartListId* | Inteiro | Aceita a ID de uma lista inteligente. Os trabalhos com esse tipo de filtro retornam todos os registros acessíveis que são membros das smart lists no momento em que o trabalho começa a ser processado. Recupere as IDs das listas inteligentes usando o ponto de extremidade Obter listas inteligentes. |

O tipo de filtro não está disponível para algumas assinaturas. Se não estiver disponível para sua assinatura, você receberá um erro ao chamar o endpoint Criar trabalho de lead de exportação (&quot;1035, Tipo de filtro não suportado para assinatura de destino&quot;). Os clientes podem entrar em contato com o Suporte da Marketo para ativar essa funcionalidade em suas assinaturas.

## Opções

O ponto de extremidade Criar trabalho de lead de exportação fornece várias opções de formatação, dando ao usuário a capacidade de incluir campos específicos no arquivo exportado, a capacidade de renomear cabeçalhos de coluna desses campos e o formato do arquivo exportado.

| Parâmetro | Tipo de dados | Obrigatório | Observações |
|---|---|---|---|
| campos | Matriz[Cadeia de Caracteres] | Sim | O parâmetro fields aceita uma matriz JSON de cadeias de caracteres. Cada string deve ser o nome da API REST de um campo de lead Marketo. Os campos listados são incluídos no arquivo exportado. O cabeçalho de coluna para cada campo será o nome da API REST de cada campo, a menos que seja substituído por columnHeader. Observação: quando o recurso [!DNL Adobe Experience Cloud Audience Sharing] é habilitado, ocorre um processo de sincronização de cookies que associa a [!DNL Adobe Experience Cloud] ID (ECID) a clientes potenciais do Marketo. Você pode especificar o campo &quot;ecids&quot; para incluir ECIDs no arquivo de exportação. |
| columnHeaderNames | Objeto | Não | Um objeto JSON que contém pares de valores chave de nomes de campos e cabeçalhos de coluna. A chave deve ser o nome de um campo incluído no trabalho de exportação. Esse é o nome da API do campo que pode ser recuperado chamando Descrever lead. O valor é o nome do cabeçalho de coluna exportado para esse campo. |
| formato | String | Não | Aceita um dos seguintes: CSV, TSV, SSV. O arquivo exportado é renderizado como um arquivo de valores separados por vírgula, valores separados por tabulação ou valores separados por espaço, respectivamente, se definido. O padrão é CSV, caso não esteja definido. |

## Criação de um trabalho

Os parâmetros do trabalho são definidos antes do início da exportação usando o ponto de extremidade [Criar Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST). Devemos definir os `fields` necessários para a exportação, o tipo de parâmetros do `filter`, o `format` do arquivo e os nomes de cabeçalho de coluna, se houver.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName",
      "id",
      "email"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "id": "Marketo Id",
      "email": "Email Address"
   },
   "filter": {
      "createdAt": {
         "startAt": "2017-01-01T00:00:00Z",
         "`endAt`": "2017-01-31T00:00:00Z"
      }
   }
}
```

Esta solicitação começará a exportar um conjunto de clientes potenciais criados entre 1º de janeiro de 2017 e 31 de janeiro de 2017, incluindo os valores dos campos `firstName`, `lastName`, `id` e `email` correspondentes.

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

Isso retorna uma resposta de status indicando que o processo foi criado. A tarefa foi definida e criada, mas ainda não foi iniciada. Para fazer isso, o ponto de extremidade [Enfileirar Trabalho de Cliente Potencial de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) deve ser chamado usando o exportId da resposta do status de criação:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "147e4#16b24d9b913",
    "result": [
        {
            "exportId": "fad2cd1b-e822-4025-be1e-9caa9cf1d4b8",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2019-06-04T23:35:43Z",
            "queuedAt": "2019-06-04T23:36:17Z"
        }
    ],
    "success": true
}
```

Isso responde com uma `status` de &quot;Em fila&quot;, após a qual será definido como &quot;Processando&quot; quando houver um slot de exportação disponível.

## Status do trabalho de pesquisa

O status `Note:` só pode ser recuperado para trabalhos que foram criados pelo mesmo usuário da API.

Como esse é um endpoint assíncrono, depois de criar o trabalho, devemos pesquisar seu status para determinar seu progresso. Sondar usando o ponto de extremidade [Obter Status do Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). O status só é atualizado uma vez a cada 60 segundos, portanto, uma frequência de polling inferior a essa não é recomendada e, em quase todos os casos, ainda é excessiva. Vamos dar uma olhada na pesquisa.

```
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

O endpoint de status responde indicando que o trabalho ainda está sendo processado, portanto, o arquivo ainda não está disponível para recuperação. Quando o status do trabalho for alterado para &quot;Concluído&quot;, ele foi preparado para download.

O campo de status pode responder com qualquer um dos seguintes:

- Criado
- Enfileirado
- Processamento
- Cancelado
- Concluído
- Falha

## Recuperação de dados

Para recuperar o arquivo de uma exportação de clientes potenciais concluída, basta chamar o [Obter Arquivo de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) com seu `exportId`.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

A resposta contém um arquivo formatado da maneira que o trabalho foi configurado. O endpoint responde com o conteúdo do arquivo.

Se um campo de cliente em potencial solicitado estiver vazio (não contiver dados), `null` será colocado no campo correspondente no arquivo de exportação. No exemplo abaixo, o campo de email do lead retornado está vazio.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Para oferecer suporte à recuperação parcial e de fácil retomada dos dados extraídos, o endpoint do arquivo oferece suporte opcional ao Intervalo do cabeçalho HTTP do tipo bytes. Se o cabeçalho não estiver definido, todo o conteúdo será retornado. Leia mais sobre como usar o cabeçalho Intervalo com a [Extração em massa](bulk-extract.md) do Marketo.

## Cancelar um trabalho

Se um trabalho for configurado incorretamente ou se se tornar desnecessário, ele poderá ser facilmente cancelado usando o ponto de extremidade [Cancelar Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST):

```
POST /bulk/v1/leads/export/{exportId}/cancel.json
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

Isso responde com um status indicando que o trabalho foi cancelado.
