---
title: "REST API"
feature: REST API
description: "Visão geral da REST API"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 1%

---


# API REST

O Marketo expõe uma API REST que permite a execução remota de muitos dos recursos do sistema. Desde a criação de programas até a importação de leads em massa, há muitas opções que permitem o controle refinado de uma instância do Marketo.

Essas APIs geralmente se dividem em duas categorias amplas: [Banco de dados de clientes potenciais](https://developer.adobe.com/marketo-apis/api/mapi/), e [Ativo](https://developer.adobe.com/marketo-apis/api/asset/). As APIs de banco de dados de clientes potenciais permitem a recuperação e a interação com registros de pessoas da Marketo e tipos de objetos associados, como Oportunidades e Empresas. As APIs de ativos permitem a interação com material de apoio de marketing e registros relacionados a fluxos de trabalho.

- **Cota Diária:** As assinaturas recebem 50.000 chamadas de API por dia (que são redefinidas diariamente às 12h00, horário padrão da região central dos Estados Unidos). Você pode aumentar sua cota diária por meio do gerente da conta.
- **Limite de taxa:** Acesso à API por instância limitado a 100 chamadas por 20 segundos.
- **Limite de simultaneidade:**  Máximo de dez chamadas de API simultâneas.

O tamanho das chamadas padrão é limitado a um comprimento de URI de 8 KB e um tamanho de corpo de 1 MB, embora o corpo possa ser de 10 MB para nossas APIs em massa. Se houver um erro na sua chamada, a API normalmente ainda retornará um código de status 200, mas a resposta JSON conterá um membro &quot;success&quot; com um valor de `false`e uma matriz de erros no membro &quot;erros&quot;. Mais sobre erros [aqui](error-codes.md).

## Introdução

As etapas a seguir exigem privilégios de administrador na instância do Marketo.

Na primeira chamada para o Marketo, você recuperará um registro de lead. Para começar a trabalhar com o Marketo, você deve obter credenciais de API para fazer chamadas autenticadas para sua instância. Faça logon na sua instância e acesse o **[!UICONTROL Admin]** -> **[!UICONTROL Usuários e funções]**.

![Usuários e funções do administrador](assets/admin-users-and-roles.png)

Clique em [!UICONTROL Funções] e, em seguida, Nova função e atribua pelo menos a permissão &quot;Cliente potencial somente leitura&quot; (ou &quot;Pessoa somente leitura&quot;) à função no grupo de APIs de acesso. Atribua um nome descritivo e clique em [!UICONTROL Criar].

![Nova Função](assets/new-role.png)

Agora, volte para a guia Usuários e clique em Convidar novo usuário. Dê ao usuário um nome descritivo que indique que ele é um usuário da API e um endereço de email e clique em **[!UICONTROL Próxima]**.

![Novas informações do usuário](assets/new-user-info.png)

Em seguida, marque a opção Somente API e conceda ao usuário a função API que você criou e clique em **[!UICONTROL Próxima]**.

![Novas permissões de usuário](assets/new-user-permissions.png)

Para concluir o processo de criação do usuário, clique em **[!UICONTROL Enviar]**.

![Nova mensagem de usuário](assets/new-user-message.png)

Em seguida, acesse o menu Admin e clique em **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Clique no menu New e selecione [!UICONTROL Novo serviço]. Dê um nome descritivo ao serviço e selecione &quot;Personalizado&quot; no menu suspenso Serviço. Forneça uma descrição, selecione o novo usuário no menu suspenso Somente API de usuário e clique em [!UICONTROL Criar].

![Novo serviço de ponto de inicialização](assets/admin-launchpoint-new-service.png)

Clique em Exibir detalhes para o novo serviço para acessar a ID do cliente e o Segredo do cliente. Por enquanto, você pode clicar no link [!UICONTROL Obter token] botão para gerar um token de acesso válido por uma hora. Salve o token em uma nota por enquanto.

![Obter token](assets/get-token.png)

Em seguida, acesse o menu Admin e, em seguida, o **[!UICONTROL Serviços da Web]**.

![Serviços da Web](assets/admin-web-services.png)

Localize o Endpoint na caixa API REST e salve em uma nota por enquanto.

![Endpoint REST](assets/admin-web-services-rest-endpoint-1.png)

Abra uma nova guia do navegador e insira o seguinte, usando as informações apropriadas para chamar [Obter clientes em potencial por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

```
<Your Endpoint URL>/rest/v1/leads.json?access_token=<Your Access Token>&filterType=email&filterValues=<Your Email Address>
```

Se você não tiver um registro de cliente potencial com seu endereço de email no banco de dados, substitua-o por um que você sabe que está lá. Pressione Enter na barra de URL e você deverá obter uma resposta JSON semelhante a:

```json
{
    "requestId":"c493#1511ca2b184",
    "result":[
       {
           "id":1,
           "updatedAt":"2015-08-24T20:17:23Z",
           "lastName":"Elkington",
           "email":"developerfeedback@marketo.com",
           "createdAt":"2013-02-19T23:17:04Z",
           "firstName":"Kenneth"
        }
    ],
    "success":true
}
```

## Utilização da API

Cada um dos usuários da API é relatado individualmente no relatório de uso da API, portanto, dividir os serviços da Web por usuário permite considerar facilmente o uso de cada uma de suas integrações. Se o número de chamadas de API para sua instância exceder o limite, causando falha nas chamadas subsequentes, o uso dessa prática permitirá considerar o volume de cada um dos serviços e avaliar como resolver o problema. Veja seu uso acessando **[!UICONTROL Admin]** -> **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]** e clicando no número de chamadas nos últimos sete dias.
