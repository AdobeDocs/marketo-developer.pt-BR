---
title: "Gerenciamento de usuários"
feature: REST API
description: "Execute operações CRUD em registros de usuários."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 0%

---


# Gerenciamento de usuários

[Referência de Ponto de Extremidade de Gerenciamento de Usuários](https://developer.adobe.com/marketo-apis/api/user/)

O Marketo fornece um conjunto de endpoints de gerenciamento de usuários que permitem executar operações CRUD em registros de usuários no Marketo. Os usuários são criados enviando um convite para um usuário, que então define uma senha e obtém acesso ao Marketo pela primeira vez.

Ao contrário de outras APIs REST do Marketo, ao usar as APIs de gerenciamento de usuários:

- Você deve usar o método de cabeçalho HTTP para enviar o token de acesso para autenticação. Não é possível passar um token de acesso como parâmetro de sequência de consulta. Mais informações sobre autenticação são [aqui](authentication.md).
- Você deve selecionar uma permissão de função de dois grupos diferentes ao criar a função de usuário para [Serviço personalizado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) para REST API:
   1. Permissão &quot;Acessar usuários&quot; na [Administrador de acesso](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions) grupo
   1. &quot;Acessar a API de gerenciamento de usuários&quot; na [API de acesso](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions) grupo
- Os corpos de resposta não contêm o atributo booleano &quot;success&quot; indicando o sucesso ou a falha de uma chamada. Em vez disso, você deve avaliar o código de status da resposta HTTP. Se uma chamada for bem-sucedida, um código de status 200 será retornado. Se uma chamada falhar, um código de status de nível diferente de 200 será retornado e o corpo da resposta conterá a matriz &quot;erros&quot; padrão com código de erro e mensagem de erro descritiva.
- O formato das cadeias de caracteres de data e hora é &quot;aaaaMMdd&#39;T&#39;HH:mm:ss.SSS&#39;+|-hhmm&quot;. Isso se aplica aos seguintes atributos: createdAt, updatedAt, expiresAt.
- Os endpoints da API de gerenciamento de usuários não recebem o prefixo &quot;/rest&quot; como outros endpoints.

## Consultar

O suporte de consulta para gerenciamento de usuários inclui a capacidade de recuperar todos os usuários, funções e espaços de trabalho. Além disso, você pode recuperar um único registro de usuário por ID de usuário ou registro de função/espaço de trabalho por ID de usuário.

### Usuário por ID

A variável [Obter Usuário por ID](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) o endpoint ocupa um único `userid` parâmetro de caminho e retorna um único registro de usuário para um usuário que aceitou seu convite.

```
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

A variável [Obter Usuário Convidado por ID](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) o endpoint ocupa um único `userid` parâmetro de caminho e retorna um único registro de usuário para um usuário &quot;pendente&quot; (ainda não aceitou o convite).

```
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

A variável [Obter Funções e Espaços de Trabalho por ID](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) o endpoint ocupa um único `userid` parâmetro de caminho e retorna uma lista de registros de função de usuário e espaço de trabalho. A resposta contém uma matriz com um objeto que contém a ID da função e do espaço de trabalho e o nome do usuário especificado.

```
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

A variável [Obter usuários](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) endpoint retorna uma lista de todos os registros de usuário. O modelo opcional `pageSize` parameter é um número inteiro que especifica o número máximo de entradas a serem retornadas. O padrão é 20. O máximo é 200. O modelo opcional `pageOffset` parameter é um número inteiro que especifica onde começar a recuperar entradas. Pode ser usado com `pageSize`. O padrão é 0.

```
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

### Procurar Funções

A variável [Obter Funções](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) endpoint retorna uma lista de todos os registros de função.

```
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

A variável [Obter Espaços de Trabalho](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) endpoint retorna uma lista de todos os registros do espaço de trabalho.

```
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

Ligado [Assinaturas integradas ao Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), esse endpoint oferece suporte ao convite de [Usuários somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) somente. Para convidar [Usuários padrão](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use o [API de gerenciamento de usuários do Adobe](https://developer.adobe.com/umapi/) em vez disso.

A variável [Convidar usuário](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) para enviar um convite por email de &quot;Boas-vindas ao Marketo&quot; ao novo usuário. O corpo do email contém um link &quot;Logon no Marketo&quot; que permite que o usuário acesse o Marketo pela primeira vez. Para aceitar o convite, o recipient do email clica no link &quot;Logon no Marketo&quot;, cria a senha e obtém acesso ao Marketo. Até que o processo de aceitação seja concluído, o convite estará &quot;pendente&quot; e o registro do usuário não poderá ser editado. Um convite pendente expira sete dias após ser enviado. Mais informações sobre o gerenciamento de usuários podem ser encontradas [aqui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Os parâmetros são passados no corpo da solicitação no formato application/json.

Os seguintes parâmetros são obrigatórios:  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. A variável `userRoleWorkspaces` é uma matriz de objetos que contém `accessRoleId` e `workspaceId` atributos.

A variável `userid` é um valor de string de identificador de usuário exclusivo, usado para fins de logon do usuário, e deve ser formatado como um endereço de email. Se não for fornecido na solicitação, o valor de `userid` O assume como padrão o valor fornecido em `emailAddress` parâmetro.

O booleano `apiOnly` parâmetro especifica se o usuário é um [Usuário somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). A variável `expiresAt` O parâmetro especifica quando o logon do usuário expira e é formatado usando o formato W3C ISO-8601 (sem milissegundos). Se não for fornecido na solicitação, o usuário nunca expirará. A variável `reason` parameter é uma string que descreve o motivo do convite do usuário.

O endpoint retorna um valor &quot;true&quot; se bem-sucedido, caso contrário, uma mensagem de erro será retornada.

```
POST /userservice/management/v1/users/invite.json
```

```
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

```
true
```

Veja abaixo um exemplo do convite por email de &quot;Boas-vindas ao Marketo&quot; enviado ao novo usuário. A linha de assunto do email é &quot;Informações de logon do Marketo&quot;, o remetente é o endereço de email do Usuário somente API associado à [Serviço personalizado de API REST](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api)e o recipient é conforme especificado por meio dos parâmetros firstName, lastName e emailAddress.

![Convidar email de usuário](assets/invite-user-email.png)

O usuário aceita o convite digitando sua senha duas vezes e clicando no botão &quot;CRIAR SENHA&quot;. Ela então recebe acesso ao Marketo pela primeira vez.

## Atualizar usuário

O suporte à atualização para usuários inclui a capacidade de atualizar atributos do usuário ou excluir um usuário. Somente os usuários que aceitaram seu convite podem ser atualizados. Os atributos são passados como parâmetros para o corpo da solicitação no formato application/json.

### Atualizar atributos do usuário

Ligado [Assinaturas integradas ao Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), esse endpoint oferece suporte à atualização de atributos de [Usuários somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) somente. Para atualizar atributos de [Usuários padrão](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use o [API de gerenciamento de usuários do Adobe](https://developer.adobe.com/umapi/) em vez disso.

A variável [Atualizar atributos do usuário](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) o endpoint ocupa um único `userid` parâmetro de caminho e retorna um único registro de usuário. O corpo da solicitação contém um ou mais atributos de usuário a serem atualizados: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```
POST /userservice/management/v1/users/{userid}/update.json
```

```
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

Ligado [Assinaturas integradas ao Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), esse endpoint oferece suporte à exclusão de [Usuários somente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) somente. Para excluir [Usuários padrão](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use o [API de gerenciamento de usuários do Adobe](https://developer.adobe.com/umapi/) em vez disso.

A variável [Excluir usuário](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) o endpoint ocupa um único `userid` parâmetro de caminho e exclui o usuário correspondente da instância. Essa é uma exclusão destrutiva e não pode ser revertida. Se for bem-sucedido, um código de status 200 será retornado, caso contrário, uma mensagem de erro será retornada.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Excluir Usuário Convidado

A variável [Excluir Usuário Convidado](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) o endpoint ocupa um único `userid` e exclui o usuário &quot;pendente&quot; correspondente da instância (o usuário ainda não aceitou o convite). Essa é uma exclusão destrutiva e não pode ser revertida. Se for bem-sucedido, um código de status 200 será retornado, caso contrário, uma mensagem de erro será retornada.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Atualizar Funções

O suporte de atualização para funções inclui a capacidade de adicionar e excluir funções. Os atributos são passados como parâmetros para o corpo da solicitação no formato application/json.

## Adicionar Funções

A variável [Adicionar Funções](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) o endpoint ocupa um único `userid` e adiciona uma ou mais funções de usuário ao usuário correspondente. O corpo da solicitação contém uma lista de um ou mais objetos, cada um contendo um  `accessRoleId` e uma `workspaceId` atributo. Se for bem-sucedido, a lista inteira de `accessRoleId/workspaceId` pares para o usuário especificado é retornado.

```
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```
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

A variável [Excluir Funções](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) o endpoint ocupa um único `userid` e exclui uma ou mais funções de usuário do usuário correspondente. O corpo da solicitação contém uma lista de um ou mais objetos, cada um contendo um  `accessRoleId` e uma `workspaceId` atributo. Se for bem-sucedido, a lista restante de pares accessRoleId/workspaceId para o usuário especificado será retornada.

```
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```
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
