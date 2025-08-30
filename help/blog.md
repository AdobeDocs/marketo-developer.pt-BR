---
title: Arquivo do blog
description: Um arquivo do blog de desenvolvedores do Marketo de 2014 a 2023
exl-id: d7ae88dd-9938-4957-9798-db43090dab4e
source-git-commit: f314f153c80416bbccc4328756e71c07e696dd00
workflow-type: tm+mt
source-wordcount: '61712'
ht-degree: 0%

---

# Arquivo do blog

>[!INFO]
>
>Este é um arquivo do blog do Marketo, de 2014 a 2023. Ela é fornecida aqui somente como uma referência histórica.
>&#x200B;>Algumas informações podem estar desatualizadas.  Sempre verifique a documentação atual para obter a funcionalidade mais recente.
>

>[!IMPORTANT]
>A API do SOAP está sendo substituída e não estará mais disponível após 31 de outubro de 2025. Todo o novo desenvolvimento deve ser feito com a API REST do Marketo, e os serviços existentes devem ser migrados até essa data para evitar interrupções no serviço. Se você tiver um serviço que usa a API do SOAP, consulte o [Guia de Migração da API do SOAP](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/migration) para obter informações sobre como migrar.
>

>[!IMPORTANT]
>O suporte para autenticação usando o parâmetro de consulta `access_token` será removido em 31 de outubro de 2025. Se o projeto usar um parâmetro de consulta para passar o token de acesso, ele deverá ser atualizado para usar o [Cabeçalho de autorização](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) o mais rápido possível. O novo desenvolvimento deve usar o cabeçalho de Autorização exclusivamente.
>

## Bem-vindo ao Blog do desenvolvedor do Marketo

Bem-vindo(a) aos desenvolvedores do Marketo. Estamos bastante ocupados na Marketo e lançamos novos recursos para ajudar os profissionais de marketing a serem mais bem-sucedidos do que nunca. Os profissionais de marketing de hoje são apoiados por uma comunidade de desenvolvedores fenomenal. Este blog é dedicado aos desenvolvedores da Web e engenheiros de software que suportam as necessidades em rápida evolução do profissional de marketing moderno. À medida que a plataforma Marketo evolui, estamos anunciando novas opções de integração e atualizações de versão da API à medida que elas são liberadas. Também introduziremos uma nova série de artigos explicativos, nos quais compartilhamos amostras de código e práticas recomendadas para integração com a Plataforma Marketo. O primeiro artigo desta série mostra como recuperar com eficiência as informações sobre as pessoas (clientes/contatos/clientes potenciais) armazenadas no Marketo usando a API. Inscreva-se usando o formulário acima para manter-se atualizado. Enviamos atualizações por email à medida que novos artigos são publicados.

Publicado em _2014-03-06_ por _David_

## Atualizações da versão de janeiro de 2014

### Forms 2.0

O Forms 2.0 permite que os profissionais de marketing criem formulários web bonitos, estáveis e flexíveis sem conhecimento de programação. Além do editor, da lógica condicional e do design robusto extremamente aprimorados, agora ficou mais fácil do que nunca incorporar os formulários do Marketo em qualquer página do seu site. Os desenvolvedores podem usar o JavaScript para estender a funcionalidade principal; os casos de uso incluem:

* Ocultar um formulário após o envio bem-sucedido para que o visitante permaneça na página após ter preenchido o formulário
* Exibição de uma mensagem de erro personalizada ao enviar com base na lógica de negócios personalizada
* Exibição do formulário em uma caixa de diálogo de estilo lightbox

A documentação para desenvolvedores está disponível [aqui](/help/javascript-api/forms-api-reference.md).

### API do SOAP versão 2_3 já disponível

* [getLeadChanges:](/help/soap-api/getleadchanges.md) introduziu o campo de solicitação `activityNameFilter`
* [ListOperation:](/help/soap-api/listoperation.md) campo de solicitação `skipActivityLog` removido

**Observação:** as revisões da API do SOAP são compatíveis com versões anteriores

Publicado em _2014-01-26_ por _Travis Kaufman_

## Zapier Parte II: Anúncio de integração do Marketo

Em uma postagem anterior, discutimos como você pode usar o Zapier para integrar fontes de dados externas com o Marketo. O post apresentou uma abordagem prática para criar seu próprio fluxo de trabalho Zapier (ou &quot;Zap&quot;) do zero para integrar o Marketo e outros aplicativos. Você gosta da ideia de usar o Zapier com o Marketo, mas precisa de ajuda para começar? Boas notícias! O Zapier acabou de lançar vários exemplos de Zaps para Marketo que permitem que você comece rapidamente:

* Capturar Novos Clientes Potenciais
* Estimule seus clientes
* Distribuir informações de contato
* Reagir a Novos Clientes Potenciais

Além desses exemplos, você pode navegar na página [Integrações do Marketo](https://zapier.com/apps/marketo/integrations) com centenas de outros aplicativos no Zapier e criar seus próprios fluxos de trabalho automatizados em minutos. Nenhum código é necessário. Economize tempo e permita que a automação faça seu trabalho manual. Seja criativo. O céu é o limite!

Publicado em _2016-06-01_ por _David_

## Recuperação de informações de clientes e clientes potenciais do Marketo usando a API

Você pode recuperar informações sobre clientes e clientes potenciais que são armazenadas no Marketo usando a API do SOAP `getLead` e [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads). Geralmente, é desejável extrair essas informações de forma recorrente para manter outro sistema atualizado à medida que as informações de clientes e clientes potenciais são atualizadas ou novos registros são criados no Marketo. Mostramos a amostra de código que seria executada de forma recorrente para pesquisar atualizações no Marketo. O diagrama abaixo descreve as chamadas de API feitas em um temporizador periódico definido. Dependendo do caso de uso, o temporizador periódico pode ser definido para executar o código abaixo a cada 10 minutos.

A primeira chamada para [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) definirá o intervalo de tempo, batchSize e quais campos serão retornados. Todos os clientes em potencial no Marketo que foram atualizados no intervalo de tempo especificado serão retornados junto com um streamPosition quando houver mais registros disponíveis do que o batchSize especificado.

**Solicitação do SOAP para primeira chamada para getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector>
  <batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

**Resposta da SOAP da primeira chamada para getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
 <returnCount>2</returnCount><remainingCount>24</remainingCount><newStreamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</newStreamPosition><leadRecordList>
      <leadRecord>
        <Id>84105</Id>
        <Email>eimang@marketo.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>French</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>Lead</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <leadRecord>
        <Id>1089965</Id>
        <Email>t@t.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
    </leadRecordList>
  </result>
</ns2:successGetMultipleLeads>
```

Desde que o valor `<remainingCount/>` seja maior que 0, você faz chamadas subsequentes a `getMultipleLeads` para paginar pelo restante passando o valor `<newStreamPosition/>` retornado na chamada anterior para o parâmetro `<streamPosition/>`. **Solicitação SOAP para chamada subsequente a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector><streamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</streamPosition><batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

Esta lógica continua enquanto `<remainingCount/>` for maior que zero. **Veja abaixo um exemplo de programa Java que executa o cenário descrito acima.**

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.GregorianCalendar;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import javax.xml.datatype.DatatypeFactory;
import javax.xml.datatype.XMLGregorianCalendar;

public class GetMultipleLeads {
  public static void main(String[] args) {
    System.out.println("Executing GetMultipleLeads");
        try {
        URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
        String marketoUserId = "CHANGE ME";
        String marketoSecretKey = "CHANGE ME";
        QName serviceName = new QName("<http://www.marketo.com/mktows/>",
        "MktMktowsApiService");
        MktMktowsApiService service = new
        MktMktowsApiService(marketoSoapEndPoint, serviceName);
        MktowsPort port = service.getMktowsApiSoapPort();

        // Create Signature
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String text = df.format(new Date());
        String requestTimestamp = text.substring(0, 22) + ":" +         text.substring(22);
        String encryptString = requestTimestamp + marketoUserId ;

        SecretKeySpec secretKey = new         SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");

        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(secretKey);
        byte[] rawHmac = mac.doFinal(encryptString.getBytes());
        char[] hexChars = Hex.encodeHex(rawHmac);
        String signature = new String(hexChars);

        // Set Authentication Header
        AuthenticationHeader header = new AuthenticationHeader();
        header.setMktowsUserId(marketoUserId);
        header.setRequestTimestamp(requestTimestamp);
        header.setRequestSignature(signature);

        // Create Request
        ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();

        // Build Request Using LastUpdateAtSelector
        LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
        GregorianCalendar gc = new GregorianCalendar();
        gc.setTimeInMillis(new Date().getTime());
        gc.add( GregorianCalendar.DAY_OF_YEAR, -20);
        DatatypeFactory factory = DatatypeFactory.newInstance();
        ObjectFactory objectFactory = new ObjectFactory();
        JAXBElement<XMLGregorianCalendar> until =         objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

        GregorianCalendar since = new GregorianCalendar();
        since.setTimeInMillis(new Date().getTime());
        since.add( GregorianCalendar.DAY_OF_YEAR, -21);
        leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
        leadSelector.setLatestUpdatedAt(until);
        request.setLeadSelector(leadSelector);

        ArrayOfString attributes = new ArrayOfString();
        attributes.getStringItems().add("FirstName");
        attributes.getStringItems().add("LastName");
        attributes.getStringItems().add("Email");
        request.setIncludeAttributes(attributes);

        JAXBElement<Integer> batchSize = new
        ObjectFactory().createParamsGetMultipleLeadsBatchSize(2);
        request.setBatchSize(batchSize);

        SuccessGetMultipleLeads result = null;

        int count = 0;

        do {
        if (count > 0) {
        // Set the streamPosition on subsequent calls to paginate
        through large result sets
        String pos = result.getResult().getNewStreamPosition();
        JAXBElement<String> streamPos = new
        ObjectFactory().createParamsGetMultipleLeadsStreamPosition(pos);
        request.setStreamPosition(streamPos);
        }

        JAXBContext context =
        JAXBContext.newInstance(ParamsGetMultipleLeads.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        System.out.println("REQUEST:");
        m.marshal(request, System.out);
        result = port.getMultipleLeads(request, header);
        System.out.println("RESPONSE:");
        m.marshal(result, System.out);
        count = result.getResult().getReturnCount();
        } while (result.getResult().getRemainingCount() > 0);
        }

        catch(Exception e) {
        // Update to include appropriate retry/error handling
        e.printStackTrace();
        }

  }

}
```

### Dicas e truques

Ao extrair grandes volumes de contatos do Marketo, é recomendável ajustar a solicitação da API junto com os seguintes parâmetros:

* `<includeAttributes/>`: é recomendável que você solicite apenas os campos que deseja manter sincronizados com o sistema. Isso reduz a carga da resposta e aumenta o desempenho da consulta.
* `<batchSize/>`: a API dá suporte para até 1000 registros a serem retornados em uma única chamada. Ajustar esse valor para 500 registros também reduz a carga da resposta.
* `<LastUpdatedAtSelector/>`: É recomendável definir `<oldestUpdatedAt/>` juntamente com o parâmetro `<latestUpdatedAt/>` para limitar o intervalo de datas. Por exemplo, em vez de fazer uma única solicitação para os dados de um ano. Divida as chamadas de API para solicitar intervalos de datas menores.

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-03-05_ por _Travis Kaufman_

## Atualizações da versão de fevereiro de 2014

### Atualização da API do SOAP

* [syncMObjects](/help/soap-api/syncmobjects.md): agora é possível adicionar e atualizar marcas e canais para programas existentes.

As atualizações estão incorporadas ao [2_3 WSDL](http://app.marketo.com/soap/mktows/2_3?WSDL).

Publicado em _2014-02-26_ por _Travis Kaufman_

## Atualizações da versão de março de 2014

### Atualização da API do SOAP

* Melhorias de desempenho em [syncLead](/help/soap-api/synclead.md) e [syncMultipleLeads](/help/soap-api/syncmultipleleads.md)

As atualizações estão incorporadas ao [2_3 WSDL](http://app.marketo.com/soap/mktows/2_3?WSDL).

Publicado em _2014-03-20_ por _Travis Kaufman_

## Mesclar atividade de visitante anônimo quando o visitante preencher o formulário

Na publicação do blog intitulada &quot;Capturar atividade de visitante anônimo com base na lógica de negócios&quot;, discutimos como criar registros de lead anônimos no Marketo com base em eventos personalizados. Nesta publicação do blog, desenvolveremos essa publicação e associaremos um registro de lead anônimo a um usuário conhecido depois que você receber as informações de contato do usuário. O [código de rastreamento do Munchkin](/help/javascript-api/lead-tracking.md) da Marketo ajuda a rastrear visitas ao seu site. Na primeira vez que alguém visita uma página em seu site que tem o código de rastreamento do Munchkin, o Marketo cria um lead anônimo e usa um cookie do navegador para rastreá-lo. Depois que se identificam, tornam-se um lead conhecido e o histórico associado ao cookie do navegador é mesclado no registro de lead do Marketo. **Clientes potenciais anônimos são criados quando alguém:**

1. Visita a página de aterrissagem do Marketo pela primeira vez
1. Visita uma página que tem código de rastreamento do Munchkin
1. Clique no link Exibir como página da Web em um email do Marketo

**Um cliente potencial anônimo é mesclado em um cliente potencial conhecido novo ou existente quando alguém:**

1. Clique em um link para um email do Marketo
1. Preenche um formulário do Marketo
1. Usa uma das técnicas abaixo para associar um lead anônimo a um registro conhecido.

Para enviar dados pessoais ou associar um cookie de rastreamento Web do navegador ao registro de pessoa correspondente no Marketo, use uma das seguintes técnicas: **Envio pelo lado do navegador** Se o caso de uso exigir o envio de dados pessoais do navegador, você deverá usar o envio de formulários em segundo plano. **Envio pelo lado do servidor** Se você não precisar enviar pelo lado do navegador, a API REST oferecerá muitos métodos para envio de dados de pessoas e um método criado com propósitos específicos para associar cookies a registros de pessoas.

Publicado em _2014-04-22_ por _Murta_

## Atualizações da versão de abril de 2014

### Atualização de segurança do Marketo Forms

Introduzimos um limite no número e na frequência de envios de formulários de publicação a partir de um único endereço IP. Esse limite agora é aplicado em 30 postagens por minuto para proteger nossos clientes contra o uso mal-intencionado de envios de formulários programáticos. A [API syncLead](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/leads/synclead) é o veículo de integração recomendado para o envio programático de novos contatos no Marketo.

Publicado em _2014-04-29_ por _Travis Kaufman_

## Substitua o formulário após a integração com a API do Marketo

É uma prática comum dos profissionais de marketing que usam o Marketo associarem o sucesso de uma determinada campanha de marketing com base no número de contatos/clientes potenciais que preencheram um formulário da Web específico. O profissional de marketing simplesmente criaria um novo formulário do Marketo, o colocaria em uma página de aterrissagem e configuraria uma campanha de acionador no Marketo para associar todos os contatos que preencheram esse formulário específico como um sucesso para esse programa de marketing. (veja a captura de tela abaixo). É uma extensão natural usar essa mesma abordagem ao registrar o sucesso do programa, mesmo quando o sucesso da campanha reside fora de alguém que preenche um formulário. Por exemplo, quando um profissional de marketing executa uma oferta promocional e a conversão é chamar e agendar um compromisso. O cliente potencial não está preenchendo um formulário em nenhum momento do processo. No artigo abaixo, mostramos como usar a API syncLead do Marketo para criar um novo contato, se ele for um novo contato, e depois solicitamos que a API do Campaign registre o sucesso de um determinado programa de marketing.

Publicado em _1970-01-01_ por _Travis Kaufman_

## Excluir um cliente potencial com a API do Marketo

Digamos que você deseje excluir um lead por meio da API do Marketo. Você pode fazer isso chamando a API requestCampaign em uma campanha que tenha uma ação de fluxo predefinida para excluir um lead. Primeiro, mostramos como criar uma Campanha inteligente, em segundo lugar, como configurar um acionador para executar uma campanha por meio da API, em terceiro, como definir a exclusão de um lead como parte de uma ação de fluxo e, em quarto lugar, uma amostra de código que seria usada para executar essa campanha. **Como criar uma nova campanha inteligente no Marketo** As campanhas inteligentes no Marketo executam todas as suas atividades de marketing. Em uma Campanha inteligente, você pode configurar uma série de ações automatizadas para executar uma lista inteligente de contatos. No caso de excluir um lead, você configurou um acionador na campanha, como mostrado abaixo. Primeiro, vamos configurar a Campanha inteligente.

1. Em Atividades de marketing, escolha um Programa e, na lista suspensa Novo, clique em Novo ativo local 1. Clique em Campanha inteligente
1. Insira o nome da campanha inteligente e clique em Criar adicionar acionadores a uma campanha inteligente** Adicionar acionadores a uma campanha inteligente permite que uma campanha inteligente seja executada em uma pessoa de cada vez com base em um evento ao vivo, que neste caso é uma solicitação por meio da API requestCampaign.
1. Procure o acionador &quot;A campanha é solicitada&quot; e arraste e solte-o na tela.
1. No acionador, selecione &quot;é&quot; e &quot;API do serviço da Web&quot;.

**Como criar uma ação de fluxo Excluir um cliente potencial em uma campanha** Clique em Fluxo no menu superior. No menu no lado direito, procure por Excluir lead e arraste-o para o centro para adicioná-lo como um acionador à campanha. Observação: se você excluir um cliente potencial somente do Marketo e deixá-lo no CRM, quando os dados desse cliente potencial forem atualizados, o cliente potencial será recriado no Marketo.  **Amostra de código para chamar a API requestCampaign** Depois de configurar a campanha e os acionadores na interface do Marketo, mostramos como usar a API para executar esta campanha. O primeiro exemplo é uma solicitação XML, o segundo é uma resposta XML e o último é uma amostra de código Java que pode ser usada para gerar a solicitação XML. Também mostramos como encontrar a ID da campanha usada ao chamar a API requestCampaign. A chamada de API também exige que você saiba a ID da campanha do Marketo antecipadamente. Você pode determinar a ID da campanha usando um dos seguintes métodos:

1. Usar a API [getCampaignsForSource](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET)
1. Abra a campanha do Marketo em um navegador e observe a barra de endereços do URL. A ID da campanha (representada como um inteiro de 4 dígitos) pode ser encontrada imediatamente após &quot;SC&quot;. Por exemplo, `https://app-stage.marketo.com/#SC**1025**A1`. A parte em negrito é a ID da campanha - &quot;1025&quot;. Solicitação SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Resposta do SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Veja abaixo um exemplo de programa Java que executa o cenário descrito acima.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-05-16_ por _Murta_

## Rastreamento de atividade personalizado com o AP- do Munchkin

Vamos acompanhar a atividade personalizada no Marketo. Por exemplo, você tem um vídeo em uma página da Web e deseja rastrear visitantes que assistem a mais de 50% de um vídeo. Você pode fazer isso usando o recurso personalizado de rastreamento de atividades do Munchkin. Isso seria implementado ouvindo um evento na página, que é o vídeo atingindo 50%, e chamando a API do Munchkin. Para fazer isso, precisamos configurar uma atividade personalizada no Marketo para chamar com base nesse evento na página. Usamos o YouTube para o reprodutor de vídeo e a [API do YouTube Iframe](https://developers.google.com/youtube/iframe_api_reference) para chamar o método na API do Munchkin.

Primeiro, mostramos como gerar o código de rastreamento do Munchkin no Marketo, segundo, como modificar seu código de amostra do Munchkin para acionar com base em eventos de página, terceiro, como configurar uma campanha com uma lista inteligente que é definida por ações na página com etapas de fluxos e quinto, como verificar se uma visita de página de um usuário anônimo foi registrada no Marketo. ==== Esta publicação do blog é um exemplo ao vivo do código que está sendo explicado. Preencha este formulário para ser um usuário conhecido no Marketo. Dessa forma, quando você assiste a 50% do vídeo, ele envia o restante do vídeo e, se você assistir a 100% do vídeo, ele envia um link para outra publicação do blog. === <https://developers.google.com/youtube/iframe_api_reference>

**Como gerar o código de rastreamento do Munchkin** O código de rastreamento do Munchkin permite rastrear visitas ao seu site. Há três tipos de código Munchkin descritos abaixo, mas neste exemplo usamos o código de rastreamento assíncrono do Munchkin. A) Simples: tem menos linhas de código, mas não otimiza o tempo de carregamento da página da Web. Este código carrega a biblioteca jQuery sempre que uma página da Web é carregada. B) Assíncrono: reduz o tempo de carregamento da página da Web. Esse código verifica se a biblioteca jQuery já existe, carrega se estiver ausente e a usa para executar o código de rastreamento depois que o restante da página da Web for carregada. C) Asynchronous jQuery: reduz o tempo de carregamento da página da Web e melhora o desempenho do sistema. Esse código pressupõe que você já tenha o jQuery e não verifica se deve carregá-lo.

1. Clique em Administração na parte superior direita do aplicativo.
1. Clique em Munchkin na árvore à esquerda.
1. Selecione Assíncrono para Tipo de código de rastreamento.
1. Clique em e copie o código de rastreamento do JavaScript para colocar em seu site. **Código YouTube** <https://developers.google.com/youtube/js_api_reference#EventHandlers> <https://developers.google.com/youtube/iframe_api_reference> reprodutor.

`getCurrentTime()` Retorna o tempo decorrido em segundos desde o início da reprodução do vídeo. `player.getDuration()` Retorna a duração em segundos do vídeo em reprodução. Observe que `getDuration()` retornará 0 até que os metadados do vídeo sejam carregados, o que normalmente acontece logo após o início da reprodução do vídeo. Quando um usuário sem cookies acessa uma página com o código de rastreamento do Munchkin, um novo cookie é criado no navegador do usuário e um novo lead anônimo é criado no Marketo. Se o usuário já estiver com cookies e for um cliente potencial existente no Marketo, a visita à página será registrada no log de atividades do usuário no Marketo. **Amostra de código para Usuário de cookie e Evento de rastreamento** Coloque o código de rastreamento nas suas páginas da Web logo antes da marca `</body>`. As landing pages criadas no Marketo contêm automaticamente um código de rastreamento, portanto, não é necessário colocar esse código nelas. Este exemplo de código chamaria a API do Munchkin depois que o script fosse carregado:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Essa amostra de código chamaria a API do Munchkin depois que o usuário estivesse na página por 5 segundos e também rolasse 500 pixels para baixo na página:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Como verificar se a visita de página por usuário anônimo foi registrada no Marketo**

1. Clique em Analytics no menu superior e, em seguida, clique em Novo relatório. Escolha Atividade da página da Web como o tipo de relatório e nomeie o relatório.
1. Depois de criar um relatório, clique em Smart List. Em seguida, selecione o filtro Página da Web visitada na caixa à direita. Insira a página da Web onde você coloca o código de rastreamento do Munchkin.
1. Clique em Configurar. Selecione Visitantes anônimos em ISPs e altere a opção para Mostrado.
1. Clique em Relatório. Agora você verá a atividade rastreada na página da Web selecionada.
1. Clique duas vezes no registro de lead, que mostrará o log de atividades, onde é possível ver a página específica que o usuário anônimo visitou.

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Para páginas com conteúdo multimídia, por exemplo, é possível fazer um rastreamento personalizado. Um exemplo comum é adicionar o código de rastreamento do Munchkin à página. Além disso, use a API do Munchkin para gerar eventos na instância do Marketo para atividades como reproduzir um vídeo ou ouvir um clipe de áudio. Recomendamos que você coloque o código de rastreamento do Munchkin na maioria ou em todas as páginas da Web. O código de rastreamento do Munchkin é incluído automaticamente nas páginas de aterrissagem criadas com o Marketo. Use esta chamada para registrar que o usuário fez algo, como visitar uma página no Ajax, Flash ou outro ambiente RIA. O URL não deve conter &quot; ou qualquer domínio, mas pode apontar para qualquer página, mesmo para páginas que não existem. Se quiser adicionar parâmetros de URL, use o argumento params.
O evento é exibido como um evento de Visita à página da Web no registro de atividades do usuário no domínio da página da Web de chamada. Observação: sua primeira chamada para `mktoMunchkin()` sempre cria um evento de Visita da página da Web para a página atual. Você não precisa chamar `visitWebPage` a menos que queira rastrear uma visita adicional à página da Web. `mktoMunchkinFunction('visitWebPage', { url: '/MyFlashMovie/Story1', params: 'x=y&2=3' });` Observação Certifique-se de ter acesso a um desenvolvedor experiente do JavaScript. O Suporte Técnico da Marketo não está configurado para ajudar na solução de problemas do JavaScript personalizado. A API do Munchkin JavaScript permite integrar um sistema da Web de terceiros à sua conta do Marketo. Com algum desenvolvimento na Web, você pode capturar novos leads ou atualizar leads atuais com aplicações existentes em seu site. Digamos que você tenha um aplicativo da Web para registro de clientes que capture informações de novos clientes. Com apenas um pouco de programação, você também pode ter informações de clientes potenciais para esses usuários capturados no Marketo, e um cookie do Marketo definido para rastreamento futuro na Web.

Além disso, outro recurso permite que os desenvolvedores da Web capturem e rastreiem informações de atividade da Web em ambientes avançados da Web, como Flash ou Ajax. Observação: se você tiver os recursos de desenvolvimento apropriados, considere usar nossa API do SOAP para integração em vez dessa API. A API do SOAP é mais robusta e tem mais funcionalidade do que a API do Munchkin. Requisitos da API do SOAP no Marketo É necessário incluir o código JavaScript do Munchkin na página da Web para que isso funcione. Você pode encontrar as tags de script necessárias no Tutorial do Munchkin. Ative também a API do Munchkin, que também está descrita no Tutorial.
Sob a capa Depois de fazer uma chamada de API do Munchkin, ela criará um cookie automaticamente para o usuário, caso ele não tenha um cookie. No Marketo, ele registra o evento (clique em um link, visite uma página da Web ou um novo lead) no Log de atividades da pessoa. Se você estiver usando o link de clique ou visitar uma chamada de página da Web, o evento será adicionado ao Log de atividades desse lead (conhecido ou anônimo). Se este for um cliente potencial novo e você usar a chamada de cliente potencial associado, esse cliente potencial se tornará um cliente potencial conhecido e seu histórico de atividades será preservado. Se este for um cliente potencial existente (com base na correspondência de endereço de email), qualquer valor alterado ou novo será atualizado no registro desse cliente potencial.

Esta é a forma geral de uma chamada `munchkinFunction`. Inclua-o como tags na página da Web onde quiser chamá-lo. Você pode chamá-la como qualquer outra função do JavaScript. No entanto, você deve chamar a função de rastreamento `mktoMunchkin()` do Munchkin antes de fazer qualquer chamada `mktoMunchkinFunction()`:

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"> mktoMunchkin("###-###-###"); mktoMunchkinFunction('function', { key: 'value', key2: 'value'}, 'hash');
```

Onde: `###-###-###` é a ID de conta da Munchkin para a função da sua conta é a chamada que você deseja fazer para que os parâmetros sejam uma matriz de parâmetros necessários para o hash de chamada; isso só é necessário para `associateLead`

Publicado em _1970-01-01_ por _Murta_

## Obtenção de dados para o Marketo

A apresentação abaixo mostra as diferentes maneiras de obter dados no Marketo. Ele se concentra em formulários, objetos personalizados e integrações.

[Obtendo Dados no Marketo](https://www.slideshare.net/MurtzaManzur/getting-data-into-marketo-35662408) de [Murtza Manzur](https://www.slideshare.net/MurtzaManzur)

Publicado em _2014-06-06_ por _Murta_

## Criação de clientes em potencial em uma Workspace

Digamos que sua empresa tenha duas divisões: América do Norte e Europa. Você gostaria de segmentar seus leads com base na divisão da empresa na Marketo. Isso pode ser feito usando espaços de trabalho, que é um recurso no Marketo que permite limitar o acesso a clientes potenciais. Para fazer isso, você criaria um espaço de trabalho para a América do Norte e outro para a Europa. Em seguida, você pode criar um cliente potencial em um espaço de trabalho específico usando a [API syncLead](/help/soap-api/synclead.md). Considere usar espaços de trabalho e partições de clientes potenciais se sua organização tiver:

1. Equipes de marketing separadas para várias linhas de produtos
1. Equipes de marketing separadas para diferentes territórios ou países
1. Uma empresa-mãe e empresas subsidiárias
1. Uma empresa principal e revendedores

Ao usar partições de lead e espaços de trabalho, você pode:

1. Restringir o acesso a clientes potenciais em sua organização
1. Restringir o acesso a ativos em sua organização
1. Compartilhar ativos entre equipes de marketing

Primeiro, mostramos como criar um espaço de trabalho no Marketo por meio da interface e, segundo, como gravar um cliente potencial nesse espaço de trabalho usando a [API syncLead](/help/soap-api/synclead.md). **Criando uma Workspace** Um espaço de trabalho é um conjunto de clientes potenciais e ativos da Marketo. Em um espaço de trabalho, você só pode ver leads desse espaço de trabalho e ativos (emails, campanhas, listas etc.) nesse espaço de trabalho. As campanhas inteligentes nesse espaço de trabalho afetam somente clientes potenciais nesse espaço de trabalho. Para ver os espaços de trabalho na sua conta:

1. Vá para a página Espaços de Trabalho e Partições de Cliente Potencial da seção Admin. Seus espaços de trabalho aparecem na guia Espaços de trabalho. 1. Para criar um novo espaço de trabalho, clique no botão Novo Workspace na barra de menus da guia Espaços de Trabalho.
1. Na caixa de diálogo, é necessário adicionar algumas informações sobre o novo espaço de trabalho:

* **Nome do Workspace** - o nome deste espaço de trabalho como ele aparece na interface
* **Descrição** - uma descrição de texto opcional do espaço de trabalho
* **Partições de cliente potencial** - quais clientes potenciais estão visíveis nesta partição.
* **Todas as Partições de Cliente Potencial** - verá todos os clientes potenciais de todas as partições presentes e futuras
* **Selecionar partições individuais** - somente os clientes em potencial dessas partições (e nenhuma partição futura) estarão visíveis
* **Partição de Cliente Potencial Principal** - Os clientes potenciais que visitam suas páginas de aterrissagem são adicionados a esta partição por padrão
* **Idioma** - o idioma comercial do espaço de trabalho

Mostramos como usar os leads de gravação da API para um espaço de trabalho específico. O primeiro exemplo é uma solicitação XML, o segundo é uma resposta XML e o último é uma amostra de código Ruby que pode ser usada para gerar a solicitação XML. 1. Depois de criar o lead, Partição do Lead é um campo em Informações do Lead. Solicitação SOAP para `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Resposta do SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Veja abaixo um exemplo de programa Java que executa o cenário descrito acima.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Como faço para me inscrever em Espaços de Trabalho e Partições de Cliente Potencial?** Para clientes do Marketo Enterprise, você tem acesso gratuito a espaços de trabalho e partições de clientes potenciais. Entre em contato com o gerente de ativação do cliente para obter detalhes sobre como ativá-los e implementá-los. Para outros clientes, entre em contato com o executivo de vendas da Marketo ou envie um email para nossa equipe de vendas para obter mais informações sobre este produto.

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _1970-01-01_ por _Murta_

## Atualizações da versão de junho de 2014

### API do Marketo Real-Time Personalization

A API do JavaScript do Marketo Real-Time Personalization (RTP) estende a capacidade de personalização automatizada da plataforma. Ele permite o rastreamento de eventos e a personalização dinâmica de uma página da Web. Veja a documentação completa [aqui](/help/javascript-api/web-personalization.md).

Publicado em _2014-06-20_ por _Travis Kaufman_

## Armazenamento de uma chave estrangeira no Marketo

Ao sincronizar registros de contato e lead entre sistemas como um CRM ou data warehouse proprietário, é um requisito comum associar um registro de lead a um identificador de sistema exclusivo. No Marketo, você pode criar ou atualizar um registro de cliente potencial por meio de uma chamada de [APIs syncMultipleLeads](/help/soap-api/syncmultipleleads.md) usando seu identificador de sistema exclusivo. Para isso, você armazenaria seu identificador de sistema exclusivo (chave primária) como uma chave estrangeira no Marketo. O nome desse campo no Marketo para armazenar uma chave estrangeira é foreignSysPersonId. Estas são três coisas importantes a observar:

1. ForeignSysPersonId não está visível na interface do usuário do Marketo. Portanto, é uma prática recomendada preencher um campo de atributo personalizado com esse valor.
1. ForeignSysPersonId é exclusiva de um lead, mas um lead pode ter mais de um ForeignSysPersonId.
1. ForeignSysPersonId não pode ser atualizada ou excluída, mas pode ser reatribuída a outro registro.

Mostramos como fazer uma chamada para a [API syncMultipleLeads](/help/soap-api/syncmultipleleads.md) para gravar um valor ForeignSysPersonId em dois registros de cliente potencial existentes no Marketo. **Como Gravar ForeignSysPersonId Usando a API syncMultipleLeads** Você pode inserir um novo registro de lead e especificar a ForeignSysPersonId. Você também pode adicioná-lo a um cliente potencial existente especificando a Marketo ID e o ForeignSysPersonId. Nós o guiaremos através do último caso. **Solicitar XML para chamada à API do SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>b5e21953ae9f1b263e644da5eccce9ff33802513</requestSignature>
      <requestTimestamp>2013-08-01T18:22:30-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMultipleLeads>
      <leadRecordList>
        <leadRecord>
          <leadId>1090240</leadId>
          <foreignSysPersonId>1224191</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1000</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1000</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
        <leadRecord>
          <leadId>1090239</leadId>
          <foreignSysPersonId>1224192</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1001</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1001</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
      </leadRecordList>
      <dedupEnabled>true</dedupEnabled>
    </ns1:paramsSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**XML de resposta para a chamada à API do SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMultipleLeads>
      <result>
        <syncStatusList>
          <syncStatus>
            <leadId>1090240</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
          <syncStatus>
            <leadId>1090239</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
        </syncStatusList>
      </result>
    </ns1:successSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Veja abaixo um exemplo de programa Ruby que gerará o XML de Solicitação acima.**

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
        :lead_record_list => {
                :lead_record => {
                        :lead_id => "1090240",
                        :foreign_sys_person_id => "1224191",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1000" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1000" }
                }
        },
        :lead_record! => {
                        :lead_id => "1090239",
                        :foreign_sys_person_id => "1224192",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1001" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1001" }
                }
        }
        },
    :dedup_enabled => "True"
}

response = client.call(:sync_multiple_leads, message: request)

puts response
```

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-06-27_ por _Murta_

## Atualizar o endereço de email de um cliente potencial

Digamos que um usuário preencha um formulário do Marketo em seu site. O que acontece? O Marketo cria um cookie com o usuário e o associa ao email fornecido. E se na próxima vez que o usuário visitar seu site e preencher o mesmo formulário novamente com um email diferente. O que acontecerá? O Marketo criará um novo registro de cliente potencial e substituirá o primeiro cookie no navegador do usuário. O usuário agora é um lead novo/diferente no Marketo. Mostramos quatro maneiras de atualizar o endereço de email de um cliente potencial no Marketo, incluindo o [método da API syncLead](/help/soap-api/synclead.md), o campo personalizado no método de formulário, a interface do usuário do Marketo e a importação de uma lista. **Por meio da API syncLead**, você pode usar a [API syncLead](/help/soap-api/synclead.md) para atualizar um registro de cliente potencial usando a Marketo ID e o novo endereço de email. Solicitar XML para `syncMultipleLeads` Chamada de API do SOAP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncLead>
      <leadRecord>
        <leadId>1090240</leadId>
        <Email>t@t.com</Email>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

XML de resposta para chamada à API do SOAP syncMultipleLeads

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1090240</leadId>
        <syncStatus>
          <leadId>1090240</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Veja abaixo um exemplo de programa Ruby que gerará o XML de solicitação acima.

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
 :lead_record => {
  :Email => "<t@t.com>",
  :lead_id => "1090240",
 :return_lead => "false"
}

response = client.call(:sync_lead, message: request)

puts response
```

**Por meio de um campo personalizado em um formulário**, você pode criar um campo personalizado para o &quot;Novo endereço de email&quot; no Marketo. Em seguida, peça ao usuário para preencher um formulário que inclua esse novo campo. Em seguida, crie um programa no Marketo que altere o valor dos dados do campo de endereço de email do sistema com o token `{{lead.newEmailAddress}}` quando houver uma alteração no novo campo personalizado &quot;Novo endereço de email&quot;. **Por meio da interface do usuário do Marketo**, é possível atualizar manualmente o endereço de email de um cliente potencial por meio da interface do usuário do Marketo. Este é um [artigo de ajuda](https://nation.marketo.com/) que descreve como fazer isso (é necessário fazer logon no Marketo para ver o artigo). **Por meio da Importação de uma Lista**. Você pode atualizar o endereço de email de um cliente potencial usando o método de importação de uma lista no Marketo descrito [aqui](https://nation.marketo.com/) (é necessário fazer logon no Marketo para ver o artigo).  

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _1970-01-01_ por _Murta_

## Armazenar um segundo endereço de email para um cliente potencial

Digamos que você queira alterar a pontuação de um lead no Marketo usando as APIs. Isso é possível para fazer com a API REST usando o endpoint Criar/atualizar lead. Se quiser armazenar mais de um email em um registro de cliente potencial, será necessário criar um campo personalizado e armazenar o segundo email nele. Veja um artigo de ajuda com mais informações: abaixo está uma amostra de código em Ruby que mostra como fazer essa chamada.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Publicado em _2015-02-20_ por _Murta_

## Crie um campo personalizado no Marketo e atualize esse campo via AP

Digamos que você tenha dados adicionais sobre seus leads que não se encaixam nos campos padrão do Marketo. Por exemplo, esse campo personalizado pode ser uma pontuação de terceiros. Você pode criar um campo personalizado no Marketo para sua pontuação de terceiros e atualizar o valor desse campo por meio das [APIs REST](https://developer.adobe.com/marketo-apis/) ou [APIs SOAP](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/soap/activity-type-filters) do Marketo. Primeiro, mostramos como criar um campo personalizado no Marketo e, segundo, como atualizar esse campo usando a API REST.

### Como criar um campo personalizado no Marketo

1. Em Administração, clique em Gerenciamento de campo.
1. Clique no botão Novo campo personalizado.
1. Escolha o Tipo de campo. Isso altera a forma como ele é renderizado nas Smart Lists e no Forms no Marketo.
1. Insira o Nome como deseja que apareça no Marketo. Escolha o Nome e o Nome da API cuidadosamente, pois renomear campos pode ser difícil e, em algumas situações, não é possível.
1. O campo personalizado criado agora pode ser acessado por meio das APIs.

### Como atualizar um campo personalizado por meio da API REST

Na seção anterior, criamos um campo personalizado chamado `myCustomField` com a cadeia de caracteres de tipo de dados. Para atualizar o valor desse campo, usamos o endpoint da API REST chamado Criar / Atualizar leads. Antes de fazer uma solicitação para a API REST, é necessário autenticar. Isso está fora do escopo deste artigo, mas informações detalhadas [ estão disponíveis no site de desenvolvedores do Marketo](/help/rest-api/authentication.md).

**Ponto de extremidade**

`/rest/v1/leads.json`

**Solicitar corpo**

```json
{
   "action":"createOrUpdate",
   "input":[
      {
         "email":"<example@example.com>",
         "myCustomField":"examplestring"
      }
   ]
}
```

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-08-19_ por _Murta_

## Integração do Unbounce e do Marketo

**OBSERVAÇÃO: Esta é uma publicação de blog de convidado do Fab Capodicasa. Ele é um consultor certificado pela Marketo na [Hoosh Marketing](http://hooshmarketing.com.au/), um parceiro de agência do Marketo LaunchPoint especializado em B2C. Ele trabalhou tanto em SaaS quanto em marketing nos últimos 13 anos. Sua experiência é uma mistura de TI pesada, marketing direto e vendas corporativas. Fab também é ex-funcionário da Marketo.**

**Visão geral** Neste artigo, demonstramos como integrar o Unbounce, uma ferramenta popular de página de aterrissagem, ao Marketo. Mostramos primeiro como inserir o rastreamento do Marketo no Unbounce e, em segundo lugar, como modificar os formulários do Unbounce para inserir dados diretamente no Marketo. O desafio de integrar o Unbounce ao Marketo é que o Unbounce não permite que os campos padrão sejam renomeados (por exemplo, first_name não pode ser alterado para FirstName). Também não permite que os rótulos de campo sejam diferentes dos nomes de campo. Essa integração envolve o JavaScript, que ajusta os formulários existentes para torná-los compatíveis com o Marketo. Recomendo que você tenha pelo menos um nível iniciante de JavaScript e um nível intermediário de conhecimento sobre Marketo para concluir as tarefas neste artigo.
**Parte 1: Adicionar código de rastreamento do Marketo à rejeição** Adicionar o script de rastreamento do Marketo Munchkin às páginas de rejeição é necessário para que o Analytics e a integração de formulários funcionem. Siga estas etapas: Copie seu código Munchkin do Marketo: Navegue até Admin -> Munchkin e copie a versão &quot;Simples&quot; do JavaScript. Abra a página de aterrissagem Unbounce e clique em JavaScript -> Adicionar nova JavaScript.  Clique em Adicionar, chame o script &quot;Munchkin&quot;, selecione &quot;Antes da tag de fim de corpo&quot; e cole o código Munchkin. Clique no botão Concluído. Para páginas futuras do Unbounce, você vai para o JavaScript e ativa o script do Munchkin que criamos. Não há necessidade de recriá-lo.
**Parte 2: Converter o formulário de Rejeição em um formulário do Marketo** Agora precisamos modificar o formulário de Rejeição adicionando alguns novos campos ocultos e o JavaScript para permitir que suas páginas de aterrissagem de Rejeição enviem informações de cliente potencial diretamente para o Marketo. Primeiro, criaremos um formulário de espaço reservado do Marketo. No Marketo, crie um formulário em branco e aprove-o.

Este é o formulário proxy no Marketo que representa o formulário Unbounce. Adicionar campos ocultos ao formulário de devolução. Esses campos ocultos são exigidos pelo Marketo para determinar a instância do Marketo e a que formulário e sessão de usuário o envio do formulário será aplicado. Em Unbounce, abra o formulário clicando duas vezes em. Adicione um campo oculto chamado `_mkt_trk`. Adicione um segundo campo oculto chamado `formid`.  233 precisa ser substituída pela id do seu formulário, que pode ser encontrada no código incorporado do formulário do Marketo no Marketo. No Marketo, Abra o formulário, selecione Ações de formulário->Código incorporado. Adicione um campo oculto chamado `returnurl`. `http://hooshmarketing.com.au/thank-you` precisa ser substituída pela URL de acompanhamento. Esta é a URL para a qual você deseja que os usuários sejam redirecionados após o envio do formulário. Por exemplo, esta pode ser a sua página de agradecimento.
**Parte 3: Formulário de Rejeição Direta para o Marketo** A URL de acompanhamento é a página para a qual seu lead será redirecionado depois que ele for enviado ao Marketo. Em Unbounce, siga estas etapas: Clique em seu formulário. Modifique a seção Confirmação de formulário. Altere a Confirmação para Postar dados de formulário em uma URL. Defina o URL para a página de acompanhamento que você deseja. `fpmarkets` precisa ser substituído pela cadeia de caracteres de sua conta do Marketo, que pode ser encontrada em Marketo em Admin->Páginas de aterrissagem.
**Parte 4: Adicionar o JavaScript à Página de Rejeição** Esta JavaScript converterá o formulário para ser compatível com o Marketo e enviá-lo para o Marketo. Em Rejeitar, siga estas etapas: abra a landing page de Rejeição e clique em JavaScript -> Adicionar nova JavaScript. Clique em Adicionar, chame o script &#39;Conversão do formulário do Marketo&#39;, selecione &#39;Antes da tag de fim de corpo&#39;. Cole o código JavaScript abaixo:

```javascript
var MARKETO_MUNCHKIN_ID='614-CGT-700';
var MARKETO_ACCOUNT_STRING = 'fpmarkets';

var UNBOUNCE_MARKETO_FIELD_MAP = new Object();

//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
UNBOUNCE_MARKETO_FIELD_MAP['last_name'] = 'LastName';
UNBOUNCE_MARKETO_FIELD_MAP['phone_number'] = 'Phone';

function getMarketoField(k) {
    return UNBOUNCE_MARKETO_FIELD_MAP[k];
}


var formFields = [];
var hiddenClonedFields = [];
var firstForm = document.forms[0];

//Convert Unbounce form names to Marketo field names
for(i=0; i<firstForm.elements.length; i++){
 var formField = firstForm.elements[i];
 var newFieldName = getMarketoField(formField.name);

  if(newFieldName != undefined) {


    //save original field as hidden field
    var hiddenField = document.createElement("input");
    hiddenField.setAttribute("type", "hidden");
    hiddenField.setAttribute("name", formField.name);
    hiddenField.setAttribute("id", formField.id);
    hiddenClonedFields.push(hiddenField);

    //change original field
    console.log ( 'Changed form field name: ' + formField.name + '=>' + newFieldName );
    formField.name = newFieldName;
    formField.id = newFieldName;
    formFields.push(formField);


  } else {
    console.log ( 'Couldn't map:' + formField.name );
  }
}

//Add hidden cloned Unbounce fields to form
//for Unbounce validation
for(i=0; i<hiddenClonedFields.length; i++){
    firstForm.appendChild(hiddenClonedFields[i]);
    formFields[i].onchange = (function(hf) {
        return function(event) {
            hf.value = event.target.value;
          console.log('Changed hidden cloned field:' + hf.name + '=>' + hf.value);
        };
    }(hiddenClonedFields[i]));
    console.log ( 'Added cloned field: ' + hiddenClonedFields[i].name );
}


//Add MunchkinId to form
var input = document.createElement("input");
input.setAttribute("type", "hidden");
input.setAttribute("name", "munchkinId");
input.setAttribute("value", MARKETO_MUNCHKIN_ID);
firstForm.appendChild(input);
console.log('Added hidden field:' + input.name + '=' + input.value);
```

Se você tiver campos personalizados com nomes de API incompatíveis com Unbounce, poderá adicioná-los ao mapeamento no JavaScript. Por exemplo, se um de seus campos personalizados no Marketo era `Comments_c`, mas você queria que o rótulo do campo aparecesse como Comentários, Unbounce não permitiria alterar o nome do campo para `Comments_c`.

```
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['comments'] = 'Comments_c';
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
```

_comments é o nome do campo em Unbounce. _Comments_c_ é o nome do campo no Marketo. Para páginas futuras do Unbounce, você simplesmente vai para o JavaScript e ativa o script do Munchkin que criamos. Não há necessidade de recriá-lo.
**Parte 5: Testando** A última etapa é testar se esta integração de formulários está funcionando. Crie um acionador no Marketo que é ativado no preenchimento do formulário do Marketo e verifique se os clientes em potencial estão sendo inseridos corretamente no Marketo. Depois que o formulário for enviado, a página deverá redirecioná-lo para a URL de acompanhamento.

Publicado em _2014-08-04_ por _

## Atualizações da versão de julho de 2014

### Atualizações de Munchkin

A nova versão do Munchkin é a 141. A versão 142 não é compatível e será removida no início de setembro de 2011. Na versão 142, havia funções publicamente acessíveis que não estavam documentadas no site dos desenvolvedores. Essas funções não documentadas não estão mais disponíveis ao público. As funções documentadas no site dos desenvolvedores continuarão sendo compatíveis a longo prazo.

### Atualizações da RTP

A API do RTP tem uma nova função chamada Obter dados do visitante. Essa chamada de API RTP permite obter dados do visitante em tempo real, como organização, setor, local e correspondência de código de segmento.

Publicado em _2014-07-30_ por _Murta_

## Utilização de vários códigos de rastreamento do Munchkin em uma única página

Digamos que você tenha várias instâncias do Marketo e deseje enviar eventos de rastreamento web como visitas de página ou links clicados para essas várias instâncias. É possível fazer isso com o Munchkin. O Marketo rastreia os visitantes do seu site por domínio (por exemplo, &quot;marketo.com&quot;). Se você estiver hospedando esse script do Munchkin em um domínio diferente do seu domínio primário (por exemplo, &quot;company.com&quot;), esses visitantes aparecem como clientes potenciais anônimos até que preencham um formulário nesse outro domínio. Para fazer isso, adicione o parâmetro `altIds` à sua chamada `Munchkin.init`. O parâmetro altIds contém uma matriz das Munchkin IDs adicionais para as quais os eventos da Web devem ser enviados. Usando o exemplo abaixo, substitua as IDs do Munchkin destacadas (XXX-XXX-XXX, YYYY-YYY e ZZZ-ZZZ-ZZ) pelas IDs do Munchkin de cada instância do Marketo para a qual as informações de rastreamento devem ser enviadas.

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"></script>
<script>Munchkin.init('XXX-XXX-XXX', { altIds:['YYY-YYY-YYY', 'ZZZ-ZZZ-ZZZ'] });</script>
```

Para obter informações adicionais sobre os parâmetros de inicialização do Munchkin, consulte [este documento](/help/javascript-api/configuration.md).

Publicado em _2014-08-08_ por _Murta_

## Integração do Munchkin com o Google Tag Manager

O Google Tag Manager permite adicionar tags ao seu site. Em vez de adicionar manualmente cada script de rastreamento, como o Munchkin, ao código-fonte do seu site, você pode colocar o [Google Tag Manager](https://marketingplatform.google.com/about/tag-manager/) no seu site e adicionar tags, como o [Munchkin](/help/javascript-api/lead-tracking.md), por meio da interface do usuário do Google Tag Manager. Nesta publicação, primeiro mostraremos como gerar o código de rastreamento do Munchkin no Marketo e, em seguida, mostraremos como adicionar esse código de rastreamento do Munchkin ao Google Tag Manager.

### Como gerar um código de rastreamento do Munchkin

1. Clique em **Administrador** na parte superior direita do aplicativo.
1. Clique em **Munchkin** na árvore à esquerda.
1. Selecione **Assíncrono** para o Tipo de Código de Rastreamento.
1. Clique em e copie o código de rastreamento do JavaScript.

**Como adicionar o código de rastreamento do Munchkin ao Gerenciador de tags da Google**

1. Faça logon em sua conta do Google Tag Manager e **Adicione uma nova tag.**
1. Crie uma nova **Marca HTML Personalizada**.
1. Copie e cole seu código Munchkin no campo **HTML** e clique em **Continuar**.
1. Selecione **Acionar em Todas as Páginas** e clique em **Criar Marca.** Observação: se você tiver um site de tráfego extremamente alto, poderá excluir seções do seu site usando **Acionar em Algumas Páginas**.
1. Clique em Salvar e verifique se o código de rastreamento do Munchkin está sendo carregado no site.

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-08-05_ por _Murta_

## Ocultar Lightbox após o envio do formulário

### Visão geral

O código incorporado atual do lightbox gerado pelo Marketo não desaparece quando o formulário é enviado. Ele recarregará a página após o envio do formulário e o formulário aparecerá novamente. Se você quiser criar um lightbox que se oculta após o envio do formulário, siga as etapas abaixo.

### Guia de instruções

1. Depois de criar o formulário no Marketo, gere o código incorporado. Para fazer isso, clique em Ações de formulário e em Lightbox como o tipo de código. Copie e cole esse código em um editor de texto para editá-lo na próxima etapa.
1. Substitua todo o código depois de &quot;(formulário)&quot; no seu código incorporado pelo código abaixo:

```javascript
{
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Após a etapa 2, a versão concluída deve se parecer com o código abaixo. O código agora está pronto para ser usado no site.

```javascript
<script src="http://app-sj04.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_0000"></form>
<script>MktoForms2.loadForm("http://app-sj04.marketo.com", "AAA-BBB-CCC", 0000, function (form){
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-08-21_ por _Murta_

## Guia de início rápido para a API REST do Marketo

Este guia mostra como fazer sua primeira chamada para a API REST do Marketo em dez minutos. Mostramos como recuperar um único cliente potencial usando o ponto de extremidade de API REST [Obter Cliente Potencial por Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Para fazer isso, orientamos você pelo processo de autenticação para gerar um token de acesso, que você usa para fazer uma solicitação HTTP GET para [Obter Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Em seguida, fornecemos o código para fazer a solicitação que retorna informações de lead formatadas como JSON.

### Como gerar um token de autenticação

>[!NOTE]
> A partir de junho de 2025, o token de autenticação não será mais compatível. Você deve usar o cabeçalho Autenticação.
>

Um serviço personalizado no Marketo permite descrever e definir a quais dados seu aplicativo terá acesso. Você precisa estar conectado como administrador do Marketo para criar um Serviço personalizado e associar esse serviço a um único usuário somente API.

1. Navegue até a área de administração do aplicativo Marketo.
1. Clique no nó Usuários e funções no painel esquerdo.
1. Crie uma nova função. Revele a lista de permissões de função clicando em API de acesso. Agora, role para baixo e selecione apenas as permissões necessárias. Nesse caso, simplesmente selecionaremos a permissão Lead somente leitura.
1. A próxima etapa é criar um usuário Somente API e associá-lo à função API criada na etapa anterior. Você pode fazer isso marcando a caixa de seleção Usuário somente API no momento da criação do usuário.
1. É necessário um serviço personalizado para identificar exclusivamente o aplicativo cliente. Para criar um aplicativo personalizado, vá para a tela Admin > LaunchPoint e crie um novo serviço.
1. Forneça o Nome de exibição, escolha o Tipo de serviço &quot;Personalizado&quot;, forneça a Descrição e o endereço de email do usuário criado na etapa 1. Recomendamos usar um Nome de exibição descritivo que represente a empresa ou a finalidade deste Serviço de API REST personalizado.
1. Clique no link &quot;Exibir detalhes&quot; na grade para obter a ID do cliente e o Segredo do cliente. O aplicativo cliente pode usar a ID do cliente e o Segredo do cliente para gerar um token de acesso.
1. Copie e cole seu token de autenticação em um editor de texto. O token de autenticação é semelhante ao exemplo abaixo:

`cdf01657-110d-4155-99a7-f986b2ff13a0:int`

### Como determinar o URL do ponto de extremidade

Ao fazer uma solicitação para a API do Marketo, você precisa especificar sua instância do Marketo no URL do endpoint. Todas as solicitações de API não em massa para a API REST do Marketo seguirão o formato abaixo:

`<REST API Endpoint URL>/rest/`

O URL do ponto de extremidade da API REST pode ser encontrado no painel Administrador do Marketo > Serviços da Web. A estrutura do URL do endpoint do Marketo deve ser semelhante ao exemplo abaixo:

`https://100-AEK-913.mktorest.com/rest/v1/lead/{id}.json`

**Observação: as URLs de API em massa não apresentam o prefixo &#39;/rest/&#39;. Para a API em Massa, você deve usar somente o host e anexar o caminho apropriado da seguinte maneira:**

`https://100-AEK-913.mktorest.com/bulk/v1/leads/export/create.json`

### Como usar o token de autenticação para chamar o lead por AP de ID

Nas seções anteriores, geramos um token de autenticação e encontramos o URL do endpoint. Agora faremos uma solicitação para um ponto de extremidade de API REST chamado [Obter lead por ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). A maneira mais fácil de fazer sua solicitação para a API REST do Marketo é colar o URL na barra de endereços do navegador da Web. Siga o formato abaixo:

`https://<REST API Endpoint URL for your Marketo instance>/rest/v1/<API that you are calling>?access_token=<access_token>`

### Exemplo

`https://100-AEK-913.mktorest.com/rest/v1/lead/318581.json?access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

Se a chamada for bem-sucedida, ela retornará JSON com o formato abaixo:

```json
{
    "requestId": "d82a#14e26755a9c",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2015-06-11T23:15:23Z",
            "lastName": "Doe",
            "email": "<jdoe@marketo.com>",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "John"
        }
    ],
    "success": true
}
```

Se você estiver interessado em saber mais sobre as APIs REST do Marketo, este é um [bom ponto de partida](https://developer.adobe.com/marketo-apis/).

Publicado em _2015-09-04_ por _David_

## Como excluir cookie de rastreamento do Marketo

Pergunta: &quot;Configuramos um formulário em nosso site para que a equipe de vendas cancele a inscrição de indivíduos que pediram verbalmente para serem removidos de qualquer mensagem de email. No entanto, o que estamos descobrindo é que quando eles entram no email e enviam que são cozidos com esse endereço de email e começam a receber alertas para sua atividade em nosso site. O formulário que temos atualmente é um formulário incorporado. Alguém descobriu uma maneira de desativar o rastreamento de cozinheiros em um formulário incorporado? Eu sei como fazer isso com uma landing page do Marketo.&quot; Nós podemos fazer isso. Para implementar isso, acelere a expiração do cookie. Depois que o cookie é criado pelo formulário, você pode forçá-lo a expirar imediatamente. Como o cookie expirou, o navegador o excluirá automaticamente. Para iniciar esse processo, vamos usar a funcionalidade de formulário nativo para chamar na função chamada `deleteCookie`.

Publicado em _2014-08-26_ por _Travis Kaufman_

## Integrar uma página de aterrissagem do Marketo com o Wordpress

Digamos que seu site seja criado usando o Wordpress, e você gostaria de incorporar uma landing page Marketo em uma das páginas. Isso é possível usando um iframe. Um iframe permite incorporar uma página em outra página. Nesta publicação, você mostra como incorporar uma landing page do Marketo em uma página do Wordpress. **Escolhendo um plug-in do Wordpress** Wordpress Existem vários plug-ins do Wordpress que permitem a você: `http://wordpress.org/plugins/iframe/` Veja alguns profissionais e esperas ao usar esta abordagem: `http://www.elixiter.com/marketo-landing-page-and-form-hosting-options/` Ótimo post Murtza! Colby, o iframe preservaria a parte em que, após enviar o formulário, você é redirecionado para uma PDF. Usamos iframes em nosso site por muito tempo. Ótimo post Murtza! Colby, o iframe preservaria a parte em que, após enviar o formulário, você é redirecionado para uma PDF. Usamos iframes em nosso site por muito tempo. Observe que, se você quiser passar parâmetros de URL do URL principal para o iframe, é necessário fazer alguma codificação. Além disso, verifique se o Google Analytics está configurado corretamente. Você não quer ter exibições de página contadas duas vezes para cada visita à página com o iframe.

Publicado em _1970-01-01_ por _Murta_

## Integrar o Otimizely a uma página de aterrissagem do Marketo

[Com otimização](https://www.optimizely.com/), você pode realizar testes A/B, multivariados e multipáginas no seu site. Você pode usar o Otimizely com uma página de aterrissagem do Marketo. Veja como fazer isso:

1. Localize e copie o trecho de código Otimizely.** Vá para o Painel em Otimizely e clique no link Código do projeto. Copie a linha de código que aparece no pop-up.
1. Faça logon no Marketo e selecione o template da landing page. Em seguida, clique em &quot;Editar rascunho&quot;.**
1. Clique em Ações da landing page. Em seguida, clique em Editar metatags da página**
1. Cole o trecho de código Otimizely na seção HEAD HTML personalizada e clique em Salvar.
1. Teste a landing page para confirmar se o trecho Otimizely está funcionando

Publicado em _2014-09-18_ por _Murta_

## Pesquisar por nome completo por meio da API REST do Marketo

**Pergunta:** Há uma maneira de consultar um cliente potencial usando APIs do Marketo apenas com o nome completo do cliente potencial?
**Resposta:** Não é diretamente possível. No entanto, a solução alternativa descrita abaixo permitirá que você faça isso.

1. Crie um campo personalizado chamado &quot;Fullname&quot; no Marketo.
1. Use a API do SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md) ou o [Get Multiple Leads por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) para consultar seu banco de dados de clientes potenciais. Inclua seu nome e sobrenome como atributos em sua solicitação para as APIs REST ou SOAP.
1. Depois de consultar o banco de dados de clientes potenciais, concatene &quot;Nome&quot; e &quot;Sobrenome&quot; para cada cliente potencial e armazene esses dados em uma coluna &quot;Nome completo&quot;. 1. Use a API do SOAP [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) para enviar esses dados para o campo personalizado &quot;Fullname&quot;. Como alternativa, você pode usar a API [Importar cliente em potencial](/help/rest-api/leads.md) ou importar um CSV ou XLS usando a interface do usuário do Marketo.
1. Agora, você pode consultar pelo nome completo usando a [Obter Vários Clientes Potenciais por API de Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) para pesquisar esse campo personalizado. Especifique &quot;Fullname&quot; como &quot;filterType&quot; e &quot;filterValue&quot; como &quot;Joe Johnson&quot; com uma chamada de API REST Obter vários clientes em potencial por tipo de filtro.

Publicado em _2014-09-09_ por _Murta_

## Excluir cookie de rastreamento do Marketo

Esse método só deve ser usado se o efeito desejado for remover o cookie completamente.

Esta amostra de código pode ser usada para excluir um cookie de rastreamento do Marketo do navegador de um usuário após o envio do formulário do Marketo. Funciona chamando o método `deleteMarketoCookie` depois que um usuário envia um formulário. Este método expira o cookie definindo a data de expiração como uma data no passado. O comportamento padrão do navegador é excluir esse cookie, pois ele expirou.

Publicado em _2014-09-09_ por _Murta_

## Restringir Domínios de Email Livres no Preenchimento de Formulário

Digamos que você gostaria de restringir os visitantes do site de enviar um formulário com um domínio de email gratuito, como Gmail ou Yahoo. Para fazer isso, inclua o script abaixo no código-fonte da página com o formulário Marketo. Ele verifica se um usuário inseriu um email não comercial (Gmail, Hotmail etc.) e impede o envio de formulários do Marketo se um usuário inserir um. É possível estender a lista de domínios de email bloqueados modificando a linha 9 para incluir os domínios que você deseja restringir.

Publicado em _2014-09-09_ por _Murta_

## Depuração de um campo que não está acessível por meio da API

Se você estiver acessando este campo <https://nation.marketo.com/> Quando tentarmos atualizar o campo AnnualRevenue usando a API do SOAP, a resposta indicará que o registro foi atualizado. No entanto, o campo AnnualRevenue não manterá a alteração. Se tentarmos atualizar o campo manualmente usando o banco de dados de clientes potenciais, as alterações neste campo também não serão persistentes. Verifique se o campo está bloqueado no Admin. A outra coisa que pode acontecer às vezes é que ele pode ser um campo de conta sincronizado do SFDC. Bloqueamos as alterações nesses campos por padrão, pois o SFDC é o sistema de registro para eles em muitos casos. 1) Criado Em 2) Marketo SFDC ID 3) Código Exclusivo Marketo 4) SFDC Tipo 5) Atualizado Em 6) Urgência 7) Prioridade 8) Data De Criação Das Vendas

Publicado em _1970-01-01_ por _Murta_

## Adicionar código personalizado a uma página de aterrissagem do Marketo

Suponhamos que você deseje adicionar um script de rastreamento de terceiros, como o Google Analytics, à sua página de aterrissagem do Marketo. Isso é possível por meio da interface do Marketo. Em geral, você pode adicionar qualquer código personalizado (HTML, CSS, JavaScript) à sua landing page do Marketo seguindo as instruções abaixo.

1. Selecione a página de aterrissagem e clique em Editar rascunho.
1. Arraste no elemento HTML.
1. Insira o código personalizado e clique em Salvar.

Publicado em _2014-09-10_ por _Murta_

## Publicação de formulário no lado do servidor

`https://community.marketo.com/MarketoResource?id=kA650000000GsXXCA0`

Publicado em _2014-09-11_ por _Murta_

## Limpeza do cookie de rastreamento do Marketo a partir de envios do Forms 2.0

### Visão geral

O Forms 1.0 continha o valor do cookie de rastreamento do Munchkin como um campo no DOM. Esta informação foi apresentada juntamente com todos os outros dados. O [Forms 2.0](/help/javascript-api/forms-api-reference.md) omite este campo e preenche dinamicamente o valor no envio, em vez de no carregamento do formulário. Embora isso seja geralmente aceitável, cria uma classe de casos de uso, que exigem que o cookie de rastreamento seja limpo para evitar rastreamento e preenchimento prévio não intencionais. Por exemplo, isso pode ocorrer em uma feira de negócios em que um cliente do Marketo está usando o mesmo formulário no mesmo dispositivo e obtendo informações de contato de várias pessoas. O trecho abaixo permitirá que você limpe o valor do cookie no envio do formulário, sem precisar excluir o próprio cookie do navegador do usuário.

### Amostra de código

Este trecho espera um único carregamento de formulário na página. Se houver vários formulários, você deverá usar os [métodos loadForm ou getForm](/help/javascript-api/forms-api-reference.md) para implementar seus retornos de chamada.

```javascript
<script>
//add a callback to the first ready form on the page
MktoForms2.whenReady( function(form){
 //add the tracking field to be submitted
        form.addHiddenFields({"_mkt_trk":""});
        //clear the value during the onSubmit event to prevent tracking association
 form.onSubmit( function(form){
  form.vals({"_mkt_trk":""});
 })
})
</script>
```

Publicado em _2014-09-11_ por _Kenny_

## Atualizações da versão de setembro de 2014

### Atualizações à API REST

Adição de um novo valor de campo opcional à API [Obter vários clientes em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), que retornará os valores de cookie do Munchkin associados a um registro de cliente potencial. Basta adicionar &quot;?fields=cookies&quot; à solicitação.

Publicado em _2014-09-16_ por _Murta_

## Adicionar dados de localização da API RTP ao Preenchimento de formulário do Marketo

**Você precisa de assinaturas RTP e MLM ativas para implementar o caso de uso descrito nesta publicação do blog.**
Usando as [APIs do RTP JavaScript](/help/javascript-api/web-personalization.md) e o [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md), você pode obter os dados de localização inferidos do RTP e enviá-los para o Marketo por meio de um preenchimento de formulário. Isso permite que você veja o local inferido do usuário (com base no endereço IP) durante a atividade mais recente do formulário. Para começar, você precisa criar três campos de sequência personalizados no Marketo. Você pode fazer isso por meio do CRM, se ele tiver uma integração nativa com o Marketo, ou pelo menu Gerenciamento de campo na seção de administrador do Marketo. Recomendo nomear esses campos como &quot;País mais recente&quot;, &quot;Estado mais recente&quot; e &quot;Cidade mais recente&quot;. Continuamos este blog usando essa convenção de nomenclatura. Os nomes da API para esses campos são &quot;mostRecentCountry&quot;, &quot;mostRecentState&quot; e &quot;mostRecentCity&quot;. Para recuperar os dados de localização, estamos usando o [método RTP para obter os dados de localização do visitante](/help/javascript-api/web-personalization.md) e, em seguida, passá-los para o formulário usando os [métodos addHiddenFields e vals](/help/javascript-api/forms-api-reference.md) do Marketo Forms 2.0. Na página, adicione a tag JS RTP e um formulário do Marketo. Em seguida, inclua o script abaixo. Você precisa alterar os nomes dos campos de formulário de destino no código de exemplo se estiver usando uma convenção de nomenclatura diferente da descrita acima.

```javascript
<script>
//modify the form and grab the user
MktoForms2.whenReady( function(form) {
        //add the hidden fields to the form
 form.addHiddenFields({
  "mostRecentCountry":"",
  "mostRecentState":"",
  "mostRecentCity":""});
 //Grab the visitor data, a JS object with it is passed in the callback function of the third argument
 rtp('get','visitor',function(visitor){
  //add the desired info from the visitor object to the form fields
  form.vals({
   "mostRecentCountry":visitor.results.location.country,
   "mostRecentCity":visitor.results.location.city,
   "mostRecentState":visitor.results.location.state});
  }
  );
 });
</script>
```

Publicado em _2014-09-17_ por _Kenny_

## Rastreamento de Blocos e Indexação de Pesquisa de uma Landing Page do Marketo

Suponhamos que você deseje bloquear o rastreamento e a exibição de uma landing page do Marketo nos resultados por mecanismos de pesquisa como o Google. Veja como fazer isso:

1. Faça logon no Marketo e selecione a landing page. Em seguida, clique em &quot;Editar rascunho&quot;.
1. Clique em Ações da landing page. Em seguida, clique em Editar metatags de página
1. Altere o campo Robots para No Index, No Follow. Em seguida, clique em Salvar.

Publicado em _2014-09-19_ por _Murta_

## Perguntas frequentes sobre Marketo REST versus APIs do SOAP

**Atualizado: março de 2016** Aqui estão as respostas para as perguntas mais frequentes sobre as APIs do [REST](/help/rest-api/rest-api.md) e do [SOAP](/help/soap-api/soap-api.md) da Marketo. **P: Quais são as principais diferenças entre as APIs REST e SOAP do Marketo?** A: Embora a capacidade de enviar/receber dados específicos por meio das APIs REST e SOAP se sobreponha, há certas funcionalidades que só existem nas APIs REST ou SOAP. Em termos de desempenho, a API REST tem [taxa de transferência](https://en.wikipedia.org/wiki/Throughput) melhor que a API SOAP. Em termos do modelo de autenticação, a API REST tem um modelo de autenticação que usa um token que expira. Nossa API REST também fornece acesso aos [ativos](https://developer.adobe.com/marketo-apis/api/asset/) do Marketo.   **P: Que recursos estão disponíveis na API REST e que não estão disponíveis na API do SOAP?** A: [Lista de APIs de listas](/help/rest-api/list-of-standard-fields.md), [remover um cliente potencial de uma API de lista](/help/rest-api/lead-database.md), [API de Uso](/help/rest-api/rest-api.md) e [API de Erro](/help/rest-api/rest-api.md) só estão disponíveis com a API REST. **P: Há planos para aumentar o número de APIs disponíveis para a API do SOAP?** A: Não. **P: Há planos para aumentar o número de APIs disponíveis para a API REST?** A: Sim. O REST é o foco principal do desenvolvimento de API da Marketo no momento.

Publicado em _2014-09-20_ por _Murta_

## Publicação de formulário no lado do servidor

**Observação: esta é uma API não pública e sem suporte, não há suporte para ela e seu comportamento pode mudar a qualquer momento** Se você usar seus próprios formulários no site, ainda poderá enviar esses dados para o Marketo usando uma publicação do lado do servidor. A vantagem dessa abordagem é que você pode manter o formulário existente e a lógica do aplicativo, mas ainda pode usar uma publicação de formulário real no Marketo. Isso fornece aos usuários do Marketo um evento &quot;Preencher formulário&quot;, que pode ser usado para acionar processos automatizados ou para segmentação. **OBSERVAÇÃO: há um limite de taxa de 30 postagens de formulários do lado do servidor por minuto a partir de um único endereço IP.** Em cada linguagem de programação ou script, há diferentes opções para enviar um formulário do lado do servidor, e elas podem ter diferentes objetos/métodos que podem ser usados para fazer a Chamada Post. Por exemplo, no PHP, muitas pessoas usam a biblioteca cURL. Em todos os casos, você está postando pares de nome-valor em um URL especificado. Os nomes precisam ser idênticos aos nomes da API dos campos do Marketo. Além disso, alguns campos de sistema precisam ser incluídos para capturar corretamente o envio do formulário.

1. Crie um formulário. A primeira etapa é criar um formulário no Marketo ou usar um formulário existente que você deseja enviar. O nome do formulário precisa ser descritivo, mas não precisa de nenhum campo de formulário. Se você criar um novo formulário, basta inserir um nome, desmarcar a caixa &quot;Abrir editor de formulário&quot; e pronto.
1. Encontrar uma ID de formulário. Na interface do usuário do Marketo, selecione o formulário e observe a URL: ela deve estar no formato `https://app-x.marketo.com/#FO8B2ZN12`. Atrás do sinal #, observe o número imediatamente após &quot;FO&quot; para encontrar a ID do formulário. Nesse caso, a ID do formulário é 8. Em alguns casos, seu primeiro formulário pode ser numerado como 1001 e contar a partir daí. A ID do formulário é uma variável, para que você possa acionar o envio de formulários diferentes.
1. Obtenha sua ID de conta da Marketo. Acesse Admin > Munchkin e copie a ID de conta do Munchkin, que tem o formato 000-AAA-000. Isso é necessário para que o formulário seja enviado para a instância correta do Marketo.
1. Determine o URL do POST. Quando estiver na interface do usuário do Marketo, observe o domínio na barra de localização, geralmente no formato `<http://app-x.marketo.com/>`. Descarte qualquer coisa após a barra e, em seguida, anexe &quot;index.php/leadCapture/save&quot; para obter o URL POST do formulário completo. Observação 1: diferencia maiúsculas de minúsculas. Observação 2: as sandboxes da Marketo podem ter um domínio diferente do seu sistema de produção Marketo. Portanto, um exemplo de URL seria: `http://app-x.marketo.com/index.php/leadCapture/save` Você também pode usar HTTPS em vez de HTTP (não use seu CNAME, pois ele fornece uma exceção de segurança).
1. Encontre os Nomes dos campos de formulário** Vá para Admin > Gerenciamento de campos e clique no botão &quot;Exportar nomes de campos&quot; para baixar uma planilha com os nomes de campos da API. Use o nome da API como o nome em seus pares de nome-valor.
1. Decida quais campos publicar. É possível incluir qualquer campo de cliente em potencial da Marketo no envio do formulário. Observe que os nomes de campo diferenciam maiúsculas de minúsculas. Além dos campos que você deseja enviar, há dois campos obrigatórios e dois campos recomendados: Campos obrigatórios no formulário: (1) `munchkinId` - Este campo é usado para a ID da conta da Munchkin (2) `formid` - Este campo indica qual formulário no Marketo foi enviado Campos recomendados no formulário: (1) Email - este campo é usado como chave primária para desduplicação. Se o Marketo encontrar um endereço de email correspondente no banco de dados do Marketo, ele atualizará o registro existente; caso contrário, criará um novo registro. Se houver várias correspondências, ele atualizará o registro atualizado mais recentemente (2) `_mkt_trk`. Esse campo contém as informações do cookie para que você possa rastrear as visitas às páginas da Web de cada indivíduo. Se você tiver o Munchkin na página do formulário, o Munchkin inserirá automaticamente um valor nesse campo de formulário oculto. Caso contrário, leia-o do cookie com o mesmo nome e transmita-o para a Marketo neste campo. Observação: o corpo da mensagem de POST para um formulário do Marketo deve ser codificado em URL.
1. Consulte a Resposta** A resposta para a publicação do formulário será um código de redirecionamento HTTP 302. Em alguns sistemas, isso aparecerá como um erro. No entanto, nesse caso, significa que o lead foi criado ou atualizado com sucesso. Se houver um erro, você receberá um código de erro 4xx ou 5xx.

Esta é uma [publicação](https://nation.marketo.com:443/t5/product-blogs/how-to-build-an-external-subscription-center/ba-p/242185) sobre o uso desta técnica para cenários de cancelamento de inscrição [ de Justin Cooperman, Gerente de Produto Sênior]

Publicado em _2014-11-07_ por _Murta_

## Localizar clientes em potencial atualizados em um intervalo de datas específico

Digamos que você queira encontrar clientes potenciais que foram atualizados em datas específicas por meio da [API do Marketo](/help/soap-api/soap-api.md). Isso é possível com a [API do SOAP getMultipleLeads](/help/soap-api/getmultipleleads.md). Este método retorna qualquer cliente em potencial com uma alteração no valor dos dados ou nova atividade no Marketo para o intervalo de datas solicitado. Para `leadSelector`, você especificaria `LastUpdateAtSelector`. Em seguida, você definiria os intervalos de datas com `oldestUpdatedAt` e `latestUpdatedAt` limites de tempo. Consulte o exemplo XML de solicitação abaixo, que mostra como encontrar clientes em potencial que foram atualizados entre 12:00 PST em 6 de junho de 2014 e 12:00 PST em 7 de junho de 2011. Observação: o intervalo de datas não deve exceder 30 dias.

**Exemplo de XML de Solicitação para Localizar Clientes Potenciais Atualizado por Data**

```xml
<soapenv:Envelope xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
<soapenv:Header>
<mkt:AuthenticationHeader>
<mktowsUserId>REPLACE</mktowsUserId>
<requestSignature>REPLACE</requestSignature>
<requestTimestamp>2014-10-23T12:19:51-07:00</requestTimestamp>
</mkt:AuthenticationHeader>
</soapenv:Header>
<soapenv:Body>
<mkt:paramsGetMultipleLeads>
<leadSelector xsi:type="mkt:LastUpdateAtSelector">
<oldestUpdatedAt>2014-06-06T00:00:00-08:00</oldestUpdatedAt>
<latestUpdatedAt>2014-06-07T00:00:00-08:00</latestUpdatedAt>
</leadSelector>
</mkt:paramsGetMultipleLeads>
</soapenv:Body>
</soapenv:Envelope>
```

Publicado em _2014-09-24_ por _Murta_

## Adicionar conteúdo dinâmico a um email

Digamos que você envie um email diário e queira incluir automaticamente a data desse dia no modelo de email. Para fazer isso, use scripts de tokens e email no Marketo.

1. Crie um token.** Navegue até o programa em que deseja usar o token. Clique em Meus tokens.
1. Clique duas vezes em Script de email. Em seguida, nomeie este token. Em seguida, clique em Editar.
1. Cole o script de email abaixo nesta janela. Em seguida, clique em Salvar.

## Acessar o objeto de calendário do Velocity

`set($x = $date.calendar)`

## Formatar data

`set($current_date = $date.format('dd-MM-yyyy', $x.getTime()))`

## Retorna a data de hoje

`$current_date`

1. Faça referência ao token no modelo de email.** Anote o nome do token. Navegue até o rascunho do seu email. Inclua o token.  Quando o email for enviado, o valor do token será preenchido. Para obter mais informações, consulte a [documentação do desenvolvedor de Script de email](https://experienceleague.adobe.com/pt-br/docs/marketo-developer/marketo/email-scripting).

Publicado em _2014-11-22_ por _Murta_

## Aviso de segurança Bash

A Marketo realizou uma investigação completa da vulnerabilidade do Bash, também conhecida como [Shellshock (CVE-2014-6271)](https://nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-6271), e concluiu que não somos susceptíveis a esses ataques. Além disso, adotamos medidas preventivas, atualizando o software para a versão mais recente para garantir a conformidade com a [recomendação do CERT](https://www.cisa.gov/news-events/alerts/2014/09/25/gnu-bourne-again-shell-bash-shellshock-vulnerability-cve-2014-6271).

Publicado em _2014-09-26_ por _Murta_

## Como atualizar credenciais da API do SOAP

É uma prática recomendada atualizar regularmente suas credenciais da [API do SOAP](/help/soap-api/soap-api.md). Atualmente, não há como fazer isso programaticamente por meio da API do Marketo. As instruções abaixo mostrarão como atualizar suas credenciais da API do SOAP por meio da interface do usuário do Marketo.

1. Vá para a seção Admin e clique em Web Services.
1. Defina uma Chave de criptografia com pelo menos 10 caracteres e clique em Salvar alterações.

Publicado em _2014-09-29_ por _Murta_

## Como limpar o banco de dados do Marketo

**OBSERVAÇÃO: esta é uma publicação de blog de convidado. Josh Hill é o líder de práticas da Marketo na Perkuto, uma agência de automação de marketing. Josh trabalha na interseção de marketing, vendas e tecnologia para fornecer sistemas de geração de receita. Ele escreve sobre automação de marketing e geração de demanda em [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).** Manter o banco de dados do Marketo limpo é uma parte importante da administração desse sistema eficiente. Se os seus dados do Marketo estiverem alterados ou os seus dados do CRM estiverem alterados, seu trabalho como gerente de marketing se tornará muito mais difícil à medida que você explica por que as campanhas foram para as pessoas erradas ou os seus relatórios são &quot;direcionais&quot;. [Um estudo de 2011 da Gartner observou](https://www.data.com/export/sites/data/common/assets/pdf/DS_Gartner.pdf) que a baixa qualidade dos dados reduziu a produtividade do trabalho em 20%. Isso representa 20% do seu tempo (8 horas por semana ou 1 dia por semana) desperdiçado porque você precisava corrigir os dados. Mas essa perda se empalidece em comparação com a receita perdida porque seu direcionamento estava errado. Estas são as cinco principais razões para investir em manter seus dados limpos: - Uma melhor segmentação de leads e clientes pode ser feita, permitindo que você concentre sua mensagem nas pessoas certas na hora certa. Esse cupom de 20% de desconto deve ser destinado apenas a novos clientes potenciais, não aos seus melhores clientes, certo? - Evite o envio duplicado de emails. Apesar de todas as precauções, é possível enviar vários emails para o mesmo lead, geralmente porque os registros duplicados não são mesclados. - Relatórios precisos para o seu chefe. Como o CMO tem uma posição na tabela de receita, você deve ter relatórios precisos, consistentes e repetíveis... ou não pode mais ser um profissional de marketing de receita. - Prejudicar as negociações pendentes se você enviar por email o material de marketing incorreto para os principais clientes potenciais durante as negociações de vendas.

Se o banco de dados não estiver fornecendo esses detalhes no estágio do ciclo de vida, isso pode afundar um ou dois negócios. - Remover clientes em potencial ruins ou antigos para ficar abaixo dos limites de preço. Muito se escreveu sobre o custo de dados ruins. Sua preocupação mais imediata é que muitos clientes em potencial ruins, duplicados ou antigos estão forçando você a ultrapassar o tipo de preços da Marketo ou da Salesforce, o que pode resultar em milhares de taxas por ano. Então, como você mantém seu banco de dados limpo? Você pode seguir esses princípios de limpeza de dados, mas como chegar lá dentro do Marketo? Eu faço algumas suposições sobre o seu sistema: - Você tem um sistema Marketo padrão. - Você tem uma configuração típica do Salesforce de conta de contato de cliente potencial. - Você está sincronizando todos os registros entre sistemas. - Você não está usando duplicatas propositais.

Encontre os clientes em potencial certos para corrigir** Vamos primeiro encontrar os clientes em potencial que são problemas em potencial. Com cada cliente, eu passo por uma série de listas inteligentes para determinar a integridade de seus bancos de dados. Sugiro que faça o mesmo mensalmente. Quando as listas inteligentes estiverem funcionando, você não levará mais do que 15 minutos por mês. Essa é uma ótima maneira de demonstrar o ROI do seu trabalho e do Marketo. Crie uma tabela para entender o impacto total no banco de dados. O exemplo abaixo inclui os critérios que procuro, portanto, inclua outros campos importantes para sua empresa. Detalhe cada grupo por Somente Marketo, SFDC Leads e Contatos da SFDC.

Usar automação para corrigir valores de dados: o uso de regras de automação para corrigir erros ortográficos comuns ou dados ausentes remonta a várias décadas. Na década de 1960, a baixa qualidade dos dados durante o envio de mala direta poderia resultar em um desperdício de milhares de dólares. Hoje, erros de qualidade de dados arruínam reputações mais rapidamente do que uma peça de correio mal direcionada. A reputação do email, a escolha de idioma e a experiência do cliente são importantes, e são mais importantes porque os erros podem se tornar virais publicamente em minutos. Salve a reputação da sua empresa com a limpeza automatizada de dados. Esses fluxos de gestão de dados são uma das primeiras configurações no Marketo. A maioria das empresas configura fluxos semelhantes: - Corretor de país (embora você devesse ter seguido o Princípio 1 para não precisar disso) - Corretor de estado ou Mapeador - geralmente útil se você tiver País e Estado inferido. - Contagem de Funcionários para Faixa de Funcionários. - Source com lead inválido para Source com bom lead - O email inválido para email é bom se o email for alterado. - Traduzir valores de campos antigos para um novo campo (Completo para Vazio). Por exemplo, esse fluxo ajusta a Faixa de Funcionário com base no Número do Funcionário.

Ferramentas de anexação de dados Preencher campos vazios é essencial para segmentar melhor seu banco de dados. Os clientes em potencial nem sempre preenchem o Setor ou a Função corretos ou até mesmo o Título em um formulário. Às vezes, você tem dados herdados de um sistema mais antigo. Essa falta de dados significa que você precisa enviar esse email para menos pessoas ou, possivelmente, para as pessoas erradas. Para corrigir isso, você precisa de ferramentas de anexação de dados.

Opção 1: Preencha você mesmo. Talvez seja possível interpolar dados para preencher retroativamente campos vazios. Talvez você tenha um código SIC em vez do nome do setor ou da Receita anual vs. intervalo da Receita anual. O Marketo pode automatizar facilmente essas correções.

Opção 2: encontre um fornecedor de acréscimo/enriquecimento de dados via LaunchPoint Há [vários fornecedores no Launchpoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO), como NetProspex e ReachForce, que podem ajudá-lo a enriquecer seus dados de clientes potenciais. Alguns pedem uma folha de seus dados, que eles então limpam e enviam de volta. A melhor opção é uma ferramenta automatizada no Marketo ou no Salesforce, que verifica os campos que você deseja e, em seguida, envia os dados corretos. A maioria dos fornecedores usa a [API do Marketo ou os Webhooks](/help/home.md) para fazer isso.

Opção 3: usar APIs do Marketo para atualizar clientes potenciais Você pode usar as APIs do Marketo para identificar clientes potenciais que precisam ser limpos e, em seguida, atualizá-los por meio da API. A [Obter Vários Clientes Potenciais por API REST de Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) é um bom ponto de partida para obter dados do Marketo que correspondem a determinados critérios. Para atualizar clientes em potencial, verifique a [API REST Criar/Atualizar clientes em potencial](/help/rest-api/leads.md).

Como alternativa, você pode configurar um [Webhook do Marketo](/help/webhooks/webhooks.md) para notificar um sistema externo que um determinado evento aconteceu, como o preenchimento de um formulário. Em seguida, você pode responder com valores para atualizar o lead.

O Que Você Deve Automatizar? Forneci algumas sugestões na Etapa 1. Mas se quisermos limpar mais do banco de dados, precisamos ir além da correção dos valores de dados. - Concorrente Blacklist: certifique-se de que você está automatizando a coleta e blacklist de seus concorrentes. Se você tiver parceiros concorrentes, ainda é bom marcá-los corretamente e colocá-los em uma lista para possivelmente suprimir.  - Várias rejeições permanentes: automatize definitivamente esse fluxo. Se um cliente potencial for rejeitado mais de duas vezes em um período de 30 dias, defina-o como Suspenso ou Inválido. Em seguida, consulte uma vez por mês para analisar se o problema foi um erro de digitação ou algo diferente.  - Várias rejeições temporárias em 30 dias: defina-as como marketing stopped=TRUE por 30 dias.  - Suspensão/exclusão de interceptação de spam: tenha cuidado se o produto indicar que as pessoas usam possíveis endereços de interceptação de spam. Consulte uma lista de armadilhas de spam. A lista inteligente.

**O que não automatizar** Nunca automatize a exclusão de clientes potenciais, pois há muito risco na exclusão em massa acidental dos registros errados. Em vez disso, revise uma vez por mês os leads marcados como Lixeira. Mas se você quiser excluir clientes em potencial enviados para a lixeira, execute um fluxo como esse com uma espera de 30 dias.

Avisos: usar a automação é uma ótima maneira de economizar tempo e manter o banco de dados limpo. Automação é uma faca de dois gumes porque se você configurá-lo errado, ele pode causar estragos em alguns minutos. Tenha cuidado ao passar por esses processos e manter todos informados. Geralmente, desaconselho a exclusão de quaisquer Contatos da SFDC, pois eles têm maior probabilidade de serem Oportunidades, Clientes ou ex-Clientes ativos. Pode ser exigido por Finanças ou assuntos legais que você retenha determinados registros e os Contatos estão anexados a esses registros. Em vez disso, concentre-se em Contatos que são rejeitados permanentemente, se tornam Inválidos ou não estão mais lá. Agora você conhece algumas técnicas para manter o banco de dados do Marketo limpo. Informe-nos se você tiver outras maneiras de automatizar esses processos usando a API ou os Webhooks!

Publicado em _2014-10-08_ por _Josh_

## Atualizações da versão de outubro de 2014

### Preenchimento prévio de página externa

Os formulários do Marketo não fornecem funcionalidade de preenchimento prévio nativa quando carregados fora de uma página de aterrissagem do Marketo. No entanto, ainda podemos implementar isso usando as [APIs do Marketo](/help/rest-api/rest-api.md) e a [API do JavaScript do Forms 2.0](/help/javascript-api/forms-api-reference.md/). A primeira etapa é recuperar os dados de clientes potenciais do Marketo por meio de uma chamada REST do servidor. Supondo que não tenhamos uma maneira imediata de cruzar IDs de clientes potenciais ou outro identificador exclusivo do servidor, precisamos usar o cookie &#39;_mkto_trk&#39; do Munchkin para recuperar dados do servidor do Marketo, usando o [método Obter Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).
Para fazer essa chamada, precisamos dos endpoints de Autenticação e REST da sua instância. Depois de autenticar com sua instância do Marketo, é necessário fazer uma chamada para a API de clientes potenciais em `https://<host>/rest/v1/leads.json`. Em seguida, precisamos criar uma sequência de consulta para filtrar no cookie do Marketo como este `?filterType=cookie&filterValues=`. Você precisa recuperar o valor específico da chave &#39;_mkto_trk&#39; enviada ao servidor pelo cliente. OBSERVAÇÃO: O valor do cookie _mkto_trk inclui um E comercial (&amp;) e precisa ser codificado na URL para `%26` para ser aceito corretamente pelo ponto de extremidade do Marketo. Por padrão, a API de clientes potenciais retorna quatro campos: `id`, `email`, `firstName` e `updatedAt`. Para definir um conjunto específico de campos, você precisa incluir um parâmetro de consulta `fields`, com nomes de campo separados por vírgulas, desta forma: `&fields=email,firstName,lastName,company`. Em última análise, nossa chamada será assim:

`https://<host>/rest/v1/leads.json?filterType=cookie&filterValues=<cookie>&fields=email,firstName,lastName,company&access_token=<token>`

Quando fazemos essa chamada, ela retorna um objeto JSON com esta aparência:

```json
{
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
 "lastName":"Elkington",
        "email":"<mkto@example.com>",
 "company":"Marketo Inc."
        }]
};
```

Agora que temos nossos detalhes de cliente potencial, podemos mapeá-los em um formulário do Marketo, usando os métodos whenReady e vals no JavaScript. Primeiro, precisamos definir os resultados de cliente potencial como uma variável em nossa página:

```javascript
<script>
//print your JSON object dynamically as the mktoLead variable
var mktoLead = {
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
  "lastName":"Elkington",
        "email":"mkto@example.com",
  "company":"Marketo Inc."
        }]
}
</script>
```

Agora que temos os resultados na página, podemos mapeá-los em nossos campos de formulário:

```javascript
<script>
MktoForms2.whenReady( function(form) {
 //set the first result as local variable
 var mktoLeadFields = mktoLead.result[0];
    //map your results from REST call to the corresponding field name on the form
 var prefillFields = {
   "Email" : mktoLeadFields.email,
   "FirstName" : mktoLeadFields.firstName,
   "LastName" : mktoLeadFields.lastName,
   "Company" : mktoLeadFields.company
   };
 //pass our prefillFields objects into the form.vals method to fill our fields
 form.vals(prefillFields);
 }
 );
</script>
```

Publicado em _2014-10-24_ por _Kenny_

## Substituir HTML de e-mail

Esta publicação mostra como remover a HTML gerada pelo Marketo para um email. Você pode usar seu próprio HTML sem reformatar o Marketo.

1. Navegue até o email e clique em Editar rascunho.
1. Clique em Ações de email, Ferramentas do HTML e, em seguida, clique em Substituir HTML.
1. Substitua o código nesta caixa pela sua HTML. Em seguida, clique em Salvar.

Publicado em _2014-10-28_ por _Murta_

## Obter uma ID de cookie do visitante e, em seguida, consultar dados de lead associados

Usando o ponto de extremidade REST [Obter Vários Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), você pode consultar dados de clientes potenciais com base na ID do cookie de um usuário. Por exemplo, você pode usar essa abordagem para preencher previamente um formulário em uma landing page que não seja da Marketo. Esta publicação mostra como capturar o valor do cookie do usuário durante uma visita à página da Web, consultar [Obter API REST de vários leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) com essa ID de cookie e retornar os dados de lead do usuário. Primeiro, precisamos do valor do cookie Munchkin do usuário, &quot;_mkto_trk&quot;. Este é um exemplo de função JavaScript que você pode usar para obter o valor do cookie. Consulte [esta página de StackOverflow](https://stackoverflow.com/questions/10730362/get-cookie-by-name) para obter mais informações sobre esta abordagem. Recomendo definir um atraso de 500 ms após o evento de carregamento de página antes de chamar essa função. Isso dá ao Munchkin tempo para carregar e criar cookies para o usuário.

```javascript
//Function to read value of a cookie
function readCookie(name) {
    var cookiename = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(cookiename) == 0) return c.substring(cookiename.length,c.length);
    }
    return null;
}

//Call readCookie function to get value of user's Marketo cookie
var value = readCookie('_mkto_trk');
```

Em seguida, passe o valor do cookie &#39;_mkto_trk&#39; para o servidor. Para recuperar dados de clientes potenciais, do seu servidor, você faz uma chamada para a [API REST de Obter Vários Clientes Potenciais](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) com este valor de cookie. Você precisa dos endpoints de Autenticação e REST da sua instância. Sua chamada deve ser estruturada da seguinte maneira:

OBSERVAÇÃO: O valor do cookie `_mkto_trk` inclui um E comercial (&amp;) e precisa ser codificado na URL para `%26` para ser aceito corretamente pelo ponto de extremidade do Marketo.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "cde42eff-aca0-48cf-a1ac-576ffec65a84:ab"
# Replace with filter type and values
filter_type_and_values = "&filterType=cookies&filterValues=id:AAA-BBB-CCC%26token:_mch-marketo.com-1418418733122-51548&fields=cookies,email"
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

O exemplo acima retornará o email e todos os cookies associados ao usuário. Em seguida, você pode usar esses dados para personalizar a página subsequente que o usuário visita.

`{"requestId":"aa00#14a405aa786","result":[{"id":583,"email":"<testaccount@gmail.com>","cookies":"_mch-marketo.com-1418418733122-51548"}],"success":true}`

Publicado em _2014-10-30_ por _Murta_

## Quando você precisa de um desenvolvedor para ajudar na automação de marketing?

OBSERVAÇÃO: Esta é uma publicação de blog de convidados. Josh Hill é o líder de práticas da Marketo na Perkuto, uma agência de automação de marketing. Josh trabalha na interseção de marketing, vendas e tecnologia para fornecer sistemas de geração de receita. Ele escreve sobre automação de marketing e geração de demanda em [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).

As plataformas de automação de marketing são tremendamente poderosas prontas para uso e nas mãos de um operador experiente. As plataformas, por definição, permitem o uso de aplicativos de extensão para fazer com que o sistema faça coisas ainda mais surpreendentes para sua equipe. Você pode achar que o mecanismo lógico do Marketo é capaz de muita coisa (e é), mas há limitações. O Marketo não pode fazer tudo por você, nem deve.

Há outras ferramentas que executam melhor suas funções do que o Marketo poderia criá-las. A plataforma da Marketo é muito aberta, permitindo que o [ecossistema de aplicativos do LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) exista. Você também pode usar essa abertura para expandir os recursos do seu site e do Marketo para atender às suas necessidades comerciais. O melhor de uma plataforma como o Marketo é que ela permite ao profissional de marketing típico a capacidade de criar páginas, emails e lógica de roteamento sem ser um programador completo.
Atualmente, um profissional de marketing precisa entender lógica, mas é melhor deixar a programação real para os especialistas. Assim, como você sabe quando precisa chamar um desenvolvedor? Tenho algumas regras básicas, ou heurística, para decidir quando um programador deve se envolver: - Quando o Marketo não tem um filtro, acionador ou recurso óbvio para a necessidade, geralmente pode ser feito com algum JavaScript ou jQuery. - Será que isso será muito complexo para a Marketo por si só? - O Marketo pode fazer isso? - Essa é uma personalização de site que não é facilmente compatível? - O Marketo precisa se comunicar com um site ou outro banco de dados? - Parece algo que um computador pode fazer, mas o Marketo não tem uma função para isso? Lembre-se de que, embora o Marketo possa não oferecer uma função pronta para uso, ele se conecta a muitas integrações de terceiros e conexões personalizadas.

Confira algumas dessas categorias no [Marketplace do LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO): - [Ferramentas do Analytics](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Anexação de Dados](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Sistemas de Gerenciamento de Conteúdo](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Alguns aplicativos de terceiros fornecem painéis de controle intuitivos e ferramentas de configuração diretamente na plataforma (GoToWebinar). Essas são integrações &quot;nativas&quot; em que o maior trabalho que você precisa fazer é configurar o logon e usá-lo no Marketo. No entanto, outras extensões exigem o uso da API mais complexa que deve ser programada mais diretamente.

**Opções de Integração da Marketo** - Integração com o LaunchPoint - geralmente um logon ou configurações fáceis. - Integração de API - requer configuração de API e programação: (1) [REST API](/help/rest-api/rest-api.md) (2) [SOAP API](/help/soap-api/soap-api.md) (3) [Integração com Webhook](/help/webhooks/webhooks.md) - requer configuração de código especial, mas bastante fácil. (4) [Scripts de email](./email-scripting.md) (Velocity) - JavaScript e jQuery: (1) [Forms 2.0](/help/javascript-api/forms-api-reference.md) (2) [Rastreamento de clientes em potencial (Munchkin)](/help/javascript-api/lead-tracking.md) (3) [RTP JS](/help/javascript-api/web-personalization.md) Veja alguns casos de uso para usar um desenvolvedor para estender os recursos da plataforma Marketo. Você tem algum desses casos de uso? Em caso afirmativo, talvez seja hora de falar com um desenvolvedor. [Visite a seção de parceiro de serviços no LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO).

Publicado em _2014-11-06_ por _Josh_

## Integração do Slack com o Marketo

[O Slack é uma plataforma de colaboração empresarial](https://slack.com/). Se sua equipe estiver no Slack, você poderá facilmente trazer notificações do Marketo para o fluxo de trabalho. Esta publicação mostra como adicionar uma notificação ao seu log de bate-papo quando uma atividade específica de lead ocorrer no Marketo. Os possíveis casos de uso incluem notificar toda a equipe sobre o preenchimento de um formulário, uma visita a uma página de preços ou um cliente em potencial que não tenha sido contatado em 30 dias. A captura de tela abaixo mostra como a notificação do Marketo será exibida no Slack depois de seguir as etapas deste artigo de ajuda.

1. Faça logon no Slack. Clique em Integrações no Slack
1. Clique no botão Adicionar para Webhooks de entrada
1. Escolha o Canal para a Notificação do Marketo. Em Seguida, Clique Em Adicionar Integração De Webhook De Entrada.
1. Copiar e colar o URL do Webhook (que é necessário para a etapa oito)
1. Escolha um nome para a notificação
1. Faça logon no Marketo. Vá para Admin. Clique em Webhooks.
1. Clique em Novo Webhook
1. Insira um Nome para o Webhook. Insira o URL do Webhook da Etapa quatro. Informe Publicar como Tipo de Solicitação. Informe um Modelo de Carga.

Este é o modelo de conteúdo da captura de tela. Ele usa tokens de nome, empresa e endereço de email de nível de lead.

`payload={"text": "DEVELOPER SITE ALERT: {{lead.First Name:default=edit me}} {{lead.Company=edit me}}, {{lead.Email Address:default=no email address}}" }`

1. Configure o Trigger Campaign no Marketo. A etapa de fluxo é chamar o Webhook para o Slack. A lista inteligente é uma visita a uma página da Web.
1. Verifique Se Funciona.

Consulte a [documentação do desenvolvedor](./webhooks/webhooks.md) para obter mais informações sobre webhooks no Marketo.

Publicado em _2014-11-10_ por _Murta_

## Integração do Litmus ao Marketo

[Litmus é um serviço](https://www.litmus.com/) para testar emails em navegadores e clientes de email. O Litmus também fornece análises sobre emails, incluindo cliques, aberturas e exclusões. Esta publicação mostra como integrar o Marketo com o Litmus.

1. Ao configurar seu programa de email no Marketo, clique em &quot;Meus tokens&quot; no painel do programa
1. Arraste o token &quot;Script de email&quot; para o painel do meio para adicioná-lo.
1. Nomeie o token e clique em &quot;Clique para editar&quot;
1. À direita, abaixo de &quot;Objetos Padrão&quot;, expanda a categoria &quot;Lead&quot;. Localize o campo &quot;Endereço de email&quot; e marque a caixa. No espaço de script vazio à esquerda desta mesma página, cole no código de rastreamento fornecido por Litmus. No código Litmus, substitua cada instância de `{{lead.Email Address}}` por `${lead.Email}`. Clique em Salvar para fechar a janela do lightbox e clique em &quot;Salvar&quot; novamente na página de tokens.
1. Anote o nome do token `{{my.LitmusToken}}`. Abra o email que deseja rastrear. Na parte inferior do email, coloque o novo token de script. Você também pode adicionar informações padrão para corresponder à versão `{{my.LitmusToken:default=editme}}` do Litmus.

Quando o email é enviado, o script é colocado no email pelo Marketo.

Publicado em _2014-11-18_ por _Murta_

## Especificar uma imagem quando uma landing page do Marketo for compartilhada no Facebook

Digamos que você queira que uma imagem seja exibida automaticamente ao compartilhar uma landing page do Marketo no Facebook. Talvez você também queira que essa imagem não esteja na landing page do Marketo em si, mas apenas no compartilhamento do Facebook. Você pode fazer isso adicionando uma meta tag de gráfico aberto à sua página de aterrissagem do Marketo. As etapas para fazer isso estão abaixo.

1. Selecione sua landing page. Em seguida, clique em Editar rascunho.
1. Clique em Editar metatags da página.
1. Adicionar meta de gráfico aberto à seção Tags OG do Facebook. Em seguida, clique em Salvar. Este é o formato: `<meta property="og:image" content="http://example.com/example.jpg"/>`

[Consulte a documentação do desenvolvedor do Facebook](https://developers.facebook.com/docs/sharing/best-practices) sobre metatags de gráfico aberto para obter mais informações.

Publicado em _2014-11-17_ por _Murta_

## Redirecionar página com base no referenciador

Suponhamos que você deseje impedir o tráfego direto em uma página de aterrissagem do Marketo. Imagine que esta página tenha conteúdo para download como um PDF que você gostaria que um usuário preenchesse um formulário antes de receber. Você pode resolver isso verificando se o usuário veio de uma determinada página. Nesse caso, seria a página na qual o usuário tem que preencher um formulário. Se o usuário não vier dessa página, você poderá redirecioná-lo para a página de preenchimento de formulário. Para fazer isso, é necessário verificar se a página do referenciador para a página de aterrissagem com conteúdo é a página de preenchimento do formulário.
Substitua ambas as instâncias de `http://example.com/PageWithForm` no trecho abaixo por um link para a página da qual você deseja que o usuário venha. Esta pode ser a página de preenchimento do formulário.**

```javascript
<script>
window.onload = function() {
  if (document.referrer !== "http://example.com/PageWithForm") {
    document.location.href = "http://example.com/PageWithForm";
  };
 };
</script>
```

Inclua o trecho personalizado do JavaScript antes de fechar a tag na página de aterrissagem do Marketo com o conteúdo.** Se o usuário não for enviado para a página de aterrissagem com conteúdo da página de preenchimento do formulário, ele será redirecionado para a página de preenchimento do formulário.

Publicado em _2014-11-18_ por _Murta_

## Integração do Trello ao Marketo

O Trello é um [aplicativo popular de gerenciamento de projetos baseados na Web](https://trello.com/). Se sua equipe usa o Trello, você pode facilmente trazer notificações do Marketo para o fluxo de trabalho. Esta publicação mostra como adicionar um cartão com uma notificação do Marketo ao quadro do Trello. Este cartão será adicionado quando uma atividade específica de lead ocorrer no Marketo. Os possíveis casos de uso incluem notificar toda a equipe sobre o preenchimento de um formulário, uma visita a uma página de preços ou um cliente em potencial que não tenha sido contatado em 30 dias.

1. Faça logon no Trello. Navegue até o quadro do Trello que adicionará notificações do Marketo. Clique em Add a List e a nomeie como.
1. Clique em Mostrar barra lateral. Clique em Configurações de email para painel. Anote o email na caixa &quot;Seu endereço de email para este quadro&quot;. Este e-mail será usado na Etapa Seis. Escolha qual lista adicionar à notificação do Marketo.
1. Faça logon no Marketo. Clique na nova Campanha inteligente. Insira um nome e clique em Salvar.
1. Navegue até Smart List. Escolha um acionador para esta campanha inteligente. Neste exemplo, usamos o acionador Preenchimento de formulário. Arraste o acionador Preenchimentos de formulário para o Painel do meio. Selecione o Formulário que deseja acionar essa notificação.
1. Crie um email. Clique em Novo. Clique em Ativo local. Clique em Novo email. Nomeie o email. Em seguida, clique em Criar.
1. Clique em &quot;Editar rascunho&quot; para o email que você criou na Etapa cinco. Arraste os tokens relevantes que você deseja mostrar no cartão Trello. A linha de assunto do email do Marketo aparece no título do cartão Trello, e o corpo do email do Marketo aparecerá na descrição do cartão Trello. Por exemplo, você pode usar &quot;ALERTA DE CLIENTE POTENCIAL: `{{lead.First Name:default=edit me}}` `{{lead.Last Name:default=edit me}}` se quiser que o nome e o sobrenome do cliente potencial estejam no título do cartão Trello. Em seguida, aprove o email.
1. Navegue até Campanha inteligente. Clique em Fluxo. Arraste Enviar alerta para o painel do meio. Selecione o email recém-criado. Selecione &quot;Enviar para&quot; como Nenhum. Selecione &quot;Para outros emails&quot; como o email do Trello da Etapa dois.
1. Clique em Agendar no menu superior. Clique em Ativar. Clique em Confirmar.
1. Teste a integração. Um cartão com o nome e sobrenome do lead no título será exibido no quadro do Trello. Consulte a [documentação do Trello](https://support.atlassian.com/trello/) para obter mais informações.

Publicado em _2014-11-18_ por _Murta_

## Localizar clientes em potencial por valor do campo personalizado

Suponhamos que você queira obter leads da API do Marketo que correspondam a determinados critérios de atividade ou inatividade. Por exemplo, talvez você queira encontrar um lead cuja pontuação não foi alterada nos últimos 30 dias. Seguindo as etapas desta publicação, você pode obter esta lista de clientes em potencial. Para fazer isso, criamos uma campanha inteligente no Marketo que identifica leads cuja pontuação não foi alterada nos últimos 30 dias e, em seguida, armazenamos um valor nesses leads para identificá-los. Em seguida, consultaremos a API com esse valor.

1. Crie um novo campo personalizado chamado customLeadStatus** Faça logon no Marketo e vá para o painel Admin. Clique em Gerenciamento de campo. Clique em &quot;Novo campo personalizado&quot;.  Nomeie o campo. Em seguida, clique em Criar.
1. Crie uma campanha inteligente com uma lista inteligente que procure clientes potenciais que não foram atualizados em 30 dias.** Clique em Nova campanha inteligente. Dê um nome à nova campanha inteligente. Arrastar sem pontuação foi alterado do painel direito para o painel do meio.
1. Adicione uma etapa de fluxo à campanha inteligente da Etapa 3 para atualizar o campo customLeadStatus com um novo valor.** Arraste Alterar valor de dados do painel direito para o painel do meio.
1. Atualize o Smart Campaign para permitir que os clientes em potencial sejam executados várias vezes.** Clique em Agendamento. Em seguida, clique em Editar.  Selecione sempre. Em seguida, clique em Salvar. A campanha começará a funcionar.
1. Consulte a [Obter Vários Clientes Potenciais por API REST de Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Fornecendo os parâmetros filterType=customLeadStatus &amp; filterValue=needsEnrichment.**

Este é um exemplo de solicitação que retorna esses dados.

`<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?access_token=><yourAccessToken>&filterType=customLeadStatus&filterValues=needsEnrichment`

Uma chamada de API bem-sucedida retorna dados JSON com leads cujo campo customLeadStatus corresponde ao valor de needsEnrichment. Revise a [Obter vários clientes em potencial por API REST de tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) para obter mais informações.

Publicado em _2014-11-22_ por _Murta_

## Sincronização de oportunidade por meio da API do SOAP

Este artigo descreve como inserir oportunidades no Marketo por meio da API do SOAP e como associá-las a empresas e clientes potenciais. Ele começa com uma explicação de como esse processo funciona e fornece amostras de código para cada um dos cenários.

**Diagrama de Estrutura de Tabela** Primeiramente, o diagrama abaixo descreve a estrutura da tabela. A ID é gerada automaticamente ao criar um registro (número sequencial). O campo em negrito é obrigatório: somente a Função da pessoa da oportunidade tem campos obrigatórios. Os campos entre parênteses são opcionais, assim como as conexões com uma linha pontilhada.

**Inserção básica de oportunidade** Primeiro, você insere a oportunidade e, em seguida, a função de pessoa da oportunidade, que vincula a oportunidade ao(s) cliente(s) potencial(is). Neste exemplo, use a ID do cliente potencial e a ID da oportunidade para especificar a função da pessoa da oportunidade. Você obtém a ID da oportunidade na Resposta da SOAP ao criar a oportunidade. A ID do cliente potencial está visível em todos os clientes potenciais no banco de dados de clientes potenciais da Marketo.

**Link de Empresa** Na maioria dos casos, você deseja vincular uma Oportunidade a uma Empresa, além de um indivíduo. No Marketo, não é possível acessar os registros de Empresa por meio da API do SOAP, apenas os registros de Cliente potencial (os registros de Cliente potencial contêm os campos de empresa). Ainda é possível vincular Oportunidades a uma Empresa adicionando um identificador de Empresa exclusivo a cada Cliente Potencial e usando essa ID em sua Oportunidade. A etapa 1 é criar um campo &quot;Id da empresa&quot; no registro de lead e preenchê-lo com um identificador exclusivo, geralmente de um sistema de back-end. A etapa 2 é criar um campo &quot;Id da empresa&quot; na oportunidade: é necessário solicitar ao Suporte ou Consultoria da Marketo a criação desse campo para você. Em seguida, preencha esse campo ao criar Oportunidades, que conecta a Oportunidade à Empresa. Isso é especialmente importante se você usar o Marketo Revenue Cycle Analytics e quiser usar o Opportunity Influence Analyzer, que depende das Informações da empresa.

**Usando identificadores externos** Em muitos casos, você pode ter seus próprios identificadores exclusivos ao integrar com um sistema back-end. É possível usar esses identificadores exclusivos no Marketo por meio de chaves estrangeiras. Para clientes potenciais, normalmente você usaria a ID de pessoa do sistema externo (FSPID), que é usada em vez da ID do Marketo ou do endereço de email como identificador exclusivo. O FSPID é um campo de sistema oculto, que não está visível no Marketo. Se você ainda não estiver fazendo isso, é necessário que a sincronização de Oportunidade também salve o FSPID em um campo personalizado, por exemplo, &quot;ID externa&quot; (é possível nomear o campo como qualquer coisa). Você mesmo pode criar esse campo como administrador do Marketo. Para Oportunidades, peça ao Suporte da Marketo para criar outro campo personalizado na Oportunidade, por exemplo, chamado &quot;ID externa&quot; (você pode nomeá-lo como quiser). Em seguida, preencha este campo ao inserir Oportunidades. Por fim, ao criar a função Pessoa da oportunidade, você usa ambas as chaves estrangeiras para especificar o vínculo entre clientes potenciais e oportunidades, em vez de usar as Marketo IDs. Também é possível usar as chaves estrangeiras para atualizar Clientes potenciais de oportunidades. No momento, não é possível adicionar uma chave estrangeira aos registros de Função da pessoa da oportunidade, portanto, você terá que usar a Marketo Id gerada automaticamente para isso (que você obtém na resposta da SOAP após criar a Função da pessoa da oportunidade).

**Exemplo De Código** Etapas: 1. Inserir/atualizar cliente em potencial com Chave estrangeira e ID da empresa 1. Inserir Oportunidade com Chave Estrangeira 1. Inserir função de pessoa da oportunidade com chaves estrangeiras 1. Solicitação SOAP - Atualização de cliente potencial existente com chave estrangeira e ID da empresa Atualiza um cliente potencial existente (ID da Marketo &quot;6&quot;) com a chave estrangeira &quot;12346&quot; e a ID de conta/empresa &quot;C123&quot;. Também estamos salvando a Chave estrangeira em um campo personalizado, porque precisamos disso para a Função da pessoa da oportunidade. O uso de uma Chave estrangeira é opcional: você também pode usar a Marketo Id para vincular este lead à Oportunidade. O uso da ID da empresa também é opcional, mas é obrigatório se você quiser usar o Analisador de influência de oportunidade no RCA. Solicitação:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncLead>
         <leadRecord>
            <Id>6</Id>
            <ForeignSysPersonId>12346</ForeignSysPersonId>
            <leadAttributeList>
               <attribute>
                  <attrName>FSPID</attrName>
                  <attrValue>12346</attrValue>
               </attribute>
               <attribute>
                  <attrName>cAccountFSID</attrName>
                  <attrValue>C123</attrValue>
               </attribute>
            </leadAttributeList>
         </leadRecord>
         <returnLead>false</returnLead>
      </mkt:paramsSyncLead>
   </soapenv:Body>
</soapenv:Envelope>
```

Resposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncLead>
         <result>
            <leadId>6</leadId>
            <syncStatus>
               <leadId>6</leadId>
               <status>UPDATED</status>
               <error xsi:nil="true"/>
            </syncStatus>
            <leadRecord xsi:nil="true"/>
         </result>
      </ns1:successSyncLead>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Solicitação SOAP - Criação de oportunidade Nesse caso, dois campos personalizados foram criados na tabela Oportunidade: - `opportunityId` → contém a ID exclusiva da oportunidade - `cAccountFSID` → contém a referência da empresa em vez de especificar sua própria ID de oportunidade. Você também pode usar a ID de oportunidade gerada pela Marketo. Nesse caso, você deixa de fora o nó External Key. A Associação da empresa também é opcional, mas é necessária caso você queira usar o Analisador de influência de oportunidade no RCA. Solicitação:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:03:28-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_4</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 4 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>501.00</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>501</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Resposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>40</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Solicitação SOAP - Função da pessoa na oportunidade Essa solicitação vincula o lead à oportunidade. Você pode especificar vários links em uma única Solicitação do SOAP (este exemplo vincula a Oportunidade a apenas um lead). Usa as chaves estrangeiras para especificar o link, mas nos comentários também mostra como usar as IDs reais (neste caso: 6 para a ID do lead e 40 para a ID da oportunidade). Os campos &quot;IsPrimary&quot; e &quot;Role&quot; são opcionais. Solicitação:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <!--id>40</id-->
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_4</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Resposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>11</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Abordagem alternativa (faça as etapas 2 e 3 em uma chamada)** Embora você possa inserir a oportunidade e, em seguida, a função de pessoa da oportunidade, também é possível fazer isso em uma chamada do SOAP. No entanto, é necessário usar a Chave estrangeira para a oportunidade (você não pode usar a ID de oportunidade gerada automaticamente na função de pessoa da oportunidade, pois a oportunidade ainda não foi gerada). É claro que você também pode vincular vários clientes em potencial a esta oportunidade na mesma chamada de API (este exemplo vincula a oportunidade a apenas um cliente potencial). Solicitação:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:44:08-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_5</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 5 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
             <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_5</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Resposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>41</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
               <mObjStatus>
                  <id>12</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Publicado em _2014-11-26_ por _Jep_

## Solicitações de API REST multithread

Se você quiser melhorar o desempenho ao chamar a API do Marketo, poderá fazer solicitações simultâneas. Essa abordagem permite obter mais dados em um período de tempo menor. Ao fazer uma solicitação de API, parte do tempo de ida e volta entre o cliente e o servidor é o tempo de transferência na conexão. Portanto, se pudermos reduzir o tempo de transferência on-line das solicitações de maneira agregada, melhoraremos o desempenho. O código de amostra abaixo mostra como fazer isso no Ruby. Ele usa EventMachine, que é uma [biblioteca de processamento de eventos usada para fazer solicitações multithread](https://github.com/igrigorik/em-http-request/wiki/Parallel-Requests). O exemplo abaixo chama a [API de Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) e faz duas solicitações simultâneas. Essa abordagem elimina o tempo de transferência do cliente para o servidor na segunda solicitação. Isso é feito incluindo a segunda solicitação ao mesmo tempo que a primeira. As respostas da API são gravadas em um arquivo de texto.

```java
require 'em-http-request'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = ["&nextPageToken=A5YMOYZQBOGD2OSYYBYDAQGEMGLBDGDANAABQGRAQWAAKKID", "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"]
# Specify activities needed
activity_type_ids = "&activityTypeIds=1&activityTypeIds=12"
requesturl_a = marketo_instance + endpoint + auth_token + since_date_time.at(0) + activity_type_ids
requesturl_b = marketo_instance + endpoint + auth_token + since_date_time.at(1) + activity_type_ids

# Make request
EventMachine.run do
  http1 = EventMachine::HttpRequest.new(requesturl_a).get
  http2 = EventMachine::HttpRequest.new(requesturl_b).get

# When API response is received, write response to a text file
  http1.callback {
  File.open('response1.txt', 'w') do |t|
  t.puts http1.response
  end }

  http2.callback {
  File.open('response2.txt', 'w') do |t|
  t.puts http2.response
  end }
end
```

Publicado em _2014-12-03_ por _Murta_

## Solicitações da API de Ajuste de Desempenho

Esta publicação discute estratégias para melhorar o desempenho ao solicitar dados da API do Marketo. No entanto, você deve pesar os benefícios dessas estratégias em relação à restrição operacional dos limites diários da API do Marketo.
**Estratégia 1 - Solicitar menos dados em cada chamada de API** Geralmente, à medida que você solicita mais dados em uma chamada de API, aumenta o tempo necessário para pesquisar os dados no banco de dados pelo servidor do Marketo. Se você estiver fazendo uma chamada de API com intervalos de datas, como a [API do SOAP getMultipleLeads](/help/soap-api/getmultipleleads.md), reduza o intervalo de tempo por chamada e compense com mais chamadas. Por exemplo, em vez de solicitar dados de 1º de junho a 1º de julho, solicite um único dia por vez, como uma chamada para 1º a 2 de junho e outra chamada para 1º a 2 de junho. Se você estiver fazendo uma chamada de API que retorna dados dos campos de cliente potencial do Marketo, solicite apenas esses campos necessários. Cada campo de lead adicional aumenta incrementalmente a quantidade de tempo que uma chamada de API leva. Outra abordagem é reduzir o tamanho do lote ou o número de leads solicitados por chamada.
**Estratégia 2 - Fazer Solicitações Simultâneas** Para melhorar o desempenho e obter mais dados de uma só vez. Você pode fazer solicitações simultâneas para a API. Essa abordagem reduz o tempo gasto na agregação das solicitações de API wire. Por exemplo, digamos que você esteja fazendo solicitações para Obter vários clientes em potencial por tipo de filtro. Você pode fazer solicitações simultâneas para uma solicitação que consulta clientes em potencial de 1 a 300 e para outra solicitação que consulta os clientes em potencial de 301 a 600.
**Estratégia 3 - Armazenar Dados em Cache** Alguns dados no Marketo são alterados com menos frequência, como a lista de campos de cliente potencial, do que outros dados, como os dados de atividade de cliente potencial. Se você armazenar em cache dados que são atualizados com menos frequência, reduzirá o número de chamadas de API que precisam ser feitas. Você também obterá melhor desempenho porque a pesquisa de dados localmente é geralmente mais rápida do que acessá-los a partir de um serviço Web remoto.

Publicado em _2014-12-05_ por _Murta_

## Enviar dados do formulário do Marketo para o Google Analytics

No Google Analytics, você pode enviar eventos de dados personalizados e, em seguida, utilizar os dados para segmentar e analisar o desempenho do site. O trecho de código JavaScript abaixo permite enviar dados do formulário Marketo 2.0 automaticamente para o Google Analytics depois que um visitante enviar um formulário web. Veja como configurar isso.

**Etapa um** Insira a tag do JavaScript em qualquer página que inclua o Marketo Forms na parte inferior do código (antes da tag). O JavaScript envia somente campos não ocultos (sendHiddenFields : false). Isso pode ser ajustado alterando sendHiddenFields de falso para verdadeiro. Você também pode selecionar campos a serem excluídos adicionando IDs de campo adicionais na matriz &quot;fieldsToExclude&quot;.

```javascript
function pushFormDataToGa(a){
setTimeout(function () {
document.getElementsByTagName('form')[0].getElementsByClassName(a.submitButton)[0].addEventListener('click', function() {
  allFields = document.getElementsByTagName('form')[0].getElementsByTagName('input');
  for(i=0;i<allFields.length;i++){
   if( (allFields[i].type !="hidden" && allFields[i].type !="submit" && allFields[i].value !="" && a.fieldsToExclude.indexOf(allFields[i].id) === -1  ) || (allFields[i].type === "hidden" && a.sendHiddenFields) ){
    console.log( allFields[i].name + ": "  + allFields[i].value);
    if(typeof(_gaq) != "undefined"){
    //Classic
    _trackEvent("Marketo Form Submission", allFields[i].value , allFields[i].name
{'nonInteraction': 1});
    }else if(typeof(ga) !="undefined"){
    //Universal
    ga('send', 'event',"Marketo Form Submission", allFields[i].value , allFields[i].name, {'nonInteraction': 1});
}}}}, false);
}, 3000);}
pushFormDataToGa({
 submitButton: "mktoButton",
 fieldsToExclude: ["Email","LastName", "FirstName"],
 sendHiddenFields : false
});
```

**Etapa dois** Os dados no GA aparecem na seção Relatórios. Vá até Comportamento > Eventos > Principais eventos. **Restrições de Script:** - Esta amostra de código é compatível somente com o [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md). - Devido à política de privacidade da Google, você não pode enviar nenhuma informação pessoal (email ou nome). Além de possíveis preocupações com a privacidade, essas são informações de identificação pessoal e, portanto, violam os [Termos de Serviço do Google Analytics](https://marketingplatform.google.com/about/analytics/terms/us/):

&quot;Você não poderá (e não permitirá que terceiros) usar o Serviço para rastrear, coletar ou carregar dados que identifiquem pessoalmente um indivíduo (como nome, endereço de email ou informações de faturamento) ou outros dados que possam ser razoavelmente vinculados a essas informações pela Google.&quot;

Publicado em _2014-12-16_ por _Yanir_

## Adicionar um campo de nome completo a um formulário do Marketo

Sabemos que formulários web mais curtos melhoram as taxas de conversão. A amostra de código JavaScript abaixo permite tornar seus formulários ainda mais curtos ao mesclar os campos Nome e Sobrenome em um campo Nome completo. Quando os visitantes digitam seu nome completo, o script divide automaticamente o texto em campos de nome e sobrenome. Para visitantes conhecidos, o script associa o nome e o sobrenome e, em seguida, os copia para o novo campo para que eles não precisem preencher o campo novamente. Veja como configurar isso.

**Etapa um** Crie um novo campo personalizado no Marketo chamado Nome Completo. Não é necessário criá-lo na plataforma do CRM, pois o script só usará esse campo para exibir o nome completo.
**Etapa dois** Adicione este campo a todos os formulários web. Defina os campos de nome e sobrenome como ocultos. No JavaScript, altere a configuração &quot;splitFullName&quot; para conter os três nomes de campo. Observação: esses nomes não devem aparecer em nenhum outro lugar da página.
**Etapa três** Insira a JavaScript em todas as páginas de aterrissagem na parte inferior do código, antes da marca.

```javascript
<script>
MktoForms2.whenReady(function (form){
        function splitFullName(a,b,c){
                String.prototype.capitalize = function(){
                        return this.replace( /(^|s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase(); } );
                };
                document.getElementsByName[c](0).oninput=function(){
                        var fullName = document.getElementsByName[c](0).value;
                        if((fullName.match(/ /g) || []).length ===0 || fullName.substring(fullName.indexOf(" ")+1,fullName.length) === ""){
                                var first = fullName.capitalize();;
                                var last = "null";
                        }else if(fullName.substring(0,fullName.indexOf(" ")).indexOf(".")>-1){
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize() + " " + fullName.substring(fullName.indexOf(" ")+1,fullName.length).substring(0,fullName.substring(fullName.indexOf(" ")+1,fullName.length).indexOf(" ")).capitalize();
                                var last = fullName.substring(first.length +1,fullName.length).capitalize();
                        }else{
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize();
                                var last = fullName.substring(fullName.indexOf(" ")+1,fullName.length).capitalize();
                        }
                        document.getElementsByName[a](0).value = first;
                        document.getElementsByName[b](0).value = last;
                };
                //Initial Values
                if(document.getElementsByName[c](0).value.length < 2 && document.getElementsByName[b](0).value.length.length >2 && document.getElementsByName[a](0).value.length.length >2 ){
                        var first = document.getElementsByName[a](0).value.capitalize();
                        var last = document.getElementsByName[b](0).value.capitalize();
                        var fullName =  first + " " + last ;
                        console.log(fullName);
                        document.getElementsByName[c](0).value = fullName;
                }
        }
        splitFullName("FirstName","LastName","leadFullName");
});
</script>
```

Observação: esse código funciona somente com o Marketo Forms 2.0.

Publicado em _2014-12-16_ por _Yanir_

## Usar cURL para importar leads por meio da API REST

Deseja importar clientes em potencial de um arquivo CSV por meio da API REST, mas notou que isso é desafiador ao usar a extensão Postman Chrome. Nesta publicação, abordamos como fazer isso com cURL.

1. [Baixe e instale o cURL](https://curl.se/download.html), uma ferramenta de linha de comando que usamos para enviar dados para a API REST do Marketo.
1. Abra a linha de comando e navegue até o local onde o arquivo CSV está localizado. Os cabeçalhos de coluna no arquivo CSV devem corresponder aos nomes dos campos da API, não aos nomes dos campos do Marketo.
1. Você precisa de um token de acesso. Faça logon no Marketo, acesse Admin e, em seguida, LaunchPoint. Encontre seu usuário da API REST e clique em &quot;Exibir detalhes&quot;. Clique no botão &quot;Obter token&quot;.
1. Você também precisará do terminal REST específico para a instância do Marketo. Faça logon no Marketo, acesse Admin e depois Serviços da Web. Na seção marcada como &quot;REST API&quot;, você encontra o URL do endpoint.
1. Na linha de comando, siga este formato para a chamada cURL. Substitua `<accesstoken>` com seu token de acesso da Etapa três e `<REST API Endpoint URL>` com sua URL de Ponto de Extremidade de API REST da Etapa quatro. Mais informações [disponíveis aqui](https://developer.adobe.com/marketo-apis/api/mapi/#operation/importLeadUsingPOST). O &quot;/bulk&quot; aqui substituirá o &quot;/rest&quot; no final do URL do endpoint. Se você tiver o ponto de extremidade definido para /rest/bulk, ele retornará um erro.

`curl -i -F format=csv -F file=@leaddata.csv -F access_token=<accesstoken> <REST API Endpoint URL>/bulk/v1/leads.json`

Publicado em _2014-12-16_ por _Jordânia_

## Adicionar um alerta de confirmação a uma Marketo para

Digamos que, quando um usuário clicar no botão &quot;Enviar&quot; em um formulário do Marketo, você deseje exibir uma notificação que pergunta ao usuário se &quot;Está realmente correto enviar?&quot;. Isso é possível implementando algumas linhas do JavaScript, que mostrarão uma caixa de confirmação quando alguém clicar no botão Enviar. Aqui está um exemplo de como fazer isso. Adicione a função onSubmit ao formulário do Marketo, como mostrado abaixo. Para obter mais informações sobre a API do Marketo Forms, [verifique a documentação do desenvolvedor](/help/javascript-api/forms-api-reference.md).

```javascript
<script src="//app-e.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_19"></form>
<script>
MktoForms2.loadForm("//app-e.marketo.com", "212-RBI-463", 19,function(form){

//Add this function to your Marketo form script
form.onSubmit(function(){
alert("Do you really want to submit the form?");
});
});
</script>
```

Publicado em _2014-12-17_ por _David_

## Mostrar mensagem de agradecimento sem uma landing page de acompanhamento

Normalmente, ao usar os formulários do Marketo, você cria duas páginas de aterrissagem: uma para colocar o formulário e outra para redirecionar após a conclusão do formulário. No entanto, em alguns casos, talvez você não queira ter duas landing pages separadas, mas muito semelhantes para manter. Na verdade, você pode usar a mesma landing page para o formulário e para a mensagem de agradecimento usando a API do JavaScript do Forms 2.0. Para fazer isso, primeiro crie a landing page e o formulário de registro e coloque o formulário na landing page como faria normalmente. Em seguida, adicione um elemento HTML à página. Neste elemento, adicionamos algum código que é ativado no momento em que o formulário é enviado. Em seguida, ele ocultará o formulário e revelará uma <div> que contém a mensagem de agradecimento. Seu JavaScript deve ter esta aparência:

```javascript
//Edit host with your Marketo instance info
<script src="//<host>/js/forms2/js/forms2.js"></script>
<script>
MktoForms2.whenReady(function (form){
 //Add an onSuccess handler
  form.onSuccess(function(values, followUpUrl){
   //get the form's jQuery element and hide it
   form.getFormElem().hide();
   document.getElementById('confirmform').style.visibility = 'visible';
   //return false to prevent the submission handler from taking the lead to the follow up url.
   return false;
 });
});
</script>
```

Editar texto da mensagem de agradecimento.

`<div id="confirmform" style="visibility:hidden;"><p><strong>Thank you. Check your email for details on your request.</strong></p></div>`

Você editará o nome do host e agradecerá a mensagem na amostra de código. A primeira deve fazer referência à instância do Marketo (por exemplo, &quot;//app-sj06.marketo.com/js/forms2/js/forms2.js&quot;) e a segunda deve conter o texto de agradecimento que você deseja exibir após o preenchimento do formulário. O texto é exibido na página de aterrissagem na posição exata em que você coloca o elemento HTML, portanto, edite-o na folha de propriedades. Verifique também se a camada do elemento HTML é menor que a camada do formulário. Por padrão, ambos serão colocados na Camada 15, portanto, você estará seguro se criar seu elemento de HTML na Camada 11. Se você não fizer isso, não poderá digitar nenhuma caixa de campo de formulário que se sobreponha à mensagem de agradecimento. Não é necessário alterar o tipo de acompanhamento no formulário ou na página de aterrissagem, pois o JavaScript substituirá essas configurações. Para obter mais informações sobre a API do Marketo Forms, consulte a [documentação para desenvolvedores](/help/javascript-api/forms-api-reference.md).

Publicado em _2014-12-19_ por _Kristin_

## Destacando projetos abertos do Source criados na plataforma Marketo

Este é o primeiro post de uma série em andamento destacando projetos de código aberto construídos em torno da plataforma Marketo pela comunidade do desenvolvedor. Mantemos [uma lista na conta GitHub da Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries), onde rastreamos bibliotecas de clientes e projetos criados pela comunidade de desenvolvedores do Marketo. Abaixo estão três projetos desenvolvidos em torno das APIs Marketo REST e SOAP. Daniel Chesterton criou [uma biblioteca cliente no PHP para a API REST do Marketo](https://github.com/dchesterton/marketo-rest-api). Atualmente, a biblioteca do cliente tem cobertura para 12 endpoints da API REST.** Kyle Halstvedt, do Elixiter, criou um projeto para [extrair leads de listas estáticas do Marketo para uma Planilha do Google](https://github.com/Elixiter/mkto_google-spreadsheet). O projeto do Kyle usa a API REST do Marketo.  David Santoso criou uma gem [Ruby para a API SOAP do Marketo.](https://github.com/davidsantoso/markety) Este projeto pode ajudá-lo a integrar mais rapidamente a API do Marketo SOAP a um aplicativo Ruby on Rails.  Estamos animados em ver mais projetos criados pela comunidade de desenvolvedores na plataforma Marketo. Se você estiver trabalhando em um projeto de código aberto para a plataforma Marketo, [envie-o para este repositório GitHub por meio de uma solicitação de pull](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publicado em _2015-01-02_ por _Murta_

## Alterar dinamicamente o conteúdo da página com base no local de um usuário

Digamos que você deseje alterar dinamicamente o número de telefone em uma página de aterrissagem, dependendo de onde um usuário está localizado. Por exemplo, se a pessoa estiver na Califórnia, você gostaria de mostrar a ela o número de telefone do escritório da Califórnia em sua página de aterrissagem; mas se ela estiver no Japão, você gostaria de mostrar a ela o número de telefone do escritório japonês.  Uma maneira de implementar isso é usando o JavaScript e a [API de geolocalização do HTML5](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). A vantagem dessa abordagem é que podemos criar uma landing page e alterá-la dinamicamente com base na localização de um usuário, em vez de várias landing pages estáticas. Abordamos os detalhes de implementação técnica abaixo. Criamos um objeto para locais de escritório com coordenadas de latitude e longitude e um segundo objeto com números de telefone de escritório. Na produção, seria uma prática recomendada combinar esses dois objetos em um único objeto.

```json
//Coordinates for Marketo offices
var officeLocations = {
    "San Mateo": {latitude: 37.5596465, longitude: -122.2870142},
    "Atlanta": {latitude: 33.8547013, longitude: -84.35552349999999},
    "Tokyo": {latitude: 35.6895, longitude: 139.6917},
    "Dublin": {latitude: 53.3478, longitude: -6.2603097},
    "Sydney": {latitude: -33.873651, longitude: 151.2068896},
    "Portland": {latitude: 45.512089, longitude: -122.6763367},
    "Tel Aviv": {latitude: 32.0852999, longitude: 34.78176759999999}
}

//Phone numbers for Marketo offices
var officePhoneNumbers = {
    "San Mateo": "+1-650-376-2300",
    "Atlanta": "+1-877-260-6586",
    "Tokyo": "+81-03-6759-8280",
    "Dublin": "+353-1-242-3000",
    "Sydney": "+61-2-9045-2711",
    "Portland": "+1-877-260-6586",
    "Tel Aviv": "+1-877-260-6586"
}
```

Criamos um método para solicitar a localização de um usuário. Para lidar com erros, se a localização do usuário não estiver acessível, assumiremos como padrão a sede da Marketo e seu número de telefone.

```javascript
//Method to get user's current location. Returns a position object with user's geo coordinates
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(findNearestOffice);
    } else {
        x.innerHTML = "Marketo Location: San Mateo
Marketo Phone Number: +1-877-260-6586";
    }
}
```

Por fim, criamos um método para encontrar o escritório mais próximo da localização do usuário e, em seguida, retornamos o número de telefone do escritório mais próximo na página. Este método usa o método findNearest de [Geolib, que é uma biblioteca JavaScript que fornece operações geoespaciais](https://github.com/manuelbieh/Geolib).

```javascript
//Find nearest Marketo office to user's location
function findNearestOffice(position) {
        var nearestOffice = geolib.findNearest({latitude: position.coords.latitude, longitude: position.coords.longitude}, officeLocations);
        x.innerHTML = "Marketo Location: " + nearestOffice.key + "
Marketo Phone Number: " +  officePhoneNumbers[nearestOffice.key];
}
```

Aqui está a implementação completa. Acionamos o método getLocation quando o usuário clica no botão na página. Este [repositório GitHub](https://github.com/MurtzaM/Find-Nearest-Marketo-Office) tem os arquivos necessários para configurar esta demonstração.

Publicado em _2014-12-20_ por _Murta_

## Rastreamento de cliente potencial e vários domínios

O código de rastreamento Munchkin do Marketo ajuda a rastrear visitas ao seu site. É provável que você queira usar o código de rastreamento do Munchkin para criar cookies de leads anônimos para a maioria ou todas as páginas do seu site. Vamos analisar como o Munchkin funciona. As visitas à página são registradas para clientes potenciais existentes, e uma visita à página por um visitante sem cookie fará com que um novo cookie seja criado e armazenado, e um novo cliente potencial anônimo será criado no banco de dados do Marketo. O Munchkin-tracker criará automaticamente um cookie em um visitante se ele ainda não tiver um cookie existente para o domínio atual. No Marketo, ele registra o evento (clique em um link, visite uma página da Web ou um novo lead) no Log de atividades do lead. O valor armazenado no cookie é exclusivo para um determinado visitante. O valor é uma combinação do identificador exclusivo de rastreamento da conta do Munchkin, do nome do domínio, do carimbo de data e hora e do número inteiro aleatório.
**O que acontece se eu tiver vários domínios?** digamos que você tenha dois sites que gostaria de rastrear: `<www.apples.com>` e `<www.bananas.com>`. Você pode colocar o código de rastreamento em ambos os sites, no entanto, é necessário considerar o seguinte. Os cookies do Marketo são &quot;cookies primários&quot; e, portanto, são específicos do domínio. Isso significa que um visitante do site 1 será criado como um lead anônimo no Marketo. Se o mesmo lead for para o site 2, isso criará um segundo lead anônimo separado no Marketo. Se o cliente potencial preencher um formulário no site 1 e esse registro for conhecido, o registro anônimo do site 2 permanecerá e continuará a acumular visitas subsequentes a esse site. Se o lead continuar preenchendo um formulário no site 2 com o mesmo endereço de email usado no site 1, ambos os leads conhecidos serão mesclados automaticamente e todos os comportamentos passados e futuros serão rastreados em um único registro no Marketo. Ambas as IDs de cookie estão vinculadas ao mesmo lead e todas as atividades da Web (de qualquer domínio) estarão nesse lead.
**E quanto a vários subdomínios?** Subdomínios não são um problema. Vamos usar Marketo.com como exemplo. Ele tem vários subdomínios para diferentes idiomas, como fr.marketo.com e de.marketo.com. Com subdomínios, todas as atividades serão registradas no mesmo registro/cookie de lead.

Publicado em _2015-01-13_ por _David_

## Alterar a Cor do Texto da Dica em um Formulário do Marketo

Digamos que você deseje alterar a cor do texto da dica (também conhecido como texto do espaço reservado) no Forms 2.0. Isso é possível por meio de CSS personalizado. Por exemplo, na captura de tela abaixo, fiz o texto de dica neste Formulário Marketo azul. Há três opções sobre como fazer isso, dependendo de como você está usando o Marketo Forms.

**Opção 1: se você estiver incorporando um formulário Marketo, adicione o CSS abaixo diretamente ao arquivo CSS principal.**

```css
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
```

**Opção 2: ao incorporar um formulário Marketo, você pode adicionar o CSS diretamente na página entre `<style></style>` marcas na seção `<head>`.**

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

**Opção 3: se estiver usando um formulário do Marketo em uma página de aterrissagem do Marketo, você pode adicionar esse CSS personalizado por meio da interface do usuário do Marketo.** Localize a página de aterrissagem na árvore de navegação do Marketo. Em seguida, clique em Editar rascunho. Clique em Editar metatags da página. Adicione o CSS abaixo da seção HTML do HEAD personalizado. As marcas `<style></style>` devem ser incluídas.

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

Clique em Aprovar rascunho. Agora, ao visitar a página de aterrissagem do Marketo, o texto de dica é a cor que você definiu no CSS. Para obter mais informações sobre o Marketo Forms, [visite a documentação](/help/javascript-api/forms-api-reference.md).

Publicado em _2015-01-14_ por _Murta_

## Obter dados de atividade por meio da API REST

Digamos que você queira obter todos os clientes em potencial que foram adicionados a uma lista este mês. Usando a [Obter API REST de Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET), você pode obter esses dados. Antes de chamar a API Obter Atividades de Cliente Potencial, é necessário obter um token de acesso da API de Autenticação e também um token de data de início da [API Obter Token de Paginação](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getActivitiesPagingTokenUsingGET). Abaixo está um código de exemplo no Ruby que percorre os endpoints de API individuais que você teria que chamar para retornar todos os leads adicionados a uma lista este mês. 1. Obter token de acesso**

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com/identity/oauth/token?grant_type=client_credentials>"
# Relace with your client id
client_id = "99985d09-22a9-3jl2-84av-f5baae7c3a45"
# Replace with your your  client secret
client_secret = "tZPVrKiEmUDezE18yZfeaPlTJ2vKn2fw"
request_url = marketo_instance + "&client_id=" + client_id + "&client_secret=" + client_secret

# Make request
response = RestClient.get request_url

# Parse reponse and return only access token
results = JSON.parse(response.body)
access_token = results["access_token"]
puts access_token
```

1. Obter token de paginação

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities/pagingtoken.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify date
since_date_time = "&sinceDatetime=2015-01-01T00:00:00-08:00"
request_url = marketo_instance + endpoint + auth_token + since_date_time

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Obter Dados de Atividade** Para determinar a ID de Tipo de Atividade necessária para esta chamada, consulte a [API de Tipos de Atividade Obtidos](/help/rest-api/activities.md). A API Obter tipos de atividade retorna um esquema com todos os tipos de atividade e IDs associadas. Por exemplo, retorna a id 12 para novos leads criados e a id 1 para visita à página da Web.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
# Specify activities needed
activity_type_ids = "&activityTypeIds=24"
request_url = marketo_instance + endpoint + auth_token + since_date_time + activity_type_ids

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. A API Obter atividades de lead retorna um token de paginação com cada resposta que você pode usar para paginar por meio do conjunto de resultados.** Para obter mais informações, consulte a [documentação da API REST](/help/rest-api/rest-api.md).

Publicado em _2015-01-20_ por _Murta_

## Destacando projetos abertos do Source criados na plataforma Marketo: Parte dois

Este é o segundo post de uma série em andamento destacando projetos de código aberto construídos em torno da plataforma Marketo pela comunidade do desenvolvedor. Mantemos [uma lista na conta GitHub da Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries), onde rastreamos bibliotecas de clientes e projetos criados pela comunidade de desenvolvedores do Marketo. Abaixo estão três projetos desenvolvidos com base nas APIs do Marketo SOAP e Munchkin. [PunchTab](https://www.punchtab.com/) criou [uma biblioteca do cliente em Python para a API SOAP do Marketo](https://github.com/PunchTab/suds-marketo). [Flickerbox](https://www.flickerbox.com/) criou [uma biblioteca do cliente no PHP para a API do Marketo SOAP](https://github.com/flickerbox/marketo).* [Richard Morrison](https://x.com/mozz100) criou [um script PHP para obter dados de cliente potencial da API do Marketo SOAP e, em seguida, passar esses dados para o cliente usando o JavaScript.](https://github.com/mozz100/marketo-whodat) Este projeto pode ajudá-lo a modificar uma página com base nos dados de um usuário no Marketo.  Estamos animados em ver mais projetos criados pela comunidade de desenvolvedores na plataforma Marketo. Se você estiver trabalhando em um projeto de código aberto para a plataforma Marketo, [envie-o para este repositório GitHub por meio de uma solicitação de pull](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publicado em _2015-01-20_ por _Murta_

## Enviar cliques do mecanismo de recomendação RTP para o Google Analytics

Esta é uma solução para os usuários do Marketo Real-Time Personalization (RTP) verem cliques do Content Recommendation Engine no Google Analytics. Depois que um visitante clica na barra Recomendação de conteúdo, um evento é enviado para o Google Analytics na Categoria do evento &quot;RTP-Recommendations&quot;. No Analytics, o Texto de recomendação (como aparece na barra) será anexado ao Rótulo de evento e o URL do ativo recomendado será anexado à Ação de evento. O script funciona para o Classic Google Analytics e o Google Universal Analytics. Esta tag deve ser colada no final do código de página do HTML, para que seja a última tag antes da tag `</body>`.

```javascript
$( document ).ready(function() {
if(document.getElementsByClassName("insightera-bar-content").length
 >0){
document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].addEventListener("click",
 function(){
assetName
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].innerText;
assetURL
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].href;
assetURL=
 assetURL.substring(assetURL.lastIndexOf("/"),assetURL.indexOf("?iesrc"));
console.log(assetName

 * " | " + assetURL);
if(typeof(_gaq)
 != "undefined"){
//Classic
_trackEvent("RTP-Recommendations",
 assetName , assetURL , {'nonInteraction': 1});
}else
 if(typeof(ga) !="undefined"){
//Universal
ga('send',
 'event',"RTP-Recommendations", assetName , assetURL, {'nonInteraction': 1});
}
});
}
});
```

Publicado em _2015-01-22_ por _Yanir_

## Uso da API REST do Marketo com Boomi: obtenção e armazenamento de um token de autenticação REST

Configurar uma exportação automática de clientes potenciais que atendam a determinados critérios é um caso de uso muito comum com o Marketo. Embora isso não possa ser feito atualmente na interface da Marketo, é muito simples usar a ferramenta de terceiros, como o Dell Boomi, uma lista estática com algumas campanhas de gerenciamento de dados e a API REST da Marketo. A API REST? Eu pensei que Boomi não tinha um Conector de API REST Marketo! Bem, atualmente, isso não acontece, mas é possível fazer a mesma coisa usando o Conector HTTP e definindo manualmente as formas de resposta jSON. A primeira etapa é configurar sua instância do Marketo para usar a API REST, conforme descrito na [página Desenvolvedor de Marketo para API REST](/help/rest-api/rest-api.md). Também presumo que você tenha acesso a uma conta Dell Boomi e tenha as habilidades da Boomi para criar esses tipos de processos de integração. O processo final tem a seguinte aparência e incluirá chamadas para as seguintes operações da API REST do Marketo, cada uma com uma forma de resposta jSON associada que pode ser encontrada no site do desenvolvedor. Para economizar tempo, listei-os abaixo Exemplo de JSON para [Autenticação](/help/rest-api/authentication.md)

```json
{
  "access_token": "",
  "token_type": "",
  "expires_in": 0,
  "scope": ""
}
```

Exemplo de JSON para [Obter Vários Clientes Potenciais por ID de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Exemplo de JSON para [Remover clientes em potencial da lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Definir propriedades: antes de começarmos a chamar REST, é importante externalizar e encapsular as variáveis que você está usando. Defini o seguinte mostrado abaixo.

* ClientID: obtenha isso do Serviço Launchpoint REST
* Segredo do cliente: obtenha isso do serviço Launchpoint REST
* AccessToken: recebemos isso de uma chamada REST
* ListID estática: a ID de LISTA da lista estática em que operaremos. Obtenha isso do URL no Marketo
* Campos: uma lista separada por vírgulas de campos que o serviço rest obtém do Marketo para cada lead. A minha é &quot;id, email,firstName,lastName&quot; * IDStringToDelete: conterá a ID de todos os clientes em potencial na lista estática a ser usada em sua remoção da lista
* ActivityTypes: será usado na parte 2 deste blog, onde eu expandi isso!
* SinceDateTime: Será utilizado na Parte 2 deste blog, onde expandi sobre isto!
* PagingToken: Será usado na parte 2 deste blog, onde eu expandi isso!
* Pasta - Saída: o caminho para a pasta de saída no servidor SFTP. Eu uso &quot;/data/outgoing&quot; neste exemplo. Ela permite parametrizar a operação SFTP para torná-la genérica.

O token de autenticação: como mencionei, colocaremos um Conector na tela após criar o processo com uma forma de início &quot;Sem dados&quot; (essa é apenas uma escolha pessoal, gosto que todos os meus conectores pareçam plugues britânicos).
O Conector deve ser configurado da seguinte maneira: - O conector é um cliente HTTP GET - A conexão usa a URL: `https://123-ABC-456.mktorest.com` (observe que não há /rest no final para que possamos usá-lo para chamadas REST, bem como para obter o token de acesso de identidade. e alteração de 123-ABC-456 para a direita (para sua instância do Marketo) - A operação é &quot;Obter token oAuth&quot; (novo!) - Perfil de solicitação = Nenhum - Perfil de resposta = JSON - Novo perfil chamado &quot;Resposta de token de autenticação&quot; - Tipo de conteúdo: Comum - Método HTTP: GET - Caminho do recurso (adicione 4 sem aspas): &quot;identity/oAuth/token?grant_type=client_credentials&amp;client_id=&quot;; &quot;ClientID (Replacement variable)&quot;; &quot;&amp;client_secret=&quot;; &quot;ClientSecret (Replacement Variável)&quot; - Definir parâmetros em Configurar —> Parâmetros —>(+): definir ClientID = ID do cliente da propriedade do processo; definir ClientSecret = Segredo do cliente da propriedade do processo Depois disso, armazene o token de sucesso na variável &quot;AccessToken&quot; das Propriedades do processo conforme mostrado, extraindo-o da resposta jSON.
O padrão para esta etapa será repetido para as próximas etapas, mas usando novas operações com diferentes perfis de retorno jSON. Na verdade, muitas das chamadas REST serão tratadas da mesma forma com pequenas alterações! Na próxima parcela, expandiremos isso e obteremos uma lista de leads de uma lista estática usando REST! Por enquanto, execute o processo, mas coloque uma forma de interrupção depois de &quot;Definir propriedades&quot; e execute em depuração para garantir que você veja o mesmo token que vê no Marketo. Eles devem combinar perfeitamente!

Publicado em _2015-01-26_ por _João_

## Usar uma API de fonte do Google para adicionar uma fonte personalizada a uma página de aterrissagem do Marketo

**Observação: esta é uma publicação do blog de [Murtza Manzur](https://www.linkedin.com/in/murtzam). Murtza é um evangelista desenvolvedor do Marketo com sede na área da baía de São Francisco.**
Digamos que você esteja criando uma página de aterrissagem no Marketo e deseje usar uma fonte personalizada. Isso é possível usando a API de fontes do Google.  Adicione um método de importação ao arquivo CSS com referência ao Google Fonts:

`@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,300,600);`

Publicado em _2015-01-26_ por _Murta_

## Colocar o nome do lead em maiúscula usando o script de e-mail

Digamos que um lead digite seu nome em minúsculas, como &quot;John Doe&quot;. Mas quando você envia uma campanha de email, gostaria de colocar o nome do lead em maiúsculas no email, como John Doe. Você pode colocar o nome de um lead em maiúsculas usando scripts de e-mail. Veja como fazer isso.

1. No seu programa de email, clique na guia &quot;Meus tokens&quot;.
1. Crie um novo token de script de email arrastando &quot;Script de email&quot; do painel direito para o painel do meio. Nomeie o token.
1. Na caixa de texto Editar token de script, cole o código abaixo. No painel direito, em Objeto de lead, marque a caixa de seleção Nome. Em seguida, clique em Salvar.

```javascript
# set($name = ${lead.FirstName})
# set($formattedFirstName = $name.substring(0).toUpperCase())
$formattedFirstName
```

1. Faça referência ao token no ativo de email. Ele exibirá o nome do lead com a primeira letra em maiúscula. Para obter mais informações sobre scripts de email, consulte a [documentação sobre scripts de email](/help/email-scripting.md).

Publicado em _2015-01-26_ por _Murta_

## Obter todos os clientes em potencial da API REST do Marketo

Havia uma [pergunta em StackOverflow perguntando como obter uma lista de todos os leads da Marketo por meio da API REST](https://stackoverflow.com/questions/28184900/how-do-i-get-the-list-of-all-the-leads-in-marketo). Você pode consultar esses dados usando o [Obter Vários Clientes Potenciais por Ponto de Extremidade de API REST do Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Os clientes potenciais no Marketo recebem IDs de clientes potenciais em ordem sequencial, começando em 1. Usando o [Obter Vários Clientes Potenciais por Ponto de Extremidade da API REST do Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), você pode consultar 300 clientes potenciais por ID de cliente potencial com cada chamada. Você teria que especificar a id como o filterType e as ids de lead como filterValues com cada chamada para esse endpoint. Para obter todos os leads, você iteraria pelo número total de leads 300 de cada vez. Y
Você pode obter a contagem total de clientes potenciais em uma instância do Marketo por meio da interface do usuário do Marketo. Na interface do usuário do Marketo, vá para a guia Banco de dados de clientes potenciais, clique em Listas inteligentes do sistema, clique em Todas as listas inteligentes de clientes potenciais e, por fim, clique na guia &quot;Clientes potenciais&quot;. Em seguida, clique na coluna Id e classifique em ordem decrescente. Depois que os clientes em potencial forem classificados, a ID do primeiro cliente potencial será o limite superior da ID do cliente potencial quando você consultar todos os clientes potenciais. Se você não tiver acesso à interface do usuário do Marketo para obter a contagem total de clientes potenciais, há uma [abordagem alternativa para obter esse valor usando a API REST de Obter Atividades de Cliente Potencial](https://stackoverflow.com/questions/28419967/get-all-leads-programmatically-in-marketo-v1).

1. Primeira chamada de API: substituir ... por todos os valores entre:

`/rest/v1/leads.json?filterType=Id&filterValues=1,2,3,...,298,299,300`

Este é um exemplo de código em Ruby para a primeira chamada.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Replace with filter type and values
ids_needed = (1..300).to_a.join(",")
filter_type_and_values = "&filterType=Id&filterValues=" + ids_needed
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. A segunda chamada de API e cada chamada de API subsequente seguiriam o mesmo padrão até que a contagem total de leads fosse atingida:

```
//replace ... with all the values in between
/rest/v1/leads.json?filterType=Id&filterValues=301,302,303,...,598,599,600
```

Para obter mais informações, consulte a [documentação da API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Publicado em _2015-01-28_ por _Murta_

## Executar ações de envio de formulário do Iframe para a página principal

Vimos alguns casos em que os usuários usam formulários iframe e desejam direcionar os visitantes que preencheram o formulário para uma página de agradecimento ou para o PDF, vídeo etc. O problema é que, como o formulário está incorporado em uma página de aterrissagem diferente da principal, a ação acontece somente na página interna onde o formulário está. Para resolver isso, abaixo estão duas tags do JavaScript que criamos. Insira como um elemento do HTML nas páginas do iframe ou diretamente no modelo de página de aterrissagem usado para iframes. Coloque-o antes da última tag `</body>`. A primeira tag executa a ação na página principal e a segunda tag a abre em uma nova guia.

**Ação de Formulário em uma Página Pai**

```javascript
<script>
MktoForms2.whenReady(function (form){
form.onSuccess(function (values, url){
window.parent.location.assign(url);
return false;
           });
});
</script>
```

**Ação de formulário em uma nova guia**

```javascript
<script>
MktoForms2.whenReady(function (form){
var newWin;
form.onSubmit(function (){
newWin = window.open('about:blank', 'myWindow');
});
form.onSuccess(function (values, url){
newWin.location.replace(url);
return
 false;
});
});
</script>
```

Publicado em _2015-02-02_ por _Yanir_

## Enviar dados de exibição de um vídeo do YouTube para o mercado

Digamos que você queira segmentar leads no Marketo com base em se eles iniciaram ou concluíram um vídeo específico. Isso é possível fazer usando o Munchkin, a API Iframe do YouTube e as Smart Lists no Marketo. O código de exemplo desta publicação permitirá o envio de eventos de vídeo iniciado e vídeo concluído para o Marketo por meio do Munchkin. Para que isso funcione, o Munchkin também deve ser carregado na página antes que você possa começar a enviar eventos de visualização de vídeo para o Marketo. O vídeo iniciado e concluído será exibido no log de atividades do lead. Depois que os dados estiverem no Marketo, você poderá criar uma lista inteligente e leads de segmento que iniciaram ou concluíram um vídeo.

1. Obtenha a ID do vídeo do YouTube que deseja incorporar.** Na URL do vídeo do YouTube que você gostaria de usar, observe a id, que é a série de caracteres aleatórios após `v=`.
1. Coloque a ID de vídeo do YouTube da Etapa um na oitava linha desta amostra de código. Em seguida, coloque o código antes de `</body>` no HTML de sua página.

```javascript
<div id="player"></div>
<script>
var tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
document.getElementsByTagName('head')[0].appendChild(tag);

//Change 'iiqxcjxJ5Us' to video needed
var player, videoId = 'iiqxcjxJ5Us';
function onYouTubeIframeAPIReady() {
player = new YT.Player('player', {
height: '390',
width: '640',
videoId: videoId,
events: {
'onStateChange': onPlayerStateChange
}
});
}

function onPlayerStateChange(event) {
switch( event.data ) {
//Send video started event to Marketo
case YT.PlayerState.PLAYING: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=started'
}
);
break;
//Send video finished event to Marketo
case YT.PlayerState.ENDED: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=finished'
}
);
break;
}

}
</script>
```

1. Crie uma Smart List no Marketo com o URL do vídeo e o evento de visualização que você está procurando como o valor de &quot;Querystring contém&quot;. Para obter mais informações sobre a API do YouTube Iframe, [visite a documentação da API do YouTube](https://developers.google.com/youtube/iframe_api_reference). Para obter mais informações sobre o Munchkin, [consulte a documentação para desenvolvedor do Marketo](/help/javascript-api/lead-tracking.md).

Publicado em _2015-02-02_ por _Murta_

## Dicas e truques da API do Marketo SOAP

OBSERVAÇÃO: Esta é uma publicação de blog de convidados. [Ed Blachman é arquiteto sênior](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D2777965) da [TIBCO Software, um conhecido fornecedor de software corporativo](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Ed está trabalhando em produtos que permitem que o que a Gartner chama de &quot;desenvolvedores cidadãos&quot; integrem os serviços em nuvem que usam sem precisar fazer qualquer programação por conta própria. A [API SOAP do Marketo](/help/soap-api/soap-api.md) é uma ferramenta poderosa pela qual os desenvolvedores podem aproveitar o poder do Marketo e integrá-lo a nossos próprios aplicativos. Entre [a documentação formal](./getting-started.md) e [os recursos da comunidade](https://nation.marketo.com/), há muitas informações disponíveis sobre como usá-la. Quando eu estava começando, eu me baseava fortemente nessas informações e as achava inestimáveis. Entretanto, naquele processo, eu construí algumas dicas e truques que não tinha visto em nenhum desses lugares. Aqui está um pouco do que eu descobri.

**Sandbox para desenvolvedores** A sandbox é, é claro, um recurso maravilhoso para desenvolvedores de API: um lugar seguro em que você pode experimentar os recursos do Marketo, adicionar e remover objetos sem interferir nas atividades de marketing reais realizadas pelos usuários reais do Marketo de sua organização. No entanto, a sandbox não é uma panaceia.
Por exemplo, eu precisava compartilhar nossa Sandbox com outro grupo de desenvolvimento, e isso exigiu um pouco de ação, porque eles se acostumaram com a noção de que eram os donos da Sandbox. Eventualmente, descobrimos algumas práticas recomendadas para compartilhamento: - Não escreva testes que dependem do conhecimento completo do conteúdo de sua sandbox. Como um recurso compartilhado, os esquemas podem estar sujeitos a alterações sem aviso prévio, bem como entradas inteiras no banco de dados de clientes potenciais, programas ou outras entidades. Se os testes suportarem um conhecimento completo da Sandbox, o ciclo de desenvolvimento criará períodos de blecaute para os grupos com os quais você a está compartilhando. Como normalmente o ciclo de desenvolvimento deles não coincidirá com o seu, isso equivale a monopolizar o recurso - não é legal. Também não é necessário, se você pensar bem. - Use uma convenção para rotular todas as suas coisas - seus leads, seus campos de esquema de leads, seus programas, o que for. Se cada um de vocês puder identificar seus próprios objetos e se concordar com seus colocatários de que cada um deixará os objetos dos outros sozinhos, você deverá ter uma base sólida para o compartilhamento. Para clientes potenciais, você pode criar um campo personalizado e criar uma convenção usando esse campo personalizado para identificá-los como clientes potenciais de teste. Para listas ou programas, você pode iniciar os nomes de seus objetos com uma string que identifica esses objetos como pertencentes a você. - Considere escrever testes que limpam depois de si mesmos - que primeiro criam os objetos em que você está interessado e, em seguida, acessam ou atualizam ou excluem seletivamente, depois finalmente os removem. (Observe que isso não é realizável a 100% na API do SOAP, pois nem tudo na sandbox, ou em uma instância real, pode ser gerenciado por meio da API do SOAP. Mesmo assim, ainda vale a pena fazer isso o máximo que puder.)

**Instâncias reais** O problema com a Sandbox é que ela não está sendo usada em produção, portanto, é difícil ter uma ideia de como o uso real se parece em uma instância do Marketo. Agora, se você tiver a sorte de ter um usuário avançado do Marketo em sua equipe, ou se estiver fazendo desenvolvimento sob medida para usuários internos do Marketo, isso não é um grande problema. Mas no caso da minha equipe, foi um grande negócio mesmo. Nenhum de nós era especialista na Marketo e, como nos pediram para compreender um grande número de serviços em nuvem, simplesmente não tínhamos pessoal para ser especialistas em nada. Estes são alguns dos insights que obtivemos do acesso a uma instância real: - Grandes esquemas de clientes potenciais. O esquema de cliente potencial na instância de produção que acessamos tem mais de 200 campos. Isso tornou extremamente claro para nossos designers de interface do usuário que a interface do usuário que eles estavam projetando tinha que acomodar esquemas desse tamanho (ou maiores). - Uso intermitente. Vimos duas diferenças de magnitude entre os tempos de maior uso e de baixo uso (em termos de números de leads criados ou atualizados). Isso afetou o volume de dados que receberíamos das chamadas de API (óbvio) e o tempo necessário para uma chamada de API responder (possivelmente menos óbvio).

**Tempo de resposta da chamada da API** Dependendo da hora do dia, dos detalhes da chamada da API e do conteúdo da sua instância, talvez você encontre um tempo de resposta da API SOAP maior do que a média. Ocasionalmente, tínhamos chamadas de API que demoravam um minuto e meio para responder. Você precisa estar ciente da possibilidade de lidar com isso: - Teste. Talvez isso não seja um problema para o seu uso. Mas não apenas suponha que, faça alguns testes. - Ajuste seu uso. No nosso caso, o maior problema foi que definimos o tamanho da página das nossas chamadas para [getMultipleLeads](/help/soap-api/getmultipleleads.md) para o tamanho permitido pela API. Em nosso contexto, isso faz algum sentido, pois nosso objetivo é ser o mais eficiente possível com a cota de API do cliente. Mas, em seu contexto, talvez você não precise se preocupar tão intensamente com as cotas de chamada da API dos seus usuários. Nesse caso, você definitivamente terá um tempo de resposta melhor solicitando páginas menores de dados.

**Particionamento de clientes potenciais** O Marketo fornece ferramentas-partições e espaços de trabalho avançados, que permitem que vários grupos de marketing compartilhem uma única instância do Marketo. No entanto, essas ferramentas não são refletidas diretamente na API do SOAP. Por exemplo, ao usar getMultipleLeads para obter todos os leads que foram atualizados ou criados desde algum datetime, você recupera todos os leads na sua instância para os quais esse é o caso, sem considerar (e sem nada indicar) qual partição ou espaço de trabalho contém determinado lead. A criação e a adição de leads a listas são outros contextos nos quais o particionamento de leads pode afetar o que as chamadas de API realmente fazem. Observe que isso significa que as partições e os espaços de trabalho podem não ser a solução necessária para o problema de compartilhamento de sandbox discutido acima. Então, como você descobre se isso é um problema para você? Achei tudo isso útil: os evangelistas dos desenvolvedores estão comprometidos com o nosso sucesso no uso das APIs e, quando há perguntas, são incrivelmente bons em trabalhar para encontrar respostas. - [Documentação da API](./getting-started.md). Os Evangelistas já trouxeram esta questão para a documentação, e como parte de seu compromisso com o nosso sucesso, eles são realmente bons em atualizar o documento. - Seus Próprios Casos De Teste. Embora o uso de partições e espaços de trabalho para compartilhar a sandbox possa não ser uma ótima ideia, a sandbox é um ótimo local para trabalhar com partições e espaços de trabalho para descobrir se eles representam desafios para o uso pretendido. (É também uma boa maneira de restringir suas perguntas para os evangelistas, que são sempre uma boa ideia.)

**TIMTOWTDI e teste** &quot;Há mais de uma maneira de fazer isso&quot; - o lema de programação Perl realmente se aplica em determinados contextos à API SOAP do Marketo. Por exemplo, eu queria combinar a atualização de um conjunto de leads com a adição desses leads a alguma lista. A API do SOAP oferece duas maneiras de fazer isso: 1. [importToList](/help/soap-api/importtolist.md) + [getImportToListStatus](/help/soap-api/getimporttoliststatus.md). Ao ler a documentação, essa é obviamente a maneira &quot;normal&quot; de fazer isso. No entanto, o fato de que você precisa pesquisar o status da sua operação de importação levantou um sinalizador amarelo para mim. Esta era realmente a maneira que eu queria implementar minha importação? 1. [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) + [listOperation](/help/soap-api/listoperation.md). Isso parece muito menos elegante do que uma chamada importToList unitária, mas não depende de pesquisa. Era uma opção viável? Casos como este são difíceis para os evangelistas lidarem, porque eles realmente dependem da natureza das instâncias com as quais você está lidando e exatamente o que você está tentando fazer. Felizmente, se você configurou um ambiente de teste de unidade robusto, é possível usá-lo para explorar questões como essas também. Nesse caso específico, descobriu-se que a opção 2 era melhor para o meu caso de uso do que a opção 1, não por causa da pesquisa, mas porque eu enfrentava limitações orientadas a campo em importToList e também porque estava tentando escrever código que poderia ser usado em contextos e instâncias sobre as quais eu não tinha controle. Mas seu caso de uso pode ser diferente — e testar é a única maneira de descobrir.

**Conclusão** Não acho que isso seja um segredo enorme. Por outro lado, eu estaria à frente do jogo se soubesse de tudo isso antes de começar. Espero que você o ache útil.

Publicado em _2015-02-05_ por _David_

## Uso da API REST do Marketo com Boomi: Obtenção e exclusão de clientes em potencial de uma lista estática

Na parte 1 desta série, discuti como era possível começar a usar a REST API por meio do Boomi com o conector HTTP do Boomi, obtendo especificamente o token de autenticação necessário para acessar a REST API e armazenando-o em uma Variável de processo. A seguir, começaremos a fazer chamadas para o Marketo e, nesta parte, mostrarei como você pode [Obter Vários Clientes Potenciais por ID de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists) e [Remover Clientes Potenciais da Lista](/help/rest-api/lead-database.md). Preste atenção especial à remoção de leads de uma lista porque há um aspecto muito &quot;levemente documentado&quot; e sutil de Boomi no trabalho lá que eu expandi quando chegamos lá.

Em nossa próxima parcela, expandimos essa funcionalidade para começar a fazer coisas interessantes, como obter atividade de lead, mas isso é um blog para outro dia. Nesta parte, veremos a segunda e terceira áreas destacadas. Como revisão, incluí as respostas JSON que precisamos abaixo. Lembre-se de que para criar um perfil JSON em Boomi, tudo o que você precisa fazer é criar um componente de perfil do tipo JSON, clicar em &quot;importar&quot; e selecionar o arquivo. Boomi faz o resto, extrapolando coisas como se houvesse várias IDs permitidas. Exemplo de JSON para [Obter Vários Clientes Potenciais por ID de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Exemplo de JSON para [Remover clientes em potencial da Solicitação de Lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
   "input":[
      {
         "id": ""
      },
      {
         "id": ""
      }
   ]
}
```

Exemplo de JSON para [Remover clientes em potencial da resposta da lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

O Obter vários clientes em potencial por ID de lista** Solte outro conector (Obter) no seu processo, usando a mesma conexão definida no artigo anterior. Crie uma nova operação chamada &quot;Obter vários clientes em potencial por ID de lista&quot; (Sou um adesivo para fins de consistência) Seus atributos são os seguintes: Perfil de solicitação: Nenhum (este usa o URL de solicitação) Tipo de perfil de resposta: jSON - Perfil de resposta: Crie um novo perfil com base na resposta Obter vários clientes em potencial por ID de lista acima. Observe que você pode alterá-lo para que ele retorne os campos desejados, não apenas os listados. É importante lembrar que o perfil de resposta JSON deve realmente corresponder à lista de campos que você está solicitando da API REST, e você deve solicitar apenas os campos necessários. No objeto Propriedades do processo, definimos uma propriedade chamada &quot;campos&quot;, que é uma lista separada por vírgulas dos campos que você deseja que REST retorne. e esta é a lista que tem que bater com o perfil. Tipo de conteúdo: texto/simples (esta é apenas uma solicitação de URL) Método HTTP: GET (você pesquisa nos documentos da API REST, ela sempre é listada) Caminho do recurso (adicione 5) rest/v1/list/ listID (variável de substituição) /leads.json?access_token= access_token (variável de substituição) &amp;fields= fields (variável de substituição). Em seguida, na guia Parâmetros no conector, é possível inserir os valores da variável, que foram preenchidos anteriormente nas propriedades do processo. Na próxima seção, falarei sobre como evitar o preenchimento manual. Vou pular a parte do processo em que mapeio a resposta de Obter vários clientes em potencial por ID de lista em um perfil de arquivo simples e colocá-la em um servidor FTP, pois essa é uma funcionalidade Boomi simples.

Excluir clientes em potencial de uma lista É interessante, um de meus colegas, [Ken Niwa](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D7429494), ensinou-me essa próxima técnica, é muito legal e baseia-se em um artigo da Boomi intitulado &quot;Como criar uma solicitação POST para um aplicativo RESTful&quot;, mostrado abaixo.  ...mas as primeiras coisas primeiro. No processo, a partir da forma &quot;Obter vários clientes em potencial por ID de lista&quot;, temos a forma Obter vários clientes em potencial por resposta de ID de lista, e precisamos mapear isso para a forma &quot;Remover clientes em potencial da solicitação de lista&quot;. Esse mapeamento é bastante simples, basta mapear a ID que obtivemos dos clientes em potencial na lista original para a lista de ID que estamos passando para o jSON de exclusão. Em seguida, solte outro Conector com uma ação de &quot;Enviar&quot;, usando a mesma Conexão. Crie uma nova operação chamada &quot;Remover leads da solicitação de lista&quot;. cujos atributos são Perfil de solicitação: Tipo de conteúdo jSON: aplicativo/perfil de solicitação json: [Perfil JSON] Remover clientes em potencial da solicitação de lista (criada a partir do arquivo acima) Tipo de perfil de resposta: Perfil de resposta jSON: [Perfil JSON] Remover clientes em potencial da resposta de lista (criada a partir do arquivo acima) Tipo de conteúdo: aplicativo/json Método HTTP: Caminho do recurso DELETE (adicionar 4) rest/v1/lists/ listID (variável de substituição) /leads.json?access_token= access_token (variável de substituição) Veja o que há de interessante sobre esse conector. NÃO adicionaremos explicitamente os parâmetros na guia do conector. Em vez disso, como o artigo declara, criamos propriedades de documento dinâmicas que têm os mesmos nomes que as variáveis de substituição. Nesse caso, essas variáveis são listID e access_token. Quando você faz isso, a forma jSON flui para a chamada REST e os parâmetros aparecem em seu lugar adequado no URL. Não podemos fazer isso com a chamada anterior porque é um GET e não um POST. Assim, neste ponto, você viu uma chamada de API REST GET e POST e pode começar a ver o padrão para fazer essas chamadas REST. Na próxima parcela, começaremos a observar a exportação da atividade de lead por meio da API REST, que é um pouco mais envolvida.

Publicado em _2015-02-06_ por _João_

## Incorporar um vídeo do YouTube com rastreamento de leads em uma página de aterrissagem do Marketo

Em uma publicação anterior no blog, descrevi como segmentar leads no Marketo com base em se eles iniciaram ou concluíram um vídeo específico do YouTube. Nesta publicação do blog, abordamos como obter a implementação dessa publicação e usá-la em uma página de aterrissagem do Marketo.

1. Navegue até o Programa no Marketo em que deseja criar a nova landing page. Clique em Novo ativo local e em Página de aterrissagem.
1. Nomeie a landing page. Atribuir um URL de página. Selecione um modelo. Em seguida, clique em Criar.
1. Depois que a landing page for criada, clique em Editar rascunho.
1. No painel direito, arraste o botão HTML até a tela principal à esquerda.
1. Na caixa Editor HTML personalizado que aparece. Em seguida, clique em Salvar.
1. Ajuste o tamanho de um elemento HTML arrastando o contorno da caixa. Em seguida, clique em Aprovar e fechar.
1. Teste a versão ao vivo da página de aterrissagem clicando em Exibir página aprovada. Uma landing page com o YouTube é aberta em uma nova janela. O vídeo iniciado e concluído será exibido no log de atividades do lead, como mostrado na primeira e segunda captura de tela abaixo. Depois que os dados estiverem no Marketo, você poderá criar uma lista inteligente e leads de segmento que iniciaram ou concluíram um vídeo, conforme mostrado na captura de tela abaixo. Para obter mais informações sobre a API do YouTube Iframe, [visite a documentação da API do YouTube](https://developers.google.com/youtube/iframe_api_reference). Para obter mais informações sobre o Munchkin, [revise a documentação para desenvolvedor do Marketo](/help/javascript-api/lead-tracking.md).

Publicado em _2015-02-09_ por _Murta_

## Rastreamento web de aplicativo de página única com o Munchkin

Um Aplicativo de página única é um site que carrega todos os recursos necessários para navegar pelo site no primeiro carregamento de página. Quando um usuário clica em um link, o conteúdo é carregado dos dados do primeiro carregamento da página. Para o usuário, o site se comporta conforme esperado porque o URL na barra de endereços é como a navegação de página tradicional. O Munchkin funciona bem com sites tradicionais, pois o Munchkin é executado sempre que os usuários carregam uma nova página. No entanto, com um aplicativo de página única, se você não estiver carregando uma nova página, o Munchkin será executado apenas uma vez. A abordagem que eu abordo nesta publicação é rastrear quando um usuário clica em um link e, em seguida, enviar essas informações para o Munchkin. Implementamos isso usando a função Munchkin `clickLink`. Abaixo está um exemplo de implementação no jQuery que vincula eventos de clique ao método Munchkin `clickLink`. Ao chamar o método Munchkin `clickLink`, ele passa o parâmetro para a URL que foi clicada.

```javascript
<script>
$("a").on('click', function(event) {
    var urlThatWasClicked = $(this).attr('href');
    Munchkin.munchkinFunction('clickLink', { href: urlThatWasClicked});
});
</script>
```

Publicado em _2015-02-11_ por _Murta_

## Alterar a pontuação de um lead por meio da API REST

Digamos que você queira alterar a pontuação de um lead no Marketo usando as APIs. Isso é possível para fazer com a API REST usando o endpoint Criar/atualizar lead. Abaixo está uma amostra de código em Ruby que mostra como fazer essa chamada.

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "https://AAA-BBB-CCC.mktorest.com"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

No corpo JSON da solicitação, especificamos `updateOnly` como a ação. Isso significa que a solicitação só funcionará se o lead existir, caso contrário, haverá uma falha. Se você deseja criar um cliente em potencial, caso um não exista, especifique `createOrUpdate` como a ação. Usamos o email do lead como o identificador principal para localizar o registro do lead no Marketo. Finalmente, especificamos o valor da pontuação do lead usando a chave `leadScore`. É possível atualizar 300 leads de cada vez usando esse método.

Publicado em _2015-02-19_ por _Murta_

## Destacando projetos abertos do Source criados na plataforma Marketo: Parte três

Este é o terceiro post de uma série em andamento, destacando projetos de código aberto construídos em torno da plataforma Marketo pela comunidade do desenvolvedor. Mantemos [uma lista na conta GitHub da Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries), onde rastreamos bibliotecas de clientes e projetos criados pela comunidade de desenvolvedores do Marketo. Abaixo estão três projetos desenvolvidos em torno das APIs REST do Marketo. **[Usermind](http://www.usermind.com/) criou [uma biblioteca de cliente Node.js para a API REST do Marketo](https://github.com/MadKudu/node-marketo).** **[Arunim Samat](https://github.com/asamat) criou [uma biblioteca do cliente em Python para a API REST do Marketo](https://github.com/asamat/python_marketo).** **[Jacques Lemieux da Marketo](https://www.linkedin.com/in/jalemieux) criou [uma biblioteca cliente em Ruby para a API REST do Marketo.](https://github.com/jalemieux/mkto_rest)** Estamos animados em ver mais projetos criados pela comunidade de desenvolvedores na plataforma Marketo. Se você estiver trabalhando em um projeto de código aberto para a plataforma Marketo, [envie-o para este repositório GitHub por meio de uma solicitação de pull](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publicado em _2015-02-20_ por _Murta_

## Inserir um formulário do Marketo em uma campanha RTP

Muitos profissionais de marketing estão interessados em inserir um formulário do Marketo em uma campanha do Marketo Real-Time Personalization (RTP). Seja um tipo de campanha de Diálogo, Na Zona ou Widget RTP, você pode copiar seu código do Form HTML e colá-lo no editor de campanha do RTP. Eu vi estes exemplos disto sendo usado: - Fazer com que os visitantes se inscrevam no seu boletim informativo depois de um segundo ou terceiro clique no seu site - Formulário de inscrição rápido e eficaz para webinários - Baixar um estudo de caso fechado - Oferecer leads que cancelaram a inscrição no passado para se reinscrever Preencha um formulário na campanha e receba o agradecimento ou conteúdo solicitado, resultando em menos um clique para atingir suas metas. Aqui vai a explicação de como fazer isso e incorporar um Marketo Form 2.0 em uma campanha Marketo RTP. Veja um ótimo exemplo abaixo do eMarketer, os usuários da RTP que deram um passo além e decidiram exibir uma mensagem de agradecimento na campanha da RTP, em vez de direcionar os visitantes para uma página de agradecimento. O código para essa opção também está abaixo. Aproveite e estou feliz por saber da sua experiência com ele!

1. Clique com o botão direito do mouse em um formulário aprovado. Selecione o **Código incorporado.**
1. Copie o **Código.**
1. No Marketo RTP, vá para **Campanhas**.
1. Clique em **CRIAR NOVA CAMPANHA**.
1. No Editor de Rich Text, clique no **ícone do HTML**.
1. Cole o código de inserção do formulário no HTML Source Editor. Clique **Atualizar**.
1. O formulário não será exibido na visualização do editor, mas você pode visualizá-lo para ver como ele será renderizado em uma campanha.
1. Clique em **Iniciar** para iniciar a campanha.

### Observação

Quaisquer alterações no formulário devem ser feitas nas Atividades de marketing da Marketo em Editar rascunho do formulário.

### Artigos relacionados

* [Forms 2.0](/help/javascript-api/forms-api-reference.md)

Publicado em _2015-12-20_ por _Yanir_

## Adicionar um botão de redefinição a um formulário do Marketo

```javascript
<script src="//app-sj01.marketo.com/js/forms2/js/forms2.min.js"></script>
<form id="mktoForm_116"></form>
<script>MktoForms2.loadForm("//app-sj01.marketo.com", "410-XOR-673", 116,
function(form) { form.getFormElem()[0].querySelector('button[type="submit"]').insertAdjacentHTML('afterend','<button type="reset" class="mktoButton">Reset</button>') });
</script>
```

Publicado em _2015-03-18_ por _Murta_

## Atualizações da versão de março de 2015

A [API do Marketo REST Asset foi lançada na versão de março de 2015](https://developer.adobe.com/marketo-apis/api/asset/). Essa API permite acessar os objetos de modelo de arquivo, pasta, token, email e email da Marketo. Observe que duas permissões de função foram adicionadas para fornecer acesso aos endpoints da API do ativo: Assets somente leitura, Assets leitura-gravação. Se sua função de usuário da API for anterior ao lançamento das APIs de ativos, será necessário criar uma nova função de usuário da API com essas permissões para habilitar o acesso. Caso contrário, você receberá uma resposta de erro 603 &quot;Acesso negado&quot;. Além da versão da API de ativos REST, houve atualizações para endpoints existentes da API REST. O [ponto de extremidade da API REST de cliente potencial de mesclagem](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) foi atualizado para permitir a mesclagem de vários clientes potenciais. O [ponto de extremidade da API REST do Agendar Campanha](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) foi atualizado para permitir a clonagem de uma campanha ao agendar uma campanha.

Publicado em _2015-03-23_ por _Murta_

## Acionando Campanhas RTP com Atraso

Esse JavaScript personalizado permite que os usuários do RTP exibam campanhas alguns segundos após o carregamento da página da Web. Isso é recomendado para campanhas de caixas de diálogo e widgets. Ele pode ser usado para mostrar uma campanha após um atraso, uma vez que o visitante visualizou o conteúdo regular na página. É recomendável implementar esse código apenas em páginas específicas nas quais as campanhas devem ser exibidas. O uso desse código em todas as páginas não é recomendado, pois pode afetar o desempenho. **Instruções de Instalação** O código personalizado envia um Evento de Dados Personalizados RTP (t=timeOnPage, ou seja: t=60) e carrega a campanha que corresponde a esse evento. Por padrão, ele será acionado após 60 segundos. Você pode personalizá-lo alterando o parâmetro sendCustomRTPEvent para qualquer outro número. Coloque o código imediatamente após o código RTP padrão:

```javascript
<script>
function sendCustomRTPEvent(a){
 var eventValue="t="+a;
 setTimeout(function(){
  rtp('send', 'event', {value: eventValue});
  rtp('get', 'campaign',true);
 }, 1000 \* a);
}
sendCustomRTPEvent(60); //Seconds
</script>
```

Para configurar uma campanha RTP para reagir após um atraso: 1. Efetue login em sua conta RTP 1. Criar um novo segmento 1. Na seção Eventos de segmento, adicione: `t=60`. Um visitante só pode corresponder a cada segmento do RTP uma vez por sessão, portanto, ver cada campanha apenas uma vez (a menos que esteja definido como aderente).

Publicado em _2015-03-24_ por _Yanir_

## Atualizações da versão de abril de 2015

### Marketo Mobile Engagement SDK v0.3.2

O Marketo agora inclui automação de marketing e engajamento do usuário para aplicativos móveis. A instalação da [Marketo Mobile SDK](/help/mobile/mobile.md) no aplicativo iOS ou Android permite que os profissionais de marketing ouçam eventos no aplicativo e enviem notificações de push relevantes.

### Aprimoramentos da API REST

* Objetos personalizados

Foram introduzidos novos [pontos de extremidade de objeto personalizados](/help/rest-api/custom-objects.md) que permitem listar, descrever e ATUALIZAR programaticamente os dados residentes com um objeto personalizado do Marketo.

Observe que as permissões de função foram adicionadas para fornecer acesso aos pontos de extremidade da API de objeto personalizado: Objeto personalizado somente leitura, Objeto personalizado leitura-gravação. Se sua função de usuário da API for anterior ao lançamento das APIs de objetos personalizados, será necessário criar uma nova função de usuário da API com essas permissões para habilitar o acesso. Caso contrário, você receberá uma resposta de erro 603 &quot;Acesso negado&quot;.

* Programar campanha - programa de clonagem

Um novo parâmetro opcional &quot;cloneToProgramName&quot; foi introduzido na [API de campanha de agendamento](/help/rest-api/data-ingestion.md). Quando esse parâmetro estiver presente, o programa principal da campanha será clonado e a campanha recém-criada será agendada. O parâmetro especifica o nome desejado para o programa resultante.

Publicado em _2015-04-28_ por _Travis Kaufman_

## Sincronização de cancelamentos de assinatura de email entre instâncias

Você gerencia várias instâncias do Marketo? Manter as informações de clientes potenciais sincronizadas entre instâncias pode ser desafiador. Esta é uma maneira de sincronizar cancelamentos de assinatura de email entre instâncias usando um webhook que chama um serviço da Web externo. O serviço Web externo faz um loop por cada instância procurando pelo lead conhecido que acionou o evento de cancelamento de inscrição. Quando um cliente potencial correspondente é encontrado, o campo &quot;cancelado&quot; no registro de cliente potencial correspondente é atualizado. Aqui está um diagrama ilustrando a ideia.  Cabe a você implementar o serviço Web, mas o código abaixo deve ajudá-lo a iniciar o processo rapidamente!

### Serviço Web Externo

O serviço Web externo executa as seguintes etapas para cada instância do Marketo que precisa ser sincronizada:

1. Compõe a API REST específica da instância [URL do Ponto de Extremidade](/help/rest-api/endpoint-reference.md)
1. Obtém o token de acesso usando a [Identidade](/help/rest-api/authentication.md)
1. Obtém a lista de registros de cliente potencial que correspondem ao endereço de email usando [Obter Vários Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)
1. Atualiza o campo &quot;cancelado&quot; de cada registro de cliente potencial usando Criar/Atualizar Clientes Potenciais

Aqui está outro diagrama mostrando a chamada de serviço Web externo e as chamadas da API REST do Marketo em detalhes.  O código de amostra abaixo não é um serviço Web pronto para uso. Em vez disso, é um programa de modo de console para o qual você pode passar argumentos por meio da linha de comando. O objetivo aqui é mostrar como chamar as APIs apropriadas do Marketo para atualizar registros de clientes potenciais nas instâncias. A implementação do serviço Web é deixada como um exercício para o leitor.
**Código de exemplo** Para ativar e executar o código de exemplo, é necessário criar um projeto Java em seu IDE favorito. Depois disso, será necessário fazer as seguintes alterações: 1. O código de exemplo usa [json-simple](https://code.google.com/archive/p/json-simple) para analisar cadeias de caracteres JSON. Adicione o jar simples de json ao seu projeto Java. 1. O código de amostra tem uma estrutura que armazena metadados para cada instância do Marketo. Coloque os valores reais das instâncias na estrutura da seguinte maneira:

```java
public static String instanceInfo[][] = {
{ "AccountId1", "ClientId1","ClientSecret1" },    // Instance 1 metadata
{ "AccountId2", "ClientId2","ClientSecret2" },    // Instance 2 metadata
{ "AccountId3", "ClientId3","ClientSecret3" }     // Instance 3 metadata
};
```

Você pode encontrar os metadados da instância no painel Admin do Marketo:

* Administrador da ID de conta > Integração > Munchkin > Conta da Munchkin
* Id Do Cliente E Administrador Do Client Secret > Integração > LaunchPoint > Sincronização De Cancelamento De Inscrição De Email > Exibir Detalhes

O código de amostra usa dois argumentos de linha de comando que simulam os parâmetros de consulta &quot;id&quot; e &quot;email&quot; para o serviço Web externo descrito acima.

1. args[0] = ID da conta
1. args[1] = Endereço de email

Transmita valores reais da sua instância como argumentos para o programa. Esta é uma captura de tela de configuração de projeto do Intellij IDEA.

**SyncEmailUnsubscribe.java**

```java
package com.marketo;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Scanner;

public class SyncEmailUnsubscribe {
    // Define Marketo instance meta data here.
    // Each row contains three elements: Account Id, Client Id, Client Secret.
    // For example:
    //  public static String instanceData[][] = {
    //    {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    //  };
    //
    public static String instanceData[][] = {
            // ADD YOUR INSTANCE META DATA HERE
    };

    public static void main(String[] args) {
        String accountId = args[0];     // Account id that processed the unsubscribe
        String emailAddress = args[1];  // Email address of lead that unsubscribed

        SyncEmailUnsubscribe seu = new SyncEmailUnsubscribe();

        // Loop through each Marketo instance
        for (int i = 0; i < instanceData.length; i++) {

            // Make sure we skip instance that triggered the webhook
            if (!accountId.equals(instanceData[i][0])) {
                String endpointUrl = String.format("https://%s.mktorest.com", instanceData[i][0]);

                // Generate access token
                String identityUrl = String.format("%s/identity/oauth/token?grant_type=client_credentials&client_id=%s&client_secret=%s", endpointUrl, instanceData[i][1], instanceData[i][2]);
                String token = seu.getToken(identityUrl);

                // Get lead records for given email address (may be duplicates)
                String getLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=email&filterValues=%s", endpointUrl, token, emailAddress);
                String leads = seu.getLeads(getLeadsUrl);

                // Update unsubscribed field in lead record
                String updateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", endpointUrl, token);
                seu.updateLeads(updateLeadsUrl, leads, accountId);
            }
        }

        System.exit(0);
    }

    // Call Identity Service to generate access token
    public String getToken(String url) {
        // Call Identity Service
        String tokenData = getData(url);

        // Convert response into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(tokenData);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }

        // Retrieve access_token
        JSONObject jsonObject = (JSONObject)obj;
        return jsonObject.get("access_token").toString();
    }

    // Call Get Multiple Leads by Filter Type Service to get lead records
    public String getLeads(String url) {
        return getData(url);
    }

    // Call Create/Update Lead Service to update "unsubscribed" flag in lead record
    public void updateLeads(String url, String leads, String account) {
        JSONObject body = composeBody(leads, account);
        if (body != null) {
            postData(url, body);
        }
    }

    // Compose JSON body for Create/Update Leads Service
    private JSONObject composeBody(String leads, String account) {
        JSONObject body = new JSONObject();

        // Convert leads into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(leads);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }
        JSONObject leadsObj = (JSONObject)obj;

        Object success = leadsObj.get("success");
        if (success.equals(true)) {
            body.put("action", "updateOnly");
            body.put("lookupField", "id");
            body.put("asyncProcessing", "true");

            // Build array of lead objects
            JSONArray input = new JSONArray();
            JSONArray result = (JSONArray) leadsObj.get("result");
            Iterator<JSONObject> iterator = result.iterator();
            while (iterator.hasNext()) {
                JSONObject leadIn = (JSONObject)iterator.next();
                JSONObject lead = new JSONObject();
                lead.put("id", leadIn.get("id"));
                lead.put("unsubscribed", "true");
                lead.put("unsubscribedReason", "Cross instance synch triggered by webhook from: " + account);
                input.add(lead);
            }

            body.put("input", input);
        }
        return body;
    }

    // HTTP POST request
    private String postData(String endpoint, JSONObject body) {
        String data = "";
        try {
            // Make request
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("POST");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "application/json");
            urlConn.connect();
            OutputStream os = urlConn.getOutputStream();
            os.write(body.toJSONString().getBytes());
            os.close();
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    // HTTP GET request
    private String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

### Configuração do Marketo

Execute as seguintes etapas para cada instância do Marketo que você deseja sincronizar.

1. Criar um Serviço personalizado com permissão de função: lead de leitura-gravação. Se você não estiver familiarizado com a criação de um Serviço Personalizado, clique [aqui](/help/rest-api/custom-services.md).
1. Crie um Webhook que chame o serviço da Web externo. Se você não estiver familiarizado com a criação de Webhooks, clique [aqui](/help/webhooks/webhooks.md).
1. Adicione o Webhook como uma etapa de fluxo no Smart Campaign.

A captura de tela abaixo mostra como criar um webhook para invocar o serviço especificado acima usando tokens para preencher automaticamente os parâmetros de consulta. Agora que criamos nosso webhook, podemos adicioná-lo a uma Campanha inteligente como uma ação de fluxo.  A Smart List deve conter um acionador &quot;Cancelar inscrição no email&quot;.

### Validação

Para testar tudo isso, crie um cliente potencial com o mesmo endereço de email em várias instâncias do Marketo. Certifique-se de que você é o proprietário do endereço de email! Em uma instância, acione uma ação de fluxo de email de envio, abra o email resultante e clique em cancelar inscrição. Para validar os resultados, faça logon em cada uma das outras instâncias e inspecione os registros de cliente potencial associados ao endereço de email. A caixa de seleção &quot;Assinatura cancelada&quot; deve ser marcada, e o campo &quot;Motivo do cancelamento da assinatura&quot; deve conter uma nota com a ID da conta de origem que iniciou a sincronização.

Publicado em _2015-05-11_ por _David_

## Sincronizando alterações de dados de clientes potenciais usando a API REST

Essa publicação apresentou uma amostra de código que poderia ser executada de forma recorrente para pesquisar atualizações no Marketo. A ideia era usar APIs do Marketo para identificar alterações nos dados de clientes potenciais e extrair os dados de clientes potenciais que foram alterados. Esses dados poderiam ser enviados a um sistema externo para fins de sincronização. A amostra de código apresentada usava nossa API SOAP. Bem, temos uma [nova maneira de caminhar](https://www.youtube.com/watch?v=G-7ZJjLy5D8&feature=youtu.be), e essa maneira é usar a [API REST do Marketo](/help/rest-api/rest-api.md). Esta publicação mostra como atingir essa mesma meta usando dois endpoints REST: [Obter Alterações de Lead](/help/rest-api/rest-api.md), [Obter Lead por Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). O programa contém duas etapas principais:

1. Chame Obter Alterações de Cliente Potencial para gerar uma lista de todas as Ids de Cliente Potencial que tiveram campos de cliente potencial específicos alterados ou que foram adicionadas durante um determinado período.
1. Chame Obter ID de lead para cada ID de lead na lista para recuperar dados de campo do registro de lead.

Pegaremos os dados recuperados na etapa 2 e os formataremos para consumo por um sistema externo.  **Entrada de programa** Por padrão, o programa &quot;volta&quot; um dia a partir da data atual para procurar alterações. Assim, você pode executar este programa na mesma hora todos os dias, por exemplo. Para voltar mais no tempo, você pode especificar o número de dias como um argumento de linha de comando, aumentando efetivamente a janela de tempo. O programa contém diversas variáveis que podem ser modificadas: CUSTOM_SERVICE_DATA - Ele contém os dados do [Serviço personalizado](/help/rest-api/custom-services.md) do Marketo (ID da conta, ID do cliente, Segredo do cliente). LEAD_CHANGE_FIELD_FILTER - Contém uma lista separada por vírgulas de campos de clientes potenciais que verificaremos em busca de alterações. READ_BATCH_SIZE - Esse é o número de registros a serem recuperados de cada vez. Use isso para ajustar a resposta ao tamanho do corpo. **Saída do Programa** O programa reúne todos os registros de clientes em potencial alterados e os formata em JSON como uma matriz de objetos de clientes em potencial, da seguinte maneira:

```json
{
    "result": [
        {
            "leadId": "318592",
            "updatedAt": "2015-07-22T19:19:07Z",
            "firstName": "David",
            "lastName": "Everly",
            "email": "<deverly@marketo.com>"
        },

        ...more lead objects here...
    ]
}
```

A ideia é que você possa passar esse JSON como a carga da solicitação para um serviço Web externo para sincronizar os dados. **Lógica do Programa** Primeiro, estabelecemos nossa janela de tempo, compondo nossas URLS de ponto de extremidade REST e obtendo nosso token de acesso de Autenticação. Em seguida, acionamos um loop Obter token de paginação/Obter alterações de lead, no qual o é executado até que o suprimento das alterações de lead seja esgotado. O objetivo deste loop é acumular uma lista de IDs de cliente potencial exclusivas para que possamos transmiti-las para Obter Cliente Potencial por ID posteriormente no programa. Neste exemplo, pedimos a Obter alterações de cliente potencial para procurar alterações nos seguintes campos: firstName, lastName, email. Você pode selecionar qualquer combinação de campos para suas finalidades. Obter alterações de cliente potencial retorna objetos de &quot;resultado&quot; que contêm uma ID de tipo de atividade, que podemos usar para filtrar resultados. Observação: você pode obter uma lista de Tipos de Atividade chamando o ponto de extremidade REST [Obter Tipos de Atividade](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getAllActivityTypesUsingGET). Estamos interessados em 2 tipos de atividades retornadas: 1. Novo lead (12)

```json
{
    "id": 12024682,
    "leadId": 318581,
    "activityDate": "2015-03-17T00:18:41Z",
    "activityTypeId": 12,
    "primaryAttributeValueId": 318581,
    "primaryAttributeValue": "David Everly",
    "attributes": [
        {
            "name": "Created Date",
            "value": "2015-03-16"
        },
        {
            "name": "Source Type",
            "value": "New lead"
        }
    ]
}
```

1. Alterar valor dos dados (13) Podemos saber qual campo foi alterado ao inspecionar a propriedade &quot;name&quot; na resposta Alterar valor dos dados.

```json
{
    "id": 12024689,
    "leadId": 318581,
    "activityDate": "2015-03-17T22:58:18Z",
    "activityTypeId": 13,
    "fields": [
        {
            "id": 31,
            "name": "lastName",
            "newValue": "Evely",
            "oldValue": "Everly"
        }
    ],
    "attributes": [
        {
            "name": "Source",
            "value": "Web form fillout"
        }
    ]
}
```

Quando um desses dois tipos de atividades é retornado, armazenamos a ID do lead associada em uma lista. Assim que tivermos nossa lista, poderemos interagir através dela chamando Obter lead por ID para cada item. Isso recuperará os dados mais recentes de cada cliente potencial na lista. Para este exemplo, recuperamos os seguintes campos de cliente em potencial: `leadId`, `updatedAt`, `firstName`, `lastName` e `email`. Você pode selecionar qualquer combinação de campos para suas finalidades. Isso é feito especificando o parâmetro dos campos para Obter lead por ID. E, finalmente, JSONificamos os resultados como uma matriz de objetos principais, conforme descrito acima.

**Código do Programa**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadChanges {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            {INSERT YOUR CUSTOM SERVICE DATA HERE};

    // Lead fields that we are interested in
    private static final String LEAD_CHANGE_FIELD_FILTER = "firstName,lastName,email";

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Activity type ids that we are interested in
    private static final int ACTIVITY_TYPE_ID_NEW_LEAD = 12;
    private static final int ACTIVITY_TYPE_ID_CHANGE_DATA_VALE = 13;

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String leadChangesUrl = String.format("%s/rest/v1/activities/leadchanges.json?access_token=%s&fields=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_CHANGE_FIELD_FILTER, READ_BATCH_SIZE);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        HashSet leadIdList = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {

            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;

            do {
                moreResult = false;

                // Call Get Lead Changes API
                JsonObject leadChangesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadChangesUrl, nextPageToken)));
                if (leadChangesObj.get("success").asBoolean()) {
                    moreResult = leadChangesObj.get("moreResult").asBoolean();
                    nextPageToken = leadChangesObj.get("nextPageToken").asString();

                    if (leadChangesObj.get("result") != null) {
                        JsonArray resultAry = leadChangesObj.get("result").asArray();
                        for (JsonValue resultObj : resultAry) {
                            int activityTypeId = resultObj.asObject().get("activityTypeId").asInt();


                            // Store lead ids for later use
                            boolean storeThisId = false;
                            if (activityTypeId == ACTIVITY_TYPE_ID_NEW_LEAD) {
                                storeThisId = true;
                            } else if (activityTypeId == ACTIVITY_TYPE_ID_CHANGE_DATA_VALE) {
                                // See if any of the changed fields are of interest to us
                                JsonArray fieldsAry = resultObj.asObject().get("fields").asArray();
                                for (JsonValue fieldsObj : fieldsAry) {
                                    String name = fieldsObj.asObject().get("name").asString();
                                    if (LEAD_CHANGE_FIELD_FILTER.contains(name)) {
                                        storeThisId = true;
                                    }
                                }

                            }

                            if (storeThisId) {
                                leadIdList.add(resultObj.asObject().get("leadId").toString());
                            }
                        }
                    }
                }

            } while (moreResult);
        }

        JsonObject result = new JsonObject();
        JsonArray leads = new JsonArray();

        for (Object o : leadIdList) {
            String leadId = o.toString();

            // Compose Get Lead by Id URL
            String getLeadUrl = String.format("%s/rest/v1/lead/%s.json?access_token=%s",
                    baseUrl, leadId, accessToken);

            // Call Get Lead by Id API
            JsonObject leadObj = JsonObject.readFrom(getData(getLeadUrl));
            if (leadObj.get("success").asBoolean()) {
                if (leadObj.get("result") != null) {
                    JsonArray resultAry = leadObj.get("result").asArray();
                    for (JsonValue resultObj : resultAry) {

                        // Create lead object
                        JsonObject lead = new JsonObject();
                        lead.add("leadId", leadId);
                        lead.add("updatedAt", resultObj.asObject().get("updatedAt").asString());
                        lead.add("firstName", resultObj.asObject().get("firstName").asString());
                        lead.add("lastName", resultObj.asObject().get("lastName").asString());
                        lead.add("email", resultObj.asObject().get("email").asString());

                        // Add lead object to leads array
                        leads.add(lead);
                    }
                }
            }
        }

        // Add leads array to result object
        result.add("result", leads);

        // Print out result object
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Então é isso, [a vida é uma música feliz](https://www.youtube.com/watch?v=zFaBwZDywLk). Aproveite!

Publicado em _2015-07-31_ por _David_

## Atualizações da versão de maio de 2015

### API REST

* API de oportunidade. Foram introduzidos novos [endpoints de oportunidade](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities) que permitem listar, descrever e CRUD programaticamente os dados que residem em um objeto de oportunidade do Marketo.

Observação: as permissões de função foram adicionadas para fornecer acesso aos pontos de extremidade da Oportunidade: Oportunidade Somente Leitura, Oportunidade Leitura-Gravação. Se sua função de usuário da API for anterior ao lançamento das APIs de oportunidade, será necessário criar uma nova função de usuário da API com essas permissões para habilitar o acesso. Caso contrário, você receberá uma resposta de erro 603 &quot;Acesso negado&quot;.

* API de ativos - Fragmentos. Novos [pontos de extremidade de ativos para trechos](https://developer.adobe.com/marketo-apis/api/asset/#snippet_endpoints) foram introduzidos para permitir que você manipule programaticamente objetos de trecho. Os trechos podem ser usados como blocos de conteúdo dinâmico em emails e landing pages.
* API de clientes potenciais - Atualizar partição de clientes potenciais. Um novo [ponto de extremidade de cliente potencial para partições](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updatePartitionsUsingPOST) foi adicionado para permitir que você atualize a partição para um ou mais clientes potenciais.
* Correção de um problema em que as APIs relacionadas ao lead não tinham deslocamento de fuso horário nos atributos &quot;createdAt&quot; e &quot;updatedAt&quot;.
* Correção de um problema em que o Schedule Campaign não retornava o código de erro correto quando o número máximo diário de chamadas era excedido.
* Correção de um problema em que Obter pasta por ID às vezes retornava nulo para atributos &quot;pai&quot; e &quot;descrição&quot;.
* Correção de um problema em que Aprovar email por ID gerava um erro de sistema em determinados casos.
* Correção de um problema em que Criar token por ID de pasta produziria um token que não podia ser usado em determinados casos.

### Real-Time Personalization (RTP)

* API de recomendação de mídia avançada. Novos recursos de [recomendação de mídia avançada](/help/javascript-api/web-personalization.md) foram adicionados à API RTP JavaScript. A Recomendação de conteúdo de mídia avançada envolve os visitantes da Web com o conteúdo mais relevante viabilizado pelo aprendizado de máquina e pela análise preditiva. Aprimore seus ativos de conteúdo com descrições de texto e imagens e incorpore várias recomendações de conteúdo ao seu site.

### SDK do Engajamento Móvel

iOS v0.3.4/Android v0.3.3

* Ações personalizadas. Adição da capacidade de rastrear a interação do usuário enviando ações personalizadas. Para obter detalhes, consulte &quot;Enviando ações personalizadas&quot; [aqui](/help/mobile/mobile.md).
* O método trackPushNotification foi descontinuado.

Publicado em _2015-05-26_ por _David_

## Atualizações da versão de junho de 2015

### API REST

* API da empresa

Foram introduzidos novos [pontos de extremidade de empresa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies) que permitem listar, descrever e ATUALIZAR programaticamente os dados que residem em um objeto de empresa do Marketo.

Observação: as permissões de função foram adicionadas para fornecer acesso aos endpoints do Programa: Empresa Somente Leitura, Empresa de Leitura e Gravação. Se sua função de usuário da API for anterior ao lançamento das APIs da empresa, será necessário atualizar sua função de usuário da API com essas permissões para habilitar o acesso. Caso contrário, você receberá uma resposta de erro 603 &quot;Acesso negado&quot;

* API de ativos - Programas

Atualização da API de ativos para oferecer suporte às pastas de aplicativos e de programas. Várias APIs de ativos aceitaram uma ID de pasta na solicitação na forma de um número inteiro. A ID da pasta pode ser passada como parte do URI ou parte do corpo da solicitação, dependendo da API.

Agora, você deve especificar o tipo de pasta junto com a id da pasta. O tipo de pasta é &quot;Programa&quot; ou &quot;Pasta&quot;. &quot;Folder&quot; especifica uma pasta de aplicativo, enquanto &quot;Program&quot; especifica uma pasta de programa. O tipo de pasta é especificado de duas maneiras diferentes, dependendo da API que você está chamando:

1. Use a estrutura de dados &quot;FolderIdType&quot;. FolderIdType é um bloco JSON simples contendo a id e o par de tipos da seguinte maneira:

`{ "id" : **id**, "type" : "**type**" }`

Onde **id** é a ID da pasta e &quot;**type**&quot; é o tipo de pasta. Os valores permitidos para &quot;**tipo**&quot; são &quot;Pasta&quot; ou &quot;Programa&quot;.

Exemplo - Criar pasta

`POST /asset/v1/folders.json`

`parent={"id":416,"type":"Folder"}&name=Test Folder&description=This is a test folder`

1. Use o parâmetro de id existente e especifique o tipo usando um parâmetro de consulta &quot;type&quot;.

Exemplo - Obter pasta por ID

`GET /rest/asset/v1/folder/1016.json?type=Program`

Todas as respostas da API que continham uma ID de pasta no objeto Resultado agora também conterão um atributo folderId cujo valor é um FolderIdType. Isso pode ser usado para determinar o tipo de pasta para uma determinada ID de pasta.

Exemplo - Obter pasta por nome

`GET /rest/asset/v1/folder/byName.json?name=Social Media`

```json
"result": [
{
...

"folderId": {"id":341, "type": "Program"},
...

"id":"341"
}
]
```

Para determinar o tipo de pasta para uma determinada ID, você pode usar a API [Procurar Pastas](https://developer.adobe.com/marketo-apis/api/asset/browse-folders/).

O atributo &quot;type&quot; nas respostas da API foi renomeado para &quot;folderType&quot;. Isso evita confusão com o elemento &quot;type&quot; contido em FolderIdType.

Por exemplo, deste:

&quot;**type**&quot;:&quot;Pasta de marketing&quot;

para isto:

&quot;**folderType**&quot;: &quot;Pasta de Marketing&quot;

### SDK do Engajamento Móvel

iOS 0.3.5

* Correção de um problema em que a caixa de diálogo Definir dispositivo de teste estava em execução no thread principal. [MOB-638]
* Adição do tratamento de erros em caso de falha ao registrar o dispositivo de teste. [MOB-639]

Android 0.3.3

* Adição do atributo android:configChanges ao elemento `<activity>` AndroidManifest.xml para impedir que a caixa de diálogo de progresso seja descartada quando você adiciona um dispositivo de teste e altera a orientação. [MOB-687]

Publicado em _2015-06-30_ por _David_

## Personalization da Web no aplicativo (Beta) usando a API RTP

Vários de nossos clientes fornecem soluções de aplicativo Web para seus usuários e recebemos solicitações se eles puderem usar o Marketo Real-Time Personalization (RTP) em seu ambiente de aplicativo Web seguro. A resposta é sim! Lançamos uma API para mensagens no aplicativo, para que você possa personalizar o conteúdo e promover atividades de marketing, como webinários, novos lançamentos de recursos, vendas adicionais e envolver seus usuários com base nos dados do aplicativo web. Por exemplo, personalizar o conteúdo no aplicativo para:

* Ofertas de avaliação com base na atividade do usuário
* Tipos de assinatura diferentes (treinamento de venda adicional, venda cruzada ou webinário)
* Novos recursos relevantes para a atividade do usuário

**Exemplo de caso de uso** A equipe de sucesso do cliente da Marketo usa o Personalization no aplicativo para se comunicar com tipos de assinatura específicos (Spark, Standard, Select ou Enterprise) com conteúdo personalizado, certificando-se de que esteja vendo campanhas progressivas e incentivando usuários no aplicativo com base em seu envolvimento. Vamos ver como isso pode ser feito para um usuário com um tipo de assinatura Enterprise. **Pré-requisito** Entenda a [API de Contexto de Usuário RTP](/help/javascript-api/web-personalization.md). **Habilite a Solicitação da API de Contexto de Usuário** do Suporte da Marketo para habilitar a API de Contexto de Usuário para sua conta RTP. **Definir a Variável Personalizada** Há 5 slots de variáveis personalizadas disponíveis no RTP para os quais enviar dados. Neste exemplo, enviaremos uma assinatura de usuário do tipo Enterprise para a Variável personalizada 1.

`rtp('set', 'customVar1', 'Enterprise');`

**Criar um Novo Segmento RTP** Vá para **Segmentos**, clique em **CRIAR NOVO**.

1. Arraste o filtro **API de Contexto de Usuário** para o construtor de segmentos.
1. Selecione a **Variável Personalizada 1** (Tipo de Assinatura) cujo **valor** é **Empresa**.

**Mostrar campanha com base no histórico de visitas anterior** Para mostrar ao visitante outra campanha se ele já clicou em uma campanha em uma visita anterior.

1. Na **API de Contexto de Usuário**, clique em **(+)** para adicionar outro atributo da API de Contexto de Usuário
1. Adicionar o operador **(AND ou OR).**
1. Selecionar **Campanhas - Clicado.** Defina **ID da Campanha** para a ID da campanha. (Consulte a nota abaixo sobre como encontrar a ID da campanha.)
1. Clique em **SALVAR E DEFINIR A CAMPANHA** para criar o criativo da campanha.

Em geral, esse segmento corresponderá se um visitante estiver associado à variável personalizada (tipo de assinatura) igual a Enterprise e se clicou na campanha (ID: 5390) em uma visita anterior. A próxima etapa é definir uma campanha personalizada para esse segmento. A captura de tela abaixo mostra uma campanha de diálogo RTP (canto inferior esquerdo) que aparece na página Meu Marketo promovendo um webinário para usuários corporativos.  **OBSERVAÇÃO:** **Localizando a ID da Campanha** Vá para **Campanhas**, passe o mouse sobre o **Nome da Campanha** para localizar a ID da Campanha.
Publicado em _2015-06-17_ por _David_

## Envio de emails transacionais com a API REST do Marketo: Parte 1

Há alguns requisitos de configuração no Marketo para executar a chamada necessária com a API REST do Marketo.

* O recipient deve ter um registro no Marketo
* É necessário ter um email transacional criado e aprovado na instância do Marketo.
* É necessário ter uma campanha de acionador ativa com a Campanha Solicitada, Source: API de serviço da Web, configurada para enviar o email

Primeiro [crie e aprove seu email](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR). Se o email for realmente transacional, você provavelmente precisará configurá-lo como operacional, mas certifique-se de que ele se qualifique legalmente como operacional. Essa configuração é feita no com a tela Editar em Ações de email > Configurações de email. Aprove-a e estamos prontos para criar nossa campanha. Se você nunca criou campanhas, confira o artigo [Criar uma nova campanha inteligente](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign) em docs.marketo.com. Depois de criar sua campanha, precisamos executar essas etapas. Configure sua Smart List com o acionador Campaign is Requested: agora precisamos configurar o fluxo para apontar uma etapa Enviar email para nosso email. Antes da ativação, você precisa decidir sobre algumas configurações na guia Schedule. Se esse email específico precisar ser enviado apenas uma vez para um determinado registro, deixe as configurações de qualificação como estão. No entanto, se for necessário que eles recebam o email várias vezes, ajuste isso para cada vez ou para uma das cadências disponíveis. Agora estamos prontos para ativar.

### Envio de chamadas de API

**Observação:** nos exemplos de Java abaixo, estamos usando o pacote minimal-json para manipular representações JSON em nosso código. Você pode ler mais sobre este projeto aqui: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) A primeira parte do envio de um email transacional por meio da API é garantir que exista um registro com o endereço de email correspondente na sua instância do Marketo e que tenhamos acesso à sua ID de cliente potencial. Para os fins desta publicação, assumiremos que os endereços de email já estão no Marketo e só precisamos recuperar a ID do registro. Para isso, estamos usando a chamada [Obter vários clientes em potencial por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) e estamos reutilizando alguns dos códigos Java da publicação anterior em. Vamos analisar nosso Método principal para solicitar a campanha:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("<requestCampaign.test@marketo.com>");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Para chegar a esses resultados da resposta JsonObject de leadsRequest, precisamos escrever algum código. Para recuperar o primeiro resultado na matriz, precisamos extrair a matriz do JsonObject e obter o objeto indexado em 0:

`JsonArray leadsResult = leadsRequest.getData().get("result").asArray();`
`int leadId = leadsResult.get(0).asObject().get("id").asInt();`

A partir daqui, tudo o que precisamos fazer é a chamada Solicitar campanha. Para isso, os parâmetros necessários são a ID no URL da solicitação e uma matriz de objetos JSON que contém um membro, &quot;id&quot;. Vamos analisar o código disso:

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
 private String endpoint;
 private Auth auth;
 public ArrayList leads = new ArrayList();
 public ArrayList tokens = new ArrayList();

 public RequestCampaign(Auth auth, int campaignId) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
 }
 public RequestCampaign setLeads(ArrayList leads) {
  this.leads = leads;
  return this;
 }
 public RequestCampaign addLead(int lead){
  leads.add(lead);
  return this;
 }
 public RequestCampaign setTokens(ArrayList tokens) {
  this.tokens = tokens;
  return this;
 }
 public RequestCampaign addToken(String tokenKey, String val){
  JsonObject jo = new JsonObject().add("name", tokenKey);
  jo.add("value", val);
  tokens.add(jo);
  return this;
 }
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   System.out.println("Executing RequestCampaign calln" + "Endpoint: " + s + "nRequest Body:n"  + requestBody);
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   System.out.println("Result:n" + result);
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonObject input = new JsonObject();
  JsonArray leadsArray = new JsonArray();
  for (int lead : leads) {
   JsonObject jo = new JsonObject().add("id", lead);
   leadsArray.add(jo);
  }
  input.add("leads", leadsArray);
  JsonArray tokensArray = new JsonArray();
  for (JsonObject jo : tokens) {
   tokensArray.add(jo);
  }
  input.add("tokens", tokensArray);
  requestBody.add("input", input);
  return requestBody;
 }

}
```

Essa classe tem um construtor que aceita um Auth e a Id da campanha. Os clientes em potencial são adicionados ao objeto transmitindo uma ArrayList `<Integer>` contendo as Ids dos registros para setLeads ou usando addLead, que pega um inteiro e o anexa à ArrayList existente na propriedade de clientes em potencial. Para acionar a chamada de API para transmitir os registros de lead para a campanha, postData precisa ser chamado, o que retorna um JsonObject que contém os dados de resposta da solicitação. Quando uma campanha de solicitação é chamada, cada lead transmitido para a chamada será processado pela campanha do acionador do target no Marketo e ser é enviado para o email que foi criado anteriormente. Parabéns, você acionou um email por meio da API REST do Marketo. Fique atento à Parte 2, onde veremos a personalização dinâmica do conteúdo de um email por meio da Campanha de solicitação.

Publicado em _2015-07-17_ por _Kenny_

## Autenticação e recuperação de dados de clientes potenciais do Marketo com a API REST

**Observação:** nos exemplos de Java abaixo, estamos usando o pacote minimal-json para manipular representações JSON em nosso código. Você pode ler mais sobre este projeto aqui: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) Um dos requisitos mais comuns ao integrar com o Marketo é a recuperação de dados de clientes potenciais. A maioria, se não todas as integrações, exigirá a recuperação ou o envio de dados de cliente potencial de uma assinatura do Marketo. Portanto, hoje vamos dar uma olhada em uma tarefa básica de recuperação de informações de cliente potencial, [autenticando](/help/rest-api/authentication.md) com uma assinatura e recuperando dados de cliente potencial dela. Para recuperar nossos leads, primeiro precisamos autenticar com a instância do Marketo de destino usando OAuth 2.0. Há três informações que precisamos autenticar com o Marketo, a ID do cliente, o segredo do cliente e o host da instância do Marketo. Esta é a classe que estamos usando para autenticar:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Auth {
 protected String marketoInstance; //Instance Host obtained from Admin -> Web Service
 private String clientId; //clientId obtained from Admin -> Launchpoint -> View Details for selected service
 private String clientSecret; //clientSecret obtained from Admin -> Launchpoint -> View Details for selected service
 private String idEndpoint; //idEndpoint constructed to authenticate with service when constructor is used
 private String token; //token is stored for reuse until expiration
 private Long expiry; //used to store time of expiration

 //Creates an instance of Auth which is used to Authenticate with a particular service on a particular instance
 public Auth(String id, String secret, String instanceUrl) {
  this.clientId = id;
  this.clientSecret = secret;
  this.marketoInstance = instanceUrl;
  this.idEndpoint = marketoInstance + "/identity/oauth/token?grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
 }
 //The only public method of Auth, used to check if the current value of Token is valid, and then to retrieve a new one if it is not
 public String getToken(){
  long now  = System.currentTimeMillis();
  if (expiry == null || expiry < now){
   System.out.println("Token is empty or expired. Trying new authentication");
   JsonObject jo = getData();
   System.out.println("Got Authentication Response: " + jo);
   this.token = jo.get("access_token").asString();
                        //expires_in is reported as seconds, set expiry to system time in ms + expires \* 1000
   this.expiry = System.currentTimeMillis() + (jo.get("expires_in").asLong() \* 1000);
  }
  return this.token;
 }
 //Executes the authentication request
 private JsonObject getData(){
  JsonObject jsonObject = null;
  try {
   URL url = new URL(idEndpoint);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
   urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "application/json");
            System.out.println("Trying to authenticate with " + idEndpoint);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                Reader reader = new InputStreamReader(inStream);
                jsonObject = JsonObject.readFrom(reader);
            }else {
                throw new IOException("Status: " + responseCode);
            }
  } catch (MalformedURLException e) {
   e.printStackTrace();
  }catch (IOException e) {
            e.printStackTrace();
        }
  return jsonObject;
 }
}
```

Esse código permite criar uma instância de Autenticação com nossa ID de cliente, segredo de cliente e host (como marketoInstance) em Admin -> Launchpoint (ID e Segredo) e Admin -> Serviços da Web (Host). Ele expõe o método getToken, que testa se o token armazenado no momento é nulo ou expirou e retorna o token existente, ou faz uma nova solicitação de autenticação e, em seguida, retorna o novo token do membro &quot;access_token&quot; da resposta JSON. Agora que você pode autenticar com sua instância do Marketo, o próximo passo é recuperar nossos clientes em potencial. Estamos usando esta classe:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Leads {
 private StringBuilder endpoint;
 private Auth auth;
 public String filterType;
 public ArrayList filterValues = new ArrayList();
 public Integer batchSize;
 public String nextPageToken;
 public ArrayList fields = new ArrayList();

 public Leads(Auth a) {
  this.auth = a;
  this.endpoint = new StringBuilder(this.auth.marketoInstance + "/rest/v1/leads.json");
 }
 public Leads setFilterType(String filterType) {
  this.filterType = filterType;
  return this;
 }
 public Leads setFilterValues(ArrayList filterValues) {
  this.filterValues = filterValues;
  return this;
 }
 public Leads addFilterValue(String filterVal) {
  this.filterValues.add(filterVal);
  return this;
 }
 public Leads setBatchSize(Integer batchSize) {
  this.batchSize = batchSize;
  return this;
 }
 public Leads setNextPageToken(String nextPageToken) {
  this.nextPageToken = nextPageToken;
  return this;
 }
 public Leads setFields(ArrayList fields) {
  this.fields = fields;
  return this;
 }

 public JsonObject getData() {
        JsonObject result = null;
        try {
         endpoint.append("?access_token=" + auth.getToken() + "&filterType=" + filterType + "&filterValues=" + csvString(filterValues));
         if (batchSize != null && batchSize > 0 && batchSize <= 300){
             endpoint.append("&batchSize=" + batchSize);
            }
            if (nextPageToken != null){
             endpoint.append("&nextPageToken=" + nextPageToken);
            }
            if (fields != null){
             endpoint.append("&fields=" + csvString(fields));
            }
            URL url = new URL(endpoint.toString());
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "text/json");
            InputStream inStream = urlConn.getInputStream();
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader);
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return result;
    }
 private String csvString(ArrayList fields) {
  StringBuilder fieldCsv = new StringBuilder();
     for (int i = 0; i < fields.size(); i++){
      fieldCsv.append(fields.get(i));
      if (i + 1 != fields.size()){
       fieldCsv.append(",");
      }
     }
  return fieldCsv.toString();
 }
}
```

Essa classe tem um único construtor que aceita um objeto Auth e, em seguida, expõe vários setters para os parâmetros opcionais e obrigatórios. Nesse caso, realmente só precisamos nos preocupar com a definição do filterType e filterValues para executar a obtenção de leads pelo endereço de email. Portanto, usaremos setFilterType como &quot;email&quot; e addFilterValue como o endereço de email para o qual precisamos recuperar uma ID. Ao definir seus parâmetros, você pode usar o método getData para recuperar um JsonObject do endpoint de leads, contendo uma matriz de resultados que tem uma representação JSON dos registros de lead que foram recuperados.

### Tudo junto na prática

Agora que já vimos o código de exemplo que estamos usando, vamos dar uma olhada em um exemplo simples para recuperar clientes em potencial que correspondam a um endereço de email de teste, <testlead@marketo.com>. Para isso, precisamos usar setFilterType para &quot;email&quot; e addFilterValue para o endereço de email para o qual precisamos recuperar informações. Ao definir seus parâmetros, você pode usar o método getData para recuperar um JsonObject do endpoint de leads, contendo uma matriz de resultados que tem uma representação JSON dos registros de lead que foram recuperados.

```java
package dev.marketo.blog_leads;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        //Change the credentials here to your own
        Auth auth = new Auth("CHANGE ME", "CHANGE ME", "CHANGE ME");
        //Create and parameterize an instance of Leads
        Leads getLeads = new Leads(auth)
              .setFilterType("email")
              .addFilterValue("<testlead@marketo.com>");
        //get the inner results array of the response
        JsonObject leadsResult = getLeads.getData();
        System.out.println(leadsResult);
    }
}
```

Neste exemplo de método principal, criamos uma instância de Auth e a passamos para um novo construtor Leads. Usando setFilterType e addFilterValue, configuramos nossa instância de clientes em potencial para recuperar apenas os clientes em potencial que correspondam ao endereço de email &quot;<testlead@marketo.com>&quot;. Este exemplo imprime isso no console:

O token está vazio ou expirou. Tentando nova autenticação
Tentando autenticar com <https://299-BYM-827.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id=b417d98f-9289-47d1-a61f-db141bf0267f&client_secret=0DipOvz4h2wP1ANeVjlfwMvECJpo0ZYc>
Resposta de autenticação obtida: {&quot;access_token&quot;:&quot;ec0f02c0-28ac-4d6c-b7d7-00e47ae85ff1:st&quot;,&quot;token_type&quot;:&quot;bearer&quot;,&quot;expires_in&quot;:538,&quot;scope&quot;:&quot;<apiuser@mktosupport.com>&quot;}
&lbrace;&quot;requestId&quot;:&quot;14fb6#14e6a7a9ad6&quot;,&quot;result&quot;:[{&quot;id&quot;:1026322,&quot;updatedAt&quot;:&quot;2015-07-07T21:43:25Z&quot;,&quot;lastName&quot;:&quot;Lead&quot;,&quot;email&quot;:&quot;<testlead@marketo.com>&quot;,&quot;createdAt&quot;:&quot;2015-07-07T21:43:25Z&quot;,&quot;firstName&quot;&quot;:&quot;Test&quot;},{&quot;id&quot;:1026323,&quot;updatedAt&quot;:&quot;2015-07-07T21:43:43Z&quot;,&quot;lastName&quot;:&quot;Lead2&quot;,&quot;email&quot;:&quot;<testlead@marketo.com>&quot;,&quot;createdAt&quot;:&quot;2015-07-07T21:43:43Z&quot;,&quot;firstName&quot;:&quot;Test&quot;}],&quot;success&quot;:true

Agora temos os dados principais que podemos processar de qualquer forma que precisarmos. Obrigado por ler. Deixe seus comentários.

Publicado em _2015-07-10_ por _Kenny_

## Atualizações da versão de julho de 2015

API REST

* API do Vendedor

Foram introduzidos novos [pontos de extremidade de vendedor](/help/rest-api/sales-persons.md) que permitem listar, descrever e ATUALIZAR programaticamente os dados que residem em um objeto de vendedor do Marketo. Além disso, um vendedor pode ser atribuído a um cliente potencial, oportunidade ou empresa. Isso é feito especificando um atributo &quot;externalSalesPersonId&quot; ao chamar o endpoint Criar/Atualizar/Substituir para lead, oportunidade ou empresa.

Observação: as permissões de função foram adicionadas para fornecer acesso aos endpoints do Programa: Vendedor Somente Leitura, Vendedor Somente Leitura e Vendedor Somente Leitura. Se sua função de usuário API for anterior ao lançamento das APIs de Vendedor, será necessário atualizar sua função de usuário API com essas permissões para habilitar o acesso. Caso contrário, você receberá uma resposta de erro 603 &quot;Acesso negado&quot;.

* API de ativo - Modelo da página de aterrissagem

Foram introduzidos novos [pontos de extremidade de modelo de página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#landing_page_templates_endpoints), que permitem listar, criar e atualizar programaticamente os dados associados a um modelo de página de aterrissagem.

* API de ativos - Segmentos

Dois endpoints relacionados ao segmento foram introduzidos:

[Obter segmentos](https://developer.adobe.com/marketo-apis/api/asset/get-segments)

[Obter Segmentação por ID](https://developer.adobe.com/marketo-apis/api/asset/get-segmentation-by-id)

* Correção de um problema em que Obter pasta por nome não respeitava o parâmetro &quot;workSpace&quot;. [LM-61059]
* Foram feitos vários aprimoramentos de desempenho nas APIs de objeto personalizado.

Publicado em _2015-07-17_ por _David_

## Criar e associar clientes em potencial, empresas e oportunidades à API REST do Marketo

Para aproveitar ao máximo o Marketo Analytics, é fundamental criar associações corretas e robustas entre os registros de cliente potencial, empresa e oportunidade. Quando você não está aproveitando uma sincronização de CRM nativa, a criação dessas relações pode apresentar algumas dificuldades, portanto, hoje analisamos a sua criação.

### Relacionamentos de objeto

No Marketo, há algumas relações vitais para estabelecer completamente os relatórios de oportunidades:

* Clientes potenciais e oportunidades têm um relacionamento muitos para muitos com o objeto OpportunityRole.
* OpportunityRole tem um campo leadId e um campo externalopportunityId para criar o relacionamento entre o lead e a oportunidade.
* Para se qualificar para um filtro de lista inteligente Tem Oportunidade, um cliente potencial deve ter uma OpportunityRole relacionada a uma oportunidade.
* As oportunidades têm uma relação muitos para um com o objeto Company por meio do campo externalCompanyId.
* Os clientes potenciais têm uma relação um para muitos com Empresas por meio do campo externalCompanyId.
* As oportunidades são atribuídas a um programa com base no Programa de Aquisição de um cliente potencial ou em sua associação e sucesso em um programa (Consulte [Noções básicas sobre atribuição](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/reporting/revenue-cycle-analytics/revenue-tools/attribution/understanding-attribution)).

A criação dessas relações no banco de dados de clientes potenciais permitirá que você aproveite totalmente o Marketo Analytics e veja a influência que seus programas têm na criação de oportunidades e nas taxas de ganhos.

### Empresas

A maneira mais simples de construir esses relacionamentos é começar com a criação da empresa. Isso garante que possamos transmitir externalCompanyId para nossas oportunidades durante a criação, em vez de precisarmos executar chamadas de API adicionais para atualizar oportunidades após sua criação. Dependendo da configuração existente, essa pode ser ou não uma etapa necessária, mas novos clientes em potencial e contatos com empresas associadas precisam adicionar esses registros à sua instância do Marketo para que os relacionamentos sejam criados. Portanto, vamos analisar alguns códigos para criar registros de empresas.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertCompanies {
 public List<JsonObject> input; //a list of Companies to use for input.  Each must have a member "externalCompanyId".
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters(externalCompanyId), idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertCompanies(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/companies.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertCompanies(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 //adds input to existing list, creates arraylist if it was built without a list
 public UpsertCompanies addCompanies(JsonObject... companies){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : companies) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our company records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertCompanies instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 //sets or replaces existing input with list
 public UpsertCompanies setInput(List input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertCompanies setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertCompanies setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

A API das empresas fornece duas opções de desduplicação, definidas pelo parâmetro dedupeBy na solicitação, &quot;dedupeFields&quot; e &quot;idField&quot;. Elas podem ser recuperadas explicitamente ao chamar Descrever empresas. Se dedupeBy não estiver definido, o padrão será dedupeFields. No caso de registros de empresas, dedupeFields sempre corresponde a &quot;externalCompanyId&quot;, que é uma cadeia de caracteres arbitrária definida por uma fonte externa, e idField, correspondente ao campo &quot;marketoId&quot;, que é um número inteiro gerado e retornado pelo Marketo após a criação. Dependendo da seleção de dedupeBy, um de externalCompanyId ou marketoId deve ser incluído em qualquer chamada de substituição para um registro de empresa. Esses mesmos requisitos se aplicam às APIs de objeto de Oportunidade e Função de Oportunidade. Nosso código expõe dois construtores: um que aceita um único argumento de um objeto Auth e outro que aceita Auth e uma lista de [JsonObject](https://github.com/ralfstx/minimal-json) registros de empresa. Se construído sem uma Lista de entrada, os registros da empresa devem ser adicionados por meio do método addCompanies, que verificará a criação de uma nova ArrayList se a entrada for nula, e adicionará todos os argumentos JsonObject à Lista de entrada. Veja um exemplo de uso:

```java
//Create a new company to associate to
JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
JsonObject companiesResult = upsertCompanies.postData();
System.out.println(companiesResult);
```

Estamos criando uma única empresa JsonObject com apenas um campo, `externalCompanyId`, em seguida, construindo uma instância de UpsertCompanies e adicionando nossa empresa à lista de entrada com `addCompanies`.

### Oportunidades

Semelhante aos objetos da empresa, a API de oportunidade tem um parâmetro `dedupeBy`, aceitando &quot;dedupeFields&quot; ou &quot;idField&quot;, correspondentes a &quot;externaloportunityid&quot; e &quot;marketoGUID&quot;, respectivamente. Então, aqui está nosso código, que se parece bastante com a classe UpsertCompanies:

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunities {
 public List<JsonObject> input; //a list of Opportunities to use for input.  Each must have a member "externalopportunityid".  Each can optionally include "externalCompanyId" for company association
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertOpportunities(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunities(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunities addOpportunities(JsonObject... opp){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : opp) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our Opportunity records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunities setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunities setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunities setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

As mesmas opções de construtor são fornecidas, utilizando um método Auth ou `Auth+List<JsonObject>` e um método `addOpportunities` para inserir oportunidades JsonObject. Este é um exemplo de uso:

```java
//Create some JsonObjects for Opportunity Data
JsonObject opp1 = new JsonObject().add("name", "opportunity1")
    .add("externalopportunityid", "Opportunity1Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");
JsonObject opp2 = new JsonObject().add("name", "opportunity2")
    .add("externalopportunityid", "Opportunity2Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");

//Create an Instance of UpsertOpportunities and POST it
UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                        .setAction("createOnly")
                        .addOpportunities(opp1, opp2);
JsonObject oppsResult = upsertOpps.postData();
System.out.println(oppsResult);
```

Aqui, estamos criando dois exemplos de oportunidades e, em seguida, dando a elas valores para os campos name, externaloportunityid, externalCompanyId e externalCreatedDate. Ainda não discutimos a externalCreatedDate, mas é importante utilizar o, pois é tratado como o campo principal no RCE para quando uma oportunidade foi criada, tornando-a importante para a atribuição correta. Você pode usar a lógica de negócios da organização para determinar o que você insere nesse campo, com base no preenchimento retroativo de dados de oportunidade existentes ou na criação de novos dados em tempo real. Criamos nossa instância de UpsertOpportunity e, em seguida, adicionamos nossos JsonObjects por meio de addOpportunity. Agora que a instância está configurada, você pode enviar isso para o Marketo com postData e imprimir seu resultado

### Funções

As funções são bastante semelhantes aos dois objetos anteriores, exceto que têm um requisito um pouco diferente ao definir dedupeBy para dedupeFields. As funções exigem que três campos sejam incluídos ao criar ou atualizar um registro por meio deste método, &quot;leadId&quot;, &quot;role&quot; e &quot;externaloportunityid&quot;. &quot;role&quot; pode ser qualquer valor em sequência de caracteres, mas os outros dois devem se referir a uma ID válida de um cliente potencial e a uma ID válida de uma oportunidade, respectivamente.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunityRoles {
 public List<JsonObject> input; //Array of Opportunity Roles as JsonObjects, must have "leadId", "role" and "externalopprtunityid"
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate, defaults to createOrUpdate if unset
 public String dedupeBy;//select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object

 //Constructs an UpsertOpportunityRoles with Auth, but with no input set
 public UpsertOpportunityRoles(Auth auth) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities/roles.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunityRoles(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunityRoles addRoles(JsonObject... role){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : role) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 //executes the request to Marketo, body will be empty if input is not set
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");//"application/json" content-type is required.
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream();  //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 public JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject();
  JsonArray in = new JsonArray();
  for (JsonObject jo : input) {
   in.add(jo);
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action);
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy);
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunityRoles setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunityRoles setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunityRoles setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Estamos seguindo um padrão semelhante para os construtores e o método addRoles como nos exemplos anteriores. Aqui está um exemplo.

```java
//Create Some opp roles now
JsonObject opp1Role = new JsonObject()
                        .add("role", "Captain")
                        .add("externalopportunityid", opp1.get("externalopportunityid").asString())
                        .add("leadId", 318794);
JsonObject opp2Role = new JsonObject()
                        .add("role", "Commander")
                        .add("externalopportunityid", opp2.get("externalopportunityid").asString())
                        .add("leadId", 318795);

//Create an Instance of UpsertOpportunityRoles and POST it
UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
                        .setAction("createOnly")
                        .addRoles(opp1Role, opp2Role);
JsonObject rolesResult = upsertRoles.postData();
System.out.println(rolesResult);
```

Aqui, estamos criando os novos JsonObjects para nossas 2 funções de exemplo e adicionando seus dedupeFields necessários, extraindo a oportunityid externa das oportunidades que já criamos e, em seguida, enviando-os para o Marketo.

### Tudo junto na prática

Este é o exemplo completo do nosso método principal:

```java
package dev.marketo.opportunities;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //create an Instance of Auth
        Auth auth = new Auth("CLIENT_ID_CHANGE_ME", "CLIENT_SECRET_CHANGE_ME", "MARKETO_HOST_CHANGE_ME");

        //Create a new company to associate to
        JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
        UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
        JsonObject companiesResult = upsertCompanies.postData();
        System.out.println(companiesResult);

        //Create some JsonObjects for Opportunity Data
        JsonObject opp1 = new JsonObject().add("name", "opportunity1")
           .add("externalopportunityid", "Opportunity1Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");
        JsonObject opp2 = new JsonObject().add("name", "opportunity2")
           .add("externalopportunityid", "Opportunity2Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");

        //Create an Instance of UpsertOpportunities and POST it
        UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                                .setAction("createOnly")
                                .addOpportunities(opp1, opp2);
        JsonObject oppsResult = upsertOpps.postData();
        System.out.println(oppsResult);

        //Create Some opp roles now
        JsonObject opp1Role = new JsonObject()
           .add("role", "Captain")
           .add("externalopportunityid", opp1.get("externalopportunityid").asString())
           .add("leadId", 318794);
        JsonObject opp2Role = new JsonObject()
           .add("role", "Commander")
           .add("externalopportunityid", opp2.get("externalopportunityid").asString())
           .add("leadId", 318795);

        //Create an Instance of UpsertOpportunityRoles and POST it
        UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
           .setAction("createOnly")
           .addRoles(opp1Role, opp2Role);
        JsonObject rolesResult = upsertRoles.postData();
        System.out.println(rolesResult);

    }
}
```

Você pode ver a sequência de criação de empresas, oportunidades e funções. Agora, você está pronto para sincronizar os dados da Empresa e da Oportunidade com a Marketo.

Publicado em _2015-08-07_ por _Kenny_

## Envio de emails transacionais com a API REST do Marketo: Parte 2, Conteúdo personalizado

Esta semana, estamos estudando como transmitir conteúdo dinâmico para nossos emails por meio da chamada de API Request Campaign. O Request Campaign não só permite o acionamento de emails externamente, como também é possível substituir o conteúdo de [Meus tokens](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program) em um email. Meus tokens são conteúdo reutilizável que pode ser personalizado no nível do programa ou da pasta de marketing. Eles também podem existir como espaços reservados que serão substituídos por meio da chamada de campanha de solicitação.

### Criação do email

Para personalizar nosso conteúdo, primeiro precisamos configurar um [programa](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program) e um [email](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) no Marketo. Para gerar nosso conteúdo personalizado, precisamos criar tokens dentro do programa e, em seguida, colocá-los no email que vamos enviar. Para simplificar, estamos usando apenas um token neste exemplo, mas você pode substituir qualquer número de tokens em um email, no campo Do email, Do nome, Responder para ou qualquer parte do conteúdo no email. Então, vamos criar um token de Rich Text para substituição e chamá-lo de &quot;bodyReplacement&quot;. O Rich Text permite substituir qualquer conteúdo no token pelo HTML arbitrário que queremos inserir. Os tokens não podem ser salvos enquanto estiverem vazios. Portanto, prossiga e insira um texto de espaço reservado aqui. Agora precisamos inserir nosso token no email: esse token agora estará acessível para substituição por meio de uma chamada de Campanha de solicitação. Esse token pode ser tão simples quanto uma única linha de texto que precisa ser substituída por email ou pode incluir quase todo o layout do email.

### O Código

Estamos expandindo o código da semana passada para enviar tokens personalizados para nossa chamada de campanha de solicitação.

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");

        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class="replacedContent"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Desta vez, estamos criando o conteúdo de nosso token na variável bodyReplacement e usando o método addToken para adicioná-lo à solicitação. O addToken pega uma chave e um valor, cria uma representação JsonObject e a adiciona à matriz de tokens interna. Isso é serializado durante o método postData e cria um corpo com esta aparência:

`{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}`

Combinada, nossa saída do console fica assim:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with&nbsp;...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"<apiuser@mktosupport.com>"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json?access_token=19d51b9a-ff60-4222-bbd5-be8b206f1d40:st
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

### Encapsulamento

Esse método é extensível de várias maneiras, alterando o conteúdo de emails em seções de layout individuais ou emails externos, permitindo que valores personalizados sejam passados para tarefas ou momentos interessantes. Em qualquer lugar que um token possa ser usado em um programa, ele pode ser personalizado usando esse método. Uma funcionalidade semelhante também está disponível com a chamada [Programar Campanha](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST), que permitirá processar tokens em uma campanha em lote inteira. Eles não podem ser personalizados com base no cliente potencial, mas são muito úteis para personalizar o conteúdo em um amplo conjunto de clientes potenciais.

Publicado em _2015-07-24_ por _Kenny_

## Atualização de dados de clientes potenciais no Marketo usando a API

Há mais de um ano, publicamos a Atualização de informações de clientes e prospetos no Marketo usando a API. Essa publicação apresentou uma amostra de código que poderia ser executada de forma recorrente para buscar atualizações em um sistema externo. A ideia era recuperar os dados externos e enviá-los para o Marketo para atualizar as informações dos clientes potenciais. A amostra de código apresentada usava nossa API SOAP. Ponto de acesso REST para atingir a mesma meta. **Entrada de programa** Provavelmente, você precisa transformar os dados do sistema externo em um formato que possa ser consumido pelas APIs REST do Marketo. Como estamos usando a API Criar/Atualizar leads, os dados devem ser formatados como JSON, que é enviado no corpo da solicitação. Isso é melhor explicado por um exemplo. No código de amostra Java abaixo, colocamos dados de lead simulados em uma matriz de strings. Cada string que segue os dados de cada lead: nome, sobrenome, endereço de email, cargo.

```java
private static String externalLeadData[] = {
        "Henry, Adams, <henry@superstar.com>, Director of Demand Generation",
        "Suzie, Smith, <ssmith@gmail.com>, VP Marketing"
};
```

O código de amostra transforma a matriz no bloco JSON abaixo.

```json
{
"input":
    [
        {"firstName":"Henry", "lastName":"Adams", "email":"<henry@superstar.com>", "title":"Director of Demand Generation"},
        {"firstName":"Suzie", "lastName":"Smith", "email":"<ssmith@gmail.com>", "title":"VP Marketing"}
    ]
}
```

Cada item de array de &quot;entrada&quot; corresponde a um lead individual no Marketo. Os itens da matriz são objetos JSON que contêm um ou mais nomes de campo de cliente potencial do Marketo e seus respectivos valores. Os nomes de campo que você decide especificar (nesse caso, firstName, lastName, email e title) devem corresponder ao nome da API REST definido para a assinatura do Marketo. Você pode encontrar os Nomes da API REST na seção de gerenciamento de campo no painel de administração do Marketo exportando os nomes de campo. Os nomes de campos serão exportados para um arquivo do Excel, como visto abaixo. Como alternativa, você pode encontrar nomes de campo de forma programática chamando a API [Descrever lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). Por exemplo, este é um trecho Descrever resposta que contém o nome da API REST para o campo Nome.

```json
{
    "id": 29,
    "displayName": "First Name",
    "dataType": "string",
    "length": 255,
    "soap": {
        "name": "FirstName",
        "readOnly": false
    },
    "rest": {
        "name": "firstName",
        "readOnly": false
    }
},
```

**Lógica do programa** Quando a carga estiver no formato adequado, estaremos prontos para chamar Criar/Atualizar clientes em potencial. Observe que nesta amostra não especificamos nenhum dos parâmetros opcionais. O comportamento padrão é criar ou atualizar registros de clientes potenciais (upsert), usar email para fins de desduplicação e usar processamento síncrono. Se a chamada para Criar/Atualizar leads for bem-sucedida, o corpo da resposta conterá uma matriz &quot;result&quot; contendo a ID do lead da Marketo e o status da operação Criar/Atualizar. Dependendo do parâmetro &quot;ação&quot; transmitido na solicitação, o status pode ser &quot;atualizado&quot;, &quot;criado&quot; ou &quot;ignorado&quot;. Continuando com nosso exemplo, suponha que o primeiro lead não existisse (Henry Adams) e o segundo lead existisse (Suzie Smith). Uma resposta semelhante à seguinte indicaria que o primeiro lead foi criado e o segundo foi atualizado.

```json
{
    "success":true,
    "requestId":"118f8#14f1a0b82fc",
    "result":[
        {
            "id":318798,
            "status":"created"
        },
        {
            "id":318797,
            "status":"updated"
        }
    ]
}
```

Por enquanto, é isso. Feliz codificação! **Código do Programa**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

public class CreateUpdateLeads {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            { INSERT YOUR CUSTOM SERVICE DATA HERE };

    //
    // Mock up data that could be read from an external data source.
    // An array of CSV strings the form:
    //   "firstName, lastName, email, title"
    private static String externalLeadData[] = {
        "Henry, Adams, henry@superstar.com, Director of Demand Generation",
        "Suzie, Smith, ssmith@gmail.com, VP Marketing"
    };

    public static void main(String[] args) {
        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URL for Create/Update Leads
        String createUpdateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", baseUrl, accessToken);

        // Convert String array into JsonArray to pass as part of request body
        JsonArray input = convertExternalLeadDataToJson(externalLeadData);

        // Build request body JSON
        JsonObject requestObj = new JsonObject();
        requestObj.add("input", input);

        // Call Create/Update Lead API
        JsonArray result = new JsonArray();
        JsonObject leadObj = JsonObject.readFrom(httpRequest("POST", createUpdateLeadsUrl, requestObj.toString()));
        if (leadObj.get("success").asBoolean()) {
            if (leadObj.get("result") != null) {
                result = leadObj.get("result").asArray();
            }
        }

        // Print out results object
        System.out.println(result);
        System.exit(0);
    }

    // Convert array of CSV formatted Strings into an array of JSON objects
    private static JsonArray convertExternalLeadDataToJson(String input[]) {
        JsonArray output = new JsonArray();

        // Loop through each CSV row in array
        for (int i = 0; i < input.length; i++) {
            // Split CSV row into separate fields
            List items = Arrays.asList(input[i].split(","));

            // Add fields to JSON object
            JsonObject lead = new JsonObject();
            lead.add("firstName", items.get(0));
            lead.add("lastName", items.get(1));
            lead.add("email", items.get(2));
            lead.add("title", items.get(3));
            output.add(lead);
        }
        return output;
    }

    // Perform HTTP request
    private static String httpRequest(String method, String endpoint, String body) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setDoOutput(true);
            urlConn.setRequestMethod(method);
            switch (method) {
                case "GET":
                    break;
                case "POST":
                    urlConn.setRequestProperty("Content-type", "application/json");
                    urlConn.setRequestProperty("accept", "text/json");
                    OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                    wr.write(body);
                    wr.flush();
                    break;
                default:
                    System.out.println("Error: Invalid method.");
                    return data;
            }
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Publicado em _2015-08-14_ por _David_

## Adicionar dados de SalesPerson ao Marketo

Com as novas APIs SalesPerson, é possível associar livremente leads da Marketo a registros SalesPerson em instâncias sem uma integração de CRM nativa. Isso permite o uso de {{lead.Lead Owner Email Address}} e campos e tokens relacionados dentro do Marketo.

### Criação de registros de SalesPerson

Para associar clientes em potencial a registros SalesPerson, primeiro precisamos inserir nossos registros SalesPerson no Marketo. Aqui está um exemplo de classe no PHP:

```php
<?php

class SalesPerson{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $dedupeBy;//dedupeFields or idField
 private $input;//array of salesperson objects for input

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->dedupeBy)){
   $body->dedupeBy = $this->dedupeBy;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/salespersons.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction($action){
  return $this->action;
 }
 public function setDedupeBy($dedupeBy){
  $this->dedupeBy = $dedupeBy;
 }
 public function getDedupeBy($dedupeBy){
  return $this->dedupeBy;
 }
 public function addSalesPersons($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else{
   $this->input = $input;
  }
 }
 public function getInput(){
  return $this->input;
 }

}
```

Na classe acima, a variável $input é uma matriz de objetos stdClass com possíveis membros:

* externalSalesPersonId (válido apenas na criação)
* id (somente como modo updateOnly de entrada de chave)
* email
* fax
* firstName
* lastName
* mobilePhone
* telefone
* título

Informações mais detalhadas sobre tipos e comprimentos de campos podem ser recuperadas por meio da chamada Descrever SalesPerson.

### Sincronização de clientes em potencial

Este é um exemplo rápido de classe para sincronizar os leads que precisamos:

```php
<?php

class Leads{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $lookupField;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 private $asyncProcessing;//specify whether input should be processed asynchronously
 private $partitionName;//partition name for update if requires

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->lookupField)){
   $body->lookupField = $this->lookupField;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/leads.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction(){
  return $this->action;
 }
 public function setDedupeBy($lookupField){
  $this->dedupeBy = $lookupField;
 }
 public function getDedupeBy(){
  return $this->dedupeBy;
 }
 public function addLeads($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else if (is_array($input)){
   $this->input = $input;
  }else{
   $this->input = [$input];
  }
 }
 public function getInput(){
  return $this->input;
 }
 public function setAsyncProcessing($asyncProcessing){
  $this->asyncProcessing = $asyncProcessing;
 }
 public function getAsyncProcessing() {
  return $this->asyncProcessing;
 }
 public function setPartitionName($partitionName){
  $this->partitionName = $partitionName;
 }
 public function getPartitionName() {
  return $this->partitionName;
 }

}
```

### Processar

Este é um exemplo de criação de dois registros de vendedor e sua associação a dois registros de lead:

```php
<?php

require 'Auth.php';
require 'SalesPerson.php';
require 'Leads.php';

//Create an Auth object for authentication
$auth = new Auth("https://284-RPR-133.mktorest.com", "7f99bd49-0e0e-4715-a72a-0c9319f75552", "O5lZHhrQDfDKRhulY89IU42PfdhRSe6m");

//Compose new SalesPerson Records
$sales1 = new stdClass();
$sales1->externalSalesPersonId = "SalesPerson 1";
$sales1->email = "sales1@example.com";
$sales1->firstName = "Jane";
$sales1->lastName = "Doe";

$sales2 = new stdClass();
$sales2->externalSalesPersonId = "SalesPerson 2";
$sales2->email = "sales2@example.com";
$sales2->firstName = "John";
$sales2->lastName = "Doe";

//Compose lead records
$lead1 = new stdClass();
$lead1->email = "marketoDev@example.com";
//Add the external id of the desired salesperson
$lead1->externalSalesPersonId = $sales1->externalSalesPersonId;

$lead2 = new stdClass();
$lead2->email = "devBlog@example.com";
$lead2->externalSalesPersonId = $sales2->externalSalesPersonId;

//Create a new instance of SalesPerson to submit records
$salesPerson = new SalesPerson($auth, [$sales1, $sales2]);
$spResponse = $salesPerson->postData();
print_r($spResponse . "rn");
$json = json_decode($spResponse);

//Check for success on synching SalesPersons
if ($json->success){
 $leads = new Leads($auth, [$lead1, $lead2]);
 $leadsResponse = $leads->postData();
 print_r($leadsResponse . "rn");
}else{
 print_r("SalesPerson request failed with errors:rn");
 foreach ($json->errors as $error){
  print_r("code: " . $error->code . ", message: " . $error->message . "rn");
 }
}
```

Para nossas classes de exemplo, estamos apenas criando objetos stdClass para representar nossos registros SalesPerson e Lead que precisam ser sincronizados, com cada campo desejado adicionado como um membro. Após a execução desse código, os clientes potenciais <marketoDev@example.com> e <devBlog@example.com> terão os campos Email do proprietário principal, Nome do proprietário principal e Sobrenome do proprietário principal preenchidos, permitindo que eles usem os tokens relevantes para esses campos e sejam filtrados pelos filtros de lista inteligente relevantes.

### Classe de autenticação

```php
<?php

class Auth{
 private $host;//host of the target Marketo instance
 private $clientId;//client Id
 private $clientSecret;//client secret
 private $token;//access_token
 private $expiry;

 function _construct($host, $clientId, $clientSecret){
  $this->host = $host;
  $this->clientId = $clientId;
  $this->clientSecret = $clientSecret;
 }
 public function getToken(){
  if (!isset($this->token) || $this->expiry > time()){
   $data = $this->getData();
   $this->expiry = time() + $data->expires_in;
   $this->token = $data->access_token;
   return $this->token;
  }else{
   return $this->token;
  }
 }
 private function getData(){
  $url = $this->host . "/identity/oauth/token?grant_type=client_credentials&client_id=" . $this->clientId . "&client_secret="
     . $this->clientSecret;
  $ch = curl_init($url);
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  $response = curl_exec($ch);
  $json = json_decode($response);
  curl_close($ch);
  return $json;
 }
 public function getHost(){
  return $this->host;
 }
}
```

Publicado em _2015-08-21_ por _Kenny_

## Práticas recomendadas para usuários da API e serviços personalizados

As REST APIs da Marketo usam serviços personalizados para autenticação e cada um desses serviços pertence a um usuário Marketo somente de API. Os recursos de cada serviço personalizado são determinados pelas permissões de cada função atribuída a esse usuário. A alocação de usuários individuais e serviços personalizados para suas integrações oferece vários benefícios:

* Você pode ajustar as [permissões](/help/rest-api/custom-services.md) fornecidas a cada serviço individual por meio da função atribuída ao usuário.
* Você pode impedir que serviços da Web individuais façam chamadas para sua instância ao excluir o serviço personalizado correspondente, sem desativar outros.
* Os relatórios sobre o uso de chamadas da API serão divididos pelo usuário, permitindo identificar utilização alta e anormal
* É mais fácil determinar a quais dados cada serviço da Web está sendo concedido acesso
* As instâncias habilitadas para o Workspace podem restringir o acesso a unidades de negócios específicas, concedendo funções somente a espaços de trabalho acessíveis

### Utilização da API

Cada um dos usuários da API é relatado individualmente no relatório de uso da API, portanto, dividir os serviços da Web por usuário permite considerar facilmente o uso de cada uma de suas integrações. Se o número de chamadas de API para sua instância exceder o limite, causando falha nas chamadas subsequentes, o uso dessa prática permitirá considerar o volume de cada um dos serviços e avaliar como resolver o problema. Acesse Admin -> Serviços da Web e clique no número de chamadas nos últimos 7 dias para ver o seu uso.

### Desativar um serviço

Se uma integração estiver tendo efeitos indesejáveis, pode ser entediante e difícil desabilitar se você não tiver atribuído a cada uma delas um serviço personalizado individual. Tê-los divididos um por um torna mais fácil do que excluir o serviço ofensivo em seu Admin -> Launchpoint.

### Gerenciamento do Workspace

Para assinaturas do Marketo Enterprise, é comum que um serviço precise acessar apenas um único espaço de trabalho, e isso pode ser [imposto pela atribuição de função ao Usuário da API](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/allow-user-access-to-a-workspace). Cada função de usuário pode ser atribuída globalmente ou por espaço de trabalho, para que o acesso possa ser restrito em espaços de trabalho sempre que apropriado, fornecendo o conjunto mínimo de permissões possível.

Publicado em _2015-08-28_ por _Kenny_

## Como especificar partições de lead usando a API REST

**Particionamento de leads** As partições de leads da Marketo oferecem uma maneira conveniente de isolar leads. As partições podem permitir que diferentes grupos de marketing em sua organização compartilhem uma única instância do Marketo. Para obter mais informações, consulte [Noções Básicas sobre Espaços de Trabalho e Partições de Cliente Potencial](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/understanding-workspaces-and-person-partitions). Suponha que você esteja usando partições de clientes potenciais e criando clientes potenciais de forma programática usando a API REST do Marketo. Como você garante que os clientes em potencial criados terminarão na partição correta? Esta publicação mostra como! Por causa deste exemplo, usaremos Espaços de trabalho e Partições para isolar nossos leads com base na localização geográfica.

Primeiro, definiremos um espaço de trabalho chamado &quot;País&quot;. Em seguida, criamos duas partições dentro desse espaço de trabalho chamadas &quot;México&quot; e &quot;Canadá&quot;.  **Criar cliente em potencial na partição** Suponha agora que desejamos criar dois clientes em potencial na partição &quot;México&quot;. Para criar leads, chamamos de. Para especificar a partição, devemos incluir o atributo &quot;partitionName&quot; no corpo da solicitação. Como sabemos o que usar para o valor partitionName? Podemos recuperar uma lista de valores de nome de partição válidos para nossa instância chamando a API [Obter Partições de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeProgramMemberUsingGET) da seguinte maneira:

`GET /rest/v1/leads/partitions.json`

```json
{
    "requestId": "20ae#14f9a1a5a30",
    "result": [
        {
            "id": 1,
            "name": "Default",
            "description": "Initial system lead partition"
        },
        {
            "id": 2,
            "name": "Mexico",
            "description": "Leads that live in Mexico"
        },
        {
            "id": 3,
            "name": "Canada",
            "description": "Leads that live in Canada"
        }
    ],
    "success": true
}
```

Nesse caso, o valor correto é &quot;México&quot;, então passamos isso para Criar/Atualizar leads da seguinte maneira:

`POST /rest/v1/leads.json`

```json
{
    "input": [
        {"email":"enrique.pena-nieto@gob.mx"},
        {"email":"angelica.rivera@gob.mx"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Aqui estão nossos leads recém-criados no Marketo.  **Atualizar cliente potencial na Partição** Para atualizar clientes potenciais existentes em uma partição, basta chamar Criar/Atualizar Clientes Potenciais e especificar partitionName da mesma forma que anteriormente. Nesta atualização, atribuiremos nome, sobrenome e título aos clientes potenciais que criamos acima.

`POST /rest/v1/leads.json`

```json
{
"input": [
        {"email":"enrique.pena-nieto@gob.mx", "firstName":"Enrique", "lastName":"Pena Neito", "title": "El Presidente"},
        {"email":"angelica.rivera@gob.mx", "firstName":"Angelica", "lastName": "Rivera", "title": "Primera Dama"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Estes são nossos clientes em potencial recém-atualizados no Marketo.
**Identificar Partição para um Cliente Potencial** Como sabemos em qual partição um cliente potencial está? Para isso, usamos a API [Obter lead por ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) e especificamos &quot;leadPartitionId&quot; no parâmetro de consulta &quot;fields&quot;. Nesse caso, recuperaremos as informações da ID do cliente potencial 318816 que criamos acima.

`GET /rest/v1/lead/318816.json?fields=leadPartitionId,email,firstName,lastName,title`

```json
{
    "requestId": "5c57#14f9a495b1f",
    "result": [
        {
            "id": 318816,
            "lastName": "Pena Neito",
            "title": "El Presidente",
            "email": "enrique.pena-nieto@gob.mx",
            "firstName": "Enrique",
            "leadPartitionId": 2
        }
    ],
    "success": true
}
```

Observe que obtemos leadPartitionId em vez do partitionName. Para obter o partitionName, precisamos revisitar a saída da API Obter partições de clientes potenciais acima.

```json
{
    "id": 2,
    "name": "Mexico",
    "description": "Leads that live in Mexico"
},
```

Nesse caso, um valor leadPartitionId de 2 mapeia para o partitionName de &quot;México&quot;. Isso é tudo por enquanto. Feliz particionamento!

Publicado em _2015-09-04_ por _David_

## Comparação de campos de pontuação no Script de email do Marketo

**Observação:** esta é uma publicação de convidado de Cathal Moran. Cathal é Consultora de soluções e trabalha no Escritório da Marketo na Europa, no Oriente Médio e na África, em Dublin, Irlanda.

### Comparação de campos de pontuação

Muitos clientes do Marketo, especialmente aqueles focados em vendas cruzadas, têm vários campos de pontuação e isso é usado com frequência para medir o interesse de um lead em um produto/área específica. Imaginem que eu vendo maçãs e bananas, se um chumbo tem uma pontuação de 50 para maçãs e 10 para bananas então fica claro onde está a preferência, não seria bom se o meu conteúdo refletisse esta preferência. O script de email pode ser usado para comparar pontuações e personalizar o conteúdo em um email, dependendo da pontuação mais alta (ou mais baixa) desse lead específico que recebe o email.

### Como eu quero comparar 2 números eu preciso converter meus valores de campo para inteiros

```
#set ($score1 = $math.toInteger(${lead.Apple_Score}))
#set ($score2 = $math.toInteger(${lead.Banana_Score}))
##check if the lead score is greater than feature score
#if($score1 >= $score2)
                ##if Apple score is greater
                #set($Interest = "Special offer on Apples")
##check is the feature score is higher
#elseif($score2 >= $score1)
                ##if Feature score is greater
                #set($Interest = "Special offer on Bananas")
#else
                ##otherwise
                #set($Interest = "Special offer on Fruit")
#end
##display the Interest as content
${Interest}
```

No exemplo acima, estou apenas personalizando o texto, mas poderia, facilmente, por exemplo, exibir uma imagem diferente com base na pontuação mais alta.

Publicado em _2015-09-14_ por _Kenny_

## Criar um envio de formulário do Marketo em segundo plano

Quando sua organização tem muitas plataformas diferentes para hospedar conteúdo da Web e dados de clientes, é bastante comum precisar de envios paralelos de um formulário para que os dados resultantes possam ser coletados em plataformas separadas. Há várias estratégias para fazer isso, mas a melhor delas geralmente é a mais simples: usar a API do Forms 2 para enviar um formulário oculto do Marketo. Isso funcionará com qualquer novo formulário do Marketo, mas idealmente você deve criar um formulário vazio para isso, que não tem campos. Isso garantirá que o formulário não carregue mais dados do que o necessário, já que não precisamos renderizar nada. Agora basta pegar o [código incorporado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) do seu formulário e adicioná-lo ao corpo da página desejada, fazendo uma pequena modificação. Seu código integrado inclui um elemento de formulário como este:

`<form id="mktoForm_1068"></form>`

Você adicionará &#39;style=&quot;display:none&quot;&#39; ao elemento para que ele não fique visível, desta forma:

`<form id="mktoForm_1068" style="display:none"></form>`

Uma vez que o formulário é incorporado e oculto, o código para enviar o formulário é realmente muito simples:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"test@example.com",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

O Forms enviado dessa maneira se comportará exatamente como se o lead tivesse preenchido e enviado um formulário visível. O acionamento do envio varia entre as implementações, pois cada uma tem uma ação diferente para solicitá-la, mas é possível fazer com que ele ocorra basicamente em qualquer ação. A parte importante é definir os campos e valores corretamente. Use o nome da API SOAP de seus campos, que você pode encontrar com [Exportar nomes de campos](/help/rest-api/list-of-standard-fields.md) para garantir o envio correto dos valores.

Migração do Munchkin Associate Lead

O envio de formulário em segundo plano é um dos métodos de substituição recomendados para o líder associado da Munchkin. A página de exemplo do HTML abaixo demonstra a migração em alto nível, reutilizando os mesmos valores que são enviados para Associar lead.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName('head')[0].appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //submit the form
            form.submit();


        })
    </script>
</body>
</html>
```

Publicado em _2015-09-25_ por _Kenny_

## Exceção da API REST do Marketo e tratamento de erros: Parte 1

A API REST do Marketo pode retornar uma exceção ou erro, que, para maior comodidade, chamaremos de erros daqui em diante. Os erros podem ocorrer em três níveis diferentes:

* Nível HTTP, esses erros são indicados por um código 4xx
* Nível de resposta, esses erros são incluídos na matriz &quot;erros&quot; da resposta JSON
* No nível do registro, esses erros são incluídos na matriz &quot;resultado&quot; da resposta JSON e são indicados em uma base de registro individual com o campo &quot;status&quot; e a matriz &quot;motivos&quot;

### Erros de HTTP

Em circunstâncias operacionais normais, o Marketo só deve retornar dois erros de status HTTP, 413 Entidade de solicitação muito grande e 414 URI de solicitação muito longo. Ambos são recuperáveis ao capturar o erro, modificar a solicitação e tentar novamente, mas com práticas de codificação inteligente, você nunca deve encontrá-los na natureza. O Marketo retornará 413 se a Carga da solicitação exceder 1 MB, ou 10 MB no caso de [Importar lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads). Na maioria dos cenários, é improvável atingir esses limites, mas adicionar uma verificação ao tamanho da solicitação e mover quaisquer registros que façam com que o limite seja excedido para uma nova solicitação deve impedir que quaisquer circunstâncias que levem a esse erro sejam retornadas por quaisquer pontos de extremidade. 414 serão retornados quando o URI de uma solicitação GET exceder 8KiB. Para evitá-lo, verifique o comprimento da sua cadeia de caracteres de consulta para ver se ela excede esse limite. Se isso fizer com que a solicitação seja alterada para um método POST, insira a string de consulta como o corpo da solicitação com o parâmetro adicional &#39;_method=GET&#39;. Isso abre mão da limitação nos URIs. É raro atingir esse limite na maioria dos casos, mas é um pouco comum ao recuperar grandes lotes de registros com valores de filtro individuais longos, como uma GUID.

### Erros de nível de resposta

Erros de nível de resposta ocorrem quando o parâmetro &quot;success&quot; da resposta é definido como false. Cada entrada em &quot;erros&quot; tem dois membros, &quot;codifica&quot; um número, 6xx ou 7xx, e uma &quot;mensagem&quot; fornecendo o motivo do texto sem formatação para o erro. Os códigos 6xx sempre indicam que uma solicitação falhou completamente e não foi executada. Um exemplo disso é um 601, &quot;Token de acesso inválido&quot;, que pode ser recuperado por meio da reautenticação e da transmissão do novo token de acesso com a solicitação. Os erros 7xx indicam que a solicitação falhou, seja porque nenhum dado foi retornado ou porque a solicitação foi parametrizada incorretamente, como a inclusão de uma data inválida ou a ausência de um parâmetro obrigatório.

### Erros de nível de registro

Os erros no nível do registro indicam que não foi possível concluir uma operação para um registro individual, mas a própria solicitação era válida. Eles ocorrem em um registro individual na matriz de &quot;resultados&quot; de uma resposta. O campo &quot;status&quot; desses registros será &quot;ignorado&quot; e uma matriz &quot;reason&quot; estará presente. Cada motivo contém um membro &quot;código&quot; e um membro &quot;mensagem&quot;. O código sempre será 1xxx e a mensagem indicará por que o registro foi ignorado. Um exemplo seria quando uma solicitação Criar/Atualizar clientes em potencial tem &quot;ação&quot; definida como &quot;createOnly&quot;, mas já existe um cliente em potencial para uma das chaves nos registros enviados. Esse caso retorna um código 1005 e uma mensagem de &quot;Lead já existe&quot;. Nas próximas publicações, analisaremos os erros recuperáveis e alguns exemplos de como lidar com eles dentro do seu código.

Publicado em _2015-10-09_ por _Kenny_

## Guest Post - Conectores SQL diretos para o banco de dados Marketo

**Esta é uma publicação de convidado de Sumit Sarkar da Progress, Inc.**

**Conectores SQL para o banco de dados Marketo** O Marketo tem APIs bem documentadas para acessar dados. No entanto, às vezes, as organizações precisam de acesso direto ao SQL. Estamos vendo as linhas borradas nas lojas da Marketo entre Marketing e TI, o que está aumentando a demanda por conectividade SQL baseada em padrões. Os conectores SQL diretos para o Marketo estão disponíveis por meio da [Progress DataDirect Cloud](https://www.progress.com/connectors/cloud-data-integration). A DataDirect Cloud é um serviço de conectividade de dados que expõe os dados do Marketo por meio de padrões abertos do setor para acesso SQL no ODBC (&quot;Conectividade de banco de dados aberta&quot;) ou JDBC (&quot;Conectividade de banco de dados Java&quot;); e REST com OData (&quot;Protocolo de dados aberto&quot;). Abaixo estão alguns casos de uso populares para conectar dados do Marketo prontos para uso usando a DataDirect Cloud:

* Detecção e visualização de dados (Qlik, Tableau, SAP Lumira)
* Emissão de relatórios corporativos (SAP Business Objects, Microstrategy, Cognos)
* Integração de dados (SQL Server Integration Services - SSIS, Oracle Data Integrator - ODI, Informatica)
* Federação de Dados (Servidor Vinculado do SQL Server, SAP Hana SDA, Oracle Database Gateway)
* Consulta Ad Hoc (Microsoft Office, Visualizador de BD, Aqua Data Studio)
* Preparação De Dados (Alteryx, Trifecta, Paxata)

**Como funciona o acesso SQL ao Marketo?**

* A DataDirect Cloud cria um esquema lógico para dados expostos pelas APIs de integração do Marketo.
* O DataDirect Cloud processa solicitações SQL de clientes ODBC ou JDBC leves usando um mecanismo de consulta elástico em tempo real nos dados obtidos das APIs do Marketo.
* A conectividade do DataDirect Cloud é direta e em tempo real, sem qualquer duplicação de dados.
* O DataDirect Cloud Service abstrai a API do Marketo de modo que os usuários finais não precisem se preocupar com as alterações de versão da API ou usar o SOAP versus REST.

O DataDirect vem criando esse estilo de conectividade com fontes de dados SaaS desde 2006, começando com o primeiro driver ODBC da Salesforce criado com base em suas APIs de serviço da Web. **Introdução à conexão com o Marketo:**

1. [Registrar-se para um Logon de Nuvem do DataDirect](https://pacific.progress.com/console/register?productName=d2c&ignoreCookie=true)
1. Clique no botão &quot;Fontes de dados&quot; e depois no botão &quot;+Novo Source de dados&quot;
1. Selecione &quot;Marketo&quot; e insira as informações de conexão. Você pode verificar com o administrador do Marketo ou fazer logon para encontrar [informações de conexão para integração com o SOAP](/help/soap-api/soap-api.md).
1. Clique no botão &quot;Testar conexão&quot;. Observe que há uma guia OData para produzir OData do Marketo e discutiremos isso em uma publicação futura do blog.
1. Clique em &quot;Teste de SQL&quot; se desejar inspecionar o esquema do Marketo exposto ou emitir consultas SQL básicas na interface do usuário.
1. Clique em &quot;Downloads&quot; à esquerda e selecione o driver DataDirect Cloud ODBC ou JDBC para o aplicativo e a plataforma a serem instalados.
1. Depois que o driver DataDirect Cloud ODBC ou JDBC estiver instalado, você poderá conectar quaisquer aplicativos baseados em padrões à Marketo.

Este é um exemplo em vídeo de [conexão usando o cliente ODBC da DataDirect Cloud](https://www.youtube.com/watch?v=H6PHra56Iig). Estes são outros tutoriais da DataDirect Cloud que se aplicam ao Marketo:

* [Análise de Dados SAP](http://scn.sap.com/community/lumira/blog/2015/08/05/connect-sap-lumira-to-eloqua-marketo-google-analytics)
* [Relatórios Corporativos da Microestratégia](https://community.microstrategy.com/t5/Tech-Corner/What-MSTR-developers-should-know-about-Cloud-Data-Sources/ba-p/253083)
* [Integração de dados do Oracle](https://www.ateam-oracle.com/post/a-universal-cloud-applications-adapter-for-odi/)

**Desafios de P&amp;D na criação de conectividade SQL entre fontes na nuvem, como o Marketo**

Nem todas as APIs SaaS expõem uma linguagem de consulta padrão. Nesses casos, a equipe de engenharia examina cada objeto individualmente. Cada objeto pode ser exposto com uma API diferente com regras exclusivas para chamar, pesquisar filtragem, etc. Isso exigiu um esforço significativo para fornecer uma experiência padrão de consulta em todo o modelo de dados.

Lidar com recursos de associação completa. Nos casos em que as APIs SaaS não oferecem suporte a uma linguagem de consulta com recurso JOIN, a equipe de engenharia deve executar essa operação. Isso requer uma conversão do SQL para chamar com eficiência as APIs do Marketo para retornar a quantidade mínima de dados antes de executar a associação. Ao unir dois objetos muito grandes, a camada de acesso a dados pode usar recursos consideráveis no servidor de aplicativos ou no desktop. Portanto, a implantação da camada de acesso a dados em um serviço na nuvem elástico, como o DataDirect Cloud, faz muito sentido por dois motivos:

1. Desempenho mais rápido e uso de menos recursos de memória/CPU no servidor de aplicativos cliente ou no desktop
1. Aproveite a largura de banda superior entre a DataDirect Cloud e a Marketo, onde os conjuntos de dados pré-unidos são trocados.

Como lidar com modelos de dados? Ele é estático ou dinâmico? Como as alterações são detectadas e comunicadas ao cliente? Cada fonte de dados SaaS é diferente e, no caso do Marketo, determinados objetos são melhor consultados por meio de visualizações e outros por meio de tabelas. Manipular essa matriz de modelos de dados e objetos em todas as fontes SaaS foi certamente um desafio.

**Referência de nuvem do Marketo e do DataDirect para desenvolvedores:**

* Propriedades de Conexão do Marketo ([link para documentos](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* SQL e extensões com suporte no Marketo ([link para docs](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html,-hubspot,-and-marketo.html))
* Tabelas e Exibições do Marketo Expostas ([link para documentos](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Mensagens de erro comuns retornadas do Marketo ([link para docs](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))

**Biografia:** Sumit Sarkar é um Divulgador-Chefe de Dados da Progress, com mais de 10 anos de experiência trabalhando no campo da conectividade de dados. O principal consultor mundial em conectividade de padrões de dados abertos com dados em nuvem, os interesses da Sumit incluem o ajuste de desempenho da camada de acesso a dados para a qual ele desenvolveu uma tecnologia de patente pendente para sua análise; business intelligence e data warehouse para plataformas SaaS; e conectividade de dados para ambientes PaaS, com foco em padrões como ODBC, JDBC, ADO.NET e ODATA. Ele é um Consultor certificado da IBM para a IBM Cognos Business Intelligence e um membro do TDWI. Ele apresentou sessões sobre conectividade de dados em várias conferências, incluindo Dreamforce, Oracle OpenWorld, Strata Hadoop, MongoDB World e SAP Analytics e Business Objects Conference, entre muitas outras. Twitter: @SAsInSumit LinkedIn: [www.linkedin.com/in/meetsumit](http://www.linkedin.com/in/meetsumit)

Publicado em _2015-12-07_ por _Kenny_

## Criar um painel para Uso de API e Contagens de erros

Como consumidor da API do Marketo, essas são informações úteis que você deve acompanhar. E se você pudesse obter dados históricos de uso para ajudar a detectar tendências ao longo do tempo? E se você pudesse obter um resumo dos códigos de erro da API para ajudar a medir a integridade da integração? Como um Parceiro de tecnologia da Marketo, e se você pudesse obter dados de uso e de erro em todas as contas de clientes em um único painel? Esta publicação fornecerá uma abordagem para responder às perguntas acima. Aperte a fivela, aqui vamos nós!
**Trabalho agendado para recuperação de estatísticas**. Vamos criar um aplicativo que recupere dados de uso e erro usando os pontos de extremidade [Obter uso diário](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLast7DaysErrorsUsingGET) e [Obter erros diários](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyErrorsUsingGET). O aplicativo foi projetado para ser programado para ser executado uma vez por dia. Cada vez que o aplicativo é executado, ele anexa dados de uso de um dia a um arquivo e dados de erro de um dia a outro arquivo. No início de cada mês, um novo par de arquivos é criado. Esses arquivos servirão como um registro histórico que podemos acessar a qualquer momento. Esta é a lógica do aplicativo...

* Leia informações da conta do Marketo (Munchkin id e credenciais do cliente) de uma fonte externa. Observação: essa fonte deve ser segura para impedir que outras pessoas acessem os dados da conta.
* Iterar em cada conta e...
   * Chamada para Obter Uso Diário para recuperar dados de uso de um dia
   * Anexar dados de uso diário a um arquivo de &quot;uso&quot; mensal
   * Chamada para Obter Erros Diários para recuperar dados de erros de um dia
   * Anexar dados de erros diários a um arquivo mensal &quot;errors&quot;

Formato do arquivo de saída O formato dos arquivos de saída é JSON, que corresponde à matriz &quot;resultado&quot; retornada das respectivas chamadas de API (Uso e Erro). Cada elemento da matriz &quot;result&quot; é um objeto JSON que contém dados de um dia. Nomeação do arquivo de saída Os arquivos de saída são nomeados da seguinte maneira:

`<type>_<yyyy>_<mm>_<account>.json`

Onde,

`<type>` - O tipo de dados (&quot;uso&quot; ou &quot;erros&quot;) `<yyyy>` - O ano (4 dígitos) `<mm>` - O mês (2 dígitos) `<account>` - A ID da conta (Munchkin id)

Exemplos de arquivo de saída usage_2015_10_111-AAA-222.json

```json
[
    { "date": "2015-10-15", "total": 0, "users" : [] },
    { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
    { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] },
]
```

`errors_2015_10_111-AAA-222.json:`

```json
[
    { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
    { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
    { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
]
```

O código deste aplicativo é apresentado no final desta publicação (Stats.java). **Serviço Web Stats** Agora precisamos encontrar uma maneira de inserir esses dados em nosso navegador. A proposta é criar um serviço Web para fornecer os dados. O serviço Web consumirá os arquivos de saída do aplicativo e retornará os dados ao navegador em um formato que possa ser apresentado prontamente. Para simplificar e contornar as restrições de política de mesma origem, aproveitaremos o padrão [JSONP](https://en.wikipedia.org/wiki/JSONP). Esta é a especificação de ponto de extremidade REST proposta para o serviço Web externo: **URI**: /stats **Método**: GET

**Parâmetro**

**Descrição**

**Exemplo**

mês

Recuperar dados deste mês. Lista separada por vírgulas de meses a serem incluídos (representação de 2 dígitos). Padrão para todos os meses.

10,11

ano

Recuperar dados deste ano. Lista separada por vírgulas de anos a serem incluídos (representação de 4 dígitos). Padrão para todos os anos.

2015

account

Recuperar dados desta conta (Munchkin id).

111-AAA-222

retorno de chamada

Nome da função com a qual vincular conteúdo JSON.

processStats

Exemplo de solicitação

`GET //localhost:8080/stats?month=10&year=2015&account=111-AAA-222&callback=processStats`

O serviço da Web lê os arquivos &quot;usage&quot; e &quot;error&quot; e os combina e os retorna neste formato:

```html
**<Name of Callback here>**

<Contents of Usage file here>,

<Contents of Error file here>
```

Este é um retorno de chamada JSONP com 2 argumentos. O primeiro argumento é a matriz de uso &quot;result&quot;. O segundo argumento é a matriz &quot;result&quot; de erros. Exemplo de resposta

```json
processStats(
    [
        { "date": "2015-10-15", "total": 0, "users" : [] },
        { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
        { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] }
    ],
    [
        { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
        { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
        { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
    ]
);
```

Como você pode ver, o serviço da Web simplesmente encapsulou o conteúdo dos dois arquivos de saída do nosso aplicativo. Criamos uma resposta de serviço Web fictícia usando [Mocky](https://designer.mocky.io). Um exemplo do serviço Web no qual o modelo está [aqui.A criação ](http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats) deste serviço Web foi deixada como um exercício para o leitor: **Página da Web do Painel**. Agora, basta uma página da Web que chame nosso serviço Web e formate os dados. Para usar o padrão JSONP, basta adicionar uma tag `<script>` que chame o serviço Web:

`<script src="http: //<hostname>/stats?month=10&year=2015&account=284-RPR-133&callback=processStats"></script>`

Isso injetará o corpo da resposta do serviço Web diretamente na página do HTML. Em seguida, adicionamos a função de retorno de chamada JSONP:

```javascript
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
;
```

Essa função é chamada automaticamente após a chamada do serviço Web. Neste exemplo, chamamos um simples &quot;dumper de variável&quot; do JavaScript chamado [prettyPrint.js](https://github.com/padolsey-archive/prettyprint.js/tree/master) em cada matriz. A função prettyPrint simplesmente produz uma tabela HTML usando o conteúdo da matriz. Esta é uma captura de tela das tabelas do HTML. Voilà, esse é o nosso painel! É certo que isso não é muito elegante, mas deve dar-lhe uma ideia do que é possível. Não há nada que o impeça de transformar os dados da maneira que você quiser para fazer suas próprias visualizações atraentes. A página do HTML está abaixo (Index.html). É possível exibir as tabelas acima em tempo real no navegador usando as seguintes etapas:

1. Salvar uma cópia local de Index.html
1. Salvar uma cópia local de prettyPrint.js
1. Abra Index.html no navegador

Então é isso. Esperamos que esta publicação tenha lhe dado algumas ideias sobre como monitorar as estatísticas da API do Marketo. Feliz codificação! **Stats.java**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import java.io.\*;
import java.lang.reflect.Array;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

import java.nio.file.Files;
import java.nio.file.Paths;

import javax.net.ssl.HttpsURLConnection;

public class Stats {
    //
    // Define Marketo instance meta data here. Each row contains three elements: Account Id, Client Id, Client Secret.
    //
    // Note: that this information would typically be stored read from an external source (i.e. database)
    //
    // For example:
    //
    // private static String final CUSTOM_SERVICE_DATA[][] = {
    // {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    // };
    //
    private static final String CUSTOM_SERVICE_DATA[][] = {
    };

    // Output directory for stats files
    private static final String OUTPUT_DIR = "C:stats";

public static void main(String[] args) {

    // Loop through each Marketo instance
    for (int i = 0; i < CUSTOM_SERVICE_DATA.length; i++) {

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA[i][0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
            baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[i][1], CUSTOM_SERVICE_DATA[i][2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose Get Last 7 Days Usage URL
        String usageUrl = String.format("%s/rest/v1/stats/usage/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Compose Get Last 7 Days Errors URL
        String errorsUrl = String.format("%s/rest/v1/stats/errors/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Process usage data
        JsonObject usageObj = JsonObject.readFrom(httpRequest("GET", usageUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (usageObj.get("result") != null) {
                updateFile(usageObj, "usage", CUSTOM_SERVICE_DATA[i][0]);
            }
        }

        // Process errors data
        JsonObject errorsObj = JsonObject.readFrom(httpRequest("GET", errorsUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (errorsObj.get("result") != null) {
                updateFile(errorsObj, "errors", CUSTOM_SERVICE_DATA[i][0]);
            }
        }
    }
    System.exit(0);
}

// Write yesterday's data to output file
private static void updateFile(JsonObject usageObj, String statsType, String account){

    JsonArray usageResultAry = usageObj.get("result").asArray();
    JsonObject yesterdayUsageResultObj = usageResultAry.get(1).asObject();

    String yesterdayDate = yesterdayUsageResultObj.get("date").asString();
    String[] yesterdayDateAry = yesterdayDate.split("[-]+");

    // "C:statsstats_yyyy_mm_account.json"
    String statsFile = String.format("%s%s_%s_%s_%s.json",
        OUTPUT_DIR, statsType, yesterdayDateAry[0], yesterdayDateAry[1], account);

    // Create file
    File file = new File(statsFile);
    try {
        if (file.createNewFile()) {
            // created new file, seed with empty array
            FileWriter fw = new FileWriter(file.getAbsoluteFile());
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[n]");
            bw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Read file
    String content = null;
    try {
        content = new String(Files.readAllBytes(Paths.get(statsFile)));
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Remove trailing "]", append new record, append trailing "]"
    content = content.substring(0, content.length() - 1);
    content += yesterdayUsageResultObj.toString();
    content += "n]";

    // Write file
    FileWriter fw = null;
    try {
        fw = new FileWriter(file.getAbsoluteFile());
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(content);
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// Perform HTTP request
private static String httpRequest(String method, String endpoint, String body) {
    String data = "";
    try {
        URL url = new URL(endpoint);
        HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
        urlConn.setDoOutput(true);
        urlConn.setRequestMethod(method);
        switch (method) {
            case "GET":
                break;
            case "POST":
                urlConn.setRequestProperty("Content-type", "application/json");
                urlConn.setRequestProperty("accept", "text/json");
                OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                wr.write(body);
                wr.flush();
                break;
            default:
                System.out.println("Error: Invalid method.");
                return data;
        }
        int responseCode = urlConn.getResponseCode();
        if (responseCode == 200) {
            InputStream inStream = urlConn.getInputStream();
            data = convertStreamToString(inStream);
        } else {
            System.out.println(responseCode);
            data = "Status:" + responseCode;
        }
    } catch (MalformedURLException e) {
        System.out.println("URL not valid.");
    } catch (IOException e) {
        System.out.println("IOException: " + e.getMessage());
        e.printStackTrace();
    }
    return data;
}

private static String convertStreamToString(InputStream inputStream) {
    try {
        return new Scanner(inputStream).useDelimiter("A").next();
    } catch (NoSuchElementException e) {
        return "";
    }
}
}
```

**Índice.html**

```html
<html>
<head>
<title>Marketo API Stats</title>
<!-- Browser JavaScript variable dumper                  -->
<!-- https://github.com/padolsey-archive/prettyprint.js  -->
<script src="prettyPrint.js"></script>
</head>
<body>

<h1>Marketo API Stats</h1>

<script>
// JSONP callback that uses prettyPrint to format API stats
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
};
</script>

<!-- Web service for you to implement as an exercise                                                        -->
<!-- <script src="http://localhost:8080/stats?month=10&account=111-AAA-222&callback=processStats"></script> -->

<!-- Mock web service that returns sample payload -->
<!-- http://www.mocky.io/                         -->
<script src="http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats"></script>"
</body>
</html>
```

Publicado em _2015-10-22_ por _David_

## Exceção da API REST do Marketo e tratamento de erros: Parte 3

Na maioria das vezes, os erros recebidos de volta da API REST do Marketo não serão recuperáveis automaticamente. No entanto, há vários casos em que você pode se recuperar automaticamente ou garantir que nunca veja um determinado tipo de erro.

### Erros de tamanho de solicitação

Conforme observamos na última publicação desta série, o Marketo emitirá o código de status HTTP 414 se o URI exceder 8KiB de comprimento, ou 413 se o corpo da solicitação exceder 1MB, ou 10MB para Importar lead. Embora os 414s sejam raros, você poderá vê-los se estiver usando o tipo Obter clientes em potencial por filtro para solicitar registros com base em 300 GUIDs separados ou critérios semelhantes. Digamos que você tenha a seguinte solicitação: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?filterType=customGUID&fields=email`, company...firstName, lastName> Quando você envia a solicitação, o Marketo retorna um status de 414 porque o URI excede 8KiB.

Para lidar com isso, precisamos alterar o padrão dessa solicitação e enviar um POST em vez de um GET, anexar &#39;_method=GET&#39; ao URI e passar a sequência de consulta no corpo da solicitação como uma solicitação x-www-form-urlencoded em vez disso: URI: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?_method=GET>` Corpo da solicitação: filterType=customGUID&amp;fields=email,company...firstName, lastName Em vez de capturar essa exceção da resposta HTTP, no entanto, podemos verificar o comprimento total da solicitação no tempo de execução e implantar esse padrão alternativo se o URI exceder 8k. Como alternativa, você pode usar o Método POST em todos os casos para recuperações de registros em lote. Para 413s, podemos seguir um padrão semelhante, verificando o comprimento do corpo da solicitação ao adicionar registros durante a etapa de serialização e dividindo a solicitação em várias partes se esse limite for excedido.

### Erros de autenticação

Nossa próxima classe de erros recuperáveis está relacionada à autenticação. Quando um token de acesso válido anteriormente é usado após o período expires_in ter decorrido, o primeiro uso retornará um código de erro 602, &quot;Token de acesso expirado&quot;. Depois disso, usar o mesmo token retornará um 601, &quot;Access token invalid&quot;. Qualquer outro uso de uma string que não seja um token de acesso válido para a assinatura do público-alvo resultará em um 601. Em ambos os casos, esse erro pode ser recuperado do reautenticando e transmitindo o novo token de acesso com uma nova tentativa da solicitação com falha.

### Tempos limite

Em circunstâncias muito raras, uma chamada do pode retornar um 604, &quot;A solicitação expirou&quot;, após o término do período de tempo limite de 30 segundos. Para solicitações em lote, como Criar/Atualizar clientes em potencial, a solicitação pode ser dividida em lotes menores e repetida até que o sucesso seja retornado (se o lote for dividido em menos de 100 registros e a solicitação ainda estiver expirada, você provavelmente deve registrar um caso de suporte). O outro caso mais comum é com chamadas de aprovação de ativos, em que um bloqueio pode ser mantido no registro aprovado atual por outro usuário ou serviço, como o caso de um [Email](https://developer.adobe.com/marketo-apis/api/asset/approve-email-by-id/) ou [Modelo de email](https://developer.adobe.com/marketo-apis/api/asset/approve-email-template-by-id/). Nesses casos, o [backoff exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) deve ser usado para tentativas, para permitir que qualquer bloqueio existente seja resolvido. Nas próximas semanas, verifique a parte final da série, onde analisaremos mais detalhadamente alguns erros específicos não recuperáveis.

Publicado em _2015-10-30_ por _Kenny_

## Aprimoramentos de segurança do Marketo

Na Marketo, levamos a segurança muito a sério. Como parte de uma **[iniciativa em todo o setor](https://security.googleblog.com/2014/09/gradually-sunsetting-sha-1.html)**, a Marketo está atualizando a autenticação e a criptografia da Web para aprimorar nossas proteções de segurança. A implantação está programada para ocorrer em 12 de dezembro de 2011. **Quem será afetado?** Somente um pequeno número de usuários será afetado, somente aqueles que tiverem uma integração com o Marketo de um sistema com mais de dez anos e que não tenha sido atualizado recentemente. Consulte **[esta lista](https://www.digicert.com/faq/sha2/sha-2-compatibility)** para obter mais informações sobre quais sistemas e versões são compatíveis. **Os seguintes usuários não serão afetados:**

* Usuários finais acessando Marketo.com por meio de navegadores modernos ([consulte a lista](https://www.digicert.com/faq/sha2/sha-2-compatibility))
* Clientes que usam parceiros de integração como Informatica, Dell Boomi e Scribe.
* Clientes que usam parceiros Launchpoint.

Todos os outros domínios do Marketo já usam certificados SHA2.

Publicado em _2015-11-18_ por _Kenny_

## Sondagem de atividades usando a API REST

As atividades são um objeto principal na plataforma Marketo. As atividades são os dados comportamentais armazenados sobre cada visita à página da Web, abertura de email, participação no webinário ou participação em feiras. Um caso de uso comum é a combinação de dados de atividade com dados de outras partes de uma organização. Este programa de exemplo contém 6 etapas:

1. Chame [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) para gerar uma lista de todos os registros de atividades criados em uma determinada data/hora. Usamos um filtro para limitar o tipo de registros de atividade retornados.
1. Extrair campos de interesse de cada registro de atividade.
1. Chame [Obter Vários Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) para gerar uma lista de registros de cliente potencial que correspondam às atividades da Etapa 1. Usamos o campo leadId extraído do registro de atividade na Etapa 2 como nosso filtro para especificar quais leads são retornados.
1. Extrair campos de interesse de cada registro de cliente potencial.
1. Una os dados da atividade da etapa 2 com os dados de clientes potenciais da Etapa 4.
1. Transforme os dados da Etapa 5 em um formato que possa ser consumido por um sistema externo.

Como mostra o diagrama abaixo, neste exemplo, optamos por capturar atividades relacionadas a email.

**Entrada de programa** Por padrão, o programa retorna no tempo um dia a partir da data atual para procurar alterações. Assim, você pode executar este programa na mesma hora todos os dias, por exemplo. Para voltar mais no tempo, você pode especificar o número de dias como um argumento de linha de comando, aumentando efetivamente a janela de tempo. O programa contém várias variáveis que podem ser modificadas: CUSTOM_SERVICE_DATA - Contém os dados do Serviço personalizado da Marketo (ID da conta, ID do cliente, Segredo do cliente). READ_BATCH_SIZE - Esse é o número de registros a serem recuperados de cada vez. Use isso para ajustar a resposta ao tamanho do corpo. LEAD_FIELDS - contém uma lista de campos de cliente potencial que queremos coletar. ACTIVITY_TYPES - contém uma lista de tipos de atividades que queremos coletar.

**Lógica do Programa** Primeiro, estabelecemos nossa janela de tempo, compondo nossas URLS de ponto de extremidade REST e obtendo nosso token de acesso de Autenticação. Em seguida, acionamos um loop Obter token de paginação/Obter atividades de lead, que é executado até que o suprimento das atividades seja esgotado. A finalidade desse loop é recuperar registros de atividade e extrair campos de interesse desses registros. Solicitamos que as Atividades com cliente potencial procurem apenas os seguintes tipos de atividades:

* E-mail enviado
* Cancelamento de inscrição do email
* Abertura de email
* Clique em Email.

Extraímos os seguintes campos de interesse de cada registro de atividade:

* leadId
* activityType
* activityDate
* primaryAttributeValue

Você pode selecionar qualquer combinação de tipos de atividade e campos de atividade para atender à sua finalidade. Em seguida, acionamos um loop Obter vários leads por tipo de filtro, que é executado até que o suprimento de leads seja esgotado. Observe que usamos o parâmetro &quot;filterType=id&quot; em combinação com uma série de parâmetros &quot;filterValues&quot; para limitar os registros de lead recuperados apenas aos leads associados às atividades recuperadas anteriormente. Extraímos os seguintes campos de interesse de cada registro de cliente potencial:

* firstName
* lastName
* email

Novamente, você pode selecionar os campos de clientes em potencial que desejar. Em seguida, unimos os campos de cliente potencial aos campos de atividade usando a ID do cliente potencial para vinculá-los. Por fim, percorremos todos os dados em loop, transformamos em formato JSON e os imprimimos no console. **Saída do programa** Este é um exemplo de saída do programa de exemplo. Isso mostra os campos de atividade e os campos de cliente potencial combinados como objetos de &quot;resultado&quot; JSON. A ideia aqui é que você possa passar esse JSON como uma carga de solicitação para um serviço Web externo.

```json
{
    "result": [
        {
            "leadId": 318581,
            "activityType": "Email Delivered",
            "activityDate": "2015-03-17T20:00:06Z",
            "primaryAttributeValue": "My Email Program",
            "firstName": "David",
            "lastName": "Everly",
            "email": "everlyd@marketo.com"
        },
        {
            "leadId":318581,
            "activityType":"Open Email",
            "activityDate":"2015-03-17T23:23:12Z",
            "primaryAttributeValue":"My Email Program - Auto Response",
            "firstName":"David",
            "lastName":"Everly",
            "email":"everlyd@marketo.com"
       },
        ... more result objects here...
    ]
}
```

**Código do Programa**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadActivities {
//
// Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    private static Map<String, String> CUSTOM_SERVICE_DATA;
    static {
        CUSTOM_SERVICE_DATA = new HashMap<String, String>();
//        CUSTOM_SERVICE_DATA.put("accountId", "111-AAA-222");
//        CUSTOM_SERVICE_DATA.put("clientId", "2f4a4435-f6fa-4bd9-3248-098754982345");
//        CUSTOM_SERVICE_DATA.put("clientSecret", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W");
    }

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Lookup lead records using lead id as primary key
    private static final String LEAD_FILTER_TYPE = "id";

    // Lead record lookup returns these fields
    private static final String LEAD_FIELDS = "firstName,lastName,email";

    // Lookup activity records for these activity types
    private static Map<Integer, String> ACTIVITY_TYPES;
    static {
        ACTIVITY_TYPES = new HashMap<Integer, String>();
        ACTIVITY_TYPES.put(7, "Email Delivered");
        ACTIVITY_TYPES.put(9, "Unsubscribe Email");
        ACTIVITY_TYPES.put(10, "Open Email");
        ACTIVITY_TYPES.put(11, "Click Email");
    }

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA.get("accountId"));

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA.get("clientId"), CUSTOM_SERVICE_DATA.get("clientSecret"));

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String activityTypesUrl = String.format("%s/rest/v1/activities/types.json?access_token=%s",
                baseUrl, accessToken);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        // Use activity ids to create filter parameter
        String activityTypeIds = "";
        for (Integer id : ACTIVITY_TYPES.keySet()) {
            activityTypeIds += "&activityTypeIds=" + id.toString();
        }

        // Compose URL for Get Lead Activities
        // Only retrieve activities that match our defined activity types
        String leadActivitiesUrl = String.format("%s/rest/v1/activities.json?access_token=%s%s&batchSize=%s",
                baseUrl, accessToken, activityTypeIds, READ_BATCH_SIZE);

        Map<Integer, List> activitiesMap = new HashMap<Integer, List>();
        Set leadsSet = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {
            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;
            do {
                moreResult = false;

                // Call Get Lead Activities API to retrieve activity data
                JsonObject leadActivitiesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadActivitiesUrl, nextPageToken)));
                if (leadActivitiesObj.get("success").asBoolean()) {
                    moreResult = leadActivitiesObj.get("moreResult").asBoolean();
                    nextPageToken = leadActivitiesObj.get("nextPageToken").asString();

                    if (leadActivitiesObj.get("result") != null) {
                        JsonArray activitiesResultAry = leadActivitiesObj.get("result").asArray();
                        for (JsonValue activitiesResultObj : activitiesResultAry) {

                            // Extract activity fields of interest (leadID, activityType, activityDate, primaryAttributeValue)
                            JsonObject leadObj = new JsonObject();
                            int leadId = activitiesResultObj.asObject().get("leadId").asInt();
                            leadObj.add("activityType", ACTIVITY_TYPES.get(activitiesResultObj.asObject().get("activityTypeId").asInt()));
                            leadObj.add("activityDate", activitiesResultObj.asObject().get("activityDate").asString());
                            leadObj.add("primaryAttributeValue", activitiesResultObj.asObject().get("primaryAttributeValue").asString());

                            // Store JSON containing activity fields in a map using lead id as key
                            List leadLst = activitiesMap.get(leadId);
                            if (leadLst == null) {
                                activitiesMap.put(leadId, new ArrayList());
                                leadLst = activitiesMap.get(leadId);
                            }
                            leadLst.add(leadObj);

                            // Store unique lead ids for use as lead filter value below
                            leadsSet.add(leadId);
                        }
                    }
                }
            } while (moreResult);
        }

        // Use unique lead id values to create filter parameter
        String filterValues = "";
        for (Object object : leadsSet) {
            if (filterValues.length() > 0) {
                filterValues += ",";
            }
            filterValues += String.format("%s", object);
        }

        // Compose Get Multiple Leads by Filter Type URL
        // Only retrieve leads that match the list of lead ids that was accumulated during activity query
        String getMultipleLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=%s&fields=%s&filterValues=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_FILTER_TYPE, LEAD_FIELDS, filterValues, READ_BATCH_SIZE);
        String nextPageToken = "";

        do {
            String gmlUrl = getMultipleLeadsUrl;

            // Append paging token to Get Multiple Leads by Filter Type URL
            if (nextPageToken.length() > 0) {
                gmlUrl = String.format("%s&nextPageToken=%s", getMultipleLeadsUrl, nextPageToken);
            }

            // Call Get Multiple Leads by Filter Type API to retrieve lead data
            JsonObject multipleLeadsObj = JsonObject.readFrom(getData(gmlUrl));
            if (multipleLeadsObj.get("success").asBoolean()) {
                if (multipleLeadsObj.get("result") != null) {
                    JsonArray multipleLeadsResultAry = multipleLeadsObj.get("result").asArray();

                    // Iterate through lead data
                    for (JsonValue leadResultObj : multipleLeadsResultAry) {
                        int leadId = leadResultObj.asObject().get("id").asInt();

                        // Join activity data with lead fields of interest (firstName, lastName, email)
                        List leadLst = activitiesMap.get(leadId);
                        for (JsonObject leadObj : leadLst) {
                            leadObj.add("firstName", leadResultObj.asObject().get("firstName").asString());
                            leadObj.add("lastName", leadResultObj.asObject().get("lastName").asString());
                            leadObj.add("email", leadResultObj.asObject().get("email").asString());
                        }
                    }
                }
            }

            nextPageToken = "";
            if (multipleLeadsObj.asObject().get("nextPageToken") != null) {
                nextPageToken = multipleLeadsObj.get("nextPageToken").asString();
            }
        } while (nextPageToken.length() > 0);

        // Now place activity data into an array of JSON objects
        JsonArray activitiesAry = new JsonArray();
        for (Map.Entry<Integer, List> activity : activitiesMap.entrySet()) {
            int leadId = activity.getKey();
            for (JsonObject leadObj : activity.getValue()) {
                // do something with leadId and each leadObj
                leadObj.add("leadId", leadId);
                activitiesAry.add(leadObj);
            }
        }

        // Print out result objects
        JsonObject result = new JsonObject();
        result.add("result", activitiesAry);
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Pronto. Feliz codificação!

Publicado em _2015-11-20_ por _David_

## Integração do reprodutor SoundCloud à API do Munchkin

O SoundCloud oferece uma incrível plataforma de hospedagem de áudio, com análise e funcionalidade avançadas para tudo, desde aspirantes a atos de indie rock, a artistas de EDM no topo da indústria da música, a podcasts de narrativa. Juntamente com a incrível funcionalidade nativa da plataforma, há um programa de API de alto nível para mover seus dados e rastrear o comportamento de escuta. Isso é especialmente útil para podcasters e pode permitir que você correlacione ações de escuta específicas, como reproduções, pausas e compartilhamentos, a um conteúdo específico no script e no áudio. Hoje vamos dar uma olhada na utilização da [API do widget do SoundCloud](https://developers.soundcloud.com/docs/api/html5-widget) para enviar e rastrear essas atividades no Marketo. Primeiro, vamos analisar a geração de uma atividade do Munchkin que será registrada na Marketo de logon da atividade de um lead. No mais básico, fazemos uma chamada para Munchkin.munchkinFunction e passamos &quot;visitWebPage&quot; como o primeiro argumento. Isso registrará uma atividade Visitas da página da Web com o Marketo e registrará quaisquer dados arbitrários de URL e Cadeia de caracteres de consulta que passarmos para o método. O segundo argumento aceita um objeto JavaScript com nossos dados, que têm dois membros, &quot;url&quot; e &quot;params&quot;, ambas as strings. O membro do url corresponde à Página da Web da atividade no Marketo, enquanto params corresponde à Sequência de consulta. Para nossos propósitos, usaremos o URL como um identificador para ações relacionadas ao SoundCloud, &quot;soundCloudInteraction&quot;, enquanto os parâmetros conterão dados adicionais sobre a atividade específica. Esta é a função que usamos para rastrear cada ação:

```javascript
var trackActivity = function(action){

    //set action param to be the string passed to the function
    var qs = "action=" + action;

    //use getCurrentSound callback to get the name of the current track
    soundCloudMunchkin.widget.getCurrentSound(function(currentSound){

        //add it to our querystring
        qs = qs + "&sound=" + currentSound.title;

        //use the getPosition callback to get the position of the track in ms
        soundCloudMunchkin.widget.getPosition(function(position){

            //add it to the querystring
            qs = qs + "&position=" + position;

            //assemble our data object for the munchkin activity
            var dataObject = {
                "url": "soundCloudInteraction",
                "params": qs
            }

            //call the munchkinFunction to submit the activity
            Munchkin.munchkinFunction("visitWebPage", dataObject);
        });
    });
}
```

Como o widget padrão SoundCloud está incorporado em um iframe, o widget usa mensagens de publicação para se comunicar e os retornos de chamada precisam ser usados para obter dados, como é possível ver com os métodos currentSound e getPosition. A API do widget SoundCloud fornece um conjunto de retornos de chamada do JavaScript que podemos usar para responder a eventos individuais no reprodutor e enviá-los para o Marketo. Os eventos em que estamos interessados são o que o usuário escuta, quanto tempo o usuário escuta e interações que ele faz com o reprodutor, portanto, estamos olhando para os seguintes eventos:

* PLAY
* PAUSAR
* CONCLUIR
* BUSCAR
* CLICK_DOWNLOAD
* CLICK_BUY
* OPEN_SHARE_PANEL

Também precisaremos usar o método bind() do widget para adicionar retornos de chamada a cada um desses eventos. Vamos analisar um exemplo:

```javascript
widget.bind(SC.Widget.Events.PLAY, function(){
    soundCloudMunchkin.trackPlay();
});
```

Assim, sempre que uma faixa for reproduzida, acionaremos o método trackPlay para enviar um evento à Marketo com dados sobre a faixa atual. Você pode encontrar o script completo [aqui](https://gist.github.com/kelkingtron/6750bb07c1397d93d9c7#file-soundcloudmunchkin-js). O objeto soundCloudMunchkin tem um método init, que aceita um objeto de widget SoundCloud como seu único argumento, que vincula os métodos de rastreamento aos retornos de chamada relevantes, e configurará seu widget para rastrear atividade no Marketo. Será necessário carregar o [código Munchkin](/help/javascript-api/lead-tracking.md) da sua página, bem como a [biblioteca de API SoundCloud](https://w.soundcloud.com/player/api.js). Você também precisará inicializar tudo, além de incorporar o widget real do SoundCloud:

```javascript
window.onload=function(){
var iframe = document.getElementById(iframeId);
    if(iframe) {
        widget = SC.Widget(iframe);
        soundCloudMunchkin.init(widget);
    };
};
```

Publicado em _2015-12-21_ por _Kenny_

## A RTP e a Diretiva Privacidade Eletrônica da UE

Esta publicação explica como você pode usar o RTP para notificar os visitantes do site de que estão sendo rastreados ou desativar automaticamente o rastreamento para visitantes europeus. Desde 2012, qualquer sítio Web disponível para visitantes europeus tem de cumprir a Diretiva Privacidade Eletrônica da UE. Novas leis entraram em vigor em 2011, que impedem que informações de identificação sejam armazenadas no computador de um usuário sem o seu conhecimento e consentimento. Se você estiver usando cookies ou qualquer outra tecnologia para um rastreamento não essencial, é necessário:

1. Informe aos usuários que as tecnologias de rastreamento são usadas.
1. Explique os motivos para o uso dessas tecnologias.
1. Obtenha o consentimento do usuário antes de usar essa tecnologia e permita que ele retire a permissão a qualquer momento.

Publicado em _1970-01-01_ por _Yanir_

## Atualização de informações de clientes e clientes potenciais no Marketo usando o AP

Há cenários em que sistemas proprietários são usados para atualizar informações de clientes atuais e potenciais. A equipe de marketing gostaria que essas atualizações fossem refletidas no Marketo para que tivessem o sistema de registro mais preciso para usar em suas campanhas de marketing. Usando a abordagem abaixo, você pode configurar uploads periódicos para o Marketo a fim de manter suas informações de contato do Marketo atualizadas com os dados modificados nesse sistema proprietário. O diagrama abaixo mostra as chamadas de API feitas em um temporizador periódico definido. À medida que o temporizador periódico é acionado, a lógica do cliente recuperará primeiro os contatos atualizados do sistema proprietário. A forma como isso é feito difere de sistema para sistema usando APIs ou exportações de dados do sistema proprietário. Detalharemos as APIs do Marketo que são executadas assim que as informações de contato atualizadas são recuperadas. Solicitação SOAP para syncMultipleLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadRecordList>
    <leadRecord>
      <Email>henry@superstar.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Henry</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Adams</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>Director of Demand Generation</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
    <leadRecord>
      <Email>ssmith@gmail.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Suzie</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Smith</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>VP Marketing</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
  </leadRecordList>
  <dedupEnabled>true</dedupEnabled>
</ns2:paramsSyncMultipleLeads>
```

Resposta do SOAP do syncMultiLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
    <syncStatusList>
      <syncStatus>
        <leadId>1094593</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
      <syncStatus>
        <leadId>1094594</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
    </syncStatusList>
  </result>
</ns2:successSyncMultipleLeads>
```

`syncMultipleLeads` executa uma operação UPSERT. Se um contato no Marketo já existir com base no endereço de email enviado, os atributos serão atualizados. Se um contato não existir, ele será criado. A resposta de `syncMultipleLeads` retorna o status para cada um dos contatos enviados. Os valores `<attrName/>` em `<leadAttributeList/>` devem corresponder ao Nome da API do SOAP definido para essa assinatura do Marketo. Você pode descobrir os Nomes de API do SOAP na seção de gerenciamento de campo no painel de administração do Marketo exportando os nomes de campo.

Consulte a amostra de programa Java abaixo que executa o cenário descrito acima:

```java
 import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import java.util.\*;

public class SyncMultipleLeadsExample {

  public static void main(String[] args) {

    try {
      URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
      String marketoUserId = "CHANGE ME";
      String marketoSecretKey = "CHANGE ME";

      QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
      MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
      MktowsPort port = service.getMktowsApiSoapPort();

      // Create Signature
      DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
      String text = df.format(new Date());
      String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
      String encryptString = requestTimestamp + marketoUserId ;

      SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
      Mac mac = Mac.getInstance("HmacSHA1");
      mac.init(secretKey);
      byte[] rawHmac = mac.doFinal(encryptString.getBytes());
      char[] hexChars = Hex.encodeHex(rawHmac);
      String signature = new String(hexChars);

      // Set Authentication Header
      AuthenticationHeader header = new AuthenticationHeader();
      header.setMktowsUserId(marketoUserId);
      header.setRequestTimestamp(requestTimestamp);
      header.setRequestSignature(signature);

      // Create Request
      ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

      ObjectFactory objectFactory = new ObjectFactory();

      JAXBElement dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
      request.setDedupEnabled(dedup);

      // The list of contacts defined here would be retrieved from the proprietary system
      Contact contact = new Contact("Henry","Adams","henry@superstar.com", "Director of Demand Generation");
      Contact contact2 = new Contact("Suzie","Smith","ssmith@gmail.com", "VP Marketing");

      ArrayList updatedContacts = new ArrayList();
      updatedContacts.add(contact);
      updatedContacts.add(contact2);

      ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

      Iterator it = updatedContacts.iterator();
      while(it.hasNext())
      {
        Contact c = it.next();

        LeadRecord leadRec = new LeadRecord();

        JAXBElement email = objectFactory.createLeadRecordEmail(c.email);
        leadRec.setEmail(email);

        Attribute attr1 = new Attribute();
        attr1.setAttrName("FirstName");
        attr1.setAttrValue(c.fname);

        Attribute attr2 = new Attribute();
        attr2.setAttrName("LastName");
        attr2.setAttrValue(c.lname);

        Attribute attr3 = new Attribute();
        attr3.setAttrName("Title");
        attr3.setAttrValue(c.title);

        ArrayOfAttribute aoa = new ArrayOfAttribute();
        aoa.getAttributes().add(attr1);
        aoa.getAttributes().add(attr2);
        aoa.getAttributes().add(attr3);

        QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
        JAXBElement attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

        leadRec.setLeadAttributeList(attrList);
        arrayOfLeadRecords.getLeadRecords().add(leadRec);

      }

      request.setLeadRecordList(arrayOfLeadRecords);

      JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
      Marshaller m = context.createMarshaller();
      m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
      m.marshal(request, System.out);

      SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

      m.marshal(result, System.out);
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }

  public static class Contact {
    public String fname, lname, email, title;

      public Contact(String fname, String lname, String email, String title) {
          this.fname = fname;
          this.lname = lname;
          this.email = email;
          this.title = title;
      }
  }
}
```

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-03-24_ por _Travis Kaufman_

## Envio de um email transacional do Marketo usando a API

Ela requer que uma Campanha inteligente existente seja criada usando a interface do usuário do Marketo. Ele também exige que o destinatário do email exista no Marketo. Portanto, antes de chamar a API requestCampaign, use a [API getLead]&#x200B;(/help/soap-api/getlead.md) para verificar se o email existe no Marketo. Depois de fazer uma chamada por meio da API requestCampaign, é possível confirmá-la verificando se a Campanha inteligente foi executada no Marketo. Primeiro, mostramos como criar uma Campanha inteligente, em segundo lugar, como configurar um acionador para enviar uma campanha por meio da API, em terceiro, como definir um email como parte de uma ação de fluxo e, em quarto lugar, uma amostra de código que seria usada para executar essa campanha.
**Como criar uma nova campanha inteligente no Marketo** As campanhas inteligentes no Marketo executam todas as suas atividades de marketing. Você pode configurar uma série de ações automatizadas para executar uma lista inteligente de contatos. No caso de enviar emails transacionais, você configura um acionador na campanha, como mostrado abaixo, para enviar emails usando a API. Primeiro, vamos configurar a Campanha inteligente. 1. Em Atividades de marketing, escolha um Programa e, na lista suspensa Novo, clique em Novo ativo local.

1. Clique em Campanha inteligente
1. Insira um nome de campanha inteligente e clique em Criar

**Adicionar acionadores a uma campanha inteligente** Adicionar acionadores a uma campanha inteligente permite que uma campanha inteligente seja executada em uma pessoa de cada vez com base em um evento ao vivo, que neste caso é uma solicitação por meio da [API requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST). 1. Procure o acionador &quot;Campanha solicitada&quot; e arraste e solte-o na tela.

1. No acionador, selecione &quot;é&quot; e &quot;API do serviço da Web&quot;.

**Como criar uma ação de fluxo de email em uma campanha** A associação de um email a uma campanha inteligente permite que os profissionais de marketing gerenciem a aparência desejada de um email e permite que o aplicativo de terceiros determine quem o receberá e quando. Depois de criar um email como um novo Ativo local, você pode defini-lo como uma ação de fluxo em uma campanha.  Localize e selecione o email que deseja enviar.

**Amostra de código para chamar a API requestCampaign** Depois de configurar a campanha e os acionadores na interface do Marketo, mostramos como usar a API para enviar um email. O primeiro exemplo é uma solicitação XML, o segundo é uma resposta XML e o último é uma amostra de código Java que pode ser usada para gerar a solicitação XML. Também mostramos como encontrar a ID da campanha usada ao chamar a API `requestCampaign`.
A chamada de API também exige que você saiba a ID da campanha do Marketo antecipadamente. Você pode determinar a ID da campanha usando um dos seguintes métodos: 1. Use a API 1 [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md). Abra a campanha do Marketo em um navegador e observe a barra de endereços do URL. A ID da campanha (representada como um inteiro de 4 dígitos) pode ser encontrada imediatamente após &quot;SC&quot;. Por exemplo, `<https://app-stage.marketo.com/#SC**1025**A1>`. A parte em negrito é a ID da campanha - &quot;1025&quot;. Solicitação SOAP para `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Resposta do SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Veja abaixo um exemplo de programa Java que executa o cenário descrito acima.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-03-27_ por _Murta_

## Envio de um email com conteúdo dinâmico do Marketo usando o AP-

Imagine que você queira automatizar os emails de acompanhamento da central de atendimento. Depois que o representante do suporte falar com um cliente, você gostaria de enviar automaticamente um email agradecendo por entrar em contato com a sua empresa. Vamos dar um passo além e digamos que você queira incluir o tópico específico de conversa discutido com o cliente que você acompanha em seu CRM. Você pode fazer isso no Marketo usando a API do SOAP requestCampaign para enviar um email com conteúdo dinâmico. A API requestCampaign permite que você passe um ou mais leads. Também permite que você passe Tokens de programa que podem ser usados com uma campanha existente para enviar conteúdo dinâmico. A API do SOAP requestCampaign exige que o destinatário do email exista no Marketo. Portanto, antes de chamar a API requestCampaign, use a [API getLead](/help/soap-api/getlead.md) para verificar se o email existe no Marketo. Primeiro, mostramos como criar uma Campanha inteligente, em segundo lugar, como configurar um acionador para enviar uma campanha por meio da API, em terceiro, como criar um email que aceite conteúdo dinâmico por meio de tokens de programa, em quarto, como definir um email como parte de uma ação de fluxo e, em quinto, uma amostra de código que seria usada para executar essa campanha. **Como criar uma nova campanha inteligente no Marketo** As campanhas inteligentes no Marketo executam todas as suas atividades de marketing. Você pode configurar uma série de ações automatizadas para executar uma lista inteligente de contatos. No caso de enviar emails transacionais, você configura um acionador na campanha, como mostrado abaixo, para enviar emails usando a API. Primeiro, vamos configurar a Campanha inteligente. 1. Em Atividades de marketing, escolha um Programa e, na lista suspensa Novo, clique em Novo ativo local

1. Clique em Campanha inteligente
1. Insira o nome da campanha inteligente e clique em Criar **Adicionar acionadores a uma campanha inteligente** Adicionar acionadores a uma campanha inteligente permite que uma campanha inteligente seja executada em uma pessoa de cada vez com base em um evento ao vivo, que, neste caso, é uma solicitação por meio da [API requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST).
1. Procure o acionador &quot;A campanha é solicitada&quot; e arraste e solte-o na tela.
1. No acionador, selecione &quot;é&quot; e &quot;API do serviço da Web&quot;.

**Como transmitir conteúdo dinâmico usando a API** No Marketo, Meus tokens são variáveis que podem ser usadas no seu programa. Meus tokens permitem que você insira informações relacionadas ao seu programa em um local, substitua essas informações por um valor especificado e recupere essas informações em outras partes do aplicativo, como um modelo de email. Usando a API requestCampaign do SOAP, você pode passar uma matriz de Tokens de programa, que substituirão os tokens existentes. Após a execução da campanha, os tokens são descartados. Você cria Meus tokens no nível da pasta Campanha ou no nível do Programa. Meus tokens no nível da pasta do Campaign serão herdados para todos os Programas contidos na pasta do Campaign. Se você criar Meus tokens no nível da pasta do Campaign, poderá substituir o valor herdado no nível do Programa. Por exemplo, se você definir tokens para a Data do programa e a Descrição do programa no nível da pasta Campanha, será possível substituir esses valores no nível do programa individual.

Veja como fazer isso. 1. Na árvore de Atividades de marketing, selecione a pasta Campanha ou Programa em que deseja criar os tokens. Na barra de menu superior, selecione Meus tokens. Em seguida, a tela Meus tokens é exibida. Na árvore do lado direito, arraste um Tipo de token para a tela, que nesse caso é &quot;Texto&quot;. No campo Nome do token, destaque Meu token e insira um Nome de token exclusivo, que neste caso é &quot;my.conversationtopic&quot;. No campo Value, insira um Value relevante para o token, que neste caso é &quot;Thank you for calls us today&quot;. Observe que ao usar a API, substituiremos o valor padrão Meu token. Clique em &quot;Salvar&quot; para salvar o token personalizado.  1. Crie um novo email clicando em Novo. Em seguida, clique em Novo Assets local e selecione Email. Em seguida, preencha os campos relevantes para nomear o email. Ao redigir o email, clique no ícone de Token para incluir tokens no email. Agora que você criou o email de modelo com Tokens, adicionaremos o email como uma ação de fluxo para a Campanha na etapa subsequente. Assim, quando você chama a campanha pela API, o email é enviado.
**Como criar uma ação de fluxo de email em uma campanha** A associação de um email a uma campanha inteligente permite que os profissionais de marketing gerenciem a aparência desejada de um email e permite que o aplicativo de terceiros determine quem o receberá e quando. Depois de criar um email como um novo Ativo local, você pode defini-lo como uma ação de fluxo em uma campanha. Localize e selecione o email que deseja enviar.
**Amostra de código para chamar a API requestCampaign** Depois de configurar a campanha e os acionadores na interface do Marketo, mostramos como usar a API para enviar um email. O primeiro exemplo é uma solicitação XML, o segundo é uma resposta XML e o último é uma amostra de código Java que pode ser usada para gerar a solicitação XML. Também mostramos como encontrar a ID da campanha usada ao chamar a API requestCampaign. A chamada de API também exige que você saiba a ID da campanha do Marketo antecipadamente. Você pode determinar a ID da campanha usando um dos seguintes métodos: 1. Use a API 1 [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md). Abra a campanha do Marketo em um navegador e observe a barra de endereços do URL. A ID da campanha (representada como um inteiro de 4 dígitos) pode ser encontrada imediatamente após &quot;SC&quot;. Por exemplo, `<https://app-stage.marketo.com/#SC&#x200B;**1025**&#x200B;A1>`. A parte em negrito é a ID da campanha - &quot;1025&quot;. Solicitação SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
      </leadList>
      <programTokenList>
        <attrib>
          <name>{{my.conversationtopic}}</name>
          <value>Thank you for calling about adding a line of service to your current plan.</value>
        </attrib>
      </programTokenList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Resposta do SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Veja abaixo um exemplo de programa Java que executa o cenário descrito acima.

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            leadKeyList.getLeadKeies().add(key);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            ArrayOfAttrib aoa = new ArrayOfAttrib();

            Attrib attrib = new Attrib();
            attrib.setName("{{my.conversationtopic}}");
            attrib.setValue("Thank you for calling about adding a line of service to your current plan.");

            aoa.getAttribs().add(attrib);

            JAXBElement<ArrayOfAttrib> arrayOfAttrib = objectFactory.createParamsRequestCampaignProgramTokenList(aoa);
            request.setProgramTokenList(arrayOfAttrib);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Depois de fazer uma chamada por meio da API requestCampaign, é possível confirmá-la verificando se a Campanha inteligente foi executada no Marketo.

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-04-03_ por _Murta_

## Capturar atividade de visitante anônimo com base na lógica de negócios

Imagine que você queira rastrear os usuários que visitam uma publicação específica no blog de sua empresa. Digamos que, do número total de usuários que visitam uma publicação, você só deseje rastrear os usuários que sinalizam interesse gastando pelo menos 5 segundos e rolando pela página. Para usuários anônimos, você gostaria de criar um novo lead no Marketo com este evento e, para usuários conhecidos, gostaria de atualizar sua atividade de lead com este evento. Você pode fazer isso usando o [código de rastreamento do Munchkin](/help/javascript-api/lead-tracking.md) em seu site. Quando um usuário sem cookies acessa uma página com o código de rastreamento do Munchkin, um novo cookie é criado no navegador do usuário e um novo lead anônimo é criado no Marketo. Se o usuário já estiver com cookies e for um cliente potencial existente no Marketo, a visita à página será registrada no log de atividades do usuário no Marketo. Mostramos primeiro como gerar o código de rastreamento do Munchkin no Marketo, segundo como modificar seu código de amostra do Munchkin para acionar somente se determinadas condições forem atendidas e terceiro como verificar se uma visita de página de um usuário anônimo foi registrada no Marketo.

**Como gerar o código de rastreamento do Munchkin** O código de rastreamento do Munchkin permite rastrear visitas ao seu site. Há três tipos de código Munchkin descritos abaixo, mas neste exemplo usamos o código de rastreamento assíncrono do Munchkin. A) Simples: tem menos linhas de código, mas não otimiza o tempo de carregamento da página da Web. Este código carrega a biblioteca jQuery sempre que uma página da Web é carregada. B) Assíncrono: reduz o tempo de carregamento da página da Web. Esse código verifica se a biblioteca jQuery já existe, carrega se estiver ausente e a usa para executar o código de rastreamento depois que o restante da página da Web for carregada. C) Asynchronous jQuery: reduz o tempo de carregamento da página da Web e melhora o desempenho do sistema. Esse código pressupõe que você já tenha o jQuery e não verifica se deve carregá-lo. 1. Clique em Administrador na parte superior direita do aplicativo.  1. Clique em Munchkin na árvore à esquerda.  1. Selecione Assíncrono para Tipo de código de rastreamento. 1. Clique em e copie o código de rastreamento do JavaScript para colocar em seu site.
**Amostra de código para Usuário de cookie e Evento de rastreamento** Coloque o código de rastreamento nas suas páginas da Web logo antes da marca `</body>`. As landing pages criadas no Marketo contêm automaticamente um código de rastreamento, portanto, não é necessário colocar esse código nelas. Este exemplo de código chamaria a API do Munchkin depois que o script fosse carregado:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Essa amostra de código chamaria a API do Munchkin depois que o usuário estivesse na página por 5 segundos e também rolasse 500 pixels para baixo na página:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Como verificar se a visita de página por usuário anônimo foi registrada no Marketo**

1. Clique em Analytics no menu superior e, em seguida, clique em Novo relatório. Escolha Atividade da página da Web como o tipo de relatório e nomeie o relatório.
1. Depois de criar um relatório, clique em Smart List. Em seguida, selecione o filtro Página da Web visitada na caixa à direita. Insira a página da Web onde você coloca o código de rastreamento do Munchkin.
1. Clique em Configurar. Selecione Visitantes anônimos em ISPs e altere a opção para Mostrado.
1. Clique em Relatório. Agora você verá a atividade rastreada na página da Web selecionada.
1. Clique duas vezes no registro de lead, que mostrará o log de atividades, onde é possível ver a página específica que o usuário anônimo visitou.

Este artigo contém o código usado para implementar integrações personalizadas. Devido à sua natureza personalizada, a equipe de suporte técnico da Marketo não pode solucionar problemas de trabalho personalizado. Não tente implementar a amostra de código a seguir sem experiência técnica adequada ou acesso a um desenvolvedor experiente.

Publicado em _2014-04-17_ por _Murta_

## Alterar Dinamicamente o Número de Telefone Local usando RTP

O Personalization é tudo - descobrimos isso há muito tempo. Dito isso, ainda é surpreendente para mim que toda vez que eu preciso de assistência imediata, é tão difícil encontrar os números de telefone locais relevantes em um site. Ainda bem que temos o [Marketo Real-Time Personalization](https://business.adobe.com/products/marketo/content-personalization.html) (RTP) instalado em <https://business.adobe.com/products/marketo/adobe-marketo.html>. Podemos aproveitar a [API de Visitante RTP](/help/javascript-api/web-personalization.md) para alterar dinamicamente o número de telefone que um visitante da Web vê em diferentes seções do site. Uau! Dá para acreditar nisso? Como essa magia funciona? Primeiro, você precisa ter o RTP instalado no seu site, conforme descrito [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript). Em seguida, siga as instruções abaixo e implemente o código do JavaScript em seu site:

1. Insira seu número de telefone internacional na configuração do **defaultPhone**
1. Insira as IDs de elemento do HTML na configuração **divIds**
1. Se você quiser transformar o número de telefone em um link de clique para chamar em navegadores móveis, defina a configuração **mobileLink** como **true**.
1. Mapeie os diferentes locais com seus números de telefone nas configurações **cityPhone**, **statePhone** e **countryPhone**

Por exemplo, estes são exemplos de valores para as definições de configuração:

```json
  defaultPhone:"+1.503.608.4679", // Optional
  divIds:["phoneId1","phoneId2"],
  mobileLink: true,
  cityPhone: {
    "<a href="#">yanir</a>": ["San Mateo", "San Francisco"],
    "+353.1.242.3000": ["tel-aviv"]
  },
  statePhone: {
    "+1.650.376.2300": ["CA"],
    "+1.650.376.2302": ["OR"]
  },
  countryPhone: {
    "+1.650.376.2300": ["United States"],
    "+353.1.242.3000": ["Israel"]
  }
```

Finalmente, insira uma marca de âncora do HTML que contenha uma ID correspondente a uma das IDs em **divIds** (da etapa 2 acima). Por exemplo, se você tiver especificado &quot;phoneId1&quot; em **divIds**, sua marca de âncora do HTML terá esta aparência:

`<a href="tel:+1800229933" id="phoneId1">+1800229933</a>`

O script verifica se há uma correspondência nesta ordem: cityPhone > statePhone > countryPhone > defaultPhone Você também pode substituir os números de telefone por texto (Exemplo: &quot;Entre no nosso grupo de usuários de São Francisco!&quot;) ou código HTML e alterar dinamicamente o conteúdo com base na localização geográfica. Aproveite!

```html
<a href="tel:+1800229933" id="phoneId1">+1800229933</a>
<script>
(function(a){
    rtp('get','visitor',function(yc){
        var location = yc.results.location;
        var loop = true;
        var phoneChanged = false;
        console.log(yc.results);
        function checkObj(obj){
            return Object.getOwnPropertyNames(obj).length >0;
        }
        function changePhone(p){
            d=a.divIds;
            for(i=0;i<d.length;i++){
                if(document.getElementById(d[i]) !== null){
                    document.getElementById(d[i]).innerHTML = p;
                    if(a.mobileLink){
                        document.getElementById(d[i]).href= "tel:" + p;
                    }
                    console.log(p);
                }
            }
            loop = false;
            phoneChanged = true;
        }
        function matchLocation(loc,objc){
            for (var key in objc) {
                for(i=0;i<objc[key].length && loop;i++){
                    if (!loop) { return true;};
                    val = objc[key][i];
                    //console.log(loc + location[loc] + " ? " + val);
                    if(location[loc].toLowerCase() === val.toLowerCase()){
                        changePhone(key);
                    }
                }
            }
        }
        if(checkObj(a.cityPhone)){
            matchLocation("city",a.cityPhone);
        }else if(checkObj(a.statePhone)){
            matchLocation("state",a.statePhone);
        }else if(checkObj(a.countryPhone)){
            matchLocation("country", a.countryPhone);
        }else if(!phoneChanged && a.defaultPhone.length > 0 ){
            changePhone(a.divId,a.defaultPhone);
        }
    });
})({
    defaultPhone:"",  //  [Optional] the number to show if visitor does not match the mapping below
    divIds:["phoneId1","Floater"],  //the phone HTML element ID, can be <div>, <a>, <span>, <p> etc.
    mobileLink: true,  //if you use click to call link (with href="tel:") you can also change its number

    cityPhone: {
        "<a href='#'>yanir</a>": ["San Mateo", "San Francisco"],
        "+353.1.242.3000": ["tel-aviv"]
    },
    statePhone: {
        "+1.650.376.2300": ["CA"],
        "+1.650.376.2302": ["OR"]
    },
    countryPhone: {
        "+1.650.376.2300": ["United States"],
        "+353.1.242.3000": ["Israel"]
    }
});
</script>
```

Publicado em _2016-02-02_ por _Yanir_

## Atualizações do inverno de 2016

### Objetos personalizados

* [Relacionamentos de Objetos Personalizados N:N agora com suporte](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)
   * Os registros de cliente potencial ou de Conta agora podem ter relações muitos para muitos por meio de objetos personalizados através da definição de objetos intermediários. Após criar um tipo de objeto personalizado independente e um tipo de objeto intermediário podem ser criados com campos de link para o objeto independente e clientes potenciais ou contas.
   * Não há novas chamadas de API para esse recurso, mas as definições de objeto devem ser configuradas corretamente para aproveitar esses relacionamentos por meio da API.
* `getLeadActivities` e `getLeadChanges` não retornarão mais atividades de clientes potenciais anônimos. Consulte as [Perguntas frequentes sobre o Rastreamento de Munchkin de última geração](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) para obter mais informações

Publicado em _2016-02-05_ por _Kenny_

## Recuperar atividades para um único cliente potencial usando a API REST

Esta é uma pergunta que nos é feita repetidamente pela nossa comunidade de desenvolvedores:

&quot;Como obter uma lista de atividades anteriores para um cliente potencial individual?&quot;

Até recentemente, não havia uma maneira direta de fazer isso usando a REST API. Mas agora há! A versão do Winter 2016 da nossa REST API contém um pequeno aprimoramento. [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) agora aceita o parâmetro **leadIds** que pode ser usado para especificar uma ID de cliente potencial. Quando o parâmetro **leadIds** for especificado, somente as atividades para essa ID de cliente potencial serão retornadas. Você pode pensar nisso como um filtro de ID de lead. Observe que o parâmetro **leadIds** pode usar uma lista separada por vírgulas de ids de leads caso você deseje filtrar resultados em mais de um lead (até 30). Isso pode ser útil, por exemplo, ao limitar atividades a clientes potenciais para uma empresa específica. **Exemplo** Abaixo está uma solicitação de exemplo para [Obter Atividades de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) que contém o parâmetro **leadIds**. Especifiquei um valor &quot;50&quot; para o parâmetro **leadIds**, que corresponde a um lead arbitrário na minha instância do Marketo. Especifiquei um valor &quot;129&quot; para o parâmetro activityTypeIds, que corresponde à atividade &quot;Sessão de aplicativo móvel&quot; na minha instância do Marketo.

`<https://123-abc-456.mktorest.com/rest/v1/activities.json?leadIds=50&activityTypeIds=129&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3J4SMAZRQO4RKIXCEMLFCM2APRSQ====>`

Veja abaixo um trecho da resposta dessa solicitação. Como você pode ver, ele contém apenas objetos de resultado com &quot;leadId&quot;: 50 e &quot;activityTypeId&quot;: 129.

```json
{
    "id": 846,
    "leadId": 50,
    "activityDate": "2015-04-06T21:58:59Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 7
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 879,
    "leadId": 50,
    "activityDate": "2015-04-07T00:45:11Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 5
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1114,
    "leadId": 50,
    "activityDate": "2015-04-08T00:02:41Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 241
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1551,
    "leadId": 50,
    "activityDate": "2015-04-09T23:31:56Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1716,
    "leadId": 50,
    "activityDate": "2015-04-15T22:44:19Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
```

## Integração contínua com o Marketo e mais de 500 aplicativos usando o Zapier

Este é um post de Philippe Delle Case, Consultor Principal de Soluções, Marketo.

### Objetivos

Este artigo explica em detalhes como integrar o Marketo com potencialmente mais de 500 aplicativos na nuvem, graças ao Zapier. Para isso, vamos criar do zero um conector Zapier para o Marketo e implementar dois casos de uso de integração prática: Caso de uso 1: integração de leads unidirecionais do FullContact Card Reader para o Marketo

* Digitalize o cartão de visita de qualquer contato com o aplicativo FullContact Mobile Card Reader e obtenha um cliente potencial criado automaticamente no Marketo.
* Adicionar um cliente potencial existente a uma lista estática no Marketo e localizar o cliente potencial adicionado automaticamente à Planilha do Google.
* Modifique qualquer lead em sua Planilha do Google e localize a alteração repercutida no Marketo.

### Pré-requisitos

**Inscreva-se para obter uma conta gratuita com o Zapier** [Zapier](https://zapier.com/) é um Serviço de Automação de aplicativos Web que permite automatizar facilmente tarefas entre outros aplicativos online sem a necessidade de programadores ou recursos de TI. Uma breve introdução pode ser encontrada [aqui](https://zapier.com/api/v4/helpdocs/category/redirect/what-is-zapier). Hoje, o Zapier oferece suporte a mais de 500 aplicativos em vários domínios diferentes, como Marketing, CRM, CMS, Suporte ao cliente, Assinatura eletrônica, Forms etc. Uma única integração entre um aplicativo e outro é chamada de Zap. Consulte o [zapbook](https://zapier.com/apps) do Zapier para obter uma lista completa de aplicativos Web compatíveis.  Cadastre-se para obter uma conta gratuita [aqui](https://zapier.com/sign-up/). Você tem acesso a até 100 tarefas/mês, 5 zaps e zaps em execução a cada 15 minutos. É claro que você pode obter muito mais, assinando os planos pagos da Zapier (básico, negócios, negócios, etc.)

**Acesso a uma instância do Marketo como administrador ou com uma conta de usuário da API fornecida** Nosso conector Zapier usará a API REST do Marketo para enviar dados de clientes potenciais para o Marketo. Para usar essa API, você precisa de um Usuário da API e um Serviço personalizado que você possa criar se for o administrador da sua instância do Marketo. Caso contrário, um administrador precisará fornecê-las a você. Também há um Webhook para criar, acessível somente a um Administrador do Marketo. Uma explicação passo a passo de como criar o Usuário da API Marketo e o Serviço Personalizado pode ser encontrada [aqui](http://rest-api/custom-services/). Quando terminar, você deve ter as seguintes credenciais para chamar a API REST do Marketo: ID do cliente, Segredo do cliente, ID da conta do Munchkin, ID da conta do Munchkin

É possível obter a ID da conta da Munchkin nas telas Munchkin ou Administrador dos serviços da Web. Seu padrão é semelhante a: `000-XXX-000`.  Não é necessário obter um token de acesso, pois ele seria válido apenas por uma hora. O conector gerará tokens automaticamente para você.
**Cadastre-se para ter uma conta gratuita com o Google Docs, Sheets e Slides são aplicativos de produtividade que permitem criar diferentes tipos de documentos online, trabalhar neles em tempo real com outras pessoas e armazená-los no Google Drive online. Nosso caso de uso precisa de uma folha do Google. Diferentes recursos do Google Docs e a criação de uma conta com o Google podem ser encontrados [aqui](https://workspace.google.com/products/docs/).
**Inscreva-se para obter uma conta gratuita com FullContact** O FullContact mantém você totalmente conectado às pessoas mais importantes, puxando todos os seus contatos e sincronizando-os continuamente com alterações em perfis sociais, fotos, assinaturas por email, informações da empresa e muito mais. Eles oferecem um leitor de cartões de visita móveis que pode digitalizar cartões em mais de 250 aplicativos Web, incluindo Zapier. Você pode se inscrever para obter uma conta gratuita [aqui](https://app.fullcontact.com/login). Você também pode assinar uma conta paga premium com mais recursos e capacidade. O aplicativo móvel pode ser baixado da [Apple AppStore](https://apps.apple.com/us/app/fullcontact-business-card/id576780807) ou da [Google Play](https://play.google.com/store/apps/details?id=com.fullcontact.cardreader). Os Zaps FullContact estão documentados [aqui](https://zapier.com/apps/contacts-plus/integrations).

### Implementação do conector do Marketo para Zapier

**Crie o aplicativo Marketo** Na interface da Web do Zapier, vá para o Portal de Desenvolvedores. Clique em **Adicionar novo aplicativo** e preencha pelo menos o Título (por exemplo, &#39;Marketo&#39;) e a Descrição. O logotipo é opcional, mas é bom ter.\ **Autenticação** Nesta seção, declaramos os diferentes campos usados para a autenticação da API REST do Marketo e as configurações de autenticação. Crie primeiro os seguintes campos:

Edite as &quot;Configurações de autenticação&quot;:

* Tipo de autenticação: autenticação da sessão
* Mapeamento de autenticação:

  `{"access_token":"{{access_token}}"}`

* Posicionamento do token de acesso&#x200B;**:** token na sequência de consulta

Depois que um serviço personalizado do Marketo é criado, a ID do cliente e a senha do cliente são disponibilizadas. Usamos a ID do cliente e o segredo do cliente para gerar um token de acesso por meio do ponto de extremidade REST API [Authentication](/help/rest-api/authentication.md). Podemos então usar esse token de acesso para fazer solicitações subsequentes à API REST. O token expira após uma hora e deve ser gerado novamente para continuar chamando a API REST. Escolhemos tipo de autenticação = &#39;Autenticação de sessão&#39;, pois ela nos permite executar um script de autenticação personalizado sempre que o token de sessão expirar. Veremos na seção &quot;API de script&quot; como implementar esse mecanismo que só pode funcionar com esse tipo de autenticação.
**Triggers** Os acionadores do Zapier estão lá para trazer dados para o Zapier. Não precisamos de um para nossos casos de uso, pois utilizaremos um Webhook do Marketo. No entanto, ainda precisamos gravar um Acionador fictício como um teste obrigatório para nosso conector do Marketo. Vamos criar um Acionador de Teste chamando o ponto de extremidade REST API do Marketo [Obter Uso Diário](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyUsageUsingGET). Clique em **Adicionar novo acionador** para iniciar o assistente e preencher os seguintes campos (os campos não mencionados podem ser deixados em branco): Nome e Descrição

* Nome: Acionador de teste
* Chave: test_trigger
* Descrição: O acionador de teste do aplicativo Marketo
* Importante? Não marcado
* Ocultar? Marcado(s)

Campos de acionamento

* None

De onde vêm os dados

* Data Source: pesquisa
* URL de sondagem: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/stats/usage.json`

Exemplo de resultado

* Deixe em branco

Clique em **Gerenciar Configurações do Acionador** e defina nosso Acionador de Teste como aquele que usaremos para verificar as credenciais de um usuário. **Ações** As ações do Zapier estão lá para enviar dados do Zapier. Vamos implementar a Ação de lead Create_Update que chama a API REST do Marketo. Esta ação permite criar um novo lead no Marketo ou, se o lead já existir, ele o atualiza com os valores enviados. Usaremos o campo &quot;email&quot; para desduplicação. Clique em **Adicionar nova ação** para iniciar o assistente e preencher os seguintes campos (os campos não mencionados podem ser deixados em branco): Nome e Descrição

* Nome: Create_Update Lead
* Substantivo: chumbo
* Chave: create-update-lead
* Descrição: Crie um novo lead no Marketo ou, se o lead já existir, atualize-o com os valores enviados
* Importante? Marcado(s)
* Ocultar? Não selecionado

Campos de ação Campos de ação são os campos para os quais os dados serão mapeados pelos usuários. Escolha-os cuidadosamente de acordo com suas necessidades, pois eles representarão todos os dados que você pode atualizar no Marketo. Há uma opção no Zapier para oferecer ao usuário final todos os campos disponíveis no Marketo, mas isso induziria mais código e complexidade, não necessária para um conector descartável. Como exemplo, selecionamos os seguintes campos.

O Nome da partição é obrigatório no nosso caso, pois nossa instância do Marketo tem Partições de cliente potencial em serviço. Caso contrário, poderia ser omitido. O separamos do grupo &quot;entrada&quot; para que o usuário final entenda que não é um campo a ser sincronizado. O campo &quot;Notas&quot; vem de uma sincronização entre o Marketo e o Salesforce. Não use-o se não estiver na instância do Marketo. O campo &quot;Chamado&quot; foi criado em nossa instância do Marketo. Não o use se não estiver na instância do Marketo. É claro que o objetivo é permitir que você escolha os campos necessários no Marketo. É recomendável começar pequeno e adicionar os campos extras posteriormente. Para onde enviar dados

* URL do Ponto de Extremidade da Ação: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/leads.json`

Exemplo de resultado

* Deixe em branco

### API de script do Zapier

O recurso de script do Zapier permite manipular as solicitações e respostas trocadas entre a API do aplicativo e o Zapier. Você pode modificar solicitações HTTP antes que sejam enviadas e pode analisar respostas antes que o Zapier faça qualquer coisa com elas. Precisamos dela para concluir nossa autenticação personalizada de &quot;Autenticação de sessão&quot; para que funcione com o Marketo. Mais informações [aqui](https://zapier.com/developer/documentation/scripting/). Copie o código a seguir e veremos algumas explicações mais tarde:

```javascript
var Zap = {

    get_session_info: function(bundle) {

       console.log('Entering get_session_info method ...');

         var access_token,
            access_token_request_payload,
            access_token_response;


        // Assemble the meta data for our Access Token swap request
         console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
        access_token_request_payload = {
            method: 'POST',
            url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
            params: {
                'grant_type' : 'client_credentials',
                'client_id' : bundle.auth_fields.client_id,
                'client_secret' : bundle.auth_fields.client_secret
            },
            headers: {
                'Content-Type': 'application/json',  // Could be anything.
                Accept: 'application/json'
            }
        };

        // Fire off the Access Token request.
        access_token_response = z.request(access_token_request_payload);

        // Extract the Access Token from returned JSON.
        access_token = JSON.parse(access_token_response.content).access_token;
        console.log('New Access_Token=' + access_token);

        // This will be mixed into bundle.auth_fields in future calls.
        //bundle.auth_fields.access_token=access_token;
        return {'access_token': access_token};
    },


    test_trigger_pre_poll: function(bundle) {

         console.log('Entering test_trigger_pre_poll method ...');

         bundle.request.params = {
         'access_token':bundle.auth_fields.access_token
         };

         return bundle.request;

    },


    test_trigger_post_poll: function(bundle) {

        console.log('Entering test_trigger_post_poll method ...');

        var data = JSON.parse(bundle.response.content);
        if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

           throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }

        return JSON.parse(bundle.response.content);
    },

    create_update_lead_pre_write: function(bundle) {

       bundle.request.params = {'access_token':bundle.auth_fields.access_token};
       return bundle.request;
    },

    create_update_lead_post_write: function(bundle) {

         var data = JSON.parse(bundle.response.content);
         if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }
        return JSON.parse(bundle.response.content);
    }
};
```

**Métodos** **get_session_info**

* Este método é responsável pela geração ou regeneração de um token de acesso que chama o ponto de extremidade da API REST do Marketo [Authentication](/help/rest-api/authentication.md).
* Ele é chamado sempre que qualquer método &quot;post_poll&quot; encontra um erro &quot;Token de acesso expirado&quot;. Um token de acesso está programado para expirar a cada 1 hora, portanto, isso é esperado.
* URL de Ponto de Extremidade de Ação: https://{{munchkin_account_id}}.mktorest.com/identity/oauth/token

**pre_poll, pre_write**

* É necessário criar um método de &quot;pré-enquete&quot; em qualquer acionador criado para modificar a solicitação HTTP antes que ela seja enviada, de modo que possamos adicionar o token de acesso do Marketo em seus parâmetros.
* Devemos criar um método de &quot;pré-gravação&quot; em qualquer Ação que criamos, pela mesma razão.

**post_poll, post_write**

* Devemos criar um método &quot;pós-enquete&quot; em qualquer Acionador criado, para analisar as respostas antes que o Zapier faça qualquer coisa com elas e, eventualmente, interceptar o erro &quot;Token de acesso expirado&quot;.
* Devemos criar um método &quot;pós-gravação&quot; em qualquer Ação que criamos, pelo mesmo motivo.
* Se tal erro tiver ocorrido, lançaremos um InvalidSessionException que dirá ao Zapier para repetir a autenticação e executar novamente o método get_session_info.

Observe que você pode acessar os logs de pacote na API de script a partir do menu &quot;Links rápidos&quot; no canto superior direito da tela. Isso é realmente útil para depurar os scripts.

Caso de uso 1: integração do Marketo com o FullContact Card Reader

Para essa integração, criamos um único Zap do FullContact para o Marketo. Com esse Zap, você pode digitalizar cartões de visita com o FullContact Mobile Card Reader e enviar os clientes em potencial para a Marketo.   **Zap FullContact -> Marketo** No Painel Zapier, clique no botão &#39;Criar um novo Zap&#39;.

**Acionador em Zapier**

* Selecione o App FullContact
* Escolher o Acionador de Contato Completo &#39;Novo Cartão de Visita&#39;
* Conectar-se à sua conta FullContact
* Testar o aplicativo FullContact

**Ação em Zapier**

* Escolha o Marketo do aplicativo que acabamos de criar antes. Ele deve ser exibido no Beta
* Escolher Ação &#39;Criar_Atualizar Cliente Potencial&#39; Do Marketo
* Conecte-se à sua conta do Marketo, preenchendo os parâmetros de autenticação (ID da conta do Munchkin, ID do cliente, Segredo do cliente)
* Mapear os campos de FullContact para Marketo
* Eventualmente, preencha um Nome de partição para onde seus novos leads devem ir (somente se as partições existirem na instância do Marketo)
* Testar o aplicativo Marketo
* Ativar o Zap

Observação: certifique-se de baixar os cartões de visita Reader do FullContact e ativar o Zapier Integration diretamente do seu dispositivo móvel.

Caso de uso 2: integração do Marketo com o Google Sheets-

Para essa integração, criamos dois Zaps. Um do Marketo para o Google Sheets e outro do Google Sheets para o Marketo. Com esse Zap, você pode sincronizar alguns de seus leads ou contatos entre o Marketo e uma Planilha da Google.   **Webhook Do Zap Marketo -> Google Sheets**
Para o primeiro Zap, não dependemos de um conector personalizado para o Marketo, mas aproveitamos os Webhooks do Marketo e o Acionador &quot;Webhooks by Zapier&quot;. No Painel Zapier, clique no botão &quot;Criar um novo Zap&quot;. **Acionar Parte 1 em Zapier**

* Escolha o aplicativo acionador &quot;Webhooks by Zapier&quot;
* Marque &quot;Catch Hook&quot; que permite aguardar um POST ou GET para um URL Zapier
* Não há necessidade de escolher uma chave filha
* O Zapier gerou um **URL de webhook** personalizado para que você envie solicitações para e copie-o na área de transferência

**Webhook no Marketo (etapas a serem executadas por um Administrador)**

* Acesse Admin -> Webhooks
* Crie um novo Webhook chamado &quot;Enviar lead para Zapier&quot; e edite o formulário Webhook.

No campo do modelo, declare todos os campos do lead que deseja transferir para o Zapier e utilize os tokens do Marketo. Para nossos Casos de uso, pegamos os mesmos campos que definimos para o conector Zapier personalizado que envia clientes potenciais para o Marketo:

```json
{
"firstName":"{{lead.First Name}}",
"lastName":"{{lead.Last Name}}",
"email":"{{lead.Email Address}}",
"phone":"{{lead.Phone Number}}",
"leadOwner":"{{lead.Lead Owner First Name}} {{lead.Lead Owner Last Name}}",
"leadOwnerEmail":"{{lead.Lead Owner Email Address}}",
"leadNotes":"{{lead.Lead Notes:default=edit me}}",
"called":"{{lead.Called}}"
}
```

* Salve o formulário
* Não há necessidade de um Mapeamento de resposta, então você terminou o Webhook

**Testar campanha no Marketo (etapas a serem executadas por um profissional de marketing ou um administrador)**

* Nas Atividades de marketing, crie uma nova Campanha inteligente

Para fins de teste, vamos criar uma campanha que aciona nosso Webhook sempre que um status de lead for alterado para MQL. É claro que você pode usar o Webhook para qualquer outro propósito comercial.

* Editar a lista inteligente
* Chamar o Webhook no fluxo
* Programar a campanha
* Certifique-se de que cada lead possa percorrer o fluxo toda vez
* Ativar a campanha inteligente

**Acionar Parte 2 em Zapier**

* Para concluir o aplicativo de acionador &quot;Webhooks by Zapier&quot;, precisamos acionar o Marketo Smart Campaign uma vez e capturar o Webhook no Zapier
* No nosso caso de teste, só precisamos acessar o banco de dados de clientes potenciais da Marketo, abrir um cliente potencial e alterar seu status para &quot;MQL&quot;

**Criar a planilha no Google Sheets**

* Criar uma nova planilha
* Criar uma Planilha ou usar a padrão
* Adicione uma coluna para cada campo que você deseja sincronizar a partir do Marketo (aqueles declarados no webhook do Marketo)

  **Ação em Zapier**

* Escolha o aplicativo Google Sheets
* Marque a opção &#39;Criar linha da planilha&#39;
* Conectar à sua conta do Google Sheets
* Selecione sua planilha do Google Sheets
* Selecione a Planilha
* Mapeie todos os campos entre o aplicativo acionador &quot;Webhooks by Zapier&quot; e as planilhas do Google.

* Testar o aplicativo Google Sheets
* Ativar o Zap

**Planilhas Do Zap Google -> Marketo**

No Painel Zapier, clique no botão &quot;Criar um novo Zap&quot;.

**Acionador em Zapier**

* Escolha o aplicativo acionador &quot;Google Sheets&quot;
* Marque a opção &quot;Linha da planilha atualizada&quot; que é acionada quando uma nova linha é adicionada ou modificada em uma planilha
* Conectar-se à sua conta do Google
* Selecione a Planilha que deseja acionar (deve ser a mesma usada no Zap anterior) e a Planilha
* Definir coluna de acionador como &#39;any_column&#39;
* Testar o aplicativo Google Sheets

**Ação em Zapier**

* Escolha o Marketo do aplicativo que acabamos de criar antes. Ele deve ser exibido no Beta
* Escolher Ação &#39;Criar_Atualizar Cliente Potencial&#39; Do Marketo
* Conecte-se à sua conta do Marketo, preenchendo os parâmetros de autenticação (ID da conta do Munchkin, ID do cliente, Segredo do cliente)
* Mapear os campos do Google Sheets para o Marketo
* Eventualmente, preencha um Nome de partição para onde seus novos leads devem ir (somente se as partições existirem na instância do Marketo)
* Testar o aplicativo Marketo
* Ativar o Zap

### Conclusão

Estas são algumas ideias para o aprimoramento do conector do Marketo para Zapier:

* Adicionar outros acionadores e ações relacionados a diversos objetos do Marketo (Listas, Objetos personalizados etc.)
* Em vez de codificar os campos do Marketo, é possível extrair dinamicamente os campos do Marketo, mas isso exigiria algum trabalho de tradução técnica entre o Marketo e o Zapier.
* Compartilhamento do conector com a equipe de desenvolvimento e, eventualmente, disponibilizá-lo no geral.

É possível que o Zapier implante um adaptador Premium do Marketo, o que facilitaria muito a implementação de nossos casos de uso. Em qualquer caso, este artigo sempre poderia ser aproveitado para integrar o Marketo com o Zapier com um plano Zapier gratuito e também para criar casos de uso personalizados que podem não ser compatíveis com um adaptador premium. Esperamos que você tenha gostado deste artigo e que ele o ajude a ter ainda mais sucesso com o Marketo e o Zapier. Obrigado!

Publicado em _2016-04-17_ por _David_

## Atualizações do primeiro trimestre de 2016

**REST API**

* API de ativo - Páginas da Web
   * As **Páginas de Aterrissagem** agora são expostas por meio de quinze novos pontos de extremidade que permitirão criar, atualizar, excluir, clonar e gerenciar rascunhos de páginas de aterrissagem. Os modelos de landing page agora também têm endpoints de gerenciamento de rascunho expostos
      * Obter páginas de destino
      * Obter página inicial por ID
      * Obter página inicial por nome
      * Criar página de destino
      * Atualizar metadados da página de aterrissagem
      * Obter conteúdo da landing page
      * Adicionar seção de conteúdo da landing page
      * Seção Atualizar conteúdo da página inicial
      * Excluir seção de conteúdo da página de aterrissagem
      * Obter Seção de Conteúdo Dinâmico
      * Seção Atualizar Conteúdo Dinâmico
      * Descartar rascunho da página inicial
      * Aprovar página
      * Cancelar aprovação de rascunho da página de aterrissagem
      * Excluir página de destino
   * **Modelos de página de aterrissagem**
      * Descartar rascunho de modelo de página de aterrissagem
      * Aprovar modelo de página
      * Cancelar aprovação de modelo de página de aterrissagem
      * Excluir modelo de página
   * **O Forms** lançou 21 novos pontos de extremidade que fornecem recursos completos de criação, edição e gerenciamento por meio da API. As APIs não oferecerão suporte a alterações em formulários do Forms 1.0.
      * Obter o Forms
      * Obter formulário por ID
      * Obter formulário por nome
      * Obter lista de campos de formulário
      * Atualizar lista de campos de formulário
      * Criar formulário
      * Obter formulário da página de agradecimento
      * Atualizar a página de agradecimento do formulário
      * Atualizar Formulário
      * Descartar Rascunho de Formulário
      * Aprovar formulário
      * Cancelar aprovação do formulário
      * Clonar formulário
      * Excluir formulário
      * Atualizar campo de formulário
      * Remover campo de formulário
      * Atualizar regras de visibilidade do campo de formulário
      * Adicionar campo de formulário Rich Text
      * Adicionar conjunto de campos
      * Remover campo do conjunto de campos
      * Obter campos de formulário disponíveis
      * Alterar posições do campo de formulário
      * Botão Enviar de Atualização
   * Ao usar **Obter ou Procurar Programas**, a ID do SFDC Campaign será retornada para Programas que estão vinculados a uma Campanha do SFDC

**Objetos Personalizados** Os Objetos Personalizados agora oferecerão suporte aos tipos de dados da Área de Texto, permitindo que campos de sequência com até 2000 caracteres sejam armazenados em campos de objeto personalizados desse tipo. **Lista de permissões de endereço IP** Os usuários administradores agora poderão gerenciar uma lista de permissões de endereços IP para impedir o acesso não autorizado por meio das APIs. [Leia mais sobre este recurso aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR). Os usuários administradores da **Interface de Usuário de Atividade Personalizada** poderão definir tipos de Atividade Personalizada no menu de administradores e adicionar registros a clientes potenciais por meio da API [Adicionar Atividades Personalizadas](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST). [Você pode ler sobre a definição de tipos de atividades personalizadas aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR).

Publicado em _2016-06-01_ por _Kenny_

## Atualizações do segundo trimestre de 2016

Para a versão de verão de 2016 em 23 de setembro, há três recursos orientados para desenvolvedores sendo lançados.

### Suporte de email 2.0 na API REST

Todas as [APIs de ativos pré-existentes](/help/rest-api/assets.md) que eram compatíveis apenas com Emails e Modelos v1.0 agora estão habilitadas para uso com ativos de email v2.0.

### Enviar lead ao Marketo

[Encaminhar lead](/help/rest-api/leads.md) é um método alternativo de sincronização de lead criado para facilitar o acionamento nas Campanhas Inteligentes. Você pode criar um único item do log de atividades, associar um cliente potencial e atualizar o registro de cliente potencial em uma chamada. Isso funciona de forma semelhante a um único formulário preenchido por um cliente potencial e pode ser usado mais facilmente como um método proxy para envio de formulários, em vez de usar o método Sincronizar clientes potenciais existente.

### Compactação HTTP

A REST API agora pode compactar respostas usando o padrão definido pela especificação HTTP 1.1. Isso pode ajudar a reduzir o tamanho da resposta, o que aumentará a velocidade de transferência, e minimizar a utilização da largura de banda.  

Publicado em _2016-09-23_ por _Kenny_

## Uso do codegen swagger com o Marketo

[Swagger-codegen](https://github.com/swagger-api/swagger-codegen) é uma poderosa biblioteca Java que pode gerar stubs de servidor e clientes de API a partir de definições de swagger. Isso pode aliviar drasticamente a dificuldade e o custo de geração de clientes para qualquer idioma específico. Para começar e gerar seu primeiro cliente, primeiro é necessário capturar uma cópia específica da instância de uma das definições do Swagger da Marketo. Insira a Munchkin ID da instância com a qual você deseja testar. Comece com a definição de identidade. Agora que você tem uma definição específica para sua instância, é necessário baixar e instalar o swagger-codegen. O processo é específico para o seu sistema operacional e você pode obter as instruções [aqui](https://github.com/swagger-api/swagger-codegen#prerequisites) Com as configurações padrão, o código gerará um cliente que cobrirá todos os modelos e pontos de extremidade fornecidos. Normalmente, eles são gerenciados por meio de uma classe chamada DefaultApi, que contém métodos para chamar os endpoints disponíveis com exemplos fornecidos em uma pasta &quot;docs&quot; (nem todas as linguagens incluem modelos para isso por padrão). Agora vamos construir o primeiro cliente. Crie uma pasta onde deseja que seu código resida, acesse a sessão de terminal e use o comando generate para criar um cliente no idioma desejado (vamos supor que você tenha usado o método de instalação homebrew):

swagger-codegen generate -i $definitionLocation -l $yourLanguage -o $yourLocation

Isso exibirá o código de cliente no local desejado. Agora vamos examinar como usar isso para chamar o endpoint de identidade e obter um token de acesso:

```java
 public static void main(String[] args){
  String client_id = args[0];
  String client_secret = args[1];
  ApiClient client = new ApiClient();
  DefaultApi id = new DefaultApi();
  try {
   String token = id.identityOauthTokenGet(client_id, client_secret, "client_credentials").getAccessToken();
   System.out.println(token);
  } catch (ApiException e) {
   e.printStackTrace();
  }
 }
```

```php
<?php
require_once(_DIR_ . '/vendor/autoload.php');

$api_instance = new Swagger\\Client\\Api\\DefaultApi();
$client_id = "client_id_example";
$client_secret = "client_secret_example";
$grant_type = "grant_type_example";

try {
    $result = $api_instance->identityOauthTokenGet($client_id, $client_secret, $grant_type);
    print_r($result->getAccessToken);
} catch (Exception $e) {
    echo 'Exception when calling DefaultApi->identityOauthTokenGet: ', $e->getMessage(), "\\n";
}
?>
```

```c#
  public static void Main(string[] args)
  {
      string clientId = "CHANGE ME";
      string clientSecret = "CHANGE ME";

      IdentityApi instance = new IdentityApi();
      ResponseOfIdentity response = instance.IdentityUsingGET(clientId, clientSecret, "client_credentials");

      string message = string.Format("Access Token: {0}, Expires In: {1}, Scope: {2}, Token Type: {3}",
          response.AccessToken, response.ExpiresIn, response.Scope, response.TokenType);
      Console.WriteLine(message);
  }
```

Agora que sabemos como obter autorização, examinaremos os casos de uso mais avançados de clientes gerados automaticamente nas próximas semanas.

Publicado em _2016-10-10_ por _Kenny_

## Integração com o Excel Parte 1: Extrair e moldar dados do Marketo usando o Power Query

Este é o primeiro de uma série de artigos que explicam como aproveitar a tecnologia do Power BI incorporada ao Microsoft Excel para criar uma verdadeira experiência de análise comercial de autoatendimento com o Marketo. Com os conceitos abordados nesses artigos, você pode:

* Importar dados do Marketo para o Excel
* Importe e combine dados de outras fontes (aplicativos SaaS, bancos de dados, arquivos simples etc.)
* Dados da forma para fins de análise e necessidades comerciais
* Atualizar os dados do Excel sob demanda
* Criar colunas calculadas e medidas usando fórmulas
* Criar relações entre dados heterogêneos
* Analisar dados e criar relatórios avançados com Tabelas Dinâmicas e Gráficos Dinâmicos
* Produzir visualizações de dados impressionantes

### Power Query para Excel

Este primeiro artigo aborda o processo de importação e modelagem de dados usando a tecnologia Power Query. O Power Query é uma tecnologia de conexão de dados que permite descobrir, conectar, combinar e refinar fontes de dados para atender às suas necessidades de análise. Os recursos do Power Query estão disponíveis no Excel e no Power BI Desktop. O Power Query pode se conectar a várias fontes de dados, como bancos de dados, Facebook, Salesforce, MS Dynamics CRM etc. O Marketo não é compatível imediatamente, mas, felizmente, podemos usar as APIs REST do Marketo para execução remota de muitos recursos do sistema, e o Power Query vem com um conjunto avançado de fórmulas (informalmente conhecidas como &quot;M&quot;) que permitem criar scripts para uma fonte de dados personalizada.

### Conector personalizado

Gerar script de uma única chamada de API REST é trivial com o Power Query, mas torna-se mais desafiador lidar com os seguintes requisitos:

* Gerenciamento de token de acesso, incluindo mecanismo de autenticação e atualização periódica de token
* Mecanismo de paginação para um grande conjunto de dados
* Tratamento de erros

Este artigo explica como criar um conector personalizado robusto que pode consumir as APIs REST do Marketo para obter todos os tipos de dados (clientes em potencial, atividades, objetos personalizados, programas etc.). Sua única restrição é o limite diário de solicitações da API do Marketo. Os conceitos explicados aqui se concentram no Marketo, mas também podem ser usados para integrar outras soluções SaaS que fornecem uma REST API.

### Pré-requisitos

#### Power Query

Antes do lançamento do Excel 2016, o Microsoft Power Query para Excel funcionava como um complemento do Excel que foi baixado e instalado no Excel 2010 ou Excel 2011. No Excel 2016, essa tecnologia é um recurso nativo integrado à faixa de opções &quot;Dados&quot; na seção &quot;Obter e transformar&quot;. Todos os scripts produzidos para este artigo foram testados no Excel 2016 para Windows. Os conceitos devem ser os mesmos para o Excel 2013 ou Excel 2010, mas podem ser necessárias algumas adaptações.  Atualmente, o Power Query está disponível apenas na versão do Excel para Microsoft Windows. Infelizmente, não há suporte para a versão do Mac.

#### Marketo

O Power Query usará as APIs REST do Marketo para acessar dados do Marketo. Para usar essas APIs, você precisa de um Usuário da API e um Serviço personalizado que você possa criar se for o administrador da instância do Marketo. Caso contrário, um administrador precisará fornecê-las a você. Uma explicação passo a passo de como criar o Usuário da API Marketo e o Serviço Personalizado pode ser encontrada [aqui](/help/rest-api/custom-services.md). Quando terminar, você deverá ter as seguintes credenciais para invocar as APIs REST do Marketo: **ID do Cliente** e **Segredo do Cliente**. O **Ponto de Extremidade da API REST** pode ser encontrado na seção API REST do Administrador de Serviços Web no Marketo e deve ter o seguinte padrão:

`<https://XXX-XXX-XXX.mktorest.com/rest>`

O Marketo tem um limite diário de solicitações para sua API e esse limite pode ser encontrado no administrador dos serviços da Web, juntamente com um relatório de consumo. **Nunca exceda seu limite diário ao criar suas consultas, pois você pode perder alguns dados em seus relatórios**.

### Criação de Pasta de Trabalho do Power Query

Vamos começar com uma nova pasta de trabalho do Excel. Criamos uma planilha de configuração específica para declarar todas as Configurações da API REST do Marketo. Nesta planilha, criamos três tabelas:

Tabela &#39;**REST_API_Authentication**&#39; com as colunas: **URL**: seu Ponto de Extremidade da API REST do Marketo. **ID do cliente**: da credencial REST API OAuth2.0 do Marketo. **Segredo do cliente**: da credencial REST API OAuth2.0 do Marketo.
Tabela &#39;**Scoping**&#39; com as colunas: **Paging Token SinceDatetime**: uma data seguindo a notação de data padrão ISO 8601 (por exemplo, &quot;2016-10-06T13:22:17-08:00&quot;, &quot;2016-10-06&quot; são data/hora válidas) usada para buscar atividades Marketo desde um determinado período, graças a um token de paginação inicial &#39;baseado em data&#39;. Essa data é usada principalmente para limitar a quantidade de dados a serem importados para a pasta de trabalho. **ID da Lista**: a ID de uma lista estática no Marketo que faz referência a todos os clientes potenciais/contatos com os quais estamos lidando. Essa lista estática pode ser gerenciada livremente no Marketo (por exemplo, uma campanha inteligente pode alimentá-la periodicamente ou em tempo real com leads e contatos).
Para obter a ID de uma lista estática, abra-a no Marketo e obtenha sua ID numérica da URL, por exemplo: `<https://myorg.marketo.com/#ST3517A1LA1>`, ID da Lista=3511. **Máximo de Páginas de Registros**: é usado para nossos algoritmos pseudo-recursivos que iteram pelos dados de saída do Marketo, usando tokens de paginação &quot;baseados em posição&quot;, com uma capacidade de 300 máx. de registros por página. Como é nosso interesse obter o máximo possível de registros por página, vamos ficar com 300. Portanto, normalmente, um número máximo de páginas de registros definido como 33,333 significa uma capacidade de 33,333 X 300 = 9,9999 milhões de registros; mas também significa 33,333 K no limite de solicitação diária da API do Marketo. Os algoritmos serão interrompidos assim que todos os dados dos queries forem obtidos, então esse parâmetro é apenas um limite de segurança para um loop.

Tabela `Leads` com a coluna: **Campos de cliente potencial**: campos de cliente potencial separados por vírgulas para coletar da Marketo ao consultar clientes potenciais e contatos. Declarar uma tabela no Excel é simples. Insira duas linhas na planilha com os nomes e valores das colunas, realce com o mouse o perímetro da tabela, selecione o ícone Tabela no menu &#39;Inserir&#39; e nomeie-o. Os nomes fornecidos às tabelas e suas colunas são importantes, pois serão chamados diretamente por nossos scripts.

## Token de autenticação e acesso

### Sobre a autenticação da API REST do Marketo

As REST APIs do Marketo são autenticadas com OAuth 2.0 de duas pernas. As IDs e os segredos do cliente são fornecidos por serviços personalizados definidos por você. Cada serviço personalizado pertence a um usuário Somente API que tem um conjunto de funções e permissões que autorizam o serviço a executar ações específicas. Um token de acesso é associado a um único serviço personalizado.
O mecanismo de Autenticação completa está documentado [aqui](/help/rest-api/authentication.md) no site do Desenvolvedor do Marketo. Quando um token de acesso é criado originalmente, sua duração é de 3600 segundos ou 1 hora. Cada chamada de autenticação consecutiva para o mesmo serviço personalizado retorna o token de acesso atual com seu tempo de vida restante. Depois que o token expira, a autenticação retorna um token de acesso totalmente novo. O gerenciamento da expiração do token de acesso é importante para garantir que sua integração funcione sem problemas e evite a ocorrência de erros inesperados de autenticação durante a operação normal.

#### Criar consulta

Crie uma nova consulta clicando no ícone &quot;Nova consulta&quot; na seção &quot;Get&amp;Transform&quot; do menu &quot;Dados&quot;. Selecione uma consulta em branco para começar e dê a ela um nome como &quot;MktoAccessToken&quot;. Inicie o Editor Avançado no Editor de Consulta, para que você possa criar scripts manualmente de algumas fórmulas do Power Query. Insira o seguinte código no editor avançado:

```
let
    // Get url and credentials from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
    clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

    // Calling Marketo API Get Access Token
    getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
    TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

    // Parsing access token
    accessTokenStr = TokenJson [access_token]

in
    accessTokenStr
```

Os comentários incorporados no código-fonte, precedidos por &quot;//&quot; tornam o código autoexplicativo. Se você precisar de qualquer referência de função, verifique os links fornecidos na seção Referência deste artigo. Clique no botão &quot;Concluído&quot;. Verifique se o token de acesso é exibido com êxito na saída da etapa final aplicada &quot;accessTokenStr&quot;.  Um comentário rápido sobre a segurança no Excel; ocasionalmente, você pode ser solicitado a ativar Conexões de dados externos a partir do banner amarelo. Isso é necessário para que as Queries funcionem corretamente.

#### Conversão de uma Consulta em uma Função

Retorne ao Editor avançado e envolva seu código com a seguinte declaração de função:

```
let
    FnMktoGetAccessToken =()=>

        let
            // Get url and credentials from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
            clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
            clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

            // Calling Marketo API Get Access Token
           getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
           TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

            // Parsing access token from Json
           accessTokenStr = TokenJson [access_token]

        in
            accessTokenStr

in FnMktoGetAccessToken
```

A função não aceita parâmetros na entrada, mas os obtém da planilha de configuração. Ele produz o token de acesso como uma saída. Renomeie sua consulta `FnMktoGetAccessToken` e salve-a. Observe que você pode ver todas as suas consultas a qualquer momento no Excel clicando no botão &quot;Mostrar consultas&quot; na seção &quot;Obter e transformar&quot; do menu Dados. Sua função agora deve ser marcada com o ícone de função &#39;Fx&#39;.

### Carregar Membros da Lista Estática

#### Obter clientes em potencial

A API de clientes potenciais do Marketo fornece operações CRUD simples em relação a registros de clientes potenciais, a capacidade de modificar a associação de um cliente potencial em listas e programas estáticos e iniciar o processamento do Campaign inteligente para clientes potenciais. Todos esses recursos estão documentados [aqui](/help/rest-api/leads.md). Um grande conjunto de registros de clientes potenciais pode ser recuperado com base na associação a uma lista estática ou a um programa. Usando o id de uma lista estática, você pode recuperar todos os registros de lead que são membros dessa lista estática. A id da lista é um parâmetro de caminho na chamada de. Consulte o capítulo &quot;Lista e membros do programa&quot; na documentação de desenvolvedores do Marketo para obter detalhes. O número máximo de registros de clientes potenciais que podemos obter por chamada de API é 300, portanto, precisamos aproveitar os tokens de paginação para coletar os registros por páginas de 300 registros. Recebemos o token de paginação na resposta Json após a primeira chamada e sabemos que terminamos quando o token de paginação não está mais na saída.

### Consulta básica

Vamos começar com uma consulta totalmente funcional destinada ao download de todos os leads de uma lista estática. Crie uma nova consulta em branco chamada &quot;MktoLeads&quot; e insira o seguinte código no editor avançado:

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the number of iterations (pages of 300 records) - Table Scoping
    iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    pagingTokenParamStr = "",

    // Function iterating though the pages
    FnProcessOnePage =
    (accessTokenParamStr, pagingTokenParamStr) as record =>
        let

            // Send REST API Request
            content = Web.Contents(getMultipleLeadsByListIdUrl & accessTokenParamStr & pagingTokenParamStr),

            // Recover Json output and watch if token is expired, in that case, regenerate access token
            newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
            getMultipleLeadsByListIdJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(getMultipleLeadsByListIdUrl & newAccessTokenParamStr & pagingTokenParamStr)),

            // Parse Json outputs: data and next page token
            data = try getMultipleLeadsByListIdJson[result] otherwise null,
            next  = try  "&nextPageToken=" & getMultipleLeadsByListIdJson[nextPageToken] otherwise null,
            res = [Data=data, Next=next, Access=newAccessTokenParamStr]
        in
            res,

    // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
    GeneratedList =
        List.Generate(
            ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
            each [i]<iterationsNum and [res][Data]<>null,
            each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
            each [res][Data])
in
    GeneratedList
```

O Power Query não oferece funções de loop tradicionais (por exemplo, For-loop, While-loop) e não oferece suporte à recursão. Uma boa solução alternativa é implementar um For-loop usando List.Generate. Esta função está documentada [aqui](http://msdn.microsoft.com/query-bi/m/list-generate). Com a lista. É possível iterar sobre as páginas. Em cada etapa da iteração, extraímos uma página de dados, mantendo o URL que inclui o token de paginação da próxima página, e armazenamos os resultados no próximo item da lista gerada. O blog do [Datachant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) foi um excelente recurso para resolver isso. Nosso parâmetro &quot;Número máximo de páginas de registros&quot; está aqui para limitar o número de páginas e restringi-lo a um intervalo realista, evitando um loop infinito. Outro desafio é garantir que o token de acesso nunca expire. O rastreamento do tempo de vida restante seria muito complexo com o Power Query. Portanto, todas as chamadas para a API REST são submetidas a uma verificação de erro; se ocorrer um erro, supomos que o token tenha expirado, o renovamos primeiro e reproduzimos a chamada novamente. Se a segunda chamada falhar, então a segunda falha será notificada ao Excel (no pior caso, você não receberá dados como resultado). Inicie a consulta depois de salvá-la ou clicando no botão &#39;Atualizar&#39; a qualquer momento.  No nosso caso, foram extraídos 1364 registros de chumbo em 5 páginas de dados em 5 listas.

### Modelagem de dados

Precisamos moldar os dados para ter todos esses registros em uma única lista simples de registros. Há duas maneiras de fazer isso:

* Usar mais código
* Aproveitar a interface do usuário do Power Query

Clique com o botão direito na grade de saída e escolha &#39;Para Tabela&#39; no menu contextual para convertê-la em uma tabela de listas.  Na janela pop-up &quot;Para tabela&quot;, deixe os valores padrão nas 2 listas de seleção.  Agora, expanda a tabela de listas resultante. Agora temos todos os registros em uma única lista. Os registros codificados no formato Json contêm os campos e seus valores associados. Expanda novamente. Selecione na janela pop-up todos os campos que deseja manter, desmarque a caixa de seleção &#39;Usar nome de coluna original como prefixo&#39;.  Et voila! Todos os registros são exibidos perfeitamente em nossa tabela. Se reabrirmos o editor avançado, poderemos ver que três linhas de código foram adicionadas para moldar nossos dados:

```
# "Converted to Table" = Table.FromList(GeneratedList, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
# "Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
# "Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"}, {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"})
```

Você pode fazer muito mais com o Power Query, como criar colunas extras com valores computados, veremos mais possibilidades posteriormente. Vamos salvar e fechar esta consulta. Agora, ele pode ser atualizado manualmente a qualquer momento ou automaticamente por meio de atualizações em segundo plano.

### Redirecionando os resultados

Agora a questão é: para onde enviar os dados do resultado? Passe o mouse sobre a consulta e selecione o menu &quot;Carregar para...&quot; no menu contextual. No pop-up, é possível selecionar:

* &#39;Table&#39; se desejar enviar todos os dados modelados para uma planilha (nova ou existente),
* &quot;Somente criar conexão&quot; se o objetivo for fazer uma análise mais aprofundada no Power Pivot.

A caixa de seleção &#39;Adicionar isso ao Modelo de Dados permite explorar os dados no Power Pivot; isso é o que queremos para a segunda parte deste artigo.

### Gerenciamento de paginação

Como o objetivo do nosso projeto é criar muito mais queries, vamos fazer uma refatoração e extrair uma função reutilizável que gerenciaria a paginação. Crie uma nova consulta em branco chamada **FnMktoGetPagedData** e insira o seguinte código no editor avançado:

```
let
    FnMktoGetPagedData =(url, accessTokenParamStr, pagingTokenParamStr)=>

    let

        // Get the number of iterations (pages of 300 records) - Table Scoping
        iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],

        // Sub-function iterating though the REST API service result pages
        FnProcessOnePage =
        (accessTokenParamStr, pagingTokenParamStr) as record =>
            let

                // Send REST API Request
                content = Web.Contents(url& accessTokenParamStr & pagingTokenParamStr),

                // Recover Json output and watch if token is expired, in that case, regenerate access token
                newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
                contentJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(url & newAccessTokenParamStr & pagingTokenParamStr)),

                // Parse Json outputs: data and next page token
                data = try contentJson[result] otherwise null,
                next  = try  "&nextPageToken=" & contentJson[nextPageToken] otherwise null,
                res = [Data=data, Next=next, Access=newAccessTokenParamStr]
            in
                res,

        // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
        GeneratedList =
            List.Generate(
                ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
                each [i]<iterationsNum and [res][Data]<>null,
                each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
                each [res][Data])
    in
        GeneratedList

in FnMktoGetPagedData
```

Salve a consulta. Vamos usá-lo em seguida.

### Consulta simplificada

Vamos regravar novamente nossa consulta &#39;MktoLeads&#39; que chamará a função **FnMktoGetPagedData**.

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    // No initial paging token required for this call
    pagingTokenParamStr = "",

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getMultipleLeadsByListIdUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Como você pode ver, nosso query agora é muito simples de ler e manter. Vamos aproveitar novamente a função **FnMktoGetPagedData** para as outras consultas.

### Carregar Atividades Específicas de um Período Definido

#### obter atividades com paginação

O Marketo permite uma grande variedade de tipos de atividades relacionadas a registros de clientes potenciais. Quase todas as alterações, ações ou etapas de fluxo são registradas no log de atividades de um lead e podem ser recuperadas por meio da API ou aproveitadas nos filtros e acionadores da Smart List e do Smart Campaign. As atividades são sempre relacionadas ao registro de lead por meio da leadId, correspondente à Id do registro, e também têm uma ID inteira exclusiva própria. Você encontra a documentação completa da API REST [aqui](/help/rest-api/activities.md).

Há um número muito grande de tipos de atividades potenciais, que podem variar de assinatura para assinatura, e têm definições exclusivas para cada um. Embora cada atividade tenha sua própria ID exclusiva, leadId e activityDate, o primaryAttributeValueId e o primaryAttributeValue variam em seu significado. Vamos focar nos Momentos Interessantes, um tipo de atividade rastreada do Marketo com a ID 41. Os novos desafios que vamos resolver são:

* Precisamos iniciar um token de paginação &quot;baseado em data&quot; para definir o período em que as atividades ocorreram,
* É um pouco mais complicado modelar os dados, pois, dependendo dos tipos de atividade, uma lista de atributos específicos da atividade é fornecida em Json e precisa ser analisada e nivelada para facilitar a análise.

#### Token de paginação baseado em data

Primeiro, precisamos criar essa função para gerar o token de paginação inicial &quot;baseado em data&quot;, necessário para determinar o escopo do período para nossas consultas de Atividade. Você encontra a documentação sobre o token de paginação [aqui](/help/rest-api/paging-tokens.md). Crie uma nova consulta em branco chamada **FnMktoGetPagingToken** e insira o seguinte código no editor avançado:

```
let
    FnMktoGetPagingToken =(accessTokenStr)=>

        let
            // Get url from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],

            // Get Paging Token SinceDatetime from config worksheet - Table Scoping
            mktoPTSinceDatetimeStr = DateTime.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Paging Token SinceDatetime], "yyyy-MM-ddThh:mm:ss"),

            // Building URL for API Call
            getPagingTokenUrl = mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & accessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr,

            // Calling Marketo API Get Paging Token
            content = Web.Contents(getPagingTokenUrl),

            // Recover Json output and watch if access token is expired, in that case, regenerate it
            newAccessTokenStr = if Json.Document(content)[success]=true then accessTokenStr else "?access_token=" & FnMktoGetAccessToken(),
            pagingTokenJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & newAccessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr)),

            // Parsing Paging Token
            pagingTokenStr = pagingTokenJson[nextPageToken]

        in
            pagingTokenStr

in FnMktoGetPagingToken
```

Salve a função. Vamos usá-lo em seguida.

#### Atividades de momentos interessantes

Agora vamos gravar a consulta &#39;MktoInterestedMomentsActivities&#39; que chamará as funções **FnMktoGetPagedData** e **FnMktoGetPagingToken**.

```
let

    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),

    // Build Get Activities URL
    getActivitiesUrl = mktoUrlStr & "/rest/v1/activities.json?ListId=" & listIdStr & "&activityTypeIds=46",

    // Build Marketo Access Token URL parameter
    accessTokenStr = FnMktoGetAccessToken(),
    accessTokenParamStr = "&access_token=" & accessTokenStr,

    // Obtain date-based paging token used to scope in time the activities
    pagingTokenParamStr = "&nextPageToken=" & FnMktoGetPagingToken(accessTokenStr),

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getActivitiesUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

O resultado desse query é novamente uma lista de listas, portanto, ele precisa de mais processamento de dados para ser utilizável para análise.

### Modelagem de dados

Vamos fazer as mesmas operações de modelagem que fizemos para os clientes potenciais:

* Clique com o botão direito na grade de saída e escolha &#39;Para tabela&#39; no menu contextual para convertê-la em uma tabela de listas,
* Expanda a tabela de listas resultante,
* Expanda mais uma vez, selecionando na janela pop-up todos os campos que deseja manter (desmarque a caixa de seleção &#39;Usar nome de coluna original como prefixo&#39;).

Você pode ver as colunas com seus valores, exceto a coluna &#39;atributos&#39; que ainda contém uma lista de atributos específicos associados aos momentos interessantes. Vamos expandir esses atributos. Agora, a lista se expandiu em registros, expandimos novamente, selecionando os campos que queremos (nome e valor de cada atributo) e desmarcamos a caixa de seleção &quot;usar nome de coluna original como prefixo&quot;.  Como resultado, todos os nossos dados são visíveis, incluindo atributos, mas cada atividade de momento interessante é distribuída por 3 linhas. Isso será difícil de usar para nossa análise.  Idealmente, queremos apenas uma linha por atividade, com todos os atributos exibidos como colunas extras. Podemos fazer isso facilmente girando os 3 atributos de nossa tabela. Selecione as 2 colunas &#39;Nome&#39; e &#39;Valor&#39; nos atributos da atividade e clique em &#39;Coluna dinâmica&#39; no menu &#39;Transformar&#39;. Solicite as opções de avanços na janela pop-up e selecione &#39;Coluna de valores&#39; = valor e a função de valor &#39;Não agregar&#39;.  Clique em &#39;OK&#39; e você precisará gerar uma única linha de dados por atividade.  As seguintes linhas de código de &quot;modelagem de dados&quot; devem ter sido anexadas automaticamente ao script do query:

```
  #"Converted to Table" = Table.FromList(result, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
    #"Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}, {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}),
    #"Expanded attributes" = Table.ExpandListColumn(#"Expanded Column2", "attributes"),
    #"Expanded attributes1" = Table.ExpandRecordColumn(#"Expanded attributes", "attributes", {"name", "value"}, {"name", "value"}),
    #"Pivoted Column" = Table.Pivot(#"Expanded attributes1", List.Distinct(#"Expanded attributes1"[name]), "name", "value")
in
    #"Pivoted Column"
```

### Próximas etapas

Agora é possível projetar todas as queries necessárias para acessar quaisquer dados específicos do Marketo disponíveis por meio das REST APIs. Esperamos que você tenha gostado deste artigo e que ele tenha ajudado você a aproveitar os excelentes benefícios combinados do Excel e do Marketo. Uma pasta de trabalho de exemplo com todas as consultas também é fornecida no segundo artigo.

### Referências

#### Power Query

* [Power Query - Visão Geral e Aprendizado](https://support.microsoft.com/en-us/article/Power-Query-Overview-and-Learning-ed614c81-4b00-4291-bd3a-55d80767f81d)
* [Referência de Fórmula do Power Query](http://msdn.microsoft.com/query-bi/m/power-query-m-reference)
* O [Blog Matt Masson](http://www.mattmasson.com/tag/power-query/) fornece alguns bons recursos sobre o Power Query
* O [Blog do DataChant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) é muito útil para a implementação do mecanismo de paginação

Publicado em _2016-10-18_ por _Philippe_

## Atualizações do último trimestre de 2016

Na versão lançada no último trimestre de 2016, adicionamos suporte à CRUD para variáveis e módulos de email v2 e suporte à CRUD para contas nomeadas. Agora é possível ler e editar localmente o conteúdo usando as APIs REST do Marketo, bem como movê-las, reordená-las e excluí-las. Consulte a lista completa de atualizações abaixo:

### APIs de banco de dados de clientes potenciais

* [**Contas nomeadas**](/help/rest-api/named-accounts.md)
   * Novos endpoints para ler, atualizar e excluir contas nomeadas
   * Problemas conhecidos:
      * A partir da versão do último trimestre de 2016, os leads não podem ser associados a contas nomeadas por meio da API

### APIs de ativos

* [**Email**](https://developer.adobe.com/marketo-apis/api/asset/#operation/describeUsingGET_5)
   * Novos endpoints para manipulação de variáveis de email v2
   * Novos endpoints para manipulação de módulos do Email v2
   * Problemas conhecidos:
      * Consultas e atualizações de seções que contêm tokens preditivos retornarão um erro
      * Emails com seções de conteúdo que contêm tokens preditivos podem não ser aprovados usando a API

Publicado em _2016-12-07_ por _Kenny_

## Por que aposentamos @MarketoDev Twitter Handl3

Decidimos retirar o nome @MarketoDev do Twitter. A conta será desativada em 9 de dezembro de 2011. Curioso por quê? **Retroceder para o início de 2014...** Acabamos de iniciar o Site de Desenvolvedores do Marketo para ajudar desenvolvedores a criarem com base em nossas APIs. Queríamos aumentar a conscientização sobre a plataforma Marketo e diminuir as barreiras para a entrada de desenvolvedores. Juntamente com esse esforço, criamos a @MarketoDev para nos envolver com nossos parceiros de tecnologia e clientes que estavam inclinados a criar soluções integradas com a Marketo. Como qualquer nova empreitada corajosa, não tínhamos certeza de como isto iria funcionar. Inicialmente, tuitamos novas postagens em blogs e novos lançamentos de API. As coisas estavam boas; o tráfego para o site de desenvolvedores aumentou! Também começamos a receber uma variedade de perguntas, que respondemos com obediência. **Avançar rapidamente até o final de 2016...** À medida que o ecossistema do [Parceiro do LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) crescia, a quantidade de atividade de tweet aumentava. Responder ao volume de perguntas postadas para @MarketoDev se tornou um grande trabalho para a nossa pequena equipe. Recebemos uma quantidade crescente de perguntas gerais sobre produtos e vendas, que redirecionamos para @Marketo ou @MarketoCares. Também descobrimos que 140 caracteres simplesmente não eram suficientes para perguntas relacionadas ao desenvolvimento. Responder a isso muitas vezes levou a longos e envolvidos tópicos de conversa, uma abordagem que não se dimensionou. E finalmente, analisamos as fontes de tráfego para os visitantes do site de desenvolvedores e descobrimos que a maioria vem da pesquisa orgânica e da funcionalidade &quot;Inscreva-se agora&quot; em nosso blog. Por essas razões, decidimos puxar a tomada em @MarketoDev. **Daqui por diante...** Se você é um fã do Twitter (e não é), não tenha medo! Nossa plataforma corporativa Twitter handle @Marketo continua; assim como nosso suporte ao cliente lida com @MarketoCares.

Publicado em _2016-12-02_ por _David_

## Integração com o Excel Parte 2: Criar relatórios e visualizações de dados avançados do Marketo usando o Power Pivot e o Power View-

Este é o segundo de uma série de dois artigos que explicam como aproveitar a tecnologia do Power BI incorporada ao Microsoft Excel para criar uma verdadeira experiência de análise de negócios de autoatendimento com o Marketo. Com os conceitos abordados nesses artigos, você pode:

* Importar dados do Marketo para o Excel
* Importe e combine dados de outras fontes (aplicativos SaaS, bancos de dados, arquivos simples etc.)
* Dados da forma para fins de análise e necessidades de negócios
* Atualizar dados sob demanda no Excel
* Criar colunas calculadas e medidas usando fórmulas
* Criar relações entre dados heterogêneos
* Analisar dados e criar relatórios avançados com Tabelas Dinâmicas e Gráficos Dinâmicos
* Produzir visualizações de dados impressionantes

### Power Pivot e Power View para Excel

Neste artigo, fornecemos exemplos de como criar o seguinte:

* Relatórios avançados do Marketo que aproveitam os relacionamentos entre diferentes coleções de dados do Marketo usando o Power Pivot
* Visualizações estáticas e animadas interessantes usando o Power View

O **Power Pivot** é um suplemento do Excel, já incluído no Excel 2016, que você pode usar para executar análise de dados avançada e criar modelos de dados sofisticados. Com o Power Pivot, você pode reunir grandes volumes de dados de várias fontes, realizar análise de informações rapidamente e compartilhar insights facilmente. Os dados extraídos de diferentes fontes de dados com o Power Query podem ser enviados para o modelo de dados, para a planilha do Excel ou para ambos. No primeiro artigo, importamos e moldamos dados do Marketo e os enviamos ao modelo de dados para executar análises mais sofisticadas antes de disponibilizá-los na planilha. O **Power View** é uma alternativa à camada de visualização do Excel. É uma experiência interativa de exploração, visualização e apresentação de dados que incentiva relatórios ad-hoc intuitivos e painéis.

Todas as etapas explicadas neste artigo foram testadas no Excel 2016 para Windows. Os conceitos devem ser os mesmos para o Excel 2013 ou Excel 2010 (sem o Power View), mas algumas adaptações podem ser necessárias. O Power Pivot e o Power View estão atualmente disponíveis apenas na versão do Excel 2016 para Microsoft Windows. A versão do Office 365 do Excel 2016 não oferece suporte total ao Power Pivot e ao Power View.

### A pasta de trabalho de energia do Marketo

#### Baixar pasta de trabalho

No primeiro artigo, abordamos o processo de importação e modelagem de dados usando a tecnologia Power Query. Aprendemos a implementar algumas consultas avançadas sobre energia para extrair leads e atividades do Marketo. Porque alguns de vocês gostariam de pular diretamente para o ponto em que eles criam relatórios e visualizações, sem escrever códigos. Esta pasta de trabalho contém todas as consultas detalhadas no primeiro artigo e alguns outros. Melhoramos o tratamento de erros e adicionamos alguns parâmetros extras na planilha de configuração. Se você concluiu o primeiro artigo, recomendamos baixar a Pasta de trabalho do Marketo Power e conferir o que foi adicionado.

Aviso de isenção de responsabilidade: a Pasta de trabalho do Marketo Power não é um produto oficial da Marketo e, portanto, não é compatível com a Marketo. Sinta-se à vontade para usar e expandir suas necessidades comerciais pessoais, mas faça isso por sua conta e risco.

#### Configurar pasta de trabalho

Preencha todas as informações necessárias na planilha de configuração do Marketo:

* **Autenticação da API REST do Marketo:** necessária
* **Escopo:** defina o Token de Paginação SinceDatetime e a Id da lista estática do Marketo contendo todos os clientes potenciais que você deseja analisar
* **Cliente Potencial:** para os relatórios futuros, você deve especificar pelo menos os seguintes campos de Cliente Potencial: `id`, `firstName`, `lastName`, `email`, criar`edAt`, `updatedAt`, `title`, `company`, `industry`, `inferredCountry`, `inferredCity`
   * Se as informações da cidade forem mais precisas em um de seus campos personalizados, você poderá usar seu próprio campo
* **Atividades:** os tipos de atividades a serem buscados no banco de dados do Marketo estão especificados aqui para cada conjunto de Atividades; não é necessário alterá-los agora.
   * Observe que fornecemos uma consulta de utilitário na pasta de trabalho que lista, diretamente na pasta de trabalho do Excel, todos os tipos de atividade existentes se você quiser ajustar essas informações posteriormente

Observe que você pode ver alguns pop-ups relacionados à segurança. Confiar em conexões externas e defini-las como &#39;Público&#39;. Se você vir o pop-up abaixo, fique com o conteúdo &quot;Anônimo&quot; de acesso à Web. A autenticação no Marketo é gerenciada diretamente por nossos queries personalizados, portanto, não é necessário habilitar outro tipo de acesso.

#### Baixar dados do Marketo

Primeiro, verifique se o parâmetro definido na área Escopo da planilha de configuração do Marketo não resultará no download de muitos dados, excedendo o limite diário de solicitações da API do Marketo. Quando estiver pronto, clique no botão &quot;Atualizar tudo&quot; no menu &quot;Dados&quot; e aguarde até que todos os dados sejam baixados na pasta de trabalho. Se mensagens de erro de formatação forem exibidas ao baixar os dados, semelhantes a &quot;coluna1 não encontrada&quot;, significa que uma ou mais consultas estão falhando ao obter os dados, portanto, a formatação também está falhando. Tente novamente mais tarde, se o erro persistir, verifique sua versão do Excel (não use o Excel 2016 do Office 365). Também é importante respeitar a latência da plataforma do Marketo. Se você fizer alterações em uma lista estática ou nos dados de clientes potenciais, é preferível aguardar antes de iniciar os Power Queries.

### Modelagem de Dados no Power Pivot

Abra o Power Pivot clicando no botão &#39;Gerenciar&#39; no menu do Power Pivot, disponível na barra de menu superior (se não estiver disponível, verifique sua versão do Excel, o Power Pivot pode ser instalado como suplemento em algumas versões do Excel). Todos os dados baixados do Marketo e enviados para o modelo de dados devem ser acessíveis a partir das diferentes guias na parte inferior da janela do Power Pivot.

### Expressões de análise de dados (DAX)

Precisamos enriquecer ou reformatar os dados para alguns relatórios. Vamos usar as Expressões de Análise de Dados Power Pivot (DAX) para definir alguns cálculos personalizados como colunas calculadas e medidas (também conhecidos como campos calculados). Consulte o link &#39;DAX no Power Pivot&#39; na seção Referências para saber mais sobre DAX. Verifique se a Área de Cálculo é exibida na janela do Power Pivot; caso contrário, habilite-a no menu Inicial do Power Pivot.  Selecione a guia **MktoLeads** e adicione a medida **Contagem de Clientes Potenciais** em qualquer lugar na Área de Cálculo de Clientes Potenciais: **Contagem de Clientes Potenciais:=**&#x200B;**DISTINCTCOUNT**&#x200B;**([id])**. Essa medida está contando os leads distintos disponíveis na lista, com base em sua id. Também teria em conta os eventuais filtros em vigor no contexto de um relatório. Essa medida não é realmente necessária, pois os relatórios são capazes de resumir o número de leads, mas fizemos isso para ter uma contagem de leads com um nome mais agradável do que &quot;soma de MktoLeads&quot;. Também é um exemplo simples que permite imaginar facilmente algumas medidas mais complexas fazendo médias, mínimo, máximo para um tipo específico de entrada de dados (por exemplo, todos os leads com uma pontuação maior que 50, pontuação média etc.). ...).  Agora vamos selecionar a guia **MktoWebActivities** e criar três colunas calculadas. Insira as seguintes colunas calculadas rolando até a extremidade direita da tabela e clicando na coluna &#39;Adicionar coluna&#39;. **Atividade:** obtenha o rótulo de Atividade amigável pesquisando a Id de Atividade na tabela MktoActivtyTypes. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoActivityTypes[name],MktoActivityTypes[id],[activityTypeId])** **Year-Month:** reformate a Data da atividade com um padrão &#39;AAAAmm&#39; mais adequado para alguns relatórios. **\=**&#x200B;**LEFT**&#x200B;**([activityDate],4)&amp;**&#x200B;**MID**&#x200B;**([activityDate],6,2)** **Data:** A Data da Atividade é apenas uma Cadeia de Caracteres de nossa consulta original, transforme-a em uma data apropriada. **\=**&#x200B;**DATE**&#x200B;**(**&#x200B;**LEFT**&#x200B;**([activityDate],4),**&#x200B;**MID**&#x200B;**([activityDate],6,2),**&#x200B;**MID**&#x200B;**([activityDate],9,2))** Agora vamos criar as três mesmas medidas para a guia **MktoEmailActivities** e 2 medidas adicionais: **Campanha:** obtenha o nome de campanha amigável pesquisando a ID da campanha na tabela MktoCampaigns. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[name],MktoCampaigns[id],[campaignId])** **Programa:** Obtenha o nome amigável do Programa procurando a Id da Campanha na tabela MktoCampaigns. A tabela MktoPrograms pode fornecer mais detalhes sobre o Programa, como pasta, espaço de trabalho, etc. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[programName],MktoCampaigns[id],[campaignId])**

### Entidade-Relações

Vimos anteriormente uma maneira de pesquisar informações de outra tabela no modelo para completar algumas informações ausentes. O Power Pivot oferece uma opção mais eficiente para definir as relações entre algumas tabelas do modelo de dados, permitindo aproveitar essas relações diretamente dos relatórios. Vamos definir as relações principais para nossos relatórios. Selecione a Exibição de Diagrama na janela do Power Pivot. Rastreie as seguintes relações no diagrama do modelo de dados:

* **MktoInterestedMomentActivities:leadId →** **MktoLeads:id**
* **MktoScoringActivities:leadId →** **MktoLeads:id**
* **MktoRevenueStageActivities:leadId →** **MktoLeads:id**
* **MktoWebActivities:leadId →** **MktoLeads:id**
* **MktoEmailActivities:leadId →** **MktoLeads: id**

Não usaremos todos esses relacionamentos e objetos em nossos relatórios, somente os clientes em potencial, atividades da Web e atividades de email. Agora é hora de criar alguns relatórios.

### Gráfico Dinâmico de Desempenho de Emails

Este primeiro relatório mostra KPIs de desempenho de email com base em um Gráfico dinâmico padrão do Excel. Ele permite filtrar dados por Setor e/ou Campanha. Você pode criar um Gráfico Dinâmico diretamente do menu do Power Pivot selecionando &#39;Gráfico Dinâmico&#39; no seletor &#39;Tabela Dinâmica&#39;.  Uma alternativa é criar um Gráfico Dinâmico diretamente da planilha do Excel, marcando a opção &#39;Usar Modelo de Dados desta pasta de trabalho&#39;.  Arraste e solte os campos das tabelas **MktoEmailActivities** e **MktoLeads**, como a figura abaixo: **MktoEmailActivities.Activity →** **Legenda** (isso usa a coluna calculada DAX que implementamos em **MktoEmailActivities** anteriormente) **MktoEmailActivities.Date →** **Eixo** (isso usa a coluna calculada DAX que implementamos em **MktoEmailActivities** anteriormente) **MktoEmailActivities.Id →** **Valores** **MktoEmailActivities.Campaign →** **Filtro** **MktoLeads.industry →** **Filtro**

Você pode criar um nome personalizado selecionando &quot;Configurações de campo de valor&quot; em cada campo solto. Nesse caso, soltamos o campo ID de atividade de email na seção &quot; Valores&quot; e editamos seu nome personalizado como &quot;Número de atividades&quot;. Agora vamos configurar o Gráfico dinâmico. Clique com o botão direito diretamente no gráfico e selecione a opção &#39;Alterar Tipo de Gráfico&#39; no menu contextual. E foi assim que selecionamos o tipo de gráfico diferente para todas as séries de dados.

### Mapa de clientes potenciais com Power View

O segundo relatório exibe seus clientes em potencial e contatos por localização geográfica em um mapa mundial e por setor. Precisamos do Power View para este relatório. Siga o link de referência abaixo para ativar o menu no Excel. Ou você pode apenas digitar &#39;power view&#39; na caixa de pesquisa do Excel. Selecione &#39;Inserir um Relatório do Power View&#39;.  No relatório em branco do Power View, selecione a tabela **MktoLeads** no painel direito e arraste e solte o campo de localização do lead (por exemplo, **inferredCity**). Agora o menu &#39;Design&#39; aparece no menu principal.

Alterne para a visualização de Mapa selecionando &#39;Mapa&#39; no menu &#39;Design&#39; do Power View. Arraste e solte os campos da tabela **MktoLeads**, como a figura abaixo: **MktoLeads.industry →** **Color** **MktoLeads.inferredCity →** **Locations** **MktoLeads.Leads Count →** **Size** (use a medida DAX que implementamos em **MktoLeads** anteriormente) e seus Leads o mapa está pronto! Basta ajustar o tamanho do mapa, personalizar o título e as legendas. O Power View permite criar painéis avançados com vários gráficos em uma única planilha. Confira o tutorial referenciado abaixo &#39;[Criar relatórios incríveis do Power View](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)&#39; para ver como proceder com mais componentes do painel com o Power View.

### Atividades da Web animadas em um mapa 3D

Esse terceiro relatório exibe suas atividades principais na Web, por setor, em um mapa de mundo 3D. Precisamos de um mapa 3D para este relatório. Basta digitar &quot;3D&quot; na caixa de pesquisa do Excel e selecionar &quot;Mapa 3D&quot;. Crie um novo tour pela janela pop-up.  Selecione o Gráfico de bolhas no painel direito. Arraste e solte os campos das tabelas **MktoLeads** e **MktoWebActivities**, como a figura abaixo: **MktoLeads.industry →** **Category** **MktoLeads.inferredCity →** **Location** **MktoWebActivities.Activity →** **Time** (use a coluna calculada DAX que implementamos **MktoWebActivities** anterior. O campo de ID também pode ser usado para contar atividades.) **MktoWebActivities.Date →** **Time** (isso usa a coluna calculada DAX que implementamos em **MktoWebActivities** anteriormente) **MktoWebActivities.Activity** também pode ser usado como filtro para filtrar os diferentes tipos de atividades da Web.

Use o botão &quot;Temas&quot; para alterar o esquema de cores do Mapa 3D. Abra as &quot;Opções de cena&quot; para personalizar as animações.
E terminamos com o Mapa do Mundo 3D, agora podemos nos divertir animando o globo e criando vídeos dele.

### Próximas etapas

Nós apenas rabiscamos a superfície do que é possível fazer com as ferramentas do Excel Power BI. Recomendamos que você pesquise na Web por outros excelentes artigos e tutoriais para expandir suas habilidades no Excel e criar os relatórios necessários para atingir suas metas comerciais. Esperamos que você tenha gostado desses artigos e que eles tenham ajudado a aproveitar os excelentes benefícios combinados do Excel e do Marketo.

### Referências

#### Power Pivot

* [Power Pivot: análise e modelagem de dados avançadas no Excel](https://support.microsoft.com/en-us/article/Power-Pivot-Powerful-data-analysis-and-data-modeling-in-Excel-d7b119ed-1b3b-4f23-b634-445ab141b59b)
* [Expressões de Análise de Dados (DAX) no Power Pivot](https://support.microsoft.com/en-us/article/Data-Analysis-Expressions-DAX-in-Power-Pivot-bab3fbe3-2385-485a-980b-5f64d3b0f730)

#### Power View

* [Ativar o Power View no Excel 2016](https://support.microsoft.com/en-us/article/Turn-on-Power-View-in-Excel-2016-for-Windows-f8fc21a6-08fc-407a-8a91-643fa848729a)
* [Tutorial: Criar Relatórios Surpreendentes do Power View](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)

Publicado em _2017-02-02_ por _Philippe_

## Alteração importante nos registros de atividade na API do Marketo

**Observação: esta postagem será atualizada para refletir as alterações feitas nos registros de atividade retornados pela API devido à migração para a nova infraestrutura.** **Última atualização: 13 de setembro de 2018** Com a implantação do Serviço de Atividade de próxima geração da Marketo a partir de setembro de 2017, não poderemos impor a exclusividade ou a presença do campo de número inteiro &quot;id&quot; em atividades, alterações no valor dos dados ou registros de exclusão de clientes potenciais retornados pelas APIs da Marketo. Para evitar interrupções do serviço em integrações que recuperam registros de atividade, o campo id deve ser tratado como opcional. A transferência desta alteração começará a afetar as assinaturas e a versão futura. Essa alteração afetará os seguintes endpoints: REST API

Os tipos de SOAP afetados são `ActivityRecord` e `LeadChangeRecord`.

### Exemplos

Os exemplos a seguir mostram os tipos de registro que serão afetados. Em ambos os exemplos, o campo afetado é chamado de &quot;id&quot;.

**Exemplo de campo REST: id**

```json
  {
      "id" : 102988,
      "leadId" : 1,
      "activityDate" : "2015-01-16T23:32:19Z",
      "activityTypeId" : 1,
      "primaryAttributeValueId" : 71,
      "primaryAttributeValue" : "localhost/munchkintest2.html",
      "attributes" : [
          {
              "name" : "Client IP Address",
              "value" : "10.0.19.252"
          },
          {
              "name" : "Query Parameters",
              "value" : ""
          },
          {
              "name" : "Referrer URL",
              "value" : ""
          },
          {
              "name" : "User Agent",
              "value" : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
          },
          {
              "name" : "Webpage URL",
              "value" : "/munchkintest2.html"
          }
      ]
  }
```

**Exemplo de campo do SOAP: id**

```xml
  <activityRecord>
    <id>1030680</id>
    <activityDateTime>2013-07-09T16:44:28-05:00</activityDateTime>
    <activityType>Visit Webpage</activityType>
    <mktgAssetName>ClickDemo</mktgAssetName>
    <activityAttributes>
      <attribute>
        <attrName>Webpage ID</attrName>
        <attrType xsi:nil="true" />
        <attrValue>2547</attrValue>
      </attribute>
      <attribute>
        <attrName>Webpage URL</attrName>
        <attrType xsi:nil="true" />
        <attrValue>/ClickDemo.html</attrValue>
      </attribute>
    </activityAttributes>
    <campaign />
    <personName xsi:nil="true" />
    <mktPersonId>1089965</mktPersonId>
    <foreignSysId xsi:nil="true" />
    <orgName xsi:nil="true" />
    <foreignSysOrgId xsi:nil="true" />
  </activityRecord>
```

Publicado em _2017-03-01_ por _Kenny_

## Atualizações do inverno de 2017

Na versão inverno 2017, adicionamos a capacidade de importar dados de objetos personalizados em massa de forma assíncrona e manipular variáveis em páginas de aterrissagem guiadas. Consulte a lista completa de atualizações abaixo.

### APIs de banco de dados de clientes potenciais

#### Importação em massa de objetos personalizados

Novos endpoints para oferecer suporte à Importação em massa de Objetos Personalizados. Detalhes podem ser encontrados [aqui](/help/rest-api/custom-objects.md).

#### Aviso de futura alteração das atividades do

Observe uma alteração significativa que ocorrerá quando a Marketo lançar seu Serviço de atividade de última geração. Essa alteração afetará os seguintes endpoints:

SOAP

[getLeadActivity](/help/soap-api/getleadactivity.md), [getLeadChanges](/help/soap-api/getleadchanges.md)

O campo inteiro &quot;id&quot; contido nos registros retornados por esses endpoints não será mais garantido como único. Isso afetará os tipos de registro de atividade, alteração do valor dos dados e exclusão de clientes potenciais. Para evitar interrupções do serviço em integrações que recuperam esses tipos de registro, o campo id deve ser tratado como opcional.

#### Remoção do token de push

Adição da capacidade de remover tokens de push por meio da API do SDK. Detalhes podem ser encontrados aqui: [iOS](/help/mobile/push-notifications.md), [Android](/help/mobile/push-notifications.md).

Publicado em _2017-03-01_ por _David_

## Atualizações do primeiro trimestre de 2017

Na versão lançada no segundo trimestre de 2017, adicionamos a capacidade de extrair dados de clientes potenciais e objetos de atividade em massa de forma assíncrona e manipular listas de contas nomeadas. Consulte a lista completa de atualizações abaixo.

### Extração em Massa de Clientes Potenciais

Novos endpoints para dar suporte à extração de leads em massa. Especifique os critérios de seleção de registro usando uma variedade de opções. Detalhes podem ser encontrados [aqui](/help/rest-api/bulk-lead-extract.md).

### Extração em massa de atividades

Novos endpoints para dar suporte à extração de Atividades em massa. Especifique os critérios de seleção de registro usando o intervalo de datas e a lista de atividades. Detalhes podem ser encontrados [aqui](/help/rest-api/bulk-extract.md).

### Listas de contas nomeadas

Novos endpoints para dar suporte a operações CRUD em Listas de Contas Nomeadas. Detalhes podem ser encontrados [aqui](/help/rest-api/named-account-lists.md).

### Outras melhorias

* O ponto de extremidade [Obter Campanhas](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignByIdUsingGET) agora permite filtrar campanhas &quot;acionáveis&quot;. Isso é feito transmitindo &quot;isTriggerable=true&quot; como um parâmetro de consulta.
* O ponto de extremidade [Clonar Programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) agora dá suporte a programas que contêm todos os tipos de ativos, exceto Mensagens SMS.

### Iônico

Agora você pode usar o MME do Marketo Mobile e com a estrutura de aplicativo [Ionic](https://ionicframework.com/). Detalhes podem ser encontrados [aqui](/help/mobile/ionic.md).

### Cancelar o registro do token de notificação por push

Um novo método foi adicionado ao SDK que permite cancelar o registro do token de notificação por push com o Marketo. Isso é útil, por exemplo, quando um usuário faz logoff do seu aplicativo. Para uso, veja `unregisterPushDeviceToken` (iOS) ou `uninitializeMarketoPush` (Android) método [aqui](/help/mobile/push-notifications.md).

Publicado em _2017-06-16_ por _David_

## Internet das coisas para profissionais de marketing com IFTTT e Zapier

A Internet das Coisas (IoT) é a inter-rede de dispositivos conectados, eletrodomésticos, dispositivos utilizáveis, veículos, etc. com produtos eletrônicos, software, sensores e conectividade de rede incorporados que permitem que esses objetos coletem e troquem dados com sistemas de informações em nuvem. Essas tecnologias estão crescendo e crescendo tão rapidamente que afetarão a maneira como vivemos, trabalhamos e fazemos negócios rapidamente. A Marketo, a principal plataforma de envolvimento de marketing, está pronta para a IoT com seus recursos para dimensionar e interagir com qualquer forma de canal de comunicação. O Marketo já pode rastrear mais de 70 tipos de atividades relacionadas a emails, Web, dispositivos móveis, CRM etc.; ele também oferece suporte a [atividades personalizadas](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-activities/create-a-custom-activity.html?lang=pt-BR) que podem ser alimentadas por qualquer sistema de terceiros. Os [objetos personalizados](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects.html?lang=pt-BR) do Marketo possibilitam o rastreamento de todos os tipos de métricas de terceiros relacionadas à sua empresa e permitem que os profissionais de marketing aproveitem essas métricas diretamente dos filtros e acionadores da campanha inteligente do Marketo. A implementação da IoT para consumidores exigiria um servidor centralizado para interagir com dispositivos de consumidor e esse servidor trocaria dados com a plataforma aberta do Marketo, com recursos como API REST, Objetos personalizados, Atividades personalizadas etc. - documentado [aqui](http://eto.com/). Não é fácil demonstrar através de uma publicação de blog. Em vez disso, vamos integrar o Serviço IFTTT ao Marketo para implementar alguns casos de uso interessantes da IoT para os profissionais de marketing como:

* Anime sua equipe de marketing sempre que um cliente potencial é registrado em um evento ao piscar uma luz colorida no escritório
* Anime sua equipe de vendas cada vez que um negócio é conquistado, disparando automaticamente um sino conectado a uma tomada de energia
* Publicar automaticamente marcos de sucesso de marketing em redes sociais, como LinkedIn, Facebook, Slack, etc. ..
* Iniciar automaticamente uma Campanha de marketing com base em:
   * quando ocorre um alerta de clima (vento, temperatura, chuva, etc.)
   * quando um novo artigo é publicado por um jornal como o New York Times, correspondendo a alguns critérios específicos
   * quando o Senado ou a Câmara dos Deputados dos EUA votarem
   * quando a Estação Espacial Internacional passa por um determinado local
   * etc. ..

Você pode achar esses cenários divertidos, mas inúteis, mas eles estão aqui para demonstrar uma nova maneira conceitual de fazer Marketing não só com as pessoas, mas também com as coisas em nosso mundo conectado. Outro ponto interessante abordado neste artigo é como aproveitar uma plataforma de integração Web aberta, como o Zapier, como uma &quot;hachura de serviço&quot; entre um sistema de terceiros e o Marketo, para gerenciar a autenticação, por exemplo.

### O serviço IFTTT

IFTTT é um acrônimo para &quot;IF This Then That&quot;. É um serviço gratuito com base na Web que as pessoas usam para criar cadeias de declarações condicionais simples, chamadas de applets. Um applet é acionado por alterações que ocorrem em alguns serviços Web de parceiros e, como resultado, ações são enviadas a outros serviços Web de parceiros. O IFTTT foi lançado em 2011 por Linden Tibbets, Jesse Tane, Scott Tong e Alexander Tibbets em São Francisco. À primeira vista, o IFTTT é semelhante a um serviço como o [Zapier](https://zapier.com/), por exemplo, ele tem um foco muito mais forte nos consumidores e nos dispositivos da IoT (controles remotos, alarmes, luzes, termostatos, carros, impressoras, telefones celulares e muito mais).  Primeiro, você deve criar uma conta para o IFTTT do [site do IFTTT](https://ifttt.com/explore). Sinta-se à vontade para descobrir todos os miniaplicativos legais já disponíveis, pois isso lhe dará algumas outras ideias de cenários com certeza!

### O Canal de Criador

Um aplicativo web que não tem um canal, ou seja, uma parceria com o IFTTT, deve usar o Maker Channel. Com o Canal Maker, você pode criar miniaplicativos que funcionam com qualquer dispositivo ou aplicativo que pode fazer ou receber uma solicitação da Web. Ele oferece as seguintes integrações:

* Acionadores de entrada para receber uma solicitação da Web de um sistema de terceiros para acionar uma ação
* Ações de saída para fazer uma solicitação da Web para um sistema de terceiros publicamente acessível na Internet

Em IFTTT, procure o serviço &quot;Criador&quot; e clique nele.  Na primeira vez, você precisa ativá-lo clicando no botão &quot;Connect&quot;.  Agora o Canal de criação está ativo. Você pode obter sua chave secreta clicando no botão Configurações do Criador: copie e cole o URL fornecido no seu navegador para obter mais detalhes.

### Acionamento direto de uma ação IFTTT do mercado

Primeiro, vamos nos concentrar em acionar todos os tipos de ações de serviços da Web de terceiros a partir do Marketo. Para isso, usaremos um [Webhook do Marketo](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook.html?lang=pt-BR). Começaremos com uma mensagem por push em seu celular ou tablet por meio do aplicativo móvel IFTTT e, em seguida, implementaremos um cenário de IoT piscando uma luz Philips Hue.

### Webhook do Marketo

Acionar um evento do Marketo, agindo como o &quot;se&quot; do IFTTT, é simples. Tudo o que você precisa fazer é enviar uma solicitação da Web POST para o IFTTT com um nome de evento e sua chave secreta, seguindo este URL padrão:

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}>`

O Criador também permite comunicar até 3 parâmetros por meio de solicitação da web. Isso pode ser feito usando parâmetros de consulta,

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={value1}&value2={value2}&value3={value3}>`

ou usando um corpo JSON que consiste em até três valores:

`{ "value1" : "{value1}", "value2" : "{value2}", "value3" : "{value3}" }`

No Marketo, crie um novo Webhook a partir da interface de administrador.  Forneça as seguintes informações para o novo Webhook:

**Nome do Webhook:** Êxito do Programa IFTTT

**Descrição:** acionar um evento no IFTTT de uma Campanha Inteligente para obter êxito no Programa

**URL:** `<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={{program.name}}&value2={{lead.Email> Address}}&value3={{lead.Full Name}}`

event_name, use MarketoProgramSuccess, por exemplo

secret_key, use a chave secreta do seu Serviço Criador IFTTT

Use texto estático ou tokens do Marketo para os três valores disponíveis. Você pode enviar mais mensagens interativas definindo seus próprios tokens no nível do programa e transmiti-los por esses valores.

**Tipo de Solicitação:** POST

**Modelo:** Deixe em branco

**Solicitar Codificação de Token:** Formulário/Url

**Tipo de resposta:** nenhum

Não é necessário definir um mapeamento de resposta.

### Miniaplicativo IFTTT

No portal da Web IFTTT, selecione &quot;Meus miniaplicativos&quot; no menu principal.  Clique no botão &quot;Novo Miniaplicativo&quot; e clique na seção **+this**.  Procure o serviço Criador.  Crie o Gatilho que será acionado sempre que o serviço de Criador receber uma solicitação da Web para notificá-lo de um evento. Use o mesmo Nome de evento que o especificado no URL do seu Webhook do Marketo, por exemplo &quot;MarketoProgramSuccess&quot; e clique no botão &quot;Criar acionador&quot;.  Agora é hora de especificar o Serviço de Ação clicando na seção **+that**.  Vamos começar com um serviço de ação simples que qualquer pessoa poderia testar sem precisar investir em dispositivos IoT, o Serviço de notificações. Procure e selecione o Serviço de notificações.
Escolha a ação &quot;Enviar uma notificação&quot; que envia uma notificação para seus dispositivos.  Você pode aproveitar os 3 valores que enviou do Marketo adicionando-os como Ingredientes para fornecer uma notificação significativa ao usuário, como no exemplo abaixo ... E então clique no botão &quot;Criar ação&quot;. Revise e finalize o miniaplicativo IFTTT. Verifique se ele está ativado.

### Testando o miniaplicativo IFTTT

Se quiser ser notificado no seu dispositivo móvel, primeiro baixe o aplicativo IFTTT do seu dispositivo.  É possível acionar um evento de sucesso do programa do Marketo usando o Webhook em um fluxo do Marketo Smart Campaign. Lembre-se de que os Webhooks do Marketo funcionam exclusivamente com Campanhas inteligentes acionadas (por exemplo, acionar depois que um formulário de contato preenchido, for adicionado a uma lista etc.).  E aqui está um exemplo de uma notificação de IFTTT em seu celular.

### Vamos obter o Creative com IFTTT

A IFTTT oferece Applet Actions com mais de 300 parceiros, de modo que seu portfólio de aplicativos e aparelhos junto com sua imaginação são os limites... Vamos ver um exemplo com as [luzes de matiz da Philips](https://www.philips-hue.com/en-us) que você pode comprar em qualquer lugar em lojas de eletrônicos ou online. Os miniaplicativos a seguir piscariam uma de suas luzes com a cor atribuída atualmente quando o Marketo aciona um programa bem-sucedido, o que poderia impulsionar sua equipe de marketing no escritório. Criamos um novo miniaplicativo, seguindo as mesmas etapas de antes, em que o Marketo é acionado com um webhook, mas dessa vez escolhemos a ação no serviço Philips Hue.

Vamos selecionar a ação &quot;Piscar luzes&quot;. O aplicativo irá solicitar a Philips Hue todas as suas luzes disponíveis, para que você possa escolher o que piscar. Você precisaria configurar uma conta com a Philips Hue primeiro, a ponte Hue e, é claro, pelo menos uma lâmpada Hue, faixa de luz, projetor ou lâmpada.  Acabamos de adicionar um novo miniaplicativo que piscará uma luz colorida sempre que um cliente potencial for registrado em um show ou webinário. Sua equipe de marketing se animará todos os dias com essa configuração no escritório.

### Execução de uma ação Marketo do IFTTT, via Zapie

Agora, acionaremos uma Campanha inteligente do Marketo a partir da plataforma IFTTT. Para isso, usaremos a [API REST do Marketo](/help/rest-api/rest-api.md). Como essa API é segura e requer uma Autenticação OAuth2 antes de chamar qualquer coisa, precisamos lidar com essa autenticação por meio de outra plataforma, como a Zapier, porque o IFTTT não permite encadear duas chamadas consecutivas em uma API com o Canal do Criador. Escolhemos o Serviço de automação do aplicativo web [Zapier](https://zapier.com/), pois publicamos já apresentando o Zapier e explicando passo a passo como implementar um conector Marketo personalizado para o Zapier. Outras plataformas, como o [Workato](https://www.workato.com/integrations/marketo), também podem ser uma solução.

### Campanha Marketo

Crie seu programa do Marketo com uma Campanha inteligente programada. Para fins de teste, você poderia criar o seguinte Campaign Inteligente como exemplo: **Smart List** Use apenas filtros, não acionadores. Certifique-se de que pelo menos você se qualificaria. **Fluxo** Enviar um email ou qualquer outro tipo de notificação. **Agendar** Verifique se você pode executar o fluxo todas as vezes para manipular seus vários testes. Você pode obter a ID do Campaign inteligente do URL. Exemplo: _https://{{marketo_url}}/#SC4289A1_ - a ID da Campanha Inteligente seria 4289. Você pode acionar essa campanha por meio da API REST do Marketo. Você pode usar, por exemplo, o plug-in [Postman](https://www.postman.com/) para o Chrome e enviar as 2 seguintes chamadas HTTPS consecutivas: **Etapa de autenticação:**

`https://{{Your Munchkin_Account_id}}.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id={{Your_Client_Id}}&client_secret={{Your_Client_Secret}}`

Recupere o token de acesso da resposta JSON. **Etapa inicial da campanha:**

`https://{{Your_Munchkin_Account_id}}.mktorest.com/rest/v1/campaigns/{{Campaign_Id}}/schedule.json?access_token={{access_token}}`

### Conector personalizado intermediário do Zapier para iniciar a campanha do Marketo

Precisamos criar um conector Zapier personalizado que se autentique com a API REST do Marketo e inicie nossa Campanha inteligente.

* Pré-requisitos
* Implementação do conector do Marketo para Zapier
* Use um título diferente, como &quot;Campanha do Marketo&quot;
   * Execute a etapa &quot;Autenticação&quot;
   * Execute a etapa &quot;Triggers&quot; (necessária para fins de teste do Zapier)
   * Faça a seguinte etapa específica &quot;Ações&quot;, responsável por iniciar uma campanha do Marketo, explicada abaixo:

Onde Enviar o URL do Ponto de Extremidade da Ação de Dados:

`https://{{munchkin_account_id}}.mktorest.com/rest/v1/campaigns/{{CampaignId}}/schedule.json`

Deixe em branco os outros campos opcionais.

#### API de script

O recurso de script do Zapier permite manipular as solicitações e respostas trocadas entre a API do aplicativo e o Zapier. Você pode modificar solicitações HTTP antes que sejam enviadas e pode analisar respostas antes que o Zapier faça qualquer coisa com elas. Precisamos dele para concluir nossa autenticação personalizada de &quot;Autenticação de sessão&quot;. Mais informações estão disponíveis no artigo original. Copie o código a seguir de forma muito semelhante ao original, acabamos de alterar os métodos de ação:

```javascript
var Zap = {

 get_session_info: function(bundle) {

 console.log('Entering get_session_info method ...');

 var access_token,
 access_token_request_payload,
 access_token_response;


 // Assemble the meta data for our Access Token swap request
 console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
 access_token_request_payload = {
 method: 'POST',
 url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
 params: {
 'grant_type' : 'client_credentials',
 'client_id' : bundle.auth_fields.client_id,
 'client_secret' : bundle.auth_fields.client_secret
 },
 headers: {
 'Content-Type': 'application/json', // Could be anything.
 Accept: 'application/json'
 }
 };

 // Fire off the Access Token request.
 access_token_response = z.request(access_token_request_payload);

 // Extract the Access Token from returned JSON.
 access_token = JSON.parse(access_token_response.content).access_token;
 console.log('New Access_Token=' + access_token);

 // This will be mixed into bundle.auth_fields in future calls.
 //bundle.auth_fields.access_token=access_token;
 return {'access_token': access_token};
 },

 test_trigger_pre_poll: function(bundle) {

 console.log('Entering test_trigger_pre_poll method ...');

 bundle.request.params = {
 'access_token':bundle.auth_fields.access_token
 };

 return bundle.request;

 },

 test_trigger_post_poll: function(bundle) {

 console.log('Entering test_trigger_post_poll method ...');

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }

 return JSON.parse(bundle.response.content);
 },

 launch_campaign_pre_write: function(bundle) {

 bundle.request.params = {'access_token':bundle.auth_fields.access_token};
 return bundle.request;
 },

 launch_campaign_post_write: function(bundle) {

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }
 return JSON.parse(bundle.response.content);
 }

};
```

##### Novo Zap

No Painel Zapier, clique no botão &quot;Criar um novo Zap&quot;. **Acionador**

* Escolha o aplicativo acionador &quot;Webhooks by Zapier&quot;
* Marque &quot;Capturar gancho&quot;
* Não há necessidade de escolher uma chave filha
* O Zapier gerou um URL de webhook personalizado para você enviar solicitações para, mantenha-o seguro em algum lugar
* Teste o URL do webhook iniciando o cenário &quot;Miniaplicativo IFTTT que chama o Webhook Zapier&quot; abaixo. Isso permite que o Zapier saiba mais sobre a carga do webhook e atribua a ID da campanha à Ação

**Ação**

* Selecione o conector do Marketo Campaign criado anteriormente
* Escolha a única ação disponível: **Iniciar campanha**
* Conecte-se à sua conta do Marketo, preenchendo os parâmetros de autenticação (ID da conta do Munchkin, ID do cliente, Segredo do cliente)
* Edite o Template e associe a ID da campanha do acionador ao parâmetro &quot;Launch Campaign&quot; da ID da campanha
* Teste a etapa e verifique se o Marketo Campaign é iniciado

### Miniaplicativo IFTTT que chama o Webhook Zapier

Começamos com um cenário simples e fácil de testar. Escolhemos no IFTTT um acionador de Data e hora que iniciará a Campanha do Marketo a cada hora. A ação é uma solicitação da Web enviada ao URL do Zapier Webhook e que transmite a ID do Smart Campaign. Verifique se o Zapier Zap e o miniaplicativo IFTTT estão ativos e teste se tudo está funcionando como esperado.

### Vamos obter o Creative com IFTTT

O IFTTT oferece Acionadores de Miniaplicativo com mais de 300 parceiros; portanto, novamente, seu portfólio de aplicativos e equipamentos, juntamente com sua imaginação, são os limites... Vamos ver um exemplo com o serviço [Weather Underground](https://www.wunderground.com/) que vamos usar para lançar nossa campanha do Marketo sobre alertas climáticos. O acionador a seguir seria acionado quando uma condição de chuva fosse anunciada. E, em seguida, associe o Acionador com a Ação de Webhook do criador e, como anteriormente, preencha os parâmetros de webhook do Zapier.  Et voila, você só precisa agora de uma boa chuva para ver se isso está realmente funcionando.

Esperamos que você se divirta aplicando os conceitos fornecidos neste artigo. Mas o mais importante é que achamos que isso ajudará qualquer um que queira integrar o Marketo a outros sistemas de terceiros, graças aos principais conceitos deste artigo:

* API REST DO MARKETO
* Webhooks do Marketo
* Como aproveitar uma plataforma de integração aberta da Web, como o Zapier, como uma &quot;hachura de servidor&quot; entre um sistema de terceiros e o Marketo para gerenciar a autenticação, por exemplo

Publicado em _2017-06-20_ por _Philippe_

## Atualizações do segundo trimestre de 2017

Na versão lançada no terceiro trimestre de 2017, lançamos algumas melhorias secundárias às APIs do programa.

### Procurar Programas

Estamos adicionando a capacidade de obter programas por intervalo de datas ao nosso ponto de extremidade [Obter Programas](https://developer.adobe.com/marketo-apis/api/asset/#operation/browseProgramsUsingGET), por meio da adição dos parâmetros opcionais firstUpdatedAt e latestUpdatedAt. Você pode definir um ou ambos os parâmetros com a data e hora de sua escolha para retornar apenas os programas que foram criados ou atualizados entre as duas datas e horas. Isso é útil para recuperar conjuntos novos e atualizados de material de apoio de marketing, principalmente para casos de uso de tradução e business intelligence.

Publicado em _1970-01-01_ por _Kenny_

## Estender a lógica de negócios do Marketo com a função da Google Cloud -

Este artigo propõe uma solução para estender o Marketo com alguns recursos de lógica de negócios com a Google Cloud Platform (GCP), com base no seguinte exemplo simples: 3 campos personalizados no registro de lead da Marketo:

* **OnLinePreference**: uma pontuação incremental que indica uma audiência de prospecto/cliente para comunicações online.
* **OfflinePreference**: uma pontuação incremental que indica uma audiência de cliente potencial/cliente para comunicações offline.
* **Preferência**: um campo calculado pelo GCP que exibe &quot;offline&quot; se a pontuação offline for maior que a online, e &quot;online&quot; ao contrário

Essa tecnologia abre caminho para uma lógica de negócios mais avançada e, eventualmente, para chamar serviços Web externos, transformando e consolidando os resultados no Marketo.  

### Sobre a plataforma e as funções da Google Cloud

A [Google Cloud Platform](https://cloud.google.com/) (GCP) é um conjunto de serviços de computação em nuvem executados na mesma infraestrutura que a Google usa internamente para seus produtos de usuário final, como o Google Search e o YouTube. Juntamente com um conjunto de ferramentas de gerenciamento, ele fornece uma série de serviços modulares em nuvem, incluindo computação, armazenamento de dados, análise de dados, aprendizado de máquina, big data e muito mais. Poderíamos ter usado vários serviços GCP diferentes para a nossa necessidade, como o Mecanismo de computação, Mecanismo de aplicativo ou Mecanismo Kubernetes, mas optamos pelas [Funções da nuvem](https://cloud.google.com/functions) (ainda no Beta) para as seguintes vantagens principais:

* A computação em nuvem sem servidor é onde a lógica pode ser ativada sob demanda em resposta a eventos como chamadas HTTP.
* Alivia a maior parte dos problemas causados pela manutenção e pelas implantações do servidor.
* Econômica, já que você paga GCP somente para cada chamada de função e não para manter um servidor ativo e em funcionamento.
* Simples e rápido de implementar, pois você se concentra somente na lógica do aplicativo.
* Dimensionamento automático, pronto para cargas de trabalho muito altas.

Verifique o [site de GCP](https://cloud.google.com/) para obter mais informações sobre esta tecnologia e seus preços. Normalmente, este tutorial não deve induzir nenhum custo importante e se encaixará perfeitamente no crédito gratuito de uma avaliação de GCP.  

### Preparação do ambiente de nuvem do Google

Você precisa de uma conta da Google Cloud. Você pode experimentar o GCP gratuitamente com um crédito mais do que suficiente para executar este tutorial, basta clicar no botão &quot;Experimentar gratuitamente&quot; no [site do GCP](https://cloud.google.com/). Siga todas as etapas da seção &#39;Antes de começar&#39; no [Tutorial HTTP](https://cloud.google.com/functions/docs/calling) do Google:

1. Criar um projeto da Cloud Platform: ACESSE A PÁGINA GERENCIAR RECURSOS
1. Habilitar faturamento para o seu projeto: [HABILITAR FATURAMENTO](https://cloud.google.com/billing/docs/how-to/modify-project?visit_id=638816637273392093-1926929734&rd=1)
1. Habilitar a API de Funções de Nuvem: [HABILITAR A API](https://accounts.google.com/InteractiveLogin?continue=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&followup=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&osid=1&passive=1209600&service=cloudconsole&ifkv=ASKV5Mh81NGNsqcJqhx7hst0KFnyA0MJ-2zay8ovyluBfpvDoM820nF9Wq_SKbC1m_sjQvvRNoKSuA)
1. [Instalar e inicializar o Cloud SDK](https://cloud.google.com/sdk/docs/)
1. Atualizar e instalar componentes do Gcloud

   **atualização de componentes da nuvem e** **instalação beta de componentes da nuvem**

1. Prepare seu ambiente para o desenvolvimento do Node.js: [CONSULTE O GUIA DE CONFIGURAÇÃO](https://cloud.google.com/nodejs/docs/setup)

### Implementação da função scoreCompare Cloud

1. Crie um bucket de Armazenamento na nuvem para preparar seus arquivos de Funções na nuvem. Você pode fazer isso com a linha de comando:

   `gsutil mb gs://[YOUR_STAGING_BUCKET_NAME]`

   ou na interface da Web do Google Cloud, selecionando o projeto e clicando no menu Armazenamento:
   * Dê um nome exclusivo ao bucket de armazenamento
   * Selecionar a classe de armazenamento padrão
   * Selecione a localização regional mais adequada

1. Crie um diretório em seu sistema local para o código do aplicativo.
1. Crie um arquivo &quot;index.js&quot; neste diretório com o seguinte código JavaScript: o código é realmente simples de entender. Ele analisa os dois parâmetros de entrada do corpo da solicitação HTTP em JSON, faz o processamento e codifica em JSON a resposta HTTP.

```javascript
 /\*\*
     \* HTTP scoreCompare Cloud Function.
     \*
     \* @param {Object} req Cloud Function request context.
     \* @param {Object} res Cloud Function response context.
     \*/
    exports.scoreCompare = function scoreCompare (req, res) {
     var onlineScore=parseInt(req.body.onlineScore);
     var offlineScore=parseInt(req.body.offlineScore);
     console.log('/scoreCompare: got values onlineScore =' + onlineScore + ', offlineScore =' + offlineScore);
     var result;
     if (onlineScore>offlineScore) {result = 'online';} else {result = 'offline';}
     console.log('/scoreCompare: and result is ' + result);
     res.status(200).json({output: result}).end();
    };
```

Implante a função scoreCompare com um acionador HTTP. Execute o seguinte comando no diretório:

**implantação de funções beta da nuvem [FUNCTION] —stage-bucket [YOUR_STAGING_BUCKET_NAME] —trigger-http**

Onde [YOUR_STAGING_BUCKET_NAME] é o nome do seu bucket de armazenamento na nuvem de preparo. No nosso exemplo:

**pontuação de implantação de funções beta da nuvemComparar —stage-bucket mktostorage —trigger-http**

1. Observe a URL de Função na Nuvem (URL httpsTrigger) na saída do console, que tem a seguinte aparência: `https://[YOUR_REGION]-[YOUR_PROJECT_ID].cloudfunctions.net/[FUNCTION]` onde

   * [SUA_REGIÃO] é a região onde sua função está implantada. Isso fica visível no terminal quando a implantação da função é concluída.
   * [YOUR_PROJECT_ID] é a ID do seu projeto na nuvem. Isso fica visível no terminal quando a implantação da função é concluída.
   * [FUNCTION] é seu nome de função.

   Em nosso exemplo: [**https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare**](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
1. Teste sua função com uma ferramenta como o [Postman](https://www.postman.com/):
   * Verbo HTTP: POST
   * URL: [https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
   * Cabeçalhos: content-type = application/json
   * Corpo: {&quot;onlineScore&quot;:110, &quot;offlineScore&quot;:200}A saída deve fornecer: {&quot;output&quot;: &quot;offline&quot;}.

### Chamar a função de nuvem a partir de um Webhook do Marketo

Os três campos personalizados a seguir devem ser criados no registro de cliente potencial no Marketo:

* **OnlinePreference**: número inteiro
* **OfflinePreference**: número inteiro
* **Preferência**: cadeia de caracteres

Crie o seguinte webhook na interface de administrador do Marketo usando o URL da função de nuvem &quot;scoreCompare&quot; e os tokens do campo personalizado. Teste o webhook com uma campanha inteligente acionada pelo Marketo.

* **Os webhooks do Marketo só podem ser invocados de campanhas inteligentes acionadas, não de campanhas inteligentes em lote.**
* **Se você não usar sua função de nuvem, exclua-a ou exclua o projeto inteiro para evitar a cobrança de tarifas na sua conta da Google Cloud Platform.**

Esperamos que este tutorial tenha valido a pena e que o fará pensar em cenários mais avançados envolvendo processamento complexo e serviços de terceiros. Um bom exemplo seria aproveitar a IA da Google Cloud, os serviços de aprendizado de máquina da Google. Você pode, por exemplo, analisar algum texto livre de um formulário do Marketo e pedir à API da linguagem natural do Google para revelar a estrutura e o significado do texto e depois salvar essa análise no Marketo; apenas abrindo as comportas para ideias.

Publicado em _2017-11-21_ por _Philippe_

## Atualizações do último trimestre de 2017

Na versão lançada no último trimestre de 2017, lançamos várias melhorias nas APIs de ativos. Consulte a lista completa de atualizações abaixo.

### Procurar Programas por Intervalo de Datas

Adicionamos a capacidade de obter programas por intervalo de datas ao nosso ponto de extremidade [Obter Programas](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST). Isso é feito usando os parâmetros `earliestUpdatedAt` e `latestUpdatedAt`. Você pode definir um ou ambos os parâmetros com a data e hora de sua escolha para retornar apenas os programas que foram criados ou atualizados entre as duas datas e horas.

### Visualizar e-mail

Agora, você pode visualizar um email usando o endpoint [Obter Conteúdo Completo de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailFullContentUsingGET), que retorna a versão serializada de HTML de um email. Todos os tokens, trechos, conteúdo dinâmico e componentes integrados são totalmente renderizados. Um parâmetro **leadId** opcional pode ser passado para representar um determinado lead.

### Substituir o HTML do Email 2.0

Adicionamos o ponto de extremidade [Atualizar conteúdo completo do email](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailFullContentUsingPOST) para permitir que você substitua blocos de conteúdo de email do HTML. Se você editar o código HTML de um email do Marketo usando o Editor do Marketo Email 2.0, a relação entre o email e seu modelo será interrompida, mais sobre isso [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html). Usando esse endpoint, é possível atualizar programaticamente o conteúdo do HTML de um email cuja relação foi interrompida. Além disso, modificamos todos os outros endpoints relacionados ao ciclo de vida do email para que sejam compatíveis com emails em que a relação foi interrompida:

* Aprovar rascunho de email
* Cancelar aprovação de email
* Excluir e-mail
* Descartar rascunho de email
* Clonar e-mail
* Atualizar metadados de email

### Outras melhorias

* O período máximo para filtros de intervalo de datas foi aumentado para 31 dias. Isso se refere a [Filtros de Extração de Lead em Massa](/help/rest-api/bulk-lead-extract.md) (createdAd ou updatedAt) e [Filtro de Extração de Atividade em Massa](/help/rest-api/bulk-activity-extract.md) (createdAt).

Publicado em _2017-12-15_ por _David_

## TEST - Link de vídeo externo na comunidade

[Vídeo do Tutorial de Entrega de Power Pack por Email](https://nation.marketo.com:443/t5/product-space-archive-videos/email-deliverability-power-pack-tutorial-video/m-p/283550)  

Publicado em _1970-01-01_ por _David_

## Atualizações do inverno de 2018

Na versão inverno de 2018, estamos lançando algumas melhorias nas APIs. Consulte a lista completa de atualizações abaixo.

### Ativar/desativar campanhas de acionadores

Adicionamos a capacidade de ativar e desativar campanhas de acionador, o que pode simplificar o processo de automatização dos modelos do programa. Isso é feito chamando dois pontos de extremidade adicionados recentemente: [Ativar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/#operation/activateSmartCampaignUsingPOST), [Desativar Campanha Inteligente](https://developer.adobe.com/marketo-apis/api/asset/#operation/deactivateSmartCampaignUsingPOST). Consulte a seção Acionador na documentação de [Campanhas](/help/rest-api/assets.md) para obter detalhes.

### Obter programa por nome

Foram adicionados dois parâmetros ao ponto de extremidade [Obter Programa por Nome](https://developer.adobe.com/marketo-apis/api/asset/#operation/getProgramByNameUsingGET) para facilitar a recuperação de custos do programa e marcas do programa. Consulte os parâmetros **includeCosts** e **includeTags** na documentação de [Programas](/help/rest-api/assets.md) para obter detalhes.

### Outras melhorias

A API de extração em massa agora é &quot;sensível ao espaço de trabalho&quot;. Ao criar um Usuário Somente de API para um [Serviço Personalizado](/help/rest-api/custom-services.md), você deve selecionar uma Função de Usuário com Acesso de API para um ou mais espaços de trabalho. Anteriormente, o Serviço personalizado recebia acesso a todos os espaços de trabalho. Agora, o Serviço personalizado recebe acesso somente aos espaços de trabalho que foram selecionados durante a criação do Usuário somente API.

Publicado em _2018-03-02_ por _David_

## Atualizações do primeiro trimestre de 2018

Na versão lançada no segundo trimestre de 2018, lançamos novas APIs REST e aprimoramentos de privacidade de rastreamento na Web. Consulte a lista completa de atualizações abaixo.

API REST

### CRUD da lista estática

Permite que os usuários criem, leiam, atualizem e excluam remotamente registros de listas estáticas. Permite o gerenciamento de todo o ciclo de vida de uma lista estática por meio de APIs REST, incluindo o preenchimento e a manutenção da associação. Detalhes podem ser encontrados [aqui](/help/rest-api/static-lists.md).

### Metadados de atividade personalizados

Permite que os usuários criem Registros de atividade personalizados remotamente. Permite o gerenciamento de todo o ciclo de vida de atividades personalizadas por meio de APIs REST, incluindo a manipulação de tipos e atributos de tipo. Detalhes podem ser encontrados [aqui](/help/rest-api/activities.md).

### Metadados da lista inteligente

Permite que os usuários leiam, clonem e excluam remotamente os registros da Smart List. Permite o gerenciamento de listas inteligentes por meio de APIs REST. Detalhes podem ser encontrados [aqui](/help/rest-api/smart-lists.md).

### Privacidade no web tracking

O código de rastreamento Web do Munchkin JavaScript foi aprimorado para incluir os seguintes aprimoramentos relacionados à privacidade:

* Recusar - capacidade de permitir que os visitantes recusem permanentemente o rastreamento web. Detalhes podem ser encontrados [aqui](/help/javascript-api/lead-tracking.md).
* Anonimização de endereço IP - Esteja em conformidade com as regulamentações de privacidade locais e internacionais, fornecendo a capacidade de tornar anônimos os endereços IP de visitantes da Web. Consulte **anonymizeIP** parâmetro de configuração [aqui](/help/javascript-api/configuration.md).

### Outras melhorias

* O ponto de extremidade [Obter Pastas](https://developer.adobe.com/marketo-apis/api/asset/#operation/getFolderUsingGET) agora retorna todas as pastas quando um pai não raiz e maxDepth=1 são especificados.
* O ponto de extremidade [Obter Página de Aterrissagem por Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getLandingPageByIdUsingGET) agora retorna atributos de URL com o protocolo em todos os casos (http:// ou https://).

Publicado em _2018-06-29_ por _David_

## Atualizações do segundo trimestre de 2018

A versão do Verão de 2018 é principalmente uma versão de manutenção composta por pequenas melhorias e resoluções de defeitos. Consulte a lista completa de atualizações abaixo.

### API REST

* Adição de suporte para campos de Disposição de email que foram omitidos desnecessariamente originalmente. Esses campos agora estarão disponíveis para leitura e gravação em REST, conforme apropriado.
   * Na lista de bloqueios
   * Campanha de marketing suspensa
   * E-mail suspenso
   * Urgência relativa
* O ponto de extremidade [Obter Clientes Potenciais por Tipo de Filtro](https://developer.adobe.com/marketo-apis/api/lead-database-endpoint-reference/#/Leads/getLeadsByFilterUsingGET) agora dá suporte a leadPartionId como um filterType.

### Resoluções de Defeitos

**Ponto de extremidade REST**

**Descrição**

[Aprovar Programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveProgramUsingPOST)

Se você tiver definido Bloquear emails não operacionais como falso ao criar o programa, uma chamada para Aprovar programa será redefinida para verdadeiro.

[Extração em massa](/help/rest-api/bulk-extract.md)

Alguns caracteres Unicode foram corrompidos no arquivo de saída de extração.

[Clonar Programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

* Se você clonou o Programa de email, a lógica do filtro SmartList foi redefinida como &quot;Todos&quot; no programa resultante, independentemente da configuração inicial.
* Se você tentou clonar um programa que continha uma lista estática (que foi excluída), recebeu um erro 709, &quot;Os seguintes ativos não são suportados:List&quot;.
* Se você tentou clonar um programa em espaços de trabalho, recebeu o erro 611, &quot;Não é possível clonar o programa&quot;.

[Obter Lista Estática por Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getStaticListByIdUsingGET)

Se seu Serviço personalizado tivesse permissão de função de Ativo somente leitura, você receberia um erro 603, &quot;Acesso negado&quot;.

[Enviar cliente em potencial para o Marketo](https://developer.adobe.com/marketo-apis/api/mapi/#operation/pushToMarketoUsingPOST)

Se o atributo **cookies** foi especificado em um objeto de lead de matriz **input**, a atividade anônima anterior não foi associada ao lead recém-criado.

[Campanha programada](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST)

Se você especificou uma data **runAt** bem distante no futuro, a campanha nunca foi executada e **success=true** foi retornado. Agora, se a data **runAt** for posterior a 2 anos, um erro será retornado [1042](/help/rest-api/error-codes.md).

[Sincronizar clientes em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)

Se você especificou `action=createDuplicate` e um parâmetro `externalCompanyId` (para associar o novo Cliente Potencial a uma Empresa existente), o Cliente Potencial foi associado a uma Empresa vazia (em vez da Empresa especificada).

#### Diversos

* O seguinte Valor de alteração de dados foi removido de todas as respostas do ponto de extremidade: mktoClientReqId. Isso era somente para uso interno.
* Adição da funcionalidade de pesquisa de erro. Insira um código de erro da API REST na caixa de pesquisa e selecione na lista de preenchimento automático abaixo para ir para a descrição do erro.
* Adição da página [Referência de Ponto de Extremidade](/help/rest-api/endpoint-reference.md). Esta é uma lista classificável de todos os endpoints da REST API em um local. Você também pode usar essa página para gerar o conjunto mínimo de permissões necessárias para seu aplicativo. Isso é útil para criar um Serviço personalizado.

Publicado em _2018-10-12_ por _David_

## Atualizações do último trimestre de 2018

A versão lançada no último trimestre de 2019 é principalmente uma versão de manutenção composta por pequenos aprimoramentos e resoluções de defeitos. Consulte a lista completa de atualizações abaixo.

### Aprimoramentos

* Adicionado suporte a [Campos de CC de email](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/email-cc) para [API de ativos](/help/rest-api/assets.md). As configurações de Campo CC são propagadas conforme esperado durante as operações de aprovação/clonagem (aprovação de rascunho de Email ou Modelo de email, clone de Email ou Programa). Todos os pontos de extremidade relacionados ao email agora retornam valores de Campos CC na propriedade **ccFields**. Role para baixo na resposta abaixo para ver um exemplo. Esta alteração afeta os seguintes pontos de extremidade: [Obter Email por ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByIdUsingGET), [Obter Email por Nome](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByNameUsingGET), [Obter Emails](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailUsingGET), [Aprovar Rascunho de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Aprovar Rascunho de Modelo de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST_1), [Clonar Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clonar Programa.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

```json
{
    "success": true,
    "errors": [],
    "requestId": "cc96#166e836348b",
    "warnings": [],
    "result": [
        {
            "id": 2061,
            "name": "Test Email",
            "description": "This is a test",
            "createdAt": "2018-11-06T05:27:10Z+0000",
            "updatedAt": "2018-11-06T08:33:16Z+0000",
            "url": "<https://app-sjqe.marketo.com/#EM2061A1LA1>",
            "subject": {
                "type": "Text",
                "value": "This is a test with CC Fields"
            },
            "fromName": {
                "type": "Text",
                "value": "Tommy Tester"
            },
            "fromEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "replyEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "folder": {
                "type": "Program",
                "value": 7494,
                "folderName": "Initiative"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "approved",
            "template": 1218,
            "workspace": "Default",
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
                {
                    "attributeId": "855",
                    "objectName": "lead",
                    "displayName": "emailcc",
                    "apiName": "emailcc"
                },
                {
                    "attributeId": "857",
                    "objectName": "lead",
                    "displayName": "leadDetails",
                    "apiName": "leadDetails"
                },
                {
                    "attributeId": "859",
                    "objectName": "company",
                    "displayName": "headquarters",
                    "apiName": "headquarters"
                }
            ]
        }
    ]
}
```

### Resoluções de Defeitos

* Ajustou o suporte a [Vários domínios de marca](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) para a [API de ativos](/help/rest-api/assets.md). Anteriormente, as configurações de Vários domínios de marca não eram propagadas ao aprovar um rascunho de Email, clonar um Email ou clonar um Programa. Isso foi corrigido. Esta alteração afeta os seguintes pontos de extremidade: [Aprovar Rascunho de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Clonar Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clonar Programa.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)
* Adição da definição de configuração [apiOnly](/help/javascript-api/configuration.md). Por padrão, as páginas da Web que contêm a tag do Munchkin acionam um evento &quot;Visitas à página da Web&quot; quando a página da Web é carregada no navegador. Em alguns casos, isso é indesejável. Por exemplo, aplicativos web de página única que precisam de controle total sobre quando esse evento é acionado. Para dar suporte a este caso de uso, adicionamos uma nova configuração **apiOnly**. Quando definida como true, a tag do Munchkin não gera uma atividade &quot;Visitas à página da Web&quot; durante o carregamento da página.
* Adição da definição de configuração [domainSelectorV2](/help/javascript-api/configuration.md). Por padrão, a marca Munchkin não manipula corretamente páginas da Web hospedadas em sites com [domínios de nível superior com código de país](https://en.wikipedia.org/wiki/Country_code_top-level_domain) de duas letras (exemplos: .io, .co, .ly). Isso faz com que o atributo de domínio do cookie do Munchkin seja definido incorretamente. Para obter uma melhor experiência imediata, adicionamos uma nova configuração do **domainSelectorV2**. Quando definido como true, um algoritmo aprimorado é usado para definir automaticamente o atributo de domínio do cookie do Munchkin.
* Ajustou o domínio do cookie [Opt-Out](/help/javascript-api/lead-tracking.md). Em certos casos, o atributo de domínio do cookie Munchkin Opt-Out (mkto_opt_out) era definido incorretamente. O cookie do Munchkin Opt-Out agora usa a mesma lógica do cookie do Munchkin (_mkto_trk) para determinar o atributo de cookie do domínio, incluindo o cumprimento da definição de configuração **domainLevel**.
* Os desenvolvedores de aplicativos do Android agora podem usar diretamente o [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) da Google com este SDK. Detalhes podem ser encontrados [aqui](/help/mobile/installation.md).

Publicado em _2018-12-07_ por _David_

## Atualizações do primeiro trimestre de 2019

A versão do segundo trimestre de 2019 é principalmente uma versão de manutenção composta por pequenas melhorias e resoluções de defeitos. Consulte a lista completa de atualizações abaixo.

Publicado em _2019-03-19_ por _David_

## Atualizações de junho de 2019

A versão de junho de 2019 é principalmente uma versão de manutenção composta por pequenas melhorias e resoluções de defeitos. Consulte a lista completa de atualizações abaixo.

### API REST

1. Adição de uma soma de verificação para endpoints de status de Exportação em massa. Você pode comparar a soma de verificação com um hash do arquivo recuperado para verificar a integridade do arquivo recuperado. A soma de verificação é um hash SHA-256 do arquivo exportado e é armazenada no atributo fileCheckSum quando um trabalho de exportação é concluído.

Os pontos de extremidade a seguir retornam a soma de verificação: [Obter Status do Trabalho de Exportação de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Obter Trabalhos de Exportação de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET), [Obter Status do Trabalho de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Obter Trabalhos de Atividade de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportActivitiesUsingGET)

#### Resoluções de Defeitos

1. Correção de um problema em [Importação de Objeto Personalizado em Massa](/help/rest-api/bulk-custom-object-import.md) ao importar números decimais para campos inteiros. Antes da correção, o número decimal era convertido em um inteiro atribuindo a parte inteira do número e descartando a parte fracional (por exemplo, 5,432 era convertido em 5). Agora, um erro &quot;Tipo de dados inválido no campo Source ID&quot; é gerado para cada linha que contém uma incompatibilidade de dados.
1. Correção de um problema em que um programa de email criado usando o ponto de extremidade [Clonar Programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) não respeitava as configurações de Limites de Comunicação em determinados casos.
1. Corrigido o problema com o ponto de extremidade [Aprovar rascunho da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST), no qual retornaria 611. &quot;Erro do sistema&quot; quando a landing page continha o formulário de cancelamento de inscrição de email.
1. Corrigido o problema com o ponto de extremidade [Aprovar rascunho da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST), no qual retornaria 611. &quot;Erro do sistema&quot; quando a página de aterrissagem foi clonada usando o ponto de extremidade [Clonar página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneLandingPageUsingPOST).

#### Descontinuações

1. Como parte da [Descontinuação do Editor de email 1.0](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666), o Assets de email 1.0 se tornará somente leitura no final de 2019. Todos os emails do Assets 1.0 devem ser convertidos para 2.0 conforme descrito em Descartar rascunho de email, bed [aqui](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666). Para ajudar os desenvolvedores a se prepararem para esse evento, adicionamos avisos a todos os endpoints relacionados ao email que tentam modificar a versão 1.0 do Email Assets. Este é um exemplo de resposta que contém o aviso:

`{` `"success": true,` `"errors": [],` `"requestId": "15c57#16b338d6e75",` `"warnings": [` `"This is a v1 email asset. API support for modifying v1 emails is being dropped, and this operation will not work on v1 emails in the future. To avoid service interruptions, upgrade this and related assets by editing them in the User Interface."` `],` `"result": [` `"{\"service\":\"sendTestEmail\",\"result\":true}"` `]` `}`

Os seguintes pontos de extremidade relacionados ao Email retornam o aviso:

* Atualizar conteúdo completo do email
* Atualizar conteúdo de email
* Enviar e-mail de exemplo
* Cancelar aprovação de email
* Clonar programa
* Clonar e-mail
* Aprovar rascunho de email
* Seção Atualizar Conteúdo Dinâmico de Email
* Atualizar metadados de email
* Aprovar programa

Script de email (Apache Velocity)

#### Descontinuações

1. Um subconjunto da funcionalidade Velocity Script foi desativado para fins de segurança. Isso inclui os seguintes métodos e variáveis: getClass(), $class, $context, $text. Mais informações podem ser encontradas [aqui](https://nation.marketo.com:443/t5/knowledgebase/unsupported-velocity-tools-disabled-in-june-2019-release/ta-p/251177).

Publicado em _2019-06-14_ por _David_

## Atualizações de agosto de 2019

Em agosto de 2019, lançaremos novas APIs REST, aprimoraremos as APIs existentes e resolveremos defeitos. Consulte a lista completa de atualizações abaixo.

### Aprimoramentos

1. Recursos aprimorados do ciclo de vida do Smart Campaign. Adição de novos endpoints para permitir que você execute as seguintes operações nas Campanhas inteligentes: obter por nome, criar, atualizar, clonar e excluir. Informações completas podem ser encontradas [aqui](/help/rest-api/smart-campaigns.md).
1. Endpoints de lista inteligente aprimorados para melhorar os recursos de consulta.
   1. O ponto de extremidade [Obter Smart List por Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByIdUsingGET) agora retorna descrições de regra de Smart List (acionadores e filtros) quando você passa o parâmetro booleano **includeRules**.
   1. O ponto de extremidade [Obter Smart Lists](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListsUsingGET) agora permite que você filtre os resultados por intervalo de datas ao passar os parâmetros de data e hora **olderUpdatedAt** e **latestUpdatedAt**. Além disso, esse endpoint agora retorna Smart Lists que são membros de campanhas e programas de email.
1. Adição de endpoints para extração de definições de Smart List.
   1. O ponto de extremidade Get [Smart List by Smart Campaign Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListBySmartCampaignIdUsingGET) retorna o registro da lista inteligente para uma determinada ID de campanha inteligente.
   1. O ponto de extremidade Get [Smart List by Program Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByProgramIdUsingGET) retorna o registro da lista inteligente para uma determinada ID de programa.
1. Aprimoramento do ponto de extremidade [Atualizar Conteúdo de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/updateEmailContentUsingPOST) para permitir atualizações nos campos de cabeçalho do email para emails que foram desfeitos de seu modelo (assunto, nome, email, resposta). A quebra do modelo está descrita [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html).

### Resoluções de Defeitos

1. Correção de um problema em que chamar [Excluir página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#operation/deleteLandingPageByIdUsingPOST) em uma página de aterrissagem aprovada excluiria a página de aterrissagem. Agora, ela retorna corretamente o erro &quot;709, A página de aterrissagem aprovada não pode ser excluída&quot;. [LM-127271]
1. Correção de um problema com o ponto de extremidade [Enviar Email de Exemplo](https://developer.adobe.com/marketo-apis/api/asset/#operation/sendSampleEmailUsingPOST) para o qual retornaria 611. &quot;Erro de sistema&quot; quando o email foi interrompido em seu modelo. [LM-127288]
1. Correção de um problema com o ponto de extremidade [Excluir programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) no qual, em alguns casos, um programa em uso era excluído em vez de emitir um programa &quot;709, Não é possível excluir. Os ativos estão em uso em outro lugar ou não podem ser excluídos&quot; erro. [LM-125431]

### Descontinuações

1. O suporte à API do Editor de email 1.0 está programado para ser descontinuado em janeiro de 2020. Lembre-se de converter seus ativos para 2.0 antes disso. As tentativas de gravar ou clonar ativos do Email 1.0 após janeiro resultarão em erros, em vez de avisos. Saiba mais sobre as APIs de email [aqui](https://nation.marketo.com:443/t5/knowledgebase/email-2-0-and-email-api-faq-s/ta-p/251423).
1. Para nos alinharmos ao padrão de classe mundial da Adobe para segurança, descontinuaremos o suporte ao TLS 1.0 e 1.1 a partir de 13 de dezembro de 2019. Os sistemas integrados com o Marketo que não estiverem em conformidade com o protocolo 1.2 poderão perder acesso aos serviços da Marketo Engage. Para manter o acesso ao Marketo Engage, verifique se todos os sistemas clientes são compatíveis com TLS 1.2 antes de 13 de dezembro de 2019. Você encontra informações mais detalhadas [aqui](https://nation.marketo.com:443/t5/knowledgebase/tls-1-0-1-1-deprecation-faq/ta-p/249085).

1. Todo o conteúdo relacionado ao Smart Campaign agora reside no item de menu [Campanhas inteligentes](/help/rest-api/smart-campaigns.md) (abaixo de REST API > Assets).

Publicado em _2019-08-16_ por _David_

## Atualizações de janeiro de 2020

Em janeiro de 2020, lançaremos novas APIs REST, aprimoraremos as APIs existentes e resolveremos defeitos. Consulte a lista completa de atualizações abaixo.

* Adição da capacidade de criar programaticamente definições de esquema de Objeto personalizado. Isso permite definir um objeto personalizado uma vez e provisioná-lo para quantas instâncias forem necessárias. Isso permite que os usuários aproveitem efetivamente a sandbox e os modelos de centro de excelência. Também permite que os ISVs simplifiquem o processo de integração do cliente. Você precisa de um tipo de assinatura apropriado para acessar a API de metadados de objeto personalizado.
* Adicionada a capacidade de importar e exportar membros do programa em massa. Esse novo conjunto de endpoints segue o padrão da API REST do Marketo existente para criar trabalhos assíncronos de processamento em massa. Os registros de Membros do programa podem conter campos personalizados de Membros do programa e/ou campos de clientes potenciais.
* Adição do endpoint Obter campos de membro do programa de formulário disponíveis para oferecer suporte ao uso de Campos personalizados de membro do programa como Campos de formulário. Isso retorna uma lista de todos os campos personalizados de membros do programa que podem ser usados em um Formulário Marketo.
* Adicionado o [Obter Modelo de Email Usado por um ponto de extremidade ](/help/rest-api/email-templates.md) que retorna uma lista de ativos de email que dependem de um determinado modelo de email. Isso permite compreender rapidamente o impacto de uma possível alteração de modelo de email e lidar mais facilmente com essas dependências.

Publicado em _2020-01-17_ por _David_

## Como recuperar todos os objetos personalizados

Geralmente nos perguntam como usar a API do Marketo para obter uma lista de todos os [objetos personalizados](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) (COs). Consultar COs requer mais do que seu nome: alguns _a priori_ conhecimentos sobre cada CO também são necessários. Os métodos para obter esse conhecimento podem não ser óbvios, pois a API não fornece um método para consultá-lo diretamente. Assim como muitos objetivos no Marketo Engage, as Smart Lists fornecem uma resposta para os COs vinculados a Pessoas (clientes potenciais). As Smart Lists funcionam de forma diferente com as Empresas e você obterá uma lista de todas as Pessoas cujas Empresas estão vinculadas ao tipo de objeto do filtro para que seja necessário desduplicar empresas dependendo de suas metas. Sempre que um novo Objeto personalizado for aprovado, um filtro associado será criado. Ele será nomeado no formato &quot;**Has CO NAME**&quot;. No exemplo abaixo, o nome do objeto personalizado é &quot;**Assinatura de Rastreamento de Conferência&quot;** e o nome do filtro é &quot;**Possui Assinatura de Rastreamento de Conferência**&quot;. Depois de criar a Smart List, você pode recuperar as informações necessárias para consultar COs associados usando o [endpoint de objetos personalizados](/help/rest-api/custom-objects.md). Exporte a lista para garantir que o campo vinculado seja incluído (ID ou endereço de email). Você pode exportar usando a [API de Extração de Cliente Potencial em Massa](/help/rest-api/bulk-lead-extract.md) pelo filtro **smartListName** ou **smartListId** ou [exportar da interface](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/managing-people-in-smart-lists/export-people-to-excel-from-a-list-or-smart-list). Você usará cada valor de campo vinculado para consultar objetos personalizados associados individualmente na próxima etapa. O nome do objeto personalizado é **&quot;Assinatura de Controle de Conferência&quot;** neste exemplo, e seu nome de API é **conferenceTrackSubscription_c**. Você encontra o nome da API na interface do usuário como &quot;**Nome da API**&quot; e por meio da API como &quot;**nome**&quot;.  Admin | Objetos personalizados do Marketo[/legenda]. E aqui está um fragmento retornado pelo ponto de extremidade da [API de Objetos personalizados da lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/listCustomObjectsUsingGET):

```json
{
    "name": "conferenceTrackSubscription_c",
    "displayName": "Conference Track Subscription",
    "description": "Track subscription for conference attendees",
    "createdAt": "2020-01-13T19:50:06Z",
    "updatedAt": "2020-01-13T19:50:06Z",
    "idField": "marketoGUID",
    "dedupeFields": [
        "subscriptionID"
    ],
    "searchableFields": [
        [
            "subscriptionID"
        ],
        [
            "marketoGUID"
        ],
        [
            "leadID"
        ]
    ],
    "relationships": [
        {
            "field": "leadID",
            "type": "child",
            "relatedTo": {
                "name": "Lead",
                "field": "Id"
            }
        }
    ]
}
```

Para recuperar os objetos personalizados associados de um para um (1:1) ou de um para muitos (1:N) com as Pessoas na sua Lista Inteligente, faça uma solicitação como esta:

`GET /rest/v1/customobjects/conferenceTrackSubscription_c.json?filterType=leadID&filterValues=1000302,1000303,1000304,1000306,1000307`

Neste exemplo, este objeto personalizado está vinculado a Pessoas pelo campo **leadID**, portanto, o tipo de filtro é &quot;**leadID**&quot;. O parâmetro de valores do filtro é uma lista separada por vírgulas das IDs obtidas da exportação da Smart List. A solicitação pode incluir quantos valores de filtro você puder ajustar em um único URI de solicitação: até 8K caracteres. As solicitações que excedem esse comprimento retornam um código de erro 414 de nível HTTP. A resposta pode ser retornada em mais de uma parte. Em caso positivo, **moreResult** será **true** e um **nextPageToken** será incluído. Em seguida, será necessário [percorrer](/help/rest-api/paging-tokens.md) os resultados até que **moreResult** seja **false**. Aqui está parte do resultado da solicitação de API acima:

```json
"result": [
    {
        "seq": 0,
        "marketoGUID": "d6b3ed3d-4eb8-4066-a9cd-184c8d385cfe",
        "leadID": "1000302",
        "subscriptionID": "4ad59184-6bf1-4eeb-a583-d82aeee68210",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 1,
        "marketoGUID": "e5e0aba4-f27f-494d-93ed-9cb580989bf3",
        "leadID": "1000303",
        "subscriptionID": "fc5596d5-6fa2-4848-b4a2-89d96e245c59",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 2,
        "marketoGUID": "e65007cd-86b1-4c17-8d55-057c96e1788a",
        "leadID": "1000304",
        "subscriptionID": "7e54b8a0-2170-4d81-a809-4eac349508d0",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 3,
        "marketoGUID": "39d956b2-85e2-4c24-94e7-e9fa5a09d3d0",
        "leadID": "1000306",
        "subscriptionID": "644c8e5b-fc0c-4d4a-80f8-358a65ce0a68",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 4,
        "marketoGUID": "a2151350-50c8-437f-bc71-7a054bb601f0",
        "leadID": "1000307",
        "subscriptionID": "bf14218c-ae6a-42b3-a14e-f7182903cbcd",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    }
```

Agora você tem os valores para cada objeto personalizado diretamente associado às Pessoas na sua Smart List e, além de recuperar os valores, você pode usar o **marketoGUID** para [Atualizar](/help/rest-api/custom-objects.md) ou [Excluir](/help/rest-api/custom-objects.md) esses objetos. Para objetos personalizados associados a Pessoas em um relacionamento muitos para muitos (N:N), a técnica acima retorna o objeto de primeiro nível, que é o objeto intermediário que conecta cada Pessoa a vários COs de segundo nível.

Para recuperar esses COs de segundo nível, inicie um novo conjunto de consultas para o tipo de CO de segundo nível filtrando no campo de link e os valores extraídos do objeto intermediário de primeiro nível. Por exemplo, o objeto &quot;**Assinatura de Rastreamento de Conferência&quot;** acima pode ter outro nível de objetos que representam sessões chamadas **&quot;Sessão&quot;**, que provavelmente seriam vinculadas pela **subscriptionID**. A solicitação para recuperar Sessões vinculadas às Assinaturas de Rastreamento de Conferência acima seria semelhante a:

`GET /rest/v1/customobjects/session_c.json?filterType=subscriptionID&filterValues=4ad59184-6bf1-4eeb-a583-d82aeee68210,e5e0aba4-f27f-494d-93ed-9cb580989bf3,e65007cd-86b1-4c17-8d55-057c96e1788a,39d956b2-85e2-4c24-94e7-e9fa5a09d3d0,bf14218c-ae6a-42b3-a14e-f7182903cbcd`

_Nota de Rodapé_ _1) Os tipos de filtro **smartListName**&#x200B;e **smartListId**&#x200B;não estão disponíveis para algumas assinaturas. Se não estiver disponível para sua assinatura, você receberá um erro ao chamar o ponto de extremidade Criar Trabalho de Cliente Potencial de Exportação (**&quot;1035, Tipo de filtro sem suporte para assinatura de destino&quot;**). Os clientes podem contatar o Suporte da Marketo para habilitar essa funcionalidade em suas assinaturas._

Publicado em _2020-01-14_ por _Tony_

## Como Recuperar Todas as Pessoas (Lead)

Recebemos muitas consultas sobre os processos necessários para recuperar cada pessoa (lead) de uma instância do Marketo Engage. Fornecemos muitas respostas úteis, mas nenhuma foi tão completa quanto esta. Identifiquei alguns conceitos-chave necessários para extrair cada lead usando a API de extração em massa do Marketo. Todos os outros detalhes podem ser aprendidos com o código de demonstração que criei para acompanhar isso. Depois de ler esta publicação e explorar o código de demonstração, você terá todas as informações necessárias para recuperar todos os clientes em potencial de uma instância do Marketo Engage.

### Visão geral

A técnica principal usa a [API de Extração de lead em massa](/help/rest-api/bulk-lead-extract.md). Você pode criar um trabalho de exportação de clientes potenciais em massa sem filtro, mas não pode: **a API exige um filtro**. Os filtros disponíveis são **createdAt**, **staticListName**, **staticListId,** **updatedAt**, **smartListName** e **smartListId**. A filtragem por uma Smart List sem filtros também parece atraente. Tente isso e você descobrirá que o sistema é inteligente o suficiente para tratar uma Smart List sem filtro da mesma forma: a API exige um filtro aqui também. Como precisamos de um filtro, o filtro confiável e canônico a ser usado é **createdAt**. Esse tipo de filtro permite intervalos de data e hora de até 31 dias, portanto, precisamos executar vários trabalhos e combinar os resultados. Começamos localizando a data de criação mais antiga possível para um lead na instância de destino. A partir dessa data mais antiga possível, criamos trabalhos que abrangem 31 dias menos um segundo (mais sobre isso posteriormente). Depois de criar cada tarefa, a colocaremos na fila e aguardaremos sua conclusão. Em seguida, baixaremos o arquivo resultante e verificaremos sua integridade usando uma soma de verificação. E, por fim, desduplicar clientes em potencial por ID e, em seguida, gravar clientes em potencial exclusivos em um arquivo CSV de saída.

### Encontrar o cliente em potencial mais antigo

Estou usando um pequeno &quot;truque&quot; para obter a data mais antiga possível para um lead na instância do público-alvo. Não há um terminal de API dedicado para essa tarefa, portanto precisamos de um pouco de criatividade. O que eu faço é consultar todas as pastas com uma **maxDepth = 1** que nos dará uma lista de todas as pastas de nível superior na instância. Em seguida, eu coleto as datas **createdAt**, analiso-as e encontro a data mais antiga. Esse método funciona porque algumas pastas padrão de nível superior são criadas com a instância e nenhum lead pode ser criado antes disso.

### Selecionar campos obrigatórios

Você precisa decidir quais campos precisa extrair. Encontre os campos disponíveis para sua instância de destino usando o [Descrever ponto de extremidade do lead 2](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). A resposta a essa solicitação incluirá uma lista chamada &quot;campos&quot;. Este é um trecho de uma resposta de exemplo:

```json
  "fields": [
      {
          "name": "AccountSource",
          "displayName": "Account Source",
          "dataType": "string",
          "length": 40,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "acquisitionProgramId",
          "displayName": "Acquisition Program",
          "dataType": "reference",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Active_c",
          "displayName": "Active",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "address",
          "displayName": "Address",
          "dataType": "text",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Address_lead",
          "displayName": "Address (L)",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "annualRevenue",
          "displayName": "Annual Revenue",
          "dataType": "currency",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "anonymousIP",
          "displayName": "Anonymous IP",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      }, ...
```

Esse endpoint retorna uma lista exaustiva incluindo campos padrão e personalizados. Quanto mais campos você solicitar, mais tempo levará para concluir o trabalho de exportação e maior será o arquivo resultante. Normalmente, você deve escolher apenas os campos necessários. Nada impede que você solicite todos os campos disponíveis, então é isso que estou demonstrando. O identificador de campo necessário ao criar um trabalho de exportação é o valor **name**. Extrairei os valores de nome para uma lista de todos os nomes de campo. Em seguida, vou usá-lo para solicitar cada campo disponível ao criar cada trabalho de exportação.

### Intervalos de datas do trabalho de exportação: 31 dias cada

Cada trabalho de exportação pode durar até 31 dias. A instância de demonstração que estou usando foi criada em agosto de 2016, portanto, preciso criar pouco mais de 40 trabalhos hoje. Esse é o número de dias desde a primeira data de criação dividido por 31 arredondado para cima. A API permite que dois trabalhos de exportação sejam processados de uma só vez, para que você possa extrair com dois trabalhos em execução paralela. Trabalhos de extração em massa são um recurso compartilhado com todas as outras integrações, então vou ser legal. Deixo a outra tarefa disponível para outras integrações e demonstrarei a execução de tarefas únicas, uma após a outra. As datas usadas para o filtro **createdAt** são formatadas usando a [especificação ISO 8601](https://www.w3.org/TR/NOTE-datetime). Eles estão sempre em GMT (Z+0000), portanto, o fuso horário será simplesmente representado como &quot;Z&quot; ou &quot;+00:00&quot;. 1º de agosto de 2016 é **2016-08-01T00:00:00+00:00** e 31 dias depois é 1º de setembro de 2016, que é **2016-09-01T00:00:00+00:00.** As horas de início e término são inclusivas, portanto, vou subtrair 1 segundo dessa hora de término: **2016-09-01T00:00:00+00:00** torna-se **2016-08-31T23:59:59+00:00**. Subtrair um segundo evita sobrepor os tempos. Como GMT é o padrão, você também pode deixar o **Z** ou **+00:00** desativado.

### Desduplicação

Embora eu tenha tido o trabalho de evitar sobreposições, também implementei a eliminação da duplicação. Eu fiz isso, pois há alguns casos de borda quando os horários mudam ([Horário de verão](https://en.wikipedia.org/wiki/Daylight_saving_time)), resultando em valores ambíguos e, como resultado, a API de Extração em massa do Marketo pode retornar clientes em potencial duplicados inesperados. É raro isso acontecer, mas o precisa ser contabilizado em qualquer integração usando intervalos de filtro de data e hora. Eu tirei um segundo para deixar claro que os tempos são inclusivos. Eu não gostaria que você pensasse que criar um trabalho com o **createdAt** e o **endAt** vezes de **2016-08-01T00:00:00Z** e **2016-09-01T00:00:00Z**, respectivamente, não incluirá clientes em potencial criados em **2016-09-01T00:00:00Z**; será.

### Criar um trabalho

A primeira etapa é criar um trabalho usando o [Ponto de extremidade Criar Trabalho de Cliente Potencial para Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createExportLeadsUsingPOST). Nesta demonstração, a solicitação para criar nosso primeiro trabalho de exportação tem esta aparência:

`POST /bulk/v1/leads/export/create.json`

```json
{ "filter": {"createdAt": {"startAt": "2016-08-01T00:00:00",
                           "endAt": "2016-09-01T00:00:00"}},
"fields":["AccountSource","acquisitionProgramId","Active_c","address","Address_lead","annualRevenue","anonymousIP","BillingAddress","billingCity","billingCountry","BillingGeocodeAccuracy","BillingLatitude","BillingLongitude","billingPostalCode","billingState","billingStreet","blackListed","blackListedCause","city","CleanStatus","CleanStatus_account","company","CompanyDunsNumber","contactCompany","cookies","country","createdAt","customfield","DandbCompanyId","DandbCompanyId_account","dateOfBirth","dddS","department","doNotCall","doNotCallReason","dS","dueDate","DunsNumber","email","EmailBouncedDate","EmailBouncedReason","emailInvalid","emailInvalidCause","emailSuspended","emailSuspendedAt","emailSuspendedCause","externalCompanyId","externalSalesPersonId","facebookDisplayName","facebookId","facebookPhotoURL","facebookProfileURL","facebookReach","facebookReferredEnrollments","facebookReferredVisits","fax","firstName","gender","GeocodeAccuracy","holll","id","industry","inferredCity","inferredCompany","inferredCountry","inferredMetropolitanArea","inferredPhoneAreaCode","inferredPostalCode","inferredStateRegion","interested","interestedIn","isAnonymous","IsEmailBounced","isLead","iSTRUE","Jigsaw","JigsawCompanyId_account","JigsawContactId_lead","Jigsaw_account","Languages_c","lastName","LastReferencedDate","LastReferencedDate_account","lastReferredEnrollment","lastReferredVisit","LastViewedDate","LastViewedDate_account","Latitude","leadPartitionId","leadPerson","leadRevenueCycleModelId","leadRevenueStageId","leadRole","leadScore","leadSource","leadStatus","linkedInDisplayName","linkedInId","linkedInPhotoURL","linkedInProfileURL","linkedInReach","linkedInReferredEnrollments","linkedInReferredVisits","links","Longitude","MailingAddress","MailingGeocodeAccuracy","MailingLatitude","MailingLongitude","mainPhone","marketingSuspended","marketingSuspendedCause","middleName","mktoAcquisitionDate","mktocomment1","mktocomments2","mktoCompanyNotes","mktoDoNotCallCause","mktoIsCustomer","mktoIsPartner","mktoName","mktoPersonNotes","mktosync","mktotest1","mobile","mobilePhone","NaicsCode","NaicsDesc","newcustom","numberOfEmployees","originalReferrer","originalSearchEngine","originalSearchPhrase","originalSourceInfo","originalSourceType","OtherAddress","OtherGeocodeAccuracy","OtherLatitude","OtherLongitude","personPrimaryLeadInterest","personTimeZone","personType","phone","PhotoUrl","PhotoUrl_account","postalCode","priority","ProductInterest_c","rating","referal","registrationSourceInfo","registrationSourceType","relativeScore","relativeUrgency","requiredNumberofCylinder","salutation","sfdcAccountId","sfdcContactId","sfdcId","sfdcLeadId","sfdcLeadOwnerId","sfdcType","ShippingAddress","ShippingGeocodeAccuracy","ShippingLatitude","ShippingLongitude","sicCode","SicDesc","site","state","surveyAnswers","syndicationId","testBooleanField","testscore","title","totalReferredEnrollments","totalReferredVisits","Tradestyle","twitterDisplayName","twitterId","twitterPhotoURL","twitterProfileURL","twitterReach","twitterReferredEnrollments","twitterReferredVisits","uNSUBSCIBE","unsubscribed","unsubscribedReason","updatedAt","urgency","url","website","YearStarted"]}
```

A resposta é semelhante a:

```json
{
  "requestId": "6902#16fb52118bf",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Created",
          "createdAt": "2020-01-17T20:10:43Z"
      }
  ],
  "success": true
}
```

### Enfileirar a tarefa

O trabalho agora está criado, mas apenas sentado lá sem fazer nada. Para executar o trabalho, precisamos chamar o [ponto de extremidade de enfileiramento](https://developer.adobe.com/marketo-apis/api/mapi/#operation/enqueueExportLeadsUsingPOST) usando o valor **exportId** para criar o URI para a solicitação. Ela tem a seguinte aparência:

`POST /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/enqueue.json`

Não há corpo para este POST; estamos simplesmente usando o verbo HTTP POST aqui. Essa solicitação gerará uma resposta como esta:

```json
{
  "requestId": "1836a#16fb5238a48",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Queued",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z"
      }
  ],
  "success": true
}
```

Como mencionei anteriormente, você está limitado ao número de trabalhos que podem ser executados de cada vez. Também há um limite para o número de tarefas em fila de uma vez: 10. Precisamos de mais de 40, para que esse limite nos impeça de criar todos os empregos de uma só vez. Outras integrações também podem executar tarefas, portanto, precisamos levar em conta a possibilidade de que todos os slots estejam cheios. Tentar enfileirar um novo trabalho quando já houver 10 trabalhos em fila gerará um erro [1029](/help/rest-api/error-codes.md). Quando você receber um **1029**, use um retrocesso exponencial até que o trabalho possa ser enfileirado. Aguardo 1 minuto e dobro esse valor sempre que recebo um código de erro **1029** de até 4 minutos entre solicitações, mas nunca mais que isso. Esta técnica é conhecida como [Retrocesso exponencial binário truncado](https://devopedia.org/binary-exponential-backoff) e é a prática recomendada para erros recuperáveis e verificações de status.

### Aguardar a Conclusão do Trabalho

Cada trabalho leva algum tempo para ser executado, portanto, chamaremos o [ponto de extremidade de status](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET) para monitorar seu progresso. Novamente, incluiremos a **exportId** no URI da solicitação desta forma:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/status.json`

Antes da conclusão da tarefa, você receberá uma resposta com esta aparência:

```json
{
    "requestId": "153cb#16fb525435d",
    "result": [
        {
            "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-17T20:10:43Z",
            "queuedAt": "2020-01-17T20:13:23Z",
            "startedAt": "2020-01-17T20:13:49Z"
        }
    ],
    "success": true
}
```

Eu executo o mesmo retrocesso exponencial (1 minuto até 4 minutos) enquanto o status do trabalho é &quot;Em fila&quot;. O status não é em tempo real; é atualizado uma vez por minuto e há muito pouco benefício em pesquisar mais rapidamente. Redefino a retirada quando o status do trabalho for alterado para &quot;Processando&quot;. Estamos aguardando o status &quot;Concluído&quot;, que se parece com o seguinte:

```json
{
  "requestId": "10ad9#16fb5268f9b",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Completed",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z",
          "startedAt": "2020-01-17T20:13:49Z",
          "finishedAt": "2020-01-17T20:15:28Z",
          "numberOfRecords": 59,
          "fileSize": 74436,
          "fileChecksum": "sha256:de553362e7ffad6556ae9ea749655c35010c7f0e944fc5a85782183130dca18d"
      }
  ],
  "success": true
}
```

O valor **numberOfRecords** é zero quando a solicitação não retorna nenhum cliente potencial. Eu verifico esse valor e pulo os próximos passos quando isso faz sentido. Quando os clientes em potencial forem retornados, eu extrairá o valor **fileChecksum**. Nós o usamos para verificar a integridade do arquivo quando ele é baixado.

### Obtenha seus clientes em potencial

Se o **numberOfRecords** for maior que zero, baixe o arquivo exportado usando o [Obter Arquivo de Cliente Potencial de Exportação](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET) com uma solicitação como esta:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/file.json`

Verifique se o arquivo foi transferido sem erros: calcule a soma de verificação do arquivo e compare com o **fileChecksum** que salvamos anteriormente. Calcule a soma de verificação usando [SHA-2](https://en.wikipedia.org/wiki/SHA-2) e especificamente a função de hash SHA256. Se as somas de verificação calculadas não corresponderem, houve um erro na transferência de arquivo e você pode tentar a transferência novamente ou abortar e recuperar manualmente.

### Agregar os dados

Depois de percorrer cada intervalo de 31 dias, da primeira pista até hoje, você terá um conjunto completo. Você também terá um arquivo para cada intervalo. A maneira mais simples de criar um único arquivo agregado com cada lead seria concatenar esses arquivos após remover a linha de cabeçalho de todos, exceto o primeiro arquivo. Se você fizer isso, não se esqueça de esperar e planejar uma possível duplicação mais tarde no pipeline de processamento de dados. Na minha demonstração, procuro os arquivos à medida que os baixo. Antes de adicionar cada linha de dados ao arquivo de saída, eu desduplico verificando se a ID do lead da linha já foi gravada.

Eu desenvolvi alguns códigos de demonstração hospedados [aqui](https://github.com/Marketo/REST-Sample-Code/tree/master/python/LeadDatabase/Leads) que, espero, preenchem os detalhes desse processo e podem servir como modelo para o seu próprio desenvolvimento. O código de demonstração deve ser uma ferramenta de aprendizagem para que haja melhorias na robustez que seriam necessárias para um sistema de produção. **O código é fornecido NO ESTADO EM QUE SE ENCONTRA** sob licença do MIT, mas provavelmente é bom o suficiente para uso único com supervisão humana. Nada está te parando agora! Ao seguir esse processo, você extrairá todos os clientes em potencial usando a API de extração em massa do Marketo e, possivelmente, todos os campos da instância do Marketo Engage de destino. Para ampliar ainda mais seus dados, obtenha as atividades de cada lead usando as técnicas.

Publicado em _2020-03-05_ por _Tony_

## Atualizações de fevereiro de 2020

Em fevereiro de 2020, lançaremos novas APIs REST. Consulte a lista completa de atualizações abaixo.

### Anúncios

* Após setembro de 2020, os Pontos de Extremidade da [API de Ativo](/help/rest-api/assets.md) não aceitarão mais o parâmetro de consulta **_method**. Isso foi usado para transmitir parâmetros de consulta em um corpo POST para ignorar limitações de comprimento de URI. Para acomodar solicitações que exigem esse parâmetro, o limite do URI para APIs de ativos será aumentado de 6 KiB para 65 KiB.
* Com relação à nossa posição sobre a ITP, consulte esta publicação da Comunidade do Marketo: [Atualizações de cookies do navegador: como o Marketo/Munchkin é afetado](https://nation.marketo.com:443/t5/knowledgebase/browser-cookie-updates-how-marketo-munchkin-is-affected/ta-p/251524)
* Uma alteração foi feita na atividade &quot;Alterar status na progressão&quot;. O atributo &quot;ID de membro do programa&quot; foi adicionado ao para apoiar o recurso futuro &quot;Campos personalizados de membro do programa&quot;.

Publicado em _2020-02-26_ por _David_

## Substituição do método de cliente em potencial do Munchkin Associate

Com a próxima versão do Munchkin JavaScript Client, versão 159, iniciaremos a desativação do [Método Associado de Cliente Potencial](/help/javascript-api/api-reference.md) do Munchkin. A partir desta versão, quando o método for chamado, um aviso será emitido no console do navegador indicando que o método será removido em uma versão futura. Depois que o método for removido, as tentativas de usá-lo resultarão em falha. Os clientes do Marketo que identificamos como tendo usado esse método recentemente serão notificados individualmente sobre seu uso. Para obter mais informações sobre a versão 159 do Munchkin, [consulte esta Publicação da Nação de Marketing](https://nation.marketo.com:443/t5/product-documents/munchkin-javascript-version-159-amp-associate-lead-deprecation/ta-p/299687).

### Perguntas frequentes

Como saber se estou afetado?

A Adobe notificará os clientes onde observamos o uso desse método em suas assinaturas e o fará várias vezes durante o período de desativação.

Quando o método será removido?

Esse método será removido no Munchkin JS v161, programado para Disponibilidade Geral junto com a versão de outubro de 2021 do Marketo.

Por que esse método está sendo removido?

Formas mais eficientes de atender aos mesmos casos de uso foram implementadas e lançadas desde a introdução deste método. Para melhorar o desempenho e a integridade de nossos serviços, às vezes é necessário remover recursos que não funcionam de acordo com padrões aceitáveis.

O que devo usar em vez deste método?

O Munchkin Associate Lead tem dois casos de uso principais, envio de dados pessoais e associação de um cookie de rastreamento Web do navegador ao registro de pessoa correspondente no Marketo. Desde que o Munchkin Associate Lead foi introduzido, maneiras mais robustas de executar o envio de dados e a associação de cookies foram implementadas.

#### Envio pelo lado do servidor

Se você não precisar enviar pelo lado do navegador, a API REST oferece muitos métodos para [envio de dados da pessoa](/help/rest-api/leads.md) e um [método criado especificamente para associar cookies a registros de pessoa](/help/rest-api/leads.md).

Quando a Versão 159 do Munchkin está sendo lançada?

A versão 159 está em disponibilidade geral desde a versão de outubro de 2020 do Marketo.

Publicado em _2020-05-06_ por _Kenny_

## Criar um envio de formulário do Marketo em segundo plano

Quando sua organização tem muitas plataformas diferentes para hospedar conteúdo da Web e dados de clientes, é bastante comum precisar de envios paralelos de um formulário para que os dados resultantes possam ser coletados em plataformas separadas. Há várias estratégias para fazer isso, mas a melhor delas geralmente é a mais simples: usar a API do Forms 2 para enviar um formulário oculto do Marketo. Isso funcionará com qualquer novo formulário do Marketo, mas idealmente você deve criar um formulário vazio para isso, que não tem campos. Isso garantirá que o formulário não carregue mais dados do que o necessário, já que não precisamos renderizar nada. Agora basta pegar o [código incorporado](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) do seu formulário e adicioná-lo ao corpo da página desejada, fazendo uma pequena modificação. Seu código integrado inclui um elemento de formulário como este:

`<form id="mktoForm_1068"></form>`

Você adicionará &#39;style=&quot;display:none&quot;&#39; ao elemento para que ele não fique visível, desta forma:

`<form id="mktoForm_1068" style="display:none"></form>`

Uma vez que o formulário é incorporado e oculto, o código para enviar o formulário é realmente muito simples:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"<test@example.com>",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

O Forms enviado dessa maneira se comportará exatamente como se o lead tivesse preenchido e enviado um formulário visível. O acionamento do envio varia entre as implementações, pois cada uma tem uma ação diferente para solicitar, mas você pode fazer com que ele ocorra basicamente em qualquer ação. A parte importante é definir os campos e valores corretamente. Certifique-se de usar o nome da API do SOAP de seus campos, que podem ser encontrados em Exportar nomes de campo, para garantir o envio correto dos valores.

### Migração do Munchkin Associate Lead

O envio de formulário em segundo plano é um dos métodos de substituição recomendados para o líder associado da Munchkin. A página de exemplo do HTML abaixo demonstra a migração em alto nível, reutilizando os mesmos valores que são enviados para Associar lead.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName['head'](0).appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //pass the same set of values to associateLead
            //hashString: secret + email
            Munchkin.munchkinFunction('associateLead', values, "CHANGE ME");

            //submit the form
            form.submit();


        })
    </script>
</body>

</html>
```

Publicado em _2020-05-26_ por _Kenny_

## Atualizações de julho de 2020

Em julho de 2020, lançaremos novas APIs REST, aprimoraremos as APIs existentes e resolveremos defeitos. Consulte a lista completa de atualizações abaixo.

* Foram adicionados dois endpoints que permitem consultar e excluir usuários que ainda não aceitaram o convite (ou seja, usuários &quot;pendentes&quot;). O ponto de extremidade [Obter Usuário Convidado por ID](/help/rest-api/user-management.md) permite que você consulte um usuário pendente. O ponto de extremidade [Excluir Usuário Convidado](/help/rest-api/user-management.md) permite excluir um usuário pendente.
* O ponto de extremidade [Convidar Usuário](/help/rest-api/user-management.md) foi atualizado para aceitar cadeias de caracteres datetime compatíveis com ISO 8601 para o parâmetro **expiresAt**.
* Os pontos de extremidade [Obter Usuário por Id](/help/rest-api/user-management.md) e [Atualizar Atributo de Usuário](/help/rest-api/user-management.md) foram atualizados para retornar o último logon de usuário no atributo **lastLoginAt**.
* Correção de um problema em que o ponto de extremidade [Criar Lista Estática](https://developer.adobe.com/marketo-apis/api/asset/#operation/createStaticListUsingPOST) retornava o erro &quot;611, Erro do sistema&quot; quando você tentava criar uma lista estática que já existia. Alterado para retornar o erro &quot;709, já existe uma Lista estática com o mesmo nome&quot;. [LM-135934]
* Correção de um problema em que o ponto de extremidade [Criar email](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailUsingPOST) retornava o erro &quot;611, Erro do sistema&quot; quando você tentava criar um email que já existia. Alterado para retornar o erro &quot;709, Já existe um email com o mesmo nome&quot;. [LM-138648]
* Correção de um problema em que os pontos de extremidade de consulta da página de aterrissagem retornavam valores incorretos de **createdAt**. Os pontos de extremidade retornavam a hora em que uma página de aterrissagem foi aprovada pela última vez. Agora eles retornam à hora em que uma landing page foi criada. [LM-138648]
* Correção de um problema em que o ponto de extremidade [Mesclar clientes em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) retornava o erro &quot;611, Erro do sistema&quot; para uma operação de mesclagem inválida. Ocorreu quando a mesclagem resultou em um cliente potencial duplicado e o **mergeinCRM** foi definido como verdadeiro. Alterado para retornar o erro &quot;712, Você está criando um registro duplicado. Recomendamos que você use um registro existente&quot;. [LM-137463]

Publicado em _2020-08-01_ por _David_

## Publicação de convidado - Aprofundamento: objeto personalizado vs. atividades personalizadas vs. campo personalizado

**Esta é uma publicação de convidado de Amit Jain, Marketo Champion 2020, MarTech IT Specialist** Como a plataforma de automação de marketing de nível empresarial, a Marketo gerencia suas informações de usuários/clientes/clientes potenciais adquiridas de várias fontes e as mantém no Marketo para melhorar a personalização, a segmentação e os relatórios. Essas fontes podem variar desde formulários de site até a lista de criação, os dados de CRM e os dados de comércio eletrônico. A Marketo oferece objetos padrão, ou seja, cliente potencial, empresa, oportunidade e atividade etc. Esses objetos padrão às vezes não são suficientes para atender às necessidades de marketing emergentes de conjuntos de dados estendidos que estarão disponíveis no Marketo.

Por exemplo, itens adicionados ao carrinho, alunos se inscreveram em diferentes cursos, produtos de propriedade de uma pessoa específica e consentimentos para diferentes assinaturas etc. Essas informações podem ser essenciais para que os profissionais de marketing mantenham o cliente/cliente potencial envolvido, fornecendo conteúdo mais personalizado e experiência do usuário unificada em todas as plataformas. Para acomodar essas necessidades comerciais, o Marketo fornece maneiras personalizadas de armazenar esse tipo de dados em Campos personalizados, Atividades personalizadas e Objetos personalizados. Eu observei pessoas com problemas para entender quando usar quais das opções. Nesta publicação do blog, **vamos entender detalhadamente o que são essas três coisas e quando usar qual delas com alguns exemplos.**

**Campos Personalizados**

O objeto &quot;Principal&quot; no Marketo é o objeto principal e tudo o mais está conectado a este, direta ou indiretamente. O Marketo permite criar campos personalizados no objeto &quot;Lead&quot; e no objeto &quot;Empresa&quot;. Recentemente, ele anunciou o suporte a campos personalizados &quot;Membro do programa&quot;. Esses campos personalizados atendem à sua necessidade de armazenar determinados tipos de dados. Por exemplo, talvez seja necessário armazenar a Função de trabalho ou consentimentos diferentes do usuário. Há dois tipos de campos personalizados no Marketo:

1. **Sincronizado a partir de campos do CRM:** Como parte da sincronização automática do Marketo ↔︎ SFDC, o Marketo sincroniza todos os campos personalizados visíveis para o usuário do Marketo no SFDC em Lead, Contato, Conta e Objeto de Oportunidade.
1. **Somente campos do Marketo:** é possível criar um campo diretamente no Marketo. Os dados desses campos não serão sincronizados com o SFDC.

**Dicas rápidas**

* Você tem um campo somente Marketo e agora deseja sincronizar os dados no SFDC? Confira [este blog](https://themarketingautomationblog.com/2019/12/06/field-management-merging-remapping-hiding-marketo-fields/) para saber como você pode fazer isso.
* Saiba mais sobre os campos personalizados [aqui](https://themarketingautomationblog.com/2019/11/15/knowing-your-marketo-fields/).
* Atualmente, as APIs do Marketo não são compatíveis com a atualização/criação de campos personalizados.

**Objetos Personalizados**

Além dos objetos padrão, o Marketo permite criar seus próprios objetos personalizados. _Você pode criar objetos personalizados e relacioná-los a um objeto de Cliente Potencial ou de Empresa ou a outro objeto personalizado._ Um objeto personalizado é um conjunto de registros personalizados que complementam os registros padrão de Cliente Potencial e Empresa. Objetos personalizados permitem armazenar dados adicionais de maneira escalável e vincular esses dados a um registro de Cliente Potencial ou Empresa. Você pode criar um objeto personalizado com qualquer combinação de campos padrão (Campos de Link) e personalizados, preencher esses campos para criar _registros_ de objeto personalizado e vincular esses registros a um registro de Cliente Potencial ou de Empresa. Registros vinculados, poderosos e flexíveis enriquecem seus Segmentos, Smart Lists e Campanhas permitindo que você crie esses ativos com informações não encontradas nos campos de clientes potenciais e de Empresa. Um objeto personalizado pode ter um dos seguintes tipos de relacionamentos:

* **Relação um para um**: onde cada objeto personalizado tem um único registro de objeto de Cliente Potencial/Empresa.
* **Relação um para muitos**: em que cada objeto personalizado contém vários registros de objeto personalizado relacionados a um Cliente Potencial/Empresa.
* **Relação Muitos para Muitos:** onde vários registros de objeto personalizado podem ser vinculados a vários objetos de cliente potencial/empresa. Por exemplo, vários alunos estão inscritos em vários cursos de um catálogo de cursos.

**Dicas rápidas**

* Saiba mais para configurar objetos personalizados [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR).
* Você pode usar o objeto personalizado do Marketo como um objeto intermediário, bem como o objeto personalizado do objeto personalizado.

### Por que objetos personalizados?

Os objetos personalizados permitem compilar e usar dados exclusivos que são relevantes para uma empresa ou cliente potencial, mas que não são necessariamente informações estáticas sobre a empresa ou cliente potencial em si. Embora os campos de cliente potencial se relacionem às informações de cliente potencial de uma pessoa (_Endereço de email_, _CEP_ e assim por diante), às informações comerciais (_Cargo_, _Setor_ e assim por diante) ou às informações orientadas pelo sistema (_Marketo ID_, SFDC ID ou Data de Criação etc.), os campos de objeto personalizado são totalmente personalizáveis. Por exemplo, use objetos personalizados para armazenar informações como as seguintes:

* Histórico de compras de um usuário.
* Informações do carrinho.
* Se um cliente usou ou não um de seus códigos de desconto promocional por tempo limitado.
* Informações complementares obtidas de resultados de pesquisas, entrevistas e participação no evento.
* Informações de avaliação de um usuário, incluindo URL da instância de avaliação, Data inicial, Data final, Número de usuários, etc.

### Limitações de objetos personalizados do Marketo

1. Por padrão, o Marketo permite criar 10 objetos personalizados. Você pode aumentar o limite com uma taxa de assinatura adicional.
1. O número padrão de campos por objeto é 50, mas você pode solicitar suporte da Marketo para campos adicionais, se necessário. Obrigado [Michael Florin](https://www.linkedin.com/in/michaelflorin/) por informações adicionais aqui.
1. Há um limite no número de registros que você pode ter em todos os objetos personalizados. Depende da sua assinatura do Marketo. Esse limite pode ser aumentado com uma taxa de assinatura adicional.
1. Se um objeto personalizado foi criado usando a API, o Marketo não permite editar o esquema CO na interface do usuário do Marketo.

**Dicas rápidas**

* Operação CRUD (Create, Read, Update, and Delete, Criar, ler, atualizar e excluir) de suporte da API do Marketo em objetos personalizados.
* Você pode usar esses dados de objetos personalizados para personalização de email usando o Velocity Script.
* Você pode encontrar todos os pontos de extremidade relacionados ao objeto Personalizado [aqui](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportProgramMembersStatusUsingGET).

### Terminologia do objeto personalizado

**Objeto Personalizado**: um contêiner que contém um agrupamento de todos os registros de objeto personalizado. Formalmente conhecido como um conjunto de cartões de dados ou tabela personalizada. **Registro de Objeto Personalizado**: registro de dados que contém informações de campo adicionais que podem ser vinculadas a um cliente potencial ou a uma empresa. Um registro pode ser composto de campos de cliente potencial ou de empresa padrão e campos de registro de objeto personalizado. Formalmente conhecido como cartão de dados ou linha da tabela de dados. **Campo de Registro de Objeto Personalizado**: campos totalmente personalizáveis para coletar informações exclusivas ou temporárias. Esses campos são criados e alojados dentro do próprio objeto personalizado. Formalmente conhecido como campo de cartão de dados ou campo/coluna de tabela do banco de dados. **Campo de link**: tipo especial de campo de registro de objeto personalizado para definir a relação entre o registro de objeto personalizado e o registro de objeto de Cliente Potencial/Empresa vinculado. Ao criar objetos personalizados, você deve fornecer campos de link para conectar o registro de objeto personalizado ao registro pai correto.

* Para uma estrutura personalizada de um para muitos ou de um para um, use o campo de link no objeto personalizado para conectá-lo a uma pessoa ou empresa.
* Para uma estrutura muitos para muitos, você usa dois campos de link, conectados de um objeto intermediário criado separadamente (que também é um tipo de objeto personalizado). Um link se conecta a pessoas ou empresas no banco de dados e o outro se conecta ao objeto personalizado. Nesse caso, o campo de link não está localizado no próprio objeto personalizado.

### Atividades personalizadas

**Há várias maneiras de alguém interagir com nossa organização. Eles podem visitar o site de sua empresa, participar de um de seus shows ou talvez clicar em um link em um email enviado por você. Essas ações são atividades** e, qualquer que seja a ação executada, a Marketo as captura para que suas equipes de marketing e vendas possam entender melhor o comportamento do usuário para um envolvimento personalizado e unificado. **_As_** Atividades personalizadas _podem ajudá-lo a rastrear uma atividade que não esteja relacionada a um formulário, email ou página de aterrissagem do Marketo_. Por exemplo, se você quiser rastrear quando alguém assistiu a um vídeo em um site ou respondeu a uma pesquisa, use uma atividade personalizada. As atividades personalizadas diferem dos objetos personalizados. Use objetos personalizados quando o valor puder mudar (ou seja, a &quot;cor do carro&quot; muda de azul para vermelho). Use atividades personalizadas ao rastrear momentos que ocorreram e seus detalhes não podem ser alterados (ou seja, &quot;carro comprado&quot;). Por padrão, o limite máximo de atividades personalizadas que podem ser definidas é 10. Isso pode ser aumentado com uma taxa de assinatura adicional. De acordo com a [política de retenção de dados do Marketo](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents), as atividades personalizadas serão excluídas automaticamente após 25 meses.

**Atividade personalizada:** eventos que não são da Marketo que você gostaria de rastrear dentro do Marketo. **ID de atividade personalizada:** o Marketo atribui uma ID numérica à atividade personalizada que pode ser usada ao tentar enviar/receber os dados da atividade usando a API do Marketo. **Campos de Atividade Personalizados:** Os metadados de atividade podem ser armazenados em um campo de atividade. Por exemplo, se você estiver rastreando as exibições no vídeo, os campos podem ser URL da página, Título do vídeo etc. **Campo Primário de Atividade Personalizado:** campos de atividade personalizados que podem ser usados como critérios de filtragem da lista inteligente.

**Dicas rápidas**

* Os pontos de extremidade de API para atividades personalizadas estão disponíveis [aqui.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST)

## Objeto personalizado vs. atividade personalizada

**Objeto personalizado**

**Atividade personalizada**

1

Máximo de 10 objetos personalizados por padrão por instância.

Máximo de 10 atividades personalizadas por padrão.

2

Máximo de 50 campos de objeto personalizados por objeto personalizado.

Cada tipo de atividade personalizada pode ter até 20 atributos secundários.

3

Máximo de registros de objetos personalizados de 1 MM; maio, com base em sua assinatura.

Após 25 meses, as atividades personalizadas serão excluídas de acordo com a política de retenção de dados da Marketo.

4

Pode ser usado como Filtro e Acionador em Smart Lists e Campanhas inteligentes.

Pode ser usado como Filtro e Acionador em Smart Lists e Campanhas inteligentes.

5

Pode ser usado para personalizar o conteúdo do email.

Não pode ser usado para personalizar o conteúdo do email.

6

Pode executar a operação CRUD em um registro de objeto personalizado.

Somente Criar e Ler é permitido em atividades personalizadas.

7

Use objetos personalizados quando o valor puder mudar (ou seja, a &quot;cor do carro&quot; muda de azul para vermelho).

Use atividades personalizadas ao rastrear momentos que ocorreram e seus detalhes não podem ser alterados (ou seja, &quot;carro comprado&quot;).

8

Objetos personalizados informam o fato. ou seja, valor presente.

As atividades personalizadas informam os eventos que ocorreram no passado.

Publicado em _2020-10-18_ por _Amit_

## Atualizações de janeiro de 2021

Em janeiro de 2021, lançaremos novas APIs REST e resolveremos vários defeitos. Consulte a lista completa de atualizações abaixo.

* Adição do ponto de extremidade [Enviar Formulário](/help/rest-api/leads.md), que permite executar envios de formulários programáticos. Formulários de terceiros agora podem ser integrados aos formulários do Marketo para aproveitar os fluxos de trabalho de marketing existentes.
* Adição do ponto de extremidade [Obter conteúdo completo da página de aterrissagem](/help/rest-api/landing-pages.md), que retorna a versão serializada do HTML de uma página de aterrissagem. Permite renderizar visualizações totalmente personalizadas de páginas de aterrissagem sem precisar fazer logon no Marketo Engage. Isso pode ajudar a simplificar os fluxos de trabalho de edição e tradução em aplicativos integrados.
* Agora você pode configurar o número de objetos personalizados disponíveis para acesso por meio do script do Velocity. As instruções de configuração podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).

### Resoluções de Defeitos

* Correção de um problema em que o ponto de extremidade [Excluir Usuário](/help/rest-api/user-management.md) permitia excluir um Usuário Somente API que estava em uso por um Serviço Personalizado. Agora, ele retorna o erro &quot;611, Não é possível excluir um usuário da API que está sendo usado no serviço de API&quot;. [LM-141893]
* Correção de um problema em que o ponto de extremidade [Obter Usuários](/help/rest-api/user-management.md) retornava usuários excluídos em alguns casos. [LM-141542]
* Correção de um problema em que o ponto de extremidade [Clonar Programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST). Se você especificou um nome de programa que excedeu 255 caracteres, ele retornaria &quot;611, Não é possível clonar o erro do programa&quot;. Agora, retorna &quot;701, o nome não pode exceder mais de 255 caracteres&quot;. [LM-143436]
* Corrigido o problema com o ponto de extremidade [Aprovar rascunho da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST). Ao aprovar uma landing page com a versão móvel ativada, você veria o conteúdo da versão móvel na versão da área de trabalho em determinados casos. [LM-146867]
* Correção de um problema com o ponto de extremidade [Cancelar aprovação da página de aterrissagem](https://developer.adobe.com/marketo-apis/api/asset/#operation/unapproveLandingPageByIdUsingPOST), que permitia cancelar a aprovação de uma página de aterrissagem que estava em uso como página de acompanhamento por um ou mais formulários. Agora, ele retorna o erro &quot;709, Falha ao desaprovar página de aterrissagem. A página de aterrissagem está sendo usada por um ou mais formulários como página de acompanhamento com IDs de formulário:[_formId1,formId2,..._]&quot;. [LM-143326]

Publicado em _2021-01-15_ por _David_

## API do Beta e do Beacon do Munchkin 160

**27 de janeiro de 2021:** alguns usuários do Marketo que serão afetados pela desativação da opção Associar cliente potencial receberam uma notificação por email com erro, indicando que a configuração do Munchkin Beta está habilitada em uma ou mais de suas instâncias. Esta versão será mantida até que o público-alvo correto possa ser notificado. A partir da versão 160 do Munchkin JavaScript, a [API de sinal](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) se torna a maneira padrão pela qual o Munchkin se comunica com o back-end do Marketo. Isso ficou disponível como uma opção no verão de 2020, com o lançamento da versão 159, por meio do parâmetro de configuração **useBeaconAPI**. A API Beacon tem várias vantagens em relação ao uso do antigo método XMLHttpRequest, mas a principal melhoria é que é uma API assíncrona sem bloqueio para comunicação HTTP que está disponível para uso em todos os navegadores de Internet modernos. Embora a maioria dos usuários do Munchkin não perceba uma alteração no comportamento do site, essa atualização impedirá que o Munchkin bloqueie a navegação enquanto aguarda o envio de um evento de clique para o back-end ou, mais simplesmente, tudo isso elimina a possibilidade de o Munchkin causar o &quot;travamento&quot; de um navegador após clicar em um link para uma nova página. Essa tem sido uma ocorrência rara, mas frustrante para alguns clientes do Marketo.

A partir de 27 de janeiro de 2021, a implantação dessa versão estará em espera, aguardando reagendamento. Embora não haja previsão de problemas relacionados a essa alteração e nenhum tenha sido identificado durante os testes, é impossível para o Marketo testar todas as configurações de implantação possíveis do Munchkin e você pode testar essas alterações antecipadamente ou renunciar a essa alteração até a disponibilidade geral dessa versão. As instruções para vários cenários são apresentadas abaixo.

### Testar API do sinal

Se você quiser testar a API de Beacon atualizada em antecipação à versão futura, poderá fazê-lo adicionando o parâmetro **useBeaconAPI** à sua configuração do Munchkin em uma página de teste externa. Este teste funcionará com a versão disponível ou beta do Munchkin. O parâmetro de configuração é mostrado abaixo no segundo argumento da invocação do método `Munchkin.init()` na linha 7: `{ 'useBeaconAPI': true}`

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827', {"useBeaconAPI":true});
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

### Desativar o Munchkin Beta nas páginas de aterrissagem do Marketo

Para desabilitar o Munchkin Beta nas páginas de aterrissagem do Marketo, você precisa acessar o menu [Treasure Chest](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) na seção Admin da sua assinatura e alterar a configuração do Munchkin Beta nas Páginas de Aterrissagem para desabilitada.

### Desabilitar o Munchkin Beta em páginas externas

Beta Se você implantou a versão do Munchkin JavaScript para páginas da Web externas e deseja renunciar a essa alteração até que ela esteja disponível, é necessário alterar seu trecho JS do Munchkin para direcionar o **munchkin.**&#x200B;**js** arquivo em vez do **munchkin-beta.Arquivo &#x200B;**&#x200B;**js**. No exemplo abaixo, este é o valor da variável **s.src** na linha 11. Seu trecho pode não ser muito semelhante ao exemplo, ou pode ser implantado por um gerenciador de tags em suas páginas externas, e talvez seja necessário entrar em contato com seus recursos de TI ou com qualquer pessoa que gerencie seus sites com o rastreamento Munchkin ativado.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';//This line should have the munchkin.js file, not munchkin-beta.js
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

Publicado em _2021-01-08_ por _Kenny_

## Substituição final da API do Email V1

[A descontinuação do Email V1 começou há quase dois anos](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) e, a partir da versão de manutenção de março das assinaturas de Londres e Holanda em 17 de março de 2021 e de todas as outras assinaturas em 19 de março de 2021, todo o suporte de API para emails V1 será encerrado. Após esta versão, qualquer tentativa de interagir com emails V1 por meio das APIs de ativos resultará em erros e nenhuma ação será tomada. Todos os usuários restantes conhecidos desde 24 de fevereiro de 2021 foram notificados, mas é possível que ainda haja integrações que possam tentar interagir com esses ativos. Os tipos mais comuns de integrações afetadas são serviços que oferecem gerenciamento de ativos digitais, tradução e localização. Se você observar falhas de integração como resultado dessa alteração, [você ainda poderá atualizar os ativos problemáticos editando e aprovando-os](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/transitioning-to-email-editor-2-0). Depois que um ativo de email for atualizado para a versão V2, você poderá retomar seu uso com os serviços integrados.

Publicado em _2021-03-17_ por _Kenny_

## Atualizações de maio de 2021

Em maio de 2021, lançaremos novas APIs REST, aprimoraremos as APIs REST existentes e resolveremos vários defeitos. Consulte a lista completa de atualizações abaixo.

* Adicionadas APIs de membros do programa que permitem recuperar, atualizar e excluir registros de associação do programa. Para obter mais informações, consulte [API REST > Banco de dados de clientes potenciais > Membros do programa](/help/rest-api/program-members.md).
* Foram adicionadas APIs de extração de objetos personalizados em massa que permitem exportar registros de primeiro nível do Objeto personalizado do Marketo que estão associados a clientes potenciais em uma relação um para muitos. Para obter mais informações, consulte [API REST > Extração em massa > Extração de objeto personalizado em massa](/help/rest-api/bulk-custom-object-extract.md).
* Aprimoramos a [API de lead](/help/rest-api/leads.md) e a [API de extração de lead em massa](/help/rest-api/bulk-lead-extract.md) para permitir que os usuários recuperem a Adobe Experience Cloud Id (ECID). Isso permite que os usuários que [Sincronizam Públicos-alvo da Adobe Experience Cloud](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html?lang=pt-BR) identifiquem clientes potenciais que tenham ECIDs associadas. Isso abre [possibilidades de integração](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360024277392-Adobe-Experience-Cloud-Using-the-ECID-for-integration) com outros produtos da Adobe Experience Cloud.
* Aprimoramos a [API de Importação de Cliente Potencial em Massa](/help/rest-api/bulk-lead-import.md) para oferecer suporte à adição de clientes potenciais aos registros da empresa durante o processo de importação. Isso é feito incluindo o campo **externalCompanyId** no arquivo de importação.
* Aprimoramos vários endpoints do Programa para fornecer paridade com a funcionalidade encontrada na interface do usuário do Marketo Engage. Aprimoramos os pontos de extremidade [Criar Programas](/help/rest-api/assets.md) e [Clonar Programas](https://developer.adobe.com/marketo-apis/api/asset/) para permitir operações de criação, clonagem ou movimentação em programas de evento. Isso é para usuários que organizam programas de evento &quot;aninhando&quot;-os em outros tipos de programas. Também aprimoramos o ponto de extremidade do [Excluir programa](https://developer.adobe.com/marketo-apis/api/asset/) para permitir a exclusão de programas que contêm os seguintes ativos: Notificações por push, Mensagens no aplicativo, Relatórios, Páginas de aterrissagem com o Social Assets inserido.
* Como Administrador do Marketo, você pode [marcar um campo específico como &quot;confidencial&quot;](https://experienceleague.adobe.com/pt-br/docs/marketo/using/home?lang=pt-BR) para que seus valores [nunca sejam preenchidos previamente em formulários](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/demand-generation/forms/form-fields/disable-pre-fill-for-a-form-field), protegendo, assim, os dados confidenciais dos usuários. Aprimoramos vários endpoints de campo de formulário para fornecer paridade com essa funcionalidade encontrada na interface do usuário do Marketo Engage.

### Resoluções de Defeitos

* Correção de um problema com o endpoint Excluir programa. Se você tentasse excluir um programa em uma pasta compartilhada, retornaria &quot;611, Erro do sistema&quot;. Agora retorna &quot;O programa de destino está em uma pasta compartilhada e não pode ser excluído. O compartilhamento da pasta deve ser cancelado antes da tentativa de exclusão.&quot;
* Correção de um problema com o endpoint do programa de clonagem. Se você tentasse clonar um programa que continha um DateTime em uma etapa de fluxo, ele retornaria &quot;611, Erro do Sistema&quot;. Agora, ela clona o programa com êxito.
* Correção de um problema no endpoint Criar programas que, inadvertidamente, permitia a criação de um programa sob um programa de email (o que não é permitido).
* Correção de um problema com o endpoint do programa de clonagem. Se você clonou um programa que continha uma landing page, o nome da landing page no programa do público-alvo não tinha um sublinhado entre o nome do programa e o nome da landing page. Ex.: `http://<_pod_\>.marketo.com/lp/<_munchkin_\>/<_program name_\>**_**<_LP name_\>.html`

Publicado em _2021-05-07_ por _David_

## Oferta por tempo limitado para ingressar no Adobe Exchange

O suporte à comunidade de parceiros da Marketo Engage é um dos pilares do sucesso de nossos clientes. Queremos garantir que o ecossistema de integração do Marketo Engage esteja bem representado no Exchange Marketplace e tenha uma oferta especial para parceiros do LaunchPoint. Por um tempo muito limitado, que não será estendido, oferecemos aos nossos parceiros do LaunchPoint uma parceria gratuita Innovate no programa Exchange até o final de 2022 (valor aproximado de US$ 15 mil). Criamos essa oferta para incentivar os parceiros do LaunchPoint a criar suas listas de integração no Portal do parceiro do Exchange, que serão pesquisadas publicamente no Adobe Exchange Marketplace. Para obter uma lista completa dos benefícios da parceria Innovate que você recebe gratuitamente até dezembro de 2022.

1. Acesse o [Centro de Suporte ao Parceiro da Adobe Exchange](https://adobeexchangeec.zendesk.com/hc/en-us?mkt_tok=NjA4LURIVi05MTUAAAF-P5lIeVWOuBmKMS_uE_NpgFKtC0ukt7z_ksnq_Sbzb6mzXUuXpqpqQeujtPdZ24WcjMdptygQSR9XrYt_Cw)
1. Clique em &quot;Enviar uma solicitação&quot; no canto superior direito
1. Em **Escolha seu problema abaixo**, selecione &quot;Suporte da Adobe Exchange&quot;
1. Em **Seu endereço de email**, digite seu endereço de email
1. Na caixa **Assunto**, digite &quot;Oferta do LaunchPoint&quot;
1. Na caixa **Descrição**, digite &quot;Oferta do LaunchPoint&quot;
1. Na lista suspensa **Tipo de suporte**, selecione &quot;Suporte a programas&quot;
1. Na lista suspensa **Produto Adobe Exchange**, selecione &quot;Programa Adobe Exchange&quot;
1. Envie o Formulário. Nossa equipe entrará em contato com você em breve.

Publicado em _2021-07-22_ por _David_

## Atualizações de agosto de 2021

Em agosto de 2021, estamos aprimorando as APIs REST existentes e resolvendo vários defeitos. Consulte a lista completa de atualizações abaixo.

* Aprimoramos a API de extração de atividade em massa para permitir que os usuários filtrem usando atributos primários para 6 tipos de atividades diferentes. Para obter mais informações, consulte [Extração de atividade em massa](/help/rest-api/bulk-activity-extract.md).
* Para dar aos usuários do Marketo Sales Connect mais acesso aos dados de atividade de vendas, ativamos atributos adicionais de atividade de vendas. Adicionamos os seguintes atributos às atividades Enviar email de vendas, Abrir email de vendas e Clicar em Email de vendas:

* ID da pessoa de vendas da Marketo - ID exclusiva para registro de pessoa no Sales Connect
* Nome da campanha de vendas - Nome da campanha de vendas, se o email fizer parte de uma campanha de vendas
* URL da campanha de vendas - URL de conexão de vendas da campanha de vendas se o email fizer parte de uma campanha de vendas
* Nome do Modelo de Vendas - Nome do modelo de email no Sales Connect, se um modelo tiver sido usado
* URL do Modelo de Vendas - URL do Sales Connect para o modelo de email, se um modelo tiver sido usado

### Emails

* Aprimoramos o ponto de extremidade Obter Emails adicionando o filtro `earliestUpdatedAt`/`latestUpdatedAt`. Isso permite usar o campo `updatedAt` para pesquisar apenas por um subconjunto de emails, permitindo a sincronização incremental.
* Aprimoramos os endpoints Obter Emails, Obter Email por Nome, Obter Email por ID para dar suporte à recuperação de registros de email do tipo [Champion e Challenger](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/email-tests-champion-challenger/add-an-email-champion-challenger).

### Resoluções de Defeitos

* Correção de um problema com o endpoint Obter usuários. Os usuários aos quais foi emitida uma licença do [Calendário de marketing](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/marketing-calendar/understanding-the-calendar/issue-revoke-a-marketing-calendar-license) não foram retornados. Os usuários do Calendário de marketing agora são retornados corretamente.
* Correção de um problema com o endpoint Enviar formulário. No caso de registros de lead duplicados, Enviar formulário é usado para emitir o erro &quot;1007, Vários critérios de pesquisa de correspondência de lead&quot;. Enviar Formulário agora atualiza o registro atualizado mais recentemente da mesma forma que a [API do Forms 2.0](/help/javascript-api/forms-api-reference.md) atualiza.
* Foram aprimoradas várias mensagens de erro enganosas retornadas por Atualizar campo de cliente potencial e Criar pontos de extremidade de campos de cliente potencial. [LM-151890, LM-151888, LM-151889]
* Correção de um problema com os endpoints Obter campo de cliente potencial por nome e Obter campos de cliente potencial. Ambos os endpoints podem retornar informações ligeiramente desatualizadas. Agora, elas sempre retornam as informações atuais.
* Correção de um problema com a [API de extração em massa](/help/rest-api/bulk-extract.md) ao usar o cabeçalho HTTP &quot;intervalo&quot; para recuperação parcial, em que o último byte no intervalo não era retornado.
* Correção de um problema com o endpoint Update Snippet Metadata. Ao atualizar o nome ou a descrição do trecho, o status era alterado para &quot;aprovado com rascunho&quot;. Agora, o status do trecho permanece inalterado após a atualização do nome ou da descrição do trecho.
* Correção de um problema com o endpoint Adicionar módulo de email. Ao adicionar um módulo que continha um trecho, ele retornava um &quot;611, Erro do sistema&quot;. Agora, ele adiciona corretamente o módulo ao email.
* Correção de um problema com o endpoint Excluir programa. Ao excluir um programa que continha um ativo local de mensagens no aplicativo, ele retornava um &quot;Erro de sistema do 611&quot;. Agora, ele exclui corretamente o programa.

Publicado em _2021-08-22_ por _David_

## Implantação do Munchkin versão 161

Em 7 de setembro de 2021, a versão 161 do Munchkin começará a ser lançada para 10% das assinaturas com o Munchkin Beta habilitado, seguido de 50% em 16 de setembro e 100% em 30 de setembro. Essa alteração afetará as páginas de aterrissagem do Marketo e a versão do arquivo munchkin-beta.js veiculada para páginas de aterrissagem externas carregadas de assinaturas para as quais a nova versão foi implementada. Essa versão substitui totalmente o método Munchkin Associate Lead, um recurso que permite o envio de dados pessoais para uma assinatura do Marketo e o histórico de navegação na Web associado a um registro de pessoa conhecida. O Associar Cliente Potencial está sendo removido em favor de alternativas mais modernas e seguras, como a [API JS do Forms](/help/javascript-api/forms-api-reference.md), a API de Envio de Formulário e a [API REST de Cliente Potencial Associado](/help/rest-api/leads.md). Se você ou sua organização usarem esse método, você deverá sair do uso até 12 de outubro de 2021, quando a implantação da versão de outubro estiver programada para começar. Se você não quiser mais participar do Munchkin beta, poderá desabilitar o uso nas páginas de aterrissagem do Marketo alternando o recurso &quot;Munchkin Beta nas Páginas de Aterrissagem&quot; para `disabled` no [menu Treasure Chest](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features). Se você implantou o Munchkin Beta JavaScript em páginas da Web externas e deseja alternar para o canal de lançamento padrão do Munchkin, é necessário atualizar o trecho de código para carregar o Munchkin JavaScript do munchkin.js em vez do munchkin-beta.js.

Publicado em _2021-08-24_ por _Kenny_

## Fim da vida útil do TLS 1.0 e TLS 1.1 para o serviço da Munchkin

A partir de 21 de outubro de 2021, o munchkin.marketo.net, que é usado para servir o Munchkin JavaScript para os visitantes, não aceitará mais conexões usando o [TLS 1.0 ou TLS 1.1.](https://en.wikipedia.org/wiki/Transport_Layer_Security) Esses padrões de criptografia não são mais aceitos como parte das práticas recomendadas de segurança da Web e não são mais compatíveis com as versões modernas do navegador. Não há impactos negativos previstos como resultado dessa alteração.

Publicado em _2021-10-04_ por _Kenny_

## Atualizações de outubro de 2021

Em outubro de 2021, estamos aprimorando as APIs REST existentes e resolvendo vários defeitos. Consulte a lista completa de atualizações abaixo.

* Aprimoramos o ponto de extremidade [Enviar Formulário](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) para oferecer suporte a campos personalizados de membros do programa como parte do envio do formulário. Opcionalmente, um programa pode ser especificado como o programa ao qual adicionar o formulário e/ou o programa ao qual adicionar campos personalizados de membros do programa, conforme descrito [aqui](/help/rest-api/leads.md).
Aprimoramos o ponto de extremidade [Obter Membros do Programa](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getProgramMembersUsingGET) para oferecer suporte a consultas baseadas em intervalo de datas com base no atributo updatedAt. Isso é feito passando os parâmetros datetime inicial e final como descrito [aqui](/help/rest-api/program-members.md).
* Aprimoramos os [Campos de cliente potencial](/help/rest-api/leads.md) APIs para oferecer suporte a [Campos Confidenciais](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/field-management/mark-a-field-as-sensitive). Os pontos de extremidade [Obter Campo de Cliente Potencial por Nome](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldByNameUsingGET), [Obter Campos de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldsUsingGET), [Criar Campos de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) e [Atualizar Campo de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updateLeadFieldUsingPOST) agora oferecem suporte ao atributo isSensitive.

### Resoluções de Defeitos

* Correção de um problema com a API [Gerenciamento de usuários](/help/rest-api/user-management.md). Refere-se aos usuários do Marketo configurados para uso com o [Sales Insight](https://business.adobe.com/products/marketo/sales-insight.html). Esses usuários agora são retornados pelo ponto de extremidade [Obter Usuários](https://developer.adobe.com/marketo-apis/api/user/#operation/getUsersUsingGET), e agora eles podem ser excluídos usando o ponto de extremidade [Excluir Usuário](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST). [LM-155864]
* Correção de um problema com o ponto de extremidade Adicionar [Campo de Rich Text](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/addRichTextFieldUsingPOST). Ao adicionar um campo de rich text com mais de 65 mil caracteres a um email, uma landing page, um trecho ou um formulário, ele retornava um &quot;611, Erro do sistema&quot;. Agora retorna o erro &quot;701, A operação não pode ser concluída. ‘conteúdo’ excede um comprimento máximo de 65.535 bytes&quot;.

Publicado em _2021-10-25_ por _David_

## Atualizações de janeiro de 2022

Em janeiro de 2022, estamos aprimorando as APIs REST existentes e resolvendo vários defeitos. Consulte a lista completa de atualizações abaixo.

* Aprimoramos a API [Extração de Objeto Personalizado em Massa](/help/rest-api/bulk-custom-object-extract.md) para permitir que os usuários filtrem usando um intervalo de datas **updatedAt**.
* Adicionadas as APIs de metadados do campo Membro do programa que permitem criar, atualizar e recuperar metadados dos campos Membro do programa. Para obter mais informações, consulte [Membros do programa > Campos](/help/rest-api/program-members.md).
* Foram adicionadas APIs de metadados de campo da empresa que permitem recuperar metadados para campos da empresa. Para obter mais informações, consulte [Empresas > Campos](/help/rest-api/companies.md).
* Adição de APIs de metadados de campo de oportunidade que permitem recuperar metadados para campos de oportunidade. Para obter mais informações, consulte [Oportunidades > Campos](/help/rest-api/opportunities.md).
* Adicionadas as APIs de metadados do campo Conta nomeada que permitem recuperar metadados para campos de Conta nomeada. Para obter mais informações, consulte [Contas nomeadas > Campos](/help/rest-api/named-accounts.md).
* Atualização de pontos de extremidade de metadados de campo para retornar uma nova propriedade booleana **isApiCreated** para indicar se um campo foi criado ou não pela API REST.

### Resoluções de Defeitos

* Correção de um problema de latência entre o momento da chamada para o ponto de extremidade [Criar Campos de Cliente Potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) e o momento em que o campo de cliente potencial recém-criado estava disponível na lista inteligente. [LM-152838]
* Correção de um problema com o ponto de extremidade [Criar campos de cliente em potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST), no qual os campos criados não estavam disponíveis na lista suspensa de campos de formulário usada para [adicionar campos ao formulário](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/demand-generation/forms/creating-a-form/add-a-field-to-a-form) na interface do usuário do Marketo Engage. [LM-158243]
* Correção de um problema no ponto de extremidade [Get Campaigns](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET), em que campanhas acionáveis não eram retornadas quando o parâmetro isTriggerable=true era especificado. [LM-158283]
* Correção de um problema em que o ponto de extremidade [Obter clientes em potencial por ID de lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteTokenByNameUsingPOST) retornava o erro &quot;611, Erro do sistema&quot; em determinados casos. [LM-157214]
* Foram limpas várias mensagens de erro retornadas pelo ponto de extremidade [Atualizar Campo de Cliente Potencial](/help/rest-api/leads.md). [LM-151886, LM-151888, LM-151889]

Publicado em _2022-01-27_ por _David_

## Atualizações de março de 2022

Em março de 2022, estamos aprimorando as APIs REST existentes e resolvendo vários defeitos. Consulte a lista completa de atualizações abaixo.

* Adicionamos o campo **actionResult** ao arquivo de exportação produzido pela API de Extração de Atividade em Massa. Este campo pode ser usado para distinguir entre atividades bem-sucedidas, ignoradas e com falha.
* Adicionamos o campo **isOpenTrackingDisabled** para respostas da [API de emails](/help/rest-api/emails.md). Este campo pode ser usado para determinar se o recurso [Desabilitar Acompanhamento Aberto](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-editor-v2-0-overview) está habilitado.
* Adicionamos dois endpoints que permitem gerenciar seletivamente as tags do programa. O ponto de extremidade [Atualizar marcas do programa](/help/rest-api/programs.md) permite atualizar seletivamente uma marca de programa. O ponto de extremidade [Excluir marcas do programa](/help/rest-api/programs.md) permite excluir seletivamente uma marca de programa.
* Adicionamos o parâmetro **isExecutable** ao ponto de extremidade [Clone Smart Campaign](/help/rest-api/smart-campaigns.md). Esse parâmetro permite clonar um programa como um programa executável.
* Adicionamos o campo **headStart** à [API de programas](/help/rest-api/programs.md). Isso permite criar, atualizar e recuperar a configuração [Head Start](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/head-start-for-email-programs) para Programas de Email.

### Resoluções de Defeitos

* Correção de um problema com o ponto de extremidade [Obter Conteúdo Dinâmico de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET). Ao tentar recuperar linhas de assunto com conteúdo dinâmico de emails que tinham relacionamentos de modelo corrompidos, um erro foi retornado, 709, &quot;API só permite operações em emails com um modelo&quot;. O endpoint agora retorna o conteúdo dinâmico. [LM-152331]
* Correção de um problema com o ponto de extremidade [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST). Ao usar externalSalesPersonId para associar o Vendedor a um cliente potencial usando externalSalesPersonId e usando a ação = createDuplicate, a associação Vendedor não ocorreria. [LM-158990]

### Integração do Adobe IMS

* Aqueles que foram integrados ao [Adobe IMS](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) não podem utilizar todas as [APIs de gerenciamento de usuários do Marketo](/help/rest-api/user-management.md). Os pontos de extremidade a seguir retornarão um erro ao serem chamados em Instâncias do Marketo que foram integradas com o Adobe IMS: [Convidar Usuário](https://developer.adobe.com/marketo-apis/api/user/#operation/inviteUserUsingPOST), [Obter Usuário Convidado por ID](https://developer.adobe.com/marketo-apis/api/user/#operation/getInvitedUserUsingGET), [Atualizar Atributos de Usuário](https://developer.adobe.com/marketo-apis/api/user/#operation/updateUserAttributeUsingPOST), [Excluir Usuário](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST) e [Excluir Usuário Convidado](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteInvitedUserUsingPOST). Como substituição, as [APIs de Gerenciamento de Usuários do Adobe](https://developer.adobe.com/umapi/) devem ser usadas.

Publicado em _2022-03-14_ por _David_

## Atualizações de maio de 2022-

Em maio de 2022, estamos aprimorando as APIs REST existentes e resolvendo vários defeitos. Consulte a lista completa de atualizações abaixo.

* Adicionamos a capacidade de recuperar registros de [Empresa](/help/rest-api/companies.md), [Oportunidade](/help/rest-api/opportunities.md) e [Vendedores](/help/rest-api/sales-persons.md) quando a [Sincronização com o SFDC](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou a [Sincronização com o Microsoft Dynamics](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) estão habilitadas em sua instância do Marketo Engage.
* Atualizamos o ponto de extremidade [Obter Conteúdo Dinâmico de Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) para permitir que você recupere o [Conteúdo Dinâmico](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/using-dynamic-content-in-an-email) de uma linha de assunto do email. Isso funciona independentemente de o email fornecido estar vinculado a um template de email.

`POST /rest/asset/v1/form/{id}/field/State.json?values=[{"label":"Alaska"},{"value":"AK"},{"label":"West Virginia","value":"WV"},{"label":"Wyoming","value":"WY"}]`

* Atualizamos o ponto de extremidade [Adicionar Regras de Visibilidade do Campo de Formulário](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllProgramMemberFieldsUsingGET) para permitir que você adicione vários valores de comparação para **isNot** tipo [Regras de Invisibilidade](/help/rest-api/forms.md). Exemplo:

`POST /rest/asset/v1/form/{id}/field/LastName/visibility.json?visibilityRule={"ruleType":"show","rules":[{"subjectField":"LastName","operator":"isNot","values":["A","B","C"]}`

### Resoluções de Defeitos

* Correção de um problema com o ponto de extremidade [Enviar Formulário](/help/rest-api/leads.md) que ocorria ao passar &quot;null&quot; para um atributo no parâmetro [leadFormFields](/help/rest-api/leads.md), um erro foi retornado, &quot;611, Erro do Sistema&quot;. Agora, ele retorna corretamente o erro &quot;1003, Falha na validação do formulário&quot;. [LM-162213]

Publicado em _2022-05-09_ por _David_

## Atualizações de agosto de 2022

Em agosto de 2022, estamos aprimorando as APIs REST existentes. Consulte a lista completa de atualizações abaixo.

Adicionamos vários novos filtros que podem ser usados ao chamar o ponto de extremidade Criar Trabalho de Membro do Programa de Exportação. Observe que muitos filtros podem ser usados em combinação um com o outro para refinar o conjunto de dados extraído.

* O filtro **programIds** pode ser usado para especificar até 10 identificadores de programa que podem ajudar a melhorar a taxa de transferência.
* O filtro **isExhausted** pode ser usado para filtrar registros de [pessoas que esgotaram o conteúdo](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content).
* O filtro **NurtureCadence** pode ser usado para filtrar registros com base em [cadência do programa de envolvimento](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/program-flow-actions/change-engagement-program-cadence).
* O filtro **statusNames** pode ser usado para filtrar registros para um ou mais [status de programa](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/understanding-program-membership).
* O filtro **updatedAt** pode ser usado para filtrar registros com base em um intervalo de datas.

### Anúncios

* O comportamento do ponto de extremidade [Identidade](https://developer.adobe.com/marketo-apis/api/identity/#operation/identityUsingGET) foi alterado. Ao chamar o ponto de extremidade e não incluir um parâmetro **access_token**, o erro &quot;603, Acesso negado&quot; é retornado. Anteriormente, o erro &quot;600, Token de acesso vazio&quot; era retornado. Observe que o erro &quot;600, Token de acesso vazio&quot; foi descontinuado.

Publicado em _2022-09-03_ por _David_

## Atualizações de outubro de 2022

Em outubro de 2022, estamos aprimorando as APIs REST existentes. Consulte a lista completa de atualizações abaixo.

* Aprimoramos a [API de Importação de Cliente Potencial em Massa](/help/rest-api/bulk-lead-import.md) para oferecer suporte à adição de Clientes Potenciais aos registros de Vendedores durante o processo de importação. Isso é feito incluindo o campo **externalSalesPersonId** no arquivo de importação.
* Correção de um problema com o ponto de extremidade [Criar campos de cliente em potencial](/help/rest-api/leads.md) que ocorria ao criar campos do tipo Pontuação. Esses campos não estavam disponíveis para uso na ação de fluxo [Pontuação da alteração](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/change-score) na interface do usuário do Marketo Engage. [LM-166815]

### Anúncios

* O atributo de Associação de Programa `acquiredBy` foi alterado para atualizável.

Publicado em _2022-10-18_ por _David_

## Alteração futura no Marketo Forms REST AP

A partir da versão 2022.R2, atualmente agendada para a semana de 24 de março de 2023, as APIs do Adobe Marketo Engage Forms Asset retornarão consistentemente apenas o nome do formulário sem um nome de programa prefixado, independentemente de o formulário ser filho de um programa ou não. Essa alteração tornará os comportamentos da API do Forms em relação aos nomes de ativos consistentes com o restante das APIs de ativos da Adobe Marketo Engage. Para evitar interrupções do serviço, você deve revisar todas as integrações que usam as APIs do Marketo Engage Forms e trabalhar com os integradores para verificar se é necessário fazer alterações para acomodar isso. Antes dessa alteração futura, os nomes retornados pelas APIs do Forms prefixavam de forma inconsistente um nome de programa para formulários que são filhos de programas em Atividades de marketing. Por exemplo, um formulário chamado &quot;Formulário 1&quot; que era filho de &quot;Programa 1&quot; pode ter seu nome retornado pela API como: Programa 1.Formulário 1 Ou Formulário 1 A partir da versão 2022.R2, o nome de um formulário sempre será retornado sem um nome de programa prefixado. Usando o mesmo exemplo, o nome sempre seria: Formulário 1

Publicado em _2022-11-04_ por _Kenny_

## Atualizações de janeiro de 2023

Em janeiro de 2023, fizemos uma melhoria relacionada à API na interface do usuário do administrador e estamos fazendo dois anúncios. Consulte a lista completa de atualizações abaixo.

Interface do administrador

### Extração de chumbo em massa

* Aprimoramos a interface do administrador do Marketo Engage para permitir que você visualize a alocação diária de capacidade da API de extração em massa para sua assinatura. Além disso, você pode visualizar o uso da capacidade por API-User nos últimos 7 dias. Mais informações podem ser encontradas [aqui](https://experienceleague.adobe.com/pt-br/docs/marketo/using/product-docs/administration/settings/bulk-export-api-information).

### Resoluções de Defeitos

* Correção de um problema com o ponto de extremidade [Excluir Oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST). Em certos casos, uma atividade &quot;Remover da oportunidade&quot; não estava sendo gerada. [LM-172208]

### Anúncios

* Consulte [este artigo](https://nation.marketo.com/t5/product-documents/upcoming-change-to-marketo-rest-api/ta-p/331698) na Comunidade do Marketo sobre a API REST e uma alteração na mensagem de resposta HTTP [frase de motivo](https://www.rfc-editor.org/rfc/rfc7230#section-3.1.2).
* O atributo de Associação de Programa **statusReason** foi alterado para ser atualizável.

Publicado em _2023-01-21_ por _David_
