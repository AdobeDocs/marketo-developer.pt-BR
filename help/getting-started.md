---
title: Introdução
description: Introdução às APIs do Marketo Engage e ao modelo de dados, incluindo leads, atividades, programas, tags, listas, orientação REST e aviso de desativação da SOAP.
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
TQID: https://experienceleague.adobe.com/0lfzor5EQJ0VqIh4fqlK29OiPmRCy6fnEtncJ38r-OM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c954475c-8548-4e33-a0b8-6b550d956115id: d1d0a9cd-295d-4976-8c39-ddae266f240eid: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1228
ht-degree: 2%

---

# Introdução

O Marketo Engage é uma plataforma de automação de marketing para gerenciar programas e campanhas multicanais personalizados para clientes atuais e potenciais. É possível estender a plataforma por meio de seus pontos de integração.

Esta página apresenta as entidades principais do Marketo Engage e seus relacionamentos.

>[!NOTE]
>
>A API do SOAP está sendo descontinuada e não estará mais disponível após 31 de julho de 2026. Use a [REST API](./rest-api/rest-api.md) do Marketo para todo o desenvolvimento novo. Migrar serviços existentes até essa data para evitar interrupções do serviço. Se um serviço usar a API do SOAP, consulte o [Guia de Migração](./soap-api/migration.md) da API do SOAP.
>

Quando a conexão Native SFDC ou MS Dynamics CRM está habilitada em uma instância do Marketo Engage, esses objetos são somente leitura:

- Empresa
- Oportunidade
- Função da oportunidade
- Vendedor

![Modelo de dados](assets/data_model.png)

## Pessoa (Clientes Potenciais)

As pessoas são a base da automação de marketing. A Marketo se refere a todos os registros de não-vendedores como leads, independentemente de as vendas os considerarem leads, prospetos, suspeitos ou contatos.

O objeto de lead inclui campos padrão como email, nome e sobrenome. Você pode adicionar campos para armazenar outras informações e pode ler e gravar atributos personalizados da mesma forma que os campos padrão. Localize a lista completa de campos em **[!UICONTROL Admin]** > **[!UICONTROL Gerenciamento de Campos]** no Marketo.

O Marketo identifica clientes em potencial de maneira exclusiva pelo campo id. Você deve aplicar outras chaves exclusivas fora do sistema.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Atividades

Os clientes potenciais podem interagir com sua organização de várias maneiras, como visitar uma página da Web, participar de uma feira comercial ou baixar um informe oficial. O Marketo captura essas ações como atividades para que os profissionais de marketing possam entender o que um lead fez e quando ele ocorreu.

As atividades estão sempre relacionadas a clientes potenciais por leadId.

