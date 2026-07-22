---
title: Objetos do Marketo
feature: Email Programs
description: Guia para usar o Marketo Velocity com leads, oportunidades e objetos personalizados, campos de carregamento, acesso aos 10 principais da lista, relacionamentos com a SFDC e $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
TQID: https://experienceleague.adobe.com/PvLJb-AOk6DKaNINycpzk5ojZiL8UNcanRg3vXmsGCI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 452
ht-degree: 1%

---

# Objetos do Marketo

A implementação do Velocity da Marketo pode usar dados destas fontes da Marketo:

- Leads
- Oportunidades
- Objetos personalizados
- Aplicativo móvel
- Instalação do aplicativo móvel

## Carregando campos

Para usar um campo em um script, selecione o campo na lista correspondente no editor de token de script.

Se um script referenciar um campo que não está carregado, o script falhará no tempo de execução. Arraste um campo do menu de campos para o script para carregá-lo e adicionar uma referência no cursor.

## Listas de Oportunidades e Objetos Personalizados

Para Oportunidades e Objetos personalizados, o Marketo carrega somente os 10 objetos atualizados mais recentemente de cada tipo. Você pode aumentar o número de Objetos personalizados disponíveis seguindo as etapas descritas aqui.

O Marketo fornece os objetos em uma lista chamada `<objectName>List`, ordenados do registro atualizado mais recentemente para o registro atualizado menos recentemente. Para acessar o campo Valor da oportunidade atualizada mais recentemente, use:

`${OpportunityList.get(0).Amount}`

Este exemplo faz referência ao objeto OpportunityList, usa o método get para acessar o registro no índice 0 e recupera a propriedade Amount desse registro.

Ao arrastar um campo Oportunidade ou Objeto personalizado para o editor, o Marketo recupera automaticamente o campo do registro no índice 0.

## Relações de objetos personalizados do SFDC

Para usar um objeto personalizado do SFDC, o objeto deve ter apenas um relacionamento com o lead do Marketo. Os objetos geralmente são vinculados por meio do contato e da conta. Sincronizar somente objetos que tenham a relação cliente potencial/contato habilitada.

## Objetos do Trigger

Quando uma campanha usa o acionador Adicionado à Oportunidade, Oportunidade é Atualizada ou Adicionada ao `<Custom Object Name>`, a variável `$TriggerObject` fica disponível para Tokens de Script que são executados na campanha do acionador. Esta variável não tem suporte para o gatilho `<Custom Object Name>` is Updated.

Essa variável faz referência ao objeto que acionou a campanha. Ele contém os mesmos dados de registro que estão disponíveis quando você acessa o objeto por meio de outro nome de variável.

Não use um token que faça referência a `$TriggerObject` em uma campanha em lote. O objeto não está disponível em campanhas em lote e o envio de email falha.

Por exemplo, se um Objeto personalizado de um pedido de produto acionar uma campanha, a variável `$TriggerObject` expõe a ordem à qual o cliente potencial foi adicionado.

O exemplo a seguir mostra um script para um email de acompanhamento de pedido:

```html
<div>
<strong>Your order information:</strong>
##pull information from the Triggering Order and format it in a list
<ul>
<li>Product Ordered: $!{TriggerObject.ProductName}</li>
<li>Product Quantity: $!{TriggerObject.Quanitity}</li>
<li>Shipping Address: $!{TriggerObject.ShippingAddress}</li>
<li>Billing Address: $!{TriggerObject.BillingAddress}</li>
<li>Order Total: $!{TriggerObject.Amount}</li>
</ul>
<p><a href="$!{TriggerObject.OrderURL}">View Your Order Online</a></p>
</div>
```

A ação de acionamento determina o objeto. Não é necessário código adicional para determinar qual objeto disponível contém os dados locais. Use `$TriggerObject` quando estiver disponível e for apropriado porque ele identifica explicitamente o objeto para referência.

Observação: ao usar `$TriggerObject`, selecione os campos do objeto no painel de edição para torná-los disponíveis para o script.

Observação 2: `$TriggerObject` funciona somente para acionadores &quot;Adicionados&quot;, não para acionadores &quot;Atualizados&quot;.
