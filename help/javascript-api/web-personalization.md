---
title: "Personalização da Web"
description: "Personalização da Web"
feature: Web Personalization, Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 5%

---


# Personalização na Web

A API JavaScript de personalização da Web estende a capacidade de personalização automatizada da plataforma. Ele permite o rastreamento de eventos e a personalização dinâmica de uma página da Web. Recursos adicionais: [Eventos de dados personalizados](custom-data-events.md), [Conteúdo dinâmico](web-personalization.md), [Obter dados do visitante](get-visitor-data.md), [Excluir tag para bots específicos](#exclude_tag_for_specific_bots).

- Você deve se tornar um cliente de Personalização da Web e ter o [Tag RTP implantada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de contexto de usuário.
- O RTP não suporta listas de contas nomeadas de Marketing Baseado em Conta. As listas e os códigos ABM pertencem apenas às listas de contas carregadas (arquivos CSV) gerenciadas no RTP.

## Configuração de tag

A tag RTP deve ser inserida no cabeçalho da página personalizada.

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

Esse método é chamado automaticamente no nível da tag para definir a ID de conta relevante. Você pode definir a ID da conta quando quiser dividir entre domínios diferentes.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Obrigatório | Sequência de caracteres | Nome do método. |
| accountId | Obrigatório | Sequência de caracteres | ID da conta. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funções de envio de eventos

Esse método envia um evento de exibição, que é usado para rastreamento de página. No exemplo abaixo, o URL da página atual é rastreado como uma exibição de página do visitante.

Ao passar o parâmetro opcional &quot;page&quot; neste método, a página atual pode ser substituída.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|-----------|-------------------|--------|---------------------------------|
| &#39;enviar&#39; | Obrigatório | Sequência de caracteres | Ação do método. |
| &#39;exibir&#39; | Obrigatório | Sequência de caracteres | Nome do método. |
| página | Opcional | Sequência de caracteres | Caminho relativo ou URL da página inteira. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Excluir tag para bots específicos (agentes do usuário)

Para excluir navegadores específicos do envio de dados para a plataforma de Personalização da Web (no caso de bots identificados), adicione a seguinte instrução IF ao script de tag.

No exemplo de código abaixo, &quot;Googlebot|msnbot&quot; é usado como exemplos de bot a serem excluídos das atividades de Personalização na Web.

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

## Explicação das chamadas de JavaScript

Descrição do JavaScript que é adicionado a um site ao usar Personalização da Web e Conteúdo preditivo.

### JavaScript principal/dependente

| Nome | Descrição | Controle |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Controlado pela Marketo |
| jquery.min.js | v1.8.3 | Pode ser desativado entrando em contato com o Suporte ao cliente da Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Pode ser desativado entrando em contato com o Suporte ao cliente da Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Pode ser desativado entrando em contato com o Suporte ao cliente da Marketo |


*Usado somente se a interface do usuário do jQuery estiver ausente

### JavaScript sob demanda

| Nome | Descrição | Controle |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Usado se a integração Google Analytics/Facebook/SiteCatalyst estiver ativada | Controlado pela Marketo |
| insightera-bar-2.1.js | Usado se a barra de recomendações de conteúdo preditivo estiver habilitada | Controlado pela Marketo |
| froogaloop2.min.js | Usado se o rastreamento de conteúdo estiver ativado e o player de Vimeo existir na página | - |
| iframe-api-v1.js | Usado se o rastreamento de conteúdo estiver ativado e o reprodutor do YouTube existir na página | - |

