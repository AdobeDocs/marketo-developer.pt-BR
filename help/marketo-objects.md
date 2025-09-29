---
title: Objetos do Marketo
feature: Email Programs
description: Guia para usar o Marketo Velocity com leads, oportunidades e objetos personalizados, campos de carregamento, acesso aos 10 principais da lista, relacionamentos com a SFDC e $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Objetos do Marketo

A implementação do Velocity da Marketo pode operar em dados de várias fontes dentro da Marketo: clientes potenciais, oportunidades, objetos personalizados, aplicativo móvel e instalação de aplicativo móvel.

## Carregando campos

Para carregar um campo para uso em um script, esse campo deve ser marcado na lista correspondente no editor de token de script.

Se você não carregar um campo e ele for referenciado no script, a execução do script falhará no tempo de execução. Você pode arrastar e soltar campos do menu de campos no script. Isso permite que eles sejam carregados e adiciona uma referência ao campo no cursor.

## Listas de Oportunidades e Objetos Personalizados

Ao recuperar de Oportunidades ou Objetos personalizados, somente os 10 objetos atualizados mais recentemente de um tipo são carregados. O número de Objetos personalizados disponíveis pode ser aumentado seguindo as etapas descritas aqui. Eles são fornecidos como uma lista, com o nome de `<objectName>List` e são ordenados do registro atualizado mais para o menos recente. Portanto, para acessar o campo Valor da oportunidade que foi atualizada mais recentemente, use o seguinte:

`${OpportunityList.get(0).Amount}`

Neste exemplo, você faz referência ao objeto OpportunityList, usa o método get para acessar o registro indexado em 0 e, em seguida, recupera a propriedade Amount do objeto retornado. Se você arrastar um campo de uma oportunidade ou objeto personalizado para o editor, ele recuperará automaticamente o campo do registro indexado em 0.

## Relações de objetos personalizados do SFDC

Para estar disponível para uso, um objeto personalizado do SFDC deve ter apenas um relacionamento com o lead do Marketo. Os objetos geralmente são vinculados por meio do contato e da conta, portanto, é importante sincronizar objetos somente com o Marketo com a relação lead/contato ativada.

## Objetos do Trigger

Quando uma campanha é acionada por meio da opção Adicionada à Oportunidade, Oportunidade Atualizada ou Adicionada aos acionadores `<Custom Object Name>`, uma variável especial é disponibilizada em Tokens de Script executados no contexto da campanha do acionador: `$TriggerObject ` (sem suporte para `<Custom Object Name>` é o acionador Atualizado).  Se um token usando uma referência `$TriggerObject` for usado em uma campanha em lote, o envio de email falhará, pois esse objeto não está disponível em campanhas em lote de nenhum tipo.  Esta é uma referência ao objeto que acionou a campanha. O objeto contém todos os dados que o registro tem quando acessado por meio de um nome de variável diferente.

Por exemplo, se uma campanha foi acionada por meio de um Objeto personalizado para um pedido de produto, a ordem à qual o cliente potencial foi adicionado é exposta na variável `$TriggerObject`.

Este é um exemplo de script para um email de acompanhamento de pedido:

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

A vantagem de usar a variável `$TriggerObject` é que você não precisa dedicar nenhum código para determinar de quais objetos disponíveis você deseja obter seus dados locais.  O objeto é determinado pela ação de acionamento. Essa é a maneira mais explícita de escolher um objeto para referência e deve ser usada sempre que disponível e apropriada.

Observação: ao usar o `$TriggerObject`, os campos devem ser verificados no painel de edição para que o objeto seja disponibilizado para o script.

Observação 2: `$TriggerObject` funciona somente para acionadores &quot;Adicionados&quot; e não para acionadores &quot;Atualizados&quot;.
