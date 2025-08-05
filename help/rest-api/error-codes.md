---
title: Códigos de erro
feature: REST API
description: Descrições do código de erro do Marketo.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
source-git-commit: 71e685efb25acffb5c1000293daaea4e936fc4f5
workflow-type: tm+mt
source-wordcount: '2320'
ht-degree: 3%

---

# Códigos de erro

Abaixo estão listas de códigos de erro da API REST e uma explicação de como os erros são retornados aos aplicativos.

## Tratando e Registrando Exceções

Ao desenvolver para o Marketo, é importante que as solicitações e respostas sejam registradas quando uma exceção inesperada for encontrada. Embora certos tipos de exceções, como autenticação expirada, possam ser manipulados com segurança por meio da reautenticação, outros podem exigir interações de suporte, e as solicitações e respostas sempre serão solicitadas nesse cenário.

## Tipos de erro

A API REST do Marketo pode retornar três tipos diferentes de erros em operação normal:

* Nível de HTTP: esses erros são indicados por um código `4xx`.
* Nível de resposta: esses erros são incluídos na matriz &quot;erros&quot; da resposta JSON.
* Nível de registro: esses erros são incluídos na matriz &quot;resultado&quot; da resposta JSON e são indicados em uma base de registro individual com o campo &quot;status&quot; e a matriz &quot;motivos&quot;.

Para os tipos de erro de Nível de resposta e Nível de registro, um código de status HTTP 200 é retornado. Para todos os tipos de erro, a frase de motivo HTTP não deve ser avaliada, pois é opcional e está sujeita a alterações.

### Erros de nível de HTTP

Em circunstâncias normais de operação, o Marketo só deve retornar dois erros de código de status HTTP, `413 Request Entity Too Large` e `414 Request URI Too Long`. Ambos são recuperáveis ao capturar o erro, modificar a solicitação e tentar novamente, mas com práticas de codificação inteligente, você nunca deve encontrá-los na natureza.

O Marketo retornará 413 se a Carga da solicitação exceder 1 MB, ou 10 MB no caso de Lead de importação. Na maioria dos cenários, é improvável atingir esses limites, mas adicionar uma verificação ao tamanho da solicitação e mover quaisquer registros, o que faz com que o limite seja excedido para uma nova solicitação, deve evitar quaisquer circunstâncias, que levam a esse erro ser retornado por quaisquer pontos de extremidade.

