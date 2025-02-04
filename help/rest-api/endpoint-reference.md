---
title: Referência do ponto de extremidade
feature: REST API
description: Referências de endpoint da API do Marketo
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 8a019985fc9ce7e1aa690ca26bfa263cd3c48cfc
workflow-type: tm+mt
source-wordcount: '4676'
ht-degree: 27%

---

# Referência do ponto de extremidade

Abaixo estão links para as referências da API REST do Marketo.

- [Ativo](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identidade](https://developer.adobe.com/marketo-apis/api/identity/)
- [Banco de Dados Principal](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Gerenciamento de usuários](https://developer.adobe.com/marketo-apis/api/user/)

## Lista de Pontos de Extremidade

Esta é uma lista abrangente de endpoints da REST API.

| Nome | Grupo | Método | URI | Permissão necessária |
|---|---|---|---|---|
| Adicionar atividades personalizadas | Atividades | POST | /rest/v1/activities/external.json | Atividade de leitura-gravação |
| Aprovar tipo de atividade personalizada | Atividades | POST | /rest/v1/activities/external/type/{apiName}/approve.json | Metadados de atividade de leitura e gravação |
| Criar atributos personalizados de tipo de atividade | Atividades | POST | /rest/v1/activities/external/type/{apiName}/attributes/create.json | Metadados de atividade de leitura e gravação |
| Criar tipos de atividade personalizados | Atividades | POST | /rest/v1/activities/external/type.json | Metadados de atividade de leitura e gravação |
| Excluir tipo de atividade personalizado | Atividades | POST | /rest/v1/activities/external/type/{apiName}/delete.json | Metadados de atividade de leitura e gravação |
| Excluir atributos de tipo de atividade personalizados | Atividades | POST | /rest/v1/activities/external/type/{apiName}/attributes/delete.json | Metadados de atividade de leitura e gravação |
| Descrever tipo de atividade personalizado | Atividades | GET | /rest/v1/activities/external/type/{apiName}/describe.json | Metadados de atividade somente de leitura |
| Descartar rascunho de tipo de atividade personalizado | Atividades | POST | /rest/v1/activities/external/type/{apiName}/discardDraft.json | Metadados de atividade de leitura e gravação |
| Obter tipos de atividade | Atividades | GET | /rest/v1/activities/types.json | Atividade somente de leitura |
| Obter Tipos de Atividades Personalizadas | Atividades | GET | /rest/v1/activities/external/types.json | Metadados de atividade somente de leitura |
| Obter Clientes Potenciais Excluídos | Atividades | GET | /rest/v1/activities/deletedleads.json | Atividade somente de leitura |
| Obter atividades de cliente em potencial | Atividades | GET | /rest/v1/activities.json | Atividade somente de leitura |
| Obter alterações de cliente potencial | Atividades | GET | /rest/v1/activities/leadchanges.json | Atividade somente de leitura |
| Obter token de paginação | Atividades | GET | /rest/v1/activities/pagingtoken.json | Atividade somente de leitura |
| Atualizar tipo de atividade personalizada | Atividades | POST | /rest/v1/activities/external/type/{apiName}.json | Metadados de atividade de leitura e gravação |
| Atualizar atributos de tipo de atividade personalizados | Atividades | POST | /rest/v1/activities/external/type/{apiName}/attributes/update.json | Metadados de atividade de leitura e gravação |
| Identidade | Autenticação | GET ou POST | /identity/oauth/token | None |
| Cancelar tarefa de atividade de exportação | Atividades de exportação em massa | POST | /bulk/v1/activities/export/{exportid}/cancel.json | Atividade somente de leitura |
| Criar Trabalho de Atividade de Exportação | Atividades de exportação em massa | POST | /bulk/v1/activities/export/create.json | Atividade somente de leitura |
| Enfileirar tarefa de atividade de exportação | Atividades de exportação em massa | POST | /bulk/v1/activities/export/{exportid}/enqueue.json | Atividade somente de leitura |
| Obter arquivo de atividade de exportação | Atividades de exportação em massa | GET | /bulk/v1/activities/export/{exportid}/file.json | Atividade somente de leitura |
| Obter Status do Trabalho de Atividade de Exportação | Atividades de exportação em massa | GET | /bulk/v1/activities/export/{exportid}/status.json | Atividade somente de leitura |
| Obter Trabalhos de Atividades de Exportação | Atividades de exportação em massa | GET | /bulk/v1/activities/export.json | Atividade somente de leitura |
| Cancelar Trabalho de Exportação de Objeto Personalizado | Exportar Objetos Personalizados em Massa | POST | /bulk/v1/customobjects/export/{exportid}/cancel.json | Objeto personalizado somente de leitura |
| Criar Trabalho de Exportação de Objeto Personalizado | Exportar Objetos Personalizados em Massa | POST | /bulk/v1/customobjects/export/create.json | Objeto personalizado somente de leitura |
| Enfileirar tarefa de exportação de objeto personalizado | Exportar Objetos Personalizados em Massa | POST | /bulk/v1/customobjects/export/{exportid}/enqueue.json | Objeto personalizado somente de leitura |
| Obter arquivo de objeto personalizado de exportação | Exportar Objetos Personalizados em Massa | GET | /bulk/v1/customobjects/export/{exportid}/file.json | Objeto personalizado somente de leitura |
| Obter Status do Trabalho de Objeto Personalizado de Exportação | Exportar Objetos Personalizados em Massa | GET | /bulk/v1/customobjects/export/{exportid}/status.json | Objeto personalizado somente de leitura |
| Obter Trabalhos de Exportação de Objeto Personalizado | Exportar Objetos Personalizados em Massa | GET | /bulk/v1/customobjects/export.json | Objeto personalizado somente de leitura |
| Cancelar tarefa de exportar lead | Clientes Potenciais de Exportação em Massa | POST | /bulk/v1/leads/export/{exportid}/cancel.json | Lead somente de leitura |
| Criar tarefa de exportação de clientes em potencial | Clientes Potenciais de Exportação em Massa | POST | /bulk/v1/leads/export/create.json | Lead somente de leitura |
| Enfileirar tarefa de lead de exportação | Clientes Potenciais de Exportação em Massa | POST | /bulk/v1/leads/export/{exportid}/enqueue.json | Lead somente de leitura |
| Obter Arquivo de Cliente Potencial de Exportação | Clientes Potenciais de Exportação em Massa | GET | /bulk/v1/leads/export/{exportid}/file.json | Lead somente de leitura |
| Obter Status do Trabalho de Exportação de Cliente Potencial | Clientes Potenciais de Exportação em Massa | GET | /bulk/v1/leads/export/{exportid}/status.json | Lead somente de leitura |
| Obter Trabalhos de Exportação de Clientes Potenciais | Clientes Potenciais de Exportação em Massa | GET | /bulk/v1/leads/export.json | Lead somente de leitura |
| Cancelar Trabalho de Membro do Programa de Exportação | Membros do programa de exportação em massa | POST | /bulk/v1/program/members/export/{exportid}/cancel.json | Lead somente de leitura |
| Criar Trabalho de Membro do Programa de Exportação | Membros do programa de exportação em massa | POST | /bulk/v1/program/members/export/create.json | Lead somente de leitura |
| Enfileirar Trabalho de Membro do Programa de Exportação | Membros do programa de exportação em massa | POST | /bulk/v1/program/members/export/{exportid}/enqueue.json | Lead somente de leitura |
| Obter arquivo de membro do programa de exportação | Membros do programa de exportação em massa | GET | /bulk/v1/program/members/export/{exportid}/file.json | Lead somente de leitura |
| Obter Status de Trabalho de Membro do Programa de Exportação | Membros do programa de exportação em massa | GET | /bulk/v1/program/members/export/{exportid}/status.json | Lead somente de leitura |
| Obter Trabalhos de Membros do Programa de Exportação | Membros do programa de exportação em massa | GET | /bulk/v1/program/members/export.json | Lead somente de leitura |
| Obter Falhas de Importação de Objeto Personalizado | Importar Objetos Personalizados em Massa | GET | /bulk/v1/customobjects/import/{id}/failures.json | Objeto personalizado de leitura-gravação |
| Obter Status do Objeto Personalizado de Importação | Importar Objetos Personalizados em Massa | GET | /bulk/v1/customobjects/import/{id}/status.json | Objeto personalizado de leitura-gravação |
| Obter Avisos de Importação de Objeto Personalizado | Importar Objetos Personalizados em Massa | GET | /bulk/v1/customobjects/import/{id}/warnings.json | Objeto personalizado de leitura-gravação |
| Importar objetos personalizados | Importar Objetos Personalizados em Massa | POST | /bulk/v1/customobjects/{apiName}/import.json | Objeto personalizado de leitura-gravação |
| Obter Falhas de Cliente Potencial de Importação | Importar clientes em potencial em massa | GET | /bulk/v1/leads/batch/{id}/failures.json | Lead de leitura-gravação |
| Obter Status de Importação de Cliente Potencial | Importar clientes em potencial em massa | GET | /bulk/v1/leads/batch/{id}.json | Lead de leitura-gravação |
| Obter Avisos de Importação de Cliente Potencial | Importar clientes em potencial em massa | GET | /bulk/v1/leads/batch/{id}/warnings.json | Lead de leitura-gravação |
| Importar clientes em potencial | Importar clientes em potencial em massa | POST | /bulk/v1/leads.json | Lead de leitura-gravação |
| Obter Falhas de Membro do Programa de Importação | Membros do programa de importação em massa | GET | /bulk/v1/program/members/import/{id}/failures.json | Lead de leitura-gravação |
| Obter Status de Membro do Programa de Importação | Membros do programa de importação em massa | GET | /bulk/v1/program/members/import/{id}/status.json | Lead de leitura-gravação |
| Obter avisos do membro do programa de importação | Membros do programa de importação em massa | GET | /bulk/v1/program/members/import/{id}/warnings.json | Lead de leitura-gravação |
| Importar membros do programa | Membros do programa de importação em massa | POST | /bulk/v1/program/{programId}/members/import.json | Lead de leitura-gravação |
| Obter Campanha por ID | Campanhas | GET | /rest/v1/campaigns/{id}.json | Campanhas somente leitura |
| Obter campanhas | Campanhas | GET | /rest/v1/campaigns.json | Campanhas somente leitura |
| Solicitar campanha | Campanhas | POST | /rest/v1/campaigns/{id}/trigger.json | Campanhas de leitura e gravação |
| Programar campanha | Campanhas | POST | /rest/v1/campaigns/{id}/schedule.json | Campanhas de leitura e gravação |
| Obter canal por nome | Canais | GET | /rest/asset/v1/channel/byName.json | Ativo somente leitura |
| Obter canais | Canais | GET | /rest/asset/v1/channels.json | Ativo somente leitura |
| Excluir Empresas | Empresas | POST | /rest/v1/companies/delete.json | Empresa de leitura-gravação |
| Descrever Empresas | Empresas | GET | /rest/v1/companies/describe.json | Empresa somente de leitura |
| Obter Empresas | Empresas | GET | /rest/v1/companies.json | Empresa somente de leitura |
| Sincronizar Empresas | Empresas | POST | /rest/v1/companies.json | Empresa de leitura-gravação |
| Obter campo da empresa por nome | Empresas | GET | /rest/v1/companies/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Obter campos da empresa | Empresas | GET | /rest/v1/companies/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Adicionar campos de tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/addField.json | Tipo de objeto personalizado de leitura-gravação |
| Aprovar tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/approve.json | Tipo de objeto personalizado de leitura-gravação |
| Excluir objetos personalizados | Objetos personalizados | POST | /rest/v1/customobjects/{name}/delete.json | Objeto personalizado de leitura-gravação |
| Excluir tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/delete.json | Tipo de objeto personalizado de leitura-gravação |
| Excluir campos de tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/deleteField.json | Tipo de objeto personalizado de leitura-gravação |
| Descrever objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects/{name}/describe.json | Objeto personalizado somente de leitura |
| Descrever tipo de objeto personalizado | Objetos personalizados | GET | /rest/v1/customobjects/schema/{apiName}/describe.json | Tipo de objeto personalizado somente de leitura |
| Descartar Rascunho de Tipo de Objeto Personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/discardDraft.json | Tipo de objeto personalizado de leitura-gravação |
| Obter objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects/{name}.json | Objeto personalizado somente de leitura |
| Obter objetos vinculáveis personalizados | Objetos personalizados | GET | /rest/v1/customobjects/schema/linkableObjects.json | Tipo de objeto personalizado somente de leitura |
| Obter Assets Dependente de Objeto Personalizado | Objetos personalizados | GET | /rest/v1/customobjects/schema/{apiName}/dependentAssets.json | Tipo de objeto personalizado somente de leitura |
| Obter Tipos de Dados de Campo de Tipo de Objeto Personalizado | Objetos personalizados | GET | /rest/v1/customobjects/schema/fieldDataTypes.json | Tipo de objeto personalizado somente de leitura |
| Lista de objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects.json | Objeto personalizado somente de leitura |
| Lista de Tipos de Objeto Personalizados | Objetos personalizados | GET | /rest/v1/customobjects/schema.json | Tipo de objeto personalizado somente de leitura |
| Sincronizar objetos personalizados | Objetos personalizados | POST | /rest/v1/customobjects/{name}.json | Objeto personalizado de leitura-gravação |
| Sincronizar tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema.json | Tipo de objeto personalizado de leitura-gravação |
| Atualizar campo de tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/updateField.json | Tipo de objeto personalizado de leitura-gravação |
| Aprovar rascunho do modelo de email | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}/approveDraft.json | Ativo de leitura-gravação |
| Clonar modelo de e-mail | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}/clone.json | Ativo de leitura-gravação |
| Criar modelo de e-mail | Modelos de e-mail | POST | /rest/asset/v1/emailTemplates.json | Ativo de leitura-gravação |
| Excluir modelo de e-mail | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}/delete.json | Ativo de leitura-gravação |
| Descartar Rascunho de Modelo de Email | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}/discardDraft.json | Ativo de leitura-gravação |
| Obter Modelo de email por ID | Modelos de e-mail | GET | /rest/asset/v1/emailTemplate/{id}.json | Ativo somente leitura |
| Obter Modelo de email por Nome | Modelos de e-mail | GET | /rest/asset/v1/emailTemplate/byName.json | Ativo somente leitura |
| Obter conteúdo do modelo de email por ID | Modelos de e-mail | GET | /rest/asset/v1/emailTemplate/{id}/content.json | Ativo somente leitura |
| Obter Modelo De Email Usado Por | Modelos de e-mail | GET | /rest/asset/v1/emailTemplates/{id}/usedBy.json | Ativo somente leitura |
| Obter Modelos de Email | Modelos de e-mail | GET | /rest/asset/v1/emailTemplates.json | Ativo somente leitura |
| Cancelar aprovação do rascunho do modelo de e-mail | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}/unapprove.json | Ativo de leitura-gravação |
| Atualizar conteúdo do modelo de e-mail | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}/content.json | Ativo de leitura-gravação |
| Atualizar metadados do modelo de email | Modelos de e-mail | POST | /rest/asset/v1/emailTemplate/{id}.json | Ativo de leitura-gravação |
| Adicionar módulo de e-mail | Emails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/add.json | Ativo de leitura-gravação |
| Aprovar rascunho de email | Emails | POST | /rest/asset/v1/email/{id}/approveDraft.json | Ativo de leitura-gravação |
| Clonar e-mail | Emails | POST | /rest/asset/v1/email/{id}/clone.json | Ativo de leitura-gravação |
| Criar e-mail | Emails | POST | /rest/asset/v1/emails.json | Ativo de leitura-gravação |
| Excluir e-mail | Emails | POST | /rest/asset/v1/email/{id}/delete.json | Ativo de leitura-gravação |
| Excluir módulo | Emails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/delete.json | Ativo de leitura-gravação |
| Descartar rascunho de email | Emails | POST | /rest/asset/v1/email/{id}/discardDraft.json | Ativo de leitura-gravação |
| Módulo de email duplicado | Emails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json | Ativo de leitura-gravação |
| Obter email por ID | Emails | GET | /rest/asset/v1/email/{id}.json | Ativo somente leitura |
| Obter email por nome | Emails | GET | /rest/asset/v1/email/byName.json | Ativo somente leitura |
| Obter conteúdo de email | Emails | GET | /rest/asset/v1/email/{id}/content.json | Ativo somente leitura |
| Obter conteúdo dinâmico de email | Emails | GET | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Ativo somente leitura |
| Obter conteúdo completo do email | Emails | GET | /rest/asset/v1/email/{id}/fullContent.json | Ativo somente leitura |
| Obter variáveis de email | Emails | GET | /rest/asset/v1/email/{id}/variables.json | Ativo somente leitura |
| Obter campos de email CC | Emails | GET | /rest/asset/v1/email/ccFields.json | Ativo somente leitura |
| Receber Emails | Emails | GET | /rest/asset/v1/emails.json | Ativo somente leitura |
| Reorganizar módulos de email | Emails | POST | /rest/asset/v1/email/{id}/content/rearrange.json | Ativo de leitura-gravação |
| Renomear módulo de email | Emails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/rename.json | Ativo de leitura-gravação |
| Enviar e-mail de exemplo | Emails | POST | /rest/asset/v1/email/{id}/sendSample.json | Ativo de leitura-gravação |
| Cancelar aprovação de email | Emails | POST | /rest/asset/v1/email/{id}/unapprove.json | Ativo de leitura-gravação |
| Atualizar conteúdo de email | Emails | POST | /rest/asset/v1/email/{id}/content.json | Ativo de leitura-gravação |
| Seção Atualizar conteúdo de email | Emails | POST | /rest/asset/v1/email/{id}/content/{htmlId}.json | Ativo de leitura-gravação |
| Seção Atualizar Conteúdo Dinâmico de Email | Emails | POST | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Ativo de leitura-gravação |
| Atualizar conteúdo completo do email | Emails | POST | /rest/asset/v1/emails/{id}/fullContent.json | Ativo de leitura-gravação |
| Atualizar metadados de email | Emails | POST | /rest/asset/v1/email/{id}.json | Ativo de leitura-gravação |
| Atualizar variável de email | Emails | POST | /rest/asset/v1/email/{id}/variable/{name}.json | Ativo de leitura-gravação |
| Criar arquivo | Arquivos | POST | /rest/asset/v1/files.json | Ativo de leitura-gravação |
| Obter arquivo por ID | Arquivos | GET | /rest/asset/v1/file/{id}.json | Ativo somente leitura |
| Obter arquivo por nome | Arquivos | GET | /rest/asset/v1/file/byName.json | Ativo somente leitura |
| Obter arquivos | Arquivos | GET | /rest/asset/v1/files.json | Ativo somente leitura |
| Atualizar conteúdo do arquivo | Arquivos | POST | /rest/asset/v1/file/{id}/content.json | Ativo de leitura-gravação |
| Criar pasta | Pastas | POST | /rest/asset/v1/folders.json | Ativo de leitura-gravação |
| Excluir pasta | Pastas | POST | /rest/asset/v1/folder/{id}/delete.json | Ativo de leitura-gravação |
| Obter pasta por ID | Pastas | GET | /rest/asset/v1/folder/{id}.json | Ativo somente leitura |
| Obter pasta por nome | Pastas | GET | /rest/asset/v1/folder/byName.json | Ativo somente leitura |
| Obter conteúdo da pasta | Pastas | GET | /rest/asset/v1/folder/{id}/content.json | Ativo somente leitura |
| Obter Pastas | Pastas | GET | /rest/asset/v1/folders.json | Ativo somente leitura |
| Atualizar metadados da pasta | Pastas | POST | /rest/asset/v1/folder/{id}.json | Ativo de leitura-gravação |
| Adicionar campo ao formulário | Campos do formulário | POST | /rest/asset/v1/form/{id}/fields.json | Ativo de leitura-gravação |
| Adicionar conjunto de campos ao formulário | Campos do formulário | POST | /rest/asset/v1/form/{id}/fieldSet.json | Ativo de leitura-gravação |
| Adicionar regras de visibilidade do campo de formulário | Campos do formulário | POST | /rest/asset/v1/form/{formId}/field/{fieldId}/visibility.json | Ativo de leitura-gravação |
| Adicionar Campo de Rich Text | Campos do formulário | POST | /rest/asset/v1/form/{id}/richText.json | Ativo de leitura-gravação |
| Excluir campo do conjunto de campos | Campos do formulário | POST | /rest/asset/v1/form/{id}/fieldSet/{fieldSetId}/field/{fieldId}/delete.json | Ativo de leitura-gravação |
| Excluir campo de formulário | Campos do formulário | POST | /rest/asset/v1/form/{id}/field/{fieldId}/delete.json | Ativo de leitura-gravação |
| Obter campos de formulário disponíveis | Campos do formulário | GET | /rest/asset/v1/form/fields.json | Ativo somente leitura |
| Obter campos de membros do programa de formulário disponíveis | Campos do formulário | GET | /rest/asset/v1/form/programMemberFields.json | Ativo somente leitura |
| Obter campos para formulário | Campos do formulário | GET | /rest/asset/v1/form/{id}/fields.json | Ativo somente leitura |
| Atualizar Posições de Campo | Campos do formulário | POST | /rest/asset/v1/form/{id}/reArrange.json | Ativo de leitura-gravação |
| Atualizar campo de formulário | Campos do formulário | POST | /rest/asset/v1/form/{id}/field/{fieldId}.json | Ativo de leitura-gravação |
| Aprovar rascunho de formulário | Formulários | POST | /rest/asset/v1/form/{id}/approveDraft.json | Ativo de leitura-gravação |
| Clonar formulário | Formulários | POST | /rest/asset/v1/form/{id}/clone.json | Ativo de leitura-gravação |
| Criar formulário | Formulários | POST | /rest/asset/v1/forms.json | Ativo de leitura-gravação |
| Obter Formulário Usado por | Formulários | GET | /rest/asset/v1/form/{id}/usedBy.json | Ativo de leitura-gravação |
| Excluir formulário | Formulários | POST | /rest/asset/v1/form/{id}/delete.json | Ativo de leitura-gravação |
| Descartar Rascunho de Formulário | Formulários | POST | /rest/asset/v1/form/{id}/discardDraft.json | Ativo de leitura-gravação |
| Obter formulário por ID | Formulários | GET | /rest/asset/v1/form/{id}.json | Ativo somente leitura |
| Obter formulário por nome | Formulários | GET | /rest/asset/v1/form/byName.json | Ativo somente leitura |
| Obter o Forms | Formulários | GET | /rest/asset/v1/forms.json | Ativo somente leitura |
| Obter página de agradecimento por ID de formulário | Formulários | GET | /rest/asset/v1/form/{id}/thankYouPage.json | Ativo somente leitura |
| Atualizar metadados do formulário | Formulários | POST | /rest/asset/v1/form/{id}.json | Ativo de leitura-gravação |
| Botão Enviar de Atualização | Formulários | POST | /rest/asset/v1/{id}/submitButton.json | Ativo de leitura-gravação |
| Atualizar página de agradecimento | Formulários | POST | /rest/asset/v1/form/{id}/thankYouPage.json | Ativo de leitura-gravação |
| Adicionar seção de conteúdo da landing page | Conteúdo da landing page | POST | /rest/asset/v1/landingPage/{id}/content.json | Ativo de leitura-gravação |
| Excluir seção de conteúdo da página de aterrissagem | Conteúdo da landing page | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}/delete.json | Ativo de leitura-gravação |
| Obter conteúdo da landing page | Conteúdo da landing page | GET | /rest/asset/v1/landingPage/{id}/content.json | Ativo somente leitura |
| Obter conteúdo dinâmico da landing page | Conteúdo da landing page | GET | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Ativo somente leitura |
| Seção Atualizar conteúdo da página inicial | Conteúdo da landing page | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}.json | Ativo de leitura-gravação |
| Seção Atualizar conteúdo dinâmico da página de aterrissagem | Conteúdo da landing page | POST | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Ativo de leitura-gravação |
| Aprovar rascunho do modelo de página de aterrissagem | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}/approveDraft.json | Ativo de leitura-gravação |
| Clonar modelo de página | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}/clone.json | Ativo de leitura-gravação |
| Criar modelo de página | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate.json | Ativo de leitura-gravação |
| Excluir modelo de página | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}/delete.json | Ativo de leitura-gravação |
| Descartar rascunho de modelo de página de aterrissagem | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}/discardDraft.json | Ativo de leitura-gravação |
| Obter modelo de landing page por ID | Modelos de páginas | GET | /rest/asset/v1/landingPageTemplate/{id}.json | Ativo somente leitura |
| Obter modelo de landing page por nome | Modelos de páginas | GET | /rest/asset/v1/landingPageTemplates/byName.json | Ativo somente leitura |
| Obter conteúdo do modelo da landing page | Modelos de páginas | GET | /rest/asset/v1/landingPageTemplate/{id}/content.json | Ativo somente leitura |
| Obter modelos de página de aterrissagem | Modelos de páginas | GET | /rest/asset/v1/landingPageTemplates.json | Ativo somente leitura |
| Cancelar aprovação do modelo de página | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}/unapprove.json | Ativo de leitura-gravação |
| Atualizar conteúdo do modelo da página de aterrissagem | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}/content.json | Ativo de leitura-gravação |
| Atualizar metadados do modelo da página de aterrissagem | Modelos de páginas | POST | /rest/asset/v1/landingPageTemplate/{id}.json | Ativo de leitura-gravação |
| Aprovar rascunho da página de destino | Páginas de aterrissagem | POST | /rest/asset/v1/landingPage/{id}/approveDraft.json | Ativo de leitura-gravação |
| Clonar página | Páginas de aterrissagem | POST | /rest/asset/v1/landingPage/{id}/clone.json | Ativo de leitura-gravação |
| Criar páginas de destino | Páginas de aterrissagem | POST | /rest/asset/v1/landingPages.json | Ativo de leitura-gravação |
| Excluir página | Páginas de aterrissagem | POST | /rest/asset/v1/landingPage/{id}/delete.json | Ativo de leitura-gravação |
| Descartar rascunho da página inicial | Páginas de aterrissagem | POST | /rest/asset/v1/landingPage/{id}/discardDraft.json | Ativo de leitura-gravação |
| Obter página inicial por ID | Páginas de aterrissagem | GET | /rest/asset/v1/landingPage/{id}.json | Ativo somente leitura |
| Obter página inicial por nome | Páginas de aterrissagem | GET | /rest/asset/v1/landingPage/byName.json | Ativo somente leitura |
| Obter variáveis de página inicial | Páginas de aterrissagem | GET | /rest/asset/v1/landingPage/{id}/variables.json | Ativo somente leitura |
| Obter páginas de destino | Páginas de aterrissagem | GET | /rest/asset/v1/landingPages.json | Ativo somente leitura |
| Visualizar página de destino | Páginas de aterrissagem | GET | /rest/asset/v1/landingPage/{id}/preview.json | Ativo somente leitura |
| Cancelar aprovação da página | Páginas de aterrissagem | POST | /rest/asset/v1/landingPage/{id}/unapprove.json | Ativo de leitura-gravação |
| Atualizar metadados da página de aterrissagem | Páginas de aterrissagem | POST | /rest/asset/v1/{id}.json | Ativo de leitura-gravação |
| Atualizar variáveis de landing page | Páginas de aterrissagem | POST | /rest/asset/v1/landingPage/{id}/variable/{variableId}.json | Ativo de leitura-gravação |
| Criar regras de redirecionamento de página inicial | Páginas de aterrissagem | POST | /rest/asset/v1/redirectRules.json | Regras de redirecionamento para leitura e gravação |
| Excluir regra de redirecionamento de página inicial | Páginas de aterrissagem | POST | /rest/asset/v1/redirectRule/{id}/delete.json | Regras de redirecionamento para leitura e gravação |
| Obter regras de redirecionamento de página de aterrissagem | Páginas de aterrissagem | GET | /rest/asset/v1/redirectRules.json | Regras de redirecionamento somente para leitura |
| Obter regra de redirecionamento de página inicial por ID | Páginas de aterrissagem | GET | /rest/asset/v1/redirectRule/{id}.json | Regras de redirecionamento somente para leitura |
| Atualizar regra de redirecionamento de página inicial | Páginas de aterrissagem | POST | /rest/asset/v1/redirectRule/{id}.json | Regras de redirecionamento para leitura e gravação |
| Obter domínios de página de aterrissagem | Páginas de aterrissagem | GET | /rest/asset/v1/landingPageDomains.json | Regras de redirecionamento somente para leitura |
| Associar lead | Leads | POST | /rest/v1/leads/{id}/associate.json | Lead de leitura-gravação |
| Alterar status do programa de clientes potenciais | Leads | POST | /rest/v1/leads/programs/{programId}/status.json | Lead de leitura-gravação |
| Excluir clientes em potencial | Leads | POST | /rest/v1/leads.json | Lead de leitura-gravação |
| Descrever lead | Leads | GET | /rest/v1/leads/describe.json | Lead somente de leitura |
| Descrever lead2 | Leads | GET | /rest/v1/leads/describe2.json | Lead somente de leitura |
| Descrever membro do programa | Leads | GET | /rest/v1/program/members/describe.json | Lead somente de leitura |
| Obter lead por ID | Leads | GET | /rest/v1/lead/{id}.json | Lead somente de leitura |
| Obter Partições de Cliente Potencial | Leads | GET | /rest/v1/leads/partitions.json | Lead somente de leitura |
| Obter clientes em potencial por tipo de filtro | Leads | GET | /rest/v1/leads.json | Lead somente de leitura |
| Obter clientes em potencial por ID de programa | Leads | GET | /rest/v1/leads/programs/{programId}.json | Lead somente de leitura |
| Mesclar leads | Leads | POST | /rest/v1/leads/{id}/merge.json | Lead de leitura-gravação |
| Obter Listas por ID de Cliente Potencial | Leads | GET | /rest/v1/leads/{leadId}.json | Ativo somente leitura |
| Obter Programas por ID de Cliente Potencial | Leads | GET | /rest/v1/leads/{leadId}programMembership.json | Ativo somente leitura |
| Obter Campanhas inteligentes por ID de cliente potencial | Leads | GET | /rest/v1/leads/{leadId}/smartCampaignMembership.json | Ativo somente leitura |
| Enviar lead ao Marketo | Leads | POST | /rest/v1/leads/partitions.json | Lead de leitura-gravação |
| Enviar formulário | Leads | POST | /rest/v1/leads/submitForm.json | Lead de leitura-gravação |
| Sincronizar clientes em potencial | Leads | POST | /rest/v1/leads.json | Lead de leitura-gravação |
| Atualizar Partição de Cliente Potencial | Leads | POST | /rest/v1/leads/partitions.json | Lead de leitura-gravação |
| Obter Campo de Cliente Potencial por Nome | Leads | GET | /rest/v1/leads/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Obter campos de cliente em potencial | Leads | GET | /rest/v1/leads/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Criar campos de cliente em potencial | Leads | POST | /rest/v1/leads/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Atualizar campo de cliente em potencial | Leads | POST | /rest/v1/leads/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Adicionar Membros da Lista de Contas Nomeadas | Listas de contas nomeadas | POST | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Conta nomeada de leitura e gravação |
| Excluir Listas de Contas Nomeadas | Listas de contas nomeadas | POST | /rest/v1/namedaccountlists/delete.json | Lista de contas nomeadas de leitura e gravação |
| Obter Membros da Lista de Contas Nomeadas | Listas de contas nomeadas | GET | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Conta nomeada somente de leitura |
| Obter Listas de Contas Nomeadas | Listas de contas nomeadas | GET | /rest/v1/namedaccountlists.json | Lista de contas nomeadas somente de leitura |
| Remover Membros da Lista de Contas Nomeadas | Listas de contas nomeadas | POST | /rest/v1/namedaccountlist/{id}/namedaccounts/remove.json | Conta nomeada de leitura e gravação |
| Sincronizar Listas de Contas Nomeadas | Listas de contas nomeadas | POST | /rest/v1/namedaccountlists.json | Lista de contas nomeadas de leitura e gravação |
| Excluir contas nomeadas | Contas nomeadas | POST | /rest/v1/namedaccounts/delete.json | Conta nomeada de leitura e gravação |
| Descrever Contas Nomeadas | Contas nomeadas | GET | /rest/v1/namedaccounts/describe.json | Conta nomeada somente de leitura |
| Obter Contas Nomeadas | Contas nomeadas | GET | /rest/v1/namedaccounts.json | Conta nomeada somente de leitura |
| Sincronizar Contas Nomeadas | Contas nomeadas | POST | /rest/v1/namedaccounts.json | Conta nomeada de leitura e gravação |
| Obter campo de conta nomeado por nome | Contas nomeadas | GET | /rest/v1/namedaccounts/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Obter campos de conta nomeados | Contas nomeadas | GET | /rest/v1/namedaccounts/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Excluir Oportunidades | Oportunidades | POST | /rest/v1/opportunities/delete.json | Oportunidade de leitura-gravação |
| Excluir Funções da Oportunidade | Oportunidades | POST | /rest/v1/opportunities/roles/delete.json | Oportunidade de leitura-gravação |
| Descrever oportunidade | Oportunidades | GET | /rest/v1/opportunities/describe.json | Oportunidade somente de leitura |
| Descrever função da oportunidade | Oportunidades | GET | /rest/v1/opportunities/roles/describe.json | Oportunidade somente de leitura |
| Obter oportunidades | Oportunidades | GET | /rest/v1/opportunities.json | Oportunidade somente de leitura |
| Obter Funções da Oportunidade | Oportunidades | GET | /rest/v1/opportunities/roles.json | Oportunidade somente de leitura |
| Oportunidades de Sincronização | Oportunidades | POST | /rest/v1/opportunities.json | Oportunidade de leitura-gravação |
| Sincronizar funções de oportunidade | Oportunidades | POST | /rest/v1/opportunities/roles.json | Oportunidade de leitura-gravação |
| Obter campo de oportunidade por nome | Oportunidades | GET | /rest/v1/opportunity/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Obter campos de oportunidade | Oportunidades | GET | /rest/v1/opportunities/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Excluir membros do programa | Membros do programa | POST | /rest/v1/programs/{programId}/members/delete.json | Lead de leitura-gravação |
| Descrever membro do programa | Membros do programa | GET | /rest/v1/programs/members/describe.json | Lead somente de leitura |
| Obter membros do programa | Membros do programa | GET | /rest/v1/programs/{programId}/members.json | Lead somente de leitura |
| Sincronizar Dados do Membro do Programa | Membros do programa | POST | /rest/v1/programs/{programId}/members.json | Lead de leitura-gravação |
| Status do Membro do Programa de Sincronização | Membros do programa | POST | /rest/v1/programs/{programId}/members/status.json | Lead de leitura-gravação |
| Obter campo de membro do programa por nome | Membros do programa | GET | /rest/v1/programs/member/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Obter campos de membro do programa | Membros do programa | GET | /rest/v1/programs/members/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Criar campos de membros do programa | Membros do programa | POST | /rest/v1/programs/members/schema/fields.json | Campo personalizado de esquema de leitura e gravação |
| Atualizar campo de membro do programa | Membros do programa | POST | /rest/v1/programs/member/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de leitura e gravação |
| Aprovar programa | Programas | POST | /rest/asset/v1/program/{id}/approve.json | Ativo de leitura-gravação |
| Clonar programa | Programas | POST | /rest/asset/v1/program/{id}/clone.json | Ativo de leitura-gravação |
| Criar programas | Programas | POST | /rest/asset/v1/programs.json | Ativo de leitura-gravação |
| Excluir programa | Programas | POST | /rest/asset/v1/program/{id}/delete.json | Ativo de leitura-gravação |
| Obter Programa por ID | Programas | GET | /rest/asset/v1/program/{id}.json | Ativo somente leitura |
| Obter programa por nome | Programas | GET | /rest/asset/v1/program/byName.json | Ativo somente leitura |
| Obter Programas | Programas | GET | /rest/asset/v1/programs.json | Ativo somente leitura |
| Obter Programas por Tag | Programas | GET | /rest/asset/v1/program/byTag.json | Ativo somente leitura |
| Obter lista inteligente por ID de programa | Programas | GET | /rest/asset/v1/program/{id}/smartList.json | Ativo somente leitura |
| Cancelar aprovação do programa | Programas | POST | /rest/asset/v1/program/{id}/unapprove.json | Ativo de leitura-gravação |
| Atualizar metadados do programa | Programas | POST | /rest/asset/v1/program/{id}.json | Ativo de leitura-gravação |
| Atualizar tag do programa | Programas | POST | /rest/asset/v1/program/{id}/tag/{tagType}.json | Ativo de leitura-gravação |
| Excluir tag do programa | Programas | POST | /rest/asset/v1/program/{id}/tag/{tagType}/delete.json | Ativo de leitura-gravação |
| Excluir Vendedores | Vendedores | POST | /rest/v1/salespersons/delete.json | Pessoa de vendas de leitura-gravação |
| Descrever Vendedores | Vendedores | GET | /rest/v1/salespersons/describe.json | Pessoa de vendas somente de leitura |
| Obter vendedores | Vendedores | GET | /rest/v1/salespersons.json | Pessoa de vendas somente de leitura |
| Sincronizar SalesPersons | Vendedores | POST | /rest/v1/salespersons.json | Pessoa de vendas de leitura-gravação |
| Obter segmentações | Segmentos | GET | /rest/asset/v1/segmentation.json | Ativo somente leitura |
| Obter Segmentos Para Segmentações | Segmentos | GET | /rest/asset/v1/segmentation/{id}/segments.json | Ativo somente leitura |
| Ativar campanha inteligente | Campanhas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/activate.json | Ativar campanha |
| Clonar campanha inteligente | Campanhas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/clone.json | Ativo de leitura-gravação |
| Criar campanha inteligente | Campanhas inteligentes | POST | /rest/asset/v1/smartCampaigns.json | Ativo de leitura-gravação |
| Desativar Campanha Inteligente | Campanhas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/deactivate.json | Desativar campanha |
| Excluir campanha inteligente | Campanhas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/delete.json | Ativo de leitura-gravação |
| Obter campanhas inteligentes | Campanhas inteligentes | GET | /rest/asset/v1/smartCampaigns.json | Ativo somente leitura |
| Obter Campanha Inteligente por ID | Campanhas inteligentes | GET | /rest/asset/v1/smartCampaign/{id}.json | Ativo somente leitura |
| Obter Campanha Inteligente por Nome | Campanhas inteligentes | GET | /rest/asset/v1/smartCampaign/byName.json | Ativo somente leitura |
| Obter lista inteligente por ID de campanha inteligente | Campanhas inteligentes | GET | /rest/asset/v1/smartCampaign/{id}/smartList.json | Ativo somente leitura |
| Atualizar campanha inteligente | Campanhas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}.json | Ativo de leitura-gravação |
| Clonar lista inteligente | Listas inteligentes | POST | /rest/asset/v1/smartList/{id}/clone.json | Ativo de leitura-gravação |
| Excluir lista inteligente | Listas inteligentes | POST | /rest/asset/v1/smartList/{id}/delete.json | Ativo de leitura-gravação |
| Obter lista inteligente por ID | Listas inteligentes | GET | /rest/asset/v1/smartList/{id}.json | Ativo somente leitura |
| Obter lista inteligente por nome | Listas inteligentes | GET | /rest/asset/v1/smartList/byName.json | Ativo somente leitura |
| Obter listas inteligentes | Listas inteligentes | GET | /rest/asset/v1/smartLists.json | Ativo somente leitura |
| Aprovar rascunho do trecho | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/approveDraft.json | Ativo de leitura-gravação |
| Clonar bloco de conteúdo | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/clone.json | Ativo de leitura-gravação |
| Criar trecho | Bl. conteúdo | POST | /rest/asset/v1/snippets.json | Ativo de leitura-gravação |
| Excluir bloco de conteúdo | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/delete.json | Ativo de leitura-gravação |
| Descartar rascunho de trecho | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/discardDraft.json | Ativo de leitura-gravação |
| Obter conteúdo dinâmico | Bl. conteúdo | GET | /rest/asset/v1/snippet/{id}/dynamicContent.json | Ativo somente leitura |
| Obter trecho por ID | Bl. conteúdo | GET | /rest/asset/v1/snippet/{id}.json | Ativo somente leitura |
| Obter conteúdo do trecho | Bl. conteúdo | GET | /rest/asset/v1/snippet/{id}/content.json | Ativo somente leitura |
| Obter trechos | Bl. conteúdo | GET | /rest/asset/v1/snippets.json | Ativo somente leitura |
| Bloco de conteúdo de cancelamento de aprovação | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/unapprove.json | Ativo de leitura-gravação |
| Atualizar conteúdo do trecho | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/content.json | Ativo de leitura-gravação |
| Atualizar conteúdo dinâmico do trecho | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}/dynamicContent/{segmentId}.json | Ativo de leitura-gravação |
| Atualizar metadados de trecho | Bl. conteúdo | POST | /rest/asset/v1/snippet/{id}.json | Ativo de leitura-gravação |
| Adicionar à lista | Listas estáticas | POST | /rest/v1/lists/{listId}/leads.json | Lead de leitura-gravação |
| Criar lista estática | Listas estáticas | POST | /asset/v1/staticLists.json | Ativo de leitura-gravação |
| Excluir lista estática | Listas estáticas | POST | /asset/v1/staticList/{id}/delete.json | Ativo de leitura-gravação |
| Obter clientes em potencial por ID de lista | Listas estáticas | GET | /rest/v1/lists/{listId}/leads.json | Lead somente de leitura |
| Obter Lista por ID | Listas estáticas | GET | /rest/v1/lists/{id}.json | Lead somente de leitura |
| Obter listas | Listas estáticas | GET | /rest/v1/lists.json | Lead somente de leitura |
| Obter Lista Estática por ID | Listas estáticas | GET | /asset/v1/staticList/{id}.json | Ativo somente leitura |
| Obter Lista Estática por Nome | Listas estáticas | GET | /asset/v1/staticList/byName.json | Ativo somente leitura |
| Obter Listas Estáticas | Listas estáticas | GET | /asset/v1/staticLists.json | Ativo somente leitura |
| Membro da lista | Listas estáticas | GET | /rest/v1/lists/{listId}/leads/ismember.json | Lead somente de leitura |
| Remover da lista | Listas estáticas | DELETE | /rest/v1/lists/{listId}/leads.json | Lead de leitura-gravação |
| Atualizar metadados da lista estática | Listas estáticas | POST | /asset/v1/staticList/{id}.json | Ativo de leitura-gravação |
| Obter tag por nome | Tags | GET | /rest/asset/v1/tagType/byName.json | Ativo somente leitura |
| Obter tipos de tag | Tags | GET | /rest/asset/v1/tagTypes.json | Ativo somente leitura |
| Criar token | Tokens | POST | /rest/asset/v1/folder/{id}/tokens.json | Ativo de leitura-gravação |
| Excluir token por nome | Tokens | POST | /rest/asset/v1/folder/{id}/tokens/delete.json | Ativo de leitura-gravação |
| Obter tokens por ID de pasta | Tokens | GET | /rest/asset/v1/folder/{id}/tokens.json | Ativo somente leitura |
| Adicionar Funções | Gerenciamento de usuários | POST | /userservice/management/v1/users/{userid}/roles/create.json | API de gerenciamento de acesso de usuários |
| Excluir Usuário Convidado | Gerenciamento de usuários | POST | /userservice/management/v1/users/{userId}/invite/delete.json | API de gerenciamento de acesso de usuários |
| Excluir Funções | Gerenciamento de usuários | POST | /userservice/management/v1/users/{userid}/roles/delete.json | API de gerenciamento de acesso de usuários |
| Excluir usuário | Gerenciamento de usuários | POST | /userservice/management/v1/users/{userId}/delete.json | API de gerenciamento de acesso de usuários |
| Obter Usuário Convidado por ID | Gerenciamento de usuários | GET | /userservice/management/v1/users/{userid}/invite.json | API de gerenciamento de acesso de usuários |
| Obter Funções | Gerenciamento de usuários | GET | /userservice/management/v1/users/roles.json | API de gerenciamento de acesso de usuários |
| Obter Funções e Espaços de Trabalho por ID | Gerenciamento de usuários | GET | /userservice/management/v1/users/{userid}/roles.json | API de gerenciamento de acesso de usuários |
| Obter usuários | Gerenciamento de usuários | GET | /userservice/management/v1/users/allusers.json | API de gerenciamento de acesso de usuários |
| Obter Usuário por ID | Gerenciamento de usuários | GET | /userservice/management/v1/users/{userid}/user.json | API de gerenciamento de acesso de usuários |
| Obter Espaços de Trabalho | Gerenciamento de usuários | GET | /userservice/management/v1/users/workspaces.json | API de gerenciamento de acesso de usuários |
| Convidar usuário | Gerenciamento de usuários | POST | /userservice/management/v1/users/invite.json | API de gerenciamento de acesso de usuários |
| Atualizar atributos do usuário | Gerenciamento de usuários | POST | /userservice/management/v1/users/{userId}/update.json | API de gerenciamento de acesso de usuários |