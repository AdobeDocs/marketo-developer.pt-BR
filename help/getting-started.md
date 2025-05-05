---
title: Introdução
description: Introdução às APIs do Marketo Engage
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
source-git-commit: 490411e411bed7b5b76fd9e5f41ccc9d156b2ba9
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 1%

---

# Introdução

O Marketo Engage é uma plataforma de automação de marketing que permite aos profissionais de marketing gerenciar programas e campanhas multicanais personalizados para clientes potenciais e de clientes. A plataforma Marketo Engage pode ser estendida usando pontos de integração. Abaixo você encontra as entidades principais e seus relacionamentos.

>[!NOTE]
>A API do SOAP está sendo descontinuada e não estará mais disponível após 31 de outubro de 2025. Todos os novos desenvolvimentos devem ser feitos com a [REST API](./rest-api/rest-api.md) do Marketo, e os serviços existentes devem ser migrados até essa data para evitar interrupções no serviço. Se você tiver um serviço que usa a API SOAP, consulte a API SOAP [Guia de Migração](./soap-api/migration.md) para obter informações sobre como migrar.
>

Quando a conexão Nativa do SFDC ou do MS Dynamics CRM está habilitada em uma instância do Marketo Engage, os seguintes objetos são Somente Leitura: Empresa, Oportunidade, Função da Oportunidade, Vendedor

![Modelo de dados](assets/data_model.png)

## Pessoa (Clientes Potenciais)

As pessoas são a base de qualquer plataforma de automação de marketing. No Marketo, todos os registros de pessoas que não são de vendas são chamados de leads, independentemente de serem designados como leads, prospetos, suspeitos, contatos e assim por diante, de uma perspectiva de vendas. O objeto principal vem com um conjunto de campos padrão, como email, nome e sobrenome. Campos adicionais podem ser adicionados ao tipo de objeto do cliente potencial para estender os tipos de informações associadas aos registros no sistema. Os atributos personalizados podem ser lidos e gravados como campos padrão. Uma lista completa de campos pode ser encontrada no menu **[!UICONTROL Admin]** > **[!UICONTROL Gerenciamento de Campos]** do Marketo. Os clientes em potencial são identificados exclusivamente no Marketo pelo campo id. Outras chaves exclusivas devem ser impostas externamente do sistema.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Atividades

Os clientes potenciais interagem com sua organização de algumas maneiras. Um cliente potencial pode visitar uma página no site da empresa, participar de uma feira comercial ou baixar um whitepaper. Cada uma dessas ações pode ser capturada no Marketo para ajudar um profissional de marketing a entender melhor quais atividades um lead realizou e quando, para que ele possa coordenar comunicações oportunas e relevantes. As atividades são sempre relacionadas a clientes potenciais por leadId.

Você pode definir suas próprias atividades personalizadas. Depois de criar e publicar uma atividade personalizada, você pode adicionar atividades personalizadas por meio da API do Marketo. Mais informações sobre atividades personalizadas podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programas e campanhas

Um Programa é o mecanismo pelo qual um profissional de marketing organiza todos os seus diferentes tipos de esforços de marketing a partir de um local central. Um exemplo de programa é uma explosão de email. Um lead pode realizar várias ações/atividades relacionadas a um determinado programa que se tornam associadas ao programa. Isso é conhecido como progressão de lead. Um exemplo de progressão de um programa de explosão de email registraria quando um cliente potencial receberia um email, quando a pessoa abriria o email ou se clicaria em um link no email.

As campanhas são criadas para atender a uma finalidade específica e um objetivo específico em um Programa. Um exemplo de campanha pode ser restringir um grupo de clientes potenciais e enviar a ele a explosão do email, ou notificar um representante de vendas para acompanhamento se um cliente potencial clicar em um link dentro do programa de explosão de email.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

## Tags

As tags são uma maneira de agrupar dados para fins de relatório. Esses identificadores fornecem a capacidade de categorizar dados e definir como você deseja relatar sobre o programa para entender a eficácia do programa e o ROI.

Como administrador do Marketo, você pode criar tipos de tag obrigatórios e opcionais disponíveis para seleção quando um usuário do Marketo cria um programa. Os valores possíveis para cada um desses tipos de tag são definidos por você e refletem como sua empresa gostaria de usar tags personalizadas para fins de relatório.

