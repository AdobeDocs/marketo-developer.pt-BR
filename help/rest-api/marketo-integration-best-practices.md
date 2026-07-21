---
title: Práticas recomendadas de integração do Marketo
feature: REST API
description: Práticas recomendadas para integrações de API do Marketo, abrangendo cotas, limites de taxa e simultaneidade, agrupamento, importação e exportação em massa, armazenamento em cache e planejamento de latência.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
TQID: https://experienceleague.adobe.com/Ld-rmFCwKSx-0W2-ceYICu0FQHK8BKAC1QgqtiOWDn4
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: df401a2a-327d-468c-a5e4-b7b7ccd071a0
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 882
ht-degree: 0%

---

# Práticas recomendadas de integração do Marketo

Projete integrações em torno dos limites da API compartilhada para sua instância do Marketo. Use o agrupamento, o armazenamento em cache e taxas de solicitação conservadoras para melhorar a taxa de transferência e a confiabilidade.

## Limites da API

- **Cota diária:** A maioria das assinaturas tem 50.000 chamadas de API alocadas por dia. A cota é redefinida diariamente às 12h (CST). Entre em contato com o gerente da conta para aumentar a cota diária.
- **Limite de taxa:** cada instância é limitada a 100 chamadas de API por 20 segundos.
- **Limite de simultaneidade:** cada instância permite no máximo dez chamadas de API simultâneas.
- **Tamanho do lote:** o BD de cliente potencial oferece suporte a 300 registros; a Consulta de Ativos oferece suporte a 200 registros.
- **Tamanho da carga da API REST:** 1 MB.
- **Tamanho do arquivo de importação em massa:** 10 MB.
- **Tamanho máximo de lote do SOAP:** 300 registros.
- **Trabalhos de extração em massa:** Dois em execução e dez em fila, inclusive.

## Dicas rápidas

- Defina limites de uso conservadores porque seu aplicativo compartilha recursos de cota, taxa e simultaneidade com outros aplicativos.
- Use os métodos em massa e em lote do Marketo quando disponíveis. Use chamadas de registro único ou resultado único somente quando necessário.
- Use o [retrocesso exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) para repetir chamadas de API que falharam devido a limites de taxa ou simultaneidade.
- Evite chamadas de API simultâneas, a menos que elas beneficiem seu caso de uso.

## Colocação em lote

Para inserções e atualizações, agrupe registros no menor número de transações possível. Ao recuperar registros de um armazenamento de dados, agregue-os antes do envio em vez de enviar uma solicitação para cada alteração.

## Latência Aceitável

Defina a latência aceitável — o tempo máximo antes do envio de uma chamada de API — ao projetar uma integração. Essa opção determina quais métodos e opções de configuração do Marketo se encaixam no caso de uso.

Por exemplo, uma integração em tempo real que notifica um vendedor quando um usuário inicia uma avaliação pode enviar lotes de um quando é necessário acompanhamento imediato. A maioria dos casos de uso tolera mais latência e opera com mais eficiência enfileirando e agrupando chamadas.

| Latência Aceitável | Métodos preferidos | Observações |
| --- | --- | --- |
| Baixa (&lt;10s) | APIs síncronas (em lote ou sem lote) | Verifique se o caso de uso exige isso. O envio de chamadas imediatas e síncronas para casos de uso de alto volume pode consumir rapidamente uma cota diária de API e potencialmente exceder os limites de taxa e simultaneidade. |
| Medium(10s - 60m) | APIs síncronas (em lote) | Para integrações de dados de entrada com o Marketo, é altamente recomendado usar uma fila com um limite de idade e tamanho. Quando um desses limites for atingido, limpe a fila e envie sua solicitação de API com os registros acumulados. Isso é um forte comprometimento entre velocidade e eficiência, garantindo que suas solicitações ocorram na cadência necessária, enquanto o agrupamento de quantos registros a idade da fila permitir. |
| Alta(>60m) | Importação/exportação em massa (se houver suporte) | Para integrações de dados de entrada, os registros devem ser enfileirados e enviados por meio de APIs em massa do Marketo, sempre que disponíveis. |

## Limites diários

Cada instância do Marketo habilitada para API tem uma alocação diária de pelo menos 10.000 chamadas de API REST, embora 50.000 ou mais sejam comuns. Cada instância também tem 500 MB ou mais de capacidade de Extração em massa. A capacidade diária adicional pode ser adquirida como parte de uma assinatura do Marketo, mas os designs de aplicativos devem levar em conta os limites de assinatura comuns.

A capacidade é compartilhada por todos os serviços e usuários da API em uma instância. Elimine chamadas redundantes e registros em lote no menor número possível de chamadas.

O método de importação com maior eficiência de chamada é a API de importação em massa do Marketo, disponível para [Clientes potenciais/Pessoas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) e [Objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Snippets/operation/createSnippetUsingPOST). A Marketo também fornece Extração em massa para [Clientes potenciais](bulk-lead-extract.md) e [Atividades](bulk-activity-extract.md).

### Armazenamento em cache

Os resultados das seguintes operações geralmente podem ser armazenados em cache no lado do cliente por um dia ou mais, pois são alterados com pouca frequência:

- Resultados das operações de Descrever
- [Tipos de atividades](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partições](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadPartitionsUsingGET)

Para casos de uso como lead ou enriquecimento de dados de atividade, você também pode armazenar em cache tipos de ativos como programas, emails e pastas.

## Limite de taxa

Cada instância do Marketo tem um limite de taxa compartilhado de 100 chamadas por 20 segundos em todos os serviços de API de terceiros. Se as chamadas excederem esse limite, a API retornará um código de erro 606.

Em geral, limite cada integração de terceiros a 50 chamadas por 20 segundos ou menos para que várias integrações de API e usuários possam compartilhar a capacidade disponível. Alguns casos de uso podem precisar do limite completo. No entanto, os aplicativos que usam agrupamento e direcionam menor throughput geralmente são mais responsivos e consistentes, com um pequeno aumento na latência.

## Limite de simultaneidade

Cada instância do Marketo tem um limite compartilhado de dez chamadas de API REST executadas simultaneamente. Não presuma que seu aplicativo é o único consumidor desse limite.

O Marketo conta as chamadas que estão sendo processadas e que ainda não foram retornadas. Quando uma chamada é retornada, ela não é mais contada para o limite de simultaneidade.

A maioria das integrações não se beneficia de chamadas simultâneas. Se você implementar a simultaneidade, limite inicialmente o aplicativo a cinco ou menos solicitações simultâneas. Aumente o limite somente depois de determinar que o aplicativo requer mais.

## Erros

Exceto em casos raros, as solicitações de API retornam o código de status HTTP 200. Os erros de lógica de negócios também retornam 200, mas incluem detalhes no corpo da resposta. Consulte [Códigos de erro](error-codes.md) para obter mais informações.

Não avalie a frase de motivo HTTP porque ela é opcional e está sujeita a alterações.
