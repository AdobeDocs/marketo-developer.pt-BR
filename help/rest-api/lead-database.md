---
title: "Banco de Dados Principal"
feature: REST API, Database
description: "Manipular o banco de dados principal de clientes potenciais."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 0%

---


# Banco de dados de leads

As APIs de banco de dados de clientes potenciais da Marketo são as APIs mais utilizadas que a Marketo fornece, pois permitem a troca de dados de pessoas e dados relacionados à pessoa da Marketo, como Atividades, Oportunidades e Empresas.

## Objetos 

Os objetos do banco de dados de clientes potenciais incluem o seguinte:

- Leads
- Empresas/contas
- Contas nomeadas
- Oportunidades
- FunçõesOportunidade
- Vendedores
- Objetos personalizados
- Atividades
- Lista e membros do programa

A maioria desses objetos inclui pelo menos os métodos Create, Read, Update e Delete. Também está incluído um método &quot;Describe&quot; que fornece uma lista de campos disponíveis para cada tipo e uma lista de campos usados para desduplicação (para objetos que não sejam de cliente potencial) e quais campos podem ser pesquisados para recuperação de registros. O conjunto mais avançado é fornecido para clientes potenciais, pois eles têm a maior variedade de recursos nos aplicativos Marketo.

## API

Para obter uma lista completa dos endpoints da API do banco de dados de clientes potenciais, incluindo parâmetros e informações de modelagem, consulte o [Referência de Ponto de Extremidade de API de Banco de Dados Principal](https://developer.adobe.com/marketo-apis/api/mapi/).

Para instâncias com uma integração nativa do CRM habilitada (Microsoft Dynamics ou Salesforce.com), as APIs Empresa, Oportunidade, Função Oportunidade e Vendedor estão desabilitadas. Os registros são gerenciados por meio do CRM quando ativados e não podem ser acessados ou atualizados por meio das APIs do Marketo.

- Tamanho máximo do lote (padrão): 300 registros
- Tamanho máximo do lote (em massa): arquivo de 10 MB
- Tamanho de lote padrão: 300 registros
- Cabeçalho tipo conteúdo (padrão): application/json
- Cabeçalho de tipo de conteúdo (em massa): multipart/form-data

## Descrever

Para clientes potenciais, empresas, oportunidades, funções, vendedores e objetos personalizados, é fornecida uma API de descrição. Chamar essa opção recupera metadados para o objeto e uma lista de campos disponíveis para atualização e consulta. Descrever é uma parte essencial do design de uma integração adequada com o Marketo. Ele fornece metadados avançados sobre como os objetos podem ou não ter interação, bem como sobre como eles podem ser criados, atualizados e consultados. Além de Descrever clientes em potencial, cada um deles retorna uma lista de chaves disponíveis para `deduplication` no `dedupeFields` parâmetro de resposta. Uma lista de campos está disponível como chaves para consulta no `searchableFields` parâmetro de resposta.

```
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

Neste exemplo, `dedupeFields` é na verdade uma chave composta. Isso significa que em atualizações futuras e cria, ao usar o `dedupeFields` , você deve incluir todos os três `externalOpportunityId`, `leadId`, e `role` para cada função. A variável `searchableFields` também fornece a lista de campos disponíveis para consultar registros de função. Isso também inclui a chave composta de `externalOpportunityId`, `leadId`, e `role`.

Também há um parâmetro de resposta de campos, que fornecerá o nome de cada campo, o campo `displayName` como aparece na interface do usuário do Marketo, o tipo de dados do campo, se ele pode ser atualizado após a criação, e o comprimento do campo, se aplicável.

## Consultar

Todos os objetos de banco de dados de clientes potenciais compartilham um padrão básico para consulta com chaves simples, onde apenas um campo é referenciado.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Para todos os objetos, exceto clientes em potencial, você pode selecionar seu {field to query} nos searchableFields da chamada de descrição correspondente e compor uma lista separada por vírgulas de até 300 valores. Há também estes parâmetros de consulta opcionais:

- `batchSize` - Uma contagem inteira do número de resultados a serem retornados. O padrão e o máximo são 300.
- `nextPageToken` - Token retornado de uma chamada anterior para paginação. Consulte [Tokens de paginação](paging-tokens.md) para obter mais detalhes.
- `fields` - Uma lista separada por vírgulas de nomes de campos a serem retornados para cada registro. Consulte a descrição correspondente para obter uma lista de campos válidos. Se um determinado campo for solicitado, mas não for retornado, o valor será considerado nulo.
- `_method` - Usado para enviar consultas usando o método HTTP POST. Consulte a seção _method=GET abaixo para obter detalhes sobre o uso.

Para ver um exemplo rápido, vamos ver as oportunidades de consulta:

```
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

A variável `filterType` especificado nesta chamada é &quot;idField&quot; e não &quot;marketoGUID&quot;. Isso e &quot;dedupeFields&quot; são casos especiais, em que o campo correspondente ao idField ou dedupeFields pode receber alias dessa maneira. O &quot;marketoGUID&quot; ainda é o campo de pesquisa resultante na chamada, mas não está explicitamente definido na chamada. Os campos e/ou conjuntos de campos indicados pelo `idField` e `dedupeFields` de uma descrição de objeto sempre será válida `filterTypes` para uma consulta. Essa chamada procura registros correspondentes às GUIDs incluídas em filterValues e retorna quaisquer registros correspondentes. Se nenhum registro for encontrado usando esse método, a resposta ainda indicará sucesso. No entanto, a matriz de resultados estará vazia, pois a pesquisa foi executada com êxito, mas não havia registros para retornar.

Se o conjunto de registros na consulta exceder 300 ou o `batchSize` especificado, o que for menor, a resposta terá um membro `moreResult` com um valor de true e um `nextPageToken`, que pode ser incluído em uma chamada subsequente para recuperar mais do conjunto. Consulte [Tokens de paginação](paging-tokens.md) para obter mais detalhes.

### URIs longos

Às vezes, como ao consultar por GUIDs, seu URI pode ser longo e exceder os 8 KB permitidos pelo serviço REST. Nesse caso, você deve usar o método HTTP POST em vez do GET e adicionar um parâmetro de consulta `_method=GET`. Além disso, o restante dos parâmetros de consulta devem ser passados no corpo do POST como uma string &quot;application/x-www-form-urlencoded&quot; e passar o cabeçalho Content-type associado.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Além de URIs longos, esse parâmetro também é necessário ao consultar chaves compostas.

### Chaves compostas

O padrão para consultar chaves compostas é diferente das chaves simples, pois requer o envio de um POST com um corpo JSON. Tal não é necessário em todos os casos, `dedupeFields` com vários campos é usada como a variável `filterType`. Atualmente, as chaves compostas são usadas apenas por Funções de oportunidade e alguns objetos personalizados. Vamos analisar um exemplo de uma consulta para Funções de oportunidade com a chave composta de `dedupeFields`:

```
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

A estrutura do objeto JSON é em sua maioria simples, e todos os parâmetros de consulta para consultas com chaves simples são membros válidos, exceto para `filterValues`. Em vez de um valor de filtro, há uma matriz &quot;entrada&quot; de objetos JSON, que cada um deve ter um membro para cada um dos campos na chave composta; nesse caso, eles são `externalOpportunityId`, `leadId`, e `role`. Isso executa uma consulta para `roles`, em relação às entradas fornecidas e retornam os resultados correspondentes. Se a resposta retornar um parâmetro com `moreResult=true`, e uma `nextPageToken`, você deve incluir todas as entradas originais e as `nextPageToken` para que a consulta seja executada corretamente.

## Criar e atualizar

As criações e atualizações de registros de bancos de dados de clientes potenciais são realizadas por meio de POSTs com corpos JSON. As interfaces para Oportunidades, Funções, Objetos Personalizados, Empresas e Vendedores são as mesmas. A interface do lead é um pouco diferente, e você pode ler mais sobre ele especificamente.

O único parâmetro obrigatório é uma matriz chamada `input` contendo até 300 objetos, cada um com os campos que você deseja inserir/atualizar como membros. Também é possível incluir uma `action` que pode ser um de: `createOnly`, `updateOnly`ou `createOrUpdate`. Se a ação for omitida, o modo assumirá como padrão `createOrUpdate`. `dedupeBy` é outro parâmetro opcional que pode ser usado quando a ação está definida como createOnly ou `createOrUpdate`. ` dedupeBy` pode ser `idField`ou `dedupeFields`. Se `idField` for selecionada, então a variável `idField` As informações listadas na descrição são usadas para desduplicação e devem ser incluídas em cada registro. `idField` não é compatível com `createOnly` modo. Se `dedupeFields` são selecionados e, em seguida, o `dedupeFields` listado na descrição de objeto usada, e cada um deve ser incluído em cada registro. Se a variável `dedupeBy` for omitido, o modo assumirá `dedupeFields`.

Ao passar uma lista de valores de campo, um valor de `null`ou uma cadeia de caracteres vazia, é gravada no banco de dados como `null`.

```
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

Além da API de clientes potenciais, as chamadas para criar ou atualizar objetos de banco de dados de clientes potenciais retornam um `seq` em cada objeto na variável `result` matriz. O número listado corresponde à ordem do registro atualizado na solicitação feita. Cada item retorna o valor da variável `idField` para o tipo de objeto e uma `status`. O campo de status indica &quot;criado&quot;, &quot;atualizado&quot; ou &quot;ignorado&quot;.  Se o status for ignorado, também haverá uma matriz de &quot;motivos&quot; correspondente com um ou mais objetos de motivo que incluem um código e uma mensagem, indicando por que um registro foi ignorado. Consulte [códigos de erro](error-codes.md) para obter detalhes adicionais.

### Excluir

A interface para exclusões é padrão para objetos de banco de dados de clientes potenciais além de clientes potenciais. Além da entrada, há apenas um parâmetro obrigatório `deleteBy,` que pode ter um valor de idField ou dedupeFields. Vamos analisar a exclusão de alguns objetos personalizados.

```
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

A variável `seq`, `status`, `marketoGUID`, e `reasons` Todos já devem estar familiarizados com você.

Para obter mais detalhes sobre como trabalhar com operações CRUD para cada tipo de objeto individual, verifique suas respectivas páginas.
