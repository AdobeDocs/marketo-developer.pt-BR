---
title: Exemplos
description: Exemplos de JavaScript do Marketo Forms 2.0 para ocultar ou redirecionar em campos de envio, definição e leitura, validação com erros personalizados, lightbox e acionadores externos.
feature: Javascript
exl-id: dc5f0cc5-ff5a-48b0-be36-52c10e56f798
TQID: https://experienceleague.adobe.com/dH1yaglpL3odGZfGk-JC8oGljBF2gDpdjdg1BPE6OcQ
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 234
ht-degree: 0%

---

# Exemplos

Esses exemplos demonstram workflows comuns de formulário web do Forms 2.0.

## Ocultar formulário após envio bem-sucedido

Este exemplo mantém o visitante na página atual após um envio bem-sucedido. Ela não abre a página de acompanhamento nem recarrega a página atual.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Get the form's jQuery element and hide it
        form.getFormElem().hide();
        // Return false to prevent the submission handler from taking the lead to the follow up url
        return false;
    });
});
```

## Levar o visitante para o URL definido pelo usuário

Esse exemplo envia o visitante para um URL definido no JavaScript após um envio bem-sucedido. O URL do JavaScript substitui a página de agradecimento configurada.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    //Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Take the lead to a different page on successful submit, ignoring the form's configured followUpUrl
        location.href = "https://google.com/?q=marketo+forms+v2+examples";
        // Return false to prevent the submission handler continuing with its own processing
        return false;
    });
});
```

## Definir valores do campo de formulário

Esse exemplo define campos de formulário.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Set the value of the Phone and Country fields
    form.vals({ "Phone":"555-555-1234", "Country":"USA"});
});
```

## Ler valores de campo de formulário no envio do formulário

Este exemplo lê os valores dos campos de formulário quando o formulário é enviado.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSubmit handler
    form.onSubmit(function(){
        // Get the form field values
        var vals = form.vals();
        // You may wish to call other function calls here, for example to fire google analytics tracking or the like
        // callSomeFunction(vals);
        // We'll just alert them to show the principle
        alert("Submitted values: " + JSON.stringify(vals));
    });
});
```

## Envio de formulário em evento de clique que não seja de formulário

Este exemplo envia um formulário quando o visitante seleciona um elemento fora do formulário.

```javascript
// Load the form normally
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057);

// Find the button element that you want to attach the event to
var btn = document.getElementById("MyAlternativeSubmitButtonId");
btn.onclick = function() {
    // When the button is clicked, get the form object and submit it
    MktoForms2.whenReady(function (form) {
        form.submit();
    });
};
```

## Impedir que um usuário envie um formulário

Neste exemplo, o visitante deve selecionar o botão do contador de cliques pelo menos três vezes antes do botão de envio do formulário funcionar.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Check if the form is submittable
    if (form.submittable()) {
        // Set it to be non submittable
        form.submittable(false);
    }
});

var clickCount = 0;
// Wire up the click counter button
var clickCounterBtn = document.getElementById("ClickCounter");
clickCounterBtn.onclick = function() {
    clickCount++;
    // Update the buttons's text
    clickCounterBtn.innerHTML = "Click Counter Button ("+clickCount+")";

    if (clickCount >= 3) {
        // Get the form and set it to be submittable
        MktoForms2.whenReady(function (form) {
            form.submittable(true);
        });
    }
};
```

## Definir valores em campos ocultos no formulário

Este exemplo define valores em campos ocultos.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Set values for the hidden fields, "userIsAwesome" and "enrollDate"
    // Note that these fields were configured in the form editor as hidden fields already
    form.vals({"userIsAwesome":"true", "enrollDate":"2014-01-01"});
});
```

## Mostrar formulário no LightBox

Este exemplo exibe o formulário em uma caixa de diálogo estilo lightbox quando a URL contém o parâmetro `lightboxForm=true`.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    if (location.href.indexOf("lightboxForm=true") != -1) {
        MktoForms2.lightbox(form).show();
    }
});
```

## Mostrar mensagem de erro personalizada

Este exemplo aplica uma lógica de negócios personalizada durante o envio e exibe uma mensagem de erro personalizada quando os valores não atendem à condição necessária.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    //listen for the validate event
    form.onValidate(function() {
        // Get the values
        var vals = form.vals();
        //Check your condition
        if (vals.Country == "USA" && vals.vehicleSize != "Massive") {
            // Prevent form submission
            form.submittable(false);
            // Show error message, pointed at VehicleSize element
            var vehicleSizeElem = form.getFormElem().find("#vehicleSize");
            form.showErrorMessage("All Americans must have a massive vehicle", vehicleSizeElem);
        }
        else {
            // Enable submission for those who met the criteria
            form.submittable(true);
        }
  });
});
```
