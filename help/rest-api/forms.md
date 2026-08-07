---
title: Formulários
feature: REST API, Forms
description: Guia da API REST do Marketo Forms para criar e gerenciar formulários, recuperar por id ou nome, navegar com filtros de status e gerenciar campos, conjuntos de campos e regras.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
TQID: https://experienceleague.adobe.com/56tc1a14d8okxweS7TK7SzfGB8G03WAI2KBlFKQbSdM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: d65b4a73-87a3-4d56-b638-74e74d9939ceid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2: id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 1447
ht-degree: 3%

---

# Formulários

[Referência de endpoint do Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms)

[Referência do ponto final dos campos de formulário](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields)

Use os endpoints de formulários para gerenciar formulários de sistemas remotos. Um formulário pode incluir vários tipos de objeto:

- Formulários
- Campos
- Conjuntos de campos
- Regras de visibilidade
- Regras da página de acompanhamento

## Consultar

A Forms oferece suporte aos métodos de recuperação de ativos padrão: [por id](https://developer.adobe.com/marketo-apis/api/asset#operation/getLpFormByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getLpFormByNameUsingGET) e por [navegação](https://developer.adobe.com/marketo-apis/api/asset#operation/browseForms2UsingGET). Uma resposta de formulário contém todas as propriedades de formulário, exceto a lista de campos.

### Por ID

Passar um formulário `id` como parâmetro de caminho para [Obter Formulário por Id](https://developer.adobe.com/marketo-apis/api/asset#operation/getLpFormByIdUsingGET). O ponto de extremidade retorna o registro de formulário correspondente.

```http
GET /rest/asset/v1/form/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Por nome

Passar um formulário `name` para [Obter Formulário por Nome](https://developer.adobe.com/marketo-apis/api/asset#operation/getLpFormByNameUsingGET). O ponto de extremidade retorna o registro de formulário correspondente.

```http
GET /rest/asset/v1/form/byName.json?name=newForm
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Procurar

[Obter Forms](https://developer.adobe.com/marketo-apis/api/asset#operation/browseForms2UsingGET) segue o padrão de navegação da API de ativos. Ele é compatível com estes filtros opcionais:

- `status`: Filtros por `approved`, `approved with draft`, ou `draft`.
- `maxReturn`: Limita o número de registros retornados.
- `offset`: Páginas no conjunto de resultados.

```http
GET /rest/asset/v1/forms.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "645d#154e3d499ac",
    "result": [
        {
            "id": 227,
            "name": "aKAUVDfbsX",
            "description": "",
            "createdAt": "2016-05-18T20:36:20Z+0000",
            "updatedAt": "2016-05-18T20:36:20Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO227B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        },
        {
            "id": 695,
            "name": "AoMXgfFbma",
            "description": "",
            "createdAt": "2016-05-19T18:50:40Z+0000",
            "updatedAt": "2016-05-19T18:50:40Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO695B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 565,
                "folderName": "WfUvYmlcyT"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

### Lista de campos

Recupere a lista de campos separadamente para cada formulário transmitindo a ID do formulário.

```http
GET /rest/asset/v1/form/{id}/fields.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2165#154eee00d01",
    "result": [
        {
            "id": "FirstName",
            "label": "First Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "LastName",
            "label": "Last Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 1,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Email",
            "label": "Email Address:",
            "dataType": "email",
            "validationMessage": "Must be valid email. <span class='mktoErrorDetail'>example@yourdomain.com</span>",
            "rowNumber": 2,
            "columnNumber": 0,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Profiling",
            "dataType": "profiling",
            "rowNumber": 3,
            "columnNumber": 0
        }
    ]
}
```

Antes de atualizar ou excluir campos ou alterar seu comportamento, recupere a lista de campos do formulário. Use a ID de campo retornada em solicitações subsequentes.

### Tipos de campos

| Tipo de interface | Nome da API |
| --- | --- |
| Caixas de seleção | caixa de seleção |
| Botão de opção | rádio |
| Área de texto | textarea |
| Lista de seleção | lista de opções |
| String | string |
| Email | email |
| Data | data |
| Número | número |
| Duplo | duplo |
| Telefone | telefone |
| URL | url |
| Moeda | currency |
| Caixa de seleção | single_checkbox |
| Barra deslizante | intervalo |

### Dependências

Passar um formulário `id` como parâmetro de caminho para [Obter Formulário Usado por](https://developer.adobe.com/marketo-apis/api/asset#operation/getFormUsedByUsingGET). O endpoint retorna ativos que dependem do formulário.

Os seguintes tipos de ativos podem usar formulários:

- Páginas de destino
- Listas inteligentes
- Campanhas inteligentes
- Relatórios
- Programas de email

```http
GET /rest/asset/v1/form/{id}/usedBy.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fdf4#17285b25038",
    "warnings": [],
    "result": [
        {
            "id": 1038,
            "name": "LP Redirect Rules Program.LP Test 01",
            "type": "Landing Page",
            "status": "approved",
            "updatedAt": "2020-02-23T01:31:21Z+0000"
        }
    ]
}
```

## Criar e atualizar

Para [criar um formulário](https://developer.adobe.com/marketo-apis/api/asset#operation/createLpFormsUsingPOST), forneça dois campos obrigatórios:

- A pasta principal do formulário.
- O nome do formulário.

Todos os outros parâmetros são opcionais e têm valores padrão. Um novo formulário inclui três campos padrão: Nome, Sobrenome e Email.

```http
POST /rest/asset/v1/forms.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=newForm&description=test&folder={"type": "Folder","id": 293}&language=French
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

Para [atualizar um formulário](https://developer.adobe.com/marketo-apis/api/asset#operation/updateFormsUsingPOST), passe sua ID. Durante a criação ou atualização, é possível definir os parâmetros de estilo base que controlam como o formulário é exibido para o usuário.

```http
POST /rest/asset/v1/form/736.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=updated name&description=This is a test for updateapi&language=English&progressiveProfiling=true&locale=en_US
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154e3cf6efe",
    "result": [
        {
            "id": 736,
            "name": "updated name",
            "description": "This is a test for update api",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:28:23Z+0000",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

Os endpoints de criar e atualizar formulário não modificam o comportamento conhecido do visitante ou da página de agradecimento. Use os endpoints correspondentes para gerenciar esses comportamentos.

## Metadados de campo

Antes de adicionar ou editar campos de formulário, recupere os campos válidos para a instância de destino. As operações de campo usam a propriedade `id` retornada para cada campo.

Para campos de cliente potencial, use o ponto de extremidade [Obter Campos de Formulário Disponíveis](https://developer.adobe.com/marketo-apis/api/asset#operation/getAllFieldsUsingGET). A resposta inclui o tipo de dados de cada campo e os metadados padrão aplicados quando o campo é adicionado a um formulário.

```http
GET /rest/asset/v1/form/fields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "176ca#167a9808f4c",
    "warnings": [],
    "result": [
        {
            "id": "AnnualRevenue",
            "isRequired": false,
            "dataType": "currency"
        },
        {
            "id": "City",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Company",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Country",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Description",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 32000,
            "visibleRows": 2
        },
        {
            "id": "Email",
            "isRequired": false,
            "dataType": "email"
        },
        {
            "id": "Fax",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "FirstName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Industry",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LastName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LeadSource",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "MobilePhone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "NumberOfEmployees",
            "isRequired": false,
            "dataType": "int"
        },
        {
            "id": "Phone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "PostalCode",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Rating",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Salutation",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "Mr.,Ms.,Mrs.,Dr.,Prof."
        },
        {
            "id": "State",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "AK::AK,AL::AL,AR::AR,AZ::AZ,CA::CA,CO::CO,CT::CT,DE::DE,FL::FL,GA::GA,HI::HI,IA::IA,ID::ID,IL::IL,IN::IN,KS::KS,KY::KY,LA::LA,MA::MA,MD::MD,ME::ME,MI::MI,MN::MN,MO::MO,MS::MS,MT::MT,NC::NC,ND::ND,NE::NE,NH::NH,NJ::NJ,NM::NM,NV::NV,NY::NY,OH::OH,OK::OK,OR::OR,PA::PA,RI::RI,SC::SC,SD::SD,TN::TN,TX::TX,UT::UT,VA::VA,VT::VT,WA::WA,WI::WI,WV::WV,WY::WY"
        },
        {
            "id": "Street",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 2000,
            "visibleRows": 2
        },
        {
            "id": "Title",
            "isRequired": false,
            "dataType": "picklist"
        }
    ]
}
```

Para campos personalizados de Membros do Programa, chame o ponto de extremidade [Obter Campos de Membros do Programa de Formulário Disponíveis](https://developer.adobe.com/marketo-apis/api/asset#operation/getAllProgramMemberFieldsUsingGET). A resposta inclui tipos de dados de campo personalizado e metadados padrão do Membro do programa.

Para usar esses campos, o formulário deve estar em um Programa, não no Design Studio. Uma landing page que contenha um formulário com esses campos também deve estar em um programa. Ele não pode estar no ou clonado no Design Studio.

```http
GET /rest/asset/v1/form/programMemberFields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "109c6#16fa0b9c51a",
    "warnings": [],
    "result": [
        {
            "id": "pMCFCustomField01",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "pMCFCustomField02",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "myPMCF",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        }
    ]
}
```

### Editar campo

Cada formulário tem uma lista editável de campos exibidos para o usuário quando o formulário é carregado. Use o endpoint correspondente para adicionar, atualizar ou excluir um campo de cada vez.

Para [adicionar um campo](https://developer.adobe.com/marketo-apis/api/asset#operation/addFieldToAFormUsingPOST), forneça a ID do formulário pai e o campo `fieldId`. Todas as outras propriedades estão vazias ou usam padrões com base no tipo de dados e nos metadados do campo.

Enviar os dados como uma POST com `application/x-www-form-urlencoded`, não como JSON.

```http
POST /rest/asset/v1/form/{id}/fields.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
fieldId=NumberOfEmployees&maxLength=125&defaultValue=this is default&required=true&fieldWidth=100&validationMessage=hey, you there?&label=employee count&hintText=Hint me&minValue=10
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1826e#154f41b214c",
    "result": [
        {
            "id": "NumberOfEmployees",
            "label": "employee count",
            "fieldWidth": 100,
            "dataType": "number",
            "defaultValue": "this is default",
            "validationMessage": "hey, you there?",
            "rowNumber": 5,
            "columnNumber": 0,
            "required": true,
            "formPrefill": true,
            "fieldMetaData": {
                "minValue": 10,
                "maxValue": null
            },
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "hintText": "Hint me"
        }
    ]
}
```

Uma atualização pode editar as mesmas propriedades usadas ao adicionar um campo. Ela também requer a ID do formulário e `fieldId`, mas o ponto de extremidade de atualização passa `fieldId` como um parâmetro de caminho em vez de um parâmetro de consulta.

```http
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
label=enter the last name here
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5634#15508303abb",
    "result": [
        {
            "id": "LastName",
            "label": "enter the last name here",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        }
    ]
}
```

O exemplo anterior atualiza `LastName`, que é um campo de cadeia de caracteres simples. Outros campos de formulário têm metadados mais complexos. Por exemplo, `Salutation` é um campo `select` com uma lista de itens e um valor padrão.

Ao adicionar ou atualizar um campo de seleção, defina o valor `isDefault` de uma escolha como `true`. Caso contrário, a primeira opção não terá valor e será rotulada `Select...`.

![Saudação](assets/form-field-salutation.png)

Para atualizar os itens da lista, formate o parâmetro `values` como mostrado no exemplo a seguir:

```http
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
values=[{"label":"Select...","value":"","isDefault":true,"selected":true}, {"label":"MR","value":"MR"}, {"label":"MS","value":"MS"}, {"label":"MRS","value":"MRS"}, {"label":"DR","value":"DR"}, {"label":"PROF","value":"PROF"}]
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "71fd#1588d9d1b0c",
  "result": [
    {
      "id": "Salutation",
      "label": "Salutation:",
      "dataType": "select",
      "validationMessage": "This field is required.",
      "rowNumber": 3,
      "columnNumber": 0,
      "required": false,
      "formPrefill": true,
      "fieldMetaData": {
        "multiSelect": false,
        "values": [
          {
            "label": "Select...",
            "value": "",
            "isDefault": true,
            "selected": true
          },
          {
            "label": "MR",
            "value": "MR"
          },
          {
            "label": "MS",
            "value": "MS"
          },
          {
            "label": "MRS",
            "value": "MRS"
          },
          {
            "label": "DR",
            "value": "DR"
          },
          {
            "label": "PROF",
            "value": "PROF"
          }
        ],
        "visibleLines": 1
      },
      "visibilityRules": {
        "ruleType": "alwaysShow"
      }
    }
  ]
}
```

Use Adicionar campo à resposta do formulário para determinar como formatar um campo de formulário complexo.

### Reorganização do campo

Use o ponto de extremidade [Alterar Posições de Campos de Formulário](https://developer.adobe.com/marketo-apis/api/asset#operation/updateFieldPositionsUsingPOST) para reorganizar todos os campos de formulário como uma única unidade. O ponto de extremidade requer `positions`, uma matriz JSON de objetos com três membros:

- `columnNumber`
- `rowNumber`
- `fieldName`, que se refere à ID do campo

Os campos de formulário usam uma organização semelhante a tabela com até três colunas e 10 linhas. Os índices de linha e coluna começam em 0, portanto, a primeira linha e a primeira coluna usam 0. Cada campo deve ocupar uma posição única.

Se o campo de destino for um conjunto de campos, seu registro em `positions` também deverá conter `fieldList`. Este parâmetro é uma matriz de objetos com os mesmos membros `columnNumber`, `rowNumber` e `fieldName`.

A lista principal trata o conjunto de campos como um campo. As posições em `fieldList` determinam a organização de seus campos filho.

```http
POST /rest/asset/v1/form/{id}/reArrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"FirstName"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"}, {"columnNumber":0,"rowNumber":2, "fieldName":"Email"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bb18#15508ef9c04",
    "result": [
        {
            "id": 764
        }
    ]
}
```

### Rich text

Use um [ponto de extremidade separado](https://developer.adobe.com/marketo-apis/api/asset#operation/addRichTextFieldUsingPOST) para adicionar campos de rich text. Transmita o conteúdo como HTML em uma solicitação `multipart/form-data`. A HTML não deve conter scripts, metatags ou tags de link.

```http
POST /rest/asset/v1/form/{id}/richText.json
```

```html
Content-Type: multipart/form-data; boundary=---------------------------9051914041544843365972754266
-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="text"
Content-Type: text/html
<div>Fancy Rich Text Component</div>
-----------------------------9051914041544843365972754266--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "82c8#154f423bf5c",
    "result": [
        {
            "id": "SHRtbFRleHRfMjAxNi0wNS0yN1QxNDozNDoyNC4xMTVa",
            "labelWidth": 260,
            "dataType": "htmltext",
            "rowNumber": 8,
            "columnNumber": 0,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "text": "<div>Fancy Rich Text Component</div>"
        }
    ]
}
```

### Conjunto de campos

Um conjunto de campos é um grupo opcional de campos. A lista de campos de nível superior trata um conjunto de campos como um campo para regras de posicionamento e visibilidade. Por exemplo, selecionar sim para um campo Requisitos de conformidade pode revelar um conjunto de campos que contém campos de conformidade com HIPAA e PCI.

Um campo deve ser exclusivo dentro do formulário. O mesmo campo não pode aparecer na lista de campos pai do formulário e em um conjunto de campos filho.

Adicione um fieldset com o [Adicionar Fieldset ao ponto de extremidade do Formulário](https://developer.adobe.com/marketo-apis/api/asset#operation/addFieldSetUsingPOST). O conjunto de campos aparece na resposta [Obter Campos para Formulário](https://developer.adobe.com/marketo-apis/api/asset#operation/getFormFieldByFormVidUsingGET). Para adicionar campos ao conjunto de campos, use [Atualizar Posições de Campo](https://developer.adobe.com/marketo-apis/api/asset#operation/updateFieldPositionsUsingPOST) para movê-las para `fieldList`.

Para esses endpoints, envie os dados como uma POST com `application/x-www-form-urlencoded`, não como JSON.

## Regra de visibilidade

As regras de visibilidade determinam se um visitante pode ver um campo com base nos valores inseridos no formulário. Cada regra compara o valor de um `subjectField` no formulário com uma lista de valores na regra.

Um campo pode ter um tipo de regra de visibilidade: `show`, `hide` ou `alwaysShow`. A API avalia as regras do campo de cima para baixo e aplica a primeira regra que é avaliada como verdadeira.

A alteração das regras de visibilidade é uma atualização destrutiva.

```http
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
visibilityRule={"ruleType":"show", "rules":[{"subjectField": "LastName", "operator": "isNotEmpty", "values": [], "altLabel": "Email:"}]}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "ab4a#15509030601",
    "result": [
        {
            "formFieldId": "Email",
            "ruleType": "show",
            "rules": [
                {
                    "subjectField": "LastName",
                    "operator": "isNotEmpty",
                    "values": [],
                    "altLabel": "Email:"
                }
            ]
        }
    ]
}
```

Para obter a lista completa de operadores, consulte [Adicionar regras de visibilidade do campo de formulário](https://developer.adobe.com/marketo-apis/api/asset#operation/addFormFieldVisibilityRuleUsingPOST).

## Acompanhamento

As regras de acompanhamento dinâmicas podem redirecionar os visitantes para uma página ou mantê-los na página atual com base nos valores de campo designados no envio. As regras de Página de agradecimento e de Página de acompanhamento se referem ao mesmo comportamento.

Representa as regras como uma matriz JSON cujos registros contêm `followupType`, `followupValue`, `operator`, `subjectField`, `values` e `default`. Somente um registro na matriz pode ter o booleano `default` definido como `true`. O formulário usa esse registro quando um visitante não se qualifica para outra regra.

O valor `followupType` pode ser `lp` ou `url`. O valor `lp` indica que `followupValue` é uma ID de página de aterrissagem da Marketo. O valor `url` indica que `followupValue` é a URL de outra página. O operador compara o valor do campo de assunto com os valores fornecidos.

## Botão Enviar

Use o ponto de extremidade [Atualizar Botão Enviar](https://developer.adobe.com/marketo-apis/api/asset#operation/updateFormSubmitButtonUsingPOST) para modificar o estilo do botão enviar. Você pode atualizar `buttonPosition`, `buttonStyle`, `label` e `waitingLabel`. O `waitingLabel` aparece enquanto o envio está pendente.

Esta é uma atualização destrutiva.

## Aprovação

O Forms segue um ciclo de vida de rascunho aprovado. Um formulário pode ter uma versão de rascunho, uma versão aprovada ou ambas. As atualizações sempre se aplicam ao rascunho e ficam online somente após a aprovação.

A aprovação de um formulário substitui a versão aprovada existente, se houver, pelo rascunho atual. Cancelar a aprovação de um formulário em tempo real exclui os rascunhos atuais e rebaixa a versão aprovada para um estado somente de rascunho. Sempre cancele a aprovação de um formulário antes de tentar excluí-lo.

## Criação de perfil progressiva

Quando a criação progressiva de perfil está habilitada, a lista de campos de formulário inclui um conjunto de campos chamado `Profiling`. Use o endpoint Atualizar posições de campo para adicionar ou remover campos da lista de criação de perfil progressiva.

Esse endpoint executa atualizações destrutivas, portanto, cada solicitação deve incluir todos os campos no formulário. O exemplo a seguir adiciona `Phone` à lista de criação de perfil progressiva.

```http
POST /rest/asset/v1/form/{id}/reArrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"Email"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"},{"columnNumber":0,"rowNumber":2,"fieldName":"Company"},{"columnNumber":0,"rowNumber":3,"fieldName":"Website"},{"columnNumber":0,"rowNumber":4,"fieldName":"Profiling","fieldList":[{"columnNumber":0,"rowNumber":0,"fieldName":"Phone"}]}]
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d6a#164190dbdf2",
    "result": [
        {
            "id": 1031
        }
    ]
}
```
