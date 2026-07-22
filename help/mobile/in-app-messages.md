---
title: Mensagens no aplicativo
feature: Mobile Marketing
description: Configure mensagens no aplicativo do Marketo com o Mobile SDK, configure acionadores de evento personalizados, rastreie a atividade de toque e corrija os problemas de primeira inicialização de abertura do aplicativo.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
TQID: https://experienceleague.adobe.com/RVkEUBaFb-PHd0gE9ngzYc5zOojINwSI7ic2TmcU7-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 321
ht-degree: 2%

---

# Mensagens no aplicativo

Conclua estas etapas para usar as mensagens no aplicativo do Marketo:

1. Instale o Marketo Mobile SDK conforme descrito em [Instalação móvel](installation.md).
1. Adicione seu aplicativo móvel ao Marketo conforme descrito em [Adicionar um aplicativo móvel](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Opcional: adicione código ao aplicativo móvel para capturar [Ações personalizadas](custom-actions.md).

Depois de instalar o Marketo Mobile SDK e adicionar seu aplicativo ao Marketo, você pode enviar mensagens no aplicativo que são exibidas quando um usuário abre seu aplicativo.

Por padrão, as mensagens no aplicativo são acionadas quando o aplicativo é aberto. Para acionar uma mensagem para outro evento, como visualizar uma página específica ou selecionar um botão específico, adicione uma ação personalizada ao código. Consulte [Ações personalizadas](custom-actions.md) para obter exemplos de código.

## Solução de problemas

**A mensagem no aplicativo não está aparecendo**

O Marketo responde aos acionadores do aplicativo somente após o Marketo Mobile SDK ser inicializado com a plataforma Marketo. A inicialização ocorre quando você instala e abre o aplicativo pela primeira vez.

Como a inicialização ocorre após a primeira abertura do aplicativo, o evento &quot;Abertura de aplicativo&quot; não é acionado até que você abra o aplicativo uma segunda vez. Feche e reabra o aplicativo. Uma mensagem acionada pela Abertura do aplicativo deve ser exibida em seu dispositivo.

Os eventos personalizados são acionados pela interação do usuário após a abertura do aplicativo. Os eventos personalizados são reconhecidos pela Marketo durante a primeira sessão.

**Rastreamento De Atividade De Toque No Aplicativo**

Para rastrear atividades de toque e basear a frequência de exibição no número de toques, atribua uma ação diferente de &quot;descartar&quot; a um botão principal ou secundário.

Para obter mais informações, consulte [Mensagens no aplicativo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) na documentação do produto.
