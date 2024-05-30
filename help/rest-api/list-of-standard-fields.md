---
title: "Campos padrão"
feature: REST API, Field Management
description: "Uma tabela de campos padrão do Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 28%

---


# Campos padrão

Esta é uma lista de campos padrão disponíveis no Marketo que podem ser acessados por meio da API.

Você pode recuperar a lista de todos os nomes de campos compatíveis disponíveis em seus registros de lead usando o REST [Descrever lead](https://developer.adobe.com/marketo-apis/api/mapi/) terminal.

| Nome da API REST | Nome da API SOAP | Rótulo intuitivo | Descrição |
| --- | --- | --- | --- |
| endereço | Endereço | Endereço | Endereço do lead |
| annualRevenue | AnnualRevenue | Receita anual | Receita anual da empresa do cliente potencial |
| anonymousIP | AnonymousIP | IP anônimo | Endereço IP da primeira visita da Web registrada do lead |
| billingCity | BillingCity | Cidade de cobrança | Cidade do endereço de cobrança do lead |
| billingCountry | BillingCountry | País de cobrança | País do endereço de cobrança do lead |
| billingPostalCode | BillingPostalCode | Código postal de cobrança | Código postal do endereço de cobrança do lead |
| billingState | BillingState | Estado de cobrança | Estado ou província do endereço de cobrança do lead |
| billingStreet | BillingStreet | Endereço de cobrança | Endereço de cobrança da empresa do lead |
| city | Cidade | Cidade | Cidade do lead |
| empresa | Empresa | Nome da empresa | Nome da empresa do lead |
| país | País | País | País do lead |
| dateOfBirth | DateofBirth | Data de nascimento | Data de nascimento do lead |
| departamento | Departamento | Departamento | Departamento do líder na empresa |
| doNotCall | DoNotCall | Não ligar | Preferência de cliente em potencial para não chamar |
| doNotCallReason | DoNotCallReason | Motivo para não ligar | Explicação da preferência de lead do tipo &quot;não chamar&quot; |
| email | Email | Endereço de e-mail | Endereço de email do lead. Campo de chave padrão do Marketo para registros de cliente potencial |
| fax | Fax | Número de fax | Número de fax do cliente potencial |
| firstName | FirstName | Nome | Nome do lead |
| indústria | Setor | Setor | Setor de lead |
| inferredCompany | InferredCompany | Empresa indicada | Nome da empresa inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead |
| inferredCountry | InferredCountry | País indicado | País inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead |
| lastName | LastName | Sobrenome | Sobrenome do Cliente Potencial |
| leadRole | LeadRole | Função | Função do lead em sua empresa |
| leadScore | LeadScore | Pontuação do lead | Pontuação inteira atribuída ao lead por meio da pontuação de campanhas e programas |
| leadSource | LeadSource | Fonte do lead | Campo que registra de qual origem o lead se originou |
| leadStatus | LeadStatus | Status do lead | Campo que registra o status atual de marketing/vendas do cliente potencial |
| mainPhone | MainPhone | Telefone principal | Número de telefone principal da empresa do cliente potencial |
| jigsawContactId | Marketo - ID do contato do Jigsaw | Marketo - ID Data.com | ID Data.com do lead, se disponível |
| jigsawContactStatus | Marketo - Status do contato do Jigsaw | Marketo - Status Data.com | Status Data.com do lead, se disponível |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Marketo - Nome de exibição no perfil social do Facebook | Nome para exibição do Facebook do lead. Sistema preenchido durante a entrada em redes sociais |
| FacebookId | MarketoSocialFacebookID | ID social do Marketo no Facebook | ID do Facebook do lead. Sistema preenchido durante a entrada em redes sociais |
| facebookPhotoURL | MarketoSocialFacebookPhotoURL | Marketo - URL da foto no perfil social do Facebook | URL da foto do perfil do Facebook do lead. Sistema preenchido durante a entrada em redes sociais |
| facebookProfileURL | MarketoSocialFacebookProfileURL | Marketo - URL do perfil social do Facebook | URL do perfil Facebook do lead. Sistema preenchido durante a entrada em redes sociais |
| facebookReach | MarketoSocialFacebookReach | Marketo - Alcance do perfil social do Facebook | O alcance da Facebook do lead. Sistema preenchido durante a entrada em redes sociais |
| FacebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Marketo - Inscrições por indicação no perfil social do Facebook | Número de inscrições referenciadas atribuídas ao lead por meio do Facebook. Gerenciado pelo sistema |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Marketo - Visitas por indicação no perfil social do Facebook | Número de visitas referenciadas atribuídas ao lead por meio do Facebook. Gerenciado pelo sistema |
| gênero | MarketoSocialGender | Marketo - Gênero em perfil social | Gênero do lead. Sistema preenchido durante a entrada em redes sociais |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Marketo - Última inscrição por indicação em perfil social | Data da última consulta concluída. Gerenciado pelo sistema |
| lastReferredVisit | MarketoSocialLastReferredVisit | Marketo - Última visita por indicação em perfil social | Data da última visita indicada. Gerenciado pelo sistema |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Marketo - Nome de exibição no perfil social do LinkedIn | Nome para exibição do LinkedIn do lead. Sistema preenchido durante a entrada em redes sociais |
| linkedInId | MarketoSocialLinkedInId | ID social do Marketo no LinkedIn | ID do LinkedIn do lead. Sistema preenchido durante a entrada em redes sociais |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | Marketo - URL da foto no perfil social do LinkedIn | URL da foto do LinkedIn do lead. Sistema preenchido durante a entrada em redes sociais |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | Marketo - URL do perfil social do LinkedIn | Perfil de LinkedIn do lead. Sistema preenchido durante a entrada em redes sociais |
| linkedInReach | MarketoSocialLinkedInReach | Marketo - Alcance do perfil social do LinkedIn | Alcance do LinkedIn do lead. Sistema preenchido durante a entrada em redes sociais |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Marketo - Inscrições por indicação no perfil social do LinkedIn | Número de inscrições referenciadas atribuídas ao lead por meio do LinkedIn. Gerenciado pelo sistema |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Marketo - Visitas por indicação no perfil social do LinkedIn | Número de visitas referenciadas atribuídas ao lead por meio do LinkedIn. Gerenciado pelo sistema |
| syndicationId |  - | Marketo - ID de distribuição social | ID do Marketo Social interno do lead. Gerenciado pelo sistema |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Marketo - Total de inscrições por indicação em perfil social | Número total de inscrições de referência concluídas atribuídas ao cliente potencial |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Marketo - Total de visitas por indicação em perfil social | Número total de visitas referenciadas atribuídas ao cliente potencial |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Marketo - Nome de exibição no perfil social do Twitter | Nome de exibição do Twitter do lead. Sistema preenchido durante a entrada em redes sociais |
| twitterId | MarketingSocialTwitterId | ID social do Marketo no Twitter | ID do Twitter do lead. Sistema preenchido durante a entrada em redes sociais |
| twitterPhotoURL | MarketoSocialTwitterPhotoURL | Marketo - URL da foto no perfil social do Twitter | URL da foto do Twitter do lead. Sistema preenchido durante a entrada em redes sociais |
| twitterProfileURL | MarketoSocialTwitterProfileURL | Marketo - URL do perfil social do Twitter | URL do perfil de Twitter do lead. Sistema preenchido durante a entrada em redes sociais |
| twitterReach | MarketoSocialTwitterReach | Marketo - Alcance do perfil social do Twitter | Alcance do Twitter do lead. Sistema preenchido durante a entrada em redes sociais |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Marketo - Inscrições por indicação no perfil social do Twitter | Número de inscrições referenciadas atribuídas ao lead por meio do Twitter. Gerenciado pelo sistema |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Marketo - Visitas por indicação no perfil social do Twitter | Número de visitas referenciadas atribuídas ao lead por meio do Twitter. Gerenciado pelo sistema |
| middleName | MiddleName | Nome do meio | Nome do Meio do Cliente Potencial |
| mobilePhone | MobilePhone | Número do celular | Número de celular do lead |
| numberOfEmployees | NumberOfEmployees | Núm. funcionários | Número de funcionários da empresa do cliente potencial |
| telefone | Telefone | Número de telefone | Número de Telefone do Cliente Potencial |
| postalCode | PostalCode | Código postal | CEP do cliente potencial |
| avaliação | Classificação | Classificação do lead | Avaliação de marketing/vendas do cliente potencial |
| saudação | Saudação | Saudação | Saudação preferida do lead, ou seja, Senhor, Erros... etc. |
| sicCode | SICCode | Código SIC | Código de Classificação Industrial Padrão da empresa do lead |
| site | Site | Site |  |
| state | Estado | Estado | Estado do lead |
| título | Título | Nome do cargo | Cargo do lead |
| cancelado | Inscrição cancelada | Inscrição cancelada | Status de cancelamento de inscrição do lead por email. Parcialmente gerenciado pelo sistema. Impedirá o recebimento de emails não operacionais se definido como verdadeiro. |
| unsubscribedReason | UnsubscribedReason | Motivo do cancelamento de inscrição | Motivo do status de cancelamento de inscrição do lead. Parcialmente gerenciado pelo sistema. Preenchido com informações de email se o lead tiver cancelado a inscrição diretamente de um email do Marketo. |
| site | Site | Site | URL do site da empresa do lead |
| createdAt |  - | Criado em | A hora em que o registro de cliente potencial foi criado inicialmente. Gerenciado pelo sistema |
| updatedAt |  - | Atualizado em | A última vez que o registro de cliente potencial foi atualizado. Gerenciado pelo sistema |
| emailInvalid |  - | E-mail inválido | Status de email inválido. Todos os emails para o endereço serão bloqueados se definidos como true. Rejeições indicando que o email é inválido definirão automaticamente esse campo como verdadeiro. |
| emailInvalidCause |  - | Motivo do e-mail inválido | Causa do status inválido do email. A mensagem de rejeição instigante será registrada neste campo quando o email inválido for definido como verdadeiro. |
| inferredCity |  - | Cidade indicada | Cidade do lead inferida pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredMetropolitanArea |  - | Área metropolitana indicada | Área metropolitana do lead inferida pela pesquisa de IP reverso da primeira visita registrada do lead na Web. |
| inferredPhoneAreaCode |  - | Código de área telefônica indicado | Código de área telefônica do lead inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredPostalCode |  - | Código postal indicado | Código postal do lead inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredStateRegion |  - | Estado/região indicado | Região do estado do lead inferida pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| isAnonymous |  - | É anônimo | Status anônimo do registro de cliente potencial. Gerenciado pelo sistema. |
| prioridade |  - | Prioridade | Prioridade de insight de vendas do cliente potencial. Gerenciado pelo sistema. |
| relativeScore |  - | Pontuação relativa | Pontuação relativa de Sales Insight do lead. Gerenciado pelo sistema. |
| urgência |  - | Urgência | Urgência do insight de vendas do cliente potencial. Gerenciado pelo sistema. |
