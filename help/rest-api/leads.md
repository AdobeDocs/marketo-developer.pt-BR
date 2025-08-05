---
title: Leads
feature: REST API
description: Detalhes sobre as chamadas de API de clientes potenciais
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '3338'
ht-degree: 3%

---

# Leads

[Referência de Ponto de Extremidade de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

A API do líder da Marketo fornece um grande conjunto de recursos para aplicativos CRUD simples contra registros de clientes potenciais, bem como a capacidade de modificar a associação de um cliente potencial em listas estáticas e programas e iniciar o processamento de Campanha inteligente para clientes potenciais.

## Descrever

Um dos principais recursos da API de clientes potenciais é o método Descrever. Use Descrever clientes em potencial para recuperar uma lista completa dos campos disponíveis para interação por meio da API REST, bem como metadados para cada:

* Tipo de dados
* Nomes da API REST
* Comprimento (se aplicável)
* Somente leitura
* Rótulo amigável

Descrever é a principal fonte da verdade sobre se os campos estão disponíveis para uso e os metadados sobre esses campos.

### Solicitação

```
GET /rest/v1/leads/describe.json
```

### Resposta

```json
{
   "requestId":"37ca#1475b74e276",
   "success":true,
   "result":[
      {
         "id":2,
         "displayName":"Company Name",
         "dataType":"string",
         "length":255,
         "rest":{
            "name":"company",
            "readOnly":false
         },
         "soap":{
            "name":"Company",
            "readOnly":false
         }
      }
}
```

Normalmente, as respostas incluem um conjunto muito maior de campos na matriz de resultados, mas nós os omitimos para fins de demonstração. Cada item na matriz de resultados corresponde a um campo disponível no registro de lead e terá, no mínimo, uma id, um displayName e um tipo de dados. Os objetos filho rest e soap podem ou não estar presentes em um determinado campo, e sua presença indicará se o campo é válido para uso nas APIs REST ou SOAP. A propriedade `readOnly` indica se o campo é somente leitura por meio da API correspondente (REST ou SOAP). A propriedade length indica o comprimento máximo do campo, se presente. A propriedade dataType indica o tipo de dados do campo.

## Consultar

Há dois métodos principais para a recuperação de leads: os métodos Obter lead por ID e Obter leads por tipo de filtro. Obter lead por ID pega uma única id de lead como parâmetro de caminho e retorna um único registro de lead.

Como opção, você pode enviar um parâmetro de campos que contém uma lista separada por vírgulas de nomes de campos para retornar. Se o parâmetro fields não estiver incluído nessa solicitação, os seguintes campos padrão serão retornados: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`. Ao solicitar uma lista de campos, se um campo específico for solicitado, mas não for retornado, o valor estará implícito em ser nulo.

### Solicitação

```
GET /rest/v1/lead/{id}.json
```

### Resposta

```json
{
   "requestId": "10226#14d3049e51b",
   "success": true,
   "result": [
      {
         "id": 318581,
         "updatedAt":"2015-05-07T11:47:30-08:00"
         "lastName": "Doe",
         "email": "jdoe@marketo.com",
         "createdAt": "2015-05-01T16:47:30-08:00",
         "firstName": "John"
      }
   ]
}
```

Para esse método, sempre haverá um único registro na primeira posição da matriz de resultados.

Obter Clientes Potenciais por Tipo de Filtro retornará o mesmo tipo de registro, mas pode retornar até 300 por página. Ela requer os parâmetros de consulta `filterType` e `filterValues`.

`filterType` aceita qualquer campo personalizado ou a maioria dos campos usados com frequência. Chame o ponto de extremidade `Describe2` para obter uma lista abrangente de campos pesquisáveis permitidos para uso em `filterType`. Ao pesquisar por Campo Personalizado, somente os seguintes tipos de dados são suportados: `string`, `email`, `integer`. Você pode obter detalhes de campo (descrição, tipo etc.) usando o método Descrever mencionado acima.

`filterValues` aceita até 300 valores em formato separado por vírgulas. A chamada procura registros em que o campo do cliente potencial corresponde a um dos `filterValues` incluídos. Se o número de clientes potenciais correspondentes ao filtro de cliente potencial for maior que 1.000, será retornado o erro: &quot;1003, Muitos resultados correspondem ao filtro&quot;.

Se o tamanho total da solicitação GET exceder 8 KB, um erro HTTP será retornado: &quot;414, URI too long&quot; (de acordo com RFC 7231). Como solução alternativa, você pode alterar o GET para POST, adicionar o parâmetro _method=GET e colocar uma string de consulta no corpo da solicitação.

### Solicitação

```
GET /rest/v1/leads.json?filterType=id&filterValues=318581,318592
```

### Resposta

```json
{
    "requestId": "12951#15699db5c97",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2016-05-17T22:11:45Z",
            "lastName": "Lincoln",
            "email": "abe@usa.gov",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "Abraham"
        },
        {
            "id": 318592,
            "updatedAt": "2016-05-17T22:20:51Z",
            "lastName": "Washington",
            "email": "george@usa.gov",
            "createdAt": "2015-04-06T16:29:21Z",
            "firstName": "George"
        }
    ],
    "success": true
}
```

Esta chamada procura registros que correspondam às IDs incluídas em `filterValues` e retorna quaisquer registros correspondentes.

Se nenhum registro for encontrado, a resposta indicará sucesso, mas a matriz de resultados estará vazia.

### Resposta

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Tanto a opção Obter lead por ID quanto a opção Obter leads por tipo de filtro também aceitarão um parâmetro de consulta de campos, que aceita uma lista separada por vírgulas de campos da API. Se isso for incluído, cada registro na resposta incluirá os campos listados.  Se for omitido, um conjunto padrão de campos será retornado: `id`, `email`, `updatedAt`, `createdAt`, `firstName` e `lastName`.

## ADOBE ECID

Quando o recurso Compartilhamento de público da Adobe Experience Cloud está ativado, ocorre um processo de sincronização de cookies que associa a Adobe Experience Cloud ID (ECID) a leads da Marketo.  Os métodos de recuperação de clientes potenciais mencionados acima podem ser usados para recuperar valores ECID associados.  Faça isso especificando `ecids` no parâmetro de campos. Por exemplo, `&fields=email,firstName,lastName,ecids`.

## Criar e atualizar

Além de recuperar dados de lead, você pode criar, atualizar e excluir registros de lead por meio da API. Criar e atualizar clientes potenciais compartilham o mesmo endpoint com o tipo de operação que está sendo definido na solicitação e até 300 registros podem ser criados ou atualizados ao mesmo tempo.

>[!NOTE]
>
> Não há suporte para a atualização de campos da Empresa usando o ponto de extremidade [Clientes Potenciais de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST). Em vez disso, use o ponto de extremidade [Sincronizar Empresas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Ao criar ou atualizar o valor de email em um registro de Pessoa, somente caracteres ASCII são suportados no campo de endereço de email.

### Solicitação

```
POST /rest/v1/leads.json
```

### Corpo

```json
{
   "action":"createOnly",
   "lookupField":"email",
   "input":[
      {
         "email":"kjashaedd-1@klooblept.com",
         "firstName":"Kataldar-1",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-2@klooblept.com",
         "firstName":"Kataldar-2",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-3@klooblept.com",
         "firstName":"Kataldar-3",
         "postalCode":"04828"
      }
   ]
}
```

### Resposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "id":52,
         "status":"created"
      }
   ]
}
```

