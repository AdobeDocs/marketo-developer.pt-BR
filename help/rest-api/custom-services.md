---
title: "Serviços personalizados"
feature: REST API
description: "Credenciais de autenticação com o Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 6%

---


# Serviços personalizados

Um Serviço personalizado fornece credenciais para autenticar com o Marketo. As credenciais são necessárias para obter um token de acesso da Marketo [Serviço de identidade](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Cada Serviço personalizado tem seu escopo definido para um único usuário somente API, do qual ele obtém suas permissões.

## Funções

A primeira etapa na criação de um Serviço personalizado é criar uma função que pode ser aplicada ao usuário relevante somente API. Isso é feito a partir do **[!UICONTROL Admin]** > **[!UICONTROL Usuários e funções]** > **[!UICONTROL Funções]** menu.

Funções são containers para permissões individuais que permitem ou restringem o acesso a determinadas funções. Em assinaturas com Espaços de Trabalho e Partições habilitados, as permissões são concedidas por espaço de trabalho. Se um usuário tiver permissão em um espaço de trabalho, mas não em outro, ele só poderá executar as ações permitidas nesse espaço de trabalho. Para criar uma função, clique no botão Nova função.

![Usuários e funções](assets/admin-users-and-roles-roles.png)

Dê um nome descritivo à sua função. Os usuários somente API têm um conjunto específico de permissões separadas e distintas das permissões normais do usuário. As permissões de API existem em sua própria hierarquia na árvore &quot;API de acesso&quot;.

![Novas permissões de função](assets/new-role-access-api-permissions.png)

### Permissões de função

Somente as permissões no grupo &quot;Access API&quot; são aplicadas aos usuários da API, ou seja, a concessão de todas as permissões de administrador não concederá permissões de API a um usuário.

Ao construir uma função, pense cuidadosamente sobre quais ações você deve permitir que o aplicativo a use para fazer. Conceda somente o conjunto mínimo de permissões necessárias para executar essas ações. Permitir um conjunto desnecessariamente permissivo de permissões pode permitir integrações para executar ações indesejadas em sua assinatura. Você pode usar o [ferramenta permissões](endpoint-reference.md) para determinar seu conjunto mínimo de permissões. Veja a lista completa de [permissões](#permission_list).

## Usuários

Depois de criar uma função, você deve criar um usuário &quot;Somente API&quot;. Usuários somente API são um tipo especial de usuário no Marketo, pois são administrados por outros usuários e não podem ser usados para fazer logon no Marketo. Usuários somente API podem:

- Criar serviços personalizados
- Permissões de escopo para esses serviços
- Acessar APIs REST

>[!MORELIKETHIS]
>
>Para criar um usuário Somente API, vá para a **[!UICONTROL Admin]** > **[!UICONTROL Usuários e funções]** > **[!UICONTROL Usuários]** e clique em [!UICONTROL Convidar novo usuário].


![Informações do novo usuário](assets/new-user-info.png)

Dê ao usuário um nome descritivo e um endereço de email (ele não deve ser válido), com base no serviço e no aplicativo para os quais será usado. Preencha os campos obrigatórios no menu da caixa de diálogo, clique na caixa de seleção &quot;Somente API&quot; e conceda uma de suas funções de API ao usuário. Isso atribui as permissões dessa função definidas para o usuário.

![Novas permissões de usuário](assets/new-user-permissions.png)

Por fim, clique em &quot;Enviar&quot; para criar o usuário somente API.

Ao provisionar um novo aplicativo com credenciais, considere criar um novo usuário para o serviço, mesmo que ele tenha o mesmo conjunto de permissões que outra integração existente. As estatísticas e erros de uso de chamadas de API são rastreados por usuário, portanto, o provisionamento de um usuário para cada aplicativo pode ajudar a isolar o uso e os problemas de aplicativos específicos. Isso é útil caso encontre problemas ao atingir os limites diários de chamada da API ou erros resultantes de chamadas da API feitas por integrações.

## Serviços personalizados

Os Serviços personalizados fornecem as credenciais reais, a ID do cliente e o segredo do cliente, necessárias para executar a autenticação com uma instância do Marketo. Para provisionar um, vá para o **[!UICONTROL Admin]** > **[!UICONTROL Integrações]** > **[!UICONTROL LaunchPoint]** e selecione **[!UICONTROL Novo serviço]**.

Dê um nome descritivo ao serviço e, na lista &quot;Serviço&quot;, selecione o &quot;Personalizado&quot;. Forneça uma descrição detalhada ao serviço, selecione um usuário apropriado na lista Somente usuário da API e clique em [!UICONTROL Criar].

![Novo serviço personalizado](assets/admin-launchpoint-new-service.png)

Isso adiciona um novo serviço à lista de serviços do LaunchPoint e a opção &quot;Exibir detalhes&quot;. Clique em &quot;Exibir detalhes&quot; e você receberá a ID do cliente e o Segredo do cliente necessários para a autenticação, o usuário proprietário e uma opção para Obter o token para fins de teste de curto prazo. O token obtido nesta caixa de diálogo tem o mesmo tempo de vida que os tokens obtidos normalmente do [Serviço de identidade](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) e é válido por 3.600 segundos a partir da criação.

![Obter token](assets/get-token.png)

## Espaços de trabalho e partições

Em assinaturas com Espaços de Trabalho e Partições, a capacidade de acessar um determinado registro ou ativo é concedida com base nas permissões que a função de um usuário tem em um determinado espaço de trabalho. Cada espaço de trabalho recebe acesso a uma ou mais partições no menu Espaços de Trabalho e Partições, e um lead pertence a uma única partição. Se o usuário somente API tiver acesso a registros de lead de leitura ou gravação em um espaço de trabalho, ele poderá acessar todos os registros nas partições às quais esse espaço de trabalho tem acesso.

Os ativos pertencem a espaços de trabalho, portanto, a capacidade de ler ou gravar um ativo é determinada pelo fato de o usuário ter uma função no espaço de trabalho relevante que tem permissão para ler ou gravar esse tipo de registro de ativo no espaço de trabalho.

## Lista de permissões

Veja a seguir uma lista de todas as permissões disponíveis para usuários somente API e o que eles permitem que um usuário com essa permissão faça.

| Permissão de Função | Concede acesso a... |
| --- | --- |
| Aprovar ativos | Aprovar ativos |
| Executar campanha | Solicitar ou agendar uma campanha |
| Atividade somente de leitura | Recuperar atividades de clientes potenciais |
| Metadados de atividade somente de leitura | Recuperar metadados de atividade de cliente potencial |
| Ativos somente de leitura | Recuperar detalhes do ativo |
| Campanha somente de leitura | Recuperar detalhes da campanha |
| Empresa somente de leitura | Recuperar detalhes da empresa |
| Objeto personalizado somente de leitura | Recuperar detalhes do objeto personalizado |
| Lead somente de leitura | Recuperar detalhes do cliente potencial |
| Conta nomeada somente de leitura | Recuperar detalhes da conta nomeada |
| Lista de contas nomeadas somente de leitura | Recuperar detalhes da lista de contas nomeadas |
| Oportunidade somente de leitura | Recuperar detalhes da oportunidade |
| Pessoa de vendas somente de leitura | Recuperar detalhes do vendedor |
| Atividade de leitura-gravação | Recuperar e criar atividades de cliente potencial |
| Metadados de atividade de leitura e gravação | Recuperar e criar metadados de atividade de cliente potencial |
| Ativos de leitura-gravação | Recuperar, criar e atualizar ativos |
| Campanha de leitura-gravação | Recuperar, criar e atualizar campanhas |
| Empresa de leitura-gravação | Recuperar, criar e atualizar empresas |
| Objeto personalizado de leitura-gravação | Recuperar, criar e atualizar objetos personalizados |
| Lead de leitura-gravação | Recuperar, criar e atualizar detalhes do cliente potencial |
| Conta nomeada de leitura e gravação | Recuperar, criar e atualizar contas nomeadas |
| Lista de contas nomeadas de leitura e gravação | Recuperar, criar e atualizar listas de contas nomeadas |
| Oportunidade de leitura-gravação | Recuperar, criar e atualizar oportunidades |
| Pessoa de vendas de leitura-gravação | Recuperar, criar e atualizar vendedores |
