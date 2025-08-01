---
title: Redes sociais
description: Redes sociais
feature: Social, Javascript
exl-id: 82d2b86f-5efe-4434-b617-d27f76515a79
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---

# Redes sociais

O [Marketo Social Marketing](https://business.adobe.com/products/marketo/social-marketing.html) permite que profissionais de marketing incorporem widgets sociais a sites e landing pages. Os widgets sociais incluem pesquisas, botões de compartilhamento social, vídeos, sorteios e promoções como ofertas de referência.

## Widget de compartilhamento incorporado de exemplo

```html
<!-- Marketo Widget Loader Script -->

<script type="text/javascript" src="//b2c-mlm.marketo.com/jsloader/271d8232-1500-4305-b7ed-05d451b9ee0c/loader.php.js">
</script>

 <!-- The Location of the Social Widget -->

<divclass='cf_widgetloader cf_w_245d8f3c0955454cbd26abc39d0d874c'="" options="{&quot;outerHeight&quot;:400, &quot;outerWidth&quot;:600}">
</divclass='cf_widgetloader'>
```

![widget de compartilhamento social](assets/social-share-widget.png)

Há dois métodos básicos para a personalização de um widget social:

1. Usar a interface normal do produto e anexar ouvintes de eventos para serem informados quando determinadas ações ocorrerem na interface para executar uma lógica de negócios adicional.
1. Substituição da interface normal do produto por uma interface personalizada e ativação dos &quot;estágios&quot; pop-up da interface quando desejado.

## Anexar eventos à interface do usuário normal

Há duas maneiras de se inscrever em eventos na biblioteca JavaScript do CF, globalmente ou para um único widget. Os eventos estão documentados abaixo na tabela de eventos.

### Assinatura de Evento Global

```html
<script>
cf_scripts.afterload(function(){
    CF.events.listen("event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever ANY widget fires the event "event_name_here".
        }
    );
});
</script>
```

### Assinatura de Evento por Widget

```html
<script>
cf_scripts.afterload(function(){
    CF.widget.listen("widget_name_here", "event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever the widget named "widget_name_here" fires the event "event_name_here".
        }
    );
});
</script>
```

## Um exemplo

Este exemplo mostra um elemento oculto anteriormente com a ID &quot;signedUp&quot; depois que um usuário conclui uma inscrição de oferta para um widget chamado &quot;referral_SignUp&quot;.

```html
<div id='signedUp'style='display:none; color:green;'>This is a custom message to let you know that you signed up!</div>
<div class='cf_widgetLoader cf_w_referral_SignUp'></div>

<script>
    cf_scripts.afterload(function(){
        CF.widget.listen("referral_SignUp", "offer_enrolled", function(){
        cf_jq("#signedUp").show();
    });
});
</script>
```

## Tabela de eventos básicos

| Nome do evento | Descrição | Widgets que usam este evento | Argumentos suportados (passados para a função de retorno de chamada de evento) |
| --- | --- | --- | --- |
| share_sent | Acionado sempre que uma solicitação de compartilhamento é enviada ao servidor para processamento | Todos os widgets que podem compartilhar | 1.&quot;share_sent&quot; (Cadeia de caracteres)<br>2. Parâmetros enviados (Objeto) |
| share_success | Acionado quando a solicitação de compartilhamento é processada com êxito. | Todos os widgets que podem ser compartilhados. | 1.&quot;share_success&quot; (cadeia de caracteres)<br>2. Compartilhar objeto de resposta, contendo mensagem enviada e url encurtado (Objeto) |
| vote_success | Acionado quando um usuário votar em uma pesquisa com êxito. | Widgets de Pesquisa, VS, Votação | &#x200B;1. &quot;vote_success&quot; (cadeia de caracteres)<br>2. Item votado, incluindo título, descrição, identificador de entidade (Objeto) |
| offer_enrolled | Acionado quando um usuário é inscrito com êxito em uma oferta | Todos os widgets de oferta | 1.&quot;offer_enrolled&quot; (Cadeia de caracteres)<br>2. Propriedades do usuário (Objeto) alteradas,<br>3. Atributos de usuário alterados (Objeto) |
| profile_saved | Acionado quando um usuário atualiza o perfil da captura de perfil | Todos os widgets que não são de oferta com captura de perfil ativada | 1.&quot;profile_saved&quot; (Cadeia de caracteres)<br>2. Propriedades do usuário (Objeto) alteradas<br>3. Atributos de usuário alterados (Objeto) |
| video_loaded | Acionado quando um vídeo incorporado é totalmente carregado e inicializado. | Widget de Compartilhamento de vídeo | &#x200B;1. &quot;video_loaded&quot; (String) 2. Elemento &quot;.cf_videoshare_wrap&quot; que contém o vídeo (Objeto jQuery) |

## Substituição da interface por uma interface personalizada

Para substituir a interface por uma interface personalizada, primeiro desative a interface normal. Isso é feito definindo a opção _popupUIOnly_ como _true_. Com essa opção definida, a interface padrão não será renderizada no carregamento da página. Em vez disso, o widget busca seus dados e espera que você inicie um de seus estágios pop-up, chamando a função _CF.widget.ativate_ e fornecendo opções para o que deve fazer.

Este é um exemplo de criação de um botão personalizado que inicia o fluxo de inscrição de oferta de referência para um widget de oferta de referência chamado _referral_SignUp_.

```html
<button id="myNewSignUpButton">My newSign Up button</button>

<!-- Turn off the defaultreferral offer UI by setting popupUIOnly to true-->
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // After the cf script library has loaded, find the button with
    // id="myNewSignUpButton", and attach a click listener to it.
    $("#myNewSignUpButton").click(function(){
        // When it is clicked, activate the popup widget flow for the referral,
        // asking it to point to the clicked button.
        CF.widget.activate("referral_SignUp", {pointTo:$(this)});
    });
});
</script>
```

Como a adição de manipuladores de clique é comum, há um método de atalho para adicioná-los. O exemplo a seguir é funcionalmente equivalente ao exemplo anterior.

```html
<button id="myNewSignUpButton">My newSign Up button</button>
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // Use the addClickActivate convenience method, which will
    // automatically make the popup point at the clicked item with id myNewSignUpButton.
    CF.widget.addClickActivate("#myNewSignUpButton", "referral_SignUp", {});
});
</script>
```

## Obtendo dados da interface do widget para colocar na sua interface de substituição

Se você precisar de dados sobre o widget para desenhar a interface de usuário de substituição, obtenha os dados do evento especial _ui_data_. Você pode acompanhar esse evento com a função normal `CF.widget.listen`, mas isso pode causar uma possível condição de corrida em que seu ouvinte de eventos é adicionado depois que o widget já acionou o evento _ui_data_, portanto, você nunca recebe dados. Para evitar essa corrida, use o evento `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data` que é acionado sempre que uma ação é executada e faria com que a interface padrão do widget fosse redesenhada, mesmo que você tenha desabilitado essa interface com a opção `popupUIOnly`.

Um exemplo que usa a função `uiData` para exibir o número de entradas que um usuário tem para um sorteio com o nome de widget _sweeps_Sweepstakes_.

```html
<span>You have <span id="entryCount">?</span> entries.</span>

<div class="cf_widgetLoader cf_w_sweeps_Sweepstakes"></div>

<button id='myNewSweepsButton'>New Sweeps Up Button!</button>

<script>
cf_scripts.afterload(function($, CF){
    CF.widget.uiData("sweeps_Sweepstakes", function(uiData){
        if(uiData.user && uiData.userStatus && uiData.userEntries){
            $("entryCount").html(""+ uiData.userEntries);
        }
        else{
            $("entryCount").html("0");
        }
    });
});
</script>
```

## Referência de dados da interface de usuário de assinatura da oferta de referência

| Tipo | Descrição |
|---------------|----------------------------------------------------|
| data | Valor da data do formulário &quot;dd-MM-yyyy&quot; |
| número | Um número inteiro ou de ponto flutuante |
| Texto formatado | Uma string do HTML |
| Pontuação | Um inteiro de 32 bits assinado |
| campanha do sfdc | Usado na integração do gerenciamento de campanhas do Salesforce |
| texto | Uma string de texto |

## Referência de dados da interface de usuário do TrackProgress da oferta de referência

| Tipo | Descrição |
|---------------|----------------------------------------------------|
| data | Valor da data do formulário &quot;dd-MM-yyyy&quot; |
| número | Um número inteiro ou de ponto flutuante |
| Texto formatado | Uma string do HTML |
| Pontuação | Um inteiro de 32 bits assinado |
| campanha do sfdc | Usado na integração do gerenciamento de campanhas do Salesforce |
| texto | Uma string de texto |

## Referência de dados da interface do Sweepstakes (para Sweepstakes de Campanha Social, não Sweepstakes de LM)

| Tipo | Descrição |
|---------------|----------------------------------------------------|
| data | Valor da data do formulário &quot;dd-MM-yyyy&quot; |
| número | Um número inteiro ou de ponto flutuante |
| Texto formatado | Uma string do HTML |
| Pontuação | Um inteiro de 32 bits assinado |
| campanha do sfdc | Usado na integração do gerenciamento de campanhas do Salesforce |
| texto | Uma string de texto |

## Referência de dados de logon social (widget de preenchimento de formulário)

| Tipo | Descrição |
|---------------|----------------------------------------------------|
| data | Valor da data do formulário &quot;dd-MM-yyyy&quot; |
| número | Um número inteiro ou de ponto flutuante |
| Texto formatado | Uma string do HTML |
| Pontuação | Um inteiro de 32 bits assinado |
| campanha do sfdc | Usado na integração do gerenciamento de campanhas do Salesforce |
| texto | Uma string de texto |

```javascript
{
"alt_id": "http://www.facebook.com/profile.php?id=1526228678",
"provider_name": "facebook",
"default_photo_url": "https://graph.facebook.com/1526228678/picture?type=large",
"email": "ian.b.taylor@gmail.com",
"verified_email": "ian.b.taylor@gmail.com",
"gender": "male",
"preferred_user_name": "IanTaylor",
"display_name": "Ian Taylor",
"birth_date": 343954800000,
"first_name": "Ian",
"last_name": "Taylor",
"city": null,
"state": null,
"region": null,
"postal_code": null,
"country": null,
"time_zone": null,
"connection_count": 0,
"credentials": {
"uid": "1526228678",
"scopes": "publish_actions",
"expires": "1371994082",
"accessToken": "BAAGFJ0KUFpcBABuNMptmYY...",
"type": "Facebook"
},
"about_me": null,
"cur_pos_title": "Senior Staff Engineer",
"phone_number": null,
"company": "Marketo",
"cur_pos_start_date": 1333231200000,
"cur_pos_summary": null
}
```
