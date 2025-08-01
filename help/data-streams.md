---
title: Fluxos de dados
description: Visão geral dos fluxos de dados
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1601'
ht-degree: 2%

---

# Fluxos de dados

>[!NOTE]
> Informações atuais sobre fluxos de dados agora estão disponíveis em [Usando Fluxos de Dados](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/).
>

As organizações de marketing de nossos clientes dependem de Campanhas de marketing focadas e oportunas para manter-se a par de seus negócios e serem competitivas. Para dar suporte a decisões rápidas e permitir alterações estratégicas rapidamente, é importante ter dados que apóiem e impulsionem as principais decisões que oferecem campanhas focadas e direcionadas. Também há alguns clientes que realizam esforços de marketing nos níveis de seus segmentos de clientes, dentro e fora da Marketo Engage. Para dar suporte a esses diferentes esforços, a Marketo criou a capacidade de adquirir grandes volumes de dados em tempo quase real por meio dos Fluxos de dados.

Além do benefício dos dados quase em tempo real, há benefícios relacionados ao produto:

- Alivia o gargalo dos limites da API, pois a transmissão é usada em vez disso.
- Reduz o cenário de limites da API, gerando menos mensagens de alerta.
- Não é necessário executar exportações em massa para extrair dados devido ao recurso de transmissão de dados.

