---
title: PhoneGap
feature: Mobile Marketing
description: Uso do PhoneGap com Marketo em dispositivos móveis
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 1%

---

# PhoneGap

Integração do plug-in Marketo PhoneGap

## Pré-requisitos

1. [Adicione um aplicativo ao Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenha a Chave Secreta e a ID do Munchkin do aplicativo).
1. Configurar notificações por push ([iOS](push-notifications.md) | [Android](push-notifications.md)).
1. [Instalar a CLI do PhoneGap/Cordova](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instruções de instalação

1. Configurar o plug-in do Marketo PhoneGap

   Supondo que a CLI do Cordova esteja instalada, vá para o diretório do aplicativo PhoneGap e execute o seguinte comando para adicionar o plug-in do Marketo ao aplicativo:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Instalar o plug-in FCM

   `$ cordova plugin add cordova-plugin-fcm`

   Para confirmar se o plug-in foi adicionado ao aplicativo, execute o comando a seguir e verifique

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migrar para uma versão mais recente (opcional)**

Para remover um plug-in existente, execute o seguinte comando:

`$ cordova plugin remove com.marketo.plugin`

Para adicionar novamente o plug-in, execute o seguinte comando:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova versão 8.0.0 (Cordova@Android7.0.0) e posterior**

Depois que a plataforma Cordova Android for criada, abra o aplicativo com o Android Studio e atualize o valor `dirs` do arquivo `Marketo.gradle` localizado na pasta `com.marketo.plugin`.

```
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Adicione as plataformas a serem segmentadas para o aplicativo `$cordova platform add android` `$ cordova platform add ios`

Verificar a lista de plataformas adicionadas `$cordova platform ls`

1. Suporte a Firebase Cloud Messaging

1. Configurar o aplicativo Firebase no console do Firebase.
   1. Criar/adicionar um projeto no [](https://console.firebase.google.com/)Console Firebase.
      1. No [console Firebase](https://console.firebase.google.com/), selecione **[!UICONTROL Adicionar projeto]**.
      1. Selecione o projeto GCM na lista de projetos existentes do Google Cloud e selecione **[!UICONTROL Adicionar Firebase]**.
      1. Na tela de boas-vindas do Firebase, selecione &quot;Adicionar o Firebase ao aplicativo do Android&quot;.
      1. Forneça o nome do pacote e SHA-1 e selecione **[!UICONTROL Adicionar aplicativo]**. Um novo arquivo `google-services.json` para seu aplicativo Firebase está sendo baixado.
   1. Navegue até **[!UICONTROL Configurações do projeto]** em [!UICONTROL Visão geral do projeto]
      1. Clique na guia **[!UICONTROL Geral]**. Baixe o arquivo &quot;google-services.json&quot;.
      1. Clique na guia **[!UICONTROL Cloud Messaging]**. Copiar [!UICONTROL Chave do Servidor] e [!UICONTROL ID do Remetente]. Forneça estas [!UICONTROL Chave do Servidor] e [!UICONTROL ID do Remetente] à Marketo.
   1. Configurar alterações do FCM no aplicativo Phonegap
      1. Mova o arquivo &quot;google-services.json&quot; baixado para o diretório raiz do módulo do aplicativo Phonegap
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

Atualize o método `applicationDidBecomeActive` conforme abaixo

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Atualize o método `applicationDidBecomeActive` conforme abaixo

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Inicializar o Marketo Framework

Para garantir que a estrutura do Marketo seja iniciada na inicialização do aplicativo, adicione o seguinte código na função `onDeviceReady` no arquivo JavaScript principal.

Observe que devemos passar `phonegap` como tipo de estrutura para Aplicativos PhoneGap.

### Sintaxe

```
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

- Retorno de Chamada com Êxito : função a ser executada se a estrutura do Marketo for inicializada com êxito.
- Retorno de Chamada de Falha : função a ser executada se a estrutura do Marketo falhar ao inicializar.
- MUNCHKIN ID : Munchkin ID recebida do Marketo no momento do registro.
- CHAVE SECRETA : chave secreta recebida do Marketo no momento do registro.

### &#x200B;6. Inicializar notificação por push do Marketo

Para garantir que a notificação por push do Marketo seja iniciada, adicione o seguinte código após a função de inicialização no arquivo principal do JavaScript.

### Sintaxe

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parâmetros

- Retorno de Chamada com Êxito : função a ser executada se a notificação por push do Marketo for inicializada com êxito.
- Retorno de Chamada de Falha : função a ser executada se a notificação por push do Marketo falhar ao inicializar.
- GCM_PROJECT_ID : ID do projeto GCM encontrada no [Console de desenvolvedores do Google](https://console.developers.google.com/) após a criação do aplicativo.

O token também pode ser cancelado no logout.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associar lead

Você pode criar um cliente potencial do Marketo chamando a função associateLead.

### Sintaxe

```
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

```
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

Você pode relatar qualquer ação realizada pelo usuário chamando a função `reportaction`.

### Sintaxe

```
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

```
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

Vincule os tipos de evento &quot;pausar&quot; e &quot;retomar&quot; conforme mostrado abaixo para relatar eventos Start e Stop.  Isso é usado para rastrear o tempo gasto no aplicativo móvel. Observação: isso é necessário no Android.

```
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

Dependendo do método usado, um lead recém-criado será reconhecido por acionadores e filtros diferentes. Clientes potenciais criados usando o MME SDK ou a API REST aparecem nos acionadores e filtros &quot;Clientes potenciais criados&quot;. Os clientes em potencial criados por envios de formulários aparecem nos acionadores e filtros &quot;Preencher formulário&quot;.

A prática recomendada é manter a consistência com o método usado pelo aplicativo web ao criar leads. Se você já tiver um aplicativo Web que usa o envio de formulários como o mecanismo para criar clientes potenciais, use o mesmo mecanismo ao criar clientes potenciais no aplicativo híbrido. Se você já tiver um aplicativo Web que usa nossa API REST como o mecanismo para criar leads, use esse mesmo mecanismo ao criar leads no aplicativo híbrido. Nos casos em que você não usa o envio de formulários nem a API REST como um mecanismo para criar clientes em potencial no aplicativo Web, é possível considerar o uso do SDK MME para criar clientes em potencial no Marketo.