Nesta solicitação, você vê dois campos importantes, `action` e `lookupField`.  `action` especifica o tipo de operação da solicitação e pode ser `createOrUpdate`, `createOnly`, `updateOnly` ou `createDuplicate`. Se for omitido, a ação assumirá `createOrUpdate` como padrão.  O parâmetro `lookupField` especifica a chave a ser usada quando a ação for `createOrUpdate` ou `updateOnly`. Se `lookupField` for omitido, a chave padrão será `email`.

Por padrão, a partição padrão é usada. Opcionalmente, você pode especificar o parâmetro `partitionName`, que só funciona se a ação for `createOnly` ou `createOrUpdate`. Para que `partitionName` funcione como critério de desduplicação adicional, ele deve fazer parte do tipo de origem nas regras de desduplicação personalizadas. Durante uma operação de atualização, se um cliente em potencial não existir na partição especificada, um erro será retornado. Se o usuário somente API não tiver permissão para acessar a partição especificada, um erro será retornado.

O campo `id` só pode ser incluído como parâmetro ao ser usada a ação `updateOnly`, pois `id` é uma chave exclusiva gerenciada pelo sistema.

A solicitação também deve ter um parâmetro `input`, que é uma matriz de registros de cliente potencial. Cada registro de lead é um objeto JSON com qualquer número de campos de lead. As chaves incluídas em um registro devem ser exclusivas para esse registro, e todas as cadeias de caracteres JSON devem ser codificadas em UTF-8. O campo `externalCompanyId` pode ser usado para vincular o registro de cliente potencial a um registro de empresa. O campo `externalSalesPersonId` pode ser usado para vincular o registro de cliente potencial a um registro de vendedor.

