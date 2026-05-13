---
title: API do JavaScript
description: Saiba como usar a API do JavaScript do Marketo com código incorporado para rastreamento de clientes potenciais do Munchkin, Forms 2.0, Web Personalization e Predictive Content.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 1%

---

# API Javascript

Veja a seguir uma visão geral dos recursos de integração do JavaScript no lado do cliente do Marketo. É necessário ter uma conta do Marketo para usar esses recursos. Normalmente, a implementação envolve simplesmente adicionar um &quot;código incorporado&quot; à propriedade da Web. Como opção, você pode usar funcionalidades adicionais chamando funções do JavaScript que são expostas pelo código incorporado. Essas funções estão totalmente documentadas aqui.

O código incorporado é exclusivo para sua instância do Marketo, pois contém um identificador de conta. Obtenha o código incorporado navegando até o painel apropriado na interface do usuário do Marketo, copiando para a área de transferência e colando na página da Web.

## Rastreamento de clientes potenciais (Munchkin)

O [código de rastreamento do Munchkin JavaScript](lead-tracking.md) da Marketo é fundamental para os recursos do Marketo. Ele permite gerar leads a partir de visitas ao seu site. Ele rastreia até mesmo visitantes que ainda não lhe forneceram suas informações pessoais, criando leads anônimos que incluem o endereço IP do usuário e outras informações. Você configura o Munchkin na página Munchkin na área Administração do Marketo.

## Forms 2.0

O [Forms 2.0](forms-api-reference.md) permite que os profissionais de marketing criem formulários web bonitos, estáveis e flexíveis sem conhecimento de programação. O Forms pode residir nas páginas de aterrissagem do Marketo e estar incorporado a qualquer página do seu site. A funcionalidade principal de um formulário web do Marketo pode ser estendida usando a API do JavaScript do Forms 2.0.

## Personalização na web

O [Marketo Web Personalization](web-personalization.md) é uma plataforma de Personalization e direcionamento que ajuda você a envolver milhares de clientes potenciais em seu site em tempo real, com base em quem eles são e no que fazem.

## Conteúdo preditivo

O [Conteúdo preditivo do Marketo](predictive-content.md) permite que você envolva seus visitantes da Web com o conteúdo mais relevante, viabilizado pelo aprendizado de máquina e análise preditiva. Aprimore seu conteúdo com descrições de texto e imagens e incorpore várias recomendações de conteúdo ao seu site.

