---
title: "Scripts de email"
feature: Email Programs
description: "Visão geral de scripts de email"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '963'
ht-degree: 0%

---


# Scripts de e-mails

OBSERVAÇÃO: é altamente recomendável que você leia a [Guia do usuário do Velocity](https://velocity.apache.org/engine/devel/user-guide.html) para aprofundar o comportamento da Linguagem de modelo do Velocity.

[Apache Velocity](https://velocity.apache.org/) é uma linguagem criada em Java projetada para modelos e script de conteúdo de HTML. O Marketo permite que ele seja usado no contexto de emails usando tokens de script. Isso dá acesso aos dados armazenados em Oportunidades e Objetos personalizados e permite a criação de conteúdo dinâmico em emails. A Velocity oferece um fluxo de controle padrão de alto nível com if/else, for e for para permitir a manipulação condicional e iterativa do conteúdo. Este é um exemplo simples para imprimir uma saudação com a saudação correta:

```java
//check if the lead is male
if(${lead.MarketoSocialGender} == "Male")
    if the lead is male, use the salutation 'Mr.'
    set($greeting = "Dear Mr. ${lead.LastName},")
//check is the lead is female
elseif(${lead.MarketoSocialGender} == "Female")
    if female, use the salutation 'Ms.'
    set($greeting = "Dear Ms. ${lead.LastName},")
else
    //otherwise, use the first name
    set($greeting = "Dear ${lead.FirstName},")
end
print the greeting and some content
${greeting}

    Lorem ipsum dolor sit amet...
```

## Variáveis

As variáveis sempre recebem o prefixo &#39;$&#39; e são definidas e atualizadas usando #set:

```
#set($variable = "value")
```

Seus valores podem ser recuperados por meio de vários tipos de referência diferentes com comportamentos diferentes:

```
$variable ##outputs 'value'
$variablename ##outputs '$variablename'
${variable}name ##outputs 'valuename'
```

Há também a notação de referência silenciosa, em que há uma `!` Incluído após o `$`. Normalmente, quando a velocidade encontra uma referência indefinida, a string que representa a referência é deixada no lugar. Com a notação de referência silenciosa, se uma referência indefinida for encontrada, nenhum valor será emitido:

```
##Defined Reference

#set($foo = "bar")
$foo ##outputs "bar"

##Undefined Reference

##normal
$baz ##outputs "$baz"

##quiet
$!baz ##outputs nothing
```

Para obter mais informações sobre como fazer referência a variáveis, consulte a [Guia do usuário do Apache](https://velocity.apache.org/engine/devel/user-guide.html#formal-reference-notation).

## Ferramentas do Velocity

O projeto Apache Velocity disponibiliza a funcionalidade por meio do uso de [Ferramentas do Velocity](https://velocity.apache.org/tools/devel/apidocs/overview-summary.html). Eles são apenas invólucros para objetos Java e expõem seus métodos por meio de variáveis globais que são disponibilizadas para todos os scripts.

- [AlternatorTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/AlternatorTool.html)
- [FerramentaDataComparação](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ComparisonDateTool.html)
- [FerramentaConversão](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ConversionTool.html)
- [FerramentaData](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DateTool.html)
- [FerramentaExibição](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DisplayTool.html)
- [FerramentaMatemática](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/MathTool.html)
- [FerramentaNúmero](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/NumberTool.html)
- [EscapeTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/EscapeTool.html)
- [LoopTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/LoopTool.html)

Por exemplo, para usar um método de `ComparisonDateTool`, acesse se for do `$date` em um token de script:

```
#set($birthday = $convert.parseDate("2015-08-07","yyyy-MM-dd"))
##use whenIs to determine how many days away it is
$date.whenIs($birthday).days ##outputs 1
```

## Criação de um token de script

O script do Velocity é incluído nos emails usando tokens de script de email. Eles podem ser criados em Atividades de marketing em uma Pasta de marketing ou em um Programa. Para que um token seja usado dentro de um email, o email deve ser filho de um programa que possua o token ou o herde de uma pasta de marketing. Para criar um token, navegue até uma pasta ou programa e selecione a [!UICONTROL Meus tokens] guia. No menu direito, arraste a opção &quot;Script de email&quot; para a lista de tokens

![Token de script](assets/script-token.png)

Aqui, você pode editar o nome do token e abrir o editor por meio da opção Click to Edit:

![Editar script](assets/script-edit.png)

Quando estiver no editor, você poderá criar um script com acesso a todas as variáveis em objetos acessíveis por script. Para obter uma referência de campo de um objeto, arraste-a da árvore direita para o script:

![Editar token de script](assets/edit-script-token.png)

## Incorporação e teste do script

Depois de definir o script em um Meu token de programa, você pode referenciá-lo em um determinado email usando o editor de email do Marketo.

![Script de email](assets/email-script-marketo-email.png)

Você pode testar seu script usando a ação de email &quot;Enviar email de amostra&quot; no designer de email do Marketo. Para que o script seja processado corretamente, você deve selecionar um cliente potencial existente para representar no campo Cliente Potencial. Se você estiver testando com `$TriggerObject`, você pode selecionar o objeto de acionamento por meio do parâmetro &quot;Acionador&quot;. Usa os dados do objeto atualizado mais recentemente desse tipo como o `$TriggerObject` variável.

![Script de email de teste](assets/velocity-test.png)

Você também pode usar a Visualização de email para testar o script. Para fazer isso, você deve selecionar Exibir como: Detalhe do lead e selecionar um lead em uma lista estática disponível. Isso tem a vantagem adicional de gerar quaisquer exceções que possam ter ocorrido durante a execução do script:

![Exibir Email como](assets/view-as.png)

## Dicas úteis

O comprimento combinado de todos os tokens de script de email em um determinado email não pode exceder 100.000 bytes. Esse limite pertence ao comprimento total das próprias cadeias de caracteres do token (não ao comprimento total após a expansão dos tokens).

- As variáveis referenciadas no script de email devem existir no Marketo em um dos objetos disponíveis para o script.
- Você pode fazer referência a objetos personalizados de primeiro e segundo nível que se originam de seu CRM integrado nativamente e que estão diretamente conectados ao cliente potencial ou contato, mas não a objetos personalizados de terceiro nível. Objetos personalizados não podem ser pais do cliente potencial ou da empresa
- Para objetos personalizados do Marketo, você pode fazer referência a objetos personalizados de segundo nível com relacionamento Pai-Filho. Por exemplo `Lead <- Parent <- Child`. Você não pode fazer referência a objetos personalizados de segundo nível com relação Edge-Bridge. por exemplo,  `Lead <- Bridge -> Edge`
- Você pode fazer referência a objetos personalizados conectados a um cliente potencial, contato ou conta, mas não a mais de um.
- Objetos personalizados só podem ser referenciados por meio de uma única conexão, cliente potencial, contato ou conta
- Você deve marcar a caixa no editor de scripts para os campos que você está usando, caso contrário eles não serão processados
- Para cada objeto personalizado, os dez registros atualizados mais recentes por pessoa/contato estão disponíveis no tempo de execução e são ordenados da atualização mais recente (em 0) para a atualização mais antiga (em 9). É possível aumentar o número de registros disponíveis até [seguindo as instruções](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).
- Se você incluir mais de um script de email em um email, eles serão executados de cima para baixo. O escopo das variáveis definidas no primeiro script a ser executado estará disponível nos scripts subsequentes.
- Referência de ferramentas: [https://velocity.apache.org/tools/2.0/index.html](https://velocity.apache.org/tools/2.0/index.html)
- Uma observação sobre tokens que contêm caracteres de nova linha &quot;\\n&quot; ou &quot;\\r\\n&quot;. Quando um email é enviado por meio do Send Sample ou por uma Campanha em lote, os caracteres de nova linha em tokens são substituídos por espaços. Quando o email é enviado por meio do Trigger Campaign, os caracteres de nova linha são deixados intocados.
- Para garantir a análise adequada dos URLs, todo o caminho deve ser definido como uma variável e, em seguida, impresso, e a variável não deve ser impressa dentro de referências de URL. O protocolo (http:// ou https://) deve ser incluído e deve ser separado do restante do URL. O URL também deve fazer parte de uma âncora totalmente formada (<a>). O script deve produzir uma tag de âncora totalmente formada para que os links sejam rastreados. Os links não serão rastreados se forem gerados de dentro de um loop for ou foreach.

```html
<!-- Correct -->
#set($url = "www.example.com/${object.id}")
<a href="http://${url}">Link Text</a>

<!-- Correct -->
<a href="http://www.example.com/${object.id}">Link Text</a>

<!-- Incorrect -->
<a href="${url}">Link Text</a>

<!-- Incorrect -->
<a href="{{my.link}}">Link Text</a>

<!-- Incorrect -->
<a href="http://{{my.link}}">Link Text</a>
```