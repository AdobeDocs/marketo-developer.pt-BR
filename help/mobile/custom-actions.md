---
title: "Ações personalizadas"
feature: "Mobile Marketing"
description: "Visão geral das ações personalizadas"
source-git-commit: c51e1b84efdf444c13714c1a08ecc4cac677f483
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Ações Personalizadas

É possível rastrear a interação do usuário enviando ações personalizadas. Quando o aplicativo móvel chama o SDK da Marketo para enviar uma ação personalizada, a ação personalizada é salva inicialmente no dispositivo. O SDK do Marketo verifica se há conectividade de Internet adequada antes de enviar a ação personalizada. Como resultado, pode haver um atraso entre o momento em que a ação personalizada é enviada e o momento em que é recebida pelo Marketo.

Ações personalizadas podem ser usadas como acionadores e filtros em Campanhas inteligentes. Para obter mais informações, consulte [Atividade do aplicativo móvel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envio de ações personalizadas no iOS

Enviar ação personalizada.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Enviar ação personalizada com metadados.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```
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

```
[sharedInstance reportAll];
```

>[!TAB Swift]

```
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

Configurar ações personalizadas para dispositivos móveis é simples, mas há restrições quanto ao número de caracteres que você pode enviar do SDK móvel para o Marketo. Verifique se todas as ações personalizadas relatadas ao Marketo por meio do SDK móvel têm menos de 20 caracteres.

**Observação sobre casos de uso de vários usuários em um dispositivo compartilhado:** Quando um usuário faz logon em um aplicativo móvel integrado ao Marketo SDK, é feita a primeira chamada para associar o cliente potencial à instalação do aplicativo. Depois que esta chamada for concluída com sucesso, outras atividades do usuário no aplicativo poderão ser vistas no log de atividades do cliente potencial. Observe que, como esta é uma chamada assíncrona, se houver ações personalizadas registradas imediatamente após o logon, elas poderão ser associadas ao usuário que estava conectado anteriormente até que a chamada associada seja bem-sucedida.
