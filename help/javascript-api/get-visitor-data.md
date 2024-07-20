---
title: Obter dados do visitante
description: Obter dados do visitante
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 5%

---

# Obter dados do visitante

Esse método é usado para obter dados de identificação do visitante em tempo real.

- Você deve se tornar um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.
- O RTP não suporta listas de contas nomeadas de Marketing Baseado em Conta. As listas e os códigos ABM pertencem apenas às listas de contas carregadas (arquivos CSV) gerenciadas no RTP.

Se ocorrer um erro, haverá uma mensagem de erro como parte da resposta JSON. Se um código 500 for retornado, entre em contato com o suporte com a solicitação feita.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|---|---|---|---|
| `get` | Obrigatório | Sequência de caracteres | Ação do método. |
| `visitor` | Obrigatório | Sequência de caracteres | Nome do método. |
| `callback` | Obrigatório | Função | Função de retorno de chamada a ser acionada para cada campanha retornada. |

## Exemplos

Obter dados de identificação do visitante:

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Resposta com Correspondência de Segmentos:

Abaixo está um exemplo de resposta que é retornada caso o visitante tenha correspondido a segmentos em tempo real antes da chamada da API Obter dados do visitante.

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

Veja abaixo um exemplo de resposta que é retornada caso o visitante não corresponda a nenhum segmento em tempo real antes da chamada da API Obter dados do visitante.

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
