---
title: Instalação
feature: Mobile Marketing
description: Como instalar SDKs para dispositivos móveis Marketo
exl-id: e0b79d85-3509-46d2-a77d-cee211c5ec7f
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Instalação

Instruções de instalação para o Marketo Mobile SDK. As etapas abaixo são necessárias para enviar notificações por push e/ou mensagens no aplicativo.

## Instalar o Marketo SDK no iOS

### Pré-requisitos

1. [Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obter a Chave Secreta e a ID do Munchkin do aplicativo)
1. [Configurar notificações por push](push-notifications.md) (opcional)

### Instalar a estrutura via CocoaPods

1. Instale o CocoaPods. `$ sudo gem install cocoapods`
1. Altere o diretório para o diretório do projeto e crie um Podfile com padrões inteligentes. `$ pod init`
1. Abra o Podfile. `$ open -a Xcode Podfile`
1. Adicione a seguinte linha ao Podfile. `$ pod 'Marketo-iOS-SDK'`
1. Salve e feche o Podfile.
1. Baixe e instale o Marketo iOS SDK. `$ pod install`
1. Abra o espaço de trabalho no Xcode. `$ open App.xcworkspace`

### Instalar a estrutura usando o Gerenciador de pacotes Swift

1. Selecione seu projeto no Navegador de projetos e em &quot;Adicionar dependência de pacote&quot;, clique em &quot;+&quot; como mostrado abaixo:

   ![Adicionar dependência](assets/dependency-manager-add.png)

1. Adicionar o pacote do Marketo a partir deste repositório. Adicione este URL para este repositório: https://github.com/Marketo/ios-sdk.

   ![URL do repositório](assets/dependency-manager-url.png)

1. Agora adicione o conjunto de recursos conforme mostrado: Localize `MarketoFramework.XCframework` no navegador do projeto e abra-o no Localizador. Arraste e solte `MKTResources.bundle` para Copiar Recursos do Pacote.

### Configurar cabeçalho de ponte Swift

1. Vá para Arquivo > Novo > Arquivo e selecione &quot;Arquivo de cabeçalho&quot;.

   ![Selecione &quot;Arquivo de Cabeçalho&quot;](assets/choose-header-file.png)

1. Nomeie o arquivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vá para Projeto > Target > Criar fases > Compilador Swift > Geração de código. Adicione o seguinte caminho ao Cabeçalho da ponte de objetivos:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![Fases de Compilação](assets/build-phases.png)

## Inicializar o SDK

Antes de usar o Marketo iOS SDK, você deve inicializá-lo com sua ID de conta do Munchkin e a chave secreta do aplicativo. Você pode encontrar cada um desses na área Administrador do Marketo, abaixo de &quot;Aplicativos e dispositivos móveis&quot;.

1. Abra o arquivo AppDelegate.m (Objetive-C) ou o arquivo Bridging (Swift) e importe o arquivo de cabeçalho Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Cole o seguinte código dentro da função `application:didFinishLaunchingWithOptions`:.

   Observe que devemos transmitir &quot;nativo&quot; como tipo de estrutura para Aplicativos nativos.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Substitua `munkinAccountId` e `secretKey` acima usando sua &quot;ID de Conta da Munchkin&quot; e &quot;Chave Secreta&quot; encontradas na seção **[!UICONTROL Admin]** > **[!UICONTROL Aplicativos e Dispositivos Móveis]** do Marketo.

## Dispositivos de teste iOS

1. Selecione Projeto > Destino > Informações > Tipos de URL.
1. Adicionar identificador: ${PRODUCT_NAME}
1. Definir Esquemas de URL: `mkto-<Secret Key_>`
1. Incluir aplicativo:openURL:sourceApplication:annotation: no arquivo AppDelegate.m (Objetive-C)

## Tratar tipo de URL personalizado no AppDelegate

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

>[!TAB Swift]

