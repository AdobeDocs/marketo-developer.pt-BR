---
title: Assimilação de dados
feature: REST API, Dynamic Content
description: Use a API de assimilação de dados do Marketo para assimilação de alto volume e baixa latência de pessoas, objetos personalizados, empresas e membros do programa.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '1789'
ht-degree: 15%

---

# API de assimilação de dados

A API de assimilação de dados é um serviço de alto volume, baixa latência e alta disponibilidade, projetado para lidar com a assimilação de grandes quantidades de dados pessoais e relacionados a pessoas de maneira eficiente e com atrasos mínimos.

Os dados são assimilados enviando solicitações que são executadas de forma assíncrona. O status da solicitação pode ser recuperado assinando-se a eventos do [Fluxo de Dados de Observação do Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).

As interfaces são oferecidas para quatro tipos de objetos: Pessoas, Objetos Personalizados, Empresas e Membros do Programa. A operação de registro é somente &quot;inserir ou atualizar&quot;, exceto para Membros do Programa que também suportam exclusão.

>[!NOTE]
>
>O acesso à API de assimilação de dados exige o direito ao pacote [Camada de desempenho do Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticação

A API de assimilação de dados usa o mesmo método de autenticação OAuth 2.0 que a API REST do Marketo para gerar um token de acesso, mas o token de acesso deve ser passado pelo cabeçalho HTTP `X-Mkto-User-Token`. Não é possível passar o token de acesso por meio de um parâmetro de consulta.

Exemplo de token de acesso pelo cabeçalho:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Permissões

A assimilação de dados usa o mesmo modelo de permissões que a API REST do Marketo e não requer permissões especiais adicionais para uso, embora permissões específicas sejam necessárias para cada endpoint.

| Terminal | Permissão |
| --- | --- |
| Pessoas | Lead de leitura-gravação |
| Objetos personalizados | Objeto personalizado de leitura-gravação |
| Empresas | Empresa de leitura-gravação |
| Membros do programa | Lead de leitura-gravação |

## Tipos de objeto suportados

| Tipo de objeto | Operações suportadas |
| --- | --- |
| Pessoas | Substituir (inserir ou atualizar) |
| Objetos personalizados | Substituir (inserir ou atualizar) |
| Empresas | Sincronizar (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Membros do programa | Sincronizar (substituir status), Excluir (remover do programa) |

## Cabeçalhos

A assimilação de dados usa os seguintes cabeçalhos HTTP personalizados.

### Solicitação

| Chave | Valor | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `X-Correlation-Id` | Sequência de caracteres arbitrária (comprimento máximo de 255 caracteres). | Não | Pode ser usado para rastrear solicitações pelo sistema. Consulte Fluxo de dados de observação do Marketo |
| `X-Request-Source` | Sequência de caracteres arbitrária (comprimento máximo de 50 caracteres). | Não | Pode ser usado para rastrear a origem de solicitações por meio do sistema. Consulte Fluxo de dados de observação do Marketo |

### Resposta

| Chave | Valor | Obrigatório |
| --- | --- | --- |
| `X-Request-Id` | Identificador exclusivo da solicitação. | Sim |

## Solicitações

Use o método POST do HTTP para enviar dados ao servidor.

A representação de dados é incluída no corpo da solicitação como application/json.

O nome do domínio é: `mkto-ingestion-api.adobe.io`

O caminho começa com `/subscriptions/MunchkinId`, onde MunchkinId é específico para sua instância do Marketo. Você pode encontrar sua Munchkin ID na interface do Marketo Engage em **Admin** > **Minha conta** > **Informações de suporte**.  O restante do caminho é usado para especificar o recurso de interesse.

Exemplo de URL para Pessoas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Exemplo de URL para objetos personalizados:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

Exemplo de URL para Empresas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

Exemplo de URL para Membros do Programa:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

### Respostas

Todas as respostas retornam uma ID de solicitação exclusiva por meio do cabeçalho `X-Request-Id`.

Exemplo de ID de solicitação via cabeçalho:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Sucesso

Quando uma chamada é bem-sucedida, um status 202 é retornado.  Nenhum corpo de resposta é retornado.

Exemplo de resposta bem-sucedida:

```http
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Erro

Quando uma chamada produz um erro, um status diferente de 202 é retornado junto com um corpo de resposta com detalhes adicionais sobre o erro. O corpo da resposta é `application/json` e contém um único objeto com os membros `error_code` e `message`.

Abaixo estão os códigos de erro reutilizados do Adobe Developer Gateway.

| Código de status HTTP | error_code | mensagem |
| --- | --- | --- |
| 401 | 401013 | O token de Oauth é inválido |
| 403 | 403010 | Token de Oauth ausente |
| 404 | 404040 | Recurso não encontrado |
| 429 | 429001 | Limite de uso do serviço atingido |

Abaixo estão códigos de erro exclusivos à API de assimilação de dados e compostos de três segmentos.  Os primeiros três dígitos são o status (retornado pelo Adobe Developer Gateway), seguido por um zero &quot;0&quot;, seguido por três dígitos.

| Código de status HTTP | error_code | mensagem |
| --- | --- | --- |
| 400 | 4000801 | Solicitação inválida |
| 400 | 4000802 | Dados inválidos |
| 403 | 4030801 | Não autorizado |
| 429 | 4290801 | Cota diária atingida |
| 500 | 5000801 | Erro interno do servidor |

## Tentativas

Quando um erro transitório é detectado, o serviço repete a operação. As tentativas ocorrem por vários motivos, principalmente quando um serviço dependente atinge o tempo limite ou não está disponível temporariamente.

Intervalos de repetição:

* Operação inicial e primeira tentativa : 5 minutos
* 1º e 2º : 15 min
* 2º e 3º : 20 min
* 3º e 4º : 20 min
* 4º e 5º : 2 horas
* após a 5ª tentativa -> 3 horas

## Pontos de acesso

Os endpoints de assimilação estão disponíveis para Pessoas, Objetos personalizados, Empresas e Membros do programa.

### Pessoas

Ponto de extremidade usado para substituir registros de pessoa.

| Método | Caminho |
| --- | --- |
| POST | /subscriptions/{munchkinId}/pessoas |

#### Cabeçalhos

| Chave | Valor |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Corpo da solicitação

| Chave | Tipo de dados | Obrigatório | Valor | Valor padrão |
| --- | --- | --- | --- | --- |
| `priority` | String | Não | Prioridade da solicitação: normal ou alta | normal |
| `partitionName` | String | Não | Nome da partição da pessoa | Padrão |
| `dedupeFields` | Objeto | Não | Atributos para desduplicar em. Um ou dois nomes de atributo são permitidos. <br/> Dois atributos são usados em uma operação AND. Por exemplo, se `email` e `firstName` forem especificados, ambos serão usados para procurar uma pessoa usando a operação AND. <br/>Os atributos com suporte são: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, Atributos personalizados (somente tipo &quot;string&quot; e &quot;integer&quot;), `email` |  |
| `persons` | Matriz de objeto | Sim | Lista de pares de nome-valor do atributo para a pessoa | - |

As permissões necessárias são `Read-Write Lead`.

### Exemplo de pessoas

#### Solicitação

`POST /subscriptions/{munchkinId}/persons`

#### Cabeçalhos

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "priority": "high",
   "partitionName": "EMEA",
   "dedupeFields": {
      "field1": "email",
      "field2": "firstName"
   },
   "persons":[
      {
         "email": "brooklyn.parker@karnv.com",
         "firstName": "Brooklyn",
         "lastName": "Parker",
         "company": "Karnv"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal",
         "company": "Acme Inc"
      }
   ]
}
```

#### Resposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Objetos personalizados

Ponto de extremidade usado para substituir registros de objeto personalizados.

| Método | Caminho |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Cabeçalhos

| Chave | Valor |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Corpo da solicitação

| Chave | Tipo de dados | Obrigatório | Valor | Valor padrão |
| --- | --- | --- | --- | --- |
| `priority` | String | Não | Prioridade da solicitação: normal, alta | normal |
| `dedupeBy` | String | Não | Atributos a serem desduplicados em: dedupeFields, marketoGUID | dedupeFields |
| `customObjects` | Matriz de objeto | Sim | Lista de pares nome-valor do atributo do objeto. | - |

As permissões necessárias são `Read-Write Custom Object`.

Se um campo de link para uma Pessoa for especificado na solicitação e essa Pessoa não existir, várias tentativas ocorrerão. Se essa pessoa for adicionada durante a janela de nova tentativa (65 minutos), a atualização será bem-sucedida. Por exemplo, se o campo de link for `email` em Pessoa e a Pessoa não existir, ocorrerão novas tentativas.

### Exemplo de Objetos personalizados

#### Solicitação

`POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}`

#### Cabeçalhos

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "dedupeBy": "dedupeFields",
   "priority": "high",
   "customObjects": [
      {
         "email": "brooklyn.parker@karnv.com",
         "vin": "20UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 330i",
         "year": 2003
      },
      {
         "email": "johnny.neal@yvu30.com",
         "vin": "19UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 325i",
         "year": 1989
      }
   ]
}
```

#### Resposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Empresas

Ponto de extremidade usado para sincronizar registros da empresa. Oferece suporte a operações de criação, atualização e substituição com desduplicação por ID de empresa externa ou ID interna da Marketo.

| Método | Caminho |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/companies` |

#### Cabeçalhos

| Chave | Valor | Obrigatório |
| --- | --- | --- |
| `Content-Type` | application/json | Sim |
| `X-Mkto-User-Token` | {accessToken} | Sim |
| `X-Correlation-Id` | Sequência de caracteres arbitrária (comprimento máximo de 255 caracteres) | Não |
| `X-Request-Source` | Sequência de caracteres arbitrária (comprimento máximo de 50 caracteres) | Não |

#### Corpo da solicitação

| Chave | Tipo de dados | Obrigatório | Valor | Valor padrão |
| --- | --- | --- | --- | --- |
| `action` | String | Não | Sincronizar ação: `createOnly`, `updateOnly` ou `createOrUpdate` | `createOrUpdate` |
| `dedupeBy` | String | Não | Campo para desduplicar em: `dedupeFields` ou `idField` (não diferencia maiúsculas de minúsculas). Para `createOnly` e `createOrUpdate`, somente `dedupeFields` é permitido. Para `updateOnly`, ambos são permitidos. | `dedupeFields` |
| `input` | Matriz de objeto | Sim | Lista de pares nome-valor do atributo da empresa. Aceita a chave JSON `input` ou `companies`. | - |

Cada objeto de empresa na matriz `input` oferece suporte aos seguintes campos:

| Chave | Tipo de dados | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `externalCompanyId` | String | Condicional | Identificador de empresa externa. Necessário quando `dedupeBy` é `dedupeFields`. Não permitido quando `dedupeBy` é `idField`. |
| `id` | Longo | Condicional | ID da empresa interna da Marketo. Necessário quando `dedupeBy` é `idField` e `action` é `updateOnly`. Não permitido quando `dedupeBy` é `dedupeFields`. |
| `company` | String | Não | Nome da empresa. |
| (qualquer campo) | Qualquer | Não | Outros campos padrão ou personalizados da empresa conforme definidos por [Descrever Empresas](companies.md). Os nomes de campos não diferenciam maiúsculas de minúsculas. |

As permissões necessárias são `Read-Write Company`.

### Exemplo de empresas

#### Solicitação

`POST /subscriptions/{munchkinId}/companies`

#### Cabeçalhos

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalCompanyId": "ext-company-001",
         "company": "Acme Corporation",
         "industry": "Technology",
         "numberOfEmployees": 5000,
         "annualRevenue": 100000000
      },
      {
         "externalCompanyId": "ext-company-002",
         "company": "Globex Industries",
         "industry": "Manufacturing",
         "numberOfEmployees": 1200
      }
   ]
}
```

#### Resposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Exemplo de atualização por ID de empresas

```json
{
   "action": "updateOnly",
   "dedupeBy": "idField",
   "input": [
      {
         "id": 12345,
         "company": "Acme Corporation (Renamed)",
         "numberOfEmployees": 5500
      }
   ]
}
```

### Regras de validação de empresas

| Regra | Detalhe |
| --- | --- |
| ação | Deve ser um de: `createOnly`, `updateOnly`, `createOrUpdate`. Diferenciação de maiúsculas e minúsculas. |
| dedupeBy | Deve ser `dedupeFields` ou `idField` (não diferencia maiúsculas de minúsculas). O padrão é `dedupeFields`. |
| ação dedupeBy + | `createOnly` e `createOrUpdate` permitem somente `dedupeFields`. `updateOnly` permite `dedupeFields` e `idField`. |
| Quando `dedupeBy=dedupeFields` | Cada empresa deve ter `externalCompanyId`. O campo `id` não deve estar presente. |
| Quando `dedupeBy=idField` | Cada empresa deve ter `id`. O campo `externalCompanyId` não deve estar presente. |
| `input` / `companies` | Não pode ser nulo ou vazio. |
| Máximo de objetos por solicitação | 1,000 |

### Membros do Programa (Sincronizar)

Ponto de extremidade usado para sincronizar o status do membro do programa, adicionar leads aos programas ou atualizar o status do programa.

| Método | Caminho |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers` |

