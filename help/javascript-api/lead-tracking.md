---
title: Acompanhamento de lead
description: API de rastreamento de lead
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 8ad3e3f0958ea705375651b1c8a75967d807ca80
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 0%

---

# API de rastreamento de lead

O Munchkin JavaScript da Marketo permite rastrear as visitas da página do usuário final e os cliques nas páginas de aterrissagem do Marketo e páginas da Web externas. Elas são registradas no Marketo como atividades &quot;Visitar página da Web&quot; e &quot;Link clicado na página da Web&quot;, que podem ser usadas em acionadores e filtros para Campanhas inteligentes e Smart Lists.

## Incorporação do código

A instância do Marketo fornece automaticamente trechos de código de rastreamento pré-configurados para incorporar o código nas páginas externas que rastreiam a atividade de volta para a instância do Marketo. O uso do código de inserção é regido por este [contrato de licença](../munchkin-license.pdf).

Há três tipos de código de rastreamento disponíveis:

1. Simples - Carrega de forma síncrona
1. Assíncrono - Carrega de forma assíncrona
1. jQuery assíncrono - carrega de forma assíncrona e requer que o jQuery seja carregado antecipadamente

É altamente recomendável que o Código de rastreamento assíncrono seja usado para incorporar o Munchkin em páginas externas. Para garantir a maior taxa de sucesso possível para execução, incorpore o código de rastreamento assíncrono em `<head>` de cada página.

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

O comportamento do Munchkin pode ser modificado com o uso das [Configurações](configuration.md) do Munchkin, por exemplo, se um cookie será criado para todos os clientes potenciais ao visitar a página com a configuração `cookieAnon` ou modificar o atraso de cliques com a configuração `clickTime`. O envio da atividade Visit pode ser desabilitado ao definir a configuração apiOnly como true. A partir da versão 162 (agosto de 2022), os links de cliques `tel` e `mailto` serão rastreados além dos links `http/s`.

## Clientes potenciais conhecidos e anônimos

Na primeira visita de um lead a uma página em seu domínio, um novo registro de lead anônimo é criado no Marketo. A chave primária para este registro é o cookie do Munchkin (`_mkto_trk`) que é criado no navegador do usuário. Toda atividade subsequente da Web nesse navegador é registrada em relação a esse registro anônimo. Para ser associado a um registro conhecido no Marketo, uma das seguintes situações deve ocorrer:

- O cliente potencial deve visitar uma página rastreada pela Munchkin com um parâmetro `mkt_tok` na cadeia de caracteres de consulta de um link de email rastreado da Marketo.
- O cliente em potencial deve preencher um formulário do Marketo.
- Uma chamada REST [Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) deve ser enviada.

Quando uma dessas condições é atendida, o cookie e todas as atividades da Web associadas são associados ao lead conhecido.

Um novo registro anônimo de atividade na Web é criado para cada navegador individual; portanto, se um cliente potencial visitar seu domínio pela primeira vez usando um novo computador e/ou navegador, essa associação deverá ocorrer novamente.

## Domínios

O Munchkin cria e rastreia cookies individuais com base no domínio; portanto, para que o rastreamento de cliente potencial conhecido ocorra entre domínios, um evento de associação de cliente potencial deve ocorrer para cada domínio. Por exemplo, se eu controlar dois domínios, `marketo.com` e `example.com`, e um cliente potencial preencher um formulário em `marketo.com`, depois navegar para `example.com`, sua atividade em `marketo.com` será rastreada em um registro de cliente potencial conhecido, mas sua atividade em `example.com` será anônima. Os clientes em potencial conhecidos persistem entre subdomínios, portanto, um cliente em potencial conhecido em `www.example.com` também é um cliente em potencial conhecido em `info.example.com`.

Caso o domínio de nível superior tenha duas partes, como `.co.uk`, adicione um parâmetro domainLevel ao trecho do Munchkin para que o código seja rastreado corretamente. Consulte [aqui](configuration.md#domainlevel) para obter mais detalhes.

## Cookie

O cookie do Munchkin usa a chave `_mkto_trk` e tem um valor seguindo este padrão:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

ou

`id:561\-HYG\-937&token:_mch\-marketo.com\-97bf4361ef4433921a6da262e8df45a`

Os cookies do Munchkin são específicos para cada domínio de segundo nível, ou seja, `example.com`. A duração padrão do cookie é de 2 anos (730 dias).

## Beta

Para aceitar o canal beta do Munchkin para suas páginas de aterrissagem, acesse o menu [Admin -> Treasure Chest](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) e ative a configuração &quot;Munchkin Beta nas páginas de aterrissagem&quot;. Isso fornece novos trechos de código no **[!UICONTROL Administrador]** ->  Menu **[!UICONTROL Munchkin]** para permitir que você use a versão beta em sites externos.

## Recusar

Os visitantes podem recusar totalmente o rastreamento do Munchkin adicionando o parâmetro `querystring` &quot;marketo_opt_out=true&quot; ao URL no navegador. Quando o Munchkin JavaScript detecta essa configuração, ele tenta definir um novo cookie &quot;mkto_opt_out&quot; com um valor de `true`. Todos os outros cookies de rastreamento do Marketo são excluídos, nenhum novo cookie é definido e nenhuma solicitação HTTP é feita pelo Munchkin quando essa configuração é detectada.
