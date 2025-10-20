---
title: API REST
feature: REST API
description: Saiba como usar a API REST do Marketo, configurar usuários da API e o LaunchPoint, exibir cotas e limites, autenticar com o cabeçalho de autorização e recuperar leads.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 5f2dcb4864cdcd110ba9f199ef9c86dcee522335
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 1%

---

# API REST

O Marketo expõe uma API REST que permite a execução remota de muitos dos recursos do sistema. Desde a criação de programas até a importação de leads em massa, há muitas opções que permitem o controle refinado de uma instância do Marketo.

Essas APIs geralmente se encaixam em duas categorias amplas: [Banco de Dados Principal](https://developer.adobe.com/marketo-apis/api/mapi/) e [Ativo](https://developer.adobe.com/marketo-apis/api/asset/). As APIs de banco de dados de clientes potenciais permitem a recuperação e a interação com registros de pessoas da Marketo e tipos de objetos associados, como Oportunidades e Empresas. As APIs de ativos permitem a interação com material de apoio de marketing e registros relacionados a fluxos de trabalho.

>[!NOTE]
>A API do SOAP está sendo substituída e não estará mais disponível após 31 de janeiro de 2026. Todos os novos desenvolvimentos devem ser feitos com a [REST API](./rest-api.md) do Marketo, e os serviços existentes devem ser migrados até essa data para evitar interrupções no serviço. Se você tiver um serviço que usa a API do SOAP, consulte o [Guia de Migração](../soap-api/migration.md) da API do SOAP para obter informações sobre como migrar.
>

>[!IMPORTANT]
>Veja esta [Nação postagem](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) sobre a descontinuação da barra dupla em URLs de gateway de API.
>

- **Cota Diária:** as assinaturas recebem 50.000 chamadas de API por dia (o que é redefinido diariamente a 12:00AM CST). Você pode aumentar sua cota diária por meio do gerente da conta.
- **Limite de Taxa:** O acesso à API por instância é limitado a 100 chamadas por 20 segundos.
- **Limite de simultaneidade:**  Máximo de dez chamadas de API simultâneas.

O tamanho das chamadas padrão é limitado a um comprimento de URI de 8 KB e um tamanho de corpo de 1 MB, embora o corpo possa ser de 10 MB para nossas APIs em massa. Se houver um erro na sua chamada, a API normalmente ainda retornará um código de status 200, mas a resposta JSON conterá um membro &quot;success&quot; com um valor de `false` e uma matriz de erros no membro &quot;errors&quot;. Mais sobre os erros [aqui](error-codes.md).

## Introdução

As etapas a seguir exigem privilégios de administrador na instância do Marketo.

Na primeira chamada para o Marketo, você recupera um registro de lead. Para começar a trabalhar com o Marketo, você deve obter credenciais de API para fazer chamadas autenticadas para sua instância. Faça logon na sua instância e acesse o **[!UICONTROL Administrador]** -> **[!UICONTROL Usuários e Funções]**.

![Usuários e funções do administrador](assets/admin-users-and-roles.png)

Clique na guia **[!UICONTROL Funções]**, em Novo Função e atribua pelo menos a permissão &quot;Líder Somente Leitura&quot; (ou &quot;Pessoa Somente Leitura&quot;) à função no grupo de APIs de Acesso. Certifique-se de dar um nome descritivo e clique em **[!UICONTROL Criar]**.

![Nova Função](assets/new-role.png)

Agora, volte para a guia [!UICONTROL Usuários] e clique em **[!UICONTROL Convidar novo usuário]**. Dê ao usuário um nome descritivo que indique que ele é um usuário da API, um Endereço de email e clique em **[!UICONTROL Avançar]**.

![Novas Informações do Usuário](assets/new-user-info.png)

Em seguida, marque a opção [!UICONTROL API Somente] e conceda ao usuário a função de API que você criou e clique em **[!UICONTROL Avançar]**.

![Novas permissões de usuário](assets/new-user-permissions.png)

Para concluir o processo de criação do usuário, clique em **[!UICONTROL Enviar]**.

![Nova Mensagem de Usuário](assets/new-user-message.png)

Em seguida, vá para o menu [!UICONTROL Admin] e clique em **[!UICONTROL LaunchPoint]**.

![Ponto de inicialização](assets/admin-launchpoint.png)

Clique no menu **[!UICONTROL Novo]** e selecione **[!UICONTROL Novo serviço]**. Dê um nome descritivo ao seu serviço e selecione **[!UICONTROL Personalizado]** no menu suspenso [!UICONTROL Serviço]. Forneça uma descrição, selecione seu novo usuário no menu suspenso [!UICONTROL Usuário único da API] e clique em **[!UICONTROL Criar]**.

![Novo Serviço de Ponto de Inicialização](assets/admin-launchpoint-new-service.png)

Clique em **[!UICONTROL Exibir Detalhes]** do novo serviço para acessar a ID do Cliente e o Segredo do Cliente. Por enquanto, você pode clicar no botão **[!UICONTROL Obter token]** para gerar um token de acesso válido por uma hora. Salve o token em uma nota por enquanto.

![Obter token](assets/get-token.png)

Em seguida, vá para o menu **[!UICONTROL Admin]** e depois para **[!UICONTROL Serviços da Web]**.

![Serviços da Web](assets/admin-web-services.png)

Localize o [!UICONTROL Ponto de extremidade] na caixa API REST e salve em uma observação por enquanto.

![Ponto de extremidade REST](assets/admin-web-services-rest-endpoint-1.png)

Ao fazer chamadas para métodos da API REST, um token de acesso deve ser incluído em cada chamada para que a chamada seja bem-sucedida. O token de acesso deve ser enviado como um cabeçalho HTTP.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 30 de junho de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

Abra uma nova guia do navegador e insira o seguinte, usando as informações apropriadas para chamar [Obter clientes em potencial por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
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
