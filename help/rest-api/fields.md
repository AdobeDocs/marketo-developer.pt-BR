---
title: "Campos"
feature: REST API, Field Management
description: "Uma lista de nomes de campo suportados."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 6%

---


# Campos

A API REST e a API SOAP usam diferentes convenções de nomenclatura para campos de lead.

## Recuperar a lista de nomes de campos

Recupere a lista de todos os nomes de campo com suporte disponíveis em seus registros de cliente potencial usando o ponto de extremidade REST &quot;Descrever cliente potencial&quot;.

## Onde usar qual tipo de nome de campo?

Às vezes, é difícil saber qual tipo de nome de campo você deve usar ao aproveitar um recurso específico relacionado à integração. Veja a seguir uma referência rápida para quais recursos usam tipos de nome de campo REST ou SOAP.

| Recurso | Tipo de nome de campo a ser usado |
|--- |--- |
| API de rastreamento de lead (Munchkin) | SOAP |
| API do Forms 2.0 | SOAP |
| Importação de lista (UI) | SOAP |
| Importação de lista (REST API) | REST |
| Mapeamentos de resposta do Webhook | SOAP |
| Script de email (Velocity) | SOAP |
| API SOAP | SOAP |
| API REST | REST |

### Por que o campo REST API sfdcId sempre retorna um valor nulo?

O campo `sfdcId` é um campo de fórmula que foi incluído incorretamente no mapa de campo original da API REST. Os registros recuperados por meio da API REST não calculam o valor de campos de fórmula, portanto, o valor sempre será nulo. Para capturar a ID real do SFDC, você deve usar os campos chamados `sfdcLeadId` e `sfdcContactId`.
