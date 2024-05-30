---
title: "Objetos do Marketo"
feature: SOAP
description: "Visão geral dos objetos do Marketo"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---


# Objetos do Marketo

O Marketo usa objetos do Marketo (MObjects) para representar várias classes, como Program, Opportunity e OpportunityPersonRole.

## Estrutura de objetos MO

Os MObjects podem ser de um destes três tipos: Padrão, Personalizado ou Virtual. Os objetos padrão e personalizados representam entidades distintas, como lead ou empresa, enquanto os objetos virtuais, como leadrecord, são compostos de campos de um ou mais objetos. Objetos virtuais são objetos de conveniência usados na API, mas não existem no aplicativo do Marketo.

Os objetos consistem em:

- Um pequeno conjunto de atributos fixos comuns a todos os MObjects
   - Tipo obrigatório
   - ExternalKey opcional
   - id somente leitura, createdAt, updatedAt
- Uma lista de um ou mais atributos específicos do objeto, como pares de nome/valor, alguns dos quais podem ser obrigatórios. Por exemplo, nome em Oportunidade.
- Uma lista de referências a objetos associadas, como nome-do-objeto plus
   - Marketo ID ou
   - Chave externa como um par nome-atributo/valor-atributo.

### Chaves externas

As chaves externas são campos personalizados definidos em objetos do Marketo, como Lead ou Oportunidade. O nome é o nome do campo e o valor é o valor do campo, gerado em um sistema externo. **A Marketo não impõe uma restrição exclusiva nesses valores.** É de responsabilidade do usuário da API garantir que os valores sejam exclusivos. Caso ocorra uma duplicação, o Marketo usará o objeto adicionado mais recentemente. Isso é semelhante ao comportamento do campo padrão Endereço de email.

### APIs disponíveis

| API | Pode Operar Em |
|---|---|
| describeMObject | Registro de atividade, Registro de lead, Oportunidade, FunçãoPessoaOportunidade |
| getMObjects | Oportunidade, FunçãoPessoaOportunidade, Programa |
| syncMObjects | Oportunidade, FunçãoPessoaOportunidade, Programa |
| deleteMObjects | Oportunidade, OpportunityPersonRole |
| listMObjects | Registro de atividade, Registro de lead, Oportunidade, FunçãoPessoaOportunidade |
