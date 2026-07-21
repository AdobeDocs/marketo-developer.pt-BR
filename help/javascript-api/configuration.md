---
title: Configuração
description: Configure o Marketo Munchkin com a API do JavaScript. Saiba mais sobre as configurações do Munchkin.init como altIds, anonymizeIP, asyncOnly, vida do cookie, domainLevel, API do Beacon.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
TQID: https://experienceleague.adobe.com/ip2cCGgoa83v8m9GYLYXe132veYxS1C6UWX1iLB6X5Q
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 541
ht-degree: 5%

---

# Configuração

O Munchkin aceita configurações que personalizam seu comportamento. Passe as configurações como propriedades de um objeto JavaScript no segundo parâmetro de [Munchkin.init()](api-reference.md#munchkin_init).

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

O objeto de definições de configuração pode conter qualquer número de propriedades na tabela a seguir.

## Propriedades

| Nome | Tipo de dados | Descrição |
| --- | --- | --- |
| altIds | Matriz | Aceita uma matriz de sequências de caracteres da Munchkin ID. Quando ativado, duplica todas as atividades da Web para as assinaturas identificadas pelas Munchkin IDs. |
| anonymizeIP | Booleano | Torna anônimo o endereço IP registrado no Marketo para novos visitantes. |
| apiOnly | Booleano | Se definida como true, a função `Munchkin.Init()` não chamará `visitsWebPage`. Isso é útil para aplicativos web de página única que precisam de controle total sobre cada evento `visitsWebPage`. |
| asyncOnly | Booleano | Se definido como verdadeiro, envia XMLHttpRequests de forma assíncrona. O padrão é falso. |
| clickTime | Inteiro | Define o tempo, em milissegundos, a ser bloqueado após um clique para que a solicitação de rastreamento de cliques possa ser concluída. A redução desse valor reduz a precisão do rastreamento de cliques. O padrão é 350 ms. |
| cookieAnon | Booleano | Se definido como falso, impede o rastreamento e a criação de cookies para novos leads anônimos. Os clientes potenciais recebem cookies e são rastreados depois de enviar um formulário do Marketo ou clicar em um email do Marketo. O padrão é verdadeiro. |
| cookieLifeDays | Inteiro | Define a data de expiração de qualquer cookie de rastreamento do Munchkin recém-criado para este número de dias no futuro. O padrão é 730 dias (2 anos). |
| customName | String | Nome de página personalizado. Somente para uso do sistema. |
| <a name="domainlevel"></a>domainLevel | Inteiro | Define quantas partes do domínio da página devem ser usadas para o atributo de domínio do cookie.<br><br>Para &quot;www.example.com&quot;, `domainLevel: 2` define o domínio do cookie como &quot;.example.com&quot; e `domainLevel: 3` o define como &quot;.www.example.com&quot;.<br><br>Por padrão, o Munchkin usa duas partes quando o domínio de nível superior tem três letras. Por exemplo, &quot;www.example.com&quot; usa &quot;.example.com&quot;.<br><br>Para códigos de país de duas letras, como &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; e &quot;.uk&quot;, o Munchkin usa três partes. Por exemplo, &quot;www.example.co.jp&quot; usa &quot;.example.co.jp&quot;.<br><br>Use o parâmetro `domainLevel` quando o padrão de domínio exigir comportamento diferente. |
| domainSelectorV2 | Booleano | Se definido como verdadeiro, o utiliza um método aprimorado para determinar como definir o atributo de domínio do cookie. |
| httpsOnly | Booleano | O padrão é false. Quando definido como true, define o cookie para usar a configuração Secure quando a página rastreada foi veiculada via https. |
| useBeaconAPI | Booleano | O padrão é false. Quando definido como verdadeiro, usa a [API Beacon](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) para enviar solicitações de não bloqueio em vez de [XMLHttpRequest](https://developer.mozilla.org/pt-BR/docs/Web/API/XMLHttpRequest). Se o navegador não for compatível com a API Beacon, o Munchkin usará XMLHttpRequest. |
| wsInfo | String | Segmenta um espaço de trabalho. Obtenha a ID do espaço de trabalho selecionando o espaço de trabalho no menu Admin > Integração > Munchkin.<br><br>Esta configuração se aplica somente quando um registro de cliente potencial anônimo é criado inicialmente. Depois que o valor do cookie do Munchkin é estabelecido para esse registro de lead, o parâmetro wsInfo não pode alterar sua partição.<br><br>Como essa configuração afeta somente clientes potenciais anônimos, ela é relevante somente para [Visitantes Anônimos em Relatórios da Web](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) específicos da partição. |

## Exemplos

### Enviar atividade para várias assinaturas

Este exemplo envia toda a atividade da Web para as instâncias com Munchkin IDs &quot;AAA-BBB-CCC&quot; e &quot;XXX-YYY-ZZZ&quot;.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCCC', { 'altIds': ['XXX-YYY-ZZZ'] });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

### Definir rastreamento para assíncrono

Este exemplo força todos os XMLHttpRequests a serem enviados de forma assíncrona a partir do thread principal.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCC', { 'asyncOnly': true });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin-beta.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```
