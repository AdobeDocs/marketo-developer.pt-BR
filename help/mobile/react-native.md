---
title: React Native
feature: Mobile Marketing
description: Instalação do React Native para Marketo
exl-id: 462fd32e-91f1-4582-93f2-9efe4d4761ff
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 0%

---

# React Native

Este artigo fornece informações sobre como instalar e configurar o SDK nativo da Marketo para integrar seu aplicativo móvel à nossa plataforma.

## Pré-requisitos

[Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenha a chave secreta e a ID do Munchkin do seu aplicativo).

## Integração do SDK

### Integração do SDK do Android

**Configurar usando Gradle**

Adicione a dependência do SDK do Marketo com a versão mais recente: no nível do aplicativo `build.gradle` arquivo, na seção dependências, adicione (incluindo a versão apropriada do SDK do Marketo)

```
implementation 'com.marketo:MarketoSDK:0.x.x'
```

**Adicionar repositório mavencentral**

O SDK do Marketo está disponível no [repositório central maven](https://mvnrepository.com/). Para sincronizar esses arquivos, adicione `mavencentral` repositório para raiz `build.gradle`

```
build script {
  repositories {
    google()
    mavencentral()
  }
}
```

Em seguida, sincronize seu projeto com os arquivos Gradle.

#### Integração do SDK do iOS

Antes de criar uma ponte para o seu projeto React Native, é importante configurar o SDK no seu projeto Xcode.

**Integração do SDK - Uso do CocoaPods**

Usar nosso SDK do iOS no seu aplicativo é fácil. Execute as seguintes etapas para configurá-lo no projeto Xcode do seu aplicativo usando o CocoaPods, para que você possa integrar nossa plataforma ao seu aplicativo.

Baixar [CocoaPods](https://cocoapods.org/) - Distribuído como uma gem Ruby, é um gerenciador de dependências para Objetive-C e Swift que simplifica o processo de uso de bibliotecas de terceiros em seu código, como o SDK do iOS.

Para baixá-lo e instalá-lo, inicie um terminal de linha de comando no Mac e execute o seguinte comando nele:

1. Instale o CocoaPods.

`$ sudo gem install cocoapods`

1. Abra o Podfile. (Na pasta iOS do projeto ReactNative)

`$ open -a Xcode Podfile`

1. Adicione a seguinte linha ao Podfile.

`$ pod 'Marketo-iOS-SDK'`

1. Salve e feche o Podfile.

1. Baixe e instale o SDK do Marketo iOS.

`$ pod install`

1. Abra o espaço de trabalho no Xcode.

`$ open App.xcworkspace`

## Instruções de instalação do módulo nativo

Às vezes, um aplicativo do React Native precisa acessar uma API de plataforma nativa que não está disponível por padrão no JavaScript, por exemplo, as APIs nativas para acessar o Apple ou o Google Pay. Talvez você queira reutilizar algumas bibliotecas Objetive-C, Swift, Java ou C++ existentes sem ter que reimplementá-las em JavaScript ou escrever algum código multithread de alto desempenho para coisas como processamento de imagem.

O sistema NativeModule expõe instâncias de classes Java/Objetive-C/C++ (nativas) para JavaScript (JS) como objetos JS, permitindo assim executar código nativo arbitrário de dentro do JS. Embora não esperemos que esse recurso faça parte do processo de desenvolvimento normal, é essencial que ele exista. Se o React Native não exportar uma API nativa de que seu aplicativo JS precisa, você mesmo poderá exportá-la!

A ponte do React Native é usada para a comunicação entre as camadas JSX e do aplicativo nativo. No nosso caso, o aplicativo host poderá gravar o código JSX que pode chamar os métodos do SDK do Marketo.

### Android

Esse arquivo contém os métodos do invólucro que podem chamar os métodos do SDK da Marketo internamente com parâmetros fornecidos por você.

```
public class RNMarketoModule extends ReactContextBaseJavaModule {

   final Marketo marketoSdk;
   RNMarketoModule(ReactApplicationContext context) {
       super(context);
       marketoSdk = Marketo.getInstance(context);
   }

   @NonNull
   @Override
   public String getName() {
       return "RNMarketoModule";
   }

   @ReactMethod
   public void associateLead(ReadableMap leadData) {

       MarketoLead mLead = new MarketoLead();
       try {
           mLead.setCity(leadData.getString(MarketoLead.KEY_CITY));
           mLead.setFirstName(leadData.getString(MarketoLead.KEY_FIRST_NAME));
           mLead.setLastName(leadData.getString(MarketoLead.KEY_LAST_NAME));
           mLead.setAddress(leadData.getString(MarketoLead.KEY_ADDRESS));
           mLead.setEmail(leadData.getString(MarketoLead.KEY_EMAIL));
           mLead.setBirthDay(leadData.getString(MarketoLead.KEY_BIRTHDAY));
           mLead.setCountry(leadData.getString(MarketoLead.KEY_COUNTRY));
           mLead.setFacebookId(leadData.getString(MarketoLead.KEY_FACEBOOK));
           mLead.setGender(leadData.getString(MarketoLead.KEY_GENDER));
           mLead.setState(leadData.getString(MarketoLead.KEY_STATE));
           mLead.setPostalCode(leadData.getString(MarketoLead.KEY_POSTAL_CODE));
           mLead.setTwitterId(leadData.getString(MarketoLead.KEY_TWITTER));
           marketoSdk.associateLead(mLead);
       }
       catch (MktoException e){
       }
   }
   @ReactMethod
   public void setSecureSignature(ReadableMap readableMap) {
       MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
       secureMode.setAccessKey(readableMap.getString("accessKey"));
       secureMode.setEmail(readableMap.getString("email"));
       secureMode.setSignature(readableMap.getString("signature"));
       secureMode.setTimestamp(readableMap.getInt("timeStamp"));
       marketoSdk.setSecureSignature(secureMode);
   }
   @ReactMethod
   public void initializeSDK(String munchkinId, String appSecreteKey){
       marketoSdk.initializeSDK(munchkinId,appSecreteKey);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId){
       marketoSdk.initializeMarketoPush( projectId);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId, String channelName){
       marketoSdk.initializeMarketoPush( projectId, channelName);
   }

   @ReactMethod
   public void uninitializeMarketoPush(){
       marketoSdk.uninitializeMarketoPush();
   }

   @ReactMethod
   public void reportAction(String action){
       Marketo.reportAction(action, null);
   }

   @ReactMethod
   public void reportAction(String action, ReadableMap readableMap){
       MarketoActionMetaData marketoActionMetaData = new MarketoActionMetaData();
       marketoActionMetaData.setActionDetails(readableMap.getString("setMetric"));
       marketoActionMetaData.setActionMetric(readableMap.getString("setLength"));
       marketoActionMetaData.setActionLength(readableMap.getString("actionDetails"));
       marketoActionMetaData.setActionType(readableMap.getString("actionType"));
       Marketo.reportAction(action, marketoActionMetaData);
   }
}
```

**Registrar o pacote**

Informe o react-native sobre o pacote do Marketo.

```
public class MarketoPluginPackage implements ReactPackage {

   @NonNull
   @Override
   public List createNativeModules(@NonNull ReactApplicationContext reactContext) {
           List modules = new ArrayList<>();

           modules.add(new RNMarketoModule(reactContext));

           return modules;    
   }

   @NonNull
   @Override
   public List createViewManagers(@NonNull ReactApplicationContext reactContext) {
       return Collections.emptyList();
   }
}
```

Para concluir o registro do pacote, adicione o MarketoPluginPackage à lista de pacotes do React na Classe do Aplicativo:

```
public class MainApplication extends Application implements ReactApplication {
 
  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
          return BuildConfig.DEBUG;
        }
 
        @Override
        protected List getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List packages = new PackageList(this).getPackages();
          packages.add(new MarketoPluginPackage());     //Add the Marketo Package here.
          // Packages that cannot be autolinked yet can be added manually here, for example:
          return packages;
        }
}
```

### iOS

No guia a seguir, você criará um módulo nativo, _RNMarketoModule_, que permitirá acessar as APIs do Marketo a partir do JavaScript.

Para começar, abra o projeto do iOS no aplicativo React Native no Xcode. Você pode encontrar seu projeto do iOS aqui em um aplicativo React Native. Recomendamos o uso do Xcode para escrever seu código nativo. O Xcode é criado para desenvolvimento em iOS e usá-lo ajudará você a resolver rapidamente erros menores, como a sintaxe de código.

Crie nosso cabeçalho de módulo nativo personalizado principal e arquivos de implementação. Crie um novo arquivo chamado `MktoBridge.h` e adicione o seguinte a ele:

```
//
//  MktoBridge.h
//
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject 

@end

NS_ASSUME_NONNULL_END
```

Crie o arquivo de implementação correspondente, MktoBridge.m, na mesma pasta e inclua o seguinte conteúdo:

```
//
//  MktoBridge.m
//
//  Created by Marketo, An Adobe company.
//

#import "MktoBridge.h"
#import <MarketoFramework/MarketoFramework.h>
#import <React/RCTBridge.h>
#import "ConstantStringsHeader.h"

@implementation MktoBridge

RCT_EXPORT_MODULE(RNMarketoModule);

+(BOOL)requiresMainQueueSetup{
  return NO;
}

RCT_EXPORT_METHOD(initializeWithMunchkin:(NSString *) munchkinId Secret: (NSString *) secretKey andFrameworkType : (NSString *) frameworkType{
  [[Marketo sharedInstance] initializeWithMunchkinID:munchkinId appSecret:secretKey  mobileFrameworkType:frameworktype launchOptions:nil];
}

RCT_EXPORT_METHOD(reportAction:(NSString *)actionName withMetaData:(NSDictionary *)metaData){
  MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
  [meta setType:[metaData objectForKey:KEY_ACTION_TYPE]];
  [meta setDetails:[metaData objectForKey:KEY_ACTION_DETAILS]];
  [meta setLength:[metaData valueForKey:KEY_ACTION_LENGTH]];
  [meta setMetric:[metaData valueForKey:KEY_ACTION_METRIC]];
  [[Marketo sharedInstance] reportAction:actionName withMetaData:meta];
}

RCT_EXPORT_METHOD(associateLead:(NSDictionary *)leadDetails){
  MarketoLead *lead = [[MarketoLead alloc] init];
  if ([leadDetails objectForKey:KEY_EMAIL] != nil) {
    [lead setEmail:[leadDetails objectForKey:KEY_EMAIL]];
  }
  if ([leadDetails objectForKey:KEY_FIRST_NAME] != nil) {
    [lead setFirstName:[leadDetails objectForKey:KEY_FIRST_NAME]];
  }
  
  if ([leadDetails objectForKey:KEY_LAST_NAME] != nil) {
    [lead setLastName:[leadDetails objectForKey:KEY_LAST_NAME]];
  }
  
  if ([leadDetails objectForKey:KEY_CITY] != nil) {
    [lead setCity:[leadDetails objectForKey:KEY_CITY]];
  }
    [[Marketo sharedInstance] associateLead:lead];
}

RCT_EXPORT_METHOD(uninitializeMarketoPush){
  [[Marketo sharedInstance] unregisterPushDeviceToken];
}

RCT_EXPORT_METHOD(reportAll){
  [[Marketo sharedInstance] reportAll];
}

RCT_EXPORT_METHOD(setSecureSignature:(NSDictionary *)secureSignature){
  MKTSecuritySignature *secSignature = [[MKTSecuritySignature alloc]
                                        initWithAccessKey:[secureSignature objectForKey:KEY_ACCESSKEY]
                                        signature:[secureSignature objectForKey:KEY_SIGNATURE]
                                        timestamp: [secureSignature objectForKey:KEY_EMAIL]
                                        email:[secureSignature objectForKey:KEY_EMAIL]];
  
    [[Marketo sharedInstance] setSecureSignature:secSignature];
}
@end
```

#### Inicializar o SDK do Marketo

Localize um local no aplicativo em que você deseje adicionar uma chamada ao método createCalendarEvent() do módulo nativo. Abaixo está um exemplo de um componente, NewModuleButton, que pode ser adicionado ao seu aplicativo. Você pode chamar o módulo nativo dentro da função onPress() de NewModuleButton.

```
import React from 'react';
import { NativeModules, Button } from 'react-native';

const NewModuleButton = () => {
  const onPress = () => {
    console.log('We will invoke the native module here!');
  };

  return (
    
  );
  };

export default NewModuleButton;
```

Esse arquivo JavaScript carrega o módulo nativo na camada JavaScript.

```javascript
import React from 'react';
import {Node} from 'react';
import { NativeModules } from 'react-native';

const { RNMarketoModule } = NativeModules;
```

Depois que os arquivos acima forem colocados corretamente, poderemos importar o módulo js em qualquer classe js e chamar seus métodos diretamente. Por exemplo:

Observe que devemos transmitir &quot;reactNative&quot; como tipo de estrutura para os aplicativos nativos React. 

```
// Initialize marketo SDK with Munchkin & Seretkey you have from step 1.
RNMarketoModule.initializeSDK("MunchkinID","SecreteKEY","FrameworkType")

//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})

//You can report any user performed action by calling the reportaction function.
RNMarketoModule.reportAction("Bought Shirt", {actionType:"Shopping", actionDetails: "Red Shirt", setLength : 20, setMetric : 30 })

//You can set Secure Signature by calling this method.
RNMarketoModule.setSecureSignature({accessKey: "Key102", email: "testleadrk@001.com", signature : "asdfghjkloiuytds", timeStamp: "12345678987654"})

//This function will Enable user notifications (Only Android)
RNMarketoModule.initializeMarketoPush("350312872033", "MKTO")

//The token can also be unregistered on logout.
RNMarketoModule.uninitializeMarketoPush()
```

#### Configurar notificações por push


Inicializar push com ID do projeto e nome do canal

```
RNMarketoModule.initializeMarketoPush("ProjectId", "Channel_name")
```

Adicionar o seguinte serviço a `AndroidManifest.xml`


```xml
<service android:exported="true" android:name=".MyFirebaseMessagingService" android:stopWithTask="true">
    <intent-filter>
        <action  android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter/>
    <intent-filter> 
        <action android:name="com.google.firebase.MESSAGING_EVENT"/> 
    </intent-filter/>
</activity/>
```

Criar uma classe com nome `FirebaseMessagingService.java` e Adicione o seguinte código

```java
import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;
import com.marketo.Marketo;

public class MyFirebaseMessagingService extends FirebaseMessagingService {

   @Override
   public void onNewToken(String token) {
       super.onNewToken(token);
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.setPushNotificationToken(token);
   }

   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.showPushNotification(remoteMessage);
   }
}
```

As permissões devem ser ativadas no projeto Xcode para enviar notificações por push para o dispositivo do usuário.

Para enviar notificações por push, [adicionar notificações por push](push-notifications.md).

Agora no seu `AppDelegate.m` arquivo no XCode, importar Marketo

```
#import <MarketoFramework/MarketoFramework.h> 
```

Adicionar `UNUserNotificationCenterDelegate` à Interface AppDelegate da seguinte maneira para lidar com Representantes

```
@interface AppDelegate () <UNUserNotificationCenterDelegate>

@end
```

Registrar para notificações remotas no `didFinishLaunchingWithOptions` método.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
#ifdef FB_SONARKIT_ENABLED
  InitializeFlipper(application);
#endif

  RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                   moduleName:@"HelloRN"
                                            initialProperties:nil];  
  
// asking user permission to send push notifications 
  [self registerForRemoteNotifications];
  
  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];

  return YES;
}

