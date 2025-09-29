---
title: Notificações por push
feature: Mobile Marketing
description: Guia para ativar notificações por push do iOS com o Marketo, de certificados APNs e configuração do Xcode à integração, registro de token e manuseio de Marketo SDK.
exl-id: 41d657d8-9eea-4314-ab24-fd4cb2be7f61
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Notificações por push

Como ativar as Notificações por push.

## Configurar notificação por push no iOS

Há três etapas para ativar as notificações por push:

1. Configure notificações por push na conta do desenvolvedor do Apple.
1. Ative notificações por push no xCode.
1. Ative notificações por push no aplicativo com o Marketo SDK.

### Configurar notificações por push na conta de desenvolvedor do Apple

1. Faça logon no Apple Developer [Member Center](http://developer.apple.com/membercenter).
1. Clique em &quot;Certificados, Identificadores e Perfis&quot;.
1. Clique na pasta &quot;Certificados->Todos&quot; abaixo de &quot;iOS, tvOS, watchOS&quot;.
1. Selecione o &quot;+&quot; na tela superior esquerda ao lado dos certificados ![](assets/certificates-plus.png)
1. Ative a caixa de seleção &quot;Apple Push Notification service SSL (Sandbox &amp; Production)&quot; e clique em &quot;Continuar&quot;.
1. Selecione o identificador de aplicativo que você está usando para compilar o aplicativo.![](assets/push-appid.png)
1. Crie e faça upload do CSR para gerar o certificado de push. ![](assets/push-ssl.png)
1. Baixe o certificado no computador local e clique duas vezes para instalá-lo. ![](assets/certificate-download.png)
1. Abra &quot;Acesso ao chaveiro&quot;, clique com o botão direito no certificado e exporte 2 itens para o arquivo `.p12`.![cadeia_de_chaves](assets/key-chain.png)
1. Faça upload desse arquivo por meio do Marketo Admin Console para configurar as notificações.
1. Atualizar perfis de provisionamento de aplicativo.

### Ativar notificações por push no xCode

Ativar o recurso de notificação por push no projeto xCode.![](assets/push-xcode.png)

### Ativar notificações por push no aplicativo com o Marketo SDK

Adicione o seguinte código ao arquivo `AppDelegate.m` para entregar notificações por push aos dispositivos do cliente.

**Observação** - Se estiver usando a extensão [!DNL Adobe Launch], use `ALMarketo` como nome de classe

Importar sequência em `AppDelegate.h`.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
#import <UserNotifications/UserNotifications.h>
```

>[!TAB Swift]

```
import UserNotifications
```

>[!ENDTABS]

Adicione `UNUserNotificationCenterDelegate` a `AppDelegate` como mostrado abaixo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
@interface AppDelegate : UIResponder <UIApplicationDelegate, UNUserNotificationCenterDelegate>
```

>[!TAB Swift]

```
class AppDelegate: UIResponder, UIApplicationDelegate , UNUserNotificationCenterDelegate
```

>[!ENDTABS]

Iniciar serviço de notificação por push. Para ativar a notificação por push, adicione o código abaixo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        center.delegate = self;
        [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
            if(!error){
                dispatch_async(dispatch_get_main_queue(), ^{
                    [[UIApplication sharedApplication] registerForRemoteNotifications];
                });
            }
        }];

    return YES;
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

    UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound,    .badge]) { granted, error in
            if let error = error {
                print("\(error.localizedDescription)")
            } else {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }

        return true
}
```

>[!ENDTABS]

Chame esse método para iniciar o processo de registro com o Apple Push Service. Se o registro for bem-sucedido, o aplicativo chamará o método `application:didRegisterForRemoteNotificationsWithDeviceToken:` do objeto delegado do aplicativo e passará um token de dispositivo.

Se o registro falhar, o aplicativo chamará o método `application:didFailToRegisterForRemoteNotificationsWithError:` do delegado do aplicativo.

Registrar token de push com o Marketo. Para receber notificações por push do Marketo, você deve registrar o token do dispositivo no Marketo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    // Register the push token with Marketo
    Marketo.sharedInstance().registerPushDeviceToken(deviceToken)
}
```

>[!ENDTABS]

O token também pode ter seu registro cancelado quando o usuário fizer logout.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
[[Marketo sharedInstance] unregisterPushDeviceToken];
```

>[!TAB Swift]

```
Marketo.sharedInstance().unregisterPushDeviceToken
```

>[!ENDTABS]

Para registrar novamente o token de push, extraia o código da etapa 3 em um método AppDelegate e chame do método de logon ViewController.

Lidar com a notificação por push. Para receber notificações por push do Marketo, você deve registrar o token do dispositivo no Marketo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    [[Marketo sharedInstance] handlePushNotification:userInfo];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
    Marketo.sharedInstance().handlePushNotification(userInfo)
}
```

>[!ENDTABS]

Adicione o seguinte método no AppDelegate

