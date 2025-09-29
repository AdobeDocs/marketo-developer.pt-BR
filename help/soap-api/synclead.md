---
title: syncLead
feature: SOAP
description: Saiba como usar o Marketo SOAP syncLead para inserir ou atualizar um único lead, manipular identificadores e espaços de trabalho, com campos de solicitação e exemplos XML e PHP.
exl-id: e6cda794-a9d4-4153-a5f3-52e97a506807
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 2%

---

# syncLead

Esta função insere ou atualiza um único registro de lead. Ao atualizar um cliente potencial existente, o cliente potencial é identificado com uma das seguintes chaves:

- ID do Marketo
- ID de sistema externo (implementada como `foreignSysPersonId`)
- Cookie do Marketo (criado pelo script JS do Munchkin)
- Email

Se uma correspondência existente for encontrada, a chamada executará uma atualização. Caso contrário, insere e cria um lead. Clientes potenciais anônimos podem ser atualizados usando a ID de cookie do Marketo e se tornarão conhecidos após a atualização.

Exceto email, todos esses identificadores são tratados como chaves exclusivas. A Marketo ID tem prioridade sobre todas as outras chaves. Se `foreignSysPersonId` e a Marketo ID estiverem presentes no registro de cliente potencial, a Marketo ID terá prioridade e a `foreignSysPersonId` será atualizada para esse cliente potencial. Se o único `foreignSysPersonId` for fornecido, ele será usado como um identificador exclusivo. Se `foreignSysPersonId` e o Email estiverem presentes, mas a Marketo ID não estiver presente, o `foreignSysPersonId` terá prioridade e o Email será atualizado para esse cliente potencial.

Opcionalmente, um Cabeçalho de contexto pode ser especificado para nomear o espaço de trabalho de destino.

Quando os espaços de trabalho do Marketo estão ativados e o cabeçalho é usado, as seguintes regras são aplicadas:

- Se as regras de atribuição forem definidas e um novo cliente potencial for qualificado para qualquer uma das regras configuradas, novos clientes potenciais serão criados na partição definida pela regra de atribuição. Caso contrário, novos clientes em potencial serão criados na partição primária do espaço de trabalho nomeado.
- Clientes potenciais correspondentes à ID do cliente potencial da Marketo, a uma ID de sistema externo ou a um Cookie da Marketo devem existir na partição primária do espaço de trabalho nomeado, caso contrário, um erro será retornado
- Se um lead existente for correspondido por email, o espaço de trabalho nomeado será ignorado e o lead será atualizado na partição atual dele

Quando os espaços de trabalho do Marketo estão ativados e o cabeçalho NÃO é usado, as seguintes regras são aplicadas:

- Se as regras de atribuição forem definidas e um novo cliente potencial for qualificado para qualquer uma das regras configuradas, novos clientes potenciais serão criados na partição definida pela regra de atribuição. Caso contrário, novos clientes em potencial serão criados na partição primária do espaço de trabalho &quot;Padrão&quot;.
- Clientes potenciais existentes são atualizados na partição atual

Se os espaços de trabalho do Marketo NÃO estiverem ativados, o espaço de trabalho de destino DEVERÁ ser o espaço de trabalho &quot;Padrão&quot;. Não é necessário passar o cabeçalho.

## Solicitação

| Nome do campo | Obrigatório/Opcional | Descrição |
| --- | --- | --- |
| leadRecord->Id | Obrigatório - Somente quando um Email ou `foreignSysPersonId` não estiver presente | A Marketo Id do registro de cliente potencial |
| leadRecord->Email | Obrigatório - Apenas quando a ID ou `foreignSysPersonId` não está presente | O endereço de email associado ao registro de cliente potencial |
| leadRecord->`foreignSysPersonId` | Obrigatório - Somente quando a ID ou o Email não estiver presente | A ID do sistema externo associada ao registro de cliente potencial |
| leadRecord->ForeignSysType | Opcional - Necessário somente quando `foreignSysPersonId` está presente | O tipo de sistema externo. Valores possíveis: CUSTOM, SFDC, NETSUITE |
| leadRecord->leadAttributeList->attribute->attrName | Obrigatório | O nome do atributo de cliente potencial do qual você deseja atualizar o valor. |
| leadRecord->leadAttributeList->attribute->attrValue | Obrigatório | O valor que você deseja definir para o atributo de cliente potencial especificado em attrName. |
| returnLead | Obrigatório | Quando verdadeiro, retorna o registro de lead completo atualizado após a atualização. |
| marketoCookie | Opcional | O cookie [Munchkin javascript](../javascript-api/lead-tracking.md) |

