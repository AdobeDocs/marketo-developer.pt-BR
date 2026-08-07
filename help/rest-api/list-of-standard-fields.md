---
title: Campos padrão
feature: REST API, Field Management
description: Navegue pela lista completa de campos de clientes potenciais padrão do Marketo com nomes REST, rótulos e descrições, além de como recuperá-los por meio da API Descrever lead.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
TQID: https://experienceleague.adobe.com/vu2wGk36XJ243vwavhfLE7Vc9vMIJKGx6vmVqMRgEDA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: bcf56d2102f2f60eac5ad3318d348fd020391e6b
workflow-type: tm+mt
source-wordcount: 688
ht-degree: 19%

---

# Campos padrão

A tabela a seguir lista os campos padrão do Marketo disponíveis por meio da API. Ele inclui o nome, o rótulo e a descrição da API REST de cada campo.

Use o ponto de extremidade REST [Descrever Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi) para recuperar todos os nomes de campo suportados pelos seus registros de cliente potencial.

| Nome da API REST | Rótulo intuitivo | Descrição |
| --- | --- | --- |
| endereço | Endereço | Endereço do lead |
| annualRevenue | Receita anual | Receita anual da empresa do cliente potencial |
| anonymousIP | IP anônimo | Endereço IP da primeira visita da Web registrada do lead |
| billingCity | Cidade de cobrança | Cidade do endereço de cobrança do lead |
| billingCountry | País de cobrança | País do endereço de cobrança do lead |
| billingPostalCode | Código postal de cobrança | Código postal do endereço de cobrança do lead |
| billingState | Estado de cobrança | Estado ou província do endereço de cobrança do lead |
| billingStreet | Endereço de cobrança | Endereço de cobrança da empresa do lead |
| city | Cidade | Cidade do lead |
| empresa | Nome da empresa | Nome da empresa do lead |
| país | País | País do lead |
| dateOfBirth | Data de nascimento | Data de nascimento do lead |
| departamento | Departamento | Departamento do líder na empresa |
| doNotCall | Não ligar | Preferência de cliente em potencial para não chamar |
| doNotCallReason | Motivo para não ligar | Explicação da preferência de lead do tipo &quot;não chamar&quot; |
| email | Endereço de email | Endereço de email do lead. Campo de chave padrão do Marketo para registros de cliente potencial |
| fax | Número de fax | Número de fax do cliente potencial |
| firstName | Nome | Nome do lead |
| indústria | Setor | Setor de lead |
| inferredCompany | Empresa indicada | Nome da empresa inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead |
| inferredCountry | País indicado | País inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead |
| lastName | Sobrenome | Sobrenome do Cliente Potencial |
| leadRole | Função | Função do lead em sua empresa |
| leadScore | Pontuação do lead | Pontuação inteira atribuída ao lead por meio da pontuação de campanhas e programas |
| leadSource | Fonte do lead | Campo que registra de qual origem o lead se originou |
| leadStatus | Status do lead | Campo que registra o status atual de marketing/vendas do cliente potencial |
| mainPhone | Telefone principal | Número de telefone principal da empresa do cliente potencial |
| jigsawContactId | Marketo - ID Data.com | ID Data.com do lead, se disponível |
| jigsawContactStatus | Marketo - Status Data.com | Status Data.com do lead, se disponível |
| middleName | Nome do meio | Nome do Meio do Cliente Potencial |
| mobilePhone | Número do celular | Número de celular do lead |
| numberOfEmployees | Núm. funcionários | Número de funcionários da empresa do cliente potencial |
| telefone | Número de telefone | Número de Telefone do Cliente Potencial |
| postalCode | Código postal | CEP do cliente potencial |
| avaliação | Classificação do lead | Avaliação de marketing/vendas do cliente potencial |
| saudação | Saudação | Saudação preferida do lead, que é Senhor, Misses... e assim por diante |
| sicCode | Código SIC | Código de Classificação Industrial Padrão da empresa do lead |
| site | Site |  |
| estado | Estado | Estado do lead |
| título | Nome do cargo | Cargo do lead |
| cancelado | Inscrição cancelada | Status de cancelamento de inscrição do lead por email. Parcialmente gerenciado pelo sistema. Impedirá o recebimento de emails não operacionais se definido como verdadeiro. |
| unsubscribedReason | Motivo do cancelamento de inscrição | Motivo do status de cancelamento de inscrição do lead. Parcialmente gerenciado pelo sistema. Preenchido com informações de email se o lead tiver cancelado a inscrição diretamente de um email do Marketo. |
| site | Site | URL do site da empresa do lead |
| createdAt | Criado em | A hora em que o registro de cliente potencial foi criado inicialmente. Gerenciado pelo sistema |
| updatedAt | Atualizado em | A última vez que o registro de cliente potencial foi atualizado. Gerenciado pelo sistema |
| emailInvalid | Email inválido | Status de email inválido. Todos os emails para o endereço serão bloqueados se definidos como true. Rejeições indicando que o email é inválido definirão automaticamente esse campo como verdadeiro. |
| emailInvalidCause | Motivo do e-mail inválido | Causa do status inválido do email. A mensagem de rejeição instigante será registrada neste campo quando o email inválido for definido como verdadeiro. |
| inferredCity | Cidade indicada | Cidade do lead inferida pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredMetropolitanArea | Área metropolitana indicada | Área metropolitana do lead inferida pela pesquisa de IP reverso da primeira visita registrada do lead na Web. |
| inferredPhoneAreaCode | Código de área telefônica indicado | Código de área telefônica do lead inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredPostalCode | Código postal indicado | Código postal do lead inferido pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| inferredStateRegion | Estado/região indicado | Região do estado do lead inferida pela pesquisa de IP reverso da primeira visita da Web registrada do lead. |
| isAnonymous | É anônimo | Status anônimo do registro de cliente potencial. Gerenciado pelo sistema. |
| prioridade | Prioridade | Prioridade de Vendas Insight do cliente potencial. Gerenciado pelo sistema. |
| relativeScore | Pontuação relativa | Pontuação relativa de Vendas Insight do lead. Gerenciado pelo sistema. |
| urgência | Urgência | Urgência de vendas do cliente potencial no Insight. Gerenciado pelo sistema. |
