---
title: Formulários
feature: REST API, Forms
description: Crie e gerencie formulários por meio da API.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 2%

---

# Formulários

[Referência de Ponto de Extremidade do Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Referência de Ponto de Extremidade de Campos de Formulário](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

O Marketo Forms tem um conjunto complexo de endpoints que permite o controle total do gerenciamento de formulários a partir de sistemas remotos. A estrutura dos formulários pode ser complexa, pois há vários tipos diferentes de objetos que devem ser gerenciados como parte de um formulário: Forms, Campos, Conjuntos de campos, Regras de visibilidade e Regras de página de acompanhamento.

## Consultar

A Forms oferece suporte aos métodos padrão de recuperação de ativos, [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [por nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) e [por navegação](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Cada resposta de formulário contém todas as suas propriedades, exceto a lista de campos.

### Por ID

[Obter Formulário por Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) toma o formulário `id` como parâmetro de caminho e retorna um registro de formulário.

```
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

[Obter Formulário por Nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) toma um formulário `name` como parâmetro de caminho e retorna um registro de formulário.

```
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

### Navegar

[Obter formulários do Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) funciona como outros pontos de extremidade de navegação da API de ativos e permite a filtragem opcional em `status`, `maxReturn` e `offset`. O status pode ser: aprovado, aprovado com rascunho ou rascunho.

```
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

A recuperação da lista de campos para um formulário é feita por formulário.

```
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

Ao editar campos ou seu comportamento dentro de um formulário, a lista de campos deve sempre ser recuperada antes de tentar editar. Isso garante que você forneça a ID de campo apropriada ao atualizar ou excluir.

### Tipos de campos

| Tipo de interface | Nome da API |
|--------------|-----------------|
| Caixas de seleção | caixa de seleção |
| Botão de opção | rádio |
| Área de texto | textarea |
| Lista de seleção | lista de opções |
| Sequência de caracteres | string |
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

O ponto de extremidade [Obter Formulário Usado por](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) pega o formulário `id` como parâmetro de caminho e retorna a lista de ativos que dependem do formulário. O Forms pode ser usado pelos seguintes tipos de ativos: Páginas de aterrissagem, Smart Lists, Campanhas inteligentes, Relatórios, Programas de email.

```
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

Ao [criar um formulário](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST), há apenas dois campos obrigatórios: a pasta pai do formulário, o nome do formulário. Todos os outros parâmetros são opcionais com o valor padrão. Quando o formulário é criado, ele vem com três campos padrão: Nome, Sobrenome, Email.

```
POST /rest/asset/v1/forms.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

As Forms estão [atualizadas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) com uma chamada semelhante por meio de sua id. Durante a criação ou atualização, qualquer um dos parâmetros de estilo base é acessível e editável, permitindo modificar como o formulário é exibido ao usuário final.

```
POST /rest/asset/v1/form/736.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Os comportamentos de página de visitante e agradecimento conhecidos não podem ser modificados por meio das chamadas de criação ou atualização de formulário e devem ser acessados por meio de seus respectivos endpoints.

## Metadados de campo

Para adicionar ou editar corretamente os campos pertencentes a um formulário, você deve recuperar a lista de campos válidos para a instância de destino. As interações de campo são sempre feitas com base na propriedade id do campo exibida para cada item no resultado.

Para campos de cliente potencial, isso é feito usando o ponto de extremidade [Obter Campos de Formulário Disponíveis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) e inclui o tipo de dados e os metadados padrão do campo quando ele é adicionado a um formulário.

```
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

Para campos personalizados de Membros de Programa, chame [Obter Campos de Membros de Programa de Formulário Disponíveis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  endpoint para recuperar tipos de dados de campo personalizado e metadados padrão do Membro do programa. Para usar esses campos em um formulário, o formulário deve estar dentro de um Programa (não no Design Studio). As Landing Pages que contêm formulários usando esses campos também devem ficar dentro de um Programa (não podem ficar no Design Studio nem ser clonadas no Design Studio).

```
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

Cada formulário contém uma lista editável de campos, que serão exibidos ao usuário final quando carregados. Cada campo é adicionado, atualizado ou excluído da lista de campos, um de cada vez, por meio de seus respectivos endpoints.

[Adicionar um campo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) requer somente a identificação do formulário pai e o fieldId do campo. Todos os outros campos estarão vazios ou terão valores padrão com base no tipo de dados e nos metadados do campo. Os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.

```
POST /rest/asset/v1/form/{id}/fields.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

As atualizações podem editar todos os mesmos campos que a adição de um campo e, de forma semelhante, exigem a ID do formulário e o fieldId, exceto que fieldId é um parâmetro de caminho e não um parâmetro de consulta ao executar atualizações.

```
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

No exemplo acima, estamos atualizando o campo LastName, que é uma sequência de caracteres simples. Alguns campos de formulário são mais complexos. Por exemplo, o campo Saudação é um tipo de campo &quot;select&quot; que contém uma lista de itens e um valor padrão. Se você adicionar ou atualizar um campo de tipo de seleção, a menos que defina uma das opções para ter um valor `isDefault` verdadeiro, a primeira opção não terá valor e será rotulada como &quot;Selecionar...&quot;

![Saudação](assets/form-field-salutation.png)

Para atualizar os itens da lista, o formato do parâmetro &quot;values&quot; é o seguinte:

```
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

 

Para determinar como formatar um campo de formulário complexo, verifique a resposta de Adicionar campo para formulário.

### Reorganização do campo

Os campos de um formulário devem ser reorganizados todos como uma única unidade por meio do ponto de extremidade [Alterar Posições dos Campos de Formulário](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). O ponto de extremidade requer um parâmetro chamado `positions`, que é uma Matriz JSON de objetos com três membros:

- columnNumber
- rowNumber
- fieldName (refere-se à id do campo)

Os campos em um formulário são organizados em uma interface semelhante a uma tabela, com até três colunas e até dez linhas. A linha e a coluna são indexadas a partir de 0, portanto, a primeira linha e a primeira coluna são ambas indicadas passando um 0. Todos os campos devem ocupar uma posição exclusiva

Se o campo de destino também for um conjunto de campos, seu registro dentro da matriz de posições também deverá conter um parâmetro chamado fieldList, uma matriz de objetos contendo os mesmos membros columnNumber, rowNumber e fieldName. O conjunto de campos em si é tratado como um único campo para sua posição na lista pai, enquanto seus subcampos são posicionados de acordo com as posições fornecidas no parâmetro fieldList.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

### Texto formatado

Campos de rich text são adicionados por meio de um [ponto de extremidade separado](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) de campos de cliente potencial. O conteúdo do campo é transmitido como multipart/form-data. Ele deve ser estruturado como conteúdo HTML que não contém nenhum script, metatags ou tag de link.

```
POST /rest/asset/v1/form/{id}/richText.json
```

```
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

Os formulários Marketo apresentam um componente opcional chamado conjuntos de campos. Conjuntos de campos são grupos de campos tratados como um único campo dentro da lista de campos de nível superior para fins de movimento e tratamento por regras de visibilidade. Por exemplo, se houver um campo para Requisitos de conformidade e um cliente selecionar sim, ele poderá revelar um conjunto de campos contendo campos para requisitos de conformidade HIPAA e PCI.

Os campos em conjuntos de campos são exclusivos ao formulário como um todo, portanto, os campos duplicados podem não estar tanto na lista de campos pai do formulário quanto em um conjunto de campos filho. Os conjuntos de campos são adicionados por meio do endpoint [Adicionar Conjunto de Campos ao Formulário](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) e aparecerão no resultado de [Obter Campos para Formulário](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Os campos são adicionados a um conjunto de campos movendo-os para fieldList do conjunto de campos por meio de [Atualizar Posições de Campo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Para esses endpoints, os dados são transmitidos como POST x-www-form-urlencoded, não como JSON.

## Regra de visibilidade

Cada campo pode ter um conjunto de regras de visibilidade que determinam se o campo pode ser visto por um visitante, dependendo dos valores inseridos no formulário. As regras fazem uma comparação entre o valor de um subjectField presente no formulário e uma lista de valores fornecidos na regra. Cada campo pode ter um tipo de regra de visibilidade, mostrar, ocultar ou sempreMostrar e, em seguida, uma lista de regras a serem avaliadas. As regras são avaliadas de cima para baixo e a primeira regra que é avaliada como verdadeira é a que será aplicada.

A alteração das regras de visibilidade é uma atualização destrutiva.

```
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Para obter a lista completa de operadores disponíveis, consulte a página de referência do ponto de extremidade para [Adicionar regras de visibilidade do campo de formulário](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Acompanhamento

Os formulários do Marketo podem ter um comportamento dinâmico de página de acompanhamento, em que as regras para redirecionar para uma determinada página ou para permanecer na página atual podem ser aplicadas com base no conteúdo dos campos designados no envio. As regras podem ser chamadas de Regras de página de agradecimento ou Regras de página de acompanhamento alternadamente. Essas regras são representadas como uma matriz JSON com os membros `followupType`, `followupValue`, `operator`, `subjectField`, `values` e `default`. `default` é um valor booleano para o qual somente um registro na matriz pode ser verdadeiro. Quando um visitante se qualifica para nenhuma outra regra, a regra designada como padrão é usada. `followupType` pode ser lp ou url, em que lp indica uma ID de página de aterrissagem da Marketo para `followupValue`, e url indicará uma URL para outra página. O operador é usado para comparar o valor do campo de assunto com a lista de valores fornecida.

## Botão Enviar

O estilo do botão de envio do formulário é gerenciado com o ponto de extremidade [Botão de Envio de Atualização](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST). ButtonPosition, buttonStyle, label e waitingLabel (o rótulo exibido enquanto o envio está pendente) podem ser modificados.

Esta é uma atualização destrutiva.

## Aprovação

Como a maioria dos outros ativos, os formulários seguem um modelo aprovado por rascunho, em que pode haver uma versão de rascunho e/ou uma versão aprovada. Sempre que as atualizações forem aplicadas a um formulário, elas serão aplicadas primeiro à versão de rascunho e só serão exibidas ao vivo depois que o formulário for aprovado. A aprovação de um formulário utiliza a versão de rascunho atual e substitui a versão aprovada, se houver, pelo rascunho. Se o formulário precisar ser retirado do ar, primeiro ele deverá ser cancelado, o que excluirá quaisquer rascunhos atuais, e rebaixará a versão aprovada para um estado somente de rascunho. O Forms sempre deve ser recusado antes de tentar excluir.

## Criação de perfil progressiva

Quando a criação progressiva de perfil é ativada para um formulário, um conjunto de campos chamado &quot;Criação de perfil&quot; é incluído na lista de campos. Para adicionar ou remover campos da lista de criação de perfil progressiva, você deve usar o ponto de extremidade Atualizar posições de campo. Esse endpoint faz atualizações destrutivas, de modo que todos os campos no formulário devem ser incluídos em cada solicitação. O exemplo abaixo adiciona o campo &quot;Telefone&quot; à lista de criação de perfil progressiva.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
