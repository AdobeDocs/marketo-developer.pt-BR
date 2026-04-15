---
title: Servidor MCP
description: Saiba como conectar um assistente de IA ao Marketo usando o servidor MCP. Configure o Claude Desktop, o Cursor, o Código Claude ou o Código VS com suas credenciais do Marketo.
hidefromtoc: true
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
source-git-commit: d659eb0f604a68d03d5b00c0109d59ff321415df
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 1%

---

# [!DNL Marketo] Servidor MCP

O protocolo MCP é um padrão aberto que permite que as ferramentas de IA se comuniquem com serviços externos. O servidor MCP [!DNL Marketo] atua como uma ponte entre o assistente de IA e o [!DNL Marketo]. Ele expõe mais de 100 operações em formulários, programas, campanhas inteligentes, leads, emails, trechos, listas e pastas.

Quando a ferramenta de IA chama o servidor MCP, o servidor executa a chamada à API REST correspondente em seu nome, usando as credenciais fornecidas em cada solicitação. Você não precisa instalar, implantar nem executar nenhum software do lado do servidor.

## Pré-requisitos

- Uma instância [!DNL Marketo] com acesso à API REST habilitado
- Acesso de administrador para criar credenciais de API no [!DNL Marketo] LaunchPoint
- Uma das seguintes ferramentas de IA: Claude Desktop, Cursor, Claude Code (CLI) ou VS Code com o GitHub Copilot
- Acesso de rede à URL do servidor MCP: `https://marketo-mcp.adobe.io/mcp`

## Obter credenciais do Marketo

Você precisa dos seguintes valores da sua instância [!DNL Marketo]:

- **ID do cliente**
- **Segredo do cliente**
- **ID da Conta da Munchkin**

