---
title: API SOAP
feature: SOAP
description: Visão geral do Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 1%

---

# API SOAP

A API do SOAP está sendo substituída e não estará mais disponível após 31 de outubro de 2025. Todos os novos desenvolvimentos devem ser feitos com a [REST API](../rest-api/rest-api.md) do Marketo, e os serviços existentes devem ser migrados até essa data para evitar interrupções no serviço. Se você tiver um serviço que usa a API do SOAP, consulte o [Guia de Migração](./migration.md) da API do SOAP para obter informações sobre como migrar.

## WSDL do SOAP

Para recuperar o documento WSDL do SOAP, obtenha o Ponto de Extremidade da API do SOAP do menu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]**.

![Ponto de Extremidade do SOAP](assets/endpoint-soap.png)

Seu URL WSDL é:

`<SOAP API Endpoint> + ?WSDL`

Não use o ponto final definido no WSDL. Cada instância do Marketo tem um ponto de extremidade exclusivo para a qual fazer chamadas.

## Limites

- **Cota Diária:** A maioria das assinaturas recebe 10.000 chamadas de API por dia (o que é redefinido diariamente a uma CST de 12:00AM). Você pode aumentar sua cota diária por meio do gerente da conta.
- **Limite de Taxa:** Acesso à API por instância limitado a 100 chamadas por 20 segundos.
- **Limite de simultaneidade:**  Máximo de dez chamadas de API simultâneas.

Nossa recomendação é que os tamanhos dos lotes não sejam maiores que 300. Não há suporte para tamanhos maiores e isso pode resultar em tempos limite e, em casos extremos, em limitação.

## Configurações da API do SOAP no Marketo

1. Vá para a seção **[!UICONTROL Admin]** e clique em **[!UICONTROL Serviços da Web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Defina uma [!UICONTROL Chave de Criptografia] apropriada, clique em **[!UICONTROL Salvar Alterações]** e use os valores de [!UICONTROL Ponto de Extremidade], [!UICONTROL ID de Usuário] e [!UICONTROL Chave de Criptografia] da API do SOAP para gerar a [assinatura de autenticação](authentication-signature.md) correta para cada chamada de API do SOAP.

![admin-web-services3](assets/admin-web-services3.png)