Observação: ao executar solicitações de substituição de lead simultaneamente ou em rápida sucessão, registros duplicados podem resultar ao fazer várias solicitações com o mesmo valor-chave se uma chamada subsequente com o mesmo valor for feita antes dos primeiros retornos. Isso pode ser evitado usando o `createOnly` ou `updateOnly` conforme apropriado, ou enfileirando chamadas e aguardando sua chamada retornar antes de fazer chamadas de substituição subsequentes com a mesma chave.

## Campos

O objeto de cliente potencial contém campos padrão e, opcionalmente, campos personalizados. Campos padrão estão presentes em todas as assinaturas do Marketo Engage, enquanto campos personalizados são criados pelo usuário conforme necessário. Cada definição de campo é composta de um conjunto de atributos que descrevem o campo. Exemplos de atributos são nome de exibição, nome da API e dataType. Esses atributos são conhecidos coletivamente como metadados.

Os endpoints a seguir permitem consultar, criar e atualizar campos no objeto de cliente potencial. Essas APIs exigem que o usuário da API proprietária tenha uma função com uma ou ambas as permissões de Campo padrão de esquema de leitura-gravação ou Campo personalizado de esquema de leitura-gravação.

## Campos de consulta

Consultar campos de cliente potencial é simples. Você pode consultar um único campo de cliente potencial por nome de API ou consultar o conjunto de todos os campos de cliente potencial. Os campos padrão e personalizados podem ser recuperados, dependendo das permissões de função que estão sendo usadas. Campos ocultos também são recuperados.

## Por nome

O ponto de extremidade Obter Campo de Cliente Potencial por Nome recupera metadados de um único campo no objeto de cliente potencial. O parâmetro de caminho fieldApiName necessário especifica o nome da API do campo. A resposta é como o endpoint Descrever lead, mas contém metadados adicionais, como o atributo isCustom, que indica se o campo é um campo personalizado.

### Solicitação

