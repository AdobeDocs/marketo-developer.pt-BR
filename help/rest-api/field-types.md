---
title: Tipos de campos
feature: REST API
description: Uma lista de tipos de campo do Marketo
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 8%

---

# Tipos de campos

Esta é uma descrição dos tipos de campo no Marketo. Informações adicionais sobre tipos de campo podem ser encontradas [aqui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Informações adicionais sobre limites de tipo de campo podem ser encontradas [aqui](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents).

| Tipo de campo | Descrição | Exemplo |
| --- | --- | --- |
| Data/hora | Usado para inserir uma data e hora. Segue o [formato W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). A prática recomendada é sempre incluir o deslocamento de fuso horário. Data de conclusão, mais horas e minutos: AAAA-MM-DDThh:mm:ssTZD, onde TZD é &quot;+hh:mm&quot; ou &quot;-hh:mm&quot; Observação: algumas APIs de ativos retornam &quot;Z+0000&quot; como TZD para updatedAt e createdAt. | 2010-05-07T15:41:32-05:00 |
| Email | Um campo de sequência de caracteres que aceita endereços de email | example@example.com |
| Flutuante | Um campo de número que contém Números Reais e pode usar uma casa decimal. | 10,4 |
| Inteiro | Números inteiros | 10 |
| Fórmula | Campos cujos valores são gerados pela manipulação de dados de outros campos presentes em um registro de cliente potencial. Elas não são exportadas e não podem ser usadas em Campanhas inteligentes. | Veja este [artigo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Percentual | Uma porcentagem expressa como um número inteiro | 30 |
| URL | Um campo de texto que restringe a entrada para URLs, incluindo o protocolo do URL. | http://example.com/ |
| Telefone | Número de telefone | 111-111-1111 |
| Área de texto | Texto mais longo. | Suporta até 30.000 bytes. Os caracteres ASCII padrão usam 1 byte por caractere (permitindo até 30.000 caracteres). Caracteres Unicode podem usar até 4 bytes por caractere (reduzindo o  número de caracteres permitidos até menos de 30.000 caracteres). |
| Sequência de caracteres | Texto mais curto (até 255 caracteres) | Lorem ipsum dolor sit amet |
| Pontuação | Um campo inteiro que pode ser manipulado com a etapa de fluxo Alterar pontuação | 10 |
| Booleano (caixa de seleção anterior) | Permite aos usuários selecionar um valor Verdadeiro (marcado) ou Falso (desmarcado). | Verdadeiro |
| Moeda | Um campo flutuante que representa o tipo de moeda padrão selecionado para a Assinatura do Marketo | 10,40 |
| Data | Usado para data. Segue o formato W3C. | 07/05/2010 |
| Referência | Um campo de string que contém uma chave para outro registro (uma chave estrangeira). | Empresa do contato |
