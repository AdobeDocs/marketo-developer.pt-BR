---
title: Campos padrão
feature: REST API, Field Management
description: Navegue pela lista completa de campos de clientes em potencial padrão do Marketo com nomes, rótulos e descrições de REST e SOAP, além de como recuperá-los por meio da API de descrição de clientes em potencial.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: d674384b3ab979df2322ece3f02155259d05431a
workflow-type: tm+mt
source-wordcount: '727'
ht-degree: 24%

---

# Campos padrão

Esta é uma lista de campos padrão disponíveis no Marketo que podem ser acessados por meio da API.

Você pode recuperar a lista de todos os nomes de campos com suporte disponíveis em seus registros de cliente potencial usando o ponto de extremidade REST [Descrever cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi/).

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
| email | Email | Endereço de email | Endereço de email do lead. Campo de chave padrão do Marketo para registros de cliente potencial |
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
| middleName | MiddleName | Nome do meio | Nome do Meio do Cliente Potencial |
| mobilePhone | MobilePhone | Número do celular | Número de celular do lead |
| numberOfEmployees | NumberOfEmployees | Núm. funcionários | Número de funcionários da empresa do cliente potencial |
| telefone | Telefone | Número de telefone | Número de Telefone do Cliente Potencial |
| postalCode | PostalCode | Código postal | CEP do cliente potencial |
| avaliação | Classificação | Classificação do lead | Avaliação de marketing/vendas do cliente potencial |
| saudação | Saudação | Saudação | Saudação preferida do lead, ou seja, Senhor, Erros... etc. |
| sicCode | SICCode | Código SIC | Código de Classificação Industrial Padrão da empresa do lead |
| site | Site | Site |  |
| estado | Estado | Estado | Estado do lead |
| título | Título | Nome do cargo | Cargo do lead |
| cancelado | Inscrição cancelada | Inscrição cancelada | Status de cancelamento de inscrição do lead por email. Parcialmente gerenciado pelo sistema. Impedirá o recebimento de emails não operacionais se definido como verdadeiro. |
| unsubscribedReason | UnsubscribedReason | Motivo do cancelamento de inscrição | Motivo do status de cancelamento de inscrição do lead. Parcialmente gerenciado pelo sistema. Preenchido com informações de email se o lead tiver cancelado a inscrição diretamente de um email do Marketo. |
| site | Site | Site | URL do site da empresa do lead |
| createdAt |  - | Criado em | A hora em que o registro de cliente potencial foi criado inicialmente. Gerenciado pelo sistema |
| updatedAt |  - | Atualizado em | A última vez que o registro de cliente potencial foi atualizado. Gerenciado pelo sistema |
| emailInvalid |  - | Email inválido | Status de email inválido. Todos os emails para o endereço serão bloqueados se definidos como true. Rejeições indicando que o email é inválido definirão automaticamente esse campo como verdadeiro. |
| emailInvalidCause |  - | Motivo do e-mail inválido | Causa do status inválido do email. A mensagem de rejeição instigante será registrada neste campo quando o email inválido for definido como verdadeiro. |
| inferredCity |  - | Cidade indicada | Cidade do lead inferida pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredMetropolitanArea |  - | Área metropolitana indicada | Área metropolitana do lead inferida pela pesquisa de IP reverso da primeira visita registrada do lead na Web. |
| inferredPhoneAreaCode |  - | Código de área telefônica indicado | Código de área telefônica do lead inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredPostalCode |  - | Código postal indicado | Código postal do lead inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredStateRegion |  - | Estado/região indicado | Região do estado do lead inferida pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| isAnonymous |  - | É anônimo | Status anônimo do registro de cliente potencial. Gerenciado pelo sistema. |
| prioridade |  - | Prioridade | Prioridade de Vendas Insight do cliente potencial. Gerenciado pelo sistema. |
| relativeScore |  - | Pontuação relativa | Pontuação relativa de Vendas Insight do lead. Gerenciado pelo sistema. |
| urgência |  - | Urgência | Urgência de vendas do cliente potencial no Insight. Gerenciado pelo sistema. |
