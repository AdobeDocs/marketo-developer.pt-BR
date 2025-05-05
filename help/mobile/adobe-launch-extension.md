---
title: Extensão do Marketo Mobile para  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Visão geral da extensão do Marketo Mobile para  [!DNL Adobe Launch]
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---

# Extensão do Marketo Mobile para [!DNL Adobe Launch]

Instruções de instalação da extensão SDK do Marketo Mobile em [!DNL Adobe Launch]. As etapas abaixo são necessárias para enviar notificações por push e/ou mensagens no aplicativo.

## Pré-requisitos

- [Adicionar um aplicativo no Marketo Admin](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenha a Chave Secreta e a ID do Munchkin do aplicativo)
- Siga as instruções fornecidas no portal [!DNL Adobe Launch] para instalação
- [Configurar notificações por push](push-notifications.md) (opcional)

## iOS

### Configurar cabeçalho de ponte Swift

1. Vá para Arquivo > Novo > Arquivo e selecione &quot;Arquivo de cabeçalho&quot;.
1. Nomeie o arquivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Vá para Projeto > Target > Criar fases > Compilador Swift > Geração de código. Adicione o seguinte caminho ao Cabeçalho da ponte de objetivos:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Para usuários Swift: remova a seguinte instrução import, pois o cabeçalho de ligação é adicionado nas etapas acima.

`import Marketo/ALMarketo`

### Dispositivos de teste iOS

Siga as instruções em [Adicionando dispositivos de teste do iOS](installation.md#ios_test_devices)

### Tratar tipo de URL personalizado no AppDelegate

Siga as instruções [aqui](installation.md#ios_test_devices)

### Configurar notificações por push no iOS

Siga as instruções [aqui](push-notifications.md) e use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;

## Android

### Configurar permissões

Abra `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se seu aplicativo já solicitar essas permissões, ignore esta etapa.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuração do ProGuard (opcional)

Se você estiver usando o ProGuard no seu aplicativo, adicione as seguintes linhas no arquivo `proguard.cfg`. O arquivo está localizado na pasta do projeto. A adição desse código exclui o Marketo SDK do processo de ofuscação.

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
