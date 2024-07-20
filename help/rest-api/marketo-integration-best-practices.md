---
title: Práticas recomendadas de integração do Marketo
feature: REST API
description: Práticas recomendadas para usar APIs do Marketo.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# Práticas recomendadas de integração do Marketo

## Limites da API

- **Cota Diária:** A maioria das assinaturas tem 50.000 chamadas de API alocadas por dia (o que é redefinido diariamente às 12h00 CST). Você pode aumentar sua cota diária por meio do gerente da conta.
- **Limite de Taxa:** Acesso à API por instância limitado a 100 chamadas por 20 segundos.
- **Limite de simultaneidade:**  Máximo de dez chamadas de API simultâneas.
- **Tamanho do Lote:** BD de Cliente Potencial - 300 registros; Consulta de Ativo - 200 registros
- **Tamanho da Carga da API REST:** 1 MB
- **Tamanho do Arquivo de Importação em Massa:** 10MB
- **Tamanho máx. do lote SOAP:** 300 registros
- **Trabalhos de Extração em Massa:** 2 em execução; 10 em fila (inclusive)

## Dicas rápidas

- Suponha que seu aplicativo concorra por recursos de cota, taxa e simultaneidade com outros aplicativos e defina limites de uso conservadores.
- Use os métodos em massa e em lote do Marketo quando disponíveis e apropriados. Use apenas chamadas de registro ou resultado único quando necessário.
- Use o [retrocesso exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) para repetir chamadas de API que falham devido a limites de taxa ou simultaneidade.
- Evite fazer chamadas de API simultâneas se o caso de uso não se beneficiar delas.

## Colocação em lote

Para garantir o melhor desempenho para suas integrações, ao executar inserções ou atualizações, os registros devem ser agrupados no menor número de transações possível. Ao recuperar registros de um armazenamento de dados para envio, os registros devem sempre ser agregados antes do envio, em vez de enviar uma solicitação para cada alteração individual.

## Latência Aceitável

Determinar suas tolerâncias de latência ou o tempo máximo que pode decorrer antes de enviar uma chamada de API informará muitas, se não a maioria, das decisões tomadas ao projetar sua integração com o Marketo. O Marketo fornece vários métodos e opções de configuração diferentes, adequados para casos de uso diferentes e classes de latência diferentes. Por exemplo, uma integração em tempo real para notificar um vendedor de um usuário que está se inscrevendo em uma avaliação só pode enviar lotes de um se for necessário acompanhamento imediato. No entanto, a maioria dos casos não exige isso e pode tolerar latência adicional e pode ser gerenciada com mais eficiência por meio de chamadas de enfileiramento e em lote.

| Latência Aceitável | Métodos preferidos | Observações |
|---|---|---|
| Baixa (&lt;10s) | APIs síncronas (em lote ou sem lote) | Verifique se o caso de uso exige isso. O envio de chamadas imediatas e síncronas para casos de uso de alto volume pode consumir rapidamente uma cota diária de API e potencialmente exceder os limites de taxa e simultaneidade. |
| Medium(10s - 60m) | APIs síncronas (em lote) | Para integrações de dados de entrada com o Marketo, é altamente recomendado usar uma fila com um limite de idade e tamanho. Quando um desses limites for atingido, limpe a fila e envie sua solicitação de API com os registros acumulados. Isso é um forte comprometimento entre velocidade e eficiência, garantindo que suas solicitações ocorram na cadência necessária, enquanto o agrupamento de quantos registros a idade da fila permitir. |
| Alta(>60m) | Importação/exportação em massa (se houver suporte) | Para integrações de dados de entrada, os registros devem ser enfileirados e enviados por meio de APIs em massa do Marketo, sempre que disponíveis. |

## Limites diários

Cada instância habilitada para API do Marketo tem uma alocação diária de pelo menos 10.000 chamadas de API REST por dia, mas geralmente tem 50.000 ou mais e 500 MB ou mais de capacidade de extração em massa. Embora a capacidade diária adicional possa ser adquirida como parte de uma assinatura do Marketo, o design do aplicativo deve considerar os limites comuns de assinaturas do Marketo.

Como a capacidade é compartilhada entre todos os serviços de API e usuários em uma instância, a prática recomendada é eliminar chamadas redundantes e agrupar registros em lote no menor número possível de chamadas. A maneira mais eficiente de chamar para importar registros é usando as APIs de importação em massa da Marketo, que estão disponíveis para [Clientes potenciais/Pessoas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) e [Objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). A Marketo também fornece Extração em massa para [Clientes potenciais](bulk-lead-extract.md) e [Atividades](bulk-activity-extract.md).

### Armazenamento em cache

Os resultados das seguintes operações geralmente podem ser armazenados em cache no lado do cliente por um dia ou mais, pois são alterados com pouca frequência:

- Resultados das operações de Descrever
- [Tipos de atividade](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partições](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

O armazenamento em cache de determinados tipos de ativos, como programas, emails e pastas, também é apropriado para determinados casos de uso, como enriquecimento de dados para registros de lead ou de atividade.

## Limite de taxa

Cada instância do Marketo tem um limite de taxa de 100 chamadas por 20 segundos, que é compartilhado entre todos os serviços de API de terceiros. Se esse limite for excedido, a API responderá com um código de erro 606 indicando que o limite de taxa foi excedido. Em geral, as integrações de terceiros devem limitar sua utilização a 50 chamadas por 20 segundos ou menos para permitir o uso correto dos limites de taxa por várias integrações de API e usuários. Embora possa ser apropriado saturar esse limite em certos casos, em geral, os aplicativos que usam lotes e direcionam seu throughput para menos que esse limite são mais responsivos e consistentes em sua operação, a um pequeno custo de latência aumentada.

## Limite de simultaneidade

Cada instância do Marketo tem um limite compartilhado de dez chamadas de API REST executadas simultaneamente. Assim como a cota diária e os limites de taxa, ela é compartilhada, portanto, você não deve supor que seu aplicativo será o consumidor exclusivo desse limite. O Marketo conta o número de chamadas simultâneas como aquelas que estão processando e que ainda não retornaram; portanto, quando uma chamada é retornada, ela não é mais contada em relação ao limite de chamadas simultâneas.

A maioria dos casos de uso de integração não se beneficia de chamadas simultâneas. Portanto, considere se seu aplicativo se beneficia antes de decidir enviar solicitações simultâneas ao Marketo. Se você quiser implementar a simultaneidade, limite o número de solicitações simultâneas para cinco ou menos no design inicial e aumente isso somente depois de determinar que seu aplicativo requer mais.

## Erros

Exceto por alguns casos raros, as solicitações de API retornam um código de status HTTP 200. Os erros de lógica de negócios também retornam um 200, mas contêm informações detalhadas no corpo da resposta. Consulte [Códigos de erro](error-codes.md) para obter uma explicação detalhada. A frase de motivo HTTP não deve ser avaliada, pois é opcional e está sujeita a alterações.