- (void)registerForRemoteNotifications {
   UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        center.delegate = self;
        [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
            if(!error){
                dispatch_async(dispatch_get_main_queue(), ^{
                    [[UIApplication sharedApplication] registerForRemoteNotifications];
                });
            }
            else{
                NSLog(@"failed");
            }
        }];
}
```

Incluir o seguinte `UNUserNotificationCenter` delegar notificações necessárias delegar métodos.

```
-(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{
    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}

- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}

- (void)applicationWillTerminate:(UIApplication *)application {
    [[Marketo sharedInstance] unregisterPushDeviceToken];
}

-(void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error{
    NSLog(@"didFailToRegisterForRemoteNotificationsWithError");
}
```

### Adicionar dispositivos de teste

**Android**

Adicionar &quot;MarketoActivity&quot; a `AndroidManifest.xml` arquivo dentro da tag do aplicativo.

```xml
<activity android:name="com.marketo.MarketoActivity" android:configChanges="orientation|screenSize" android:exported="true">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

**iOS**

1. Selecione Projeto > Destino > Informações > Tipos de URL.

1. Adicionar identificador: ${PRODUCT_NAME}

1. Definir esquemas de URL: `mkto-<S_ecret Key_>`

1. Incluir `application:openURL:sourceApplication:annotation:` para `AppDelegate.m` arquivo (Objetive-C)

**iOS - Manipular tipo/deeplinks de URL personalizado no AppDelegate** 

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options{
   
    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];    
}
```

Essas constantes são usadas ao chamar a API do javascript. Você deve criar arquivos constantes e adicionar o seguinte.

```
// Lead attributes.
static NSString *const KEY_FIRST_NAME = @"firstName";
static NSString *const KEY_LAST_NAME = @"lastName";
static NSString *const KEY_ADDRESS = @"address";
static NSString *const KEY_CITY = @"city";
static NSString *const KEY_STATE = @"state";
static NSString *const KEY_COUNTRY = @"country";
static NSString *const KEY_GENDER = @"gender";
static NSString *const KEY_EMAIL = @"email";
static NSString *const KEY_TWITTER = @"twitterId";
static NSString *const KEY_FACEBOOK = @"facebookId";
static NSString *const KEY_LINKEDIN = @"linkedinId";
static NSString *const KEY_LEAD_SOURCE = @"leadSource";
static NSString *const KEY_BIRTHDAY = @"dateOfBirth";
static NSString *const KEY_FACEBOOK_PROFILE_URL = @"facebookProfileURL";
static NSString *const KEY_FACEBOOK_PROFILE_PIC = @"facebookPhotoURL";

// Custom actions
static NSString *const KEY_ACTION_TYPE = @"actionType";
static NSString *const KEY_ACTION_DETAILS = @"actionDetails";
static NSString *const KEY_ACTION_LENGTH = @"setLength";
static NSString *const KEY_ACTION_METRIC = @"setMetric";

//Secure Signature
static NSString *const KEY_ACCESSKEY = @"accessKey";
static NSString *const KEY_SIGNATURE = @"signature";
static NSString *const KEY_TIMESTAMP = @"timeStamp";
```

Exemplo de uso

```
//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})
```
