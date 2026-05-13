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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 6%

---

# Campos

A API REST e a API SOAP usam diferentes convenções de nomenclatura para campos de cliente potencial.

## Recuperar a lista de nomes de campos

Recupere a lista de todos os nomes de campo com suporte disponíveis em seus registros de cliente potencial usando o ponto de extremidade REST &quot;Descrever cliente potencial&quot;.

## Onde usar qual tipo de nome de campo?

Às vezes, é difícil saber qual tipo de nome de campo você deve usar ao aproveitar um recurso específico relacionado à integração. Veja a seguir uma referência rápida para quais recursos usam os tipos de nome de campo REST ou SOAP.

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

O campo `sfdcId` é um campo de fórmula que foi incluído incorretamente no mapa de campos original para a API REST. Os registros recuperados por meio da API REST não calculam o valor de campos de fórmula, portanto, o valor sempre será nulo. Para capturar a ID real do SFDC, você deve usar os campos chamados `sfdcLeadId` e `sfdcContactId`.
