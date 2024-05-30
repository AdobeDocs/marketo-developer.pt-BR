---
title: "Objetos personalizados"
feature: SOAP
description: "Criação de objetos personalizados."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Objetos personalizados

Um objeto personalizado do Marketo permite a criação de uma relação um para muitos entre seus leads do Marketo e os registros do objeto personalizado. Por exemplo, talvez você queira rastrear todos os roadshows acompanhados de um cliente potencial. Como os clientes em potencial podem participar de várias apresentações (ao longo de vários anos), os objetos personalizados são mais adequados para armazenar essas informações.

Objetos personalizados são suportados em todas as edições do Marketo, exceto no Spark. Quando o objeto personalizado do Marketo for provisionado com sucesso em sua conta, você poderá

- Criar/Ler/Atualizar/Excluir registros no objeto personalizado por meio da API SOAP do Marketo
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

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
