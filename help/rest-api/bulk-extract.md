---
title: Extração em massa
feature: REST API
description: Saiba como usar a API REST de extração em massa do Marketo para exportar clientes em potencial, atividades, membros de programas e objetos personalizados, com OAuth, filas de trabalhos e limites diários de 500 MB.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
TQID: https://experienceleague.adobe.com/ECSchsjqp8fyxXbUGl5DgXHUkXuN0sIUc3yJfVaIe1E
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1549
ht-degree: 1%

---

# Extração em massa

A Extração em massa do Marketo fornece interfaces para recuperar grandes conjuntos de dados relacionados a pessoas e pessoas. Atualmente, as interfaces estão disponíveis para quatro tipos de objeto:

- Clientes Potenciais (Pessoas)
- Atividades
- Membros do programa
- Objetos personalizados

Para executar uma extração em massa:

1. Crie um trabalho e defina os dados a serem recuperados.
1. Enfileire a tarefa.
1. Aguarde até que o trabalho termine de gravar o arquivo.
1. Recuperar o arquivo por HTTP.

Os trabalhos de extração em massa são executados de forma assíncrona. Consulte o trabalho para recuperar o status de exportação.

`Note:` Os pontos de extremidade de API em massa não apresentam o prefixo &#39;/rest&#39; como outros pontos de extremidade.

## Autenticação

As APIs de extração em massa usam o mesmo método de autenticação OAuth 2.0 que outras APIs REST do Marketo. Envie um token de acesso válido no cabeçalho HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 31 de agosto de 2026. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

## Limites

- Máximo de trabalhos de exportação simultâneos: 2
- Máximo de trabalhos de exportação em fila, incluindo os trabalhos que estão sendo exportados no momento: 10
- Período de retenção do arquivo: sete dias
- Alocação de exportação diária padrão: 500 MB. A alocação é redefinida diariamente às 12h00, horário padrão da região central dos EUA. Os aumentos estão disponíveis para compra.
- Período máximo para o filtro de intervalo de datas (`createdAt` ou `updatedAt`): 31 dias

Os filtros de Extração de lead em massa para UpdatedAt e Smart List não estão disponíveis para alguns tipos de assinatura. Se esses filtros não estiverem disponíveis, o ponto de extremidade Criar trabalho de lead de exportação retornará o erro &quot;1035, Tipo de filtro não suportado para assinatura de destino&quot;. Entre em contato com o Suporte da Marketo para ativar essa funcionalidade para sua assinatura.

### Fila

As APIs de extração em massa usam uma fila de trabalhos compartilhada entre clientes potenciais, atividades, membros de programas e objetos personalizados. Primeiro, chame um endpoint Create Export Lead/Activity/Program Member Job (Criar cargo de lead/atividade/membro do programa de exportação) para criar um trabalho de extração. Em seguida, chame o endpoint correspondente de Enfileiramento de Lead de Exportação/Atividade/Tarefa do Membro do Programa para enfileirar a tarefa. O trabalho começa quando os recursos de computação ficam disponíveis.

A fila pode conter no máximo 10 tarefas. Se você tentar enfileirar um trabalho quando a fila estiver cheia, o ponto de extremidade Enfileirar tarefa de exportação retornará o erro &quot;1029, Muitos trabalhos na fila&quot;. No máximo dois trabalhos podem ter o status de &quot;Processando&quot; e serem executados simultaneamente.

### Tamanho do arquivo

As APIs de extração em massa são medidas com base no tamanho em disco dos dados que um trabalho de extração em massa recupera. Para determinar o tamanho do arquivo em bytes, leia o atributo `fileSize` na resposta de status concluída para um trabalho de exportação.

