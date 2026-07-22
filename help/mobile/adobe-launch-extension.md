---
title: Extensão do Marketo Mobile para  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Instale e configure a extensão Marketo Mobile SDK no Adobe Launch para iOS e Android, incluindo a configuração para notificações por push e mensagens no aplicativo.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
TQID: https://experienceleague.adobe.com/Bk5GTnQjm6NDosl5Iw6TS-NRjH8owNRUKoE0mZ-H3pY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 303
ht-degree: 0%

---

# Extensão do Marketo Mobile para [!DNL Adobe Launch]

Instale a extensão Marketo Mobile SDK em [!DNL Adobe Launch] para enviar notificações por push, mensagens no aplicativo ou ambas.

## Pré-requisitos

- [Adicione um aplicativo ao Administrador do Marketo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e obtenha a Chave Secreta e a ID do Munchkin do aplicativo.
- Siga as instruções de instalação no portal [!DNL Adobe Launch].
- Opcional: [Configurar notificações por push](push-notifications.md).

## iOS

### Configurar cabeçalho de ponte Swift

1. Vá para Arquivo > Novo > Arquivo e selecione &quot;Arquivo de cabeçalho&quot;.
1. Nomeie o arquivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Vá para Projeto > Target > Criar fases > Compilador Swift > Geração de código.
1. Adicione o seguinte caminho ao Cabeçalho da ponte de objetivos:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Para Swift, remova a seguinte instrução import porque as etapas anteriores adicionam o cabeçalho de Bridging.

`import Marketo/ALMarketo`

### Dispositivos de teste iOS

Siga as instruções em [Adicionando dispositivos de teste do iOS](installation.md#ios_test_devices).

### Tratar tipo de URL personalizado no AppDelegate

Siga as [instruções da URL personalizada](installation.md#ios_test_devices).

### Configurar notificações por push no iOS

Siga as [instruções de notificação por push](push-notifications.md). Use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;.

## Android

### Configurar permissões

Abra `AndroidManifest.xml` e adicione as seguintes permissões. Seu aplicativo deve solicitar as permissões &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Ignore esta etapa se o aplicativo já as solicitar.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuração do ProGuard (opcional)

Se o aplicativo usar ProGuard, adicione as seguintes linhas ao arquivo `proguard.cfg` na pasta do projeto. Essa configuração exclui a SDK do Marketo da ofuscação.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Dispositivos de teste Android

Siga as instruções em [Dispositivos de teste da Android](installation.md#android_test_devices).

## Configurar notificações por push no Android

Siga as [instruções do Android Firebase Cloud Messaging](installation.md#android_firebase_cloud_messaging_support). Use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;.

Para configurar perfis de usuário, siga as [instruções do perfil de usuário](user-profiles.md). Para configurar ações personalizadas, siga as [instruções da ação personalizada](custom-actions.md#android_custom_action). Em ambos os conjuntos de instruções, use o nome de classe &quot;ALMarketo&quot; em vez de &quot;Marketo&quot;.
