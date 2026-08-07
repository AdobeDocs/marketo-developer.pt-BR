---
title: Extração de chumbo em massa
feature: REST API
description: Saiba como usar as APIs REST de extração de lead em massa do Marketo para exportar leads em massa com filtros de data, lista e lista inteligente, campos personalizados e formatos CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
TQID: https://experienceleague.adobe.com/4eMJR87fHDdccrVid3wHtspvBVQmrBGHYMlIwFCSdEI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1017
ht-degree: 3%

---

# Extração de chumbo em massa

[Referência de Ponto de Extremidade de Extração de Lead em Massa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads)

As APIs REST de extração de lead em massa recuperam grandes conjuntos de registros de lead/pessoa da Marketo. Você também pode recuperar clientes em potencial de forma incremental com base na data de criação do registro, na atualização mais recente, na associação à lista estática ou na associação à lista inteligente.

Use a extração de leads em massa para a troca contínua de dados entre a Marketo e sistemas externos, incluindo ETL, data warehouse e workflows de arquivamento.

## Permissões

O usuário da API que é proprietário do trabalho deve ter uma função com a permissão Lead somente leitura, com a permissão Lead de leitura e gravação ou com ambas as permissões.

## Filtros

Os trabalhos de exportação de clientes potenciais são compatíveis com vários tipos de filtro. Cada trabalho de exportação pode usar apenas um tipo de filtro.

Os filtros `updatedAt`, `smartListName` e `smartListId` exigem uma infraestrutura que não está disponível em todas as assinaturas.

| Tipo de filtro | Tipo de dados | Observações |
| --- | --- | --- |
| createdAt | Date Range | Um objeto JSON com `startAt` e `endAt` membros. `startAt` é o datetime de marca d&#39;água baixa e `endAt` é o datetime de marca d&#39;água alta. Use valores de data e hora ISO-8601 sem milissegundos. O intervalo deve ser de 31 dias ou menos. A tarefa retorna todos os registros acessíveis criados dentro do intervalo de datas. |
| updatedAt* | Date Range | Um objeto JSON com `startAt` e `endAt` membros. `startAt` é o datetime de marca d&#39;água baixa e `endAt` é o datetime de marca d&#39;água alta. Use valores de data e hora ISO-8601 sem milissegundos. O intervalo deve ser de 31 dias ou menos. Este filtro não usa o campo `updatedAt` visível, que reflete atualizações somente para campos padrão. Em vez disso, ele usa o tempo da atualização de campo mais recente para um registro de cliente potencial. O processo retorna todos os registros acessíveis atualizados mais recentemente dentro do intervalo de datas. |
| staticListName | String | O nome de uma lista estática. A tarefa retorna todos os registros acessíveis que são membros da lista estática quando a tarefa começa a ser processada. Recupere nomes de lista estáticos usando o ponto de extremidade Obter Listas. |
| staticListId | Número inteiro | A ID de uma lista estática. A tarefa retorna todos os registros acessíveis que são membros da lista estática quando a tarefa começa a ser processada. Recupere IDs de lista estáticas usando o ponto de extremidade Obter Listas. |
| smartListName* | String | O nome de uma lista inteligente. A tarefa retorna todos os registros acessíveis que são membros da lista inteligente quando a tarefa começa a ser processada. Recupere nomes de listas inteligentes usando o ponto de extremidade Obter Smart Lists. |
| smartListId* | Inteiro | A ID de uma lista inteligente. A tarefa retorna todos os registros acessíveis que são membros da lista inteligente quando a tarefa começa a ser processada. Recupere as IDs das listas inteligentes usando o ponto de extremidade Obter listas inteligentes. |

Os tipos de filtro marcados com um asterisco não estão disponíveis para algumas assinaturas. Se um tipo de filtro não estiver disponível para sua assinatura, o ponto de extremidade Criar trabalho de lead de exportação retornará o erro &quot;1035, Tipo de filtro não suportado para assinatura de destino&quot;. Entre em contato com o Suporte da Marketo para ativar essa funcionalidade para sua assinatura.

## Opções

O ponto de extremidade Criar trabalho de lead de exportação fornece opções para selecionar campos exportados, renomear cabeçalhos de coluna e definir o formato de arquivo.

