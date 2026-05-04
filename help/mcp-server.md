---
title: Servidor MCP
description: Saiba como conectar um assistente de IA ao Marketo usando o servidor MCP. Configure o Claude Desktop, o Cursor, o Código Claude ou o Código VS com suas credenciais do Marketo.
badgeBeta: label="Beta" type="informative" tooltip="No momento, esse recurso está em uma versão beta fechada"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
source-git-commit: 738fabea9eefbc6aafee4ffa7972c9e3e2bd430c
workflow-type: tm+mt
source-wordcount: '1478'
ht-degree: 0%

---

# [!DNL Marketo] Servidor MCP

>[!NOTE]
>
>O servidor MCP está atualmente em uma versão beta fechada. Não está disponível para todos os usuários no momento.

O protocolo de contexto de modelo (MCP) é um padrão aberto que permite que as ferramentas de IA se comuniquem com serviços externos. O servidor MCP [!DNL Marketo] atua como uma ponte entre o assistente de IA e o [!DNL Marketo]. Ele expõe mais de 100 operações em formulários, programas, campanhas inteligentes, leads, emails, trechos, listas e pastas.

Quando a ferramenta de IA chama o servidor MCP, o servidor executa a chamada à API REST correspondente em seu nome, usando as credenciais fornecidas em cada solicitação. Você não precisa instalar, implantar nem executar nenhum software do lado do servidor.

>[!IMPORTANT]
>
>O protocolo de contexto de modelo (MCP) é um padrão de código aberto emergente e pode apresentar riscos de segurança ou confiabilidade. As integrações do servidor Adobe MCP e a documentação relacionada são fornecidas &quot;no estado em que se encontram&quot;, sem garantias de nenhum tipo.
>Conectar clientes ou servidores MCP a produtos Adobe é uma configuração escolhida pelo cliente, e os clientes são responsáveis por avaliar a segurança e a adequação de qualquer integração MCP. O Adobe não é responsável por problemas resultantes de configuração incorreta, uso incorreto do MCP, vulnerabilidades em implementações de terceiros ou ações não intencionais executadas por meio de fluxos de trabalho habilitados para MCP.
>Para reduzir os riscos, a Adobe incentiva o teste de integrações em um ambiente de sandbox antes do uso produtivo e a análise e validação cuidadosas de todas as ações e respostas iniciadas pelo MCP antes de confirmar ou confiar nelas.

## Noções básicas sobre MCP

>Pense no MCP como uma porta USB-C para aplicativos de IA. Assim como o USB-C oferece uma maneira padronizada para conectar seus dispositivos a vários periféricos e acessórios, o MCP fornece uma maneira padronizada para conectar modelos de IA a diferentes fontes de dados e ferramentas. — [Protocolo de Contexto de Modelo](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

O MCP permite que uma ferramenta de IA se conecte a vários serviços externos ao mesmo tempo. Por exemplo, um assistente de IA pode:

* Conectar-se a um processador de texto para geração de documentos assistidos por IA
* Conecte-se a aplicativos de modelagem 3D, como o Blender, para criar animações
* Conecte-se ao After Effects para edição de vídeo

O MCP é um protocolo de comunicação — um padrão aberto que qualquer aplicativo pode implementar para expor seus dados e ações às ferramentas de IA.

## O que o MCP [!DNL Marketo] faz ou não faz

Compreender o escopo do MCP ajuda a definir expectativas antes de conectar sua ferramenta de IA.

**O MCP faz:**

* Fornecer acesso aos dados e recursos do [!DNL Marketo] por meio de APIs REST padrão
* Executar chamadas de API em seu nome usando credenciais fornecidas com cada solicitação
* Suporte a vários usuários simultâneos, cada um conectado com suas próprias credenciais
* Manipule a atualização automática do token OAuth — não é necessário gerenciar a expiração do token
* Operar em ambientes isolados de locatários para que seus dados nunca cruzem com a sessão de outro usuário

**O MCP não:**

* Usar, hospedar ou executar qualquer modelo de IA ou de aprendizado de máquina — todo o processamento de IA acontece na ferramenta de IA, não no MCP
* Treine ou aprenda com quaisquer dados, incluindo os dados do cliente
* Gerar previsões, recomendações ou decisões — a tomada de decisões é responsabilidade da ferramenta ou do usuário downstream da IA
* Armazenar ou reter credenciais, dados de solicitação ou estado de sessão entre solicitações
* Exigir a instalação, implantação ou gerenciamento de qualquer software do lado do servidor

## Pré-requisitos

* Uma instância [!DNL Marketo] com acesso à API REST habilitado
* Acesso de administrador para criar credenciais de API no [!DNL Marketo] LaunchPoint
* Uma das seguintes ferramentas de IA: Claude Desktop, Cursor, Claude Code (CLI) ou VS Code com o GitHub Copilot
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

Cada ferramenta de IA lê a configuração do servidor MCP de um local diferente. Encontre sua ferramenta abaixo e siga as etapas para adicionar o servidor MCP [!DNL Marketo].

>[!TIP]
>
>Para conectar a várias instâncias do [!DNL Marketo], adicione entradas separadas na configuração do MCP com nomes exclusivos — por exemplo, `marketo-prod` e `marketo-staging` — cada uma com as credenciais correspondentes.

### Claude Desktop

Arquivo de configuração `claude_desktop_config.json`. Abra-o em um destes locais:

* **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
* **Janelas**: `%APPDATA%\Claude\claude_desktop_config.json`

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

Pressione **[!UICONTROL Ctrl+Shift+P]** (ou **[!UICONTROL Cmd+Shift+P]** no macOS), digite **[!UICONTROL MCP: Abrir Configuração de Usuário]** e pressione Enter. Isso abre `mcp.json`. Adicionar a entrada `marketo` dentro do objeto `servers`:

```json
{
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
```

>[!NOTE]
>
>Para fins de segurança, use a interpolação da variável de ambiente nos arquivos de configuração em vez de colar as credenciais diretamente. Você pode fazer referência a variáveis usando sintaxe como `${MARKETO_CLIENT_SECRET}` e defini-las em seu ambiente. Isso impede que as credenciais sejam armazenadas em texto sem formatação em arquivos que possam ser confirmados no controle de versão.

## Operações disponíveis

Depois de conectado, você pode solicitar que o assistente de IA execute operações nas seguintes categorias.

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

Localizar clientes potenciais por endereço de email, criar ou atualizar registros de clientes potenciais e gerenciar associação estática de listas.

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

Navegue por pastas, canais, tipos de marcas e tipos de atividades para entender sua configuração do [!DNL Marketo].

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
| &quot;Credenciais do Marketo não fornecidas&quot; | Um ou mais de `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` ou `X-Marketo-Munchkin-Id` está(ão) ausente(s). | Verifique se todos os quatro cabeçalhos estão presentes na sua configuração. |
| &quot;Erro de autenticação&quot; | Suas credenciais são inválidas ou expiraram. | Verifique novamente a ID do Cliente e o Segredo do Cliente em **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. |
| &quot;403 Proibido&quot; | Sua Munchkin ID não está no incluo na lista de permissões do servidor. | Contate o administrador do MCP [!DNL Marketo] para adicionar sua Munchkin ID. |
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
* **Manter credenciais fora do controle de versão.** Use a interpolação de variável de ambiente (`${MARKETO_CLIENT_SECRET}`) se a ferramenta de IA permitir, de modo que as credenciais não sejam armazenadas em texto sem formatação em arquivos confirmados em um repositório.