O 414 será retornado quando o URI de uma solicitação GET exceder 8KB. Para evitá-lo, verifique o comprimento da sua cadeia de caracteres de consulta para ver se ela excede esse limite. Se isso fizer com que sua solicitação seja alterada para um método POST, insira sua cadeia de caracteres de consulta como o corpo da solicitação com o parâmetro adicional `_method=GET`. Isso abre mão da limitação nos URIs. É raro atingir esse limite na maioria dos casos, mas é um pouco comum ao recuperar grandes lotes de registros com valores de filtro individuais longos, como uma GUID.
O ponto de extremidade [Identidade](https://developer.adobe.com/marketo-apis/api/identity/) pode retornar um erro 401 Não Autorizado. Normalmente, isso se deve a uma ID de cliente inválida ou a um Segredo do cliente inválido. Códigos de erro de nível HTTP

<table>
  <thead>
    <tr>
      <th> Código de resposta </th>
      <th> Descrição </th>
      <th colspan="1"> Comentário </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="413"></a>413</td>
      <td>Entidade de Solicitação Muito Grande</td>
      <td>A carga excedeu o limite de 1 MB.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>URI de solicitação muito longo</td>
      <td>O URI da solicitação excedeu 8k. A solicitação deve ser repetida como um POST com o parâmetro `_method=GET` no URL e o restante da sequência de consulta no corpo da solicitação.</td>
    </tr>
  </tbody>
</table>

#### Erros de nível de resposta

Erros de nível de resposta estão presentes quando o parâmetro `success` da resposta é definido como false, e são estruturados como:

```json
{
    "requestId": "e42b#14272d07d78",
    "success": false,
    "errors": [
        {
            "code": "601",
            "message": "Unauthorized"
        }
    ]
}
```

Cada objeto na matriz &quot;errors&quot; tem dois membros, `code`, que é um inteiro entre aspas de 601 a 799 e um `message` que fornece a razão do texto sem formatação para o erro. Os códigos 6xx sempre indicam que uma solicitação falhou completamente e não foi executada. Um exemplo é um 601, &quot;Token de acesso inválido&quot;, que pode ser recuperado através da reautenticação e transmissão do novo token de acesso com a solicitação. Os erros 7xx indicam que a solicitação falhou, seja porque nenhum dado foi retornado ou porque a solicitação foi parametrizada incorretamente, como a inclusão de uma data inválida ou a ausência de um parâmetro obrigatório.

#### Códigos de erro de nível de resposta

Uma chamada de API que retorna esse código de resposta não é contabilizada em relação à sua cota diária ou ao seu limite de taxa.

<table>
  <thead>
    <tr>
      <th> Código de resposta </th>
      <th> Descrição </th>
      <th> Comentário </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="502"></a>502</td>
      <td>Gateway inválido</td>
      <td>O servidor remoto retornou um erro. Provavelmente, um tempo limite. A solicitação deve ser repetida com o retrocesso exponencial.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Token de acesso inválido</td>
      <td>Um parâmetro de token de acesso foi incluído na solicitação, mas o valor não era um token de acesso válido.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Token de acesso expirado</td>
      <td>O token de acesso incluído na chamada não é mais válido devido à expiração.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Acesso negado</td>
      <td>A autenticação foi bem-sucedida, mas o usuário não tem permissão suficiente para chamar essa API. Talvez seja necessário atribuir [permissões adicionais](custom-services.md) à função de usuário ou habilitar o <a href="https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">Incluir na lista de permissões Acesso à API Baseada em IP</a>.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Tempo limite da solicitação</td>
      <td>A solicitação estava em execução por muito tempo (por exemplo, encontrou contenção de banco de dados) ou excedeu o período de tempo limite especificado no cabeçalho da chamada.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>Método HTTP não suportado</td>
      <td>O GET não é compatível com o ponto de extremidade Sync Leads. O POST deve ser usado.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Limite máximo de taxa `%s`; excedido com em `%s` segundos</td>
      <td>O número de chamadas nos últimos 20 segundos foi maior que 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Cota diária atingida</td>
      <td>O número de chamadas hoje excedeu a cota da assinatura (é redefinido diariamente às 12h00 CST).&gt;Sua cota pode ser encontrada no menu Admin-&gt;Serviços da Web. Você pode aumentar sua cota por meio do gerente da conta.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>API temporariamente indisponível</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>JSON inválido</td>
      <td>O corpo incluído na solicitação não é um JSON válido.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>Recurso solicitado não encontrado</td>
      <td>O URI na chamada não correspondia a um tipo de recurso da API REST. Geralmente, isso se deve a um URI de solicitação com ortografia ou formatação incorreta</td>
    </tr>
    <tr>
      <td><a name="611"></a>611 *</td>
      <td>Erro do sistema</td>
      <td>Todas as exceções não tratadas</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Tipo de conteúdo inválido</td>
      <td>Se você vir esse erro, adicione um cabeçalho de tipo de conteúdo especificando o formato JSON à solicitação. Por exemplo, tente usar "content type: application/json". <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Consulte esta pergunta sobre StackOverflow</a> para obter mais detalhes.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Solicitação de várias partes inválida</td>
      <td>O conteúdo multiparte da publicação não foi formatado corretamente</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Assinatura inválida</td>
      <td>A assinatura de destino não pode ser encontrada ou está inacessível. Isso geralmente indica inacessibilidade temporária.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Limite de acesso simultâneo atingido</td>
      <td>No máximo, as solicitações são processadas por qualquer assinatura 10 de cada vez. Isso é retornado se já houver 10 solicitações em andamento.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Tipo de assinatura inválido</td>
      <td>O tipo apropriado de assinatura do Marketo é necessário para acessar a API de metadados de objeto personalizado. Consulte seu CSM para obter detalhes.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s não pode ficar em branco</td>
      <td>O campo relatado não deve estar vazio na solicitação</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Nenhum dado encontrado para um determinado cenário de pesquisa</td>
      <td>Nenhum registro correspondeu aos parâmetros de pesquisa fornecidos.
        Observação: muitas operações de pesquisa com falha retornam `success = true` e sem erros e definem uma cadeia informativa de avisos.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>O recurso não está habilitado para a assinatura</td>
      <td>Um recurso beta que não foi ativado na assinatura de um usuário</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Formato de data inválido</td>
      <td><ul>
          <li>Foi especificada uma data que não estava no formato correto</li>
          <li>Uma ID de conteúdo dinâmico inválida foi especificada</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Violação de Regra de Negócios</td>
      <td>A chamada não pode ser atendida porque viola um requisito para criar ou atualizar um ativo, por exemplo, tentar criar um email sem um modelo. Também é possível obter esse erro ao tentar:
        <ul>
          <li>Recupere conteúdo para páginas de aterrissagem que contenham conteúdo social.</li>
          <li>Clonar um programa que contenha determinados tipos de ativos (consulte <a href="programs.md#clone">Clonar programa</a> para obter mais informações).</li>
          <li>Aprovar um ativo que não tem rascunho (ou seja, já foi aprovado).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Pasta pai não encontrada</td>
      <td>A pasta pai especificada não foi encontrada</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Tipo de pasta incompatível</td>
      <td>A pasta especificada não era do tipo correto para atender à solicitação</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>A operação de mesclagem para conta de pessoa é inválida</td>
      <td>Falha na chamada de Mesclagem de Clientes Potenciais devido a uma tentativa de mesclar clientes potenciais que são Contas Pessoais da Salesforce.  As contas de pessoas da Salesforce devem ser mescladas no Salesforce.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Erro transitório</td>
      <td>Um recurso do sistema estava temporariamente indisponível no momento da chamada à API. Quando esse erro é encontrado, é recomendável aguardar o tempo e tentar a solicitação novamente.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Não é possível localizar o tipo de registro padrão</td>
      <td>Falha em uma chamada de Mesclagem de Clientes Potenciais porque não foi possível localizar um tipo de registro padrão.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID não encontrado</td>
      <td>Uma chamada de Oportunidades de sincronização foi feita com um valor "ExternalSalesPersonID" inexistente.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Exceção de tempo limite de espera de bloqueio</td>
      <td>Uma chamada de Programa de Clonagem foi feita e atingiu o tempo limite aguardando um bloqueio.</td>
    </tr>
  </tbody>
</table>

### Nível de registro {#record_level_errors}

Os erros no nível do registro indicam que não foi possível concluir uma operação para um registro individual, mas a própria solicitação era válida. Uma resposta com erros de nível de registro segue este padrão:

#### Resposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "status":"skipped",
         "reasons":[
            {
               "code":"1005",
               "message":"Lead already exists"
            }
         ]
      }
   ]
}
```

Os registros incluídos na matriz de resultados das chamadas são ordenados da mesma forma que a matriz de entrada de uma solicitação.
Cada registro em uma solicitação bem-sucedida pode ter êxito ou falha individualmente, o que é indicado pelo campo de status de cada registro incluído na matriz de resultados de uma resposta. O campo &quot;status&quot; desses registros será &quot;ignorado&quot; e uma matriz &quot;reason&quot; estará presente. Cada motivo contém um membro &quot;código&quot; e um membro &quot;mensagem&quot;. O código é sempre 1xxx e a mensagem indica por que o registro foi ignorado. Um exemplo seria quando uma solicitação Sincronizar clientes em potencial tem &quot;ação&quot; definida como &quot;createOnly&quot;, mas já existe um cliente em potencial para uma das chaves nos registros enviados. Esse caso retorna um código 1005 e uma mensagem de &quot;Lead já existe&quot; conforme exibido acima.

#### Códigos de erro de nível de registro

>[!NOTE]
>
><table>
><tbody>
>    <tr>
>      <td>Código de resposta</td>
>      <td>Descrição</td>
>      <td>Comentário</td>
>    </tr>
>    <tr>
>      <td><a name="1001"></a>1001</td>
>      <td>Valor inválido ‘%s’. Obrigatório do tipo ‘%s’</td>
>      <td>O erro é gerado sempre que um valor de parâmetro tem uma incompatibilidade de tipo. Por exemplo, o valor da string especificado para um parâmetro inteiro.</td>
>    </tr>
>    <tr>
>      <td><a name="1002"></a>1002</td>
>      <td>Valor ausente para o parâmetro obrigatório ‘%s’</td>
>      <td>O erro é gerado quando um parâmetro obrigatório está ausente na solicitação</td>
>    </tr>
>    <tr>
>      <td><a name="1003"></a>1003</td>
>      <td>Dados inválidos</td>
>      <td>Quando os dados enviados não são de um tipo válido para o endpoint ou modo especificado, como quando a ID é enviada para um cliente potencial com a ação designada como createOnly ou ao usar a Campanha de solicitação em uma campanha em lote.</td>
>    </tr>
>    <tr>
>      <td><a name="1004"></a>1004</td>
>      <td>Lead não encontrado</td>
>      <td>Para syncLead, quando a ação for "updateOnly" e se o lead não for encontrado</td>
>    </tr>
>    <tr>
>      <td><a name="1005"></a>1005</td>
>      <td>O cliente em potencial já existe</td>
>      <td>Para syncLead, quando a ação for "createOnly" e um lead já existir</td>
>    </tr>
>    <tr>
>      <td><a name="1006"></a>1006</td>
>      <td>Campo ‘%s’ não encontrado</td>
>      <td>Um campo incluído na chamada não é válido.</td>
>    </tr>
>    <tr>
>      <td><a name="1007"></a>1007</td>
>      <td>Vários leads correspondem aos critérios de pesquisa</td>
>      <td>Vários clientes em potencial correspondem aos critérios de pesquisa. As atualizações só podem ser executadas quando a chave corresponde a um único registro</td>
>    </tr>
>    <tr>
>      <td><a name="1008"></a>1008</td>
>      <td>Acesso negado à partição ‘%s’</td>
>      <td>O usuário do serviço personalizado não tem acesso a um espaço de trabalho com a partição em que o registro existe.</td>
>    </tr>
>    <tr>
>      <td><a name="1009"></a>1009</td>
>      <td>O nome da partição deve ser especificado</td>
>      <td></td>
>    </tr>
>    <tr>
>      <td><a name="1010"></a>1010</td>
>      <td>Atualização de partição não permitida</td>
>      <td>O registro especificado já existe em uma partição de cliente potencial separada.</td>
>    </tr>
>    <tr>
>      <td><a name="1011"></a>1011</td>
>      <td>O campo ‘%s’ não é suportado</td>
>      <td>Quando o campo de pesquisa ou "filterType" é especificado com campos padrão sem suporte (por exemplo: firstName, lastName)</td>
>    </tr>
>    <tr>
>      <td><a name="1012"></a>1012</td>
>      <td>Valor de cookie inválido ‘%s’</td>
>      <td>Pode ocorrer ao chamar o <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST">Associar lead</a> com um valor inválido para o parâmetro "cookie".
>        Isso também ocorre ao chamar <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET">Obter clientes em potencial por Tipo de filtro</a> com "filterType=cookies" e um valor inválido para o parâmetro "filterValues".</td>
>    </tr>
>    <tr>
>      <td><a name="1013"></a>1013</td>
>      <td>Objeto não encontrado</td>
>      <td>Obter objeto (lista, campanha) por id retorna este código de erro</td>
>    </tr>
>    <tr>
>      <td><a name="1014"></a>1014</td>
>      <td>Falha ao criar objeto</td>
>      <td>Falha ao criar objeto (lista)</td>
>    </tr>
>    <tr>
>      <td><a name="1015"></a>1015</td>
>      <td>Cliente em potencial não está na lista</td>
>      <td>O cliente em potencial designado não é um membro da lista de público alvo</td>
>    </tr>
>    <tr>
>      <td><a name="1016"></a>1016</td>
>      <td>Muitas importações</td>
>      <td>Há muitas importações na fila. Um máximo de 10 é permitido</td>
>    </tr>
>    <tr>
>      <td><a name="1017"></a>1017</td>
>      <td>O objeto já existe</td>
>      <td>Falha na criação porque o registro já existe</td>
>    </tr>
>    <tr>
>      <td><a name="1018"></a>1018</td>
>      <td>CRM Habilitado</td>
>      <td>A ação não pôde ser executada porque a instância tem uma integração nativa do CRM habilitada.</td>
>    </tr>
>    <tr>
>      <td><a name="1019"></a>1019</td>
>      <td>Importação em andamento</td>
>      <td>A lista de destino já está sendo importada para</td>
>    </tr>
>    <tr>
>      <td><a name="1020"></a>1020</td>
>      <td>Muitos clones para o programa</td>
>      <td>A assinatura atingiu o uso alocado de "cloneToProgramName" no Programa de agendamento para o dia</td>
>    </tr>
>    <tr>
>      <td><a name="1021"></a>1021</td>
>      <td>Atualização de empresa não permitida</td>
>      <td>Atualização da empresa não permitida durante syncLead</td>
>    </tr>
>    <tr>
>      <td><a name="1022"></a>1022</td>
>      <td>Objeto em uso</td>
>      <td>A exclusão não é permitida quando um objeto está em uso por outro objeto</td>
>    </tr>
>    <tr>
>      <td><a name="1025"></a>1025</td>
>      <td>Status do programa não encontrado</td>
>      <td>Foi especificado um status para Alterar o status do programa de cliente potencial que não corresponde a um status disponível para o canal do programa.</td>
>    </tr>
>    <tr>
>      <td><a name="1026"></a>1026</td>
>      <td>Objeto personalizado não habilitado</td>
>      <td>A ação não pôde ser executada, pois a instância não tem a integração de objetos personalizados habilitada.</td>
>    </tr>
>    <tr>
>      <td><a name="1027"></a>1027</td>
>      <td>Limite máximo de tipos de atividade atingido</td>
>      <td>A assinatura atingiu o número máximo de tipos de atividades personalizadas disponíveis.</td>
>    </tr>
>    <tr>
>      <td><a name="1028"></a>1028</td>
>      <td>Limite máximo de campos atingido</td>
>      <td>As atividades personalizadas têm no máximo 20 atributos secundários.</td>
>    </tr>
>    <tr>
>      <td><a name="1029"></a>1029</td>
>      <td><ul>
>          <li>Muitos trabalhos na fila</li>
>          <li>Exportação de cota diária excedida</li>
>          <li>Tarefa já na fila</li>
>        </ul></td>
>      <td><ul>
>          <li>As assinaturas podem ter no máximo 10 trabalhos de extração em massa na fila em um determinado momento.</li>
>          <li>Por padrão, os trabalhos de extração são limitados a 500 MB por dia (é redefinido diariamente às 12h00 CST).</li>
>          <li>A ID de exportação já foi colocada na fila.</li>
>        </ul></td>
>    </tr>
>    <tr>
>      <td><a name="1035"></a>1035</td>
>      <td>Tipo de filtro incompatível</td>
>      <td>Em algumas assinaturas, os seguintes tipos de filtro de Extração de lead em massa não são compatíveis: updatedAt, smartListId, smartListName.</td>
>    </tr>
>    <tr>
>      <td><a name="1036"></a>1036</td>
>      <td>Objeto duplicado encontrado na entrada</td>
>      <td>Foi feita uma chamada para atualizar dois ou mais registros usando a mesma chave estrangeira. Por exemplo, uma chamada de Empresas de sincronização usando a mesma externalCompanyId para mais de uma empresa.</td>
>    </tr>
>    <tr>
>      <td><a name="1037"></a>1037</td>
>      <td>O lead foi ignorado</td>
>      <td>O cliente em potencial foi ignorado porque já está neste status ou já passou dele.</td>
>    </tr>
>    <tr>
>      <td><a name="1042"></a>1042</td>
>      <td>Data runAt inválida</td>
>      <td>A data runAt especificada para o Schedule Campaign estava muito distante no futuro (o máximo é 2 anos).</td>
>    </tr>
>    <tr>
>      <td><a name="1048"></a>1048</td>
>      <td>Falha ao descartar rascunho de objeto personalizado</td>
>      <td>Foi feita uma chamada para descartar a versão de rascunho de um objeto personalizado.</td>
>    </tr>
>    <tr>
>      <td><a name="1049"></a>1049</td>
>      <td>Falha ao criar atividade</td>
>      <td>Matriz de atributos muito longa.
>        A matriz de atributos passada para o registro excedeu o comprimento máximo de 65.536 bytes</td>
>    </tr>
>    <tr>
>      <td><a name="1076"></a>1076</td>
>      <td>A chamada <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Mesclar clientes em potencial</a> com o sinalizador mergeInCRM é 4.</td>
>      <td>Você está criando um registro duplicado. É recomendável usar um registro existente.
>        Essa é a mensagem de erro, que o Marketo recebe ao mesclar no Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1077"></a>1077</td>
>      <td>A chamada <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Mesclar clientes em potencial</a> falhou devido ao comprimento do "Campo SFDC"</td>
>      <td>Uma chamada de Mesclagem de leads com mergeInCRM definido como true falhou porque o "Campo SFDC" excede o limite de caracteres permitidos. Para corrigir, reduza o comprimento de "SFDC Field" ou defina mergeInCRM como false.</td>
>    </tr>
>    <tr>
>      <td><a name="1078"></a>1078</td>
>      <td>A chamada <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Mesclar clientes em potencial</a> falhou devido à entidade excluída, não a um cliente potencial/contato ou a critérios de filtro de campo não correspondem.</td>
>      <td>Falha na mesclagem; não é possível executar a operação de mesclagem no CRM sincronizado nativamente
>        Essa é a mensagem de erro, que o Marketo recebe ao mesclar no Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1079"></a>1079</td>
>      <td>A chamada <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Mesclar clientes em potencial</a> falhou devido ao conflito de URL personalizado em registros duplicados</td>
>      <td>Uma chamada de Mesclar leads especificou muitos leads com a mesma URL personalizada. Para resolver, use a interface do usuário do Marketo Engage para mesclar esses registros.</td>
>    </tr>
>  </tbody>
></table>
