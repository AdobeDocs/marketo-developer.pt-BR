---
title: Ativos
feature: REST API
description: Uma API para trabalhar com ativos do Marketo.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---

# Ativos

O Marketo fornece APIs para interagir com a maioria dos ativos de marketing e organizacionais no Marketo.

## Ativos

Os ativos da Marketo incluem:

- Pastas
- Programas
- E-mails
- Modelos de e-mail
- Páginas de aterrissagem
- Modelos de páginas
- Bl. conteúdo
- Formulários
- Tokens
- Arquivos

## API

Para obter uma lista completa de pontos de extremidade da API de ativos, incluindo parâmetros e informações de modelagem, consulte a [Referência de ponto de extremidade da API de ativos](endpoint-reference.md).

## Consultar

O Assets normalmente tem três padrões pelos quais eles podem ser recuperados: por id, por nome e pela navegação.  Por ID e por nome recuperará um único ativo para um determinado parâmetro, enquanto a navegação retornará e permitirá a paginação por toda a lista de ativos desse tipo.  Tipos individuais de ativos têm parâmetros variáveis pelos quais podem ser filtrados. Portanto, verifique seus documentos individuais para obter detalhes.

Em certos casos, o endpoint de navegação para alguns tipos de ativos não retornará ativos secundários, como os valores permitidos para uma tag, e eles devem ser recuperados individualmente usando o endpoint Por nome ou Por ID para retornar o conjunto completo de metadados.  Outros podem ter endpoints totalmente separados para recuperar objetos dependentes como Campos de formulário.

### Por ID

```
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

### Por nome

Por motivos técnicos, as APIs de ativos não podem pesquisar nomes de ativos que contenham vírgulas (,).  Recomenda-se que sua convenção de nomenclatura exclua vírgulas para todos os tipos de ativos.

```
GET /rest/asset/v1/file/byName.json?name=My File
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":148,
         "size":270313,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/piKLbhVFvW",
         "folder":{
            "type":"Email",
            "id":10614
         },
         "name":"My File",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Navegar

Navegar pelos ativos sempre permitirá dois parâmetros de consulta:

- offset - Um deslocamento inteiro do qual retornar resultados.
- maxReturn - Limita o número de registros retornados.  O padrão é 20 se não estiver definido e o máximo é 200.

```
GET /rest/asset/v1/emailTemplates.json?offset=10&maxReturn=50
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"33c4#14a1832b4a8",
   "result":[
      {
         "id":18,
         "name":"AAA0unit3CreateTestEmailTemplateName.2314673e-7bc2-47da-a1e8-66dfdd8a1f1d",
         "description":"AssetAPI: getTemplates test",
         "createdAt":"2014-11-03T19:52:58Z+0000",
         "updatedAt":"2014-11-03T19:52:58Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":177,
         "name":"ABfRGutnwN",
         "description":"HMmHkdTRrGaRpPakdgGKICxfMunCEWDUWiThgAbInfaBXxGxSFfjKQIwerngCHRlGTnAJhKPmwlXLcsjGPtWEiILGyeIJTNVHoHg",
         "createdAt":"2014-11-20T19:31:06Z+0000",
         "updatedAt":"2014-11-20T19:31:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":148,
         "name":"ADVHJBQLyw",
         "description":null,
         "createdAt":"2014-11-20T06:42:57Z+0000",
         "updatedAt":"2014-11-20T06:42:57Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

## Criar e atualizar

Para tipos de ativos simples como Pastas, Tokens e Arquivos, normalmente há apenas um único endpoint para criação e, em seguida, um endpoint adicional para atualizar registros por ID.  Os Assets são criados com um nome que é sempre obrigatório e, em seguida, todos os metadados e IDs são retornados pela resposta de criação ou atualização.

Por exemplo, veja como criar um token:

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&value=2015-04-01&type=date&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

Para atualizar uma pasta, faça o seguinte:

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

Outros ativos têm estruturas mais complexas e exigem atualizações em subseções adicionais ou objetos filho e, em seguida, precisam passar por aprovação antes de serem colocados em uso.  Esses tipos de ativos incluem Forms, Emails, Modelos de email, Páginas de aterrissagem e Modelos de página de aterrissagem.  Cada um deles terá um único endpoint para a criação de um registro e, depois, endpoints adicionais para a atualização de metadados, conteúdo e seções de conteúdo.

Por exemplo, para criar uma Landing page, você deverá chamar seu endpoint de criação com uma ID de modelo e recuperar suas seções de conteúdo e atualizar cada uma individualmente para adicionar conteúdo, antes de aprová-lo para que ele possa ser implantado em tempo real.

### Criação complexa

Primeiro, as landing pages exigem a criação de um ativo de landing page usando um modelo principal.  Isso cria uma nova landing page contendo o conteúdo padrão do template para cada seção de conteúdo.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

#### Obter Seções

Para preencher o conteúdo de uma página de aterrissagem, você deve recuperar a lista de seções de conteúdo e executar atualizações individuais para qualquer seção que se desvie do modelo.

```
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