Você também pode definir atividades personalizadas. Depois de criar e publicar uma atividade personalizada, você pode adicionar instâncias dela por meio da API do Marketo. Para obter mais informações, consulte [Noções básicas sobre atividades personalizadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programas e campanhas

Um Programa organiza os esforços de marketing relacionados a um profissional de marketing em um local. Por exemplo, uma explosão de email pode ser um Programa.

Um cliente potencial pode realizar várias ações ou atividades associadas a um Programa. Esse processo é conhecido como progressão de lead. Para um programa de explosão de email, a progressão pode registrar quando o Marketo envia o email, quando a pessoa o abre e se ela clica em um link.

Uma campanha serve uma finalidade e uma meta específicas em um programa. Por exemplo, uma Campanha pode selecionar um grupo de clientes potenciais e enviar uma explosão de email. Outra Campanha pode notificar um representante de vendas quando um lead clicar em um link na explosão do email.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

## Tags

O grupo de tags e categoriza os dados do programa para relatórios. Use tags para medir a eficácia e o ROI do programa.

Como administrador do Marketo, você pode criar tipos de tags obrigatórios e opcionais que os usuários selecionam ao criar um programa. Você define os valores possíveis para cada tipo de tag com base nos requisitos de relatórios da empresa.

Por exemplo, crie um tipo de tag personalizado &quot;Região&quot; com valores como Nordeste e Sudeste para analisar qual região gera mais leads. Crie um tipo de tag &quot;Proprietário&quot; para comparar quais proprietários de programas, como Maria, David ou John, têm maior impacto na criação de leads e oportunidades. Para obter mais informações, consulte [Noções Básicas sobre Marcas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset)

## Listas

Listas organizam coleções de clientes potenciais. O Marketo fornece dois tipos:

- Uma lista estática é uma coleção fixa da qual um profissional de marketing pode adicionar ou remover leads.
- Uma lista inteligente é uma coleção dinâmica baseada em características definidas.

Por exemplo, uma lista inteligente chamada &quot;Todos os clientes potenciais que visitaram a página de preços em nosso site&quot; continua a crescer à medida que mais clientes potenciais visitam essa página. Para obter mais informações, consulte a [documentação do Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/home?lang=pt-BR).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

## Oportunidades

Uma oportunidade representa um possível acordo de vendas que os profissionais de marketing oferecem às vendas. No Marketo, uma oportunidade é associada a um cliente potencial ou contato e a uma organização.

Uma função de oportunidade conecta um cliente potencial a uma organização e descreve a função do cliente potencial nessa organização.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

## Empresas

Uma organização, às vezes chamada de conta no Marketo, é a organização à qual uma pessoa pertence.

Para uma atribuição precisa de ROI nos relatórios de ROI do Marketo ou no Revenue Cycle Analytics (RCA), associe as pessoas às suas organizações e oportunidades.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

## Ativos

O Assets inclui landing pages, emails, formulários e imagens usadas em um programa. Um ativo pode ser local para um Programa específico ou global. Os ativos globais estão disponíveis para todos os programas.

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset)

## Tokens

Os tokens permitem que os profissionais de marketing personalizem mensagens com ativos e adicionem lógica às ações de fluxo. A Marketo fornece tokens para o sistema geral, Programas, clientes potenciais e empresas.

Por exemplo, coloque o token de cliente potencial `{{lead.First Name}}` em um email para exibir o nome do cliente potencial.

Os tokens definidos no nível de programa ou pasta são chamados de &quot;Meus tokens&quot; no Marketo. Meus tokens têm três tipos:

- Local: criado em uma pasta de campanha ou programa específico e disponível somente nessa pasta ou programa.
- Herdado: criado no nível da pasta da campanha e disponível para todos os programas nessa pasta.
- Substituído: modificado com um valor personalizado no nível do Programa sem alterar o valor pai Meu Token no nível da pasta do Programa.

Meus Tokens usam a convenção de nomenclatura `{{my.My Token}}`, com a palavra &quot;my&quot; no início do nome do token. Por exemplo, um tipo de Data Meu Token chamado EventDate tem o nome de token `{{my.EventDate}}`. Para obter mais informações, consulte [Entendendo Meus Tokens em um Programa](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

## Objetos personalizados

Um objeto personalizado do Marketo cria uma relação um para muitos ou muitos para muitos (Edge-Bridge-Edge) entre o Marketo Leads e registros de objeto personalizado.

Depois de criar e publicar um objeto personalizado do Marketo, você pode executar operações CRUD nele por meio da API do Marketo. Quando novos registros são adicionados, você pode usar um acionador de lista inteligente para responder. Você também pode usar dados de objetos personalizados como um filtro de lista inteligente para segmentação ou em emails por meio do [Script de email](email-scripting.md). Para obter mais informações sobre como criar objetos personalizados, consulte a [documentação do Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/home?lang=pt-BR).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

## Vendedores

Você pode gerenciar registros de Vendedor e seus relacionamentos de clientes potenciais no Marketo quando nenhuma integração de CRM nativa estiver habilitada. Esses registros contêm informações como Nome, Email e Cargo. Quando um Vendedor possui um cliente potencial, você pode usar essas informações para filtrar e usar tokens.

Gerenciar o relacionamento com um vendedor no nível de cliente potencial por meio do campo &quot;externalSalesPersonId&quot;. Atualize este campo por meio da API [Clientes Potenciais de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).

APIs relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)