| Parâmetro | Tipo de dados | Obrigatório | Observações |
| --- | --- | --- | --- |
| campos | Matriz[Cadeia de Caracteres] | Sim | Uma matriz JSON de cadeias de caracteres. Cada string deve ser o nome da API REST de um campo de lead Marketo. A exportação inclui cada campo listado e usa seu nome de API REST como o cabeçalho da coluna, a menos que `columnHeaderNames` o substitua. Quando o recurso [!DNL Adobe Experience Cloud Audience Sharing] está habilitado, um processo de sincronização de cookies associa a [!DNL Adobe Experience Cloud] ID (ECID) ao Marketo leads. Especifique o campo `ecids` para incluir ECIDs no arquivo de exportação. |
| columnHeaderNames | Objeto | Não | Um objeto JSON de pares de valores-chave de campo e cabeçalho de coluna. Cada chave deve ser o nome da API de um campo incluído no trabalho de exportação. Recupere o nome da API chamando Descrever lead. Cada valor é o cabeçalho de coluna exportado para esse campo. |
| formato | String | Não | O formato do arquivo de exportação: CSV para valores separados por vírgula, TSV para valores separados por tabulação ou SSV para valores separados por espaço. O padrão é CSV. |

## Criação de um trabalho

Use o ponto de extremidade [Criar Trabalho de Exportação de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/createExportLeadsUsingPOST) para definir um trabalho de exportação. Especifique o `fields` a ser exportado, um tipo de `filter` e seus parâmetros, o arquivo `format` e qualquer nome de cabeçalho de coluna personalizado.

```http
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
         "endAt": "2017-01-31T00:00:00Z"
      }
   }
}
```

Essa solicitação cria um trabalho de exportação para clientes potenciais criados entre 1º de janeiro de 2017 e 31 de janeiro de 2017. A exportação inclui valores dos campos `firstName`, `lastName`, `id` e `email`.

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

A resposta confirma que o job foi criado, mas não foi iniciado. Para iniciar o trabalho, chame o ponto de extremidade [Enfileirar Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/enqueueExportLeadsUsingPOST) com o `exportId` da resposta de criação.

```http
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

A resposta do enfileiramento tem um `status` de &quot;Em fila&quot;. Quando um slot de exportação se torna disponível, o status muda para &quot;Processando&quot;.

## Status do trabalho de pesquisa

Você pode recuperar o status somente para trabalhos criados pelo mesmo usuário da API.

Os trabalhos de exportação de clientes potenciais são executados de forma assíncrona. Sonde o ponto de extremidade [Obter Status do Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportLeadsStatusUsingGET) para acompanhar o progresso do trabalho.

O status é atualizado apenas uma vez a cada 60 segundos. Não faça enquetes com mais frequência; em quase todos os casos, esse intervalo ainda é excessivo.

```http
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

Essa resposta mostra que a tarefa ainda está sendo processada, portanto, o arquivo não está disponível. Quando o status do trabalho muda para &quot;Concluído&quot;, o arquivo está pronto para download.

O campo `status` pode retornar qualquer um dos seguintes valores:

- Criado
- Enfileirado
- Processamento
- Cancelado
- Concluído
- Falha

## Recuperação de dados

Para recuperar uma exportação de clientes potenciais concluída, chame o ponto de extremidade [Obter Arquivo de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getExportLeadsFileUsingGET) com o `exportId`.

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

O corpo da resposta contém o arquivo no formato configurado para o trabalho.

Se um campo de cliente potencial solicitado não contiver dados, o campo correspondente no arquivo de exportação conterá `null`. No exemplo a seguir, o lead retornado tem um campo de email vazio.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Para recuperação parcial ou retomável, o ponto de extremidade do arquivo dá suporte ao cabeçalho HTTP `Range` opcional com o tipo `bytes`. Se você não definir o cabeçalho, o endpoint retornará todo o conteúdo. Saiba mais sobre como usar o cabeçalho `Range` com a [Extração em massa](bulk-extract.md) do Marketo.

## Cancelar um trabalho

Para cancelar um trabalho desnecessário ou configurado incorretamente, chame o ponto de extremidade [Cancelar Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi#operation/cancelExportLeadsUsingPOST).

```http
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

A resposta confirma que o processo foi cancelado.
