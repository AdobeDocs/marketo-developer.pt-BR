---
title: Scripts de e-mails
feature: Email Programs
description: Saiba como criar scripts de emails dinâmicos do Marketo usando tokens, variáveis, ferramentas do Velocity e testar o com Enviar amostra e Visualização de email do Apache.
exl-id: ff396f8b-80c2-4c87-959e-fb8783c391bf
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 0%

---

# Scripts de e-mails

OBSERVAÇÃO: é altamente recomendável que você leia o [Guia do usuário do Velocity](https://velocity.apache.org/engine/devel/user-guide.html) para aprofundar-se no comportamento da Linguagem de modelo do Velocity.

[Apache Velocity](https://velocity.apache.org/) é uma linguagem criada em Java projetada para modelagem e script de conteúdo HTML. O Marketo permite que ele seja usado no contexto de emails usando tokens de script. Isso dá acesso aos dados armazenados em Oportunidades e Objetos personalizados e permite a criação de conteúdo dinâmico em emails. A Velocity oferece um fluxo de controle padrão de alto nível com if/else, for e for para permitir a manipulação condicional e iterativa do conteúdo. Este é um exemplo simples para imprimir uma saudação com a saudação correta:

```java
##check if the lead is male
#if(${lead.MarketoSocialGender} == "Male")
    ##if the lead is male, use the salutation 'Mr.'
    #set($greeting = "Dear Mr. ${lead.LastName},")
##check is the lead is female
#elseif(${lead.MarketoSocialGender} == "Female")
    ##if female, use the salutation 'Ms.'
    #set($greeting = "Dear Ms. ${lead.LastName},")
#else
    ##otherwise, use the first name
    #set($greeting = "Dear ${lead.FirstName},")
#end
##print the greeting and some content
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

Também há uma notação de referência silenciosa, em que há um `!` Incluído após o `$`. Normalmente, quando a velocidade encontra uma referência indefinida, a string que representa a referência é deixada no lugar. Com a notação de referência silenciosa, se uma referência indefinida for encontrada, nenhum valor será emitido:

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

Para obter mais informações sobre como fazer referência a variáveis, consulte o [Guia do Usuário do Apache](https://velocity.apache.org/engine/devel/user-guide.html#formal-reference-notation).

## Ferramentas do Velocity

O projeto Apache Velocity disponibiliza a funcionalidade por meio das [Ferramentas do Velocity](https://velocity.apache.org/tools/devel/apidocs/overview-summary.html). Eles são apenas invólucros para objetos Java e expõem seus métodos por meio de variáveis globais que são disponibilizadas para todos os scripts.

- [AlternatorTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/AlternatorTool.html)
- [FerramentaDeDataDeComparação](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ComparisonDateTool.html)
- [FerramentaConversão](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ConversionTool.html)
- [FerramentaDeData](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DateTool.html)
- [FerramentaExibição](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DisplayTool.html)
- [FerramentaMatemática](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/MathTool.html)
- [FerramentaNúmero](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/NumberTool.html)
- [FerramentaEscape](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/EscapeTool.html)
- [LoopTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/LoopTool.html)

Por exemplo, para usar um método de `ComparisonDateTool`, acesse se for da variável `$date` em um token de script:

```
#set($birthday = $convert.parseDate("2015-08-07","yyyy-MM-dd"))
##use whenIs to determine how many days away it is
$date.whenIs($birthday).days ##outputs 1
```

## Criação de um token de script

O script do Velocity é incluído nos emails usando tokens de script de email. Eles podem ser criados em Atividades de marketing em uma Pasta de marketing ou em um Programa. Para que um token seja usado dentro de um email, o email deve ser filho de um programa que possua o token ou o herde de uma pasta de marketing. Para criar um token, navegue até uma pasta ou programa e selecione a guia [!UICONTROL Meus tokens]. No menu direito, arraste a opção &quot;Script de email&quot; para a lista de tokens

![Token de Script](assets/script-token.png)

Aqui, você pode editar o nome do token e abrir o editor por meio da opção [!UICONTROL Clique para editar]:

![Editar Script](assets/script-edit.png)

Quando estiver no editor, você poderá criar um script com acesso a todas as variáveis em objetos acessíveis por script. Para obter uma referência de campo de um objeto, arraste-a da árvore direita para o script:

![Editar token de script](assets/edit-script-token.png)

## Incorporação e teste do script

Depois de definir o script em um Meu token de programa, você pode referenciá-lo em um determinado email usando o editor de email do Marketo.

![Script de email](assets/email-script-marketo-email.png)

Você pode testar seu script usando a ação de email [!UICONTROL Enviar Email de Exemplo] no designer de email do Marketo. Para que o script seja processado corretamente, você deve selecionar um cliente potencial existente para representar no campo [!UICONTROL Cliente Potencial]. Se você estiver testando com `$TriggerObject`, é possível selecionar o objeto de acionamento por meio do parâmetro [!UICONTROL Acionador]. Isso usa os dados do objeto atualizado mais recentemente desse tipo como a variável `$TriggerObject`.

![Script de Email de Teste](assets/velocity-test.png)

Você também pode usar a [!UICONTROL Visualização de email] para testar seu script. Para fazer isso, selecione **[!UICONTROL Exibir como: Detalhe de Cliente Potencial]** e selecione um cliente potencial em uma lista estática disponível. Isso tem a vantagem adicional de gerar quaisquer exceções que possam ter ocorrido durante a execução do script:

![Exibir Email Como](assets/view-as.png)

## Dicas úteis

O comprimento combinado de todos os tokens de script de email em um determinado email não pode exceder 100.000 bytes. Esse limite pertence ao comprimento total das próprias cadeias de caracteres do token (não ao comprimento total após a expansão dos tokens).

- As variáveis referenciadas no script de email devem existir no Marketo em um dos objetos disponíveis para o script.
- Você pode fazer referência a objetos personalizados de primeiro e segundo nível que se originam de seu CRM integrado nativamente e que estão diretamente conectados ao cliente potencial ou contato, mas não a objetos personalizados de terceiro nível. Objetos personalizados não podem ser pais do cliente potencial ou da empresa
- Para objetos personalizados do Marketo, você pode fazer referência a objetos personalizados de segundo nível com relacionamento Pai-Filho. Por exemplo `Lead <- Parent <- Child`. Não é possível fazer referência a objetos personalizados de segundo nível com uma relação Edge-Bridge. por exemplo, `Lead <- Bridge -> Edge`
- Você pode fazer referência a objetos personalizados conectados a um cliente potencial, contato ou conta, mas não a mais de um.
- Objetos personalizados só podem ser referenciados por meio de uma única conexão, cliente potencial, contato ou conta
- Você deve marcar a caixa no editor de scripts para os campos que você está usando, caso contrário eles não serão processados
- Para cada objeto personalizado, os dez registros atualizados mais recentes por pessoa/contato estão disponíveis no tempo de execução e são ordenados da atualização mais recente (em 0) para a atualização mais antiga (em 9). Você pode aumentar o número de registros disponíveis por [seguindo as instruções](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).
- Se você incluir mais de um script de email em um email, eles serão executados de cima para baixo. O escopo das variáveis definidas no primeiro script a ser executado estará disponível nos scripts subsequentes.
- Referência de ferramentas: [https://velocity.apache.org/tools/2.0/index.html](https://velocity.apache.org/tools/2.0/index.html)
- Uma observação sobre tokens que contêm caracteres de nova linha &quot;\\n&quot; ou &quot;\\r\\n&quot;. Quando um email é enviado por meio do Send Sample ou por uma Campanha em lote, os caracteres de nova linha em tokens são substituídos por espaços. Quando o email é enviado por meio do Trigger Campaign, os caracteres de nova linha são deixados intocados.
- Para garantir a análise adequada dos URLs, todo o caminho deve ser definido como uma variável e, em seguida, impresso, e a variável não deve ser impressa dentro de referências de URL. O protocolo (http:// ou https://) deve ser incluído e deve ser separado do restante do URL. A URL também deve fazer parte de uma marca de âncora (<a>) totalmente formada. O script deve produzir uma tag de âncora totalmente formada para que os links sejam rastreados. Os links não serão rastreados se forem gerados de dentro de um loop for ou foreach.

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
