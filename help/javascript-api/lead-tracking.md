---
title: Acompanhamento de lead
description: Saiba como incorporar o Marketo Munchkin JavaScript, rastrear visitas e cliques, gerenciar leads conhecidos e anônimos, cookies entre domínios e recusar campanhas inteligentes.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
TQID: https://experienceleague.adobe.com/nGUcLLgL9X7PBKf2E5IzppDj8e-SyEtxmkQaESd90mE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 718
ht-degree: 0%

---

# API de rastreamento de lead

O Munchkin JavaScript da Marketo rastreia visitas à página e cliques em links em páginas de aterrissagem do Marketo e páginas da Web externas. O Marketo registra essas interações como atividades &quot;Visitar página da Web&quot; e &quot;Link clicado na página da Web&quot;.

Use as atividades em acionadores e filtros para Campanhas inteligentes e Smart Lists.

## Incorporação do código

A instância do Marketo fornece trechos de código pré-configurados para rastrear a atividade de páginas externas. O uso do código de inserção é regido por este [contrato de licença](../munchkin-license.pdf).

Há três tipos de código de rastreamento disponíveis:

1. Simples — Carrega de forma síncrona.
1. Assíncrono — Carrega de forma assíncrona.
1. jQuery assíncrono — carrega de forma assíncrona e requer que o jQuery seja carregado primeiro.

Use o Código de rastreamento assíncrono para incorporar o Munchkin em páginas externas. Para obter a maior taxa de êxito de execução possível, coloque o código no elemento `<head>` de cada página.

Alguns sistemas de gerenciamento de conteúdo podem ter restrições ou métodos específicos ao incorporar scripts arbitrários.

Sua página final deve incluir código semelhante ao seguinte exemplo no elemento `<head>` do documento HTML:

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

Por padrão, o Marketo Munchkin executa as seguintes ações quando uma página é carregada:

1. Verifica se o navegador atual tem um cookie Munchkin e cria um, se necessário.
1. Envia um evento &quot;Visitar página da Web&quot; para a instância do Marketo designada usando informações da página atual e do navegador. Esse evento registra uma atividade no registro Marketo correspondente.
1. Envia um evento &quot;Link clicado na página da Web&quot; quando o usuário seleciona um link.

Use as [Configurações](configuration.md) do Munchkin para alterar esse comportamento. Por exemplo, use `cookieAnon` para controlar se o Munchkin cria um cookie para todos os clientes potenciais que visitam a página, ou use `clickTime` para alterar o atraso de cliques.

Para desabilitar a atividade de Visita, defina `apiOnly` como verdadeiro. A partir da versão 162 (agosto de 2022), o Munchkin rastreia cliques em links `tel` e `mailto`, além de links `http/s`.

## Clientes potenciais conhecidos e anônimos

Quando um lead visita uma página no seu domínio pela primeira vez, o Marketo cria um registro de lead anônimo. A chave primária para este registro é o cookie do Munchkin (`_mkto_trk`) criado no navegador do usuário.

O Marketo registra a atividade subsequente da Web nesse navegador no registro anônimo. Para associar a atividade a um registro Marketo conhecido, um dos seguintes eventos deve ocorrer:

- O cliente potencial deve visitar uma página rastreada pela Munchkin com um parâmetro `mkt_tok` na cadeia de caracteres de consulta de um link de email rastreado da Marketo.
- O cliente em potencial deve preencher um formulário do Marketo.
- Uma chamada REST [Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/associateLeadUsingPOST) deve ser enviada.

Quando um desses eventos ocorre, o Marketo associa o cookie e todas as atividades da Web relacionadas ao lead conhecido.

O Marketo cria um registro anônimo de atividade da Web para cada navegador. Se um cliente potencial visitar seu domínio de um novo computador ou navegador, a associação deverá ocorrer novamente.

## Domínios

O Munchkin cria e rastreia cookies por domínio. Para rastrear um lead conhecido entre domínios, um evento de associação de lead deve ocorrer em cada domínio.

Por exemplo, suponha que você controle `marketo.com` e `example.com`. Um cliente potencial envia um formulário em `marketo.com` e depois vai para `example.com`. A atividade em `marketo.com` está associada ao cliente potencial conhecido, mas a atividade em `example.com` é anônima.

Clientes potenciais conhecidos persistem entre subdomínios. Um cliente potencial conhecido em `www.example.com` também é um cliente potencial conhecido em `info.example.com`.

Se o domínio de nível superior tiver duas partes, como `.co.uk`, adicione um parâmetro `domainLevel` ao seu trecho do Munchkin. Para obter mais informações, consulte [Configuração](configuration.md#domainlevel).

## Cookie

O cookie do Munchkin usa a chave `_mkto_trk` e um valor que segue um destes padrões:

`id:561-HYG-937&token:_mch-marketo.com-1374552656411-90718`

Ou

`id:561-HYG-937&token:_mch-marketo.com-97bf4361ef4433921a6da262e8df45a`

Os cookies do Munchkin são específicos para cada domínio de segundo nível, como `example.com`. A duração padrão do cookie é de 2 anos (730 dias).

## Beta

Para aceitar o canal beta do Munchkin para suas páginas de aterrissagem, acesse [Admin -> Treasure Chest](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) e ative a configuração &quot;Munchkin Beta nas páginas de aterrissagem&quot;.

Esta configuração adiciona trechos de código ao menu **[!UICONTROL Admin]** -> **[!UICONTROL Munchkin]**. Use esses trechos para executar a versão beta em sites externos.

## Recusar

Os visitantes podem recusar o rastreamento do Munchkin adicionando o parâmetro `querystring` &quot;marketo_opt_out=true&quot; ao URL no navegador. Quando o Munchkin JavaScript detecta essa configuração, ele tenta definir um novo cookie &quot;mkto_opt_out&quot; com um valor de `true`.

Em seguida, o Munchkin exclui todos os outros cookies de rastreamento do Marketo, não define novos cookies e não faz solicitações HTTP.
