---
title: Servidor MCP do Marketo Engage
description: Saiba como conectar um assistente de IA ao Marketo usando o servidor MCP do Marketo Engage. Configure o Claude Desktop, o Cursor, o Código Claude ou o Código VS com suas credenciais do Marketo.
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
    internal-label: Integrations
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
    internal-label: Configuration
  - id: c2dbad80-0f5c-4d96-a798-2a65f93b8721
    internal-label: Assets
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
    internal-label: Templates
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '2620'
ht-degree: 0%
---

# [!DNL Marketo Engage] Servidor MCP

O Protocolo de contexto de modelo (MCP) é um padrão aberto que conecta ferramentas de IA a serviços externos. O servidor MCP [!DNL Marketo] conecta seu assistente de IA ao [!DNL Marketo]. Ele oferece suporte a operações em gerenciamento de ativos, dados de clientes potenciais e automação de campanhas. Consulte [Operações MCP com suporte](mcp-server-operations.md) para obter as exceções e os requisitos de disponibilidade.

Quando a ferramenta de IA chama o servidor MCP, o servidor usa as credenciais nessa solicitação para executar a chamada de API REST correspondente. Não é necessário instalar, implantar ou executar software do lado do servidor.

Para obter mais informações sobre como os dados são tratados com a IA do Marketo e o servidor MCP do Marketo Engage, consulte a página [Informações de Dados](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/marketo-ai/data-information).

>[!IMPORTANT]
>
>O protocolo de contexto de modelo (MCP) é um padrão de código aberto emergente e pode apresentar riscos de segurança ou confiabilidade. As integrações do servidor Adobe MCP e a documentação relacionada são fornecidas &quot;no estado em que se encontram&quot;, sem garantias de nenhum tipo.
>Conectar clientes ou servidores MCP a produtos Adobe é uma configuração escolhida pelo cliente, e os clientes são responsáveis por avaliar a segurança e a adequação de qualquer integração MCP. O Adobe não é responsável por problemas resultantes de configuração incorreta, uso incorreto do MCP, vulnerabilidades em implementações de terceiros ou ações não intencionais executadas por meio de fluxos de trabalho habilitados para MCP.
>Para reduzir os riscos, a Adobe incentiva o teste de integrações em um ambiente de sandbox antes do uso produtivo e a análise e validação cuidadosas de todas as ações e respostas iniciadas pelo MCP antes de confirmar ou confiar nelas.

## Noções básicas sobre MCP

