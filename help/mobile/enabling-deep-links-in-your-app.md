---
title: Ativação de deep links
feature: Mobile Marketing
description: Saiba como habilitar deep links em seu aplicativo para mensagens de push do Marketo usando esquemas de URI personalizados, com orientação e práticas recomendadas para iOS, Android e PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
TQID: https://experienceleague.adobe.com/UswOvHXGlfTrTUqr4Gsf3j2Z7Xpv2FF2luXeygT4qE0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 1%

---

# Ativação de deep links

Os deep links direcionam as pessoas para um conteúdo específico no seu aplicativo. Por exemplo, quando uma pessoa seleciona uma mensagem por push móvel que anuncia uma camiseta roxa, o aplicativo pode abrir o conteúdo de camiseta roxa em vez da página inicial.

O processo funciona assim:

1. Um usuário do Marketo coloca um URI personalizado na Ação de toque para uma mensagem de push.
1. Quando uma pessoa toca na mensagem de push em seu dispositivo, o Marketo MME SDK aciona um evento com o URI personalizado.
1. Seu aplicativo processa o evento e direciona a pessoa para o conteúdo correspondente.

Para habilitar este processo:

1. Defina uma estrutura URI personalizada para seu aplicativo.
1. Registre o esquema no manifesto do aplicativo.
1. Adicione o código que processa eventos de deep link e roteia pessoas para o conteúdo correspondente.

Para o iOS, consulte a documentação do Apple em [Definição de um esquema de URL personalizado para seu aplicativo](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Para o Android, consulte a documentação do Google em [Habilitar deep links para conteúdo do aplicativo](https://developer.android.com/training/app-links/deep-linking?hl=pt-br).

Para aplicativos PhoneGap, use um plug-in para permitir que seu aplicativo híbrido responda a esquemas de URL personalizados e a links universais/de aplicativos no iOS e no Android. Consulte os [plug-ins de deep link](https://cordova.apache.org/plugins/?q=deeplink) disponíveis.

Quando você tiver ativado o deep linking no seu aplicativo, compartilhe os URIs personalizados com os usuários do Marketo para que eles possam inseri-los na Ação de toque para mensagens de push.

O Marketo usa uma estrutura URI predefinida ao configurar dispositivos de teste. Para obter mais informações, consulte &quot;Dispositivos de Teste&quot; no [Guia de Instalação](installation.md).

## Práticas recomendadas para definir uma estrutura URI

Se sua marca tiver um site para dispositivos móveis, siga a estrutura do URL ao definir o URI do deep link. Por exemplo, se a URL do produto for `https://myappname.com/products/purple-shirt`, use `myappname://products/purple-shirt` como o URI do deep link correspondente.

Use um esquema exclusivo da sua marca. Embora nenhuma regulamentação exija que os esquemas sejam globalmente exclusivos, você pode ajudar a criar um esquema exclusivo revertendo o nome do domínio, como `org.companyname`.
