---
title: Objetos personalizados
feature: SOAP
description: Saiba como os Objetos personalizados do Marketo vinculam um lead a muitos registros, com estrutura, limites e chamadas de API do SOAP para obter, sincronizar, excluir, além de Smart List e uso de email.
exl-id: 29d65841-4b44-4d94-b14e-c583d433d015
TQID: https://experienceleague.adobe.com/Fy3h8qfKs8gizeakXkhoj5mJhib-QA2kYOu41YMIEZs
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 286
ht-degree: 2%

---

# Objetos personalizados

Um objeto personalizado do Marketo permite a criação de uma relação um para muitos entre seus leads do Marketo e os registros do objeto personalizado. Por exemplo, talvez você queira rastrear todos os roadshows acompanhados de um cliente potencial. Como os clientes em potencial podem participar de várias apresentações (ao longo de vários anos), os objetos personalizados são mais adequados para armazenar essas informações.

Objetos personalizados são suportados em todas as edições do Marketo, exceto no Spark. Quando o objeto personalizado do Marketo for provisionado com sucesso em sua conta, você poderá

- Criar/Ler/Atualizar/Excluir registros no objeto personalizado por meio da API do Marketo SOAP
- Usar o acionador de Smart List quando novos registros forem adicionados ao objeto personalizado
- Usar os dados do objeto personalizado como um filtro em Smart Lists
- Usar os dados do objeto personalizado em emails usando o script de email do Marketo

## Estrutura de objetos personalizados

Os objetos personalizados consistem em:

- Um pequeno conjunto de atributos fixos que são comuns a todos os objetos personalizados:
   - Nome do objeto (também conhecido como Nome do tipo de objeto)
   - Descrição do objeto
   - Personalizar objeto para o nome do campo do link de cliente potencial do Marketo - este é um campo no objeto de cliente potencial (Pessoa) ao qual o objeto personalizado faz referência
   - Nome do campo Chave do objeto - a chave primária usada pelo objeto
- Um ou mais campos específicos de objeto (oferecemos suporte a no máximo 50 desses campos)

## Operações de objetos personalizados

As chamadas a seguir podem ser usadas para interagir com COs.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
