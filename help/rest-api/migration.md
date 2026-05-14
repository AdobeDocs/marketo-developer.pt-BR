---
title: Obter atualizações da API do lead
feature: REST API
description: Saiba mais sobre as alterações nos limites para Obter atividades de cliente potencial e Obter endpoints de alterações de cliente potencial.
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 0%

---

# Obter atualizações da API do lead

A partir de 30 de setembro de 2026, ocorrerá uma falha nas chamadas para os pontos de extremidade [Obter atividades de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) ou [Obter alterações de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) que incluem o parâmetro `listId` se as listas de destino contiverem 10.000 ou mais clientes potenciais com um Código de Erro 1003 indicando que a lista estática de destino tem muitos registros. Recentemente, foram feitas uma ou mais chamadas de API que seriam afetadas por essa alteração. Para evitar interrupções do serviço, talvez seja necessário atualizar a maneira como seus aplicativos se integram ao Marketo até 30 de setembro de 2026.

Esses tipos de consultas geralmente criam pesquisas que não têm resultados potenciais ou atingem o tempo limite antes que qualquer resultado seja encontrado. Limitar o tamanho do conjunto melhora a capacidade de resposta desses tipos de consultas e garante que uma pesquisa no conjunto de dados possa ser concluída em tempo hábil.

## Como Posso Saber Se Sou Afetado?

Essa alteração afetará somente um pequeno número de instâncias do Marketo Engage. Os administradores de assinaturas afetadas serão notificados no aplicativo antes que a alteração seja aplicada.

## O que preciso fazer?

Você deve compartilhar esse documento com as pessoas ou a equipe responsável pelas integrações do Marketo Engage.

Dependendo do caso de uso, há duas opções básicas para migrar seu aplicativo para o:

* Limite as listas estáticas das quais você extrai atividades para um máximo de 10.000 membros. Você pode dividir qualquer uma de suas listas existentes em listas menores para continuar pesquisando o mesmo público-alvo por atividades.
* Extraia suas Atividades ou Alterações no Valor dos Dados usando a Extração de Atividade em Massa ou Fluxos de Dados e associe esses resultados à associação de lista estática com [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) ou [Extração de Cliente Potencial em Massa](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract)

## O Que Acontecerá Se Eu Não Fizer Nada?

Você pode enfrentar interrupções na função das integrações de API devido a erros não tratados ao consultar atividades de listas estáticas com um grande número de membros.