Por exemplo, você pode criar um tipo de tag personalizado &quot;Região&quot; com vários valores de tag (por exemplo, Nordeste, Sudeste), permitindo analisar qual região está gerando mais leads. Ou, por exemplo, você pode criar um tipo de tag &quot;Proprietário&quot;, que permite avaliar e entender quais proprietários de programas (por exemplo, Maria, David ou John) estão tendo maior impacto na criação de leads e oportunidades. Mais informações sobre marcas podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Listas

As listas permitem que um profissional de marketing organize uma coleção de leads. Há dois tipos de listas no Marketo: estáticas e inteligentes. Uma lista estática é uma lista fixa de clientes potenciais que um profissional de marketing pode adicionar ou remover à sua escolha. Uma lista inteligente é uma coleção dinâmica de leads com base em um conjunto de características designadas. Um exemplo de lista inteligente seria &quot;Todos os clientes potenciais que visitaram a página de preços em nosso site&quot;. Essa lista inteligente continua crescendo à medida que mais clientes potenciais visitam a página de preços. Mais informações sobre listas podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

## Oportunidades

Os profissionais de marketing fornecem leads para vendas na forma de uma oportunidade. Uma oportunidade representa um possível acordo de vendas e está associada a um cliente potencial ou contato e a uma organização na Marketo. Uma função de oportunidade é a interseção entre um determinado lead e uma organização. A função da oportunidade pertence à função de um lead na organização.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

## Empresas

Uma organização, às vezes chamada de conta no Marketo, se refere à organização à qual uma pessoa pertence. Ao usar relatórios de ROI no Marketo ou no Revenue Cycle Analytics (RCA), é importante associar pessoas à sua organização e oportunidades para que a atribuição de ROI adequada possa ser determinada.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

## Ativos

O Assets se refere às páginas de aterrissagem, emails, formulários e imagens usados em um programa. O Assets pode ser local para um determinado programa ou global. Os ativos globais estão disponíveis em qualquer programa.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Tokens

Os tokens permitem que um profissional de marketing personalize mensagens com ativos e adicione lógica nas ações de fluxo. Há tokens para o sistema geral, programas, clientes potenciais e empresas. Um exemplo de token de cliente potencial é {{lead.First Name}}. Esse token pode ser colocado em um email para exibir o nome do lead.

Os tokens definidos no nível de programa ou pasta são chamados de &quot;Meus tokens&quot; no Marketo. Meus tokens podem ser de um dos três tipos: local, herdado ou substituído.

Meus Tokens criados localmente em uma pasta de campanha ou programa específico estão disponíveis para esse programa específico ou pasta de campanha (local). Meus tokens criados no nível da pasta da campanha estão disponíveis para uso em todos os programas contidos nessa pasta da campanha (herdada). Meus tokens modificados no nível do programa com valores personalizados não alteram o valor principal de Meu token no nível da pasta do programa (substituído).

Meus Tokens usam a convenção de nomenclatura {{my.My Token}}, com a palavra &quot;my&quot; adicionada ao início do nome do token. Por exemplo, se você criar um tipo de Data Meu token com o nome EventDate, o nome do token será {{my.EventDate}}. Mais informações sobre Meus Tokens podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

## Objetos personalizados

Um objeto personalizado do Marketo permite a criação de uma relação um para muitos ou muitos para muitos (Edge-Bridge-Edge) entre seus clientes potenciais do Marketo e os registros de objeto personalizado. Depois de criar e publicar um objeto personalizado do Marketo, você pode executar operações CRUD no objeto personalizado por meio da API do Marketo. Mais informações sobre a criação de objetos personalizados podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR). Quando novos registros são adicionados ao objeto personalizado, você pode usar um acionador de lista inteligente para responder. Você também pode usar dados de objetos personalizados como filtro em listas inteligentes (segmentação) ou em emails usando o [Script de email](email-scripting.md).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects)

## Vendedores

Os registros de Vendedor e relacionamentos de cliente potencial podem ser gerenciados no Marketo quando não há integração de CRM nativa habilitada. Esses registros contêm informações básicas sobre o Vendedor, como Nome, Email e Cargo, que podem ser usadas para filtragem e tokens no Marketo quando um cliente potencial é de propriedade de um. A relação com um vendedor é gerenciada no nível de cliente potencial por meio do campo &quot;externalSalesPersonId&quot;, que deve ser atualizado por meio da API [Sincronizar Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