A cota diária é de 500 MB e é compartilhada entre clientes potenciais, atividades, membros de programa e objetos personalizados. Quando a cota é excedida, não é possível criar ou colocar na fila outro trabalho até que a cota seja redefinida à meia-noite [Horário Central](https://en.wikipedia.org/wiki/Central_Time_Zone). Até a redefinição, a API retorna o erro &quot;1029, Exportação de cota diária excedida&quot;. Além da cota diária, não há tamanho máximo de arquivo.

Depois que um trabalho é enfileirado ou processado, ele é executado até a conclusão, a menos que ocorra um erro ou você cancele o trabalho. Se uma tarefa falhar, você deverá recriá-la.

A API grava o arquivo completo somente quando o trabalho atinge o estado concluído. Ele não grava arquivos parciais. Para verificar o arquivo, calcule o hash SHA-256 e compare-o com a soma de verificação retornada pelo endpoint de status do trabalho.

Para determinar o espaço em disco total usado para o dia atual, chame um endpoint Obter lead de exportação/atividade/tarefas de membro do programa. Esses pontos de extremidade retornam todos os trabalhos dos últimos sete dias.

Filtre a lista para trabalhos que foram concluídos durante o dia atual usando os atributos `status` e `finishedAt`. Em seguida, adicione os tamanhos dos arquivos desses trabalhos. Não é possível excluir um arquivo para recuperar espaço em disco.

## Permissões

A Extração em massa usa o mesmo modelo de permissões que a API REST do Marketo. Ela não requer permissões especiais adicionais, mas cada conjunto de endpoints requer permissões específicas.

Somente o usuário da API que criou um trabalho de Extração em massa pode acessá-lo, sondar seu status ou recuperar o conteúdo do arquivo.

Os pontos de extremidade de Extração em massa não reconhecem os espaços de trabalho do Marketo. As solicitações de extração incluem dados de todos os espaços de trabalho, independentemente de como você define a API somente de usuário para o serviço personalizado.

## Criação de um trabalho

As APIs de extração em massa do Marketo usam trabalhos para iniciar e executar extrações de dados. A solicitação a seguir cria um trabalho de exportação de clientes potenciais:

```http
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name"
   },
   "filter": {
      "createdAt": {
         "startAt": "2023-01-01T00:00:00Z",
         "endAt": "2023-01-31T00:00:00Z"
      }
   }
}
```

Essa solicitação cria um trabalho que exporta cada lead criado entre 1º de janeiro de 2023 e 31 de janeiro de 2023. O arquivo CSV contém valores dos campos &quot;firstName&quot; e &quot;lastName&quot; e usa os cabeçalhos de coluna &quot;First Name&quot; e &quot;Last Name&quot;.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2023-01-21T11:47:30-08:00",
         "queuedAt": "2023-01-21T11:48:30-08:00",
         "format": "CSV",
      }
   ]
}
```

A resposta retorna a ID do trabalho no atributo `exportId`. Use essa ID de tarefa para enfileirar ou cancelar a tarefa, verificar seu status ou recuperar o arquivo concluído.

### Parâmetros comuns

Cada endpoint de criação de trabalho tem parâmetros comuns para configurar o formato de arquivo, nomes de campo e filtro. Cada subtipo de trabalho de extração também pode ter parâmetros adicionais:

| Parâmetro | Tipo de dados | Observações |
| --- | --- | --- |
| formato | String | Determina o formato de arquivo dos dados extraídos com opções para valores separados por vírgula, valores separados por tabulação e valores separados por ponto e vírgula. Aceita um dos seguintes: CSV, SSV, TSV. O formato é padronizado como CSV. |
| columnHeaderNames | Objeto | Permite definir os nomes dos cabeçalhos de coluna no arquivo retornado. Cada chave do membro é o nome do cabeçalho da coluna a ser renomeado, e o valor é o novo nome do cabeçalho da coluna. Por exemplo, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filtro | Objeto | Filtro aplicado ao trabalho de extração. Os tipos e as opções variam entre os tipos de trabalho. |

## Recuperando tarefas

Use o ponto de extremidade Obter Trabalhos de Exportação para o tipo de objeto correspondente para recuperar trabalhos recentes. Cada endpoint de Obter Trabalhos de Exportação oferece suporte a estes parâmetros:

- `status` filtra trabalhos por status de exportação. Os valores válidos são Criado, Enfileirado, Processando, Cancelado, Concluído e Falha.
- `batchSize` limita o número de trabalhos retornados. O valor padrão e máximo é 300.
- `nextPageToken` páginas por meio de conjuntos de resultados grandes.

A solicitação a seguir recupera trabalhos de exportação de clientes potenciais com status Concluído ou Falha:

```http
GET /bulk/v1/leads/export.json?status=Completed,Failed
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
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
      ...
   ]
}
```

A matriz de resultados contém a resposta de status para cada job criado para esse tipo de objeto durante os últimos sete dias. A resposta inclui somente os trabalhos que pertencem ao usuário da API que faz a chamada.

## Iniciar um trabalho

Depois de criar uma tarefa, use sua ID para enfileirá-la e iniciá-la:

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

A solicitação inicia o processo e retorna uma resposta de status. Como as exportações são executadas de forma assíncrona, sonde o status do trabalho para determinar quando a exportação é concluída.

## Status do trabalho de pesquisa

Consulte o endpoint de status para determinar o progresso de um trabalho. Somente o usuário da API que criou um trabalho pode sondar seu status.

Um status de trabalho não é atualizado com mais frequência do que uma vez a cada 60 segundos. Não faça enquetes com mais frequência do que isso. Para a maioria dos casos de uso, pesquisar uma vez a cada 5 minutos é suficiente. Os dados de cada exportação bem-sucedida são mantidos por 10 dias.

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
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:d9c73f0b6960c71623c8bafe29603b3e8e20fd0e4eeaefd119c0227506ea9be4"
      }
   ]
}
```

