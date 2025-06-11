---
title: Migração para a API REST
feature: SOAP
description: Migração do SOAP para APIs REST
exl-id: c2956db3-defe-4163-99f3-58654ce8ee2b
source-git-commit: 8a785b0719e08544ed1a87772faf90bd9dda3077
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---

# Migração para a API REST

A API do Marketo Engage SOAP será desativada após 31 de outubro de 2025. Todas as integrações existentes que usam a API do SOAP devem ser removidas ou migradas para a [API REST do Marketo Engage](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/rest-api) até esta data para evitar interrupções no serviço.

## Migração

A API do SOAP oferece suporte a uma variedade limitada de casos de uso em comparação ao [REST AP](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/rest-api)I. Ao determinar quais endpoints mapear seus casos de uso, você deve seguir as [Práticas recomendadas de integração do Marketo](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/marketo-integration-best-practices)

[Arquiteturas de Referência](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/reference-architectures) estão disponíveis para casos de uso de [Sincronização de CRM](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=pt-BR) e [Exportação de Data Warehouse](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=pt-BR).

## Autenticação

[Documentação de Autenticação](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/authentication)

A API REST do Marketo usa autenticação baseada no OAuth 2.0 com o tipo de concessão Credenciais de cliente. Os tokens de acesso são válidos por uma hora após a criação.

## Leads

[Documentação da API de clientes potenciais](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/leads)

A API do SOAP oferece suporte à sincronização de dados de clientes potenciais, à [associação de cookies do Munchkin](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking) e à mesclagem de clientes potenciais. Se o aplicativo chamar o método SOAP syncLead e definir o parâmetro `marketoCookie`, você poderá migrar:

1. Usando o método REST [Sincronizar clientes em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST), seguido por [Cliente em potencial Associado](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. Você pode chamar [Enviar Formulário](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/leads), embora isso exija a configuração de alguma Assets de marketing e alguma interação com a [API do Forms](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/assets/forms)

Os aplicativos que usam o tipo de chave `foreignSysPersonId` devem migrar para o uso de um campo de cliente potencial personalizado para representar esse identificador externo e usar os Métodos REST [Sincronizar Clientes Potenciais](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) ou [Importação de Cliente Potencial em Massa](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import).

| Método SOAP | Método(s) REST |
| --- | --- |
| [getLead](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/leads/getlead) | [Obter Cliente Potencial por ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Obter Cliente Potencial por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [getMultipleLeads](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [Obter Cliente Potencial por ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Obter Cliente Potencial por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [Obter Cliente Potencial por ID de Programa](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [Obter Cliente Potencial por ID de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [Exportação de Cliente Potencial em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [mergeLeads](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/leads/mergeleads) | [Mesclar clientes em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [syncLead](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/leads/synclead) | [Sincronizar Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Enviar Formulário](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) [Associar Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST) |
| [syncMultipleLeads](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [Clientes Potenciais de Sincronização](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Importação em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## Objetos M

M Objects era um conceito abrangente para oferecer suporte à exportação de dados de Atribuição de oportunidades para análise externa e trabalhou com três tipos de objetos: Oportunidades, Funções de oportunidades e Programas.

Documentação REST:

- [Oportunidade](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [Funções](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [Programas](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/assets/programs)

| Método SOAP | Método(s) REST |
| --- | --- |
| [excluirMObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [Excluir Oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [Excluir Funções De Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [descreverMObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [Descrever oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [Descrever função da oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [getMObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [Obter Oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [Obter Funções de Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [listMObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | N/D |
| [syncMObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [Sincronizar Oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [Sincronizar Funções de Oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [getChannels](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/programs/getchannels) | [Obter Canais](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [getTags](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/programs/gettags) | [Obter Tipos de Marca](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [Obter Marca por Nome](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Listas estáticas

Os casos de uso de lista estática na API do SOAP estão limitados à assimilação de dados de associação e de cliente potencial, bem como à remoção de associação, o que pode ser feito com os Métodos REST [Adicionar à lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST), [Importar clientes potenciais em massa](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) ou [Remover da lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE).

| Método SOAP | Método(s) REST |
| --- | --- |
| [getImportToListStatus](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [Clientes Potenciais de Importação em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [importToList](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [Adicionar À Lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) [Clientes Potenciais De Importação Em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [listOperation](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [Remover da lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Atividades

A API do SOAP só oferece suporte à recuperação de atividades.

Documentação REST:

- [Atividades síncronas](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/activities)
- [Extração de Atividade em Massa](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| Método SOAP | Método(s) REST |
| --- | --- |
| [getLeadActivity](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/activities/getleadactivity) | [Atividades de Exportação em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) |
| [getLeadChanges](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/activities/getleadchanges) | [Atividades de Exportação em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Obter Alterações de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET) |

## Campanhas

Documentação REST:

- [Campanhas inteligentes](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

A API do SOAP só oferece suporte a três casos de uso para campanhas inteligentes: [Acionar clientes em potencial para qualificar-se para uma campanha inteligente solicitável](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger), recuperar essas campanhas solicitáveis e [Agendar uma execução futura de uma campanha inteligente](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| Método SOAP | Método(s) REST |
| --- | --- |
| [getCampaignsForSource](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [Obter Campanhas Inteligentes](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [requestCampaign](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [Solicitar Campanha](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [scheduleCampaign](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [Campanha programada](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Objetos personalizados

Documentação REST:

- [Objetos Personalizados](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

A API do SOAP só oferecia suporte a operações CRUD para objetos personalizados.

| Método SOAP | Método(s) REST |
| --- | --- |
| [deleteCustomObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [Excluir Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [getCustomObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [Obter Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [syncCustomObjects](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | [Sincronizar Objetos Personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) [Importar Objeto Personalizado em Massa](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import) |
