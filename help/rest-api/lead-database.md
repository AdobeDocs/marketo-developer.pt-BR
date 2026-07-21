---
title: Banco de dados de leads
feature: REST API, Database
description: Guia para APIs de banco de dados de clientes potenciais da Marketo que abrangem objetos, métodos CRUD e Describe, padrões de consulta, limites de lote e restrições de integração de CRM.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
TQID: https://experienceleague.adobe.com/7lGbhE92lvIE-XkMyUIaK9GrreZVRdM-WVZTpHARhxE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1058
ht-degree: 1%

---

# Banco de dados de leads

As APIs do banco de dados de clientes potenciais da Marketo trocam dados pessoais e relacionados com a Marketo. Esses dados incluem Atividades, Oportunidades e Empresas.

## Objetos

O banco de dados de clientes potenciais inclui os seguintes objetos:

- Leads
- Empresas/contas
- Contas nomeadas
- Oportunidades
- FunçõesOportunidade
- Vendedores
- Objetos personalizados
- Atividades
- Lista e membros do programa

A maioria dos objetos de banco de dados de clientes potenciais suporta os métodos Create, Read, Update e Delete. O método Descrever fornece os campos disponíveis para cada tipo de objeto. Para objetos que não são de cliente potencial, também identifica campos usados para desduplicação e campos que podem ser pesquisados ao recuperar registros.

Os objetos de cliente potencial oferecem suporte ao mais amplo conjunto de recursos, pois os clientes potenciais têm a maior variedade de usos em aplicativos Marketo.

## API

