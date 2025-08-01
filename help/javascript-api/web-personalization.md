---
title: Personalização na Web
description: Personalização na Web
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 6%

---

# Personalização na Web

A API do JavaScript do Web Personalization estende a capacidade de personalização automatizada da plataforma. Ele permite o rastreamento de eventos e a personalização dinâmica de uma página da Web. Recursos adicionais: [Eventos de Dados Personalizados](custom-data-events.md), [Conteúdo Dinâmico](web-personalization.md), [Obter Dados do Visitante](get-visitor-data.md), [Excluir Marca para Bots Específicos](#exclude_tag_for_specific_bots).

- Você deve se tornar um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.
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
| &#39;setAccount&#39; | Obrigatório | String | Nome do método. |
| accountId | Obrigatório | String | ID da conta. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funções de envio de eventos

Esse método envia um evento de exibição, que é usado para rastreamento de página. No exemplo abaixo, o URL da página atual é rastreado como uma exibição de página do visitante.

Ao passar o parâmetro opcional &quot;page&quot; neste método, a página atual pode ser substituída.

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
|-----------|-------------------|--------|---------------------------------|
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

Para excluir navegadores específicos do envio de dados para a plataforma Web Personalization (no caso de bots identificados), adicione a seguinte instrução IF ao script de tag.

No exemplo de código abaixo, &quot;Googlebot|msnbot&quot; é usado como exemplos de bot a serem excluídos das atividades do Web Personalization.

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

Descrição do JavaScript que é adicionado a um site ao usar o Web Personalization e o Conteúdo preditivo.

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
| ga-integration-2.0.1.js | Usado se a integração Google Analytics/Facebook/SiteCatalyst estiver habilitada | Controlado pela Marketo |
| insightera-bar-2.1.js | Usado se a barra de recomendações de conteúdo preditivo estiver habilitada | Controlado pela Marketo |
| froogaloop2.min.js | Usado se o rastreamento de conteúdo estiver ativado e o player de Vimeo existir na página | - |
| iframe-api-v1.js | Usado se o rastreamento de conteúdo estiver ativado e o reprodutor do YouTube existir na página | - |
