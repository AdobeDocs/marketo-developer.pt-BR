---
title: Importação em massa
feature: REST API
description: Importação em lote de dados da pessoa.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
source-git-commit: e7d893a81d3ed95e34eefac1ee8f1ddd6852f5cc
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

---

# Importação em massa

O Marketo fornece interfaces para a inserção de grandes conjuntos de dados relacionados a pessoas e pessoas, chamados de Importação em massa. Atualmente, as interfaces são oferecidas para três tipos de objeto:

- Clientes Potenciais (Pessoas)
- Objetos personalizados
- Membros do programa

A importação em massa é executada criando um trabalho e aguardando a conclusão do trabalho ao ler um arquivo. Esses trabalhos são executados de forma assíncrona e podem ser consultados para recuperar o status da importação. Os arquivos são carregados usando HTTP multipart/form-data conforme RFC 2399.

Os pontos de extremidade da API em massa não recebem o prefixo &#39;/rest&#39; como outros pontos de extremidade.

## Autenticação

As APIs de importação em massa usam o mesmo método de autenticação OAuth 2.0 que as outras APIs REST do Marketo.  Isso requer um token de acesso válido enviado como um cabeçalho HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 30 de junho de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

## Limites

- Máximo de Trabalhos de Importação Simultâneos: 2
- Máximo de Trabalhos de Importação em Fila (incluindo os trabalhos que estão sendo importados no momento): 10
- Tamanho máximo do arquivo de importação: 10 MB

## Permissões

A Importação em massa usa o mesmo modelo de permissões que a API REST do Marketo e não requer permissões especiais adicionais para ser usada, embora permissões específicas sejam necessárias para cada conjunto de endpoints.

## Operações de registro

A importação em massa é uma operação de registro &quot;inserir ou atualizar&quot;. Se um registro correspondente for encontrado no banco de dados, ele será atualizado. Caso contrário, um novo registro será criado. A resposta da importação em massa não indica se um determinado registro foi atualizado ou inserido.

## Criação de um trabalho

As APIs de importação em massa do Marketo usam o conceito de um trabalho para executar a importação de dados. Vamos analisar a criação de um trabalho de importação de clientes potenciais simples usando o ponto de extremidade [Importar clientes potenciais](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST).  Observe que este ponto de extremidade usa [multipart/form-data como o tipo de conteúdo](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Isso pode ser difícil de corrigir, portanto, a prática recomendada é usar uma biblioteca de suporte HTTP para o idioma de sua escolha.  Se você está apenas molhando os pés, sugerimos que use [curl](https://curl.se/).

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Esta solicitação construirá um trabalho que importará valores contidos no arquivo CSV chamado &quot;leads.csv&quot; com os cabeçalhos de coluna &quot;FirstName&quot;, &quot;LastName&quot;, &quot;Email&quot; e &quot;Company&quot;.

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Quando enviarmos o trabalho, ele retornará um batchId, que podemos usar para verificar seu status.

### Parâmetros comuns

Cada endpoint de criação de trabalho compartilha alguns parâmetros comuns para configurar o formato de arquivo, nomes de campo e filtro de um trabalho de extração em massa.  Cada subtipo de trabalho de extração pode ter parâmetros adicionais:

| Parâmetro | Tipo de dados | Observações |
|---|---|---|
| formato | String | Determina o formato de arquivo dos dados importados com opções para valores separados por vírgula, valores separados por tabulação e valores separados por ponto e vírgula. Aceita um dos seguintes: CSV, SSV, TSV. O formato é padronizado como CSV. |
| arquivo | String | Os dados são especificados por meio de dados de formulário de várias partes no arquivo. |


## Status do trabalho de pesquisa

É simples determinar o status do trabalho usando o ponto de extremidade [Obter Status de Cliente Potencial de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}.json
```

```json
{
    "requestId": "1f63#15d6738fd15",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Complete",
            "numOfLeadsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

O membro interno `status` indicará o progresso do trabalho e poderá ser um dos seguintes valores: Queued, Importing, Complete, Failed. Nesse caso, nosso trabalho foi concluído, então podemos parar a pesquisa.

## Falhas

As falhas são indicadas pelo atributo `numOfRowsFailed` na resposta Obter Status de Lead de Importação. Se `numOfRowsFailed` for maior que zero, esse valor indicará o número de falhas que ocorreram.

Para recuperar os registros e as causas de linhas com falha, você deverá recuperar o arquivo de falha usando o ponto de extremidade [Obter Falhas de Importação de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

O arquivo indica quais linhas falharam, juntamente com uma mensagem indicando por que o registro falhou.