>Pense no MCP como uma porta USB-C para aplicativos de IA. O USB-C fornece uma maneira padronizada para conectar seus dispositivos a vários periféricos e acessórios, e o MCP fornece uma maneira padronizada para conectar modelos de IA a fontes de dados e ferramentas. — [Protocolo de Contexto de Modelo](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

O MCP permite que uma ferramenta de IA se conecte a vários serviços externos simultaneamente. Por exemplo, um assistente de IA pode:

* Conectar-se a um processador de texto para geração de documentos assistidos por IA
* Conecte-se a ferramentas de animação, como o Blender, para criar visualizações
* Conecte-se ao Adobe After Effects para edição de vídeo

Qualquer aplicativo pode implementar o MCP para expor dados e ações a ferramentas de IA.

## O que o MCP [!DNL Marketo Engage] faz ou não faz

Compreender o escopo do MCP ajuda a definir expectativas antes de conectar sua ferramenta de IA.

**O MCP faz:**

* Fornecer acesso aos dados e recursos do [!DNL Marketo] por meio de APIs REST padrão
* Executar chamadas de API em seu nome usando credenciais fornecidas com cada solicitação
* Suporte a vários usuários simultâneos, cada um conectado com suas próprias credenciais
* Manipule a atualização automática do token OAuth. Não é necessário gerenciar a expiração do token
* Operar em ambientes isolados de locatários para que seus dados nunca cruzem com a sessão de outro usuário

**O MCP não:**

* Use, hospede ou execute modelos de aprendizado de máquina ou IA. Todo o processamento de IA acontece na ferramenta de IA, não no MCP
* Treine ou aprenda com quaisquer dados, incluindo os dados do cliente
* Gerar previsões, recomendações ou decisões. A tomada de decisões é da responsabilidade do utilizador ou da ferramenta de IA a jusante
* Armazenar ou reter credenciais, dados de solicitação ou estado de sessão entre solicitações
* Exigir a instalação, implantação ou gerenciamento de qualquer software do lado do servidor

O MCP pode transmitir dados, incluindo campos potencialmente confidenciais, dependendo do uso da API, mas os dados B2B envolvem dados de negócios do cliente e não envolvem dados PII.

## Pré-requisitos

* Uma instância [!DNL Marketo] com acesso à API REST habilitado
* Acesso de administrador para criar credenciais de API no [!DNL Marketo] LaunchPoint
* Acesso de rede à URL do servidor MCP: `https://marketo-mcp.adobe.io/mcp`

## Obter credenciais do Marketo

Você precisa dos seguintes valores da sua instância [!DNL Marketo]:

* **ID do cliente**
* **Segredo do cliente**
* **ID da Conta da Munchkin**

Se já os tiver, pule para [Configurar a ferramenta de IA](#configure-your-ai-tool).

### ID do cliente e segredo do cliente

1. Vá para **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Selecione seu serviço de API. Se você não tiver um, selecione **[!UICONTROL Novo]** > **[!UICONTROL Novo serviço]**, escolha **[!UICONTROL Personalizado]** como o tipo de serviço e atribua um usuário de API dedicado.
1. Selecione **[!UICONTROL Exibir Detalhes]** e copie os valores de **[!UICONTROL ID do Cliente]** e **[!UICONTROL Segredo do Cliente]**.

### ID da conta do Munchkin

1. Vá para **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Copie a **[!UICONTROL ID da Conta da Munchkin]**. O formato é `XXX-XXX-XXX` e corresponde ao prefixo da URL da instância.

## Configurar a ferramenta de IA

As configurações diferem levemente dependendo da ferramenta. Esta seção fornece exemplos de conexão para ferramentas comuns.

>[!TIP]
>
>Para se conectar a várias instâncias do [!DNL Marketo], adicione entradas separadas na configuração do MCP com nomes exclusivos: `marketo-prod` e `marketo-staging`, cada uma com as credenciais correspondentes.

>[!BEGINTABS]

>[!TAB Claude Desktop]

### Claude Desktop {#claude-desktop}

Para se conectar ao Claude Desktop, baixe o [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) e descompacte-o. Coloque `marketo-mcp-bridge.mjs` em um local conhecido e use seu caminho absoluto na configuração abaixo.

Você também precisa de uma versão LTS suportada do Node.js instalada em seu computador. A ponte não requer pacotes npm adicionais.

1. Abra o Claude Desktop.
1. Abrir `claude_desktop_config.json`. No macOS, o arquivo está em `~/Library/Application Support/Claude/`. No Windows, ele está em `%APPDATA%\Claude\`. Em versões com o menu de configurações do desenvolvedor, selecione **[!UICONTROL Configurações]** > **[!UICONTROL Desenvolvedor]** > **[!UICONTROL Editar Configuração]** para abri-lo.
1. Adicione a seguinte entrada em `mcpServers`, preservando quaisquer configurações existentes. Substitua o caminho e os espaços reservados da credencial por seus valores:

```json
{
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/marketo-mcp-bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Reinicie o Claude Desktop.

Se o Claude Desktop não puder encontrar o Node.js, substitua `node` em `command` pelo caminho absoluto para o executável do Node.js. Para obter mais informações, consulte [Conectar a servidores MCP locais](https://modelcontextprotocol.io/docs/develop/connect-local-servers){target="_blank"}.

>[!TAB Cursor]

### Cursor {#cursor}

Adicione a entrada `marketo` em `mcpServers` em `~/.cursor/mcp.json` para sua conta de usuário ou `.cursor/mcp.json` no diretório do projeto. Preserve todos os servidores existentes e substitua os espaços reservados para credenciais por seus valores:

```json
{
  "mcpServers": {
    "marketo": {
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

Abra **[!UICONTROL Personalizar]** no Cursor e confirme se o servidor está ligado. Para obter mais informações, consulte [Configuração do MCP do cursor](https://cursor.com/docs/context/mcp){target="_blank"}.

>[!TAB CLI do código Claude]

### Código Claude (CLI) {#claude-code}

Execute o seguinte comando no terminal, substituindo suas credenciais:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

O comando adiciona o servidor para o projeto atual e sua conta de usuário. Para disponibilizá-lo em projetos, adicione `--scope user`. No Código Claude, execute `/mcp` para verificar a conexão. Para obter mais informações, consulte [configuração do MCP do Claude Code](https://code.claude.com/docs/en/mcp){target="_blank"}.

>[!TAB OpenAI Codex]

### OpenAI Codex {#codex}

Adicione o seguinte a `~/.codex/config.toml`, preservando qualquer configuração existente:

```toml
[mcp_servers.marketo]
url = "https://marketo-mcp.adobe.io/mcp"

[mcp_servers.marketo.env_http_headers]
X-Marketo-Client-Id = "MARKETO_CLIENT_ID"
X-Marketo-Client-Secret = "MARKETO_CLIENT_SECRET"
X-Marketo-Munchkin-Id = "MARKETO_MUNCHKIN_ID"
```

Defina `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` e `MARKETO_MUNCHKIN_ID` no ambiente do processo que executa o Codex. Os valores em `env_http_headers` são nomes de variáveis de ambiente, não credenciais.

Inicie uma nova sessão do Codex e execute `/mcp` para verificar a conexão. Para obter mais informações, consulte [Configuração do Codex MCP](https://developers.openai.com/codex/mcp){target="_blank"}.

>[!TAB CLI do Gemini]

### CLI do Gemini

Para adicionar o servidor MCP [!DNL Marketo Engage] à CLI do Gemini, adicione a seguinte entrada `mcpServers` a `.gemini/settings.json` no diretório do projeto ou `~/.gemini/settings.json` para a conta de usuário. Preserve todas as configurações existentes e substitua os espaços reservados das credenciais por seus valores.

```json
{
  "mcpServers": {
    "marketo": {
      "httpUrl": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Como alternativa, defina `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` e `MARKETO_MUNCHKIN_ID` em seu ambiente de shell e execute o seguinte comando. O shell expande as variáveis antes que a CLI armazene os valores do cabeçalho na configuração do usuário:

```bash
gemini mcp add --transport http -s user marketo https://marketo-mcp.adobe.io/mcp \
  -H "X-Marketo-Client-Id: $MARKETO_CLIENT_ID" \
  -H "X-Marketo-Client-Secret: $MARKETO_CLIENT_SECRET" \
  -H "X-Marketo-Munchkin-Id: $MARKETO_MUNCHKIN_ID"
```

Reinicie a sessão e execute `/mcp` para verificar a conexão. Para obter mais informações, consulte [Configuração de MCP Gemini CLI](https://geminicli.com/docs/tools/mcp-server/){target="_blank"}.

>[!TAB Código VS com GitHub Copilot]

### Código VS com Copilot do GitHub {#vscode}

Use a configuração portátil do Copilot para as versões mais recentes do código VS. Adicione a seguinte entrada em `mcpServers` em `~/.copilot/mcp-config.json`, ou `$COPILOT_HOME/mcp-config.json` se você definir `COPILOT_HOME`. Preservar os servidores existentes:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "${MARKETO_CLIENT_ID}",
        "X-Marketo-Client-Secret": "${MARKETO_CLIENT_SECRET}",
        "X-Marketo-Munchkin-Id": "${MARKETO_MUNCHKIN_ID}"
      }
    }
  }
}
```

Defina as três variáveis de ambiente no ambiente que executa o código VS ou no host remoto que se conecta ao servidor. Inicie uma nova sessão do Copilot e revise todos os prompts de confiança do servidor.

Para usar a configuração guiada, execute **[!UICONTROL MCP: Adicionar Servidor]**, selecione **[!UICONTROL HTTP (HTTP ou Eventos Enviados pelo Servidor)]**, insira a URL do servidor e escolha **[!UICONTROL Global]** > **[!UICONTROL Copilot Global]**. Em seguida, adicione os cabeçalhos à configuração salva.

As versões anteriores também oferecem suporte a **[!UICONTROL MCP: Abrir Configuração de Usuário]** ou `.vscode/mcp.json`. Esses arquivos usam a sintaxe `servers` em vez de `mcpServers` e `${env:MARKETO_CLIENT_ID}` para variáveis de ambiente. Não misture os dois formatos. Para obter mais informações, consulte [Configuração de MCP do VS Code](https://code.visualstudio.com/docs/agents/reference/mcp-configuration){target="_blank"}.

>[!TAB Copilot Studio]

### Copilot Studio

Ative a orquestração gerativa para seu agente antes de adicionar ferramentas MCP. Use um conector personalizado para definir todos os três cabeçalhos [!DNL Marketo] necessários.

Na página **[!UICONTROL Ferramentas]** do seu agente, selecione **[!UICONTROL Adicionar uma ferramenta]** > **[!UICONTROL Nova ferramenta]** > **[!UICONTROL Conector personalizado]**. No Power Apps, crie um conector personalizado e use o **[!UICONTROL editor do Swagger]** para inserir a seguinte definição:

```yaml
swagger: '2.0'
info:
  title: Marketo MCP Server
  description: Connect to the Marketo MCP server.
  version: 1.0.0
host: marketo-mcp.adobe.io
basePath: /
schemes:
  - https
paths:
  /mcp:
    post:
      summary: Marketo MCP Server
      description: Invoke the Marketo MCP server.
      operationId: InvokeServer
      x-ms-agentic-protocol: mcp-streamable-1.0
      parameters:
        - name: X-Marketo-Client-Id
          in: header
          description: Client ID.
          type: string
          required: true
        - name: X-Marketo-Client-Secret
          in: header
          description: Client secret.
          type: string
          required: true
        - name: X-Marketo-Munchkin-Id
          in: header
          description: Munchkin ID.
          type: string
          required: true
      responses:
        '200':
          description: Immediate Response
securityDefinitions: {}
security: []
```

Salve ou atualize o conector, retorne ao Copilot Studio e adicione-o ao seu agente. Crie uma conexão e forneça os três valores de cabeçalho quando solicitado. Mantenha as credenciais fora da definição de OpenAPI e das instruções do agente. Para obter mais informações, consulte [Conectar um agente a um servidor MCP existente](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-add-existing-server-to-agent){target="_blank"}.

>[!TAB Obter]

### Glean {#glean}

Para conectar o Glean ao servidor MCP [!DNL Marketo Engage], siga as [instruções de configuração do MCP remoto do Glean](https://docs.glean.com/administration/tools/connect-remote-mcp-servers-to-glean){target="_blank"} para adicionar `https://marketo-mcp.adobe.io/mcp`. Verifique primeiro o catálogo **[!UICONTROL Conectores]**. Para um servidor personalizado ou não listado, use o **[!UICONTROL Admin Console]** > **[!UICONTROL Platform]** > **[!UICONTROL Tools]** > **[!UICONTROL Import tools from MCP server]**.

A [equipe de suporte do Glean deve configurar os cabeçalhos personalizados](https://docs.glean.com/administration/tools/mcp-custom-headers){target="_blank"} abaixo. Inclua o nome do servidor, o URL e os nomes de cabeçalho necessários em sua solicitação de suporte e coordene o tratamento seguro dos valores da credencial. Uma conexão de chave de API padrão sozinha não fornece os três cabeçalhos.

| Header | Valor |
| ------ | ----- |
| `X-Marketo-Client-Id` | Sua ID do cliente |
| `X-Marketo-Client-Secret` | Segredo do cliente |
| `X-Marketo-Munchkin-Id` | Sua ID de conta da Munchkin |

Depois que o Glean confirmar que os cabeçalhos estão ativos, teste a conexão com uma solicitação somente leitura antes de disponibilizar as ferramentas para outros usuários.

>[!TAB Outras ferramentas]

### Outras ferramentas {#other-tools}

O Adobe hospeda o servidor MCP [!DNL Marketo] e o expõe em uma URL pública. Qualquer cliente MCP que suporte servidores remotos por meio de transporte HTTP eficiente pode se conectar a ele.
Os clientes que oferecem suporte a conexões HTTP remotas diretas não precisam de uma ponte ou de um software de servidor instalado localmente. Se sua ferramenta não estiver listada acima, use os detalhes de conexão abaixo para configurá-la manualmente.

**Detalhes da conexão:**

| Configuração | Valor |
| ------- | ----- |
| Transporte | HTTP (HTTP dinamizável) |
| URL do servidor | `https://marketo-mcp.adobe.io/mcp` |

**Cabeçalhos de autenticação:**

Envie todos os três cabeçalhos com cada solicitação. O local em que você insere o URL e os cabeçalhos do servidor depende da ferramenta. Consulte a documentação do MCP.

| Header | Valor |
| ------ | ----- |
| `X-Marketo-Client-Id` | Sua ID do cliente |
| `X-Marketo-Client-Secret` | Segredo do cliente |
| `X-Marketo-Munchkin-Id` | Sua ID de conta da Munchkin |

Se a ferramenta aceitar uma configuração JSON, comece com os exemplos de [Cursor](#cursor) ou [Código VS](#vscode) e ajuste as chaves e a sintaxe da variável de ambiente para corresponder ao esquema da ferramenta.

>[!ENDTABS]

Após a conexão, peça ao seu assistente de IA para usar o servidor MCP [!DNL Marketo] para listar os canais disponíveis sem alterar os dados. Inspecione a chamada de ferramenta e sua resposta para confirmar se a solicitação usa o servidor desejado e retorna os dados sem um erro de conexão ou autenticação.

## Operações disponíveis

Depois de conectado, você pode solicitar que o assistente de IA execute operações nas seguintes categorias. Para obter as exceções e os requisitos de disponibilidade, consulte [Operações MCP suportadas](mcp-server-operations.md).

### Formulários

Procurar, criar, clonar e aprovar formulários. Adicione ou remova campos, configure regras de visibilidade de campo e identifique onde os formulários são incorporados.

Exemplo de prompts:

* &quot;Mostrar todos os formulários aprovados&quot;
* &quot;Clonar o formulário Fale conosco na pasta Q2 Campaign&quot;
* &quot;Adicionar um campo Empresa ao formulário de Solicitação de demonstração&quot;

### Campanhas inteligentes

Crie campanhas inteligentes, configure filtros de lista inteligente, adicione etapas de fluxo e ative ou desative campanhas.

Exemplo de prompts:

* &quot;Quais campanhas inteligentes estão ativas no momento?&quot;
* &quot;Criar uma nova campanha inteligente chamada Atualização de Pontuação de Cliente Potencial na pasta Operações&quot;
* &quot;Mostrar as etapas de fluxo na campanha de email de boas-vindas&quot;

### Clientes potenciais e listas

Localizar clientes potenciais por endereço de email, criar registros de clientes potenciais e gerenciar associação estática de listas.

Exemplo de prompts:

* &quot;Encontre o cliente em potencial com e-mail jane@example.com&quot;
* &quot;Adicionar ID de lead 12345 à lista Q2 MQL&quot;
* &quot;Criar uma nova lista estática chamada Participantes do Evento de Verão&quot;

### Programas

Crie, clone e marque programas. Procure programas por tipo, canal ou intervalo de datas.

Exemplo de prompts:

* &quot;Clonar o programa Q4 Webinar na pasta Eventos 2026&quot;
* &quot;Crie um novo programa de email chamado Vendas de Verão na pasta Campanhas&quot;
* &quot;Mostrar todos os programas marcados como Webinar&quot;

### Emails e trechos

Navegue por emails, crie emails de modelos, atualize seções de conteúdo e gerencie trechos reutilizáveis.

Exemplo de prompts:

* &quot;Mostrar todos os rascunhos de email&quot;
* &quot;Atualizar a seção de cabeçalho do email de boas-vindas&quot;
* &quot;Quais ativos usam o trecho promocional de feriado?&quot;

### Estrutura da instância

Para entender sua configuração do [!DNL Marketo], navegue por pastas, canais, tipos de tags e tipos de atividades.

Exemplo de prompts:

* &quot;Listar todas as pastas no Marketo&quot;
* &quot;Mostrar todos os canais disponíveis&quot;
* &quot;Quais tipos de tag são configurados?&quot;

### Operações em massa

Exportar dados de clientes potenciais em massa e verificar o status do trabalho de importação ou exportação.

Exemplo de prompts:

* &quot;Criar uma exportação em massa de clientes potenciais criados nos últimos 30 dias&quot;
* &quot;Verificar o status do trabalho de exportação xx&quot;

## Solução de problemas

| Erro | Causa | Corrigir |
| ------- | ------- | ----- |
| &quot;Credenciais do Marketo não fornecidas&quot; | Um ou mais de `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` ou `X-Marketo-Munchkin-Id` está(ão) ausente(s). | Verifique se todos os cabeçalhos de credencial de cliente Marketo estão presentes em sua configuração. |
| &quot;401 Não autorizado&quot; | Suas credenciais estão ausentes, inválidas ou expiradas. Com as credenciais do cliente Marketo, a ID do cliente ou o Segredo do cliente está incorreto. Com um token IMS, o token é inválido ou expirou. | Verifique as credenciais do método de autenticação. Para obter as credenciais do cliente, verifique novamente a **[!UICONTROL ID do Cliente]** e o **[!UICONTROL Segredo do Cliente]** em **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. Para um token IMS, gere um novo token e atualize o cabeçalho `Authorization`. |
| &quot;403 Proibido&quot; | Suas credenciais são válidas, mas a instância [!DNL Marketo] não está habilitada para acesso MCP. | Contate o administrador do MCP [!DNL Marketo] para habilitar o acesso ao MCP para sua ID de Conta da Munchkin. |
| &quot;Muitas solicitações&quot; (limite de taxa) | Você enviou muitas solicitações em um curto período ou muitas solicitações ao mesmo tempo e atingiu os limites da API da instância [!DNL Marketo]. | Reduza a frequência e o número de solicitações enviadas de uma só vez e aguarde um pouco antes de tentar novamente. Use um usuário de API dedicado para rastrear e gerenciar sua cota. |
| Tempo limite de conexão ou recusado | O servidor MCP está inacessível na sua rede. | Confirme se você pode acessar o URL do servidor do seu ambiente. Verifique os requisitos de VPN, se aplicável. |
| As chamadas de ferramenta retornam resultados vazios | O usuário da API não tem permissões para o tipo de ativo solicitado. | Peça ao administrador do [!DNL Marketo] para revisar a função de usuário e as permissões da API. |

## Considerações de segurança

>[!IMPORTANT]
>
>Use um usuário da API dedicado no [!DNL Marketo] com apenas as permissões necessárias para seu trabalho. Não reutilize credenciais de administrador para acessar a API.

* **Credenciais por solicitação.** A ID do cliente, o Segredo do cliente, a ID do Munchkin e o endpoint da API REST são transmitidos em cabeçalhos HTTP com cada solicitação. O servidor não os armazena ou armazena em cache.
* **Isolamento de vários locatários.** Cada solicitação usa seu próprio conjunto de credenciais. Seus dados não fazem interseção com a sessão de nenhum outro usuário.
* **incluo na lista de permissões de Munchkin ID.** O servidor só aceita solicitações para [!DNL Marketo] instâncias aprovadas. As solicitações que usam uma Munchkin ID não autorizada são rejeitadas com um erro 403.
* **Limites de taxa de API.** O servidor MCP herda os limites de taxa da API da sua instância [!DNL Marketo]. Use um usuário de API dedicado para rastrear e gerenciar o consumo de cotas.
* **Manter credenciais fora do controle de versão.** Use a interpolação de variável de ambiente (`${MARKETO_CLIENT_SECRET}`) se a ferramenta de IA permitir, de modo que as credenciais não sejam armazenadas em texto sem formatação nos arquivos do repositório.

## Governança e retenção de dados

### Manuseio de credenciais

* As credenciais do cliente não são persistentes no lado do servidor e são fornecidas pelo cliente por solicitação, o que ajuda a limitar a exposição da credencial no serviço.

### Modelo de interação de API

* Uso do agente: os agentes podem usar o servidor MCP para chamar APIs do Marketo compatíveis.
* Alinhamento do modelo de autenticação: o serviço usa o mesmo modelo de autenticação de API externa documentado para APIs do Marketo.

### Autenticação e autorização

* Privilégio mínimo: as permissões efetivas são herdadas do usuário somente API do Marketo atribuído ao serviço LaunchPoint do cliente, permitindo a administração do privilégio mínimo na configuração Marketo do cliente.
* Nenhuma persistência de token do lado do servidor: o serviço continua a evitar o armazenamento do lado do servidor de credenciais ou tokens do cliente.  

### Registro e monitoramento

* Registro de segurança: os registros JSON estruturados são roteados por meio do Fluent Bit para o Splunk, com mascaramento de dados confidenciais e filtragem adicional para oferecer suporte aos requisitos de conformidade.
* Suporte de auditoria: esses controles oferecem suporte ao monitoramento contínuo da disponibilidade de serviços, eventos relevantes para a segurança e qualidade operacional.
* Nenhum armazenamento secreto do lado do servidor: as credenciais do cliente não são armazenadas pela implantação do MCP e devem ser fornecidas pelos clientes por solicitação.
* Manuseio de token: os tokens de acesso têm vida curta, as respostas de token são marcadas como sem armazenamento e os tokens são aceitos por meio de mecanismos de autorização padrão em vez de transmissão de sequência de consulta.
* Acesso operacional com base em função: o acesso à implantação administrativa é regido pelas funções da infraestrutura do Adobe e pelos controles com base em grupo, enquanto as permissões do plano de dados são herdadas da configuração do usuário da API do Marketo do cliente.
* Auditoria e capacidade de observação: registro, mascaramento, monitoramento e alertas de segurança estão habilitados para oferecer suporte à investigação, ao rastreamento da integridade do serviço e à supervisão operacional.