```
GET /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Resposta

```json
{
    "requestId": "cd97#1793ee0fec4",
    "result": [
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        }
    ],
    "success": true
}
```

## Navegar

O ponto de extremidade Obter campos de cliente potencial recupera metadados para todos os campos no objeto de cliente potencial, incluindo. Por padrão, no máximo 300 registros são retornados. Você pode usar o parâmetro de consulta `batchSize` para reduzir esse número. Se o atributo `moreResult` for true, significa que mais resultados estarão disponíveis. Continue a chamar este ponto de extremidade até que o atributo `moreResult` retorne falso, o que significa que não há resultados disponíveis. O `nextPageToken` retornado desta API deve sempre ser reutilizado para a próxima iteração desta chamada.

### Solicitação

```
GET /rest/v1/leads/schema/fields.json
```

### Resposta (Truncada)

```json
{
    "requestId": "142c3#1793eb976d8",
    "result": [
        {
            "displayName": "Salutation",
            "name": "salutation",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "First Name",
            "name": "firstName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Middle Name",
            "name": "middleName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Last Name",
            "name": "lastName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Date of Birth",
            "name": "dateOfBirth",
            "description": null,
            "dataType": "date",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Phone Number",
            "name": "phone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Mobile Phone Number",
            "name": "mobilePhone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Fax Number",
            "name": "fax",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Job Title",
            "name": "title",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Unsubscribed",
            "name": "unsubscribed",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        ...
    ],
    "success": true,
    "moreResult": false
}
```

## Criar campos

O ponto de extremidade Criar campos de cliente potencial cria um ou mais campos personalizados no objeto de cliente potencial. Esse endpoint fornece funcionalidade comparável à que está disponível na interface do usuário do Marketo Engage. É possível criar até 100 campos personalizados usando esse endpoint.
Considere cuidadosamente cada campo criado na instância de produção do Marketo Engage usando a API.  Depois que um campo é criado, não é possível excluí-lo (você só pode ocultá-lo). A proliferação de campos não utilizados é uma prática recomendada que causará desordem na sua instância.

O parâmetro de entrada necessário é uma matriz de objetos de campo de cliente potencial. Cada objeto contém um ou mais atributos. Os atributos necessários são `displayName`, `name` e `dataType`, que correspondem ao nome de exibição da interface do usuário do campo, ao nome da API do campo e ao tipo de campo, respectivamente.  Opcionalmente, você pode especificar `description`, `isHidden`, `isHtmlEncodingInEmail` e `isSensitive`.

Há algumas regras associadas ao nome e à nomenclatura `displayName`. O atributo name deve ser exclusivo, começar com uma letra e conter apenas letras, números ou sublinhado. O `displayName` deve ser exclusivo e não pode conter caracteres especiais.  Uma convenção de nomenclatura comum é aplicar camel case a `displayName` para produzir nome. Por exemplo, um `displayName` de &quot;Meu campo personalizado&quot; produziria um nome de &quot;myCustomField&quot;.

### Solicitação

```
POST /rest/v1/leads/schema/fields.json
```

### Corpo

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "name": "acmeAccessCode",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      },
      {
        "displayName": "Acme Mail Date",
        "name": "acmeMailDate",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      }
  ]
}
```

### Resposta

```json
{
    "requestId": "d9f1#17943666811",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "created"
        },
        {
            "name": "acmeMailDate",
            "status": "created"
        }
    ],
    "success": true
}
```

## Atualizar campo

O ponto de extremidade Atualizar campo de cliente potencial atualiza um único campo personalizado no objeto de cliente potencial. Na maioria das vezes, as operações de atualização de campo executadas usando a interface do usuário do Marketo Engage são viáveis usando a API. Há algumas diferenças resumidas na tabela abaixo.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Atributo</strong></td>
<td style="width: 35%;" colspan="2"><strong>Campo Padrão</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Campo personalizado</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Atualizável por API?</strong></td>
<td style="width: 17.551%;"><strong>Atualizável pela interface?</strong></td>
<td style="width: 19.3878%;"><strong>Atualizável por API?</strong></td>
<td style="width: 18.8776%;"><strong>Atualizável pela interface?</strong></td>
</tr>
<tr>
<td style="width: 26.5306%;">dataType</td>
<td style="width: 17.449%;">não</td>
<td style="width: 17.551%;">não</td>
<td style="width: 19.3878%;">não</td>
<td style="width: 18.8776%;">sim</td>
</tr>
<tr>
<td style="width: 26.5306%;">descrição</td>
<td style="width: 17.449%;">sim</td>
<td style="width: 17.551%;">sim</td>
<td style="width: 19.3878%;">sim</td>
<td style="width: 18.8776%;">sim</td>
</tr>
<tr>
<td style="width: 26.5306%;">displayName</td>
<td style="width: 17.449%;">não</td>
<td style="width: 17.551%;">não</td>
<td style="width: 19.3878%;">sim</td>
<td style="width: 18.8776%;">sim</td>
</tr>
<tr>
<td style="width: 26.5306%;">isCustom</td>
<td style="width: 17.449%;">não</td>
<td style="width: 17.551%;">não</td>
<td style="width: 19.3878%;">não</td>
<td style="width: 18.8776%;">não</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHidden</td>
<td style="width: 17.449%;">não</td>
<td style="width: 17.551%;">sim</td>
<td style="width: 19.3878%;">sim (se criado pela API)</td>
<td style="width: 18.8776%;">sim</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHtmlEncodingInEmail</td>
<td style="width: 17.449%;">sim</td>
<td style="width: 17.551%;">sim</td>
<td style="width: 19.3878%;">sim</td>
<td style="width: 18.8776%;">sim</td>
</tr>
<tr>
<td style="width: 26.5306%;">isSensitive</td>
<td style="width: 17.449%;">sim</td>
<td style="width: 17.551%;">sim</td>
<td style="width: 19.3878%;">sim</td>
<td style="width: 18.8776%;">sim</td>
</tr>
<tr>
<td style="width: 26.5306%;">length</td>
<td style="width: 17.449%;">não</td>
<td style="width: 17.551%;">não</td>
<td style="width: 19.3878%;">não</td>
<td style="width: 18.8776%;">não</td>
</tr>
<tr>
<td style="width: 26.5306%;">name</td>
<td style="width: 17.449%;">não</td>
<td style="width: 17.551%;">não</td>
<td style="width: 19.3878%;">não</td>
<td style="width: 18.8776%;">não</td>
</tr>
</tbody>
</table>

O parâmetro de caminho `fieldApiName` necessário especifica o nome da API do campo a ser atualizado. O parâmetro de entrada necessário é uma matriz que contém um único objeto de campo de cliente potencial.  O objeto de campo contém um ou mais atributos.

### Solicitação

```
POST /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Corpo

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "description": "Acme Direct Mail Integration",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

### Resposta

```json
{
    "requestId": "9f57#1794324f44c",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Enviar lead ao Marketo

O lead de push é uma alternativa para sincronização de leads no Marketo, projetado principalmente para permitir um grau maior de capacidade de acionamento do que os leads de sincronização padrão (uso semelhante a um formulário do Marketo). Além da sincronização de campos de cliente potencial, esse endpoint permite a associação de clientes potenciais com base em valores de cookie, que são passados para o endpoint. Isso é feito passando o valor `mkt_tok` gerado ao clicar em um email do Marketo ou passando um nome de programa na chamada. Esse endpoint também cria uma única atividade acionável, que está associada a um programa e/ou campanha no Marketo. Isso permite acionar eventos de captura de leads atribuídos a uma campanha ou programa específico para iniciar workflows associados no Marketo.

A interface do lead de push é muito semelhante à Sincronização de leads. Todas as mesmas chaves primárias são válidas, e os mesmos nomes de API são usados para campos (não há parâmetro de ação porque esta é sempre uma operação de substituição). Os parâmetros `programName` e de entrada são obrigatórios, e os parâmetros `lookupField`, `source` e `reason` são opcionais. O parâmetro de entrada é uma matriz de objetos de lead. A atividade resultante é atribuída ao programa nomeado correspondente. Os parâmetros `source` e `reason` são campos de sequência arbitrários que podem ser adicionados à solicitação para incorporar esses valores nas atividades resultantes. Eles podem ser usados como restrições nos acionadores correspondentes (o lead é enviado para o Marketo) e filtros (o lead é enviado para o Marketo).

Observação sobre atividades anônimas. Se desejar associar atividades anônimas anteriores ao lead recém-criado, não especifique o atributo de cookies no objeto do lead e chame Associar Lead após Lead Push. Se quiser criar um novo cliente potencial sem histórico de atividades, basta especificar o atributo de cookies no objeto do cliente potencial.

### Solicitação

```
POST /rest/v1/leads/push.json
```

### Corpo

```json
{
    "programName": "Big Blue Thing Product Launch",
    "source": "Cool Sales Site",
    "reason": "Downloaded pricing sheet",
    "lookupField": "email",
    "input": [
        {
             "email": "Theresa.May@westminister.gov.uk",
             "country": "united kingdom",
             "firstName": "Theresa",
             "website": "www.brexit.com",
             "leadScore": 45,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/23434456",
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/42434",
             "jobTitle": "Sonny"
         }
     ]
}
```

### Resposta

```json
{
    "requestId": "939079529805",
    "success": true,
    "warnings": [],
    "result": [
       {
           "id": 483894,
           "status": "created"
       },
       {
           "id": 1087425,
           "status": "updated"
       },
       {
           "id": 3525,
           "reasons": [
                    {
                        "code": "501",
                        "message": "Bad stuff happened"
                    }
           ]
       }
    ]
}
```

Para passar o parâmetro `mkt_tok`, atribua o valor ao membro mktToken em um registro de cliente potencial no parâmetro de entrada da seguinte maneira.

### Corpo

```json
{
  "programName": "Big Blue Thing Product Launch",
  "source": "Cool Sales Site",
  "reason": "Downloaded pricing sheet",
  "lookupField": "mktToken",
  "input" : [
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Thelma"
     },
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Louise"
     }
   ]
}
```

## Enviar formulário

Enviar formulário é uma alternativa para sincronizar clientes potenciais com o Marketo e foi projetado para fornecer funcionalidade equivalente ao envio de um Formulário do Marketo. Isso permite acionar eventos de captura de leads atribuídos a uma campanha ou programa específico para iniciar workflows associados no Marketo.

O endpoint Enviar formulário é compatível com a seguinte funcionalidade:

* Substitui um registro de cliente potencial usando o campo de email como chave primária
* Cria uma atividade &quot;Preencher formulário&quot; associada a um programa e/ou campanha
* Permite a associação de clientes potenciais com base no valor do cookie
* Executa validação de campo de formulário

O envio de um formulário segue o padrão de banco de dados de clientes potenciais padrão. Um único registro de objeto é transmitido no membro de entrada necessário do corpo JSON de uma solicitação POST. O membro `formId` necessário contém a ID de formulário do Marketo de destino.

O `programId` opcional pode ser usado para especificar o programa ao qual adicionar o cliente em potencial e/ou especificar o programa ao qual adicionar campos personalizados de membros do programa. Se `programId` for fornecido, o cliente potencial será adicionado ao programa e todos os campos de membro do programa presentes no formulário também serão adicionados. Observe que o programa especificado deve estar no mesmo espaço de trabalho que o formulário. Se o formulário não contiver campos personalizados de membros do programa e `programId` não for fornecido, o cliente em potencial não será adicionado a um programa. Se o formulário residir em um programa e `programId` não for fornecido, esse programa será usado quando um ou mais campos personalizados de membro do programa estiverem presentes no formulário.

No registro de entrada, o objeto `leadFormFields` é obrigatório. Este objeto contém um ou mais pares nome/valor que correspondem aos campos de formulário a serem preenchidos.  Todos os campos especificados devem ser definidos no formulário especificado. O nome é o nome da API REST do campo. Observe que o campo `email` é obrigatório.

O objeto membro `visitorData` é opcional e contém pares de nome/valor que correspondem aos dados de visita de página, incluindo `pageURL`, `queryString`, `leadClientIpAddress` e `userAgentString`. Pode ser usado para preencher campos de atividade adicionais para fins de filtragem e acionamento.

A string do membro do cookie é opcional e permite associar um cookie do Munchkin a um registro de pessoa no Marketo. Quando um novo lead é criado, todas as atividades anônimas anteriores são associadas a esse lead, a menos que o valor do cookie tenha sido previamente associado a outro registro conhecido. Se o valor do cookie foi associado anteriormente, as novas atividades são rastreadas em relação ao registro, mas as atividades antigas não serão migradas do registro conhecido existente. Para criar um novo lead sem histórico de atividade, basta omitir o membro do cookie.

Novos clientes potenciais são criados na partição primária do espaço de trabalho no qual o formulário reside.

### Solicitação

```
POST /rest/v1/leads/submitForm.json
```

### Header

```
Content-Type: application/json
```

### Corpo

```json
{
  "formId": 1029,
  "input": [
    {
      "leadFormFields": {
        "firstName": "Marge",
        "lastName": "Simpson",
        "email": "marge.simpson@fox.com",
        "pMCFField": "PMCF value"
      },
      "visitorData": {
        "pageURL": "https://na-sjst.marketo.com/lp/063-GJP-217/UnsubscribePage.html",
        "queryString": "Unsubscribed=yes",
        "leadClientIpAddress": "192.150.22.5",
        "userAgentString": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
      },
      "cookie": "id:063-GJP-217&token:_mch-marketo.com-1594662481190-60776"
    }
  ]
}
```

### Resposta

```json
{
  "requestId": "10667#173bc585ca5",
  "result": [
    {
      "id": 319174,
      "status": "updated"
    }
  ],
  "success": true
}
```

Aqui podemos ver os detalhes da atividade &quot;Preencher formulário&quot; correspondente na interface do usuário do Marketo Engage:

![Preencher Interface do Usuário do Formulário](assets/fill_out_form_activity_details.png)

## Mesclar

Às vezes, é necessário mesclar registros duplicados, e o Marketo facilita isso por meio da API de mesclagem de leads. A mesclagem de clientes potenciais combinará seus logs de atividades, programas, campanhas e associações de listas e informações de CRM, bem como mesclará todos os valores de campo em um único registro. A Mesclagem de Clientes Potenciais utiliza uma ID de cliente potencial como parâmetro de caminho e um único `leadId` como parâmetro de consulta ou uma lista de IDs separadas por vírgulas no parâmetro `leadIds`.

### Solicitação

```
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Resposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

O lead especificado no parâmetro de caminho é o lead vencedor, portanto, se houver campos em conflito entre os registros que estão sendo mesclados, o valor do vencedor será obtido, exceto se o campo no registro vencedor estiver vazio e o campo correspondente no registro perdedor não estiver. Os clientes potenciais especificados nos parâmetros `leadId` ou `leadIds` são os clientes potenciais perdidos.

Se você tiver uma assinatura habilitada para sincronização com SFDC, também poderá usar o parâmetro `mergeInCRM` em sua solicitação. Se definido como true, a mesclagem correspondente no CRM também será executada. Se ambos os clientes em potencial estiverem na SFDC e um for um cliente potencial do CRM e o outro for um contato do CRM, o vencedor será o contato do CRM (independentemente de qual cliente potencial for especificado como vencedor). Se um dos leads estiver no SFDC e o outro for somente no Marketo, o vencedor será o lead da SFDC (independentemente de qual lead é especificado como vencedor).

## Associar Atividade da Web

Por meio do Rastreamento de leads (Munchkin), a Marketo registra a atividade dos visitantes do seu site e das páginas de aterrissagem da Marketo na Web. Essas atividades, Visitas e Cliques, são registradas com uma chave que corresponde a um cookie &quot;_mkto_track&quot; definido no navegador do lead, e o Marketo usa isso para rastrear as atividades da mesma pessoa. Normalmente, a associação a registros de cliente potencial ocorre quando um cliente potencial clica em um email do Marketo ou preenche um formulário do Marketo, mas às vezes uma associação pode ser acionada por um tipo diferente de evento e você pode usar o ponto de extremidade Associar cliente potencial para fazer isso. O endpoint assume a ID do registro de lead conhecido como um parâmetro de caminho e o valor do cookie &quot;_mkto_trk&quot; no parâmetro de consulta de cookie.

### Solicitação

```
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Resposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Se um cookie já estiver associado a um registro de cliente potencial conhecido, o uso dessa API em um registro de cliente potencial diferente fará com que a nova atividade da Web seja registrada em relação a esse registro, mas não moverá nenhuma atividade da Web existente para o novo registro.
Associação

Os registros de clientes potenciais também podem ser recuperados com base na associação a uma lista estática ou a um programa. Além disso, você pode recuperar todas as listas estáticas, programas ou campanhas inteligentes das quais um lead é membro.

A estrutura de resposta e os parâmetros opcionais são idênticos aos de Obter leads por tipo de filtro, embora filterType e filterValues não possam ser usados com essa API.
Para acessar a ID da lista por meio da interface do usuário do Marketo, navegue até a lista. A lista `id` está na URL da lista estática, `https://app-****.marketo.com/#ST1001A1`. Neste exemplo, 1001 é o `id` da lista.

### Solicitação

```
GET /rest/v1/list/{listId}/leads.json?batchSize=3
```

### Resposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "nextPageToken":
"PS5VL5WD4UOWGOUCJR6VY7JQO2KUXL7BGBYXL4XH4BYZVPYSFBAONP4V4KQKN4SSBS55U4LEMAKE6===",
    "result":[
       {
            "id":50,
            "email":"kjashaedd@klooblept.com",
            "firstName":"Kataldar",
             "postalCode":"04828"
       },
       {
           "id":2343,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
           "postalCode":"04828"
       },
      {
           "id":88498,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
         "postalCode":"04828"
         }
    ]
}
```

O ponto de extremidade Obter Listas por Id de Cliente Potencial pega o parâmetro de caminho `id` do registro de cliente potencial e retorna todos os registros de lista estática dos quais o cliente potencial é membro.

### Solicitação

```
GET /rest/v1/leads/{id}/listMembership.json?batchSize=3
```

### Resposta

```json
{
    "requestId": "1184b#1706f0ec23f",
    "result": [
        {
            "listId": 3379,
            "createdAt": "2016-05-17T19:32:44Z",
            "updatedAt": "2016-05-17T19:32:44Z"
        },
        {
            "listId": 2792,
            "createdAt": "2009-05-19T18:29:15Z",
            "updatedAt": "2009-05-19T18:29:15Z"
        },
        {
            "listId": 42,
            "createdAt": "2009-04-22T19:24:22Z",
            "updatedAt": "2009-04-22T19:24:22Z"
        }
    ],
    "success": true,
    "nextPageToken": "BFRV7OMVSNJWDVKVTUFS3XHT4E======",
    "moreResult": true
}
```

## Programas

A associação ao programa pode ser recuperada de maneira semelhante às listas. Os mesmos parâmetros de solicitação opcionais estão disponíveis ao chamar o ponto de extremidade Obter clientes em potencial por ID de programa e passar o parâmetro de caminho `programId`.

Como opção, você pode enviar um parâmetro de campos que contém uma lista separada por vírgulas de nomes de campos para retornar. Se o parâmetro fields não estiver incluído nessa solicitação, os seguintes campos padrão serão retornados: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` e `id`. Ao solicitar uma lista de campos, se um campo específico for solicitado, mas não for retornado, o valor estará implícito em ser nulo.

A estrutura de resposta é muito semelhante, pois cada item na matriz de resultados é um lead, exceto que cada registro também tem um objeto filho chamado &quot;associação&quot;. Este objeto de associação inclui dados sobre a relação do cliente potencial com o programa indicado na chamada, sempre mostrando seus `progressionStatus`, `acquiredBy`, `reachedSuccess` e `membershipDate`. Se o programa pai também for um programa de compromisso, a associação terá membros `stream`, `nurtureCadence` e `isExhausted` para indicar sua posição e atividade no programa de compromisso.

### Solicitação

```
GET /rest/v1/leads/programs/{programId}.json?batchSize=3
```

### Resposta

```json
{
    "requestId": "13ad4#1727b748a17",
    "result": [
        {
            "id": 319141,
            "firstName": "Meera",
            "lastName": "Reed",
            "email": "mree@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319142,
            "firstName": "Jon",
            "lastName": "Umber",
            "email": "jumb@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319143,
            "firstName": "Lyanna",
            "lastName": "Mormont",
            "email": "lmor@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        }
    ],
    "success": true,
    "nextPageToken": "SW3PTMBVFCNHSHJGZ7LQH3ZWNUOHKADJZ3MOQ2LOZZVNO3WEIUPDKPRTTHBSMW756KOCWURTOF2XS==="
}
```

O ponto de extremidade Obter Programas por ID de Cliente Potencial pega um parâmetro de caminho de ID de registro de cliente potencial e retorna todos os registros de programa dos quais o cliente potencial é membro. Os parâmetros opcionais `filterType` e `filterValues` permitem filtrar pela ID do programa.

### Solicitação

```
GET /rest/v1/leads/{id}/programMembership.json
```

### Resposta

```json
{
    "requestId": "12e84#1706f13a379",
    "result": [
        {
            "id": 1044,
            "progressionStatus": "Sent",
            "isExhausted": false,
            "acquiredBy": false,
            "reachedSuccess": false,
            "membershipDate": "2016-05-27T19:50:29Z",
            "updatedAt": "2016-05-27T19:50:29Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Campanhas inteligentes

O ponto de extremidade Obter campanhas inteligentes por ID do lead usa um parâmetro de caminho de ID de registro do lead e retorna todos os registros de campanha inteligente dos quais o lead é membro.

### Solicitação

```
GET /rest/v1/leads/{id}/smartCampaignMembership.json?batchSize=3
```

### Resposta

```json
{
    "requestId": "e7b0#1706f163632",
    "result": [
        {
            "smartCampaignId": 3746,
            "createdAt": "2018-06-01T18:00:04Z",
            "updatedAt": "2018-06-01T18:00:06Z"
        },
        {
            "smartCampaignId": 3678,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:41Z"
        },
        {
            "smartCampaignId": 3680,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:40Z"
        }
    ],
    "success": true,
    "nextPageToken": "TNGAH3NKDUFDHNXUVGTNBXJCQM======",
    "moreResult": true
}
```

## Excluir

A remoção de clientes em potencial é simples e direta usando o ponto de extremidade Excluir clientes em potencial.  Especifique as IDs de cliente potencial a serem excluídas usando os atributos de ID no corpo.  O máximo é de 300 clientes em potencial por solicitação.  Usar tipo de conteúdo: cabeçalho application/json.

### Solicitação

```
POST /rest/v1/leads/delete.json
```

### Corpo

```json
{
   "input":[
      {
         "id": 235
      },
      {
         "id":766
      }
   ]
}
```

### Resposta

```json
{
  "requestId":"3608#16664333670",
  "result":[
    {
      "id":235,
      "status":"deleted"
    },
    {
      "id":766,
      "status":"deleted"
    }
  ],
  "success":true
}
```

## Relações

* Empresas por meio do campo externalCompanyId no registro de cliente potencial
* SalesPersons por meio do campo externalSalesPersonId no registro de cliente potencial
* Programas por meio da associação a programas
* Listas por meio da associação à lista
* Atividades por meio do campo leadId na atividade
* Segmentação por campos de segmento individuais no registro de cliente potencial
* Partições por meio de leadPartitionId no registro de cliente potencial

## Tempos limite

Os pontos de extremidade de clientes potenciais têm um tempo limite de 30s, a menos que observado abaixo:

* Clientes potenciais de sincronização: 90s
* Associar lead: 60s
* Mesclar leads: 180s
* Atualizar Partição de Cliente Potencial: 60s
* Enviar lead para o Marketo: 90s
* Obter clientes em potencial por tipo de filtro: 60s
* Obter leads pela ID de lista: 60s