Para obter uma lista completa de pontos de extremidade, parâmetros e informações de modelagem da API do Banco de Dados Principal, consulte a [Referência de Ponto de Extremidade da API do Banco de Dados Principal](https://developer.adobe.com/marketo-apis/api/mapi).

Quando uma instância tem uma integração nativa de CRM do Microsoft Dynamics ou Salesforce.com, as APIs Empresa, Oportunidade, Função da oportunidade e Vendedor são desabilitadas. O CRM gerencia esses registros para que você não possa acessá-los ou atualizá-los por meio das APIs do Marketo.

- Tamanho máximo do lote (padrão): 300 registros
- Tamanho máximo do lote (em massa): arquivo de 10 MB
- Tamanho de lote padrão: 300 registros
- Cabeçalho tipo conteúdo (padrão): application/json
- Cabeçalho de tipo de conteúdo (em massa): multipart/form-data

## Descrever

A API de descrição está disponível para clientes potenciais, empresas, oportunidades, funções, vendedores e objetos personalizados. Use-a para recuperar metadados de objeto e os campos disponíveis para atualizações e consultas.

Exceto para Descrever clientes em potencial, cada ponto de extremidade Descrever retorna:

- `dedupeFields`: chaves disponíveis para desduplicação.
- `searchableFields`: Chaves disponíveis para consultas.

```http
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

Neste exemplo, `dedupeFields` é uma chave composta. Ao usar o modo `dedupeFields` para criações e atualizações futuras, inclua `externalOpportunityId`, `leadId` e `role` para cada função.

A matriz `searchableFields` lista os campos disponíveis para consultar registros de função. Esta lista inclui a chave composta de `externalOpportunityId`, `leadId` e `role`.

O parâmetro de resposta `fields` fornece as seguintes informações para cada campo:

- Nome.
- `displayName` conforme mostrado na interface do Marketo.
- Tipo de dados.
- Se o campo pode ser atualizado após a criação.
- Comprimento do campo, se aplicável.

## Consultar

Os objetos de Banco de Dados de Cliente Potencial compartilham um padrão de consulta básico para chaves simples que fazem referência a um campo.

```http
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Para todos os objetos, exceto clientes em potencial, selecione `{field to query}` de `searchableFields` na resposta Descrever correspondente. Fornece uma lista separada por vírgulas de até 300 valores.

Você também pode incluir estes parâmetros de consulta opcionais:

- `batchSize`: um número inteiro que especifica o número de resultados a serem retornados. O valor padrão e máximo é 300.
- `nextPageToken`: Um token retornado de uma chamada anterior para paginação. Consulte [Tokens de paginação](paging-tokens.md) para obter mais informações.
- `fields`: uma lista separada por vírgulas de nomes de campos a serem retornados para cada registro. Consulte a descrição correspondente para campos válidos. Se você solicitar um campo que não é retornado, seu valor estará implícito em ser nulo.
- `_method`: envia consultas usando o método POST HTTP. Consulte a seção _method=GET para obter detalhes sobre o uso.

O exemplo a seguir consulta oportunidades:

```http
GET /rest/v1/opportunities.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

O `filterType` nesta chamada é &quot;idField&quot;, não &quot;marketoGUID&quot;. &quot;idField&quot; e &quot;dedupeFields&quot; são casos especiais que permitem usar um alias para o campo ou campos correspondentes. Embora a chamada não defina explicitamente &quot;marketoGUID&quot;, ela permanece como o campo de pesquisa.

Os campos ou conjuntos de campos identificados por `idField` e `dedupeFields` em uma descrição de objeto são sempre válidos `filterTypes` para uma consulta. Essa chamada retorna registros que correspondem às GUIDs em filterValues. Se nenhum registro for correspondente, a resposta indicará sucesso e retornará uma matriz de resultados vazia.

Se o conjunto de registros correspondente exceder 300 ou o `batchSize` especificado, o que for menor, a resposta incluirá `moreResult` com um valor true e um `nextPageToken`. Inclua o token em uma chamada subsequente para recuperar mais registros. Consulte [Tokens de paginação](paging-tokens.md) para obter mais informações.

### URIs longos

Um URI pode exceder o limite de 8 KB do serviço REST, como quando você consulta por GUIDs. Nesse caso, use o método HTTP POST em vez de GET e adicione o parâmetro de consulta `_method=GET`.

Passe os parâmetros de consulta restantes no corpo do POST como uma string &quot;application/x-www-form-urlencoded&quot;. Passe também o cabeçalho Content-type associado.

```http
POST /rest/v1/opportunities.json?_method=GET
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

O parâmetro `_method=GET` também é necessário ao consultar chaves compostas.

### Chaves compostas

Para consultar uma chave composta, envie uma solicitação POST com um corpo JSON. Use este padrão somente quando `filterType` for uma opção `dedupeFields` com vários campos.

Atualmente, as chaves compostas são usadas apenas por Funções de oportunidade e alguns objetos personalizados. O exemplo a seguir consulta Funções de Oportunidade com a chave composta de `dedupeFields`:

```http
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input":[
      {
        "externalOpportunityId":"Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId":"Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId":"Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

O objeto JSON aceita todos os parâmetros de consulta usados para consultas de chave simples, exceto `filterValues`. Em vez de `filterValues`, forneça uma matriz de &quot;entrada&quot; de objetos JSON. Cada objeto deve incluir cada campo na chave composta. Neste exemplo, os campos são `externalOpportunityId`, `leadId` e `role`.

A solicitação consulta `roles` com base nas entradas fornecidas e retorna resultados correspondentes. Se a resposta incluir `moreResult=true` e um `nextPageToken`, inclua todas as entradas originais e o `nextPageToken` na próxima solicitação.

## Criar e atualizar

Criar e atualizar registros do Banco de Dados de Cliente Potencial enviando solicitações POST com corpos JSON. Oportunidades, Funções, Objetos Personalizados, Empresas e Vendedores usam a mesma interface. Os clientes potenciais usam uma interface diferente, descrita na documentação de Clientes potenciais.

O único parâmetro necessário é `input`, uma matriz de até 300 objetos. Cada objeto contém os campos a serem inseridos ou atualizados.

Você também pode incluir estes parâmetros opcionais:

- `action`: Aceita `createOnly`, `updateOnly`, ou `createOrUpdate`. Se omitido, o modo assumirá `createOrUpdate` como padrão.
- `dedupeBy`: Aceita `idField` ou `dedupeFields` quando a ação está definida como createOnly ou `createOrUpdate`. Se omitido, o modo assumirá `dedupeFields` como padrão.

Quando `dedupeBy` é `idField`, o `idField` listado na descrição é usado para desduplicação e deve ser incluído em cada registro. O modo `idField` não é compatível com o modo `createOnly`.

Quando `dedupeBy` for `dedupeFields`, inclua cada campo `dedupeFields` listado na descrição do objeto em cada registro.

Quando você passa valores de campo, o banco de dados grava um valor de `null` ou uma cadeia de caracteres vazia como `null`.

```http
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

Exceto para a API de clientes potenciais, as chamadas de criação e atualização retornam um campo `seq` em cada objeto na matriz `result`. O número corresponde à posição do registro atualizado na solicitação.

Cada resultado também retorna o valor `idField` do tipo de objeto e um `status` de &quot;criado&quot;, &quot;atualizado&quot; ou &quot;ignorado&quot;. Se o status for ignorado, o resultado incluirá uma matriz de &quot;motivos&quot;. Cada objeto de motivo contém um código e uma mensagem que explicam por que o registro foi ignorado. Consulte [códigos de erro](error-codes.md) para obter mais informações.

### Excluir

Exceto para clientes potenciais, os objetos de Banco de Dados de Clientes Potenciais usam uma interface de exclusão padrão. Além da entrada, o único parâmetro necessário é `deleteBy,` que aceita idField ou dedupeFields.

O exemplo a seguir exclui objetos personalizados:

```http
POST /rest/v1/customobjects/{name}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

A resposta inclui `seq`, `status` e `marketoGUID`. Para registros ignorados, também inclui `reasons`.

Para obter detalhes sobre operações CRUD para um tipo de objeto específico, consulte a documentação desse objeto.
