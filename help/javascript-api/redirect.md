---
title: Redirecionar
description: Implemente a API de redirecionamento RTP para enviar visitantes segmentados para URLs direcionados usando campos como ABM, organização, local e segmentos, com exemplos e dicas.
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
TQID: https://experienceleague.adobe.com/frvGjN7DBJ1RJ3QFvWxo1qGiTNFmvyxi3H6FeynJHLU
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 8%

---

# Redirecionar

Use a API de redirecionamento RTP para enviar públicos segmentados para um URL de destino.

- Você deve ser um cliente do Web Personalization e implantar a [tag RTP](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) no site antes de usar a API de Contexto de Usuário.
- O RTP não suporta listas de contas nomeadas de Marketing Baseado em Conta. As listas e os códigos ABM pertencem apenas às listas de contas carregadas (arquivos CSV) gerenciadas no RTP.

## Uso

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| &#39;enviar&#39; | Obrigatório | String | Ação do método. |
| &#39;redirecionar&#39; | Obrigatório | String | Nome do método. |
| field_name | Obrigatório | String | Nome do campo para corresponder. Exemplo: &#39;abm.name&#39; (veja abaixo). |
| values_array | Obrigatório | Matriz | Lista de valores para corresponder ao campo (não diferencia maiúsculas de minúsculas). |
| redirect_url | Obrigatório | String | URL do Target para redirecionar visitantes que corresponderam à condição. |
| redirect_matched_visitors | Opcional | Booleano | Se true, os visitantes correspondentes à condição serão redirecionados. Se for falso, os visitantes sem correspondência da condição serão redirecionados. Padrão: verdadeiro. |

As condições de redirecionamento podem usar organizações, setores, listas de ABM, locais, ISPs ou segmentos correspondentes.

| Condição | Hierarquia de dados | Exemplo |
| --- | --- | --- |
| Segmentos correspondentes (funciona somente após o primeiro clique) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchedSegments.name&#39; , [&#39;Fortune 1.000&#39; , &#39;Enterprise&#39;] , &#39;<https://www.example.com>&#39;); |
| Segmentos correspondentes (funciona somente após o primeiro clique) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchedSegments.id&#39; , [106 , 107 , 190] , &#39;<https://www.example.com>&#39;); |
| Listas ABM | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;ative_customers&#39;] , &#39;<https://www.example.com>&#39;); |
| Listas ABM | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13 , 15] , &#39;<https://www.example.com>&#39;); |
| Organizações | org | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &#39;<https://www.example.com>&#39;); |
| Localização | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&#39;Estados Unidos&#39;], &#39;<https://www.example.com>&#39;); |
| Localização | location.state | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.state&#39;, [&#39;ca&#39;], &#39;<https://www.example.com>&#39;); |
| Localização | location.city | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;<https://www.example.com>&#39;); |
| Setores | indústrias | rtp( &#39;enviar&#39;, &#39;redirecionar&#39; , &#39;indústrias&#39; , [&#39;Educação&#39;], &#39;<https://www.example.com>&#39;); |
| ISP | isp | rtp( &#39;send&#39;, &#39;redirect&#39; , isp , [&#39;False&#39;], &#39;<https://www.example.com>&#39;); |

## Observações

- Para reduzir a latência de um redirecionamento com base em Firmographics, como empresa, setor ou local, insira o código de redirecionamento antes de rtp(&#39;send&#39;, &#39;view&#39;) e rtp(&#39;get&#39;,&#39;campaign&#39;).
- Coloque o código de redirecionamento imediatamente após a tag rtp no cabeçalho da página.
- Otimize o carregamento do site para melhorar a velocidade do redirecionamento do JavaScript no lado do navegador.
- Evite autoredirecionamentos. o rtp inclui uma proteção que bloqueia chamadas de redirecionamento cíclicas.

```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//xyz.marketo.com/rtp-api/v1/rtp.js","xyz");

// START REDIRECT EXAMPLE
//   - Using a helper redirect function
//   - Redirect based on named account
rtp('send','redirect','org', ['microsoft'],'http://www.marketo.com');

// Redirect based on named account list (ABM)
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
// END REDIRECT EXAMPLE
rtp('send','view');
rtp('get','campaign');
</script>
<!-- End of RTP tag -->
```

## Como redirecionar visitantes rastreados

1. Anexe o parâmetro ao URL de destino, por exemplo, &lt;www.marketo.com?rtp=redirect>.
1. Crie um segmento com o nome &quot;Redirecionado pelo RTP&quot;.
1. Use o parâmetro &quot;Páginas específicas&quot; para direcionar os visitantes que visualizam uma página que contém o parâmetro.

![visitantes-redirecionados-de-rastreamento](assets/tracking-redirected-vistors.png)

## Como definir mais de uma condição com URLs de destino diferentes

A chamada de redirecionamento oferece suporte a várias chamadas. Use várias chamadas para combinar campos e criar condições com URLs e valores diferentes.

### Uso

`rtp('send', 'redirect', field_name, url_values_map);`

| Parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| &#39;enviar&#39; | Obrigatório | String | Ação do método. |
| &#39;redirecionar&#39; | Obrigatório | String | Nome do método. |
| field_name | Obrigatório | String | Nome do campo para corresponder. Exemplo: &#39;abm.name&#39; (veja acima). |
| url_values_map | Obrigatório | Objeto | Mapear entre o URL de redirecionamento e a lista de valores. Exemplo:{&#39;<https://www.example.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |

#### Exemplo

```javascript
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
rtp('send','redirect','org', {
    // Redirect visitors from 'Microsoft' to www.marketo.com/enterprise
    'http://www.marketo.com/enterprise' : ['microsoft']
});
```
