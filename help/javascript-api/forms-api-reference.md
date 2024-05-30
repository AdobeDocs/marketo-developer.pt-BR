---
title: "Referência da API do Forms"
description: "Referência da API do Forms"
feature: Forms, Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1327'
ht-degree: 2%

---


# Referência da API do Forms

Há dois objetos principais com os quais você interagirá usando a API do Forms 2.0. A variável `MktoForms2` e o `Form` objeto. A variável `MktoForms2` object é o namespace publicamente visível de nível superior para a funcionalidade Forms2 e contém funções para criar, carregar e buscar objetos Form.

## Métodos MktoForms2

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Método</strong></td>
      <td><strong>Descrição</strong></td>
      <td><strong>Parâmetros</strong></td>
      <td><strong>Devoluções</strong></td>
    </tr>
    <tr valign="top">
      <td>.loadForm(baseUrl, munchkinId, formId, retorno de chamada)</td>
      <td>Carrega um descritor de formulário dos servidores da Marketo e cria um novo objeto de formulário.</td>
      <td> baseUrl(String) - URL para a instância do servidor Marketo da sua assinatura</td>
      <td>não definido(a)s</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (String) - ID do Munchkin da assinatura</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (String ou Número) - A ID de versão do formulário (Vid) a ser carregada</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (opcional) (Função) - Uma função callback para passar o objeto Form construído para depois de ter sido carregado e inicializado.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(formulário, opções)</td>
      <td>Processa uma caixa de diálogo modal de estilo lightbox com o objeto Formulário nela.</td>
      <td>formulário (Objeto de formulário) - Uma instância de um objeto de formulário que você deseja renderizar em uma lightbox.</td>
      <td>Um objeto lightbox com os métodos .show() e .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (opcional)(Objeto) - Um objeto de opções passado para o objeto lightbox</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Function) - Um retorno de chamada que é acionado quando o formulário é enviado.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolean) default true - Controla se um botão fechar (X) é exibido na caixa de diálogo lightbox.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, retorno de chamada)</td>
      <td>Cria um novo objeto de formulário a partir de um objeto JS do descritor de formulário. Adiciona uma função de retorno de chamada que é chamada depois que todas as folhas de estilos e informações de cliente potencial conhecidas são buscadas e o objeto de formulário é criado.</td>
      <td>formData (Objeto Descritor de Formulário) - Um objeto descritor de formulário, conforme criado pelo Editor Forms V2 do Marketo</td>
      <td>não definido(a)s</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (opcional)(Função) - Esse callback é chamado com um único argumento, uma instância recém-criada do objeto Form.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Obtém um objeto de formulário criado anteriormente pelo identificador de formulário</td>
      <td> formId (Número ou String) - Identificador de Vid do Formulário.</td>
      <td>Objeto de formulário</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Busca uma matriz de todos os objetos de formulário que foram construídos anteriormente na página.</td>
      <td>n/d</td>
      <td>Matriz de objeto de formulário</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Obtém um objeto JS contendo dados do URL e do referenciador que podem ser interessantes para fins de rastreamento.</td>
      <td>n/d</td>
      <td>Objeto</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(retorno de chamada)</td>
      <td>Adiciona um retorno de chamada que é chamado exatamente uma vez para cada formulário na página que fica "pronto". Prontidão significa que o formulário existe, foi renderizado inicialmente e teve seus retornos de chamada iniciais chamados. Se já houver um formulário que esteja pronto no momento em que essa função for chamada, o retorno de chamada transmitido será chamado imediatamente.</td>
      <td>callback(Função) - O callback recebe um único argumento, um objeto de formulário.</td>
      <td>Objeto MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Adiciona um retorno de chamada que é chamado toda vez que qualquer formulário na página é renderizado. Os Forms são renderizados quando criados inicialmente, em seguida, sempre que as regras de visibilidade alteram a estrutura do formulário.</td>
      <td>callback (Função) - O callback recebe um único argumento, o objeto de formulário do formulário que foi renderizado.</td>
      <td>Objeto MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(retorno de chamada)</td>
      <td>Assim como onFormRender, adiciona um retorno de chamada que é chamado sempre que um formulário é renderizado. Além disso, isso também chama o retorno de chamada imediatamente para todos os formulários que já foram renderizados.</td>
      <td>callback(Função) - O callback recebe um único argumento, o objeto de formulário do formulário renderizado.</td>
      <td></td>
    </tr>
</table>


