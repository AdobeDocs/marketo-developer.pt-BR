---
title: Modelos de email
feature: REST API
description: Use a API REST do Marketo Asset para consultar, criar, atualizar, clonar, excluir, aprovar e inspecionar dependências de modelos de email.
exl-id: 50bb0047-d6ea-4c94-a900-18c37b17a147
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 9%

---

# Modelos de email

[Referência de endpoint de modelo de email](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates)

Os modelos de email definem a estrutura e o layout reutilizável usados ao criar emails.

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

Você pode recuperar metadados de modelo de email por ID de ativo ou com o endpoint de filtro.

### Por ID

#### Solicitação

```http
GET /rest/asset/v2/emailtemplate/{id}
```

#### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "14f9e#1900ab14001",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "description": "Core responsive template",
      "status": "draft"
    }
  ]
}
```

### Filtro

O endpoint de filtro permite pesquisar em um espaço de trabalho e restringir os resultados com parâmetros de consulta adicionais. `workspaceId` é obrigatório.

Os filtros suportados incluem `folderId`, `folderIds` repetido, `status`, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` e `includeArchived`.

#### Solicitação

```http
GET /rest/asset/v2/emailtemplate/filter?workspaceId=1001&name=Newsletter&pageIndex=0&pageSize=20
```

#### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "33c4#1900ab1402f",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Use o parâmetro `name` quando precisar localizar um modelo por nome.

## Criar

Crie um modelo de email enviando uma carga JSON. `name` e `appData` são obrigatórios. `appData` deve incluir pelo menos `folderId` ou `workspaceId`.

### Solicitação

```http
POST /rest/asset/v2/emailtemplate
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template",
  "description": "Core responsive template",
  "appData": {
    "workspaceId": "1001",
    "folderId": "15",
    "editorType": "emailTemplate"
  },
  "themeId": "42"
}
```

### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "a99f#1900ab1407e",
  "result": [
    {
      "id": "1022",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

O corpo da solicitação também pode incluir `data`, `editorContext`, `appType` e `status`.

### Criar campos

`appData` identifica onde o modelo está armazenado e como ele é editado.

`themeId` pode ser usado para associar um tema ao modelo quando aplicável.

## Atualização

Atualizar um modelo por ID do ativo.

### Solicitação

```http
POST /rest/asset/v2/emailtemplate/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template v2",
  "description": "Updated responsive template",
  "appData": {
    "folderId": "15"
  }
}
```

### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "cf10#1900ab140b3",
  "result": [
    {
      "id": "1022"
    }
  ]
}
```

## Gerenciar estado

Os modelos de e-mail usam um ciclo de vida de rascunho e aprovado. Use o endpoint de transição de estado para aprovar um modelo, cancelar a aprovação, descartar um rascunho ou criar um novo rascunho.

Os valores válidos de `action` são:

- `approve`
- `unapprove`
- `discard`
- `create_draft`

### Solicitação

```http
POST /rest/asset/v2/emailtemplate/state/transition
Content-Type: application/json
```

### Resposta

```json
{
  "contentId": "1022",
  "action": "approve"
}
```

## Clonar

Use o endpoint de clonagem para criar uma cópia de um template existente.

### Solicitação

```http
POST /rest/asset/v2/emailtemplate/clone
Content-Type: application/json
```

### Resposta

```json
{
  "assetId": "1022",
  "newAsset": {
    "name": "Base Newsletter Template Copy",
    "description": "Cloned template"
  }
}
```

## Excluir

Excluir um modelo por ID do ativo.

### Solicitação

```http
POST /rest/asset/v2/emailtemplate/{id}/delete
Content-Type: application/json
```

Esse endpoint pega a id do modelo no caminho e não define um corpo de solicitação.

## Usado por

Use o ponto de extremidade `usedby` para recuperar ativos que fazem referência a um determinado modelo.

### Solicitação

```http
POST /rest/asset/v2/emailtemplate/usedby
Content-Type: application/json
```

### Resposta

```json
{
  "assetId": "1022",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Observações

Os endpoints de consulta retornam metadados para o ativo. Use a referência de endpoint para o esquema de resposta completa e qualquer propriedade específica do ambiente.
