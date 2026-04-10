---
title: Emails
feature: REST API
description: Use a API REST do Marketo Asset para consultar, criar, atualizar, clonar, excluir, aprovar e inspecionar dependências de ativos de email.
exl-id: b41a3ae5-2b25-4103-84b4-320fc2c44bd6
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 5%

---

# Emails

[Referência de ponto de extremidade de email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails_New)

Emails são registros de ativos que definem os metadados da mensagem, a configuração do conteúdo, as configurações e o estado de aprovação.

## Acesso

Os endpoints descritos neste artigo exigem um token de acesso:

```text
?access_token=<access_token>
```

As solicitações também exigem o cabeçalho `x-app-type`:

```text
x-app-type: <app-type>
```

## Consultar

Você pode recuperar metadados de email por ativo `id` ou com o ponto de extremidade de filtro.

### Por ID

#### Solicitação

```http
GET /rest/asset/v2/email/{id}
```

#### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7b3c#1900ab12cd3",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "description": "Main announcement email",
      "status": "draft"
    }
  ]
}
```

### Filtro

O endpoint de filtro permite pesquisar em um espaço de trabalho e restringir os resultados com parâmetros de consulta adicionais.

`workspaceId` é obrigatório.

Parâmetros de consulta compatíveis:

| Parâmetro | Descrição |
| - | - |
| `workspaceId` | Identificador de espaço de trabalho necessário usado para determinar o escopo da solicitação de filtro. |
| `folderId` | Filtra os resultados em uma única pasta. |
| `folderIds` | Parâmetro repetido que filtra os resultados em várias pastas. |
| `status` | Parâmetro repetido que filtra por um ou mais status de email. |
| `pageIndex` | Índice de página com base em zero para resultados paginados. |
| `pageSize` | Número máximo de resultados a serem retornados por página. |
| `createdBy` | Filtra os resultados pelo usuário que está criando. |
| `createdAtStart` | Retorna ativos criados nesse carimbo de data e hora ou depois dele. |
| `createdAtEnd` | Retorna ativos criados nesse carimbo de data e hora ou antes dele. |
| `modifiedBy` | Filtra os resultados pelo último usuário modificador. |
| `modifiedAtStart` | Retorna ativos modificados em ou após esse carimbo de data e hora. |
| `modifiedAtEnd` | Retorna ativos modificados nesse carimbo de data e hora ou antes dele. |
| `name` | Filtra resultados por nome de email. |
| `sortKey` | Seleciona o campo usado para classificar os resultados. |
| `sortOrder` | Define a direção da classificação. |
| `isCreatedByMe` | Limita os resultados aos ativos criados pelo usuário atual. |
| `isModifiedByMe` | Limita os resultados aos ativos modificados pela última vez pelo usuário atual. |
| `templateId` | Filtra resultados por identificador de modelo. |
| `scriptEngine` | Filtra os resultados pela configuração do mecanismo de script. |
| `isValueNonNullable` | Filtra com base no fato de o valor não ser anulável. |
| `includeArchived` | Inclui ativos arquivados nos resultados. |

#### Solicitação

```http
GET /rest/asset/v2/email/filter?workspaceId=1001&name=Spring%20Launch&status=draft&status=approved&pageIndex=0&pageSize=20
```

#### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "5dd1#1900ab13011",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Use o parâmetro `name` quando precisar localizar um email por nome.

## Criar

Crie um email enviando uma carga JSON. `name`, `appData` e `headers` são obrigatórios. `headers.subject` é obrigatório e `appData` deve incluir pelo menos um de `folderId`, `workspaceId` ou `programId`.

### Solicitação

```http
POST /rest/asset/v2/email
Content-Type: application/json
```

```json
{
  "name": "Spring Launch Email",
  "description": "Main announcement email",
  "appData": {
    "workspaceId": "1001",
    "folderId": "2002",
    "editorType": "email"
  },
  "headers": {
    "subject": "Introducing the Spring Launch",
    "fromName": "Marketing Team",
    "fromEmail": "marketing@example.com",
    "replyEmail": "reply@example.com",
    "preheader": "See what changed this quarter",
    "ccEmails": [
      "owner@example.com"
    ]
  },
  "settings": {
    "isOperational": false,
    "isTextOnly": false,
    "isWebPageView": true,
    "enableUrlTracking": true
  },
  "templateId": "3003"
}
```

### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "238c#1900ab1313f",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

O corpo da solicitação também pode incluir `data`, `editorContext`, `themeId`, `appType` e `status`.

### Criar campos

* `appData` identifica onde o email é criado e como ele é editado.
* `headers` contém os valores do cabeçalho da mensagem, incluindo assunto, remetente, endereço de resposta, pré-cabeçalho e destinatários CC opcionais.
* `settings` controla o comportamento operacional e de renderização, como modo somente texto, exibição de página da Web e rastreamento de URL.
* `templateId` identifica o modelo de email usado quando o email é criado.

## Atualização

Atualizar um email por ID de ativo. O corpo da solicitação usa o esquema `UpdateEmailRequest` e todas as propriedades são opcionais, portanto, você pode enviar somente os campos que deseja alterar.

### Solicitação

```http
POST /rest/asset/v2/email/{id}/update
Content-Type: application/json
```

```json
{
  "description": "Updated announcement email",
  "headers": {
    "subject": "Spring Launch Is Live",
    "preheader": "Read the latest release notes"
  },
  "settings": {
    "enableUrlTracking": true,
    "isWebPageView": true
  }
}
```

### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "9fd3#1900ab13210",
  "result": [
    {
      "id": "1017"
    }
  ]
}
```

## Gerenciar estado

Os emails usam um ciclo de vida de rascunho e aprovado. Use o endpoint de transição de estado para aprovar um email, cancelar a aprovação, descartar um rascunho ou criar um novo rascunho.

Os valores válidos de `action` são:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Solicitação

```http
POST /rest/asset/v2/email/state/transition
Content-Type: application/json
```

### Resposta

```json
{
  "contentId": "1017",
  "action": "approve"
}
```

## Clonar

Use o endpoint de clonagem para criar uma cópia de um email existente.

### Solicitação

```http
POST /rest/asset/v2/email/clone
Content-Type: application/json
```

### Resposta

```json
{
  "assetId": "1017",
  "newAsset": {
    "name": "Spring Launch Email Copy",
    "description": "Cloned from Spring Launch Email"
  }
}
```

## Excluir

Excluir um email por ID do ativo.

### Solicitação

```http
POST /rest/asset/v2/email/{id}/delete
Content-Type: application/json
```

Esse ponto de extremidade pega o ativo `id` no caminho e não define um corpo de solicitação.

## Usado por

Use o ponto de extremidade `usedby` para recuperar ativos que fazem referência a determinado email.

### Solicitação

```http
POST /rest/asset/v2/email/usedby
Content-Type: application/json
```

### Resposta

```json
{
  "assetId": "1017",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Observações

Os endpoints de consulta retornam metadados para o ativo. Use a referência de endpoint para o esquema completo de campos disponíveis e qualquer propriedade específica do ambiente.
