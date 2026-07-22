---
title: URL básica
feature: REST API
description: Saiba como criar solicitações de API REST do Marketo, entender o recurso e os parâmetros de caminho do URL de base e encontrar seu URL de base exclusivo.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
TQID: https://experienceleague.adobe.com/NZisV6V-FMPi0RHpdaFrc1kZc3nb15YomwRgohaQmEE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 146
ht-degree: 3%

---

# URL básica

Cada chamada de API na [Referência de Ponto de Extremidade](endpoint-reference.md) especifica o método REST, o caminho, o recurso e os parâmetros. Anexe esses componentes ao URL base para formar uma solicitação.

Este é um exemplo de um URL REST bem formado:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

O exemplo contém os seguintes componentes:

- **URL Base:** `https://284-RPR-133.mktorest.com/rest`
- **Caminho:** `/v1/lead/`
- **Recurso:** `318582.json`
- **Parâmetro de consulta:** `fields=email,firstName,lastName`

O URL base contém a ID da conta, também conhecida como Munchkin ID, e é exclusiva de cada assinatura do Marketo.

Para localizar a URL base, faça logon no Marketo e vá para **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]**. O URL base é rotulado como &quot;Endpoint:&quot; na seção &quot;REST API&quot;, conforme mostrado na imagem a seguir.

![Ponto de Extremidade da URL Base dos Serviços Web](assets/rest-api-base-url-web-services.png)

Copie o URL de base e inclua-o no URL para cada chamada à API REST.