Ao usar esse método, você pode apresentar alertas, sons ou aumentar o selo enquanto o aplicativo está em primeiro plano. Você deve chamar o completionHandler de sua escolha neste método.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
-(void)userNotificationCenter:(UNUserNotificationCenter *)center
    willPresentNotification:(UNNotification *)notification
        withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{

    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter,
            willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (
    UNNotificationPresentationOptions) -> Void) {
    completionHandler([.alert, .sound,.badge])
}
```

>[!ENDTABS]

Lidar com notificações por push recebidas recentemente no AppDelegate

O método será chamado no delegado quando o usuário responder à notificação abrindo o aplicativo, descartando a notificação ou escolhendo um UNNotificationAction. O delegado deve ser definido antes que o aplicativo retorne de applicationDidFinishLaunching:.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter,
                                didReceive response: UNNotificationResponse,
                                withCompletionHandler
                                completionHandler: @escaping () -> Void) {
        Marketo.sharedInstance().userNotificationCenter(center, didReceive: response, withCompletionHandler: completionHandler)
}
```

>[!ENDTABS]

Rastrear notificações por push

Se o aplicativo estiver sendo executado em segundo plano (ou não estiver ativo), o dispositivo receberá uma notificação por push, como mostrado abaixo. O Marketo rastreará quando o usuário tocar na notificação.

![celular8](assets/mobile8.png)

Se o dispositivo receber uma notificação por push, ela será passada para o retorno de chamada `application:didReceiveRemoteNotification:` no delegado do aplicativo.

Veja a seguir um registro de atividades do Marketo no Marketo que mostra eventos de aplicativo e eventos de notificação por push.

![celular9](assets/mobile9.png)

## Configurar notificação por push no Android

1. Adicione a seguinte permissão dentro da tag do aplicativo.

   Abra `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se seu aplicativo já solicitar essas permissões, ignore esta etapa.

   ```xml
   <uses‐permission android:name="android.permission.INTERNET"/>
   <uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
   
   <!‐‐Following permissions are required for push notification.‐‐>
   <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
   <!‐‐Keeps the processor from sleeping when a message is received.‐‐>
   <uses-permission android:name="android.permission.WAKE_LOCK"/>
   <permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
   <uses-permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" />
   <!-- This app has permission to register and receive data message. -->
   <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   ```

1. Configuração do FCM com HTTPv1 (o Google tem [protocolo XMPP descontinuado](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) em 12 de junho de 2023 e será removido em junho de 2024)

- Habilitar MME FCM HTTPv1 no gerenciador de recursos do Marketo ![](assets/feature-manager.png)
   - Faça upload do arquivo Json da conta de serviço para o aplicativo em MLM.
   - É possível baixar o arquivo Json da conta de serviço no console do Firebase.   ![](assets/fcm-console.png)
   - Aguarde uma hora depois de fazer upload do arquivo Json da conta de serviço no Marketo antes de enviar as notificações por push.  

## Dispositivos de teste Android

Adicione a atividade Marketo no arquivo de manifesto dentro da tag do aplicativo.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

## Registrar serviço de push do Marketo

1. Para receber notificações por push do Marketo, você deve adicionar o serviço de mensagens do Firebase ao `AndroidManifest.xml`. Adicione antes de fechar a tag do aplicativo.

   ```xml
   <meta-data
       android:name="com.google.android.gms.version"
       android:value="@integer/google_play_services_version" />
   <service android:name=".MyFirebaseMessagingService">
   <intent-filter>
   <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
   <action android:name="com.google.firebase.MESSAGING_EVENT"/>
   </intent-filter>
   </service>
   ```

1. Adicione métodos do Marketo SDK no arquivo `MyFirebaseMessagingService` da seguinte maneira

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String s) {
           super.onNewToken(s);
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.setPushNotificaitonToken(s);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.showPushNotificaiton(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

   **Observação** - Se estiver usando a extensão do Adobe, adicione como abaixo

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String token) {
           super.onNewToken(token);
           ALMarketo.setPushNotificationToken(token);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           ALMarketo.showPushNotification(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

**OBSERVAÇÃO**: o FCM SDK adiciona automaticamente todas as permissões necessárias, bem como a funcionalidade do receptor necessária. Remova os seguintes elementos obsoletos (e possivelmente prejudiciais, pois podem causar duplicação de mensagem) do manifesto do aplicativo se você tiver usado versões anteriores do SDK

```xml
<receiver android:name="com.marketo.MarketoBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <!‐‐Receives the actual messages.‐‐>
        <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
        <!‐‐Register to enable push notification‐‐>
        <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
        <!‐‐‐Replace YOUR_PACKAGE_NAME with your own package name‐‐>
        <category android:name="YOUR_PACKAGE_NAME"/>
    </intent-filter>
</receiver>

