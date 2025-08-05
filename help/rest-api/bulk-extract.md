---
title: Extração em massa
feature: REST API
description: Operações em lote para extração de dados do Marketo.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1682'
ht-degree: 1%

---

# Extração em massa

O Marketo fornece interfaces para a recuperação de grandes conjuntos de dados pessoais e relacionados a pessoas, chamados de Extração em massa. Atualmente, as interfaces são oferecidas para três tipos de objeto:

- Clientes Potenciais (Pessoas)
- Atividades
- Membros do programa
- Objetos personalizados

A extração em massa é executada criando um trabalho, definindo o conjunto de dados a serem recuperados, enfileirando o trabalho, aguardando a conclusão do trabalho gravando um arquivo e recuperando o arquivo por HTTP. Esses trabalhos são executados de forma assíncrona e podem ser pesquisados para recuperar o status da exportação.

`Note:` Os pontos de extremidade de API em massa não apresentam o prefixo &#39;/rest&#39; como outros pontos de extremidade.

## Autenticação

As APIs de extração em massa usam o mesmo método de autenticação OAuth 2.0 que outras APIs REST do Marketo. Isso requer que um token de acesso válido seja enviado como um cabeçalho HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 30 de junho de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

## Limites

- Máximo de Trabalhos de Exportação Simultâneos: 2
- Máximo de trabalhos de exportação em fila (incluindo os trabalhos que estão sendo exportados no momento): 10
- Período de retenção do arquivo: sete dias
- Alocação de exportação diária padrão: 500 MB (que é redefinido diariamente no CST 12:00AM). Aumentos disponíveis para compra.
- Período máximo para o filtro de intervalo de datas (createdAt ou updatedAt): 31 dias

Os filtros de Extração de lead em massa para UpdatedAt e Smart List não estão disponíveis para alguns tipos de assinatura. Se não estiver disponível, uma chamada para o ponto de extremidade Criar trabalho de lead de exportação retornará o erro &quot;1035, Tipo de filtro não suportado para assinatura de destino&quot;. Os clientes podem entrar em contato com o Suporte da Marketo para ativar essa funcionalidade em suas assinaturas.

### Fila

As APIs de extração em massa usam uma fila de trabalhos (compartilhada entre clientes potenciais, atividades, membros de programas e objetos personalizados). Os trabalhos de extração devem primeiro ser criados e, em seguida, enfileirados, chamando Criar tarefa de lead/atividade/membro do programa de exportação e Enfileirar pontos finais de tarefa de lead/atividade/membro do programa de exportação. Uma vez enfileiradas, as tarefas são extraídas da fila e iniciadas quando os recursos de computação ficam disponíveis.

O número máximo de tarefas na fila é 10. Se você tentar enfileirar um trabalho quando a fila estiver cheia, o ponto de extremidade Enfileirar tarefa de exportação retornará o erro &quot;1029, Muitos trabalhos na fila&quot;. No máximo dois trabalhos podem ser executados simultaneamente (o status é &quot;Processando&quot;).

### Tamanho do arquivo

As APIs de extração em massa são medidas com base no tamanho em disco dos dados recuperados por um trabalho de extração em massa. O tamanho explícito em bytes para um trabalho pode ser determinado pela leitura do atributo `fileSize` da resposta de status concluída de um trabalho de exportação.

