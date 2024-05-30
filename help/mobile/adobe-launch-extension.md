---
title: "Extensão móvel do Marketo para [!DNL Adobe Launch]"
feature: Mobile Marketing
description: "Extensão móvel do Marketo para [!DNL Adobe Launch] visão geral"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# Extensão móvel do Marketo para [!DNL Adobe Launch]

Instruções de instalação da extensão SDK do Marketo Mobile no [!DNL Adobe Launch]. As etapas abaixo são necessárias para enviar notificações por push e/ou mensagens no aplicativo.

## Pré-requisitos

- [Adicionar um aplicativo no Administrador do Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenha a chave secreta e a ID do Munchkin do seu aplicativo)
- Siga as instruções fornecidas no [!DNL Adobe Launch] portal para instalação
- [Configurar notificações por push](push-notifications.md) (opcional)

## iOS

### Configurar cabeçalho de ponte Swift

1. Vá para Arquivo > Novo > Arquivo e selecione &quot;Arquivo de cabeçalho&quot;.
1. Nomeie o arquivo &quot;&lt;_NomeProjeto_>-Bridging-Header&quot;.
1. Vá para Projeto > Target > Criar fases > Compilador Swift > Geração de código. Adicione o seguinte caminho ao Cabeçalho da ponte de objetivos:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Para usuários Swift: remova a seguinte instrução import, pois o cabeçalho de ligação é adicionado nas etapas acima.

`import Marketo/ALMarketo`

### Dispositivos de teste iOS

Siga as instruções em [Adicionar dispositivos de teste do iOS](installation.md#ios_test_devices)

### Tratar tipo de URL personalizado no AppDelegate

Siga as instruções [aqui](installation.md#ios_test_devices)

### Configurar notificações por push no iOS

Siga as instruções [aqui](push-notifications.md) e use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;

## Android

### Configurar permissões

Abertura `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se seu aplicativo já solicitar essas permissões, ignore esta etapa.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuração do ProGuard (opcional)

Se você estiver usando o ProGuard para o seu aplicativo, adicione as seguintes linhas no `proguard.cfg` arquivo. O arquivo está localizado na pasta do projeto. A adição desse código exclui o Marketo SDK do processo de ofuscação.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Dispositivos de teste Android

Siga as instruções [aqui](installation.md#android_test_devices)

## Configurar notificações por push no Android

Siga as instruções [aqui](installation.md#android_firebase_cloud_messaging_support) e use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;

Para configurar perfis de usuário, siga as instruções [aqui](user-profiles.md) e para ações personalizadas, siga as instruções [aqui](custom-actions.md#android_custom_action). Nas instruções a seguir, use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;