Se já os tiver, pule para [Configurar a ferramenta de IA](#configure-your-ai-tool).

### ID do cliente e segredo do cliente

1. Vá para **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Clique no serviço de API. Se você não tiver um, selecione **[!UICONTROL Novo]** > **[!UICONTROL Novo serviço]**, escolha **[!UICONTROL Personalizado]** como o tipo de serviço e atribua um usuário de API dedicado.
1. Clique em **[!UICONTROL Exibir Detalhes]** e copie os valores de **[!UICONTROL ID do Cliente]** e **[!UICONTROL Segredo do Cliente]**.

### ID da conta do Munchkin

1. Vá para **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Copie a **[!UICONTROL ID da Conta da Munchkin]**. O formato é `XXX-XXX-XXX` e corresponde ao prefixo da URL da instância.

## Configurar a ferramenta de IA

Cada ferramenta de IA lê a configuração do servidor MCP de um local diferente. Encontre sua ferramenta abaixo e siga as etapas para adicionar o servidor MCP [!DNL Marketo].

### Claude Desktop

Arquivo de configuração `claude_desktop_config.json`. Abra-o em um destes locais:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Janelas**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

Se o arquivo já contiver outros servidores MCP, adicione a entrada `marketo` em `mcpServers`. O exemplo a seguir mostra o bloco `mcpServers` completo:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Salve o arquivo, saia do Claude Desktop e abra-o novamente.

### Cursor

Se a configuração do MCP do cursor já contiver outros servidores, adicione a entrada `marketo` em `mcpServers`. O exemplo a seguir mostra o bloco `mcpServers` completo em **[!UICONTROL Configurações]** > **[!UICONTROL MCP]** ou `.cursor/mcp.json` no diretório do projeto:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Reiniciar o cursor.

### Código Claude (CLI)

Execute o seguinte comando no terminal, substituindo suas credenciais:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

### Código VS com Copilot do GitHub

Abra o Código VS `settings.json` pressionando **[!UICONTROL Ctrl+Shift+P]** ou **[!UICONTROL Cmd+Shift+P]** no macOS e selecionando **[!UICONTROL Preferências: Abrir Configurações de Usuário (JSON)]**. Adicione o exemplo a seguir:

```json
{
  "mcp": {
    "servers": {
      "marketo": {
        "type": "http",
        "url": "https://marketo-mcp.adobe.io/mcp",
        "headers": {
          "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
          "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
          "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
        }
      }
    }
  }
}
```

Pressione **[!UICONTROL Ctrl+Shift+P]** (ou **[!UICONTROL Cmd+Shift+P]** no macOS), digite **[!UICONTROL Recarregar Janela]** e pressione Enter.

>[!NOTE]
>
>Para fins de segurança, use a interpolação da variável de ambiente nos arquivos de configuração em vez de colar as credenciais diretamente. Você pode fazer referência a variáveis usando sintaxe como `${MARKETO_CLIENT_SECRET}` e defini-las em seu ambiente. Isso impede que as credenciais sejam armazenadas em texto sem formatação em arquivos que possam ser confirmados no controle de versão.

## Operações disponíveis

Depois de conectado, você pode solicitar que o assistente de IA execute operações nas seguintes categorias.

### Formulários

Procurar, criar, clonar e aprovar formulários. Adicione ou remova campos, configure regras de visibilidade de campo e identifique onde os formulários são incorporados.

Exemplo de prompts:

- &quot;Mostrar todos os formulários aprovados&quot;
- &quot;Clonar o formulário Fale conosco na pasta Q2 Campaign&quot;
- &quot;Adicionar um campo Empresa ao formulário de Solicitação de demonstração&quot;

### Campanhas inteligentes

Crie campanhas inteligentes, configure filtros de lista inteligente, adicione etapas de fluxo e ative ou desative campanhas.

Exemplo de prompts:

- &quot;Quais campanhas inteligentes estão ativas no momento?&quot;
- &quot;Criar uma nova campanha inteligente chamada Atualização de Pontuação de Cliente Potencial na pasta Operações&quot;
- &quot;Mostrar as etapas de fluxo na campanha de email de boas-vindas&quot;

### Clientes potenciais e listas

Localizar clientes potenciais por endereço de email, criar ou atualizar registros de clientes potenciais e gerenciar associação estática de listas.

Exemplo de prompts:

- &quot;Encontre o cliente em potencial com e-mail jane@example.com&quot;
- &quot;Adicionar ID de lead 12345 à lista Q2 MQL&quot;
- &quot;Criar uma nova lista estática chamada Participantes do Evento de Verão&quot;

### Programas

Crie, clone e marque programas. Procure programas por tipo, canal ou intervalo de datas.

Exemplo de prompts:

- &quot;Clonar o programa Q4 Webinar na pasta Eventos 2026&quot;
- &quot;Crie um novo programa de email chamado Vendas de Verão na pasta Campanhas&quot;
- &quot;Mostrar todos os programas marcados como Webinar&quot;

### Emails e trechos

Navegue por emails, crie emails de modelos, atualize seções de conteúdo e gerencie trechos reutilizáveis.

Exemplo de prompts:

- &quot;Mostrar todos os rascunhos de email&quot;
- &quot;Atualizar a seção de cabeçalho do email de boas-vindas&quot;
- &quot;Quais ativos usam o trecho promocional de feriado?&quot;

### Estrutura da instância

Navegue por pastas, canais, tipos de marcas e tipos de atividades para entender sua configuração do [!DNL Marketo].

Exemplo de prompts:

- &quot;Listar todas as pastas no Marketo&quot;
- &quot;Mostrar todos os canais disponíveis&quot;
- &quot;Quais tipos de tag são configurados?&quot;

### Operações em massa

Exportar dados de clientes potenciais em massa e verificar o status do trabalho de importação ou exportação.

Exemplo de prompts:

- &quot;Criar uma exportação em massa de clientes potenciais criados nos últimos 30 dias&quot;
- &quot;Verificar o status do trabalho de exportação xx&quot;

## Solução de problemas

| Erro | Causa | Corrigir |
| ------- | ------- | ----- |
| &quot;Endpoint do Marketo não fornecido&quot; | O cabeçalho `X-Marketo-Endpoint` está ausente da sua configuração. | Verifique novamente a configuração do MCP e confirme se todos os quatro cabeçalhos estão presentes. |
| &quot;Credenciais do Marketo não fornecidas&quot; | Um ou mais de `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` ou `X-Marketo-Munchkin-Id` está(ão) ausente(s). | Verifique se todos os quatro cabeçalhos estão presentes na sua configuração. |
| &quot;Erro de autenticação&quot; | Suas credenciais são inválidas ou expiraram. | Verifique novamente a ID do Cliente e o Segredo do Cliente em **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. |
| &quot;403 Proibido&quot; | Sua Munchkin ID não está no incluo na lista de permissões do servidor. | Contate o administrador do MCP [!DNL Marketo] para adicionar sua Munchkin ID. |
| Tempo limite de conexão ou recusado | O servidor MCP está inacessível na sua rede. | Confirme se você pode acessar o URL do servidor do seu ambiente. Verifique os requisitos de VPN, se aplicável. |
| As chamadas de ferramenta retornam resultados vazios | O usuário da API não tem permissões para o tipo de ativo solicitado. | Peça ao administrador do [!DNL Marketo] para revisar a função de usuário e as permissões da API. |

## Perguntas frequentes

### Meus dados estão seguros?

As credenciais são transmitidas em cabeçalhos HTTP com cada solicitação individual. O servidor não armazena ou armazena em cache credenciais entre sessões e cada solicitação é totalmente isolada.

### Várias pessoas podem usar isso ao mesmo tempo?

Sim. O servidor tem vários locatários. Cada usuário se conecta com suas próprias credenciais e as solicitações são isoladas umas das outras.

### O que acontece se meu token de acesso expirar?

Ao autenticar usando a ID do cliente e o segredo do cliente, o servidor manipula a atualização do token automaticamente. Você não precisa realizar nenhuma ação.

### Preciso instalar ou executar algo?

Não. O servidor MCP é hospedado pela Adobe. Você só precisa configurar a ferramenta de IA para se conectar a ela.

### De quais [!DNL Marketo] permissões meu usuário da API precisa?

O usuário da API precisa acessar os tipos de ativos que você pretende gerenciar. No mínimo, atribua uma função Somente leitura para operações de navegação e uma função Leitura e gravação para criar ou modificar ativos. Trabalhe com o administrador do [!DNL Marketo] para atribuir as permissões apropriadas.

### Quais são os limites de taxa?

O servidor MCP herda os limites de taxa da API da instância do Marketo. Use um usuário de API dedicado para rastrear e gerenciar o consumo de cotas.

### Quais ferramentas de IA são compatíveis?

Claude Desktop, Cursor, Claude Code (CLI) e Código VS com o GitHub Copilot. Qualquer ferramenta de IA compatível com o Protocolo de contexto de modelo por HTTP deve funcionar.

### Posso me conectar a várias instâncias do [!DNL Marketo]?

Sim. Adicione várias entradas na configuração MCP da ferramenta de IA, cada uma com um nome exclusivo e as credenciais para a instância correspondente. Por exemplo, você pode configurar o `marketo-prod` e o `marketo-staging` como servidores separados.

## Considerações de segurança

>[!IMPORTANT]
>
>Use um usuário da API dedicado no [!DNL Marketo] com apenas as permissões necessárias para seu trabalho. Não reutilize credenciais de administrador para acessar a API.

- **Credenciais por solicitação.** A ID do cliente, o Segredo do cliente, a ID do Munchkin e o endpoint da API REST são transmitidos em cabeçalhos HTTP com cada solicitação. O servidor não os armazena ou armazena em cache.
- **Isolamento de vários locatários.** Cada solicitação usa seu próprio conjunto de credenciais. Seus dados não fazem interseção com a sessão de nenhum outro usuário.
- **incluo na lista de permissões de Munchkin ID.** O servidor só aceita solicitações para [!DNL Marketo] instâncias aprovadas. As solicitações que usam uma Munchkin ID não autorizada são rejeitadas com um erro 403.
- **Manter credenciais fora do controle de versão.** Use a interpolação de variável de ambiente (`${MARKETO_CLIENT_SECRET}`) se a ferramenta de IA permitir, de modo que as credenciais não sejam armazenadas em texto sem formatação em arquivos confirmados em um repositório.
