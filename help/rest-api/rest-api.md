---
title: API REST
feature: REST API
description: Saiba como usar a API REST do Marketo, configurar usuários da API e o LaunchPoint, exibir cotas e limites, autenticar com o cabeçalho de autorização e recuperar leads.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
TQID: https://experienceleague.adobe.com/GqhWI816wWX-2zf89wWj-GXpg9i615HRFVl2ljdYVj0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 803
ht-degree: 2%

---

# API REST

A API REST do Marketo fornece acesso remoto a vários recursos do sistema. Você pode usá-lo para criar programas, importar leads em massa e controlar uma instância do Marketo em um nível detalhado.

As REST APIs se dividem em duas categorias amplas:

- As APIs do [Banco de Dados de Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi) recuperam e interagem com registros de pessoas da Marketo e tipos de objetos associados, como Oportunidades e Empresas.
- As APIs do [Ativo](https://developer.adobe.com/marketo-apis/api/asset) interagem com material de apoio de marketing e registros relacionados ao fluxo de trabalho.

>[!NOTE]
>
>A API do SOAP está sendo descontinuada e não estará mais disponível após 31 de julho de 2026. Todos os novos desenvolvimentos devem ser feitos com a [REST API](./rest-api.md) do Marketo, e os serviços existentes devem ser migrados até essa data para evitar interrupções no serviço. Se você tiver um serviço que usa a API do SOAP, consulte o [Guia de Migração](../soap-api/migration.md) da API do SOAP para obter informações sobre como migrar.
>

>[!IMPORTANT]
>
>Veja esta [Nação postagem](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) sobre a descontinuação da barra dupla em URLs de gateway de API.
>

- **Cota diária:** cada assinatura recebe 50.000 chamadas de API por dia. A cota é redefinida diariamente às 12h (CST). Entre em contato com o gerente da conta para aumentar a cota diária.
- **Limite de taxa:** cada instância é limitada a 100 chamadas de API por 20 segundos.
- **Limite de simultaneidade:** cada instância permite no máximo dez chamadas de API simultâneas.

As chamadas de API padrão têm um comprimento máximo de URI de 8 KB e um tamanho máximo de corpo de 1 MB. As chamadas de API em massa suportam um tamanho máximo de corpo de 10 MB.

Quando uma chamada contém um erro, a API normalmente ainda retorna o código de status HTTP 200. A resposta JSON contém um membro `success` com valor `false` e uma matriz de erros no membro `errors`. Mais informações sobre erros estão disponíveis [aqui](error-codes.md).

## Introdução

Você precisa de privilégios de administrador na instância do Marketo para concluir as etapas a seguir. Este fluxo de trabalho cria credenciais de API e as usa para recuperar um registro de cliente potencial.

Primeiro, crie um usuário da API e obtenha credenciais para chamadas autenticadas. Faça logon na sua instância e acesse **[!UICONTROL Administrador]** > **[!UICONTROL Usuários e Funções]**.

![Usuários e funções do administrador](assets/admin-users-and-roles.png)

Selecione a guia **[!UICONTROL Funções]** e selecione Nova Função. Atribua à função pelo menos a permissão &quot;Lead somente leitura&quot; (ou &quot;Pessoa somente leitura&quot;) do grupo de APIs de acesso. Dê um nome descritivo à função e selecione **[!UICONTROL Criar]**.

![Nova Função](assets/new-role.png)

Retorne à guia [!UICONTROL Usuários] e selecione **[!UICONTROL Convidar Novo Usuário]**. Insira um nome descritivo que identifique o usuário como um usuário da API, insira um endereço de email e selecione **[!UICONTROL Avançar]**.

![Novas Informações do Usuário](assets/new-user-info.png)

Selecione a opção [!UICONTROL Somente API], atribua a função de API que você criou e selecione **[!UICONTROL Avançar]**.

![Novas permissões de usuário](assets/new-user-permissions.png)

Selecione **[!UICONTROL Enviar]** para criar o usuário.

![Nova Mensagem de Usuário](assets/new-user-message.png)

Em seguida, vá para o menu [!UICONTROL Admin] e selecione **[!UICONTROL LaunchPoint]**.

![Ponto de inicialização](assets/admin-launchpoint.png)

Selecione **[!UICONTROL Novo]** > **[!UICONTROL Novo serviço]**. Insira um nome descritivo e uma descrição e selecione **[!UICONTROL Personalizado]** no menu [!UICONTROL Serviço]. Selecione o novo usuário no menu [!UICONTROL Somente Usuário da API] e selecione **[!UICONTROL Criar]**.

![Novo Serviço de Ponto de Inicialização](assets/admin-launchpoint-new-service.png)

Selecione **[!UICONTROL Exibir Detalhes]** para que o novo serviço acesse a ID do Cliente e o Segredo do Cliente. Selecione **[!UICONTROL Obter Token]** para gerar um token de acesso válido por uma hora. Salve o token para a primeira chamada de API.

![Obter token](assets/get-token.png)

Vá para **[!UICONTROL Admin]** > **[!UICONTROL Serviços da Web]**.

![Serviços da Web](assets/admin-web-services.png)

Localize o [!UICONTROL Ponto de extremidade] na caixa API REST e salve-o para a primeira chamada de API.

![Ponto de extremidade REST](assets/admin-web-services-rest-endpoint-1.png)

Todas as chamadas à API REST devem incluir um token de acesso em um cabeçalho HTTP.

```text
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 30 de junho de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

Abra uma nova guia do navegador e insira o seguinte URL. Substitua os espaços reservados pelo ponto de extremidade e endereço de email da sua instância para chamar [Obter Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET).

```text
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Se o banco de dados não contiver um registro de cliente potencial com seu endereço de email, use o endereço de email de um cliente potencial existente. Envie o URL para receber uma resposta JSON semelhante ao seguinte exemplo:

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

O relatório de uso da API rastreia cada usuário da API separadamente. Atribuir um usuário separado a cada serviço da Web ajuda a identificar o uso da API de cada integração.

Se as chamadas excederem seu limite de instância e as chamadas subsequentes falharem, use o relatório para identificar o volume de chamadas de cada serviço. Vá para **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]** e selecione o número de chamadas feitas nos últimos sete dias.

Para os pontos de extremidade REST que retornam estatísticas de uso e erro diárias e dos últimos sete dias, consulte [Uso](usage.md).