Os Fluxos de Dados estão disponíveis para quem comprou um [Pacote da Camada de Desempenho do Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Visão Geral do Fluxo de Dados da Atividade de Cliente Potencial

O Fluxo de dados de atividades de clientes potenciais fornece transmissão quase em tempo real de atividades de clientes potenciais de rastreamento de auditoria, nas quais grandes volumes de atividades de clientes potenciais podem ser enviados para o sistema externo de um cliente. Os fluxos permitem que os clientes auditem com eficiência eventos relacionados a clientes potenciais, padrões de uso, fornecem exibições de alterações de clientes potenciais e acionam processos e fluxos de trabalho com base nos diferentes tipos de eventos potenciais. Há mais de 144 tipos de atividades que podem ser assinadas e enviadas pelo fluxo.

Tipos de dados de clientes potenciais transmitidos:

1. Alterações de Cliente Potencial - todas as alterações em todos os campos e novos Clientes Potenciais
1. Atividades de cliente potencial - todos os tipos de atividades de cliente potencial descritos no documento
1. Clientes Potenciais Excluídos
1. Todos os objetos personalizados no lead (se solicitado). É tudo ou nada neste momento.

Ao fornecer visualizações sobre alterações de lead, os clientes podem tomar decisões mais rápidas sobre suas estratégias de marketing gerais e criar campanhas direcionadas mais focadas. Alguns casos de uso populares seriam:

- Alertas personalizados: quando determinados clientes em potencial são encontrados com condições inconsistentes, eles podem ser adicionados à lista. Os fluxos de atividade podem coletá-los e enviar a atividade &quot;Adicionar à lista&quot; para que os clientes realizem qualquer ação de acompanhamento.
- Alimentação de modelos de ML: alguns clientes planejam criar modelos de pontuação que usam insights de atividade e os retornam ao Marketo ou usam em seus próprios modelos de pontuação internos, conforme desejado. Ao pontuar um lead, os clientes podem informar ao Marketo para adicionar clientes às campanhas de criação para aumentar sua pontuação.

Lista de atividades transmitidas:

| AtingirMetaNaReferência | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | ReferToSocialApp |
| AddToOpportunity | ClickSharedLink | RemoverDaLista |
| AddToSalesCampaign | ConverterCliente Potencial | RemoverDaOportunidade |
| CallWebhook | ExcluirClientePotencial | RequestCampaign |
| ChangeDataValue | Desqualificar sorteios | SalesEmailBounce |
| AlterarPartiçãodeClientePotencial | GanharEntradaNoAplicativoSocial | SendAlert |
| ChangeNurtureCadence | EmailDevolvido | SendEmail |
| ChangeNurtureTrack | EmailBounceSoft | SendSalesEmail |
| ChangeOwner | EmailDelivered | SentForwardToFriendEmail |
| ChangeProgramData | EnrichWithDataDotCom | Atividade SFDCA |
| ChangeProgramMemberData | EnterSweepstakes | CompartilharConteúdo |
| ChangeRevenueStage | PreencherFormulárioDeAnúnciosEmPotencialDoFacebook | InscreverParaOfertaDeReferência |
| ChangeRevenueStageManually | PreencherFormulário | SyncLeadToMicrosoft |
| ChangeScore | MomentoInteressante | SyncLeadToSFDC |
| ChangeSegment | MesclarClientes Potenciais | Cancelar assinatura de email |
| ChangeStatusInProgression | Novo cliente em potencial | AtualizarOportunidade |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | VoteInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Observe que se for necessário transmitir os objetos personalizados, eles deverão ser todos os objetos personalizados relacionados ao cliente potencial. No momento, não há como selecionar quais são desejados.

## Visão geral do fluxo de dados de auditoria do usuário

O Fluxo de dados de auditoria do usuário fornece rastreamento de auditoria quase em tempo real de alterações de ativos por usuários&#x200B;. Isso permite que um cliente auditore com eficiência eventos de ativos, forneça uma visualização das alterações do usuário e acione processos ou workflows com base em diferentes tipos de eventos de auditoria. As alterações de ativos quase em tempo real são enviadas por meio de eventos do Adobe I/O para um endpoint configurável. Os eventos de auditoria são divididos por tipo de ativo e podem assinar eventos de auditoria importantes para eles.

Um bom caso de uso para assinar esse fluxo seria:

- Rastreamento de alterações ao usar vários sistemas de marketing: há alguns clientes que também realizam algum nível de atividades de marketing em outro sistema, como um CRM, como o Salesforce, e depois transmitem o lead para o Marketo. Às vezes, o lead é atualizado e sincronizado de um lado para o outro. Portanto, é importante rastrear quais sistemas fizeram alterações recentes.

Lista de eventos de auditoria de usuários transmitidos:

| COMPONENTE | LISTA DE TIPOS DE EVENTO |
|--- |--- |
| Programa padrão | clonar, criar, excluir, editar canal, exportar, modificar configuração de programa, modificar token de programa, renomear |
| Email | aprovar, clonar, criar, excluir, editar, mover, renomear, cancelar aprovação |
| Programa de e-mail em lote | aprovar, childUpdate, clonar, criar, excluir, editar, editar canal, modificar programação de programa, modificar configuração de programa, modificar token de programa, renomear, cancelar aprovação |
| Modelo de e-mail | aprovar, clonar, criar, excluir, rascunhoCriar, rascunhoDescartar, editar, renomear, cancelar aprovação |
| Programa de engajamento | clonar, criar, excluir, editar canal, modificar configuração de programa, modificar fluxo de programa, modificar token de programa, renomear |
| Programa de eventos | clonar, criar, excluir, editar canal, modificar programação de programa, modificar configuração de programa, modificar token de programa, renomear |
| Pasta | criar, excluir, editar, renomear |
| Formulário | aprovar, clonar, criar, excluir, rascunhoCriar, editar, mover, renomear |
| Formulário -> Formulário de página de aterrissagem | criar, clonar, editar, excluir, aprovar, renomear |
| Página de destino | aprovar, clonar, criar, excluir, rascunhoDescartar, editar, renomear, cancelar aprovação |
| Modelo de página | aprovar, clonar, criar, excluir, rascunhoCriar, rascunhoDescartar, editar, renomear, cancelar aprovação |
| Lista inteligente | clonar, criar, excluir, editar, exportar, modificar configuração de lista inteligente, renomear |
| Pasta de marketing | criar, editar, excluir |
| Programa de promoção | clonar, criar, excluir, editar canal, modificar configuração de programa, modificar fluxo de programa, modificar token de programa, renomear |
| Segmento | criar, excluir, editar, renomear |
| Segmentação | aprovar, criar, excluir, rascunhoCriado, rascunhoDescartado, renomear, cancelar aprovação |
| Campanha inteligente | abortar, ativar, clonar, criar, desativar, excluir, editar, modificar programação de campanha, modificar ação da etapa do fluxo, modificar configuração da smart list, mover, renomear |
| Bloco de conteúdo | aprovar, aprovar sem rascunho, clonar, criar, excluir, editar, renomear, cancelar aprovação |
| Interface do administrador -> Launchpoint -> Integração | criar, excluir, editar |
| Interface do administrador -> Usuário | criar, editar, excluir (o mesmo para o usuário somente API) |
| Login do administrador -> Usuário | sucesso de login, falha de login |
| Programa -> Programa em lote de emails | editar (para alterar o endereço de email selecionado) a API do ativo |
| Programa -> Programa de marketing | criar, clone |

Exemplo de evento de auditoria do usuário:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "id": "b77c743a-8e28-40f2-8aab-9541bbc85e68",
        "type": "com.adobe.platform.marketo.audit.user.email",
        "source": "https://www.marketo.com",
        "time": "2020-05-28T19:20:47.28Z",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentId": 232459,
            "componentType": "Email",
            "eventAction": "approve",
            "munchkinId": "123-ABC-456",
            "imsOrgId": "ADOBEORGID@AdobeOrg",
            "user": 253,
            "userId": "example@marketo.com"
        }
    }
}
```

## Visão geral do fluxo de dados de notificação

O Fluxo de dados de notificação está disponível como parte das ofertas de nível de desempenho do Marketo Engage.

Atualmente, o centro de notificações no Marketo pode ser configurado para enviar notificações a um endereço de email. O Fluxo de dados de notificação permite que as notificações sejam enviadas diretamente para um endpoint configurável por meio de eventos do Adobe I/O. As notificações são fornecidas por meio da interface do usuário hoje e podem ser referenciadas pelo sino laranja na parte superior direita da tela e esse fluxo pega essas notificações e as envia por um fluxo.

Lista de eventos de notificação:

| COMPONENTE | LISTA DE TIPOS DE EVENTO |
|--- |--- |
| Notificação | interrupção da campanha, falha da campanha, promoção (programa esgotado), falha de sincronização do salesforce, grupo de teste (resultado do teste A/B), serviços da web (cota diária) |

Exemplo de evento de notificação:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.notification.campaign_abort",
        "source": "https://www.marketo.com",
        "time": "2021-05-27T10:22:37.489-5:00",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentType": "campaign_abort",
            "subType": "user_campaign_abort",
            "eventAction": {
                "campaignId":1234,
                "userId":"example@marketo.com",
            }
            "imsOrgId":"ADOBEORGID@AdobeOrg",
            "munchkinId":"123-ABC-456"
        }
    }
}
```

