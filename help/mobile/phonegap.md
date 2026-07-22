---
title: PhoneGap
feature: Mobile Marketing
description: Configure o plug-in Marketo PhoneGap com o Cordova, configure o Firebase Cloud Messaging, ative o iOS e o Android por push, rastreie as notificações e inicialize o SDK.
exl-id: 99f14c76-9438-4942-9309-643bca434d07
TQID: https://experienceleague.adobe.com/eFAwR7r5IE6vKigsEWrJdCmC3VrfB-nl0h8x7Vgt1VY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 775
ht-degree: 2%

---

# PhoneGap

Integre o plug-in Marketo PhoneGap a um aplicativo Cordova.

## Pré-requisitos

1. [Adicione um aplicativo ao Administrador do Marketo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e obtenha a Chave Secreta e a ID do Munchkin do aplicativo.
1. Configurar notificações por push para [iOS](push-notifications.md) ou [Android](push-notifications.md).
1. [Instalar a CLI do PhoneGap/Cordova](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instruções de instalação

1. Configurar o plug-in Marketo PhoneGap.

   Vá para o diretório do aplicativo PhoneGap e execute o seguinte comando para adicionar o plug-in do Marketo:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Instale o plug-in FCM.

   `$ cordova plugin add cordova-plugin-fcm`

   Execute o seguinte comando para confirmar que o plug-in foi adicionado:

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migrar para uma versão mais recente (opcional)**

Para remover um plug-in existente, execute o seguinte comando:

`$ cordova plugin remove com.marketo.plugin`

Para adicionar o plug-in novamente, execute o seguinte comando:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova versão 8.0.0 (Cordova@Android7.0.0) e posterior**

Depois de criar a plataforma Cordova Android, abra o aplicativo no Android Studio. Atualize o valor `dirs` no arquivo `Marketo.gradle` da pasta `com.marketo.plugin`.

```groovy
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Adicionar as plataformas de destino para o aplicativo: `$cordova platform add android` `$ cordova platform add ios`

Verifique as plataformas adicionadas: `$cordova platform ls`

1. Suporte a Firebase Cloud Messaging

1. Configure o aplicativo Firebase no Console do Firebase.
   1. Crie ou adicione um projeto no [&#128279;](https://console.firebase.google.com/)Console Firebase.
      1. No [console Firebase](https://console.firebase.google.com/), selecione **[!UICONTROL Adicionar projeto]**.
      1. Selecione o projeto GCM na lista de projetos existentes do Google Cloud e selecione **[!UICONTROL Adicionar Firebase]**.
      1. Na tela de boas-vindas do Firebase, selecione &quot;Adicionar o Firebase ao aplicativo do Android&quot;.
      1. Forneça o nome do pacote e SHA-1 e selecione **[!UICONTROL Adicionar aplicativo]**. Um novo arquivo `google-services.json` para seu aplicativo Firebase está sendo baixado.
   1. Vá para **[!UICONTROL Configurações do Projeto]** em [!UICONTROL Visão Geral do Projeto].
      1. Selecione a guia **[!UICONTROL Geral]** e baixe o arquivo &quot;google-services.json&quot;.
      1. Selecione a guia **[!UICONTROL Cloud Messaging]**. Copie a [!UICONTROL Chave do Servidor] e a [!UICONTROL ID do Remetente] e forneça-as para a Marketo.
   1. Configure o FCM no aplicativo PhoneGap.
      1. Mova o arquivo &quot;google-services.json&quot; baixado para o diretório raiz do módulo do aplicativo PhoneGap.
      1. Remover o arquivo &#39;MyFirebaseInstanceIDService&#39; do local `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (obsoleto)
      1. Modifique o arquivo &#39;MyFirebaseMessagingService&#39; no local `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` da seguinte maneira:

         ```
         import com.marketo.Marketo;
         
         public class MyFirebaseMessagingService extends FirebaseMessagingService{
         
         @Override
         public void onNewToken(String s){
           super.onNewToken(s);
           MarketoExtension.setPushNotificaitonTokens(s);
           //Add your code here
         }
         
         @Override
         public void onMessageReceived(RemoteMessage remoteMessage) {
           MarketoExtension.showPushNotificaiton(remoteMessage);
           //Add your code here
         }
         }
         ```

         1. Modifique o arquivo &quot;fcm_config_files_process.js&quot; em plug-ins de localização/cordova-plugin-fcm/scripts da seguinte maneira

            ```
            //change
            var strings = fs.readFileSync("platforms/android/res/values/strings.xml").toString();
            //to
            var strings = fs.readFileSync("platforms/android/app/src/main/res/values/strings.xml").toString();
            
            //AND change
            fs.writeFileSync("platforms/android/res/values/strings.xml", strings);
            //to
            fs.writeFileSync("platforms/android/app/src/main/res/values/strings.xml", strings);
            ```

### &#x200B;3. Ativar notificações por push no xCode

Ative o recurso de notificação por push no projeto xCode.

### &#x200B;4. Rastrear notificações por push

Cole o código a seguir dentro da função `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objetivo C]

Atualize o método `applicationDidBecomeActive` da seguinte maneira.

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Atualize o método `applicationDidBecomeActive` da seguinte maneira.

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Inicializar a estrutura do Marketo

Para inicializar a estrutura do Marketo quando o aplicativo for iniciado, adicione o seguinte código na função `onDeviceReady` no arquivo JavaScript principal.

Passar `phonegap` como o tipo de estrutura para aplicativos PhoneGap.

### Sintaxe

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

### Parâmetros

- Retorno de chamada bem-sucedido: Função a ser executada se a estrutura do Marketo for inicializada com sucesso.
- Failure Callback: função a ser executada se a estrutura do Marketo falhar ao inicializar.
- MUNCHKIN ID: Munchkin ID recebida do Marketo durante o registro.
- SECRET KEY: chave secreta recebida do Marketo durante o registro.

### &#x200B;6. Inicializar notificação por push do Marketo

Para inicializar as notificações por push do Marketo, adicione o seguinte código após a função de inicialização no arquivo principal do JavaScript.

### Sintaxe

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parâmetros

- Retorno de chamada bem-sucedido: função a ser executada se a notificação por push do Marketo for inicializada com sucesso.
- Failure Callback: função a ser executada se a notificação por push do Marketo falhar ao inicializar.
- GCM_PROJECT_ID: ID do projeto GCM encontrada no [Console de desenvolvedores do Google](https://console.developers.google.com/) após a criação do aplicativo.

Você também pode cancelar o registro do token ao fazer logoff.

```javascript
marketo. uninitializeMarketoPush(
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
lead[marketo.KEY_FIRST_NAME] = "Phone";
lead[marketo.KEY_LAST_NAME] = "Gap";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";
// To use lead custom field, use the REST API NAME as key
lead["REST API NAME"] = "value";

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
