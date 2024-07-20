---
title: URL base
feature: REST API
description: Descreve como inserir URLs para o Marketo.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# URL base

A documentação de [Referência de Ponto de Extremidade](endpoint-reference.md) para cada chamada de API mostra o método REST, o caminho, o recurso e os parâmetros que devem ser anexados à URL base para formar uma solicitação.

Este é um exemplo de um URL REST bem formado:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

que é composto pelas seguintes partes:

- URL Base: `https://284-RPR-133.mktorest.com/rest`
- Caminho: `/v1/lead/`
- Recurso: `318582.json`
- Parâmetro de consulta: `fields=email,firstName,lastName`

O URL base contém a ID da conta (também conhecida como ID do Munchkin) e, portanto, é exclusivo para cada assinatura do Marketo. Sua URL base é encontrada efetuando logon no Marketo e navegando até o menu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços Web]**. Ela é rotulada como &quot;Endpoint:&quot; na seção &quot;REST API&quot;, como mostrado nas capturas de tela a seguir.

![Ponto de Extremidade da URL Base dos Serviços Web](assets/rest-api-base-url-web-services.png)

Depois de encontrar o URL base, copie-o e inclua-o nos URLs que você usa ao chamar qualquer uma das APIs REST.
