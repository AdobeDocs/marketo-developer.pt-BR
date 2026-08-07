---
title: Mapeamentos de resposta
feature: Webhooks
description: O Marketo Webhooks responde a mapeamentos para JSON e XML, mapeia atributos para campos de cliente potencial, notação de pontos e matriz e compatibilidade de tipo.
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
TQID: https://experienceleague.adobe.com/-OGDeKLPS1KmWGIKj6BGq5DGXoCSj5ip-dVr7-kKDro
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: bcf56d2102f2f60eac5ad3318d348fd020391e6b
workflow-type: tm+mt
source-wordcount: 370
ht-degree: 1%

---

# Mapeamentos de resposta

O Marketo pode traduzir dados de webhook de JSON ou XML e gravar os valores para campos de cliente potencial. O parâmetro Campo do Marketo sempre usa o nome da API do SOAP do campo.

Cada webhook pode ter um número ilimitado de mapeamentos de resposta. Para adicionar ou editar mapeamentos, selecione [!UICONTROL Editar] no painel Mapeamentos de Resposta do webhook:

![Mapeamento de Resposta](assets/response-mapping.png)

Um mapeamento de resposta emparelha esses valores:

- &quot;Atributo de resposta&quot;: o caminho para a propriedade desejada no documento XML ou JSON.
- &quot;Campo do Marketo&quot;: o campo de cliente potencial no qual o Marketo grava o valor do atributo de resposta.

Para acessar uma propriedade por meio dos mapeamentos de resposta do Marketo, sua chave deve conter apenas caracteres alfanuméricos, traço (-), sublinhado (_), dois pontos (:) e espaço em branco.

## Mapeamentos JSON

Acesse propriedades JSON com a notação de pontos e a notação de matriz. A notação de matriz Marketo aceita apenas números inteiros, não sequências de caracteres.

Para recuperar dados de um documento JSON, defina o tipo de resposta como JSON:

```json
{ "foo":"bar"}
```

A propriedade `foo` está no primeiro nível do objeto JSON. Usar sua propriedade `name`, `foo`, no mapeamento de resposta:

![Mapeamento de Resposta](assets/json-resp.png)

O exemplo a seguir contém uma matriz:

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

Para acessar orderDate a partir do primeiro elemento da matriz orders, use `orders[0].orderDate`.

## Mapeamentos XML

Acesse valores de elementos XML individuais usando a notação de pontos, semelhante aos mapeamentos JSON. Considere este exemplo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Para acessar a propriedade foo, use `example.foo`.

Referencie o elemento de exemplo antes de acessar `foo`. Um mapeamento deve fazer referência a cada elemento na hierarquia de propriedades.

Para um documento XML com uma matriz, considere este exemplo:

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

A matriz pai é `elementList`. Cada elemento filho contém a propriedade `foo`. Os mapeamentos de resposta do Marketo fazem referência à matriz como `elementList.element` e acessam seus filhos por meio de `elementList.element[i]`.

Para obter o valor de foo do primeiro filho de elementList, use o atributo de resposta `elementList.element[0].foo`. Esse mapeamento retorna o valor &quot;baz&quot; para o campo designado.

Acessar propriedades dentro de elementos que contêm nomes de elementos exclusivos e não exclusivos produz comportamento indefinido. Cada elemento deve ser uma única propriedade ou uma matriz. Não misture os tipos.

## Tipos

Ao mapear atributos para campos, verifique se o tipo de resposta do webhook é compatível com o campo de destino. Por exemplo, o Marketo não grava um valor de resposta de string em um campo do tipo inteiro. Para obter mais informações, consulte [Tipos de Campo](../rest-api/field-types.md).