```
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Como instalar o Marketo SDK no Android

### Pré-requisitos

1. [Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obter a Chave Secreta e a ID do Munchkin do aplicativo)
1. [Configurar notificações por push](push-notifications.md#android_setup_push) (opcional)
1. [Baixar o Marketo SDK para Android](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Configuração do Android SDK com Gradle

1. No arquivo build.gradle do nível do aplicativo, na seção dependências, adicione

`implementation 'com.marketo:MarketoSDK:0.8.9'`

1. O arquivo raiz `build.gradle` deve ter

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Sincronizar seu projeto com arquivos Gradle

### Configurar permissões

Abra `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se seu aplicativo já solicitar essas permissões, ignore esta etapa.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Inicializar o SDK

1. Abra a classe Aplicativo ou Atividade no aplicativo e importe o Marketo SDK para sua Atividade antes de setContentView ou no Contexto do aplicativo.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. Configuração do ProGuard (opcional)

   Se você estiver usando o ProGuard no seu aplicativo, adicione as seguintes linhas no arquivo `proguard.cfg`. O arquivo está localizado na pasta do projeto. A adição desse código exclui o Marketo SDK do processo de ofuscação.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
   ```

## Dispositivos de teste Android

Adicionar &quot;MarketoActivity&quot; ao arquivo `AndroidManifest.xml` dentro da marca do aplicativo.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize" >
    <intent-filter android:label="MarketoActivity" >
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto" />
    </intent-filter>
</activity>
```

## Suporte a Firebase Cloud Messaging

O MME Software Development Kit (SDK) for Android foi atualizado para uma estrutura mais moderna, estável e escalável, que contém mais flexibilidade e novos recursos de engenharia para o desenvolvedor de aplicativos Android.

Os desenvolvedores de aplicativos do Android agora podem usar diretamente o [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) da Google com este SDK.

### Adicionar o FCM ao aplicativo

1. Integre o Marketo Android SDK mais recente ao aplicativo do Android.  As etapas estão disponíveis em [GitHub](https://github.com/Marketo/android-sdk).
1. Configurar o aplicativo Firebase no console do Firebase.
   1. Criar/adicionar um projeto no [](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Console Firebase.
      1. No [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), selecione `Add Project`.
      1. Selecione o projeto GCM na lista de projetos existentes da Google Cloud e selecione `Add Firebase`.
      1. Na tela de boas-vindas do Firebase, selecione `Add Firebase to your Android App`.
      1. Forneça o nome do pacote e SHA-1 e selecione `Add App`. Um novo arquivo `google-services.json` para seu aplicativo Firebase está sendo baixado.
      1. Selecione `Continue` e siga as instruções detalhadas para adicionar o plug-in do Google Services no Android Studio.

   1. Navegue até &quot;Configurações do projeto&quot; na Visão geral do projeto
      1. Clique na guia &#39;Geral&#39;. Baixe o arquivo &quot;google-services.json&quot;.
      1. Clique na guia &quot;Cloud Messaging&quot;. Copie &quot;Chave do servidor&quot; e &quot;ID do remetente&quot;. Forneça essas &quot;Chave do servidor&quot; e &quot;ID do remetente&quot; à Marketo.
   1. Configurar alterações no FCM no aplicativo do Android
      1. Alterne para a exibição Projeto no Android Studio para ver o diretório raiz do projeto
         1. Mova o arquivo &quot;google-services.json&quot; baixado para o diretório raiz do módulo do aplicativo Android
         1. Em build.gradle no nível do projeto, adicione o seguinte:

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. Em build.gradle no nível do aplicativo, adicione o seguinte:

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            }
            // Add to the bottom of the file
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Por fim, clique em &quot;Sincronizar agora&quot; na barra exibida na ID
   1. Editar o manifesto do aplicativo O FCM SDK adiciona automaticamente todas as permissões necessárias e a funcionalidade necessária do receptor. Remova os seguintes elementos obsoletos (e possivelmente prejudiciais, pois podem causar duplicação de mensagem) do manifesto do seu aplicativo:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND"
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```
