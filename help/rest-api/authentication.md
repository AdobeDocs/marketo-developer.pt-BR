---
title: Autenticação
feature: REST API
description: Autenticação de usuários do Marketo para uso da API.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
source-git-commit: 9830572277db2709c6853bea56fc70c455fd5e54
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---

# Autenticação

As REST APIs do Marketo são autenticadas com OAuth 2.0 de duas pernas. As IDs e os segredos do cliente são fornecidos por serviços personalizados definidos por você. Cada serviço personalizado pertence a um usuário Somente API que tem um conjunto de funções e permissões que autorizam o serviço a executar ações específicas. Um token de acesso é associado a um único serviço personalizado. A expiração do token de acesso independe dos tokens associados a outros serviços personalizados que podem estar presentes em uma instância.

## Criação de um token de acesso

O `Client ID` e o `Client Secret` são encontrados no menu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL LaunchPoint]** selecionando o serviço personalizado e clicando em **[!UICONTROL Exibir Detalhes]**.

![Obter Detalhes do Serviço REST](assets/authentication-service-view-details.png)

![Credenciais do ponto de inicialização](assets/admin-launchpoint-credentials.png)

O `Identity URL` é encontrado no menu **[!UICONTROL Admin]** > **[!UICONTROL Integração]** > **[!UICONTROL Serviços da Web]** na seção API REST.

Crie um token de acesso usando uma solicitação HTTP GET (ou POST) da seguinte maneira:

```
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Se sua solicitação for válida, você receberá uma resposta JSON semelhante à seguinte:

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

Definição de resposta

- `access_token` - O token que você passa com as chamadas subsequentes para autenticar com a instância de destino.
- `token_type` - O método de autenticação OAuth.
- `expires_in` - A duração restante do token atual em segundos (após o qual ele é inválido). Quando um token de acesso é criado originalmente, sua duração é de 3600 segundos ou uma hora.
- `scope` - O usuário proprietário do serviço personalizado que foi usado para autenticação.

## Uso de um token de acesso

Ao fazer chamadas para métodos da API REST, um token de acesso deve ser incluído em cada chamada para que a chamada seja bem-sucedida.

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 30 de junho de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

O token de acesso deve ser enviado como um cabeçalho HTTP. Por exemplo, em uma solicitação CURL:

```bash
$ curl -H 'Authorization: Bearer cdf01657-110d-4155-99a7-f984b2ff13a0:int`' 'https://123-ABC-456.mktourl.com/rest/v1/apicall.json?filterType=id&filterValues=4,5,7,12,13'
```

## Dicas e práticas recomendadas

O gerenciamento da expiração do token de acesso é importante para garantir que sua integração funcione sem problemas e evite a ocorrência de erros inesperados de autenticação durante a operação normal. Ao projetar a autenticação para sua integração, armazene o token e o período de expiração contidos na resposta de identidade.

Antes de fazer qualquer chamada REST, verifique a validade do token com base no tempo de vida restante. Se o token tiver expirado, renove-o chamando o ponto de extremidade [Identidade](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Isso ajuda a garantir que a chamada REST nunca falhe devido a um token expirado. Isso ajuda a gerenciar a latência das chamadas REST de maneira previsível, o que é crucial para os aplicativos voltados para o usuário final.

Se um token expirado for usado para autenticar uma chamada REST, ela falhará e retornará um código de erro 602. Se um token inválido for usado para autenticar uma chamada REST, um código de erro 601 será retornado. Se qualquer um desses códigos for recebido, o cliente deverá renovar o token chamando o endpoint de identidade.

Se você chamar o endpoint de identidade antes que o token expire, o mesmo token e o tempo de vida restante serão retornados na resposta.

Lembre-se de que seus tokens de acesso são de propriedade de cada Serviço personalizado e não de cada usuário. Embora duas respostas de identidade possam ter o escopo do mesmo usuário, os tokens de acesso e os períodos de expiração são independentes uns dos outros se tiverem sido feitos com credenciais de dois serviços diferentes. Lembre-se disso quando você tiver vários conjuntos de credenciais no mesmo aplicativo; a ID do cliente pode ser uma chave útil para gerenciá-los independentemente.
