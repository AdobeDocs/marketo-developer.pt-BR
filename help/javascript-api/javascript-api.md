---
title: API do JavaScript
description: Saiba como usar a API do JavaScript do Marketo com código incorporado para rastreamento de clientes potenciais do Munchkin, Forms 2.0, Web Personalization e Predictive Content.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 267
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