## Métodos de formulário

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Método</strong></td>
      <td><strong>Descrição</strong></td>
      <td><strong>Parâmetros</strong></td>
      <td><strong>Devoluções</strong></td>
    </tr>
    <tr valign="top">
      <td>.render(formElem)</td>
      <td>Renderiza um objeto de formulário, retornando um objeto jQuery encapsulando um elemento de formulário que contém o formulário. Se for transmitido um formElem, ele usará isso como o elemento de formulário, caso contrário, criará um novo.</td>
      <td>formElem (opcional) - um elemento de formulário jQuery encapsulado por objetos no qual será renderizado.</td>
      <td> Um elemento de formulário jQuery com quebra de objeto contendo o formulário renderizado.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Obtém a ID do formulário.</td>
      <td>n/d</td>
      <td>Número - A ID do objeto de formulário que este formulário representa</td>
    </tr>
    <tr valign="top">
      <td>.getFormElem()</td>
      <td>Obtém o elemento de formulário encapsulado jQuery de um formulário renderizado.</td>
      <td>n/d</td>
      <td>Um elemento de formulário jQuery envolvido em objetos ou nulo se o formulário ainda não tiver sido renderizado com o método render().</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Força a validação do formulário, destacando todos os erros que possam existir e retornando o resultado. Não envia o formulário.</td>
      <td>n/d</td>
      <td>Booleano - Retorna verdadeiro se todos os validadores no formulário forem transmitidos; caso contrário, retorna falso.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(retorno de chamada)</td>
      <td>Adiciona um retorno de chamada de validação que será chamado sempre que a validação for acionada.</td>
      <td>callback(Função) - Um callback que será acionado sempre que a validação ocorrer. A chamada de retorno receberá um parâmetro, um booleano informando se a validação foi bem-sucedida.</td>
      <td>Objeto de formulário - O mesmo objeto de formulário no qual o método foi chamado, para fins de encadeamento.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Aciona o evento de envio do formulário. Isso iniciará o fluxo de envio do, executando a validação, acionando qualquer evento onSubmit, enviando o formulário e acionando qualquer evento onSuccess se o envio do formulário tiver sido bem-sucedido.</td>
      <td>n/d</td>
      <td>Objeto de formulário - O mesmo objeto de formulário no qual o método foi chamado, para fins de encadeamento.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Adiciona um retorno de chamada que será chamado quando o formulário for enviado. Isso é acionado quando o envio começa, antes que o sucesso/falha da solicitação seja conhecido.</td>
      <td>callback - Uma função que será chamada quando o formulário for enviado. Esse retorno de chamada receberá um argumento, esse objeto Form.</td>
      <td>Objeto de formulário - O mesmo objeto de formulário no qual o método foi chamado, para fins de encadeamento.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Adiciona um retorno de chamada que será chamado quando o formulário for enviado com êxito, mas antes do cliente em potencial ser encaminhado para a página de acompanhamento. Pode ser usado para impedir que o cliente potencial seja encaminhado para a página de acompanhamento após o envio bem-sucedido.</td>
      <td>callback - Uma função que será chamada quando o formulário for enviado com êxito. Essa chamada de retorno receberá dois argumentos. Um objeto JS contendo os valores que foram enviados e um URL de string da página de acompanhamento à qual o usuário será encaminhado, ou uma string nula ou vazia se não houver uma página de acompanhamento configurada. Comportamento especial: se esse retorno de chamada retornar "false" (medido usando ===), o visitante NÃO será encaminhado para a página de acompanhamento e a página NÃO será recarregada. Isso permite que o implementador faça processamento extra no url de acompanhamento ou execute uma ação na página usando JavaScript em vez de sair da página.</td>
      <td>Objeto de formulário - O mesmo objeto de formulário no qual o método foi chamado, para fins de encadeamento.</td>
    </tr>
    <tr valign="top">
      <td>.enviável(canSubmit) <em>também disponível como:</em> <em>.enviável(canSubmit)</em></td>
      <td>Obtém ou define se o formulário pode ser enviado. Se chamado sem argumentos, ele obtém o valor; se chamado com um argumento, ele define o valor. Isso pode ser usado para impedir que um formulário seja enviado, enquanto outros critérios fora do formulário normal devem ser atendidos.</td>
      <td>canSubmit (opcional)(Booleano) - Define o formulário como enviado ou não.</td>
      <td>Booleano ou objeto de formulário - Se chamado sem argumentos, retorna um booleano indicando se o formulário é enviado. Se chamado com um argumento, retorna este Objeto de formulário para fins de encadeamento. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Retornará true se todos os campos no formulário tiverem valores não vazios definidos.</td>
      <td>n/d</td>
      <td>Booleano - Verdadeiro se todos os campos tiverem valores não vazios/vazios/não definidos/nulos; caso contrário, falso.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Define valores em um ou mais campos no formulário.</td>
      <td>vals - Um Objeto JS. Para cada par de chave/valor no objeto, o campo de formulário chamado chave será definido como valor.</td>
      <td>não definido(a)s</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Obtém todos os valores de todos os campos do formulário.</td>
      <td>n/d</td>
      <td>Objeto - Um objeto JS que contém pares de chave/valor representando os nomes e valores dos campos no formulário.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(valores)</td>
      <td>Adiciona campos de entrada type=hidden ao formulário.</td>
      <td>valores - um objeto JS que contém pares de chave/valor representando os nomes e valores dos campos ocultos a serem adicionados ao formulário.</td>
      <td>não definido(a)s</td>
    </tr>
    <tr valign="top">
      <td>.vals(valores)</td>
      <td>jQuery style .vals() setter/getter. Se chamado sem argumentos, é equivalente a chamar getValues(). Se chamado com um argumento, é equivalente a chamar setValues()</td>
      <td>valores (opcional) - Objeto</td>
      <td>não definido(a)s</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Mostra uma mensagem de erro, apontando para elem.</td>
      <td>msg (String of HTML) - Uma string que contém o texto do erro que você deseja mostrar.</td>
            <td>Objeto de formulário - Este objeto de formulário, para encadeamento.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>elem (opcional)(Objeto jQuery)- O elemento para o qual o erro aponta. Se não estiver definido, o botão enviar do formulário será usado.</td>
<td></td>
    </tr>
  </tbody>
</table>
