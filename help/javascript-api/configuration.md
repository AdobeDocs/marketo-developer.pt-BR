---
title: Configuração
description: Configure o Marketo Munchkin com a API do JavaScript. Saiba mais sobre as configurações do Munchkin.init como altIds, anonymizeIP, asyncOnly, vida do cookie, domainLevel, API do Beacon.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 3%

---

# Configuração

O Munchkin pode aceitar várias definições de configuração para personalizar o comportamento. As definições de configuração são propriedades de um objeto JavaScript passado como segundo parâmetro ao chamar [Munchkin.init()](api-reference.md#munchkin_init)

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

O objeto de definições de configuração pode conter qualquer número de propriedades da tabela abaixo.

## Propriedades

| Nome | Tipo de dados | Descrição |
|---|---|---|
| altIds | Matriz | Aceita uma matriz de sequências de caracteres da Munchkin ID. Quando ativado, duplica todas as atividades da Web nas assinaturas direcionadas, com base na Munchkin ID. |
| anonymizeIP | Booleano | Torna anônimo o endereço IP registrado no Marketo para novos visitantes. |
| apiOnly | Booleano | Se definida como true, a função `Munchkin.Init()` não chamará `visitsWebPage`. Isso é útil para aplicativos web de página única que precisam de controle total sobre cada evento `visitsWebPage`. |
| asyncOnly | Booleano | Se definido como verdadeiro, envia o de XMLHttpRequest de forma assíncrona. O padrão é falso. |
| clickTime | Inteiro | Define o tempo a ser bloqueado após um clique para permitir a solicitação de rastreamento de cliques (em milissegundos). A redução desse número reduz a precisão do rastreamento de cliques. O padrão é 350 ms. |
| cookieAnon | Booleano | Se definido como falso, impede o rastreamento e a criação de cookies de novos leads anônimos. Os clientes potenciais têm cookies e são rastreados após preencher um formulário do Marketo ou clicando em um email do Marketo. O padrão é verdadeiro. |
| cookieLifeDays | Inteiro | Define a data de expiração de qualquer cookie de rastreamento do Munchkin recém-criado para este número de dias no futuro. O padrão é 730 dias (2 anos). |
| customName | String | Nome de página personalizado. Somente para uso do sistema. |
| <a name="domainlevel"></a>domainLevel | Inteiro | Define o número de partes do domínio da página a serem usadas ao definir o atributo de domínio do cookie. Por exemplo, suponha que o domínio da página atual seja &quot;www.example.com&quot;.domainLevel: 2 definirá o atributo de domínio do cookie como &quot;.example.com&quot;domainLevel: 3 definirá o atributo de domínio do cookie como &quot;.www.example.com&quot;Background:Munchkin gerenciará automaticamente determinados domínios de nível superior com duas letras. O padrão é duas partes nos casos normais em que o domínio de nível superior é de três letras. Por exemplo &quot;www.example.com&quot;, as duas partes mais à direita são usadas para definir o cookie, &quot;.example.com&quot;.Para códigos de país com duas letras, como &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; e &quot;.uk&quot;, o código assume três partes como padrão. Por exemplo, &quot;www.example.co.jp&quot; usará três partes de domínio mais à direita, &quot;.example.co.jp&quot;. Se o padrão de domínio exigir um comportamento diferente, isso deverá ser especificado usando o parâmetro `domainLevel`. |
| domainSelectorV2 | Booleano | Se definido como verdadeiro, o utiliza um método aprimorado para determinar como definir o atributo de domínio do cookie. |
| httpsOnly | Booleano | O padrão é false. Quando definido como true, define o cookie para usar a configuração Secure quando a página rastreada foi veiculada via https. |
| useBeaconAPI | Booleano | O padrão é false. Quando definido como verdadeiro, usa a [API Beacon](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) para enviar solicitações de não bloqueio em vez de [XMLHttpRequest](https://developer.mozilla.org/pt-BR/docs/Web/API/XMLHttpRequest). Se o navegador não suportar essa API, o Munchkin voltará a usar XMLHttpRequest. |
| wsInfo | String | Usa uma string para direcionar um espaço de trabalho. Essa ID de espaço de trabalho é obtida selecionando o Workspace no menu Admin > Integração > Munchkin. Essa configuração se aplica somente à criação inicial de um registro de lead anônimo. Depois que o valor do cookie do Munchkin tiver sido estabelecido para esse registro de lead, o parâmetro wsInfo não poderá ser usado para alterar sua partição. Como essa configuração afeta apenas clientes potenciais anônimos, ela só é relevante para [Visitantes Anônimos em Relatórios da Web](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) específicos da partição. |

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

Este exemplo força todos os XMLHttpRequest a serem enviados de forma assíncrona a partir do thread principal.

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
