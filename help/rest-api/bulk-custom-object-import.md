---
title: "Importação de Objeto Personalizado em Massa"
feature: Custom Objects
description: "Importação em lote de objetos personalizados."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 0%

---


# Importação de Objeto Personalizado em Massa

[Referência de Ponto de Extremidade de Importação de Objeto Personalizado em Massa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Quando você tem muitos registros de objeto personalizados para importar, é uma prática recomendada importá-los de forma assíncrona usando a API em massa. Isso é feito importando um arquivo simples que contém registros delimitados (vírgula, tabulação ou ponto e vírgula). O arquivo pode conter qualquer número de registros, desde que seu tamanho seja inferior a 10 MB (caso contrário, um código de status HTTP 413 será retornado). O conteúdo do arquivo depende da definição do objeto personalizado. A primeira linha sempre contém um cabeçalho que lista os campos para os quais mapear valores de cada linha. Todos os nomes de campos no cabeçalho devem corresponder a um nome de API (conforme discutido abaixo). As linhas restantes contêm os dados a serem importados, um registro por linha. A operação de registro é somente &quot;inserir ou atualizar&quot;.

## Limites de processamento

Você pode enviar mais de uma solicitação de importação em massa, dentro dos limites. Cada solicitação é adicionada como um trabalho a uma fila FIFO para ser processada. No máximo dois trabalhos são processados ao mesmo tempo. São permitidos no máximo dez trabalhos na fila em um determinado momento (incluindo os 2 que estão sendo processados no momento). Se você exceder o máximo de dez tarefas, um erro &quot;1016, Muitas importações&quot; será retornado.

## Exemplo de objeto personalizado

Antes de usar a API em massa, é necessário usar a interface do administrador do Marketo para [criar seu objeto personalizado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Por exemplo, suponha que criamos um objeto personalizado &quot;Carro&quot; com campos &quot;Cor&quot;, &quot;Criar&quot;, &quot;Modelo&quot; e &quot;VIN&quot;. Abaixo estão as telas da interface do administrador mostrando o objeto personalizado. Você pode ver que usamos o campo VIN para desduplicação. Os nomes das APIs são destacados porque devem ser usados ao chamar pontos de extremidade relacionados à API em massa.

![Inserir objeto personalizado](assets/bulk-insert-co-car-1.png)

Estes são os campos de objeto personalizados, conforme apresentados na interface do usuário do administrador.

![Inserir campos de objeto personalizado](assets/bulk-insert-co-car-fields.png)

### Nomes da API

Você pode recuperar nomes de API de forma programática, transmitindo o nome da API do objeto personalizado para o [Descrever objeto personalizado](#describe) terminal.

```
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2017-02-22T19:55:51Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                }
            ]
        }
    ],
    "success": true
}
```

### Importar arquivo

Agora suponha que você deseja importar três registros de objeto personalizado &quot;Carro&quot;. Usando o formato delimitado por vírgula (CSV), o arquivo pode ter esta aparência:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

A linha 1 é o cabeçalho e as linhas 2 a 4 são os registros de dados do objeto personalizado.

## Criação de um trabalho

Para fazer a solicitação de importação em massa, você deve incluir o nome da API do objeto personalizado no caminho para o [Importar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST) terminal. Você também deve incluir um parâmetro &quot;file&quot; que faça referência ao nome do arquivo de importação e um parâmetro &quot;format&quot; que especifique como o arquivo de importação é delimitado (&quot;csv&quot;, &quot;tsv&quot; ou &quot;ssv&quot;).

```
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Disposition: form-data; name="file"; filename="custom_object_import.csv"
Content-Type: text/csv

color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo--
```

```json
{
    "requestId": "c015#15a68a23418",
    "result": [
        {
            "batchId": 1013,
            "status": "Queued",
            "objectApiName": "car_c"
        }
    ],
    "success": true
}
```

Neste exemplo, especificamos o formato &quot;csv&quot; e nomeamos nosso arquivo de importação como &quot;custom_object_import.csv&quot;.

Observe que, na resposta à nossa chamada, não há nenhuma lista de sucessos ou falhas como você obteria do endpoint Sincronizar objetos personalizados. Em vez disso, você receberá um `batchId`. Isso ocorre porque a chamada do é assíncrona e pode retornar um `status` de &quot;Em fila&quot;, &quot;Importando&quot; ou &quot;Falha&quot;. Você deve reter o batchId para obter o status do trabalho de importação ou recuperar falhas e/ou avisos após a conclusão. O batchId permanece válido por sete dias.

Uma maneira simples de replicar a solicitação de importação em massa é usar o curl da linha de comando:

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Onde o arquivo de importação &quot;custom_object_import.csv&quot; contém o seguinte:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Status do trabalho de pesquisa

Após criar o trabalho de importação, você deve consultar seu status. É prática recomendada pesquisar o trabalho de importação a cada 5-30 segundos. Faça isso transmitindo o nome da API do objeto personalizado e o `batchId` no caminho para o [Obter Status do Objeto Personalizado de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) terminal.

```
GET /bulk/v1/customobjects/{apiName}/import/{batchId}/status.json
```

```json
{
    "requestId": "2a5#15a68dd9be1",
    "result": [
        {
            "batchId": 1013,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "importTime": "2 second(s)",
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Essa resposta mostra uma importação concluída, mas a variável `status` pode ser um dos seguintes: Concluído, Em fila, Importando, Falha. Se o trabalho tiver sido concluído, você terá uma lista do número de linhas processadas, com falhas e com avisos. O atributo de mensagem também é um bom local para procurar informações adicionais sobre a tarefa.

## Falhas

As falhas são indicadas pela variável `numOfRowsFailed` atributo em [Obter Status do Objeto Personalizado de Importação](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) resposta. Se numOfRowsFailed for maior que zero, esse valor indicará o número de falhas que ocorreram. Chame [Obter Falhas de Importação de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) para obter um arquivo com os detalhes da falha. Novamente, você deve passar o nome da API do objeto personalizado e `batchId` no caminho. Se nenhum arquivo de falha existir, um código de status HTTP 404 será retornado.

Continuando com o exemplo, podemos forçar uma falha modificando o cabeçalho e alterando &quot;vin&quot; para &quot; vin&quot; (adicionando um espaço entre a vírgula e &quot;vin&quot;).

```
color,make,model, vin
```

Quando reimportamos e verificamos o status, vemos essa resposta com `numRowsFailed`: 3. Isso indica três falhas.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/status.json
```

```json
{
    "requestId": "12260#15a68f491ed",
    "result": [
        {
            "batchId": 1016,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 0,
            "numOfRowsFailed": 3,
            "numOfRowsWithWarning": 0,
            "importTime": "1 second(s)",
            "message": "Import completed with errors, 0 records imported (0 members), 3 failed"
        }
    ],
    "success": true
}
```

Agora fazemos a chamada de endpoint Obter Falha de Importação de Objeto Personalizado para obter detalhes adicionais de falha:

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

E podemos ver que estamos perdendo nosso campo de desduplicação `vin`.

## Avisos

Os avisos são indicados pela variável `numOfRowsWithWarning` atributo na resposta Obter Status do Objeto Personalizado de Importação. Se numOfRowsWithWarning for maior que zero, esse valor indicará o número de avisos que ocorreram. Chame [Obter Avisos de Importação de Objeto Personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) para obter um arquivo com detalhes de aviso. Novamente, você deve passar o nome da API do objeto personalizado e `batchId` no caminho. Se nenhum arquivo de aviso existir, um código de status HTTP 404 será retornado.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
