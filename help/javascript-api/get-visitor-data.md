---
title: Obter dados do visitante
description: Obtenha identificação de visitantes em tempo real usando a API de contexto de usuário do RTP com parâmetros, exemplo de retorno de chamada e respostas de amostra para segmentos, ABM e localização.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
TQID: https://experienceleague.adobe.com/B-JMACtMs3aRVsb1eJKaRoQGgVKB6MTbd0KBoZ7Ay6k
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 214
ht-degree: 4%

---

# Obter dados do visitante

Use esse método para obter dados de identificação do visitante em tempo real.

- Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.
- O RTP não suporta listas de contas nomeadas de Marketing Baseado em Conta. As listas e os códigos ABM pertencem apenas às listas de contas carregadas (arquivos CSV) gerenciadas no RTP.

Se ocorrer um erro, o JSON de resposta incluirá uma mensagem de erro. Se a API retornar um código 500, entre em contato com o suporte e forneça a solicitação que causou o erro.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| `get` | Obrigatório | String | Ação do método. |
| `visitor` | Obrigatório | String | Nome do método. |
| `callback` | Obrigatório | Função | Função de retorno de chamada a ser acionada para cada campanha retornada. |

## Exemplos

O exemplo a seguir obtém os dados de identificação do visitante.

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Resposta com Correspondência de Segmentos:

A resposta a seguir inclui `matchedSegments` porque o visitante correspondeu a segmentos em tempo real antes da chamada da API Obter dados do visitante.

```json
{
    "status": 200,
    "results": {
        "matchedSegments": [
            {
                "name": "first click",
                "id": 177
            }
        ],
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```

Resposta sem correspondência de segmentos:

A resposta a seguir não inclui `matchedSegments` porque o visitante não correspondeu a nenhum segmento em tempo real antes da chamada da API Obter dados do visitante.

```json
{
    "status": 200,
    "results": {
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```
