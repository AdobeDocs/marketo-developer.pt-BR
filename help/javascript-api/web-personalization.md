---
title: Personalização na web
description: Guia da API do JavaScript e da tag RTP do Web Personalization, abordando eventos de exibição de página, configuração de conta, exclusões de bot e scripts principais e sob demanda
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
TQID: https://experienceleague.adobe.com/yplunKmgjOJ7gJTA2TDc9cfJXyXbrVWuM-NdVbDMN4A
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2: id: cdd4e0f6-e87e-453f-88ee-2ee54a7de272
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 435
ht-degree: 6%

---

# Personalização na web

A API do JavaScript do Web Personalization rastreia eventos e personaliza dinamicamente páginas da Web. Ele estende os recursos de personalização automatizada da plataforma.

Os recursos relacionados incluem [Eventos de Dados Personalizados](custom-data-events.md), [Conteúdo Dinâmico](web-personalization.md), [Obter Dados do Visitante](get-visitor-data.md) e [Excluir Marca para Bots Específicos](#exclude_tag_for_specific_bots).

- Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.
- O RTP não suporta listas de contas nomeadas de Marketing Baseado em Conta. As listas e os códigos ABM pertencem apenas às listas de contas carregadas (arquivos CSV) gerenciadas no RTP.

## Configuração de tag

Insira a tag RTP no cabeçalho de cada página personalizada.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
(function(c,h,a,f,e,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].p=e;c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b)})
(window,document,"rtp","[rtp-js-cdn-url]","[pod-url]","[accountId]");
</script>
<!-- End of RTP tag -->
```

## Configuração da conta

A tag chama automaticamente esse método para definir a ID de conta relevante. Defina a ID da conta explicitamente quando quiser usar contas diferentes para domínios diferentes.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| &#39;setAccount&#39; | Obrigatório | String | Nome do método. |
| accountId | Obrigatório | String | ID da conta. |

```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funções de envio de eventos

Esse método envia um evento de exibição para o rastreamento de página. A primeira chamada no exemplo a seguir rastreia o URL da página atual como uma exibição de página do visitante.

Passe o parâmetro opcional &quot;page&quot; para substituir a página atual, como mostrado na segunda chamada.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| &#39;enviar&#39; | Obrigatório | String | Ação do método. |
| &#39;exibir&#39; | Obrigatório | String | Nome do método. |
| página | Opcional | String | Caminho relativo ou URL da página inteira. |

```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Excluir tag para bots específicos (agentes do usuário)

Para impedir que bots identificados enviem dados para a plataforma Web Personalization, adicione a seguinte instrução `if` ao script de tag.

Este exemplo exclui os agentes do usuário &quot;Googlebot|msnbot&quot; das atividades do Web Personalization.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
if(navigator.userAgent.match(/.(Googlebot|msnbot)./gi) == null){
    (function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
    g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//[cdn-pod-X-url]/rtp-api/v1/rtp.js","[accountId]");

    rtp('send','view');
    rtp('get', 'campaign', true);
}
</script>
<!-- End of RTP tag -->
```

## Explicação das chamadas para o JavaScript

As tabelas a seguir descrevem a JavaScript adicionada a um site que usa Web Personalization e Conteúdo preditivo.

### JavaScript principal/dependente

| Nome | Descrição | Controle |
| --- | --- | --- |
| rtp.js | - | Controlado pela Marketo |
| jquery.min.js | v1.8.3 | Pode ser desativado entrando em contato com o Suporte ao cliente da Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Pode ser desativado entrando em contato com o Suporte ao cliente da Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Pode ser desativado entrando em contato com o Suporte ao cliente da Marketo |

*Usado somente se a caixa de diálogo jQuery UI estiver ausente.

### JavaScript sob demanda

| Nome | Descrição | Controle |
| --- | --- | --- |
| ga-integration-2.0.1.js | Usado se a integração Google Analytics/Facebook/SiteCatalyst estiver habilitada | Controlado pela Marketo |
| insightera-bar-2.1.js | Usado se a barra de recomendações de conteúdo preditivo estiver habilitada | Controlado pela Marketo |
| froogaloop2.min.js | Usado se o rastreamento de conteúdo estiver ativado e o player de Vimeo existir na página | - |
| iframe-api-v1.js | Usado se o rastreamento de conteúdo estiver ativado e o reprodutor do YouTube existir na página | - |