#### Atualizar seção

```
POST /rest/asset/v1/landingPage/{id}/content/{contentId}.json?type=Form&value=1
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#154ea32cf11",
    "result": [
        {
            "id": 174
        }
    ]
}
```

## Aprovação

Muitos tipos de ativos têm um sistema de rascunho e aprovação associado, incluindo emails, landing pages, trechos, Forms e seus modelos correspondentes.  Tentar aprovar um ativo o avaliará em relação a um conjunto específico de regras de validação e, em seguida, o definirá para um estado aprovado ou retornará um motivo de falha.  Para esses tipos de ativos, sempre que uma atualização é feita no conteúdo de um ativo específico, as alterações são feitas em um rascunho do ativo, que não afeta a versão aprovada.  Isso permite que as alterações no conteúdo sejam feitas com segurança sem afetar as versões em tempo real do ativo.  As alterações podem ser aplicadas à versão ao vivo usando o endpoint de aprovação.  Isso também limpa o estado de rascunho do ativo até que qualquer atualização adicional seja aplicada.

```
POST /rest/asset/v1/emailTemplate/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"abe2#14a1832a97d",
   "result":[
      {
         "id":338,
         "name":"lvAVYMZqPS",
         "description":"fZLJQSJRvnYbjGTUpIHHqDOuQgQzXQcWIXoOUPwrVLdMHKcbRqwLoSLkWZTUmaMiCIJSfQiufnnrgITUIqjuAPBLpmliiKuIUFYG",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Approved",
         "workspace":"Default"
      }
   ]
}
```

A aprovação bem-sucedida substitui a versão ao vivo anterior pela versão atualizada.

Descartar rascunhos também está disponível por meio de um endpoint para cada tipo de ativo válido.  Usar isso em um ativo que está em um estado aprovado com rascunho descartará o rascunho atual e todas as alterações pendentes que ele tiver.  Usar isso em um ativo que atualmente não tem uma versão aprovada não fará nada e retornará um erro.  Os ativos somente de rascunho podem ser excluídos, mas não podem ser descartados.

```
POST /rest/asset/v1/emailTemplate/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
         "description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

O Assets também pode ser desaprovado se estiver em um estado somente aprovado.  Isso removerá todas as versões ativas do ativo e retornará o ativo a um estado somente de rascunho, além de descartar qualquer rascunho associado.  Essa ação só poderá ser executada na maioria dos ativos se não estiver em uso em nenhum lugar do Marketo, como um email referido em uma etapa do fluxo Enviar email ou um trecho incorporado em um email.

```
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

## Excluir

O Assets com estados de aprovação e rascunho, exceto de formulários, não pode ser excluído enquanto for aprovado e deve ser cancelado antes da exclusão.  As exclusões geralmente só podem ser realizadas quando um ativo não é aprovado e está fora de uso e, no caso de pastas, está vazio de ativos.  Uma exceção notável são programas, que podem ser excluídos junto com todo o seu conteúdo secundário, desde que o programa e seu conteúdo não estejam em uso em nenhum lugar fora dos limites do programa.

```
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```

## Tempos limite

As APIs de ativo têm um tempo limite de 300s