#### Cabeçalhos

| Chave | Valor | Obrigatório |
| --- | --- | --- |
| Tipo de conteúdo | application/json | Sim |
| X-Mkto-User-Token | {accessToken} | Sim |
| X-Correlation-Id | Sequência de caracteres arbitrária (comprimento máximo de 255 caracteres) | Não |
| X-Request-Source | Sequência de caracteres arbitrária (comprimento máximo de 50 caracteres) | Não |

#### Corpo da solicitação

| Chave | Tipo de dados | Obrigatório | Valor | Valor padrão |
| --- | --- | --- | --- | --- |
| Programas | Matriz de objeto | Sim | Lista de operações do programa. Cada especifica um programa, um status de público-alvo e os leads para a sincronização. | - |

Cada objeto na matriz `programs` contém:

| Chave | Tipo de dados | Obrigatório | Descrição |
| --- | --- | --- | --- |
| programId | Longo | Sim | A ID do programa Marketo. Deve ser um número inteiro positivo. |
| status | String | Sim | O status do membro do programa a ser definido, por exemplo `"Member"` ou `"Influenced"`. Aceita a chave JSON `statusName` ou `status`. O valor não deve ser `"Not in Program"`; use o ponto de extremidade de exclusão. |
| membros | Matriz de objeto | Sim | Lista de referências de clientes potenciais a serem adicionadas ou atualizadas no programa. Aceita a chave JSON `input` ou `members`. |

