---
title: Campos
feature: REST API, Field Management
description: Saiba mais sobre a nomenclatura de campos do lead REST e do SOAP, campos de lista por meio de REST Descrever lead, mapeamento de recursos, por que o sfdcId é nulo e use o sfdcLeadId ou o sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
TQID: https://experienceleague.adobe.com/H2Bvhy-67U8JJ1V3JwYJ0O0vj4i11fwUCyYQtjxm8u0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 194
ht-degree: 6%

---

# Campos

A API REST e a API SOAP usam diferentes convenções de nomenclatura para campos de cliente potencial. Use a convenção de nome de campo exigida por cada recurso de integração.

## Recuperar a lista de nomes de campos

Use o ponto de extremidade REST &#39;Descrever cliente potencial&#39; para recuperar todos os nomes de campo aceitos para registros de cliente potencial.

## Onde usar qual tipo de nome de campo?

O tipo de nome de campo necessário depende do recurso de integração. A tabela a seguir identifica se cada recurso usa nomes de campo REST ou SOAP.

| Recurso | Tipo de nome de campo a ser usado |
| --- | --- |
| API de rastreamento de lead (Munchkin) | SOAP |
| API do Forms 2.0 | SOAP |
| Importação de lista (UI) | SOAP |
| Importação de lista (REST API) | REST |
| Mapeamentos de resposta do Webhook | SOAP |
| Script de email (Velocity) | SOAP |
| API SOAP | SOAP |
| API REST | REST |

### Por que o campo REST API sfdcId sempre retorna um valor nulo?

O campo `sfdcId` é um campo de fórmula incluído no mapa de campos original para a API REST. Os registros recuperados por meio da API REST não calculam valores de campo de fórmula, portanto, `sfdcId` sempre retorna nulo.

Para recuperar a SFDC ID, use os campos `sfdcLeadId` e `sfdcContactId`.
