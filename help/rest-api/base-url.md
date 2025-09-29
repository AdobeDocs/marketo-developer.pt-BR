---
title: URL básica
feature: REST API
description: Saiba como criar solicitações de API REST do Marketo, entender o recurso e os parâmetros de caminho do URL de base e encontrar seu URL de base exclusivo.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 2%

---

# URL básica

A documentação de [Referência de Ponto de Extremidade](endpoint-reference.md) para cada chamada de API mostra o método REST, o caminho, o recurso e os parâmetros que devem ser anexados à URL base para formar uma solicitação.

Este é um exemplo de um URL REST bem formado:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

que é composto pelas seguintes partes:

- URL Base: `https://284-RPR-133.mktorest.com/rest`
- Caminho: `/v1/lead/`
- Recurso: `318582.json`
- Parâmetro de consulta: `fields=email,firstName,lastName`

O URL de base contém a ID da conta (ou seja, Munchkin id) e, portanto, é exclusivo para cada assinatura do Marketo. Sua URL base é encontrada efetuando logon no Marketo e navegando até o menu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços Web]**. Ela é rotulada como &quot;Endpoint:&quot; na seção &quot;REST API&quot;, como mostrado nas capturas de tela a seguir.

![Ponto de Extremidade da URL Base dos Serviços Web](assets/rest-api-base-url-web-services.png)

Depois de encontrar o URL base, copie-o e inclua-o nos URLs que você usa ao chamar qualquer uma das APIs REST.
