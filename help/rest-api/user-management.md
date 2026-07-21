---
title: Gerenciamento de usuários
feature: REST API
description: Guia das APIs de gerenciamento de usuários do Marketo para CRUD em usuários, autenticação baseada em cabeçalho, funções e espaços de trabalho, manipulação de código de status, formato de data e hora e pontos de extremidade de consulta.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
TQID: https://experienceleague.adobe.com/V1NzpIl-peHBi9rqy8YwdJDh3O-dViIdF0cBsDSI-w8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: d1d0a9cd-295d-4976-8c39-ddae266f240eid: d65b4a73-87a3-4d56-b638-74e74d9939ce
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1440
ht-degree: 6%

---

# Gerenciamento de usuários

[Referência de Ponto de Extremidade de Gerenciamento de Usuários](https://developer.adobe.com/marketo-apis/api/user/)

Os endpoints de Gerenciamento de usuários do Marketo executam operações CRUD em registros de usuários. Para criar um usuário, envie um convite. O usuário então define uma senha e acessa o Marketo pela primeira vez.

Ao contrário de outras APIs REST do Marketo, ao usar as APIs de gerenciamento de usuários:

- Envie o token de acesso em um cabeçalho HTTP. Não é possível passar o token de acesso como um parâmetro de sequência de consulta. Consulte o [Guia de autenticação](authentication.md).
- Ao criar a função de usuário para um [Serviço personalizado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) da API REST, selecione uma permissão de cada um destes grupos:
  1. Permissão &quot;Usuários de Acesso&quot; do grupo [Administrador de Acesso](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
  1. &quot;Acessar API de Gerenciamento de Usuários&quot; do grupo [Acessar API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Avalie o código de status de resposta HTTP porque os corpos de resposta não contêm o atributo booleano &quot;success&quot;. Uma chamada bem-sucedida retorna o código de status 200. Uma chamada com falha retorna um código de status diferente de 200 e a matriz de &quot;erros&quot; padrão com um código de erro e uma mensagem descritiva.
- Formatar cadeias de caracteres datetime como `yyyyMMdd'T'HH:mm:ss.SSS't'+|-hhmm`. Este formato se aplica a `createdAt`, `updatedAt` e `expiresAt`.
- Não coloque prefixos &quot;/rest&quot; nos endpoints da API de gerenciamento de usuários.

## Consultar

As consultas de gerenciamento de usuários podem recuperar todos os usuários, funções e espaços de trabalho. Eles também podem recuperar um usuário ou a função associada e os registros do espaço de trabalho por ID de usuário.

### Usuário por ID

O ponto de extremidade [Obter Usuário por Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) pega um único parâmetro de caminho `userid` e retorna um único registro de usuário para um usuário que aceitou seu convite.

```http
GET /userservice/management/v1/users/{userid}/user.json
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "Jamie",
  "lastName": "Lannister",
  "emailAddress": "jamie@lannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2020-12-31T08:00:00.000t+0000",
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

### Usuário Convidado por ID

O ponto de extremidade [Obter Usuário Convidado por Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) pega um único parâmetro de caminho `userid` e retorna um único registro de usuário para um usuário &quot;pendente&quot; (ainda não aceitou o convite).

```http
GET /userservice/management/v1/users/{userid}/invite.json
```

```json
{
    "id": 25112,
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@lannister.com",
    "userId": "jamie@lannister.com",
    "subscriptionId": 3381,
    "status": "pending",
    "expiresAt": "20200807T20:49:54.0t+0000",
    "createdAt": "20200731T20:49:54.0t+0000",
    "updatedAt": "20200731T20:49:54.0t+0000"
}
```

### Funções e espaços de trabalho por ID

O ponto de extremidade [Obter Funções e Espaços de Trabalho por Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) pega um parâmetro de caminho `userid` e retorna os registros de função e espaço de trabalho do usuário. Cada objeto no array de resposta contém a função, a ID do espaço de trabalho e o nome.

```http
GET /userservice/management/v1/users/{userid}/roles.json
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

### Procurar Usuários

O ponto de extremidade [Obter Usuários](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) retorna todos os registros de usuário. Ela é compatível com estes parâmetros inteiros opcionais:

- `pageSize` especifica o número máximo de entradas para retornar. O padrão é 20 e o máximo é 200.
- `pageOffset` especifica onde começar a recuperar entradas. O padrão é 0, e pode ser usado com `pageSize`.

```http
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "jamie@lannister.com",
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@houselannister.com",
    "id": 6785,
    "apiOnly": false
  },
  {
    "userid": "jeoffery@housebaratheon.com",
    "firstName": "Jeoffery",
    "lastName": "Baratheon",
    "emailAddress": "jeoffery@housebaratheon.com",
    "id": 7718,
    "apiOnly": false
  },
  {
    "userid": "rickon@housestark.com",
    "firstName": "Rickon",
    "lastName": "Stark",
    "emailAddress": "rickon@housestark.com",
    "id": 8612,
    "apiOnly": false
  }
]
```

>[!NOTE]
>
>Na amostra de código acima, o `userid` exibido é para um cliente que foi migrado para o Adobe IMS. Os clientes que ainda não foram migrados verão um endereço de email comum no campo `userid`.

### Procurar Funções

O ponto de extremidade [Obter Funções](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) retorna uma lista de todos os registros de função.

```http
GET /userservice/management/v1/users/roles.json
```

```json
[
    {
        "id": 1,
        "name": "Admin",
        "description": "All permissions",
        "type": "system",
        "hidden": false,
        "onlyAllZones": true,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 2,
        "name": "Standard User",
        "description": "All permissions except Admin",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 24,
        "name": "RTP Launcher",
        "description": "Role required for launcher in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 25,
        "name": "RTP Editor",
        "description": "Role required for editor in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 101,
        "name": "Analytics User",
        "description": "Has access to Analytics",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 102,
        "name": "Marketing User",
        "description": "All permissions except Admin",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 103,
        "name": "Web Designer",
        "description": "Has access to Design Studio except approval permission",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    }
]
```

### Procurar Espaços de Trabalho

O ponto de extremidade [Get Workspaces](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) retorna uma lista de todos os registros do espaço de trabalho.

```http
GET /userservice/management/v1/users/workspaces.json
```

```json
[
  {
    "id": 1,
    "name": "Default",
    "description": "Initial workspace for Marketing Activities, Design Studio, and so on.",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20160910T23:08:05.0t+0000",
    "updatedAt": "20160910T23:08:05.0t+0000"
  },
  {
    "id": 1008,
    "name": "World",
    "description": "",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20181119T21:59:36.0t+0000",
    "updatedAt": "20181119T21:59:36.0t+0000"
  },
  {
    "id": 1009,
    "name": "Reproduction - US English - All Leads",
    "description": "A Workspace for recreating customer-reported problems.",
    "globalViz": 1,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190129T23:36:37.0t+0000",
    "updatedAt": "20190129T23:36:37.0t+0000"
  },
  {
    "id": 1010,
    "name": "US",
    "description": "United States - Qualified Leads",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190322T15:55:40.0t+0000",
    "updatedAt": "20190322T15:55:40.0t+0000"
  }
]
```

## Convidar usuário

Em [assinaturas integradas ao Adobe IMS](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), este ponto de extremidade oferece suporte somente ao convite de [Usuários Somente de API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Para convidar [Usuários padrão](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use a [API de Gerenciamento de Usuários do Adobe](https://developer.adobe.com/umapi/).

O ponto de extremidade [Convidar Usuário](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) envia um convite de email de &quot;Boas-vindas ao Marketo&quot; para um novo usuário. O email contém um link &quot;Logon no Marketo&quot;. O recipient seleciona o link, cria uma senha e obtém acesso ao Marketo.

Até que o recipient aceite o convite, seu status será &quot;pendente&quot; e o registro do usuário não poderá ser editado. Um convite pendente expira sete dias após ser enviado. Consulte a [documentação de gerenciamento de usuários do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) para obter mais informações.

Transmita parâmetros no corpo da solicitação no formato `application/json`.

Os parâmetros necessários são `emailAddress`, `firstName`, `lastName` e `userRoleWorkspaces`. O parâmetro `userRoleWorkspaces` é uma matriz de objetos que contém `accessRoleId` e `workspaceId` atributos.

O parâmetro `userid` é o identificador de usuário exclusivo usado para logon e deve ser formatado como um endereço de email. Se a solicitação omitir `userid`, seu valor padrão será o valor de `emailAddress`.

O parâmetro booleano `apiOnly` especifica se o usuário é um [usuário Somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). O parâmetro `expiresAt` especifica quando o logon do usuário expira e usa o formato W3C ISO-8601 sem milissegundos. Se a solicitação omitir `expiresAt`, o usuário nunca expirará. O parâmetro `reason` descreve o motivo do convite.

O endpoint retorna &quot;true&quot; quando o convite é bem-sucedido. Caso contrário, retornará uma mensagem de erro.

```http
POST /userservice/management/v1/users/invite.json
```

```text
Content-Type: application/json
```

```json
{
  "emailAddress": "daenerys@housetargaryen.com",
  "firstName": "Daenerys",
  "lastName": "Targaryen",
  "expiresAt": "2020-12-31T23:59:59-05:00",
  "reason": "Keeper of dragons",
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "workspaceId": 0
    }
  ]
}
```

```text
true
```

A imagem a seguir mostra o email de boas-vindas ao Marketo enviado ao novo usuário. O assunto é &quot;Informações de logon do Marketo&quot;. O remetente é o endereço de email do Usuário Somente API associado ao [Serviço Personalizado da API REST](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api). Os parâmetros firstName, lastName e emailAddress especificam o recipient.

![Convidar email de usuário](assets/invite-user-email.png)

O usuário aceita o convite digitando uma senha duas vezes e selecionando o botão &quot;CRIAR SENHA&quot;. O usuário recebe acesso ao Marketo.

## Atualizar usuário

Você pode atualizar os atributos do usuário ou excluir um usuário depois que ele aceitar o convite. Transmita atributos como parâmetros no corpo da solicitação no formato application/json.

### Atualizar atributos do usuário

Em [assinaturas integradas ao Adobe IMS](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), este ponto de extremidade oferece suporte à atualização de atributos somente de [Usuários somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Para atualizar os atributos de [Usuários padrão](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use a [API de Gerenciamento de Usuários do Adobe](https://developer.adobe.com/umapi/).

O ponto de extremidade [Atualizar Atributos de Usuário](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) pega um único parâmetro de caminho `userid` e retorna um único registro de usuário. O corpo da solicitação contém um ou mais atributos de usuário a serem atualizados: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```http
POST /userservice/management/v1/users/{userid}/update.json
```

```text
Content-Type: application/json
```

```json
{
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "expiresAt": "20211231T08:00:00.000t+0000"
}
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "emailAddress": "jamie@houselannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2021-12-31T08:00:00.000t+0000"
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

#### Excluir usuário

Em [assinaturas integradas ao Adobe IMS](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), este ponto de extremidade oferece suporte à exclusão somente de [Usuários somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Para excluir os [Usuários padrão](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use a [API de Gerenciamento de Usuários do Adobe](https://developer.adobe.com/umapi/).

O ponto de extremidade [Excluir Usuário](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) pega um único parâmetro de caminho `userid` e exclui o usuário correspondente da instância. Essa é uma exclusão destrutiva e não pode ser revertida. Se for bem-sucedido, um código de status 200 será retornado, caso contrário, uma mensagem de erro será retornada.

```http
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Excluir Usuário Convidado

O ponto de extremidade [Excluir Usuário Convidado](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) pega um único parâmetro de caminho `userid` e exclui o usuário &quot;pendente&quot; correspondente da instância (o usuário ainda não aceitou o convite). Essa é uma exclusão destrutiva e não pode ser revertida. Se for bem-sucedido, um código de status 200 será retornado, caso contrário, uma mensagem de erro será retornada.

```http
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Atualizar Funções

É possível adicionar ou excluir funções. Transmita atributos como parâmetros no corpo da solicitação no formato application/json.

## Adicionar Funções

O ponto de extremidade [Adicionar Funções](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) pega um único parâmetro de caminho `userid` e adiciona uma ou mais funções de usuário ao usuário correspondente. O corpo da solicitação contém uma lista de um ou mais objetos, cada um contendo um atributo `accessRoleId` e um `workspaceId`. Se for bem-sucedido, toda a lista de pares de `accessRoleId/workspaceId` do usuário especificado será retornada.

```http
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```text
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

## Excluir Funções

O ponto de extremidade [Excluir Funções](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) pega um único parâmetro de caminho `userid` e exclui uma ou mais funções de usuário do usuário correspondente. O corpo da solicitação contém uma lista de um ou mais objetos, cada um contendo um atributo `accessRoleId` e um `workspaceId`. Se for bem-sucedido, a lista restante de pares accessRoleId/workspaceId para o usuário especificado será retornada.

```http
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```text
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  }
]
```