A cota diária é de no máximo 500 MB por dia, compartilhados entre clientes potenciais, atividades, membros de programas e objetos personalizados. Quando a cota é excedida, você não pode Criar ou Enfileirar outro trabalho até que a cota diária seja redefinida à meia-noite [Hora Central](https://en.wikipedia.org/wiki/Central_Time_Zone). Até lá, é retornado o erro &quot;1029, Export daily quota aded&quot;. Além da cota diária, não há tamanho máximo de arquivo.

Quando uma tarefa está na fila ou em processamento, ela é executada até a conclusão (exceto por um erro ou cancelamento de tarefa). Se uma tarefa falhar por algum motivo, você deverá recriá-la. Os arquivos são totalmente gravados somente quando um trabalho atinge o estado concluído (arquivos parciais nunca são gravados). Você pode verificar se um arquivo foi totalmente gravado calculando seu hash SHA-256 e comparando-o com a soma de verificação retornada pelos pontos de extremidade de status do trabalho.

Você pode determinar a quantidade total de discos usados para o dia atual, chamando Obter lead de exportação/atividade/tarefas de membro do programa. Esses endpoints retornam uma lista de todas as tarefas nos últimos sete dias. Você pode filtrar essa lista apenas para os trabalhos concluídos no dia atual (usando os atributos `status` e `finishedAt`). Em seguida, some os tamanhos dos arquivos desses trabalhos para produzir a quantidade total usada. Não há como excluir um arquivo para recuperar espaço em disco.

## Permissões

A Extração em massa usa o mesmo modelo de permissões que a API REST do Marketo e não requer permissões especiais adicionais para uso, embora permissões específicas sejam necessárias para cada conjunto de endpoints.

Os trabalhos de extração em massa só podem ser acessados pelo usuário da API que os criou, incluindo a pesquisa de status e a recuperação do conteúdo do arquivo.

Os pontos de extremidade de Extração em massa não reconhecem os espaços de trabalho do Marketo. As solicitações de extração sempre incluem dados em todos os espaços de trabalho, independentemente de como você define a API somente de usuário para o serviço personalizado.

## Criação de um trabalho

As APIs de extração em massa do Marketo usam o conceito de um trabalho para iniciar e executar a extração de dados. Vamos analisar a criação de um trabalho simples de exportação de clientes potenciais.

```
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

Essa solicitação simples construirá um trabalho que retornará os valores contidos nos campos &quot;firstName&quot; e &quot;lastName&quot;, com os cabeçalhos de coluna &quot;First Name&quot; e &quot;Last Name&quot; como um arquivo CSV, contendo cada lead criado entre 1º de janeiro de 2023 e 31 de janeiro de 2023.

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

Quando criamos o trabalho, ele retorna uma ID de trabalho no atributo `exportId`. Podemos então usar essa ID de trabalho para enfileirar o trabalho, cancelá-lo, verificar seu status ou recuperar o arquivo concluído.

### Parâmetros comuns

Cada endpoint de criação de trabalho compartilha alguns parâmetros comuns para configurar o formato de arquivo, nomes de campo e filtro de um trabalho de extração em massa. Cada subtipo de trabalho de extração pode ter parâmetros adicionais:

| Parâmetro | Tipo de dados | Observações |
|---|---|---|
| formato | String | Determina o formato de arquivo dos dados extraídos com opções para valores separados por vírgula, valores separados por tabulação e valores separados por ponto e vírgula. Aceita um dos seguintes: CSV, SSV, TSV. O formato é padronizado como CSV. |
| columnHeaderNames | Objeto | Permite definir os nomes dos cabeçalhos de coluna no arquivo retornado. Cada chave do membro é o nome do cabeçalho da coluna a ser renomeado, e o valor é o novo nome do cabeçalho da coluna. Por exemplo, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filtro | Objeto | Filtro aplicado ao trabalho de extração. Os tipos e as opções variam entre os tipos de trabalho. |

## Recuperando tarefas

Às vezes, você pode precisar recuperar seus trabalhos recentes. Isso é feito facilmente com Obter trabalhos de exportação para o tipo de objeto correspondente. Cada ponto de extremidade de Obter Trabalhos de Exportação oferece suporte a um campo de filtro `status`, um  `batchSize` para limitar o número de trabalhos retornados e `nextPageToken` para paginação por meio de conjuntos de resultados grandes. O filtro de status suporta cada status válido para um trabalho de exportação: Criado, Em fila, Processando, Cancelado, Concluído e Falha. O batchSize tem um máximo e o padrão de 300. Vamos obter a lista de Tarefas de exportação de clientes potenciais:

```
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

O ponto de extremidade responde com a resposta `status` de cada trabalho criado nos últimos sete dias para esse tipo de objeto na matriz de resultados. A resposta incluirá apenas resultados para tarefas de propriedade do usuário da API que está fazendo a chamada.

## Iniciar um trabalho

Com a ID do trabalho em mãos, vamos iniciar o trabalho:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Isso inicia a execução do job e retorna uma resposta de status. Como a exportação é sempre feita de forma assíncrona, devemos pesquisar o status do trabalho para determinar se ele foi concluído. O status de uma determinada tarefa não será atualizado com mais frequência do que uma vez a cada 60 segundos, portanto, o status nunca deve ser sondado com mais frequência do que isso. No entanto, lembre-se de que a maioria dos casos de uso nunca deve exigir sondagem com mais frequência do que uma vez a cada 5 minutos. Os dados de cada exportação bem-sucedida são mantidos por 10 dias.

## Status do trabalho de pesquisa

Determinar o status do processo é simples.

O status só pode ser sondado para trabalhos criados pelo mesmo usuário da API que os criou.

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

O membro interno `status` indica o progresso do trabalho e pode ser um dos seguintes valores: Created, Queued, Processing, Cancelled, Completed, Failed. Nesse caso, nosso trabalho foi concluído, para que possamos interromper a sondagem e continuar a recuperar o arquivo. Quando concluído, o membro `fileSize` indica o comprimento total do arquivo em bytes, e o membro `fileChecksum` contém o hash SHA-256 do arquivo. O status do trabalho fica disponível por 30 dias após o status Concluído ou Falha ser atingido.

## Recuperação de dados

Quando a tarefa for concluída, você poderá recuperar facilmente o arquivo.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

A resposta contém um arquivo formatado da maneira que o trabalho foi configurado. O endpoint responde com o conteúdo do arquivo. Se uma tarefa não tiver sido concluída ou uma ID de tarefa incorreta for transmitida, os endpoints de arquivo responderão com o status 404 Não encontrado e uma mensagem de erro de texto sem formatação como carga, ao contrário da maioria dos outros endpoints REST do Marketo.

Para oferecer suporte à recuperação parcial e de fácil retomada de dados extraídos, o ponto de extremidade do arquivo oferece suporte opcionalmente ao cabeçalho HTTP `Range` do tipo `bytes` (por [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Se o cabeçalho não estiver definido, todo o conteúdo será retornado. Para recuperar os primeiros 10.000 bytes de um arquivo, você passaria o seguinte cabeçalho como parte de sua solicitação GET para o endpoint, começando pelo byte 0:

```
Range: bytes=0-9999
```

Ao recuperar o arquivo parcial, o endpoint responde com o código de status 206 e retorna os cabeçalhos Accept-range, Content-Length e Content-Range:

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Recuperação e retomada parciais

Os arquivos podem ser recuperados em parte ou retomados posteriormente usando o cabeçalho `Range`. O intervalo de um arquivo começa no byte 0 e termina no valor de `fileSize` menos 1. O comprimento do arquivo também é relatado como o denominador no valor do cabeçalho de resposta `Content-Range` ao chamar um ponto de extremidade Obter Arquivo de Exportação. Se uma recuperação falhar parcialmente, ela poderá ser retomada posteriormente. Por exemplo, se você tentar recuperar um arquivo com 1000 bytes de comprimento, mas apenas os primeiros 725 bytes forem recebidos, a recuperação poderá ser repetida a partir do ponto de falha, chamando o endpoint novamente e transmitindo um novo intervalo:

```
Range: bytes 724-999
```

Isso retorna os 275 bytes restantes do arquivo.

#### Verificação da integridade do arquivo

Os pontos de extremidade de status do trabalho retornam uma soma de verificação no atributo `fileChecksum` quando `status` é &quot;Concluído&quot;. A soma de verificação é um hash SHA-256 do arquivo exportado. Você pode comparar a soma de verificação com o hash SHA-256 do arquivo recuperado para verificar se ele está concluído.

Este é um exemplo de resposta contendo a soma de verificação:

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

Este é um exemplo de criação do hash SHA-256 de um arquivo recuperado chamado &quot;bulk_lead_export.csv&quot; usando o utilitário de linha de comando sha256sum:

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Cancelar um trabalho

Se uma tarefa for configurada incorretamente ou se se tornar desnecessária, ela poderá ser facilmente cancelada:

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
         "format": "CSV",
      }
   ]
}
```

Isso responde com um status indicando que o trabalho foi cancelado.
