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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 336
ht-degree: 1%

---

# Ações Personalizadas

É possível rastrear a interação do usuário enviando ações personalizadas. Quando o aplicativo móvel chama o Marketo SDK para enviar uma ação personalizada, a ação personalizada é salva inicialmente no dispositivo. O Marketo SDK verifica se há conectividade de Internet adequada antes de enviar a ação personalizada. Como resultado, pode haver um atraso entre o momento em que a ação personalizada é enviada e o momento em que é recebida pelo Marketo.

Ações personalizadas podem ser usadas como acionadores e filtros em Campanhas inteligentes. Para obter mais informações, consulte [Atividade no aplicativo móvel](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envio de ações personalizadas no iOS

Enviar ação personalizada.

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

Enviar ação personalizada com metadados.

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

Relatar todas as ações imediatamente (enviar todas as ações salvas).

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

1. Enviar ação personalizada.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Enviar ação personalizada com metadados.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Relatar todas as ações personalizadas imediatamente (enviar todas as ações salvas).

   ```
   Marketo.reportAll();
   ```

## Solução de problemas de ações personalizadas

A configuração de ações personalizadas para dispositivos móveis é simples, mas há restrições quanto ao número de caracteres que você pode enviar do SDK móvel para o Marketo. Certifique-se de que todas as ações personalizadas relatadas ao Marketo por meio do SDK móvel tenham menos de 20 caracteres.

**Observação sobre casos de uso de vários usuários em um dispositivo compartilhado:** quando um usuário faz logon em um aplicativo móvel integrado ao Marketo SDK, é feita a primeira chamada para associar o cliente potencial à instalação do aplicativo. Depois que esta chamada for concluída com sucesso, outras atividades do usuário no aplicativo poderão ser vistas no log de atividades do cliente potencial. Observe que, como esta é uma chamada assíncrona, se houver ações personalizadas registradas imediatamente após o logon, elas poderão ser associadas ao usuário que estava conectado anteriormente até que a chamada associada seja bem-sucedida.
