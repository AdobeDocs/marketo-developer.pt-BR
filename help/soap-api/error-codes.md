---
title: "Códigos de erro"
feature: SOAP
description: "Códigos de erro para chamadas SOAP"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 11%

---


# Códigos de erro

Ao desenvolver para o Marketo, é muito importante que as solicitações e respostas sejam registradas quando uma exceção inesperada for encontrada.  Embora certos tipos de exceções, como autenticação expirada, possam ser tratados com segurança por meio da reautenticação, outras podem exigir interações de suporte, e as solicitações e respostas sempre serão solicitadas nesse cenário.

Veja abaixo uma lista de códigos de erro da API SOAP.

| Código | Mensagem | Observações |
|--- |--- |--- |
| 10001 | Erro interno | Falha grave do sistema |
| 20011 | Erro interno | Falha no serviço de API |
| 20012 | Solicitação não compreendida | Mensagem SOAP inesperada |
| 20013 | Acesso negado | O cliente está bloqueado para acessar a API |
| 20014 | Falha na autenticação | O cliente não forneceu credenciais válidas |
| 20015 | Limite de solicitações excedido | O número de chamadas hoje excedeu a cota da assinatura. A cota de assinatura padrão é de 10.000/dia. |
| 20016 | Solicitação expirada | A assinatura da solicitação é muito antiga. O carimbo de data e hora e a assinatura de solicitação fornecidos estão no passado e não são mais válidos. A solicitação pode ser repetida com um carimbo de data e hora e uma assinatura recém-gerados. |
| 20017 | Solicitação inválida | A solicitação não tem um parâmetro esperado |
| 20019 | Operação incompatível | A operação invocada não está definida no WSDL da API do Marketo |
| 20022 | O intervalo de tempo especificado no filtro de consulta excedeu o limite | O número de dias decorridos entre os campos &quot;olderUpdatedAt&quot; e &quot;latestUpdatedAt&quot; foi maior que 30 |
| 20023 | Limite de taxa excedido | O número de chamadas nos últimos 20 segundos foi maior que 100 |
| 20024 | Limite de simultaneidade excedido | O número de chamadas simultâneas era maior que 10 |
| 20101 | Chave de Cliente Potencial Necessária | LeadKey é Necessário, mas não foi fornecido |
| 20102 | Chave de lead incorreta | LeadKeyType inválido |
| 20103 | Cliente em potencial não encontrado | O valor de LeadKey não correspondeu a nenhum lead |
| 20104 | Detalhe de Cliente Potencial Obrigatório | LeadRecord é necessário, mas não foi fornecido |
| 20105 | Atributo de lead inválido | LeadRecord contém um atributo com um nome incorreto |
| 20106 | Falha na sincronização do lead | Não foi possível atualizar ou criar o LeadRecord |
| 20107 | Chave de atividade incorreta | LeadActivityFilter contém um tipo de atividade incorreto |
| 20108 | Proprietário do Cliente Potencial Não Encontrado | LeadKey especifica um proprietário de lead que não existe |
| 20109 | Parâmetro obrigatório | Valor de parâmetro nulo ou ausente |
| 20110 | Parâmetro incorreto | Um valor de parâmetro é incorreto |
| 20111 | Lista não encontrada | ListKey especifica uma lista que não existe |
| 20113 | Campanha não encontrada | A campanha não existe |
| 20114 | Parâmetro incorreto | O valor do parâmetro é incorreto |
| 20122 | Posição de fluxo incorreta | A posição do fluxo é ruim |
| 20123 | Transmitir no Final | A posição do fluxo indica que não há mais registros disponíveis |
