---
title: Fragmentos
feature: REST API
description: Use a API REST do Marketo Asset para consultar, criar, atualizar, clonar, excluir, aprovar e inspecionar dependências de fragmentos.
exl-id: 9dd532d1-1dd7-4581-86dd-1943fab66cbb
source-git-commit: 0e0a3e5a08e81f349044cbc327d1aba963ab30e4
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 9%

---

# Fragmentos

Os fragmentos são ativos de conteúdo reutilizáveis que podem ser referenciados por outros ativos, como emails.

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

Você pode recuperar metadados do fragmento por ID do ativo ou com o endpoint do filtro.

### Por ID

#### Solicitação

```text
GET /rest/asset/v2/fragment/{id}
```

#### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "fa0f#1900ab15001",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "description": "Reusable hero block",
      "status": "approved"
    }
  ]
}
```

### Filtro

O endpoint de filtro permite pesquisar em um espaço de trabalho e restringir os resultados com parâmetros de consulta adicionais. `workspaceId` é obrigatório.

todo: fazer uma tabela
Os filtros suportados incluem `folderId`, `folderIds` repetido, `status`, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `fragmentType`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` e `includeArchived`.

#### Solicitação

```text
GET /rest/asset/v2/fragment/filter?workspaceId=1001&fragmentType=email&pageIndex=0&pageSize=20
```

#### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "f9cc#1900ab1504a",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "status": "approved"
    }
  ]
}
```

Use o parâmetro `name` quando precisar localizar um fragmento por nome.

## Criar

Crie um fragmento enviando uma carga JSON. `name`, `appData` e `settings` são obrigatórios. `settings` deve incluir `fragmentType` e `supportedChannels`.

### Solicitação

```text
POST /rest/asset/v2/fragment
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment",
  "description": "Reusable hero block",
  "appData": {
    "workspaceId": "1001",
    "folderId": "395",
    "editorType": "fragment"
  },
  "settings": {
    "fragmentType": "email",
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "bd57#1900ab1509d",
  "result": [
    {
      "id": "13",
      "name": "Hero Banner Fragment",
      "status": "draft"
    }
  ]
}
```

O corpo da solicitação também pode incluir `data`, `editorContext`, `themeId`, `appType` e `status`.

### Criar campos

* `appData` identifica onde o fragmento está armazenado e como ele é editado.
* `settings.fragmentType` identifica a categoria do fragmento, como um fragmento de email.
* `settings.fragmentSubType` pode ser usado para definir ainda mais o formato do fragmento.
* `settings.supportedChannels` lista os canais nos quais o fragmento pode ser usado.

## Atualização

Atualizar um fragmento por ID do ativo.

### Solicitação

```text
POST /rest/asset/v2/fragment/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment v2",
  "description": "Updated reusable hero block",
  "settings": {
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Resposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "73d9#1900ab150f0",
  "result": [
    {
      "id": "13"
    }
  ]
}
```

## Gerenciar estado

Os fragmentos usam um ciclo de vida de rascunho e aprovado. Use o endpoint de transição de estado para aprovar um fragmento, cancelar a aprovação, descartar um rascunho ou criar um novo rascunho.

Os valores válidos de `action` são:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Solicitação

```text
POST /rest/asset/v2/fragment/state/transition
Content-Type: application/json
```

### Resposta

```json
{
  "contentId": "13",
  "action": "approve"
}
```

## Clonar

Use o endpoint de clonagem para criar uma cópia de um fragmento existente.

### Solicitação

```text
POST /rest/asset/v2/fragment/clone
Content-Type: application/json
```

### Resposta

```json
{
  "assetId": "13",
  "newAsset": {
    "name": "Hero Banner Fragment Copy",
    "description": "Cloned fragment"
  }
}
```

## Excluir

Excluir um fragmento por ID do ativo.

### Solicitação

```text
POST /rest/asset/v2/fragment/{id}/delete
Content-Type: application/json
```

Esse ponto de extremidade pega a ID do fragmento no caminho e não define um corpo de solicitação.

## Usado por

Use o ponto de extremidade `usedby` para recuperar ativos que fazem referência a um determinado fragmento.

### Solicitação

```text
POST /rest/asset/v2/fragment/usedby
Content-Type: application/json
```

### Resposta

```json
{
  "assetId": "13",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```
