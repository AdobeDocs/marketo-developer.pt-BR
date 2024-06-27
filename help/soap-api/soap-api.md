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

A API do SOAP não está mais em desenvolvimento ativo. As chamadas ainda funcionam, mas nosso desenvolvimento está focado em [REST](https://developer.adobe.com/marketo-apis/) daqui para frente.

A API SOAP do Marketo permite a criação, a recuperação e a remoção de entidades e dados armazenados no Marketo. Você pode encontrar o [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) no GitHub. Há também [bibliotecas de clientes](https://github.com/Marketo/Community-Supported-Client-Libraries) para poupar tempo.

Versão mais recente da API: 3_1

## WSDL SOAP

Para recuperar o documento WSDL do SOAP, obtenha seu endpoint da API SOAP de seu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]** menu.

![Endpoint SOAP](assets/endpoint-soap.png)

Seu URL WSDL é:

`<SOAP API Endpoint> + ?WSDL`

Não use o ponto final definido no WSDL. Cada instância do Marketo tem um ponto de extremidade exclusivo para a qual fazer chamadas.

## Limites

- **Cota Diária:** A maioria das assinaturas recebe 10.000 chamadas de API por dia (que são redefinidas diariamente às 12h00, horário padrão da região central dos EUA). Você pode aumentar sua cota diária por meio do gerente da conta.
- **Limite de taxa:** Acesso à API por instância limitado a 100 chamadas por 20 segundos.
- **Limite de simultaneidade:**  Máximo de dez chamadas de API simultâneas.

Nossa recomendação é que os tamanhos dos lotes não sejam maiores que 300. Não há suporte para tamanhos maiores e isso pode resultar em tempos limite e, em casos extremos, em limitação.

## Configurações da API do SOAP no Marketo

1. Vá para a **[!UICONTROL Admin]** e clique em **[!UICONTROL Serviços da Web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Defina um apropriado [!UICONTROL Chave de criptografia], clique em **[!UICONTROL Salvar alterações]** e usar a API do SOAP [!UICONTROL Endpoint], [!UICONTROL ID de usuário], e [!UICONTROL Chave de criptografia] valores para gerar a correta [assinatura de autenticação](authentication-signature.md) para cada chamada de API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
