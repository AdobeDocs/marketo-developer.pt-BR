---
title: Importação em massa
feature: REST API
description: Importação em massa do Marketo para carregar clientes em potencial, objetos personalizados e membros do programa por meio de uploads de várias partes, criação de trabalhos assíncronos, status de pesquisa e falhas de manuseio.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
TQID: https://experienceleague.adobe.com/lr9dyX-fY-oJ2LM5P0zE1m24HtFYKQYYbxMkVe--PkE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 526
ht-degree: 3%

---

# Importação em massa

A Importação em Massa fornece interfaces para inserir grandes conjuntos de dados relacionados a pessoas e pessoas. Você pode importar três tipos de objetos:

- Clientes Potenciais (Pessoas)
- Objetos personalizados
- Membros do programa

Para executar uma importação em massa, crie um trabalho que leia um arquivo carregado. O trabalho é executado de forma assíncrona, portanto, sonde-o para recuperar o status de importação.

Carregar arquivos usando HTTP `multipart/form-data` por RFC 2399.

Ao contrário de outros pontos de extremidade, os pontos de extremidade de API em massa não recebem o prefixo `/rest`.

## Autenticação

As APIs de importação em massa usam o mesmo método de autenticação OAuth 2.0 que as outras APIs REST do Marketo. Envie um token de acesso válido no cabeçalho HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>O suporte para autenticação usando o parâmetro de consulta **access_token** será removido em 30 de junho de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o cabeçalho **Autorização** o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho **Autorização** exclusivamente.

## Limites

- Máximo de trabalhos de importação simultâneos: 2
- Máximo de trabalhos de importação em fila, incluindo os trabalhos que estão sendo importados no momento: 10
- Tamanho máximo do arquivo de importação: 10 MB

## Permissões

A Importação em massa usa o mesmo modelo de permissões que a API REST do Marketo. Ela não requer permissões adicionais, mas cada conjunto de endpoints requer permissões específicas.

## Operações de registro

A importação em massa é uma operação de registro &quot;inserir ou atualizar&quot;. Se o banco de dados contiver um registro correspondente, a operação o atualizará. Caso contrário, a operação cria um registro.

A resposta da importação em massa não indica se um registro individual foi atualizado ou inserido.

## Criação de um trabalho

Crie um trabalho de importação de cliente potencial chamando o ponto de extremidade [Importar Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi#operation/importLeadUsingPOST). Este ponto de extremidade usa [multipart/form-data como o tipo de conteúdo](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html).

Use uma biblioteca de suporte HTTP para seu idioma preferido para criar a solicitação multipart. Você também pode usar o [curl](https://curl.se/) para começar.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Essa solicitação cria um trabalho que importa valores do arquivo CSV chamado `leads.csv`.

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

A resposta retorna um `batchId`. Use esse valor para verificar o status do trabalho.

### Parâmetros comuns

Cada endpoint de criação de trabalho compartilha parâmetros para configurar o arquivo de importação. Um subtipo de importação também pode suportar parâmetros adicionais.

| Parâmetro | Tipo de dados | Observações |
| --- | --- | --- |
| formato | String | Determina o formato de arquivo dos dados importados com opções para valores separados por vírgula, valores separados por tabulação e valores separados por ponto e vírgula. Aceita um dos seguintes: CSV, SSV, TSV. O formato é padronizado como CSV. |
| arquivo | String | Os dados são especificados por meio de dados de formulário de várias partes no arquivo. |

## Status do trabalho de pesquisa

Passe o `batchId` para o ponto de extremidade [Obter Status de Cliente Potencial de Importação](https://developer.adobe.com/marketo-apis/api/mapi#operation/getImportLeadStatusUsingGET) para recuperar o status do trabalho.

```http
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

O membro `status` indica o progresso do trabalho. Seu valor pode ser `Queued`, `Importing`, `Complete` ou `Failed`.

Neste exemplo, o trabalho está concluído, portanto, a pesquisa pode ser interrompida.

## Falhas

O atributo `numOfRowsFailed` na resposta Obter Status de Cliente Potencial de Importação indica o número de linhas com falha. Um valor maior que zero significa que ocorreram falhas.

Para recuperar os registros com falha e suas causas, use o [Obter Falhas de Importação de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi#operation/getImportLeadFailuresUsingGET).

```http
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

O arquivo de falha identifica cada linha com falha e explica por que o registro falhou.
