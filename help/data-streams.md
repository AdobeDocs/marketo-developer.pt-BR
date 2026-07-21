---
title: Fluxos de dados
description: Visão geral dos fluxos de dados do Marketo Engage, permitindo atividades de clientes potenciais quase em tempo real e eventos de auditoria de usuários, diminuindo os limites da API para clientes da camada de desempenho
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
TQID: https://experienceleague.adobe.com/JnhN70HexjmNueZa9MAVrxjEhZ5yJatWqZiowl22quA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1347
ht-degree: 4%

---

# Fluxos de dados

>[!NOTE]
>
>Informações atuais sobre fluxos de dados agora estão disponíveis em [Usando Fluxos de Dados](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams#).
>

Os fluxos de dados fornecem grandes volumes de dados do Marketo Engage para sistemas externos em tempo quase real. Use dados transmitidos para apoiar decisões oportunas, campanhas direcionadas, processos de marketing externo e auditoria.

Os fluxos de dados oferecem estes benefícios:

- Reduza a dependência de solicitações de API com taxa limitada.
- Reduza os alertas de limite de API.
- Fornecer dados sem executar exportações em massa.

Os Fluxos de Dados estão disponíveis para quem comprou um [Pacote da Camada de Desempenho do Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Visão Geral do Fluxo de Dados da Atividade de Cliente Potencial

O Fluxo de Dados da Atividade do Cliente Potencial envia grandes volumes de dados da atividade do cliente potencial para um sistema externo em tempo quase real. Use o fluxo para auditar eventos de cliente potencial e padrões de uso, exibir alterações de cliente potencial e acionar workflows de eventos de cliente potencial.

É possível assinar mais de 144 tipos de atividades.

O fluxo pode incluir:

1. Alterações em todos os campos de cliente potencial e clientes potenciais recém-criados.
1. Todos os tipos de atividades de cliente potencial documentados.
1. Clientes potenciais excluídos.
1. Todos os objetos personalizados principais, quando solicitado. Não é possível selecionar objetos personalizados individuais.

Casos de uso comuns incluem:

- Alertas personalizados: adicionar leads com condições inconsistentes a uma lista. O fluxo envia a atividade Adicionar à lista para um processo de acompanhamento.
- Modelos de aprendizado de máquina: use insights de atividade em modelos de pontuação externos e envie pontuações à Marketo para influenciar campanhas de promoção ou outros processos.

Lista de atividades transmitidas:

| AtingirMetaNaReferência | ClickPredictiveContent | ReceivedForwardToFriendEmail |
| --- | --- | --- |
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

Ao transmitir objetos personalizados, inclua todos os objetos personalizados relacionados a leads. Não é possível selecionar objetos personalizados individuais.

## Visão geral do fluxo de dados de auditoria do usuário

O Fluxo de dados de auditoria do usuário rastreia as alterações do usuário em ativos em tempo quase real. Use-o para auditar eventos de ativos, exibir alterações de usuários e acionar processos de eventos de auditoria.

O Adobe I/O Events envia as alterações para um endpoint configurável. Inscreva-se nos tipos de evento necessários para cada tipo de ativo.

Um caso de uso é:

- Rastreamento de alterações em sistemas de marketing: quando um CRM ou outro sistema troca clientes potenciais com o Marketo, use eventos de auditoria para identificar qual sistema fez a alteração mais recente.

Lista de eventos de auditoria de usuários transmitidos:

| COMPONENTE | LISTA DE TIPOS DE EVENTO |
| --- | --- |
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
| Modelo de página de destino | aprovar, clonar, criar, excluir, rascunhoCriar, rascunhoDescartar, editar, renomear, cancelar aprovação |
| Lista inteligente | clonar, criar, excluir, editar, exportar, modificar configuração de lista inteligente, renomear |
| Pasta de marketing | criar, editar, excluir |
| Programa de estímulo | clonar, criar, excluir, editar canal, modificar configuração de programa, modificar fluxo de programa, modificar token de programa, renomear |
| Segmento | criar, excluir, editar, renomear |
| Segmentação | aprovar, criar, excluir, rascunhoCriado, rascunhoDescartado, renomear, cancelar aprovação |
| Campanha inteligente | abortar, ativar, clonar, criar, desativar, excluir, editar, modificar programação de campanha, modificar ação da etapa do fluxo, modificar configuração da smart list, mover, renomear |
| Snippet | aprovar, aprovar sem rascunho, clonar, criar, excluir, editar, renomear, cancelar aprovação |
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

O centro de notificações da Marketo pode enviar notificações para um endereço de email. O Fluxo de dados de notificação também envia essas notificações para um terminal configurável por meio do Adobe I/O Events. Essas são as mesmas notificações disponíveis no ícone de sino na interface do Marketo.

Lista de eventos de notificação:

| COMPONENTE | LISTA DE TIPOS DE EVENTO |
| --- | --- |
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

As seções a seguir descrevem a configuração necessária para receber dados de cada fluxo. Cada fluxo requer configuração de endpoint e código de integração.

### Fluxo de Dados da Atividade do Cliente Potencial

O Fluxo de Atividades de Cliente Potencial envia eventos de atividade de cliente potencial inscritos com estas características de serviço:

- Os dados são enviados a cada dois segundos por padrão.
- Cada assinatura usa lotes de 100 a 500 registros.
- O serviço REST do cliente tem um tempo limite de 20 segundos e três tentativas em intervalos de três minutos. Uma nova tentativa bem-sucedida habilita automaticamente o serviço. Após três falhas, o serviço é pausado e repetido a cada três minutos, a menos que seja desprovisionado manualmente.
- Os dados em fila são retidos por até sete dias.

Para implementar o Fluxo de Dados de Atividade de Lead:

1. Exponha um terminal HTTP que pode receber solicitações POST com um corpo JSON da Internet pública. O fluxo de dados de push da atividade envia solicitações para:
1. Forneça o seguinte à Adobe:
   1. Marketo Munchkin ID para sua assinatura
   1. O URL do endpoint na etapa 1
   1. Os tipos de atividade que eles desejam receber (lista completa acima)
   1. Um meio de autenticação, para que o cliente possa verificar se as solicitações são legítimas. Ou:
      1. Uma URL de provedor de identidade, ID do Cliente e Segredo do Cliente para OAuth [Autenticação de Credenciais do Cliente](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Um token de API, que pode ser incluído em solicitações enviadas pela sequência de dados da atividade principal em um cabeçalho http de autorização

O Adobe ativa o fluxo de dados após receber as informações necessárias. O endpoint começa a receber dados.

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

Consulte o [exemplo do consumidor de Fluxo de Dados de Atividade de Cliente Potencial](https://github.com/ihgrant/activity-stream-consumer-example) para obter o código do aplicativo de exemplo.

### Fluxo de dados de auditoria do usuário e fluxo de dados de notificação

Os eventos de Auditoria de usuário são enviados por meio do Adobe I/O. Para consumi-los com uma Adobe ID:

1. Forneça as seguintes informações à Adobe:
   1. Adobe ID
   1. Marketo Munchkin ID para sua assinatura
1. Exponha um terminal REST, normalmente um webhook, para consumir eventos.
1. Depois de receber as informações do endpoint, o Adobe habilita o fluxo para a assinatura.
1. Configure o fluxo no Adobe I/O.
   1. Esta etapa requer uma organização da Adobe
   1. Exige que o usuário da organização Adobe tenha a função de desenvolvedor ou administrador do sistema

Para configurar o Adobe I/O, consulte [Configuração de fluxos de dados de auditoria de usuário do Marketo com o Adobe I/O](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup#).

### Configuração do fluxo de dados de auditoria do usuário no Marketo

O Fluxo de dados de auditoria do usuário está disponível atualmente como parte dos pacotes de desempenho, juntamente com os outros 3 Fluxos de dados. Para obter mais informações sobre os Pacotes, consulte a [Página de Descrição do Produto](https://helpx.adobe.com/br/legal/product-descriptions/adobe-marketo-engage---product-description.html) para obter os limites e recursos do produto.

### Configuração do Adobe I/O

[Consulte Introdução ao Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Para obter instruções básicas para este caso de uso, a partir de [console.adobe.io](https://developer.adobe.com/console):

Quando solicitado, selecione **[!UICONTROL Criar novo projeto]** ou **[!UICONTROL Adicionar evento]**.

### Introdução ao novo projeto

Para começar a usar os serviços da Adobe, adicione uma API, eventos ou tempo de execução, exiba nossa [documentação](https://developer.adobe.com/runtime/docs/).

## Documentação pública

- [Fluxos de dados do Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Introdução aos eventos e webhooks do Adobe IO](https://developer.adobe.com/events/docs/guides/)
- [Blog de fluxos de dados](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
