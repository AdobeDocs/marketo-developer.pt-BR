---
title: Contexto do usuário
feature: REST API
description: Visão geral do contexto do usuário e descrições da API
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 5%

---

# Contexto do usuário

Contexto do usuário A API do JavaScript expõe os dados no nível do usuário e do visitante em várias sessões para ativar o recurso de personalização avançada usando o comportamento e os dados históricos do usuário. A API vai além da leitura de dados e expõe variáveis personalizadas que permitem enviar dados e eventos significativos para o back-end do RTP para fins avançados de segmentação e personalização. Recursos adicionais: [Triggers](../javascript-api/triggers.md), [Correspondência de Padrões](../javascript-api/pattern-match.md).

- Você deve se tornar um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.
- A API de contexto de usuário é um recurso que deve ser ativado pelo Suporte da Marketo mediante solicitação. Quando a API estiver ativada, um objeto userContext no objeto global RTP será exposto.

## Atributos de Contexto do Usuário

| Nome | Tipo | Descrição |
|------------------|-------------|------|
| customVar[1-5] | Sequência de caracteres | Dados personalizados salvos no contexto do usuário. |
| viewedCampaigns | IDs de campanha como sequência separada por vírgulas | Campanhas visualizadas nas visitas atuais ou anteriores. |
| Campanhas clicadas | IDs de campanha como sequência separada por vírgulas | Clicou por meio de campanhas em visitas atuais ou anteriores. |

## Definir variáveis personalizadas

Adicionar dados personalizados ao Contexto do Usuário.

### Uso

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|-----------------|-------------------|--------|-----------------|
| &#39;definir&#39; | Obrigatório | Sequência de caracteres | Ação do método. |
| customVar | Obrigatório | Sequência de caracteres | Nome da variável personalizada. |
| my_custom_value | Obrigatório | Sequência de caracteres | Valor personalizado a ser salvo na variável personalizada no índice de 1 a 5. |

Observação: as variáveis personalizadas são enviadas ao RTP somente na chamada de exibição, portanto, é recomendável definir variáveis personalizadas antes que a exibição seja chamada. Caso contrário, ele será enviado somente na próxima chamada de visualização.

Restrições de Var personalizadas

- O comprimento da variável personalizada não pode ultrapassar 100 caracteres.
- Os dados da campanha são limitados às últimas dez visitas, com dez campanhas por visita.

### Uso

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');
 
// Read location 
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}
 
// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
