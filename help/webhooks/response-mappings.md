---
title: Mapeamentos de resposta
feature: Webhooks
description: Mapeamentos de resposta do Marketo
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 1%

---

# Mapeamentos de resposta

O Marketo pode traduzir dados recebidos por um Webhook de dois tipos de conteúdo e retornar esses valores para um campo de cliente potencial: JSON e XML. O parâmetro Campo do Marketo sempre usará o [nome da API SOAP](../rest-api/fields.md) do campo. Cada Webhook pode ter um número ilimitado de mapeamentos de resposta, que são adicionados e editados ao clicar no botão [!UICONTROL Editar] no painel Mapeamentos de Resposta do Webhook:

![Mapeamento de Resposta](assets/response-mapping.png)

Os Mapeamentos de resposta são criados por meio de um emparelhamento de um &quot;Atributo de resposta&quot;, o caminho para a propriedade desejada no documento XML ou JSON e o &quot;Campo do Marketo&quot;, que especifica o campo Lead que tem o valor gravado nele a partir do Atributo de resposta.

As chaves das propriedades devem consistir em caracteres alfanuméricos, traço (-), sublinhado (_), dois pontos (:) e espaço em branco para serem acessados por meio dos mapeamentos de resposta do Marketo.

## Mapeamentos JSON

As propriedades JSON são acessadas com notação de pontos e notação de matriz. A notação de matriz no Marketo não aceitará sequências de caracteres como entrada e aceitará somente números inteiros. Para recuperar dados de um documento JSON, o tipo de resposta deve ser definido como JSON:

```json
{ "foo":"bar"}
```

Para acessar a propriedade `foo` em um mapeamento de resposta, use o `name` da propriedade, pois ela está no primeiro nível do objeto JSON, `foo`. Veja como isso se parece no Marketo:

![Mapeamento de Resposta](assets/json-resp.png)

Este é um exemplo mais complicado com uma matriz:

```json
{
    "profileId" : 1234,
    "firstName" : "Jane",
    "lastName" : "Doe",
    "orders" : [
        {
            "orderId" : 5678,
            "orderDate" : "2015-01-01",
            "orderProductId" : "4982"
        },
        {
            "orderId" : 5678,
            "orderDate" : "2014-05-07",
            "orderProductId" : "4982"
        }
    ]
}
```

Queremos acessar a orderDate a partir do primeiro elemento da matriz orders. Para acessar esta propriedade, use o seguinte: `orders[0].orderDate`

## Mapeamentos XML

Os valores podem ser acessados de elementos individuais em documentos XML. Usa notação de pontos semelhante aos mapeamentos JSON. Vamos ver este exemplo simples:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Para acessar a propriedade foo aqui, use o seguinte: `example.foo`

O elemento de exemplo deve primeiro ser referenciado antes de acessar `foo`. Para acessar uma propriedade, todos os elementos na hierarquia devem ser referenciados no mapeamento. Os documentos XML com arrays são um pouco mais complicados. Use o exemplo a seguir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<elementList>
    <element>
        <foo>baz</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
</elementList>
```

O documento consiste na matriz pai `elementList`, com filhos, elemento que contém uma propriedade: `foo`. Para fins de mapeamentos de resposta do Marketo, a matriz é referenciada como `elementList.element`, para que os filhos de elementList sejam acessados por `elementList.element[i]`. Para obter o valor de foo do primeiro filho de elementList, usamos este atributo de resposta: `elementList.element[0].foo` Isso retorna o valor &quot;baz&quot; ao nosso campo designado. Tentar acessar propriedades dentro de elementos que contêm nomes de elementos exclusivos e não exclusivos resulta em um comportamento indefinido. Cada elemento deve ser uma única propriedade ou uma matriz, os tipos não podem ser misturados.

## Tipos

Ao mapear atributos para campos, você deve garantir que o tipo na resposta do webhook seja compatível com o campo de destino. Por exemplo, se o valor na resposta for uma string e o campo selecionado for do tipo inteiro, o valor não será gravado. Leia sobre [Tipos de Campos](../rest-api/field-types.md).
