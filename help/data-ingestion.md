---
title: "Assimilação de dados"
description: "Visão geral da API de assimilação de dados"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 10%

---


# Assimilação de dados

A API de assimilação de dados é um serviço de alto volume, baixa latência e alta disponibilidade, projetado para lidar com a assimilação de grandes quantidades de dados pessoais e relacionados a pessoas de maneira eficiente e com atrasos mínimos. 

Os dados são assimilados enviando solicitações que são executadas de forma assíncrona. O status da solicitação pode ser recuperado inscrevendo-se em eventos do [Fluxo de dados de observação do Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup/)&#x200B;

As interfaces são oferecidas para dois tipos de objeto: Pessoas, Objetos personalizados. A operação de registro é somente &quot;inserir ou atualizar&quot;.

A API de assimilação de dados está em beta privado. Os convidados devem ter direito a [Pacote de camada de desempenho do Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticação

A API de assimilação de dados usa o mesmo método de autenticação OAuth 2.0 que a API REST do Marketo para gerar um token de acesso, mas o token de acesso deve ser passado pelo cabeçalho HTTP `X-Mkto-User-Token`. Não é possível passar o token de acesso por meio de um parâmetro de consulta.

Exemplo de token de acesso pelo cabeçalho:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Permissões

A assimilação de dados usa o mesmo modelo de permissões que a API REST do Marketo e não requer permissões especiais adicionais para uso, embora permissões específicas sejam necessárias para cada endpoint.

| Terminal | Permissão |
|---|---|
| Pessoas | Lead de leitura-gravação |
| Objetos personalizados | Objeto personalizado de leitura-gravação |

## Cabeçalhos

A assimilação de dados usa os seguintes cabeçalhos HTTP personalizados.

### Solicitar

| Chave | Valor | Obrigatório | Descrição |
|---|---|---|---|
| X-Correlation-Id | Sequência de caracteres arbitrária (comprimento máximo de 255 caracteres). | Não | Pode ser usado para rastrear solicitações pelo sistema. Consulte Fluxo de dados de observação do Marketo |
| X-Request-Source | Sequência de caracteres arbitrária (comprimento máximo de 50 caracteres). | Não | Pode ser usado para rastrear a origem de solicitações por meio do sistema. Consulte Fluxo de dados de observação do Marketo |

### Resposta

| Chave | Valor | Obrigatório | Descrição |
|---|---|---|---|
| X-Request-Id | Identificador exclusivo da solicitação. | Sim | |

## Solicitações

Use o método POST do HTTP para enviar dados ao servidor.

A representação de dados é incluída no corpo da solicitação como application/json.

O nome do domínio é: `mkto-ingestion-api.adobe.io`

O caminho começa com `/subscriptions/_MunchkinId_` onde `_MunchkinId_` é específico para a sua instância do Marketo. Você pode encontrar sua ID do Munchkin na interface do usuário do Marketo Engage em **Admin** >**Minha conta** > **Informações de suporte**. O restante do caminho é usado para especificar o recurso de interesse.

Exemplo de URL para Pessoas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Exemplo de URL para objetos personalizados:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

## Respostas

Todas as respostas retornam uma ID de solicitação exclusiva por meio do `X-Request-Id` cabeçalho.

Exemplo de ID de solicitação via cabeçalho:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Sucesso

Quando uma chamada é bem-sucedida, um status 202 é retornado. Nenhum corpo de resposta é retornado.

Exemplo de resposta bem-sucedida:

`HTTP/1.1 202 Accepted` `X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598` `Content-Length: 0` `Date: Wed, 18 Oct 2023 18:56:49 GMT`

### Erro

Quando uma chamada produz um erro, um status diferente de 202 é retornado junto com um corpo de resposta com detalhes adicionais sobre o erro. O corpo da resposta é application/json e contém um único objeto com membros `error_code` e `message`.

Abaixo estão os códigos de erro reutilizados do Adobe Developer Gateway.

| Código de status HTTP | error_code | mensagem |
|--- |--- |--- |
| 401 | 401013 | O token de Oauth é inválido |
| 403 | 403010 | Token de Oauth ausente |
| 404 | 404040 | Recurso não encontrado |
| 429 | 429001 | Limite de uso do serviço atingido |

Abaixo estão códigos de erro exclusivos à API de assimilação de dados e compostos de três segmentos. Os primeiros três dígitos são o status (retornado pelo Gateway de E/S Adobe), seguido por um zero &quot;0&quot;, seguido por três dígitos.

| Código de status HTTP | error_code | mensagem |
|--- |--- |--- |
| 400 | 4000801 | Solicitação inválida |
| 400 | 4000802 | Dados inválidos |
| 403 | 4030801 | Não autorizado |
| 429 | 4290801 | Cota diária atingida |
| 500 | 5000801 | Erro interno do servidor |

Exemplo de resposta de erro:

`HTTP/1.1 403 Forbidden` `Content-Type: application/json` `Content-Length: 54` `Date: Wed, 18 Oct 2023 19:10:07 GMT` `X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw` `{"error_code":"403010","message":"Oauth token is missing"}`

## Tentativas

Quando um erro transitório é detectado, o serviço repete a operação três vezes. A primeira tentativa ocorre após um período de espera de 5 minutos, a segunda após mais 30 minutos e, finalmente, a terceira após mais 30 minutos. As tentativas ocorrem por vários motivos, principalmente quando um serviço dependente atinge o tempo limite ou não está disponível temporariamente.

## Endpoints

Os endpoints de assimilação estão disponíveis para Pessoas e Objetos personalizados.

### Pessoas

Ponto de extremidade usado para substituir registros de pessoa.

| Método |
|---|
| POST |

| Caminho |
|---|
| /subscriptions/{munchkinId}/pessoas |

| HeadersKey | Valor |
|---|---|
| Tipo de conteúdo | application/json |
| X-Mkto-User-Token | {accessToken} |

Corpo da solicitação

| Chave | Tipo de dados | Obrigatório | Valor | Valor padrão |
|---|---|---|---|---|
| prioridade | Sequência de caracteres | Não | Prioridade da solicitação:normalhigh | normal |
| partitionName | Sequência de caracteres | Não | Nome da partição da pessoa | Padrão |
| dedupeFields | Objeto | Não | Atributos para desduplicar em. Um ou dois nomes de atributo são permitidos. Dois atributos são usados em uma operação AND. Por exemplo, se ambos `email` e `firstName` forem especificadas, ambas serão usadas para procurar uma pessoa usando a operação AND. Os atributos compatíveis são:`idemail`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId`, `sfdcLeadOwnerIdCustom` atributos (somente tipo &quot;string&quot; e &quot;integer&quot;) | email |
| pessoas | Matriz de objeto | Sim | Lista de pares de nome-valor do atributo para a pessoa | - |

| Permissão |
|---|
| Lead de leitura-gravação |

#### Exemplo de pessoas

```
POST /subscriptions/{munchkinId}/persons
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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
         "lastName": "Parker"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal"
      }
   ]
}
```

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

### Objetos personalizados

Ponto de extremidade usado para substituir registros de objeto personalizados.

| Método |
|---|
| POST |

| Caminho |
|---|
| /subscriptions/{munchkinId}/customobjects/{customObjectAPIName} |

Cabeçalhos

| Chave | Valor |
|---|---|
| Tipo de conteúdo | application/json |
| X-Mkto-User-Token | {accessToken} |

Corpo da solicitação | Chave | Tipo de dados | Obrigatório | Valor | Valor padrão | |—|—|—|—|—| | prioridade | String | Não | Prioridade da solicitação:normalhigh | normal | | dedupeBy | String | Não | Atributos a serem desduplicados em:dedupeFieldsmarketoGUID | dedupeFields | | customObjects | Matriz de objeto | Sim | Lista de pares nome-valor do atributo do objeto. | - |

| Permissão |
|---|
| Objeto personalizado de leitura-gravação |

#### Pessoa Não Presente

Se um campo de link para uma Pessoa for especificado na solicitação e essa Pessoa não existir, várias tentativas ocorrerão. Se essa pessoa for adicionada durante a janela de nova tentativa (65 minutos), a atualização será bem-sucedida. Por exemplo, se o campo de link for `email` em Pessoa, e a Pessoa não existir, ocorrerão novas tentativas.

#### Exemplo de objetos personalizados

```
POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

## Limites

Esta é uma lista de uso de medidas de proteção:

- Tamanho máximo da solicitação: 1 MB
- Máximo de objetos por solicitação por tipo de objeto: 1.000
- Máximo de solicitações por segundo por ID de cliente: 5.000
- Máximo de objetos por dia: 10.000.000

## API de assimilação de dados versus API REST

Esta é uma lista de diferenças entre a API de assimilação de dados e outras APIs REST do Marketo:

- Esta não é uma interface CRUD completa, ela suporta somente &quot;upsert&quot;
- Para autenticar, você deve passar o token de acesso usando o `X-Mkto-User-Token` cabeçalho
- O nome de domínio do URL é `mkto-ingestion-api.adobe.io`
- O caminho do URL começa com `/subscriptions/_MunchkinId_`
- Não há parâmetros de consulta
- Se a chamada for bem-sucedida, um status 202 será retornado e o corpo da resposta ficará vazio
- Se a chamada falhar, um status diferente de 202 será retornado e o corpo da resposta conterá `{ "error_code" : "_Error Code_", "message" : "_Message_" }`
- A ID da solicitação é retornada via `X-Request-Id` cabeçalho
