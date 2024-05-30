---
title: "Rastreamento de leads"
description: "API de rastreamento de leads"
feature: Munchkin Tracking Code, Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# API de rastreamento de lead

O JavaScript Munchkin da Marketo permite rastrear as visitas da página do usuário final e os cliques nas páginas de aterrissagem do Marketo e páginas da Web externas. Elas são registradas no Marketo como atividades &quot;Visitar página da Web&quot; e &quot;Link clicado na página da Web&quot;, que podem ser usadas em acionadores e filtros para Campanhas inteligentes e Smart Lists.

## Incorporação do código

A instância do Marketo fornece automaticamente trechos de código de rastreamento pré-configurados para incorporar o código nas páginas externas que rastreiam a atividade de volta para a instância do Marketo. O uso do código incorporado é regido por este [contrato de licença](../munchkin-license.pdf).

Há três tipos de código de rastreamento disponíveis:

1. Simples - Carrega de forma síncrona
1. Assíncrono - Carrega de forma assíncrona
1. jQuery assíncrono - carrega de forma assíncrona e requer que o jQuery seja carregado antecipadamente

É altamente recomendável que o código de rastreamento assíncrono seja usado para incorporar o Munchkin em páginas externas. Para garantir a maior taxa de sucesso possível para execução, incorpore o código de rastreamento assíncrono no `<head>` de cada página.

Alguns sistemas de gerenciamento de conteúdo podem ter restrições ou métodos específicos ao incorporar scripts arbitrários.

Para referência, sua página final deve incluir código semelhante a este em `<head>` do seu documento HTML:

```html
<head>
    <script type="text/javascript">
    (function() {
        var didInit = false;
        function initMunchkin() {
            if(didInit === false) {
                didInit = true;
                Munchkin.init('CHANGE-ME');
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
    ...
</head>
```

## Comportamento do Munchkin

O comportamento padrão do Marketo Munchkin é fazer o seguinte no carregamento da página:

1. Verifique se o navegador atual tem um cookie Munchkin e crie um se ele não estiver lá.
1. Envie um evento &quot;Visitar página da Web&quot; para a instância do Marketo designada usando as informações da página atual e do navegador. Isso registra uma atividade no registro correspondente no Marketo.
1. Envie o evento &quot;Link clicado na página da Web&quot; para qualquer clique do usuário que ocorra em links.

O comportamento do Munchkin pode ser modificado através do uso do Munchkin [Definições de configuração](lead-tracking.md#lead-tracking-api), como se um cookie é criado para todos os leads ao visitarem a página com a `cookieAnon` definindo ou modificando o atraso de clique com `clickTime` configuração. O envio da atividade Visit pode ser desabilitado ao definir a configuração apiOnly como true. A partir da versão 162 (agosto de 2022), cliques `tel` e `mailto` os links são rastreados além de `http/s` links.

## Clientes potenciais conhecidos e anônimos

Na primeira visita de um lead a uma página em seu domínio, um novo registro de lead anônimo é criado no Marketo. A chave primária para esse registro é o cookie Munchkin (`_mkto_trk`) que é criado no navegador do usuário. Toda atividade subsequente da Web nesse navegador é registrada em relação a esse registro anônimo. Para ser associado a um registro conhecido no Marketo, uma das seguintes situações deve ocorrer:

- O lead deve visitar uma página rastreada pelo Munchkin com uma `mkt_tok` parâmetro na cadeia de caracteres de consulta de um link de email rastreado do Marketo.
- O cliente em potencial deve preencher um formulário do Marketo.
- UM SOAP [syncLead](../soap-api/leads.md) ou REST [Associar lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) a chamada deve ser enviada.

Quando uma dessas condições é atendida, o cookie e todas as atividades da Web associadas são associados ao lead conhecido.

Um novo registro anônimo de atividade na Web é criado para cada navegador individual; portanto, se um cliente potencial visitar seu domínio pela primeira vez usando um novo computador e/ou navegador, essa associação deverá ocorrer novamente.

## Domínios

O Munchkin cria e rastreia cookies individuais por domínio, de modo que para que o rastreamento de lead conhecido ocorra entre domínios, um evento de associação de lead deve ocorrer para cada domínio. Por exemplo, se eu controlar dois domínios, `marketo.com`, e `example.com`, e um cliente potencial preenche um formulário em `marketo.com`, em seguida, acessa `example.com` posteriormente, em seguida, sua atividade em `marketo.com` é rastreado em um registro de cliente potencial conhecido, mas sua atividade em `example.com` é anônimo. No entanto, os leads conhecidos persistem entre subdomínios, portanto, um lead conhecido em `www.example.com` também é um lead conhecido em `info.example.com`.

Caso o domínio de nível superior tenha duas partes, como `.co.uk`, em seguida, adicione um parâmetro domainLevel ao seu trecho do Munchkin para que o código seja rastreado corretamente. Consulte [aqui](lead-tracking.md#domains) para obter mais detalhes.

## Cookie

O cookie Munchkin usa a chave `_mkto_trk`, e tem um valor seguindo este padrão:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

Os cookies do Munchkin são específicos para cada domínio de segundo nível, ou seja, `example.com`. A duração padrão do cookie é de 2 anos (730 dias).

## Beta

Para participar do canal beta do Munchkin para suas páginas de aterrissagem, acesse seu [Admin -> Peito do tesouro](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) e ative a configuração &quot;Beta do Munchkin em páginas iniciais&quot;. Isso fornece novos snippets de código no **[!UICONTROL Admin]** ->  **[!UICONTROL Munchkin]** para permitir que você use a versão beta em sites externos.

## Recusar

Os visitantes podem recusar totalmente o rastreamento do Munchkin adicionando o `querystring` parâmetro &quot;marketo_opt_out=true&quot; para o URL no navegador. Quando o JavaScript do Munchkin detecta essa configuração, ele tenta definir um novo cookie &quot;mkto_opt_out&quot; com um valor de `true`. Todos os outros cookies de rastreamento do Marketo são excluídos, nenhum cookie novo é definido e nenhuma solicitação HTTP é feita pelo Munchkin quando essa configuração é detectada.
