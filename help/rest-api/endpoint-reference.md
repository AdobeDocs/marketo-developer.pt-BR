---
title: "Referência do endpoint"
feature: REST API
description: "Referências de endpoint da API do Marketo"
source-git-commit: 2454f126dc4275697ef6773420453ad8853eae73
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---


# Referência do ponto de extremidade

- [Ativo](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identidade](https://developer.adobe.com/marketo-apis/api/identity/)
- [Banco de dados de clientes potenciais](https://developer.adobe.com/marketo-apis/api/mapi/)
- [User Management](https://developer.adobe.com/marketo-apis/api/user/)

## Referência do ponto de extremidade

O Marketo usa o Swagger para fornecer uma definição formal da interface pública para suas APIs REST. O Swagger fornece um modelo de definição avançado para estruturas de URL, modelos de solicitação e modelos de resposta, e tem um ecossistema desenvolvido de ferramentas para uso com interação de API, testes e geração de clientes.

A referência do endpoint usa a variável [Swagger-UI](https://swagger.io/tools/swagger-ui/) Pacote JavaScript para gerar as páginas de referência no lado do cliente. Cada endpoint público é listado e fornece a estrutura do modelo de resposta, os parâmetros de solicitação necessários e o modelo de solicitação, se necessário.

## Uso das definições do Swagger da Marketo

O padrão Swagger exige que um host seja fornecido ou que o host seja gerado pelo host que atende o arquivo. É importante corrigir o host na definição antes de empregar qualquer ferramenta existente, pois o Marketo fornece um parâmetro de host vazio com a definição. O host da API REST para cada instância do Marketo é exclusivo e segue o padrão:

`{Munchkin ID}.mktorest.com`

## APIs de ativos

As APIs de ativos do Marketo usam `application/x-www-url-formencoded` parâmetros de estilo em solicitações para endpoints que exigem um método POST. Em alguns casos, no entanto, como parâmetros de pasta, o valor do parâmetro pode ser uma matriz JSON ou um objeto. Esses parâmetros são anotados na referência do endpoint.