<!‐‐Marketo service to handle push registration and notification‐‐>
<service android:name="com.marketo.MarketoIntentService"/>
```

1. Inicializar o Marketo Push Depois de salvar a configuração acima, você deve inicializar a Notificação por push do Marketo. Crie ou abra a classe Application e copie/cole o código abaixo. Você pode obter a ID do remetente no console Firebase.

   ```java
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   
   // Enable push notification here. The push notification channel name can by any string
   marketoSdk.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Se estiver usando a Extensão [!DNL Adobe Launch], use estas instruções

   ```java
   // Enable push notification here. The push notification channel name can by any string
   ALMarketo.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Se você não tiver um SENDER_ID, habilite o Serviço de Cloud Messaging do Google completando as etapas detalhadas neste [tutorial](https://developers.google.com/cloud-messaging/).

   O token também pode ter seu registro cancelado quando o usuário fizer logout.

   ```java
   marketoSdk.uninitializeMarketoPush();
   ```

   Se estiver usando a extensão [!DNL Adobe Launch], use a instrução abaixo

   ```java
   ALMarketo.uninitializeMarketoPush();
   ```

   Observação: para registrar novamente o token de push, extraia o código da etapa 3 em um método AppDelegate e chame do método de logon ViewController.

1. Definir ícone de notificação (opcional) Para configurar um ícone de notificação personalizado, o método a seguir deve ser chamado.

   ```java
   MarketoConfig.Notification config = new MarketoConfig.Notification();
   // Optional bitmap for honeycomb and above
   config.setNotificationLargeIcon(bitmap);
   
   // Required icon Resource ID
   config.setNotificationSmallIcon(R.drawable.notification_small_icon);
   
   // Set the configuration
   //Use the static methods on ALMarketo class when using Adobe Extension
   Marketo.getInstance(context).setNotificationConfig(config);
   
   // Get the configuration set
   Marketo.getInstance(context).getNotificationConfig();
   ```

## Solução de problemas

A configuração de mensagens de push móveis envolve muitas etapas e a coordenação de desenvolvedores e profissionais de marketing. Se você estiver com dificuldades, há algumas coisas simples que você pode verificar.

Depois de garantir que as coisas simples estejam corretas, você pode se aprofundar nos detalhes da programação.

### A mensagem de push não está aparecendo

Primeiro, verifique se as mensagens de push estão desativadas no fone. Os usuários móveis podem controlar se recebem ou não mensagens para qualquer aplicativo específico. Geralmente, os desenvolvedores (e profissionais de marketing) desativarão essas mensagens em algum momento durante o desenvolvimento. Portanto, a primeira coisa a verificar é se o recipient desativou as mensagens de push para o seu aplicativo.

Segundo, o aplicativo já está aberto e ativo no dispositivo? Quando seu aplicativo é o aplicativo ativo no dispositivo, as mensagens por push móveis não aparecem na tela. Em vez disso, elas são exibidas na área &quot;notificações locais&quot; do aplicativo.

### Exibir os logs de atividade no Marketo

O primeiro local a ser observado ao rastrear um erro é nos logs de atividade do Marketo. Você pode usar os logs de atividades para verificar se uma mensagem foi enviada.

No registro de atividades, verifique os registros de atividade de uma pessoa que deveria receber uma mensagem. Se a mensagem foi enviada, haverá um registro no log de atividades. Caso contrário, o problema provavelmente ocorre devido à configuração do certificado do iOS ou da chave da API do Android no Marketo.

### O certificado ou a chave é inválido

Verifique novamente sua configuração para garantir que você tenha o certificado adequado carregado para Sandbox ou Produção. Às vezes, é melhor fazer com que o desenvolvedor reexporte os certificados (iOS) ou as chaves (Android) e, em seguida, recarregue-os no Marketo para garantir que estejam corretos.

### Um certificado ou uma chave (iOS) está ausente no arquivo .p12

Ao exportar o certificado, exporte a chave _e_ do certificado.

### Provisionamento de perfis desatualizados (iOS)

Sempre que você adicionar um novo dispositivo, deverá atualizar os perfis de provisionamento e gerar novos certificados. Verifique se o projeto Xcode aponta para os perfis e certificados corretos e importe esses certificados para a Marketo.

### Não é possível carregar o certificado do iOS (IOS)

Verifique se a senha usada ao exportar o certificado não contém espaços.  Por exemplo, em vez disso:

`Hello World 123`

use isto:

`HelloWorld123`

### Solução de problemas de certificados iOS

Para aplicativos de sandbox, você pode usar um certificado &quot;desenvolvedor&quot; ou &quot;universal&quot;. Mas para aplicativos de produção é necessário carregar um certificado válido de &quot;distribuição&quot; ou &quot;universal&quot;.

### Rejeição por push/token inválido

Um token de registro existente pode deixar de ser válido em vários cenários, incluindo:

- Se o aplicativo cliente cancelar o registro no GCM.
- Se o aplicativo cliente tiver o registro cancelado automaticamente, o que pode acontecer se o usuário desinstalar o aplicativo. Por exemplo, no iOS, se o Serviço de feedback APNS relatou o token APNS como inválido.
- Se o token de registro expirar. Por exemplo, a Google pode decidir atualizar tokens de registro ou o token APNS expirou para dispositivos iOS.
- Se o aplicativo cliente estiver atualizado, mas a nova versão não estiver configurada para receber mensagens.
