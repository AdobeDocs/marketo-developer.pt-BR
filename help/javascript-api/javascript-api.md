---
title: API do JavaScript
description: Saiba como usar a API do JavaScript do Marketo com código incorporado para rastreamento de clientes potenciais do Munchkin, Forms 2.0, Web Personalization e Predictive Content.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: 'https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
    internal-label: Personalization
  - id: 1c1a93b2-f024-5627-9905-6faf6fcc22be
    internal-label: Munchkin Tracking Code
  - id: 664d862c-1673-5ed4-a3d6-386ac83225e4
    internal-label: Web Personalization
  - id: 52412b34-abb2-53fa-9fea-8547c07823df
    internal-label: Predictive Content
  - id: dda1332a-c3e0-583f-9d9b-15f1934e0ad3
    internal-label: Javascript
  - id: c2dbad80-0f5c-4d96-a798-2a65f93b8721
    internal-label: Assets
subfeature_v2:
  - id: e9b7b90f-6f8a-4637-a2ca-00239808918c
    internal-label: Social
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
    internal-label: Machine learning
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 2%
---
# API Javascript

As integrações do JavaScript no lado do cliente do Marketo fornecem rastreamento de clientes potenciais, formulários, personalização da Web e recursos de conteúdo preditivo. É necessário ter uma conta do Marketo para usar esses recursos.

A implementação geralmente envolve a adição de código de inserção à propriedade da Web. Você também pode chamar as funções do JavaScript expostas pelo código incorporado para adicionar funcionalidade.

O código incorporado é exclusivo para sua instância do Marketo porque contém um identificador de conta. Na interface do usuário do Marketo, vá para o painel apropriado, copie o código incorporado na área de transferência e cole-o na página da Web.

## Rastreamento de clientes potenciais (Munchkin)

O [código de rastreamento do Munchkin JavaScript](lead-tracking.md) da Marketo gera clientes potenciais a partir das visitas ao seu site. Ele também rastreia visitantes que não forneceram informações pessoais e cria leads anônimos que incluem o endereço IP do usuário e outras informações.

Configure o Munchkin na página Munchkin na área Administração do Marketo.

## Forms 2.0

O [Forms 2.0](forms-api-reference.md) permite que os profissionais de marketing criem formulários web sem conhecimento de programação. O Forms pode residir nas páginas de aterrissagem do Marketo ou estar incorporado a qualquer página do seu site.

Use a API do JavaScript do Forms 2.0 para estender a funcionalidade principal de um formulário web do Marketo.

## Personalização na web

O [Marketo Web Personalization](web-personalization.md) ajuda você a envolver clientes potenciais em seu site em tempo real, com base em quem eles são e no que fazem.

## Conteúdo preditivo

O [Conteúdo preditivo do Marketo](predictive-content.md) usa aprendizado de máquina e análise preditiva para apresentar conteúdo relevante aos visitantes da Web. Adicione descrições de texto e imagens ao seu conteúdo e incorpore várias recomendações de conteúdo em seu site.
