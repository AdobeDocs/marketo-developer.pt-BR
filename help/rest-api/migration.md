---
title: Obter atualizações da API do lead
feature: REST API
description: Saiba mais sobre as alterações nos limites para Obter atividades de cliente potencial e Obter endpoints de alterações de cliente potencial.
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Obter atualizações da API do lead

A partir de 30 de setembro de 2026, as chamadas para os pontos de extremidade [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) ou [Obter Alterações de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) que incluem o parâmetro `listId` falharão se as listas de destino contiverem 10.000 ou mais clientes potenciais. Os pontos de extremidade retornarão um Código de Erro 1003 indicando que a lista estática de destino tem muitos registros.

Uma ou mais chamadas de API recentes seriam afetadas por essa alteração. Para evitar interrupções do serviço, talvez seja necessário atualizar como seus aplicativos se integram ao Marketo até 30 de setembro de 2026.

Essas consultas geralmente criam pesquisas que não têm resultados potenciais ou cujo tempo limite expirou antes de encontrar resultados. Limitar o tamanho definido melhora a capacidade de resposta da consulta e ajuda as pesquisas a serem concluídas em tempo hábil.

## Como Posso Saber Se Sou Afetado?

Essa alteração afeta apenas uma pequena quantidade de instâncias do Marketo Engage. Os administradores de assinaturas afetadas receberão uma notificação no aplicativo antes que a alteração seja aplicada.

## O que preciso fazer?

Compartilhe este documento com as pessoas ou a equipe responsável pelas integrações do Marketo Engage.

Dependendo do caso de uso, use uma destas opções de migração:

* Limitar listas estáticas usadas para extração de atividade a 10.000 membros. Divida as listas existentes em listas menores para continuar pesquisando o mesmo público-alvo para atividades do.
* Extraia atividades ou alterações no valor dos dados usando a Extração de atividade em massa ou os Fluxos de dados. Ingresse os resultados na associação de lista estática com [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) ou [Extração de Cliente Potencial em Massa](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract).

## O Que Acontecerá Se Eu Não Fizer Nada?

Suas integrações de API podem ser interrompidas por erros não tratados ao consultar atividades de listas estáticas com um grande número de membros.