Cada objeto na matriz `members` contém:

| Chave | Tipo de dados | Obrigatório | Descrição |
| --- | --- | --- | --- |
| leadId | Longo | Sim | A ID do lead Marketo. |
| (qualquer campo) | Qualquer | Não | Campos adicionais de membros personalizados do programa. Os nomes de campos não diferenciam maiúsculas de minúsculas. |

As permissões necessárias são `Read-Write Lead`.

### Exemplo de sincronização de Membros do Programa

#### Solicitação

`POST /subscriptions/{munchkinId}/programmembers`

#### Cabeçalhos

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "programs": [
      {
         "programId": 1001,
         "status": "Member",
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "status": "Influenced",
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Resposta

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Regras de validação de sincronização de Membros do Programa

| Regra | Detalhe |
| --- | --- |
| Programas | Não pode ser nulo ou vazio. |
| programId | Obrigatório. Deve ser um número inteiro positivo. |
| status | Obrigatório. Não pode ficar em branco. Não deve ser `"Not in Program"` (não diferencia maiúsculas de minúsculas). Em vez disso, use o terminal delete. |
| membros | Não pode ser nulo ou vazio. |
| leadId | Obrigatório para cada membro na matriz de entrada. |
| Máximo de clientes em potencial por solicitação | 1.000 membros totais em todos os programas. |

### Membros do programa (excluir)

Ponto de extremidade usado para remover clientes em potencial de programas. Isso define o status de associação do cliente potencial como `"Not in Program"` e remove o membro desse programa.

>[!NOTE]
>
>Esse endpoint usa POST em vez de DELETE porque a solicitação requer um corpo JSON com dados estruturados.

| Método | Caminho |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers/delete` |

#### Cabeçalhos

| Chave | Valor | Obrigatório |
| --- | --- | --- |
| Tipo de conteúdo | application/json | Sim |
| X-Mkto-User-Token | {accessToken} | Sim |
| X-Correlation-Id | Sequência de caracteres arbitrária (comprimento máximo de 255 caracteres) | Não |
| X-Request-Source | Sequência de caracteres arbitrária (comprimento máximo de 50 caracteres) | Não |

#### Corpo da solicitação

| Chave | Tipo de dados | Obrigatório | Valor | Valor padrão |
| --- | --- | --- | --- | --- |
| Programas | Matriz de objeto | Sim | Lista de operações de exclusão de programas. Cada especifica um programa e os clientes em potencial a serem removidos. | - |

Cada objeto na matriz `programs` contém:

| Chave | Tipo de dados | Obrigatório | Descrição |
| --- | --- | --- | --- |
| programId | Longo | Sim | A ID do programa Marketo. Deve ser um número inteiro positivo. |
| membros | Matriz de objeto | Sim | Lista de referências de clientes potenciais a serem removidas do programa. Aceita a chave JSON `input` ou `members`. |

Cada objeto na matriz `members` contém:

| Chave | Tipo de dados | Obrigatório | Descrição |
| --- | --- | --- | --- |
| leadId | Longo | Sim | A ID do lead Marketo. |

As permissões necessárias são `Read-Write Lead`.

### Exemplo de exclusão de membros do programa

#### Solicitação

`POST /subscriptions/{munchkinId}/programmembers/delete`

#### Cabeçalhos

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "programs": [
      {
         "programId": 1001,
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Resposta

`HTTP/1.1 202`
`X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### Membros do Programa excluem regras de validação

| Regra | Detalhe |
| --- | --- |
| Programas | Não pode ser nulo ou vazio. |
| programId | Obrigatório. Deve ser um número inteiro positivo. |
| membros | Não pode ser nulo ou vazio. |
| leadId | Obrigatório para cada membro na matriz de entrada. |
| Máximo de clientes em potencial por solicitação | 1.000 membros totais em todos os programas. |

## Limites

Esta é uma lista atualizada de medidas de proteção:

* Tamanho máximo da solicitação: 1 MB
* Máximo de objetos por solicitação por tipo de objeto: 1.000
* Máximo de solicitações por segundo por ID de cliente: 5.000
* Máximo de objetos por dia: 10.000.000

Esses limites se aplicam uniformemente a Pessoas, Objetos Personalizados, Empresas e Membros do Programa. Para membros do programa, &quot;objetos por solicitação&quot; é o número total de referências de clientes potenciais em todos os programas em uma única solicitação.

## API de assimilação de dados versus API REST

Esta é uma lista de diferenças entre a API de assimilação de dados e outras APIs REST do Marketo:

* Para autenticar, você deve passar o token de acesso usando o cabeçalho `X-Mkto-User-Token`
* O nome de domínio da URL é `mkto-ingestion-api.adobe.io`
* O caminho da URL começa com `/subscriptions/MunchkinId`
* Não há parâmetros de consulta
* Se a chamada for bem-sucedida, um status 202 será retornado e o corpo da resposta ficará vazio
* Se uma chamada falhar, um status diferente de 202 será retornado e o corpo da resposta conterá `{ "error_code" : "Error Code", "message" : "Message" }`
* A ID da solicitação é retornada pelo cabeçalho `X-Request-Id`
