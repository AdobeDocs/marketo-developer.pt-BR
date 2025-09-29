---
title: Listas de contas nomeadas
feature: REST API
description: Saiba como gerenciar Listas de contas nomeadas do Marketo com a API REST, incluindo permissões, campos, filtragem e endpoints para consultar, criar, atualizar e excluir.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 2%

---

# Listas de contas nomeadas

[Referência de Ponto de Extremidade de Listas de Contas Nomeadas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Listas de contas nomeadas](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/target-account-management/target/account-lists) no Marketo representam coleções de contas nomeadas. Eles podem ser usados para uma grande variedade de casos, incluindo categorização, enriquecimento de dados e filtragem inteligente de campanha. As APIs da Lista de contas nomeadas permitem o gerenciamento remoto desses ativos de lista e sua associação.
`Content`

## Permissões

Para consultar Listas de Contas Nomeadas, é necessária a permissão Lista de Contas Nomeadas Somente Leitura ou Lista de Contas Nomeadas Leitura-Gravação. Para Criar, Atualizar ou Excluir Listas, é necessária a permissão Ler-Gravar Lista de Contas Nomeadas. A consulta de associação de lista requer as permissões de Conta Nomeada Somente Leitura ou Conta Nomeada Leitura-Gravação, enquanto o gerenciamento de associação requer as Permissões de Conta Nomeada Leitura-Gravação.

## Modelo

As Listas de contas nomeadas têm um número limitado de campos padrão e não são extensíveis com campos personalizados.
`Named Account List Field`

| Nome | Tipo de dados | Atualizável | Observações |
|---|---|---|---|
| marketoGUID | String | Falso | Identificador de sequência de caracteres exclusivo da lista de contas nomeadas. Este campo é gerenciado pelo sistema e não é permitido como um campo ao criar um registro. Campo usado por &quot;dedupeBy&quot;:&quot;idField&quot; ao criar ou atualizar. |
| name | String | Verdadeiro | Nome da lista. Campo usado por &quot;dedupeBy&quot;:&quot;dedupeFields&quot; ao executar uma criação ou atualização. |
| createdAt | Data/hora | Falso | Data e hora de criação da lista. Este campo é gerenciado pelo sistema e não é permitido como um campo ao criar ou atualizar um registro. |
| updatedAt | Data/hora | Falso | Data e hora da atualização mais recente da lista. Este campo é gerenciado pelo sistema e não é permitido como um campo ao criar ou atualizar um registro. |
| tipo | String | Falso | Tipo da lista. Pode ter um valor &quot;padrão&quot; ou &quot;externo&quot;. Listas externas são aquelas criadas pela Exibição de Conta do CRM. |

## Consultar

Consultar listas de contas é simples e fácil. Atualmente, há apenas dois filterTypes válidos para consultar listas de contas nomeadas: &quot;dedupeFields&quot; e &quot;idField&quot;. O campo para filtrar está definido no parâmetro `filterType` da consulta, e os valores estão definidos em `filterValues as` uma lista separada por vírgulas. Os filtros `nextPageToken` e `batchSize` também são parâmetros opcionais.

```
GET /rest/v1/namedAccountLists.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fb,dff23271-f996-47d7-984f-f2676861b5fc
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      }
   ]
}
```

## Criar e atualizar

A criação e a atualização de registros de lista de contas nomeadas seguem os padrões estabelecidos para outras operações de criação e atualização de bancos de dados de clientes potenciais. Lembre-se de que as listas de contas nomeadas têm apenas um campo atualizável, `name`.

O endpoint permite os dois tipos de ação padrão: &quot;createOnly&quot; e &quot;updateOnly&quot;.  O `action defaults` para &quot;createOnly&quot;.

O `dedupeBy parameter` opcional poderá ser especificado se a ação for `updateOnly`.  Os valores permitidos são &quot;dedupeFields&quot; (correspondente a &quot;name&quot;) ou &quot;idField&quot; (correspondente a &quot;marketoGUID&quot;).  Nos modos `createOnly`, somente &quot;name&quot; é permitido como o campo `dedupeBy`. É possível enviar até 300 registros de cada vez.

```
POST /rest/v1/namedAccountLists.json
```

```json
{
   "action": "createOnly",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "name": "SAAS List"
      },
      {
         "name": "Manufacturing (Domestic)"
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
         "seq": 0,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

## Excluir

A exclusão de Listas de Contas Nomeadas é simples e pode ser feita com base no `name` ou no `marketoGUID` da lista. Para selecionar a chave que deseja usar, passe &quot;dedupeFields&quot; para o nome ou &quot;idField&quot; para marketoGUID no membro `deleteB` da sua solicitação. Se desdefinido, o padrão será dedupeFields. É possível excluir até 300 registros de cada vez.

```
POST /rest/v1/namedAccountLists/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
         "name": "Saas List"
      },
      {
         "name": "B2C List"
      },
      {
         "name": "Launchpoint Partner List"
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
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq": 1,
         "id": "dff23271-f996-47d7-984f-f2676861b5fc",
         "status": "deleted"
      },
      {
         "seq": 2,
         "status": "skipped",
         "reasons": [
            {
               "code": "1013",
               "message": "Record not found"
            }
         ]
      }
   ]
}
```

Caso um registro não possa ser encontrado para uma determinada chave, o item de resultado correspondente terá um`status` de &quot;ignorado&quot; e um motivo com um código e uma mensagem descrevendo a falha, como mostrado no exemplo acima.

## Gerenciar associação

### Associação de consulta

Consultar a associação de uma lista de contas nomeadas é simples, exigindo apenas o `i` da lista de contas. Os parâmetros opcionais são:

-`field` - uma lista separada por vírgulas de campos a serem incluídos nos registros de resposta
-`nextPageToke` - para paginação através do conjunto de resultados
-`batchSiz` - para especificar o número de registros a serem retornados

Se `field` não estiver definido, então`marketoGUI`,`nam`, `createdA` e`updatedA` serão retornados. `batchSiz` tem um valor máximo e padrão de 300.

```
GET /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      }
   ]
}
```

### Adicionar membros

Contas nomeadas podem ser facilmente adicionadas a uma Lista de contas nomeadas. Contas só podem ser adicionadas usando marketoGUID. Você pode adicionar até 300 registros por vez.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true,
}
```

### Remover membros

A remoção de registros de uma lista de contas tem um caminho diferente, mas a mesma interface, exigindo `marketoGUI` para cada registro que você deseja excluir. É possível remover até 300 registros de cada vez.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts/remove.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true
}
```

## Tempos limite

- Os endpoints da Lista de Contas Nomeadas têm um tempo limite de 30s, a menos que indicado abaixo
   - Sincronizar listas de contas nomeadas: 60s
   - Excluir Listas de Contas Nomeadas: 60s
   - Obter Listas de Contas Nomeadas: 60s
   - Adicionar membros da lista de contas nomeadas: 60s
   - Remover Membros da Lista de Contas Nomeadas: 60s
   - Obter Membros da Lista de Contas Nomeadas: 60s
