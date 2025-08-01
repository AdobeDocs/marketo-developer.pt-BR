---
title: Instalação da Extensão [!DNL Adobe Launch]
feature: Mobile Marketing
description: Visão geral da instalação da extensão do [!DNL Adobe Launch]
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---

# Instalação da Extensão [!DNL Adobe Launch]

Instruções de instalação para a extensão do Marketo [!DNL Adobe Launch]. As etapas abaixo são necessárias para enviar notificações por push e/ou mensagens no aplicativo.

## Pré-requisitos

1. [Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obter a Chave Secreta e a ID do Munchkin do aplicativo)
1. [Configurar a propriedade no [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configure a chave secreta do aplicativo e a Munchkin ID para a propriedade no portal [!DNL Adobe Launch]
1. [Configurar notificações por push](push-notifications.md) (opcional)

## Como instalar a extensão do Marketo no iOS

### Configurar cabeçalho de ponte Swift

1. Vá para [!UICONTROL Arquivo] > [!UICONTROL Novo] > [!UICONTROL Arquivo] e selecione **[!UICONTROL Arquivo de cabeçalho]**.

1. Nomeie o arquivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vá para [!UICONTROL Projeto] > [!UICONTROL Destino] > [!UICONTROL Configurações de Compilação] > [!UICONTROL Compilador Swift] > [!UICONTROL Geração de Código]. Adicione o seguinte caminho ao cabeçalho &quot;Objetive-Bridging&quot;:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inicializar extensão

>[!BEGINTABS]

>[!TAB Objetivo C]

Atualize o método `applicationDidBecomeActive` conforme abaixo

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Atualize o método `applicationDidBecomeActive` conforme abaixo

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivos de teste iOS

1. Selecione **[!UICONTROL Projeto]** > **[!UICONTROL Destino]** > **[!UICONTROL Informações]** > **[!UICONTROL Tipos de URL]**.
1. Adicionar identificador: ${PRODUCT_NAME}
1. Definir esquemas de URL: mkto-&lt;Chave_secreta_>
1. Incluir `application:openURL:sourceApplication:annotation:` a `AppDelegate.m file` (Objetive-C)

### Tratar tipo de URL personalizado no AppDelegate

>[!BEGINTABS]

>[!TAB Objetivo C]

```
#ifdef __IPHONE_10_0
-(BOOL)application:(UIApplication *)application
           openURL:(NSURL *)url
           options:(NSDictionary *)options{
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
#endif

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Como instalar o Marketo SDK no Android

### Configuração de extensão do Android

Siga as instruções no portal [!DNL Adobe Launch]

### Configurar permissões

Abra `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se seu aplicativo já solicitar essas permissões, ignore esta etapa.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inicializar extensão

Configuração do ProGuard (opcional)

Se você estiver usando o ProGuard no seu aplicativo, adicione as seguintes linhas no arquivo `proguard.cfg`. O arquivo está localizado na pasta `project`. A adição desse código exclui o Marketo SDK do processo de ofuscação.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Teste  Dispositivos

Adicione &quot;MarketoActivity&quot; a `AndroidManifest.xml` dentro da marca do aplicativo.

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
      1. No [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), selecione **[!UICONTROL Adicionar projeto]**.
      1. Selecione o projeto GCM na lista de projetos existentes do Google Cloud e selecione **[!UICONTROL Adicionar Firebase]**.
      1. Na tela de boas-vindas do Firebase, selecione **[!UICONTROL Adicionar o Firebase ao seu aplicativo Android]**.
      1. Forneça o nome do pacote e SHA-1 e selecione **[!UICONTROL Adicionar aplicativo]**. Um novo arquivo `google-services.json` para seu aplicativo Firebase está sendo baixado.
      1. Selecione **[!UICONTROL Continuar]** e siga as instruções detalhadas para adicionar o plug-in do Google Services no Android Studio.

   1. Navegue até **[!UICONTROL Configurações do projeto]** em [!UICONTROL Visão geral do projeto]
      1. Clique na guia **[!UICONTROL Geral]**. Baixe o arquivo `google-services.json`.
      1. Clique na guia **[!UICONTROL Cloud Messaging]**. Copiar [!UICONTROL Chave do Servidor] e [!UICONTROL ID do Remetente]. Forneça estas [!UICONTROL Chave do Servidor] e [!UICONTROL ID do Remetente] à Marketo.
   1. Configurar alterações no FCM no aplicativo do Android
      1. Alterne para a exibição Projeto no Android Studio para ver o diretório raiz do projeto
         1. Mova o arquivo `google-services.json` baixado para o diretório raiz do módulo de aplicativo do Android
         1. No nível de projeto `build.gradle` adicione o seguinte:

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

         1. Finalmente, clique em **[!UICONTROL Sincronizar agora]** na barra que aparece na ID
   1. Editar o manifesto do aplicativo O FCM SDK adiciona automaticamente todas as permissões necessárias e a funcionalidade necessária do receptor. Remova os seguintes elementos obsoletos (e possivelmente prejudiciais, pois podem causar duplicação de mensagem) do manifesto do seu aplicativo:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```


### Perguntas frequentes sobre o FCM

Perguntas frequentes sobre o suporte ao Firebase Cloud Messaging.

**P: Onde posso encontrar instruções para atualizar para a versão mais recente do MME SDK?As instruções** podem ser encontradas no Site do Desenvolvedor do Marketo [AQUI](installation.md).

**P: A atualização para a versão mais recente do SDK exigirá que eu publique uma versão atualizada do meu aplicativo Android para meus usuários existentes?** Não.

**P: Como isso afeta os clientes MME existentes que publicaram aplicativos Android integrados ao Marketo Android SDK?** Eles podem migrar um aplicativo cliente GCM existente no Android para o Firebase Cloud Messaging (FCM) da seguinte maneira:

1. No [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), selecione **[!UICONTROL Adicionar projeto]**.
1. Selecione o projeto GCM na lista de projetos existentes do Google Cloud e selecione **[!UICONTROL Adicionar Firebase]**.
1. Na tela de boas-vindas do Firebase, selecione **[!UICONTROL Adicionar o Firebase ao seu aplicativo Android]**.
1. Forneça o nome do pacote e SHA-1 e selecione **[!UICONTROL Adicionar aplicativo]**. Um novo arquivo google-services.json para o
1. O aplicativo Firebase foi baixado.
1. Selecione **[!UICONTROL Continuar]** e siga as instruções detalhadas para adicionar o plug-in do Google Services no Android Studio.

**P: Podemos segmentar os clientes em potencial criados usando o antigo Marketo SDK que usava o aplicativo GCM?** Sim. Todos os clientes potenciais criados com o Marketo SDK podem ser direcionados para envio de notificações por push.
