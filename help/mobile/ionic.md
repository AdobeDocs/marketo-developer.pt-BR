---
title: "Iônico"
feature: "Mobile Marketing"
description: "Uso do Ionic com Marketo para dispositivos móveis"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 1%

---


# Iônico

Este tópico descreve como integrar o plug-in Cordova do Marketo. O capacitor iônico não é suportado no momento.

## Pré-requisitos

1. [Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenha a chave secreta e a ID do Munchkin do seu aplicativo).
1. Configurar notificações por push ([iOS](push-notifications.md) | [Android](push-notifications.md) ).
1. Instalar [Iônico](https://ionicframework.com/getting-started/) &amp; [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instruções de instalação

### Configurar o plug-in jônico do Marketo

1. Supondo que a CLI do Cordova esteja instalada, vá para o diretório de aplicativos Ionic e execute o seguinte comando para adicionar o plug-in do Marketo ao aplicativo:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Para confirmar que o plug-in foi adicionado ao aplicativo, execute o seguinte comando:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migrar para a versão mais recente (opcional)

1. Para remover um plug-in existente, execute o seguinte comando:

   `$ ionic plugin remove com.marketo.plugin`

1. Para ler o plug-in, execute o seguinte comando:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Ativar notificações por push no xCode

1. Ative o recurso de notificação por push no projeto xCode.![Recurso de notificação](assets/notification-capability.png)

### Rastrear notificações por push

Cole o código a seguir dentro da `application:didFinishLaunchingWithOptions:` função.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Inicializar a estrutura do Marketo

Para garantir que a estrutura do Marketo seja iniciada na inicialização do aplicativo, adicione o seguinte código no `onDeviceReady` no arquivo JavaScript principal.

Você deve passar `ionicCordova` como tipo de estrutura para aplicativos Cordova Iônicos.

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

- Retorno de Chamada com Êxito : função a ser executada se a estrutura do Marketo for inicializada com êxito.
- Retorno de Chamada de Falha : função a ser executada se a estrutura do Marketo falhar ao inicializar.
- ID do MUNCHKIN : ID do Munchkin recebida do Marketo no momento do registro.
- CHAVE SECRETA : chave secreta recebida do Marketo no momento do registro.

### Inicializar notificação por push do Marketo

Para garantir que a notificação por push do Marketo seja iniciada, adicione o seguinte código após a função inicializada no arquivo JavaScript principal.

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

- Retorno de Chamada com Êxito : função a ser executada se a notificação por push do Marketo for inicializada com êxito.
- Retorno de Chamada de Falha : função a ser executada se a notificação por push do Marketo falhar ao inicializar.
- GCM_PROJECT_ID : ID do projeto GCM encontrada em [Console de desenvolvedores do Google](https://accounts.google.com/ServiceLogin?service=cloudconsole&amp;passive=1209600&amp;osid=1&amp;continue=https://console.cloud.google.com/apis/dashboard&amp;followup=https://console.cloud.google.com/apis/dashboard) após criar o aplicativo.

O token também pode ser cancelado no logout.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associar lead

Você pode criar um cliente potencial do Marketo chamando a função associateLead.

### Sintaxe

```javascript
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parâmetros

- Retorno de Chamada com Êxito : função a ser executada se o Marketo framework associar o lead com êxito.
- Retorno de Chamada de Falha : função a ser executada se o Marketo framework falhar ao associar o lead.
- Dados de Cliente Potencial : dados de cliente potencial no formato de cadeia de caracteres JSON.

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

Você pode relatar qualquer ação realizada pelo usuário chamando o `reportaction` função.

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

- Retorno de Chamada com Êxito : função a ser executada se a estrutura do Marketo relatar ação com êxito.
- Retorno de Chamada de Falha : função a ser executada se a estrutura do Marketo falhar ao relatar a ação.
- Nome da Ação : nome da ação.
- Dados de Ação : dados de ação no formato de sequência JSON.

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

Vincule os tipos de evento &quot;pausar&quot; e &quot;retomar&quot; conforme mostrado abaixo para relatar eventos Start e Stop. Isso é usado para rastrear o tempo gasto no aplicativo móvel. Observação: isso é necessário no Android.

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

Dependendo do método usado, um lead recém-criado é reconhecido por acionadores e filtros diferentes. Os clientes em potencial criados usando o SDK do MME ou a API REST aparecem nos acionadores e filtros &quot;Criados por clientes potenciais&quot;. Os clientes em potencial criados por envios de formulários aparecem nos acionadores e filtros &quot;Preencher formulário&quot;.

A prática recomendada é manter a consistência com o método usado pelo aplicativo web ao criar leads. Se você já tiver um aplicativo Web que usa o envio de formulários como o mecanismo para criar clientes potenciais, use o mesmo mecanismo ao criar clientes potenciais no aplicativo híbrido. Se você já tiver um aplicativo Web que usa nossa API REST como o mecanismo para criar leads, use esse mesmo mecanismo ao criar leads no aplicativo híbrido. Nos casos em que você não usa o envio de formulários nem a API REST como um mecanismo para criar clientes em potencial no aplicativo Web, é possível considerar o uso do SDK do MME para criar clientes em potencial no Marketo.
