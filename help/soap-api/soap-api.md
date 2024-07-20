---
title: API SOAP
feature: SOAP
description: Visão geral do Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# API SOAP

A API do SOAP não está mais em desenvolvimento ativo. As chamadas ainda funcionam, mas nosso desenvolvimento está focado em [REST](https://developer.adobe.com/marketo-apis/) a partir de agora.

A API SOAP do Marketo permite a criação, a recuperação e a remoção de entidades e dados armazenados no Marketo. Você pode encontrar o [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) no GitHub. Também existem [bibliotecas de clientes](https://github.com/Marketo/Community-Supported-Client-Libraries) para economizar seu tempo.

Versão mais recente da API: 3_1

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