## XML de solicitação

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
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
        <Email>t@t.com</Email>
        <leadAttributeList>
          <attribute>
            <attrName>FirstName</attrName>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de resposta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1089965</leadId>
        <syncStatus>
          <leadId>1089965</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Código de exemplo - PHP

```php
 <?php

  $debug = true;

  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";

  // Create Signature
  $dtzObj = new DateTimeZone("America/Los_Angeles");
  $dtObj  = new DateTime('now', $dtzObj);
  $timeStamp = $dtObj->format(DATE_W3C);
  $encryptString = $timeStamp . $marketoUserId;
  $signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);

  // Create SOAP Header
  $attrs = new stdClass();
  $attrs->mktowsUserId = $marketoUserId;
  $attrs->requestSignature = $signature;
  $attrs->requestTimestamp = $timeStamp;
  $authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
  $options = array("connection_timeout" =20, "location" =$marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }

  // Create Request
  $leadKey = new stdClass();
  $leadKey->Email = "george@jungle.com";

  // Lead attributes to update
  $attr1 = new stdClass();
  $attr1->attrName  = "FirstName";
  $attr1->attrValue = "George";

  $attr2= new stdClass();
  $attr2->attrName  = "LastName";
  $attr2->attrValue = "of the Jungle";

  $attrArray = array($attr1, $attr2);
  $attrList = new stdClass();
  $attrList->attribute = $attrArray;
  $leadKey->leadAttributeList = $attrList;

  $leadRecord = new stdClass();
  $leadRecord->leadRecord = $leadKey;
  $leadRecord->returnLead = false;
  $params = array("paramsSyncLead" =$leadRecord);

  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $result = $soapClient->__soapCall('syncLead', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }

  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }
  print_r($result);

?>
```

## Código de exemplo - Java

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


public class SyncLead {

    public static void main(String[] args) {
        System.out.println("Executing syncLead");
        try {
            URL marketoSoapEndPoint = new URL("https://100-AEK-913.mktoapi.com/soap/mktows/2_1" + "?WSDL");
            String marketoUserId = "demo17_1_809934544BFABAE58E5D27";
            String marketoSecretKey = "27272727aa";

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
            ParamsSyncLead request = new ParamsSyncLead();
            LeadRecord key = new LeadRecord();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Stringemail = objectFactory.createLeadRecordEmail("george@jungle.com");
            key.setEmail(email);
            request.setLeadRecord(key);

            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George2");

            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");

            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);

            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttributeattrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);
            key.setLeadAttributeList(attrList);

            MktowsContextHeader headerContext = new MktowsContextHeader();
            headerContext.setTargetWorkspace("default");

            SuccessSyncLead result = port.syncLead(request, header, headerContext);

            JAXBContext context = JAXBContext.newInstance(SuccessSyncLead.class);
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

## Código de exemplo - Ruby

```ruby
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "http://www.marketo.com/mktows/"

#Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

#Create SOAP Header
headers = {
    'ns1:AuthenticationHeader' ={ "mktowsUserId" =mktowsUserId, "requestSignature" =requestSignature,
    "requestTimestamp"  =requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record ={
        :Email ="t@t.com",
          :lead_attribute_list ={
              :attribute ={
                :attr_name ="FirstName",
                :attr_value ="George" },
              :attribute! ={
                :attr_name ="LastName",
                :attr_value ="of the Jungle" }
        }
    },
    :return_lead ="false"
}

response = client.call(:sync_lead, message: request)

puts response
```