O membro interno `status` indica o progresso do trabalho. Seu valor pode ser Criado, Enfileirado, Processando, Cancelado, Concluído ou Falha.

Neste exemplo, o trabalho está concluído, portanto, é possível interromper a pesquisa e recuperar o arquivo. Para um trabalho concluído, o membro `fileSize` indica o comprimento total do arquivo em bytes, e o membro `fileChecksum` contém o hash SHA-256 do arquivo. O status do trabalho fica disponível por 30 dias após o trabalho atingir o status Concluído ou Falha.

## Recuperação de dados

Após a conclusão do trabalho, recupere o arquivo exportado:

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

A resposta contém o arquivo no formato configurado para o trabalho. Se o trabalho estiver incompleto ou a solicitação contiver uma ID de trabalho inválida, o endpoint do arquivo retornará um status 404 Não encontrado e uma mensagem de erro de texto sem formatação. Essa resposta é diferente da maioria das outras respostas de endpoint REST do Marketo.

Para oferecer suporte à recuperação parcial e retomável, o ponto de extremidade do arquivo oferece suporte ao cabeçalho HTTP `Range` opcional com o tipo `bytes`, conforme definido em [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233). Se você não definir o cabeçalho, o endpoint retornará o arquivo inteiro.

Para recuperar os primeiros 10.000 bytes de um arquivo, passe o seguinte cabeçalho na solicitação GET. O intervalo começa no byte 0:

```text
Range: bytes=0-9999
```

Para um arquivo parcial, o endpoint retorna o código de status 206 e os cabeçalhos Accept-range, Content-Length e Content-Range:

```text
Accept-Ranges: bytes
Content-Length: 10000
Content-Range: bytes 0-9999/123424
```

### Recuperação e retomada parciais

Use o cabeçalho `Range` para recuperar parte de um arquivo ou retomar uma recuperação. O intervalo de arquivos começa no byte 0 e termina no valor de `fileSize` menos 1. O ponto de extremidade Get Export File também relata o comprimento do arquivo como o denominador no cabeçalho de resposta `Content-Range`.

Se uma recuperação falhar parcialmente, você poderá retomá-la. Por exemplo, se você tentar recuperar um arquivo de 1000 bytes, mas receber apenas os primeiros 725 bytes, chame o endpoint novamente e passe um novo intervalo:

```text
Range: bytes=725-999
```

Essa solicitação retorna os 275 bytes restantes do arquivo.

#### Verificação da integridade do arquivo

Quando `status` é &quot;Concluído&quot;, os pontos de extremidade do status do trabalho retornam uma soma de verificação no atributo `fileChecksum`. A soma de verificação é o hash SHA-256 do arquivo exportado. Compare-o com o hash SHA-256 do arquivo recuperado para verificar se o arquivo está completo.

A resposta a seguir contém uma soma de verificação:

```json
{
    "exportId": "45547609-6732-418a-bb7b-17b0160b2317",
    "format": "CSV",
    "status": "Completed",
    "createdAt": "2019-06-04T23:13:12Z",
    "queuedAt": "2019-06-04T23:14:02Z",
    "startedAt": "2019-06-04T23:15:19Z",
    "finishedAt": "2019-06-04T23:36:40Z",
    "numberOfRecords": 1776,
    "fileSize": 400785,
    "fileChecksum": "sha256:83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6"
}
```

O exemplo a seguir usa o utilitário de linha de comando sha256sum para criar o hash SHA-256 de um arquivo recuperado chamado &quot;bulk_lead_export.csv&quot;:

```bash
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Cancelar um trabalho

Se uma tarefa for configurada incorretamente ou não for mais necessária, cancele-a:

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
         "format": "CSV",
      }
   ]
}
```

O status da resposta indica que a tarefa foi cancelada.
