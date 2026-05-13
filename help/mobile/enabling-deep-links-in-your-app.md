---
title: Ativação de deep links
feature: Mobile Marketing
description: Saiba como habilitar deep links em seu aplicativo para mensagens de push do Marketo usando esquemas de URI personalizados, com orientação e práticas recomendadas para iOS, Android e PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
TQID: https://experienceleague.adobe.com/UswOvHXGlfTrTUqr4Gsf3j2Z7Xpv2FF2luXeygT4qE0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 420
ht-degree: 1%

---

# Ativação de deep links

Os deep links permitem redirecionar as pessoas para conteúdo (recursos) específico no aplicativo. Por exemplo, quando uma pessoa clica em uma mensagem por push para dispositivo móvel que anuncia uma camiseta roxa, você pode abrir o aplicativo diretamente para o conteúdo de camiseta roxa (em vez da página inicial).

O processo funciona assim:

1. O usuário do Marketo coloca um URI personalizado na Ação de toque para sua mensagem de push.
1. Quando uma pessoa toca na mensagem de push em seu dispositivo, o Marketo MME SDK aciona um evento com o URI personalizado.
1. Em seguida, o aplicativo processa o evento e redireciona a pessoa para o conteúdo apropriado no aplicativo.

Isso requer a definição de uma estrutura URI personalizada para o aplicativo, o registro do esquema no manifesto do aplicativo e a adição de código para processar eventos de deep link e direcionar para o local adequado no aplicativo.

Para o iOS, consulte a documentação do Apple em [Definindo um esquema de URL personalizado para seu aplicativo](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Para o Android, consulte a documentação do Google em [Habilitar deep links para conteúdo do aplicativo](https://developer.android.com/training/app-links/deep-linking?hl=pt-br).

Para aplicativos PhoneGap, o deep linking não é tão direto como com aplicativos nativos iOS ou Android, mas há plug-ins que permitem que seu aplicativo híbrido responda a esquemas de URL personalizados de deep link e links universais/de aplicativos no iOS e no Android. Considere [estes plug-ins](https://cordova.apache.org/plugins/?q=deeplink).

Quando você tiver ativado o deep linking no seu aplicativo, compartilhe os URIs personalizados com os usuários do Marketo para que eles possam inseri-los na Ação de toque para mensagens de push.

O Marketo usa uma estrutura URI predefinida ao configurar dispositivos de teste. Consulte a seção &quot;Dispositivos de Teste&quot; do [Guia de Instalação](installation.md) para obter mais informações.

## Práticas recomendadas para definir uma estrutura URI

Se sua marca tiver um site para dispositivos móveis existente, a prática recomendada é seguir também a estrutura de URL do URI do deep link. Por exemplo, se `https://myappname.com/products/purple-shirt` for o endereço do seu site para o produto em questão, `myappname://products/purple-shirt` seria uma boa estrutura de URI de deep link para ser usada no seu aplicativo.

Geralmente, seus esquemas devem ser exclusivos para sua marca. Embora atualmente não haja regulamentos para tornar os esquemas exclusivos em todo o mundo, uma maneira de ajudar a garantir que seus esquemas sejam exclusivos é reverter seu nome de domínio (por exemplo, `org.companyname`).
