---
title: "Mensagens no aplicativo"
feature: "Mobile Marketing"
description: "Visão geral das mensagens no aplicativo"
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 1%

---


# Mensagens internas do aplicativo

Para usar os recursos de mensagens no aplicativo do Marketo, você deve executar as seguintes etapas:

1. Instale o SDK do Marketo Mobile conforme descrito na seção [Instalação móvel](installation.md).
1. Adicione seu aplicativo móvel ao Marketo, conforme descrito em [Adicionar um aplicativo móvel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Opcionalmente, adicione o código ao aplicativo móvel para capturar [Ações Personalizadas](custom-actions.md).

Depois de instalar o Marketo Mobile SDK e concluir a adição do aplicativo no Marketo, você estará pronto para enviar mensagens no aplicativo que são exibidas quando um usuário abre o aplicativo.

Por padrão, as mensagens no aplicativo são acionadas quando o aplicativo é aberto. Se você quiser acionar mensagens no aplicativo para outros eventos, como quando uma página específica é exibida ou quando um botão específico é pressionado, é necessário adicionar ações personalizadas ao código. Consulte [Ações Personalizadas](custom-actions.md) para amostras de código deste.

## Solução de problemas

**A mensagem no aplicativo não é exibida**

A Marketo responde aos acionadores dos aplicativos somente após o SDK do Marketo Mobile ser inicializado com a plataforma Marketo. O processo de inicialização ocorre ao instalar e abrir o aplicativo pela primeira vez. Como a inicialização ocorre após a primeira abertura do aplicativo, o evento &quot;Abertura de aplicativo&quot; não é acionado até que o aplicativo seja aberto uma segunda vez. Feche o aplicativo e abra-o novamente, e uma mensagem acionada pela opção Abrir aplicativo deve ser exibida em seu dispositivo.

Os eventos personalizados são acionados pela interação do usuário após a abertura do aplicativo. Os eventos personalizados são reconhecidos pela Marketo durante a primeira sessão.

**Rastreamento de atividade de toque no aplicativo**

Atribua uma ação além de &quot;dispensar&quot; a um dos botões primário ou secundário para rastrear atividades de toque e usar frequências de exibição base com base no número de toques.

Para obter informações adicionais, consulte a seção [Mensagens no aplicativo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) seção na documentação do produto.
