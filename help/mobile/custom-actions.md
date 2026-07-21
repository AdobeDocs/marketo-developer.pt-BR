---
title: Ações Personalizadas
feature: Mobile Marketing
description: Saiba como enviar e relatar ações personalizadas com o Marketo Mobile SDK para iOS e Android, colocar em fila offline, acionar Campanhas inteligentes e atender aos 20 caracteres...
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
TQID: https://experienceleague.adobe.com/yZKzdm-dH0cYPGGKE-Z-4KcbhGIwyFl0Z9vEqcv1QXI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 259
ht-degree: 2%

---

# Ações Personalizadas

As ações personalizadas rastreiam as interações do usuário no aplicativo móvel. Quando o aplicativo chama o Marketo SDK para enviar uma ação personalizada, o SDK salva primeiro a ação no dispositivo. O SDK envia a ação depois de detectar a conectividade adequada com a Internet, para que o Marketo possa receber a ação após um atraso.

Ações personalizadas podem ser usadas como acionadores e filtros em Campanhas inteligentes. Para obter mais informações, consulte [Atividade no aplicativo móvel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envio de ações personalizadas no iOS

Enviar uma ação personalizada.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```swift
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Enviar uma ação personalizada com metadados.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```swift
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Relatar todas as ações salvas imediatamente.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
[sharedInstance reportAll];
```

>[!TAB Swift]

```swift
sharedInstance.reportAll();
```

>[!ENDTABS]

## Envio de ações personalizadas no Android

1. Enviar uma ação personalizada.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Enviar uma ação personalizada com metadados.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Relate todas as ações personalizadas salvas imediatamente.

   ```
   Marketo.reportAll();
   ```

## Solução de problemas de ações personalizadas

Os nomes de ação personalizados enviados do Mobile SDK para o Marketo devem ter menos de 20 caracteres.

**Casos de uso de vários usuários em um dispositivo compartilhado:** Quando um usuário faz logon em um aplicativo móvel que usa o Marketo SDK, a primeira chamada associa o cliente potencial à instalação do aplicativo. Depois que a chamada é bem-sucedida, as atividades subsequentes do usuário aparecem no log de atividades do lead.

A chamada de associação é assíncrona. As ações personalizadas registradas imediatamente após o logon podem ser associadas ao usuário conectado anteriormente até que a chamada seja bem-sucedida.
