---
title: " Instalação da Extensão do [!DNLAadobe Launch]"
feature: "Mobile Marketing"
description: "[!DNL Adobe Launch] visão geral da instalação da extensão"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# [!DNL Adobe Launch] Instalação da extensão

Instruções de instalação para [!DNL Adobe Launch] Extensão do Marketo. As etapas abaixo são necessárias para enviar notificações por push e/ou mensagens no aplicativo.

## Pré-requisitos

1. [Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenha a chave secreta e a ID do Munchkin do seu aplicativo)
1. [Configurar a propriedade no [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configure a chave secreta do aplicativo e a ID do Munchkin para a propriedade na [!DNL Adobe Launch] portal
1. [Configurar notificações por push](push-notifications.md) (opcional)

## Como instalar a extensão do Marketo no iOS

### Configurar cabeçalho de ponte Swift

1. Vá para Arquivo > Novo > Arquivo e selecione &quot;Arquivo de cabeçalho&quot;.

1. Nomeie o arquivo &quot;&lt;_NomeProjeto_>-Bridging-Header&quot;.

1. Acesse Projeto > Target > Configurações de build > Compilador Swift > Geração de código. Adicione o seguinte caminho ao cabeçalho &quot;Objetive-Bridging&quot;:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inicializar extensão

>[!BEGINTABS]

>[!TAB Objetivo C]

Atualize o `applicationDidBecomeActive` como abaixo

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Atualize o `applicationDidBecomeActive` como abaixo

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivos de teste iOS

1. Selecionar [!UICONTROL Projeto] > [!UICONTROL Target] > [!UICONTROL Informações] > Tipos de URL.
1. Adicionar identificador: ${PRODUCT_NAME}
1. Definir esquemas de URL: mkto-&lt;s_ecret key_=&quot;&quot;>
1. Incluir `application:openURL:sourceApplication:annotation:` para `AppDelegate.m file` (Objetivo-C)

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

## Como instalar o SDK do Marketo no Android

### Configuração de extensão do Android

Siga as instruções em [!DNL Adobe Launch] portal

### Configurar permissões

Abertura `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se seu aplicativo já solicitar essas permissões, ignore esta etapa.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inicializar extensão

Configuração do ProGuard (opcional)

Se você estiver usando o ProGuard para o seu aplicativo, adicione as seguintes linhas no `proguard.cfg` arquivo. O arquivo está localizado em seu `project` pasta. A adição desse código exclui o Marketo SDK do processo de ofuscação.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Dispositivos de teste Android

Adicionar &quot;MarketoActivity&quot; a `AndroidManifest.xml` dentro da tag do aplicativo.

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

O MME Software Development Kit (SDK) para Android foi atualizado para uma estrutura mais moderna, estável e escalável, que contém mais flexibilidade e novos recursos de engenharia para o desenvolvedor de aplicativos do Android.

Google Os desenvolvedores de aplicativos com Android agora podem usar diretamente o [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) com este SDK.

### Adicionar o FCM ao aplicativo

1. Integre o SDK do Marketo Android mais recente ao aplicativo Android.  As etapas estão disponíveis em [GitHub](https://github.com/Marketo/android-sdk).
1. Configurar o aplicativo Firebase no console do Firebase.
   1. Criar/adicionar um projeto em [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Console Firebase.
      1. No [Console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), selecione [!UICONTROL Adicionar projeto].
      1. Selecione o projeto GCM na lista de projetos existentes do Google Cloud e selecione [!UICONTROL Adicionar Firebase].
      1. Na tela de boas-vindas do Firebase, selecione &quot;Adicionar o Firebase ao aplicativo Android&quot;.
      1. Forneça o nome do pacote e o SHA-1 e selecione [!UICONTROL Adicionar aplicativo]. Um novo `google-services.json` o arquivo do aplicativo Firebase foi baixado.
      1. Selecionar [!UICONTROL Continuar] e siga as instruções detalhadas para adicionar o plug-in do Google Services no Android Studio.

   1. Navegue até &quot;Configurações do projeto&quot; na Visão geral do projeto
      1. Clique na guia &#39;Geral&#39;. Baixe o `google-services.json` arquivo.
      1. Clique na guia &quot;Cloud Messaging&quot;. Copie &quot;Chave do servidor&quot; e &quot;ID do remetente&quot;. Forneça essas &quot;Chave do servidor&quot; e &quot;ID do remetente&quot; à Marketo.
   1. Configurar alterações do FCM no aplicativo Android
      1. Alterne para a exibição Projeto no Android Studio para ver o diretório raiz do projeto
         1. Mover o baixado `google-services.json` arquivo no diretório raiz do módulo de aplicativo Android
         1. No nível do projeto `build.gradle` adicione o seguinte:

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

         1. Por fim, clique em &quot;[!UICONTROL Sincronizar agora]&quot; na barra que aparece na ID
   1. Editar o manifesto do aplicativo O SDK do FCM adiciona automaticamente todas as permissões necessárias e a funcionalidade do receptor necessária. Remova os seguintes elementos obsoletos (e possivelmente prejudiciais, pois podem causar duplicação de mensagem) do manifesto do seu aplicativo:

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

**P: Onde posso encontrar instruções para atualizar para a versão mais recente do SDK do MME?** As instruções podem ser encontradas no Site do Desenvolvedor do Marketo [AQUI](installation.md).

**P: A atualização para a versão mais recente do SDK exigirá que eu publique uma versão atualizada do aplicativo Android para meus usuários existentes?** Não.

**P: Como isso afeta os clientes MME existentes que publicaram aplicativos Android integrados ao Marketo Android SDK?** Eles podem migrar um aplicativo cliente GCM existente no Android para o Firebase Cloud Messaging (FCM) da seguinte maneira:

1. No [Console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), selecione [!UICONTROL Adicionar projeto].
1. Selecione o projeto GCM na lista de projetos existentes do Google Cloud e selecione **[!UICONTROL Adicionar Firebase]**.
1. Na tela de boas-vindas do Firebase, selecione **[!UICONTROL Adicionar o Firebase ao aplicativo Android]**.
1. Forneça o nome do pacote e o SHA-1 e selecione **[!UICONTROL Adicionar aplicativo]**. Um novo arquivo google-services.json para o
1. O aplicativo Firebase foi baixado.
1. Selecionar **[!UICONTROL Continuar]** e siga as instruções detalhadas para adicionar o plug-in do Google Services no Android Studio.

**P: Podemos direcionar os clientes potenciais criados usando o antigo SDK do Marketo que usava o aplicativo GCM?** Sim. Todos os clientes potenciais criados usando o SDK da Marketo podem ser direcionados para enviar as notificações por push.
