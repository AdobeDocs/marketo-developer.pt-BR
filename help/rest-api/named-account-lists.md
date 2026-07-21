---
title: Listas de contas nomeadas
feature: REST API
description: Saiba como gerenciar Listas de contas nomeadas do Marketo com a API REST, incluindo permissões, campos, filtragem e endpoints para consultar, criar, atualizar e excluir.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
TQID: https://experienceleague.adobe.com/18lMhheW21Gz1-3TMHwleHhmLTOqJsZSQ5aqkbbchhM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 686
ht-degree: 3%

---

# Listas de contas nomeadas

[Referência de Ponto de Extremidade de Listas de Contas Nomeadas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Account-Lists)

[Listas de Contas Nomeadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) são coleções de contas nomeadas na Marketo. Use-os para categorização, enriquecimento de dados e filtragem de campanha inteligente.

As APIs da Lista de contas nomeadas permitem gerenciar remotamente os ativos de lista e seus membros.
`Content`

## Permissões

A permissão necessária depende da operação:

- Consultar Listas de Contas Nomeadas: Lista de Contas Nomeadas Somente Leitura ou Lista de Contas Nomeadas Leitura-Gravação.
- Criar, atualizar ou excluir listas: Lista de Contas Nomeadas de Leitura/Gravação.
- Associação da lista de consultas: Conta Nomeada Somente Leitura ou Conta Nomeada Leitura/Gravação.
- Gerenciar associação de lista: Conta Nomeada de Leitura-Gravação.

## Modelo

As listas de contas nomeadas têm um conjunto limitado de campos padrão e não são compatíveis com campos personalizados.
`Named Account List Field`

| Nome | Tipo de dados | Atualizável | Observações |
| --- | --- | --- | --- |
| marketoGUID | String | Falso | Identificador de sequência de caracteres exclusivo da lista de contas nomeadas. Este campo é gerenciado pelo sistema e não é permitido como um campo ao criar um registro. Campo usado por &quot;dedupeBy&quot;:&quot;idField&quot; ao criar ou atualizar. |
| name | String | Verdadeiro | Nome da lista. Campo usado por &quot;dedupeBy&quot;:&quot;dedupeFields&quot; ao executar uma criação ou atualização. |
| createdAt | Data/hora | Falso | Data e hora de criação da lista. Este campo é gerenciado pelo sistema e não é permitido como um campo ao criar ou atualizar um registro. |
| updatedAt | Data/hora | Falso | Data e hora da atualização mais recente da lista. Este campo é gerenciado pelo sistema e não é permitido como um campo ao criar ou atualizar um registro. |
| tipo | String | Falso | Tipo da lista. Pode ter um valor &quot;padrão&quot; ou &quot;externo&quot;. Listas externas são aquelas criadas pela Exibição de Conta do CRM. |

## Consultar

As consultas de lista de contas nomeadas suportam dois filterTypes: &quot;dedupeFields&quot; e &quot;idField&quot;. Defina o campo no parâmetro de consulta `filterType` e forneça os valores em `filterValues as` uma lista separada por vírgulas.

Os filtros `nextPageToken` e `batchSize` são opcionais.

```http
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

Criar e atualizar registros de lista de contas nomeadas usando o padrão padrão de Banco de Dados de Cliente Potencial padrão. As listas de contas nomeadas têm apenas um campo atualizável: `name`.

O endpoint oferece suporte a dois tipos de ação padrão: &quot;createOnly&quot; e &quot;updateOnly&quot;. O `action defaults` para &quot;createOnly&quot;.

Você pode especificar o `dedupeBy parameter` opcional quando a ação for `updateOnly`. Os valores permitidos são &quot;dedupeFields&quot;, que corresponde a &quot;name&quot;, e &quot;idField&quot;, que corresponde a &quot;marketoGUID&quot;.

Nos modos `createOnly`, somente &quot;name&quot; é permitido como o campo `dedupeBy`. É possível enviar até 300 registros de cada vez.

```http
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

Exclua Listas de Contas Nomeadas usando `name` ou `marketoGUID` da lista. Para selecionar a chave, passe &quot;dedupeFields&quot; como nome ou &quot;idField&quot; como marketoGUID no membro `deleteB` da solicitação.

Se não estiver definido, o valor padrão será dedupeFields. É possível excluir até 300 registros de cada vez.

```http
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

Se um registro não puder ser encontrado para uma chave, o item de resultado correspondente terá um `status` de &quot;ignorado&quot;. Também inclui um motivo com um código e uma mensagem que descrevem a falha.

## Gerenciar associação

### Associação de consulta

Consultar associação à lista de contas nomeadas fornecendo o `i` da lista de contas. Os parâmetros opcionais são:

-`field` - uma lista separada por vírgulas de campos a serem incluídos nos registros de resposta
-`nextPageToke` - para paginação através do conjunto de resultados
-`batchSiz` - para especificar o número de registros a serem retornados

Se `field` não estiver definido, então`marketoGUI`,`nam`, `createdA` e`updatedA` serão retornados. `batchSiz` tem um valor máximo e padrão de 300.

```http
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

Adicione contas nomeadas a uma Lista de contas nomeadas usando o marketoGUID. Você pode adicionar até 300 registros por vez.

```http
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

A remoção de registros de uma lista de contas usa um caminho diferente, mas a mesma interface. Forneça um `marketoGUI` para cada registro a ser removido. É possível remover até 300 registros de cada vez.

```http
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

- Os endpoints da Lista de Contas Nomeadas têm um tempo limite de 30s, a menos que especificado de outra forma.
- Sincronizar Listas de Contas Nomeadas tem um tempo limite de 60s.
- Excluir listas de contas nomeadas tem um tempo limite de 60s.
- Obter Listas de Contas Nomeadas tem um tempo limite de 60s.
- Adicionar membros da lista de contas nomeadas tem um tempo limite de 60s.
- O tempo limite para Remover Membros da Lista de Contas Nomeadas é de 60s.
- Obter Membros da Lista de Contas Nomeadas tem um tempo limite de 60s.
