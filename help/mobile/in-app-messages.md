---
title: Mensagens no aplicativo
feature: Mobile Marketing
description: Configure mensagens no aplicativo do Marketo com o Mobile SDK, configure acionadores de evento personalizados, rastreie a atividade de toque e corrija os problemas de primeira inicialização de abertura do aplicativo.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 1%

---

# Mensagens no aplicativo

Para usar os recursos de mensagens no aplicativo do Marketo, você deve executar as seguintes etapas:

1. Instale o Marketo Mobile SDK conforme descrito em [Instalação móvel](installation.md).
1. Adicione seu aplicativo móvel ao Marketo conforme descrito em [Adicionar um aplicativo móvel](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Opcionalmente, adicione o código ao seu aplicativo móvel para capturar [Ações personalizadas](custom-actions.md).

Depois de instalar o Marketo Mobile SDK e concluir a adição do aplicativo no Marketo, você estará pronto para enviar mensagens no aplicativo que são exibidas quando um usuário abre o aplicativo.

Por padrão, as mensagens no aplicativo são acionadas quando o aplicativo é aberto. Se você quiser acionar mensagens no aplicativo para outros eventos, como quando uma página específica é exibida ou quando um botão específico é pressionado, é necessário adicionar ações personalizadas ao código. Consulte a seção [Ações Personalizadas](custom-actions.md) para obter exemplos de código.

## Solução de problemas

**A mensagem no aplicativo não está aparecendo**

A Marketo responde aos acionadores dos aplicativos somente depois que o Marketo Mobile SDK é inicializado com a plataforma Marketo. O processo de inicialização ocorre ao instalar e abrir o aplicativo pela primeira vez. Como a inicialização ocorre após a primeira abertura do aplicativo, o evento &quot;Abertura de aplicativo&quot; não é acionado até que o aplicativo seja aberto uma segunda vez. Feche o aplicativo e abra-o novamente, e uma mensagem acionada pela opção Abrir aplicativo deve ser exibida em seu dispositivo.

Os eventos personalizados são acionados pela interação do usuário após a abertura do aplicativo. Os eventos personalizados são reconhecidos pela Marketo durante a primeira sessão.

**Rastreamento De Atividade De Toque No Aplicativo**

Atribua uma ação além de &quot;dispensar&quot; a um dos botões primário ou secundário para rastrear atividades de toque e usar frequências de exibição base com base no número de toques.

Para obter informações adicionais, consulte a seção [Mensagens no aplicativo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) na documentação do produto.