## Detalhes técnicos

Esta seção fornece diretrizes sobre o que é necessário, como conectar e receber dados de transmissão para cada um dos fluxos. Há um certo nível de codificação e configuração envolvidas para cada um.

### Fluxo de Dados da Atividade do Cliente Potencial

O Fluxo de atividades de lead fornece transmissão quase em tempo real dos eventos de atividade de lead da Marketo e envia as alterações do tipo de atividade que você assinou com os atributos configuráveis:

- Frequência de envios de dados a cada 2 segundos por padrão.
- Lotes de 100 a 500 por assinatura.
- O tempo limite do serviço REST do cliente é de 20 segundos com 3 tentativas a cada 3 minutos e ativado automaticamente após o sucesso. Caso contrário, depois disso, eles serão pausados. Quando estiver pausado, o serviço tentará novamente a cada 3 minutos, tentando reativar a menos que seja desprovisionado manualmente.
- Retenção de dados em uma fila por até 7 dias.

Para implementar o Fluxo de Dados da Atividade Principal, siga estas etapas para os clientes:

1. Exponha um terminal HTTP que pode receber solicitações POST com um corpo JSON da Internet pública. O fluxo de dados de push da atividade envia solicitações para:
1. Forneça o seguinte à Adobe:
   1. Marketo Munchkin ID para sua assinatura
   1. O URL do endpoint na etapa 1
   1. Os tipos de atividade que eles desejam receber (lista completa acima)
   1. Um meio de autenticação, para que o cliente possa verificar se as solicitações são legítimas. Ou:
      1. Uma URL de provedor de identidade, ID do Cliente e Segredo do Cliente para OAuth [Autenticação de Credenciais do Cliente](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Um token de API, que pode ser incluído em solicitações enviadas pela sequência de dados da atividade principal em um cabeçalho http de autorização

O Adobe ativa o fluxo de dados, momento em que os clientes começam a receber dados.

Diagrama UML de uma chamada típica de Fluxo de Dados de Atividade Principal:

![Diagrama de Fluxo de Dados da Atividade Principal](assets/lead-activity-data-stream.png)

Exemplo de criação de endpoint de URL:

```javascript
/*
Copyright 2022 Adobe
All Rights Reserved.

NOTICE: Adobe permits you to use, modify, and distribute this file in
accordance with the terms of the Adobe license agreement accompanying
it.
*/
constexpress=require('express')
constwinston=require('winston');
constport=3000

constapp=express().use(express.json())

constlogger=winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: {service: 'activity-stream-consumer-example'},
  transports: [
    // - Write all logs with level `error` and below to `error.log`
    newwinston.transports.File({filename: 'error.log',level: 'error'}),
    // - Write all logs with level `info` and below to `combined.log`
    newwinston.transports.File({filename: 'combined.log'}),
    newwinston.transports.Console({format: winston.format.simple()})
  ],
});

app.get('/',(req,res)=>{
  logger.info(JSON.stringify(req.query))
  res.sendStatus(200)
})

app.post('/',(req,res)=>{
  logger.info(JSON.stringify(req.body))
  res.sendStatus(200)
})

app.listen(port,()=>{
  logger.info(`app listening on port ${port}`)
})
```

Uma amostra de código para um aplicativo que consome o Fluxo de Dados de Atividade de Cliente Potencial da Marketo pode ser encontrada [aqui](https://github.com/ihgrant/activity-stream-consumer-example).

### Fluxo de dados de auditoria do usuário e fluxo de dados de notificação

Os eventos de Auditoria de usuário são enviados ao Adobe IO e podem ser consumidos ao fazer logon com uma Adobe ID. Estas são as etapas a serem seguidas:

1. Os clientes fornecem o seguinte à Adobe:
   1. Adobe ID
   1. Marketo Munchkin ID para sua assinatura
1. O cliente expõe um terminal REST para consumir eventos normalmente na forma de um webhook.
1. Depois de fornecido, o Adobe habilita o fluxo para a assinatura do cliente.
1. O cliente então configura o fluxo no Adobe IO (instruções a serem fornecidas)
   1. Esta etapa requer uma organização da Adobe
   1. Exige que o usuário da organização Adobe tenha a função de desenvolvedor ou administrador do sistema

Para configurar o Adobe IO, consulte [Configuração de fluxos de dados de auditoria de usuário do Marketo com o Adobe IO](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) na seção Documentação pública.

### Configuração do fluxo de dados de auditoria do usuário no Marketo

O Fluxo de dados de auditoria do usuário está disponível atualmente como parte dos pacotes de desempenho, juntamente com os outros 3 Fluxos de dados. Para obter mais informações sobre os Pacotes, consulte a [Página de Descrição do Produto](https://helpx.adobe.com/legal/product-descriptions/adobe-marketo-engage---product-description.html) para obter os limites e recursos do produto.

### Configuração do Adobe I/O

[Consulte Introdução ao Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Para obter instruções básicas para este caso de uso, a partir de [console.adobe.io](https://developer.adobe.com/console):

Quando solicitado, selecione **[!UICONTROL Criar novo projeto]** ou **[!UICONTROL Adicionar evento]**.

### Introdução ao novo projeto

Para começar a usar os serviços da Adobe, adicione uma API, eventos ou tempo de execução, exiba nossa [documentação](https://developer.adobe.com/runtime/docs/).

## Documentação pública

- [Fluxos de dados do Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Introdução a eventos e webhooks do Adobe IO](https://developer.adobe.com/events/docs/guides/)
- [Blog de Fluxos de Dados](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
