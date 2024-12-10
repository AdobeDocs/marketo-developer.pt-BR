---
title: API SOAP
feature: SOAP
description: Visão geral do Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# API SOAP

A API do SOAP está sendo descontinuada e não estará mais disponível após 31 de outubro de 2025.  Todos os novos desenvolvimentos devem ser feitos com a API [REST](https://developer.adobe.com/marketo-apis/) do Marketo, e os serviços existentes devem ser migrados até essa data para evitar interrupções no serviço.

## WSDL SOAP

Para recuperar o documento WSDL SOAP, obtenha o Ponto de Extremidade da API SOAP do menu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]**.

![Ponto de extremidade SOAP](assets/endpoint-soap.png)

Seu URL WSDL é:

`<SOAP API Endpoint> + ?WSDL`

Não use o ponto final definido no WSDL. Cada instância do Marketo tem um ponto de extremidade exclusivo para a qual fazer chamadas.

## Limites

- **Cota Diária:** A maioria das assinaturas recebe 10.000 chamadas de API por dia (que são redefinidas diariamente às 12h00 CST). Você pode aumentar sua cota diária por meio do gerente da conta.
- **Limite de Taxa:** Acesso à API por instância limitado a 100 chamadas por 20 segundos.
- **Limite de simultaneidade:**  Máximo de dez chamadas de API simultâneas.

Nossa recomendação é que os tamanhos dos lotes não sejam maiores que 300. Não há suporte para tamanhos maiores e isso pode resultar em tempos limite e, em casos extremos, em limitação.

## Configurações da API do SOAP no Marketo

1. Vá para a seção **[!UICONTROL Admin]** e clique em **[!UICONTROL Serviços da Web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Defina uma [!UICONTROL Chave de Criptografia] apropriada, clique em **[!UICONTROL Salvar Alterações]** e use os valores de API de SOAP [!UICONTROL Ponto de Extremidade], [!UICONTROL ID de Usuário] e [!UICONTROL Chave de Criptografia] para gerar a [assinatura de autenticação](authentication-signature.md) correta para cada chamada de API de SOAP.

![admin-web-services3](assets/admin-web-services3.png)
