---
title: Teste de visualização de EXL
description: Exemplos de sintaxe de marcação do Adobe EXL para testar a visualização da extensão.
source-git-commit: 87d2584ed0ef2c1fa219f2a3ad120c91dc5491e0
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 11%

---


# Teste de visualização de EXL

## Blocos de alerta

>[!NOTE]
>
>Isto é uma nota. Use as notas para obter informações adicionais que o leitor deve conhecer.

>[!TIP]
>
>Isto é uma dica. Use dicas para obter informações opcionais, mas úteis.

>[!IMPORTANT]
>
>Este é um alerta importante. Use para obter informações que o leitor não deve ignorar.

>[!WARNING]
>
>Isso é um aviso. Use para obter informações sobre problemas em potencial.

>[!CAUTION]
>
>Isso é um aviso. Use para obter informações sobre riscos potenciais.

>[!ADMIN]
>
>Este é um alerta de administrador. Para conteúdo exclusivo do administrador.

>[!AVAILABILITY]
>
>Esta é uma nota de disponibilidade. Para obter detalhes sobre a disponibilidade de recursos.

>[!PREREQUISITES]
>
>Este é um bloco de pré-requisitos. Liste o que o leitor precisa antes de iniciar.

## Sombrear caixas

>[!BEGINSHADEBOX &quot;Título opcional&quot;]

Este conteúdo aparece com um plano de fundo cinza. Use as caixas de sombra para agrupar visualmente o conteúdo relacionado.

É possível incluir listas:

- Item um
- Item dois
- Item três

>[!ENDSHADEBOX]

>[!BEGINSHADEBOX]

Sombrear a caixa sem um título.

>[!ENDSHADEBOX]

## Seções flexíveis

+++Clique para expandir — exemplo básico

Esse conteúdo fica oculto até que o usuário selecione o título.

Você pode incluir qualquer conteúdo aqui, incluindo blocos de código:

```javascript
const example = 'hello world';
console.log(example);
```

+++

+++Configuração avançada

Use seções que podem ser recolhidas para conteúdo opcional ou avançado que, de outra forma, fragmentaria o fluxo principal.

| Configuração | Valor | Descrição |
| --- | --- | --- |
| timeout | 30 | Segundos antes do tempo limite da solicitação |
| tentativas | 3 | Número de novas tentativas |

{style="table-layout:auto"}

+++

## Ajuda contextual

A Ajuda contextual está oculta da visualização. Veja!
>[!CONTEXTUALHELP]
>id="models_insights_undefinedchannels"
>title="Canais indefinidos"
>abstract="Canais indefinidos são incluídos, mas não têm conversões atribuídas."

## Vídeo incorporado

>[!VIDEO](https://video.tv.adobe.com/v/3427028/?quality=12&learn=on)

## Macros de localização

Use [!DNL Marketo] para vincular os nomes de produtos de modo que eles não sejam localizados.

Use **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** para rótulos de elementos da interface do usuário.

Exemplo combinado: Em [!DNL Adobe Analytics], selecione **[!UICONTROL Workspace]** > **[!UICONTROL Criar projeto]**.

## Medalhas

[!BADGE Beta]{type=Informative}

[!BADGE Disponibilidade geral]{type=Positive}

[!BADGE Obsoleto]{type=Negative}

[!BADGE Experimental]{type=Caution}

## Guias

>[!BEGINTABS]

>[!TAB Requisitos]

>[!IMPORTANT]
>
>Você deve ter permissões de administrador para concluir esta tarefa.

Campos obrigatórios:

| Campo | Tipo | Obrigatório |
| --- | --- | --- |
| Nome | String | Sim |
| Email | String | Sim |
| Função | Lista Discriminada | Não |

>[!TAB Etapas]

1. Abra o Admin Console.
1. Selecione **[!UICONTROL Usuários]** > **[!UICONTROL Adicionar usuário]**.
1. Preencha os campos obrigatórios.
1. Selecione **[!UICONTROL Salvar]**.

>[!NOTE]
>
>As alterações entrarão em vigor imediatamente.

>[!TAB Resultado]

O novo usuário recebe um email de boas-vindas com um link para definir sua senha.

- O link expira após 24 horas.
- Os usuários podem solicitar um novo link da página de logon.

>[!ENDTABS]

## Blocos de código

```json
{
  "name": "example",
  "version": "1.0.0",
  "enabled": true
}
```

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```

## Tabelas

| Coluna um | Coluna dois | Coluna três |
| --- | --- | --- |
| [!UICONTROL Linha 1], célula 1 | Linha 1, célula 2 | [!DNL Row 1, cell 3] |
| Linha 2, célula 1 | Linha 2, célula 2 | Linha 2, célula 3 |

