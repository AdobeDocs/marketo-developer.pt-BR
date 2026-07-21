---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Guia passo a passo para integrar o plug-in do Marketo Cordova com o Ionic, ativar notificações por push, inicializar o SDK, rastrear sessões e associar leads.
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
TQID: https://experienceleague.adobe.com/UTNWd69NliR896RcO-XM2GG35liuLeNNhTXo9GRtB4o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 581
ht-degree: 2%

---

# Iônico

Integre o Plug-in Cordova do Marketo com um aplicativo [!DNL Ionic]. [!DNL Ionic] O capacitor não é compatível no momento.

## Pré-requisitos

1. [Adicione um aplicativo ao Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e obtenha a Chave Secreta e a ID do Munchkin do aplicativo.
1. Configurar notificações por push para [iOS](push-notifications.md) ou [Android](push-notifications.md).
1. Instale o [[!DNL Ionic]](https://ionicframework.com/getting-started/) e a [CLI do Cordova](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instruções de instalação

### Configurar o plug-in [!DNL Ionic] do Marketo

1. Vá para o diretório de aplicativos [!DNL Ionic] e execute o seguinte comando para adicionar o Plug-in do Marketo:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Execute o seguinte comando para confirmar que o plug-in foi adicionado:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migrar para a versão mais recente (opcional)

1. Para remover um plug-in existente, execute o seguinte comando:

   `$ ionic plugin remove com.marketo.plugin`

1. Para adicionar o plug-in novamente, execute o seguinte comando:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Ativar notificações por push no xCode

1. Ative o recurso de notificação por push no projeto xCode.![Recurso de Notificação](assets/notification-capability.png)

### Rastrear notificações por push

Cole o código a seguir dentro da função `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Inicializar a estrutura do Marketo

Para inicializar a estrutura do Marketo quando o aplicativo for iniciado, adicione o seguinte código na função `onDeviceReady` no arquivo JavaScript principal.

Passar `ionicCordova` como o tipo de estrutura para [!DNL Ionic] aplicativos Cordova.

#### Sintaxe

```javascript
// This method will Initialize the Marketo Framework using Your MunchkinId and Secret Key
marketo.initialize(
  function() { console.log("MarketoSDK Init done."); },
  function(error) { console.log("an error occurred:" + error); },
  'YOUR_MUNCHKIN_ID',
  'YOUR_SECRET_KEY',
  'FRAMEWORK_TYPE'
);

// For session tracking, add following.
marketo.onStart(
  function(){ console.log("onStart."); },
  function(error){ console.log("Failed to report onStart." + error); }
);
```

#### Parâmetros

- Retorno de chamada bem-sucedido: Função a ser executada se a estrutura do Marketo for inicializada com sucesso.
- Failure Callback: função a ser executada se a estrutura do Marketo falhar ao inicializar.
- MUNCHKIN ID: Munchkin ID recebida do Marketo durante o registro.
- SECRET KEY: chave secreta recebida do Marketo durante o registro.

### Inicializar notificação por push do Marketo

Para inicializar as notificações por push do Marketo, adicione o seguinte código após a função de inicialização no arquivo principal do JavaScript.

#### Sintaxe

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

#### Parâmetros

- Retorno de chamada bem-sucedido: função a ser executada se a notificação por push do Marketo for inicializada com sucesso.
- Failure Callback: função a ser executada se a notificação por push do Marketo falhar ao inicializar.
- GCM_PROJECT_ID: ID do projeto GCM encontrada no [Console de desenvolvedores do Google](https://accounts.google.com/ServiceLogin?service=cloudconsole&passive=1209600&osid=1&continue=https://console.cloud.google.com/apis/dashboard&followup=https://console.cloud.google.com/apis/dashboard) após a criação do aplicativo.

Você também pode cancelar o registro do token ao fazer logoff.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associar lead

Chame a função associateLead para criar um lead Marketo.

### Sintaxe

```javascript
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parâmetros

- Retorno de chamada de sucesso: função a ser executada se a estrutura do Marketo associar o lead com sucesso.
- Retorno de chamada de falha: função a ser executada se a estrutura do Marketo falhar ao associar o lead.
- Dados de lead: dados de lead no formato de string JSON.

### Exemplo

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Ionic";
lead[marketo.KEY_LAST_NAME] = "App";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";

// Use associateLead function to associate it.
marketo.associateLead(
  function() { console.log("MarketoSDK : Lead Associated"); },
  function(error) { console.log("an error occurred:" + error); },
  JSON.stringify(lead)
);
```

## Ação do relatório

Chame a função `reportaction` para relatar uma ação do usuário.

### Sintaxe

```javascript
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parâmetros

- Retorno de chamada bem-sucedido: função a ser executada se a estrutura do Marketo relatar a ação com êxito.
- Retorno de chamada de falha: função a ser executada se a estrutura do Marketo falhar ao relatar a ação.
- Nome da ação: Nome da ação.
- Dados de ação: dados de ação no formato de sequência JSON.

### Exemplo

```javascript
// First create an event as below
var event = {
    "Action Type":"Add To Cart",
    "Action Details":"Adding Product in cart",
    "Action Metric":"10",
    "Action Length":"1"
}

marketo.reportaction(
    function(){ console.log("Reported action successfully."); },
    function(error){ console.log("Failed to report action." + error); },
    "Add To Cart",
    JSON.stringify(event)
);
```

## Relatório da sessão

Vincule os tipos de evento &quot;pausar&quot; e &quot;retomar&quot; para relatar eventos Start e Stop. Esses eventos rastreiam o tempo gasto no aplicativo móvel e são necessários no Android.

```javascript
//Add the following code in your www/js/index.js

bindEvents: function() {
   document.addEventListener('pause', this.onStop, false);
   document.addEventListener('resume', this.onStart, false);
},
onStop: function() {
   marketo.onStop(
       function(){ console.log("onStop"); },
       function(error){ console.log("Failed to report onStop." + error); }
   );
},
onStart: function() {
   marketo.onStart(
       function(){ console.log("onStart."); },
       function(error){console.log( "Failed to report onStart." + error); }
   );
},
```

## Criação de clientes em potencial

Há três maneiras de criar leads a partir de um aplicativo híbrido:

1. MARKETO MME SDK
1. API REST DO MARKETO
1. Envio de formulários

Os acionadores e filtros que reconhecem um novo lead dependem do método de criação:

- Clientes potenciais criados com o MME SDK ou a REST API aparecem nos acionadores e filtros &quot;Clientes potenciais criados&quot;.
- Os clientes potenciais criados pelo envio do formulário aparecem nos acionadores e filtros &quot;Preencher formulário&quot;.

Use o mesmo método de criação de leads no aplicativo híbrido e no aplicativo da Web. Se o aplicativo web usar o envio de formulário ou a API REST, use esse método no aplicativo híbrido. Se o aplicativo web não usar nenhum dos métodos, considere usar o SDK MME para criar leads no Marketo.
