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
source-git-commit: 5c356507ac2551edfbb1c60dba7b167d7ef0b4c0
workflow-type: tm+mt
source-wordcount: '2137'
ht-degree: 0%
---

# [!DNL Marketo Engage] Servidor MCP

O Protocolo de contexto de modelo (MCP) é um padrão aberto que conecta ferramentas de IA a serviços externos. O servidor MCP [!DNL Marketo] conecta seu assistente de IA ao [!DNL Marketo]. Ele fornece mais de 100 operações para formulários, programas, campanhas inteligentes, leads, emails, trechos, listas e pastas.

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

A configuração é ligeiramente diferente, dependendo da ferramenta. Veja a seguir exemplos de conexão para ferramentas comuns.

>[!TIP]
>
>Para se conectar a várias instâncias do [!DNL Marketo], adicione entradas separadas na configuração do MCP com nomes exclusivos: `marketo-prod` e `marketo-staging`, cada uma com as credenciais correspondentes.

>[!BEGINTABS]

>[!TAB Claude Desktop]

### Claude Desktop {#claude-desktop}

Para se conectar ao Claude Desktop, baixe o [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) e descompacte-o. Coloque `marketo-mcp-bridge.mjs` em um local conhecido para que você possa consultar na próxima etapa.

Você também precisará de:

* Node.js v18+
* npm

1. Abra o Claude Desktop.
1. Vá para **Configurações > Desenvolvedor > Editar configuração**.
1. Adicionar o seguinte a `claude_desktop_config.json`:

```json
{
  "preferences": {
    ...
  },
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/bridge.mjs"],
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

>[!TAB Cursor]

### Cursor {#cursor}

Se a configuração do MCP do cursor já contiver outros servidores, adicione a entrada `marketo` em `mcpServers`.
O exemplo a seguir mostra o bloco `mcpServers` completo em **[!UICONTROL Configurações]** > **[!UICONTROL MCP]** ou `.cursor/mcp.json` no diretório do projeto:

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

>[!TAB OpenAI Codex]

### OpenAI Codex {#codex}

1. Vá para Configurações > Servidores MCP > Adicionar servidor.
1. Adicionar a URL do servidor: `https://marketo-mcp.adobe.io/mcp`.
1. Adicione os cabeçalhos para seu método de autenticação:

* X-Marketo-Client-Id: &quot;YOUR-CLIENT-ID&quot;
* X-Marketo-Client-Secret: &quot;YOUR-CLIENT-SECRET&quot;
* X-Marketo-Munchkin-Id: &quot;YOUR-MUNCHKIN-ID&quot;

1. Selecione Salvar para concluir o processo.

>[!TAB CLI do Gemini]

### CLI do Gemini

Para adicionar o servidor MCP do Marketo Engage à CLI do Gemini, adicione o seguinte ao `.gemini/mcp.json` no diretório do projeto.

```json
{
  "mcpServers": {
    "marketo": {
      "httpUrl": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "$MARKETO_CLIENT_ID",
        "X-Marketo-Client-Secret": "$MARKETO_CLIENT_SECRET",
        "X-Marketo-Munchkin-Id": "$MARKETO_MUNCHKIN_ID"
      }
    }
  }
}
```

Ou por linha de comando:

```bash
gemini mcp add --transport http -s user marketo https://marketo-mcp.adobe.io/mcp \
  -H "X-Marketo-Client-Id: $MARKETO_CLIENT_ID" \
  -H "X-Marketo-Client-Secret: $MARKETO_CLIENT_SECRET" \
  -H "X-Marketo-Munchkin-Id: $MARKETO_MUNCHKIN_ID"
```

Reinicie a sessão para coletar a nova configuração do servidor MCP.

>[!TAB Código VS com GitHub Copilot]

### Código VS com Copilot do GitHub {#vscode}

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

>[!TAB Copilot Studio]

### Copilot Studio

O Copilot Studio funciona de forma um pouco diferente. Você cria um arquivo de definição YAML e, em seguida, o Copilot Studio cria a interface do usuário do conector a partir dele. É aqui que você define os cabeçalhos personalizados.

Ao criar seu novo Conector, abra o &quot;Editor do Swagger&quot; e cole no seguinte código:

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
        - name: MARKETO_MCP_PROD_CLIENT_ID
          in: header
          description: Client ID.
          type: string
          required: true
        - name: MARKETO_MCP_PROD_CLIENT_SECRET
          in: header
          description: Client secret.
          type: string
          required: true
        - name: MARKETO_MCP_PROD_MUNCHKIN_ID
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

Atualize o Conector, feche-o e reabra-o.
Agora você pode seguir o fluxo de conexão e preencher os valores do cabeçalho.

>[!TAB Obter]

### Glean {#glean}

Para conectar o Glean ao Servidor MCP do Marketo Engage, a [equipe de suporte do Glean](https://docs.glean.com/release-notes/releases/2026-04-22-april-release#admin-features) deve configurar os seguintes cabeçalhos personalizados.

| Header | Valor |
| ------ | ----- |
| `X-Marketo-Client-Id` | Sua ID do cliente |
| `X-Marketo-Client-Secret` | Segredo do cliente |
| `X-Marketo-Munchkin-Id` | Sua ID de conta da Munchkin |

>[!TAB Outras ferramentas]

### Outras ferramentas {#other-tools}

O Adobe hospeda o servidor MCP [!DNL Marketo] e o expõe em uma URL pública. Qualquer cliente MCP que suporte servidores remotos por meio de transporte HTTP eficiente pode se conectar a ele.
Você não precisa de uma ponte específica da ferramenta ou de qualquer software instalado localmente. Se sua ferramenta não estiver listada acima, use os detalhes de conexão abaixo para configurá-la manualmente.

**Detalhes da conexão:**

| Configuração | Valor |
| ------- | ----- |
| Transporte | HTTP (HTTP dinamizável) |
| URL do servidor | `https://marketo-mcp.adobe.io/mcp` |

**Cabeçalhos de autenticação:**

Envie os cabeçalhos de um dos métodos de autenticação a seguir com cada solicitação. O local em que você insere o URL e os cabeçalhos do servidor depende da ferramenta. Consulte a documentação do MCP.

| Header | Valor |
| ------ | ----- |
| `X-Marketo-Client-Id` | Sua ID do cliente |
| `X-Marketo-Client-Secret` | Segredo do cliente |
| `X-Marketo-Munchkin-Id` | Sua ID de conta da Munchkin |

Se a ferramenta aceitar uma configuração JSON, comece com os exemplos de [Cursor](#cursor) ou [Código VS](#vscode) e ajuste as chaves (`mcpServers`, `servers`) para corresponder ao esquema da ferramenta.

>[!ENDTABS]

## Operações disponíveis

Depois de conectado, você pode solicitar que o assistente de IA execute operações nas seguintes categorias. Para obter a lista completa de operações suportadas com referências de API, consulte [Operações MCP suportadas](mcp-server-operations.md).

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
