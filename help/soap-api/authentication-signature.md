---
title: "Assinatura de autenticação"
feature: SOAP
description: "Segurança de API com assinaturas de autenticação"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 3%

---


# Assinatura de autenticação

A segurança da API do Marketo usa um modelo simples, mas altamente seguro, com base em assinaturas HMAC-SHA1 com mensagens transmitidas por HTTPS. Uma vantagem importante desse modelo é que ele fornece autenticação sem estado.
As assinaturas HMAC-SHA1 exigem o seguinte:

* Uma ID de usuário (também chamada de Chave de acesso) transmitida com a solicitação de serviço
* Uma Assinatura que é calculada usando uma chave secreta compartilhada e o conteúdo da mensagem e é transmitida com a solicitação de serviço
* Uma chave secreta compartilhada (também chamada de Chave de criptografia) que não é transmitida com a solicitação de serviço

Essas informações de segurança são confirmadas por meio de Admin > API SOAP no Marketo.
O programa cliente calculará a assinatura HMAC-SHA1 usando a chave secreta compartilhada e parte do conteúdo da mensagem de solicitação. O cliente deve incluir um cabeçalho SOAP, AuthenticationHeaderInfo, para passar informações de autenticação com a mensagem SOAP.
O seguinte pseudocódigo demonstra o algoritmo:

```
// Request timestamp: a timestamp string in W3C WSDL date format
stringToEncrypt = requestTimestamp + clientAccessID;

signatureBytes = Encryter.encrypt('SHA1', secretKey, stringToEncrypt);

signature = toLowerCase( hexEncode(signatureBytes) );

authHeader = "<ns1:AuthenticationHeader>" +
    "<mktowsUserId>" + clientAccessID + "</mktowsUserId>" +
    "<requestSignature>" + signature + "</requestSignature>" +
    "<requestTimestamp>" + requestTimestamp + "</requestTimestamp>"
    "</ns1:AuthenticationHeader>";
```

## Cabeçalho da solicitação

| Nome do campo | Obrigatório/Opcional | Descrição |
|--- |--- |--- |
| mktowsUserId | Obrigatório | A ID de acesso do cliente do Marketo é encontrada no painel da API SOAP de administrador do Marketo, em Integração. |
| requestSignature | Obrigatório | Assinatura HMAC-SHA1 com base na chave de segredo compartilhado, requestTimestamp e ID de usuário do Marketo |
| requestTimestamp | Obrigatório | Solicitar carimbo de data e hora (formato de data WSDL W3C Ex. &quot;06/2013/09T14:04:54-08:00&quot;) |
| partnerId | Opcional | Chave da API do parceiro de tecnologia do LaunchPoint. |

## Solicitar XML de getLeadActivity

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mkt="http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>mktodemoaccount881_536240405411DF5316D5C9</mktowsUserId>
         <requestSignature>3f4b21eb586063dc65774a2733713cac342e9c81</requestSignature>
         <requestTimestamp>2017-03-09T17:40:00-08:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsGetLeadActivity>
         <leadKey>
            <keyType>IDNUM</keyType>
            <keyValue>318815</keyValue>
         </leadKey>
         <activityFilter>
               <includeTypes>
                   <activityType>NewLead</activityType>
               </includeTypes>
         </activityFilter>
        </mkt:paramsGetLeadActivity>
   </soapenv:Body>
</soapenv:Envelope>
```

## Êxito no XML de resposta

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successGetLeadActivity>
         <leadActivityList>
            <returnCount>1</returnCount>
            <remainingCount>0</remainingCount>
            <newStartPosition>
               <latestCreatedAt xsi:nil="true"/>
               <oldestCreatedAt xsi:nil="true"/>
               <activityCreatedAt xsi:nil="true"/>
               <offset>1</offset>
            </newStartPosition>
            <activityRecordList>
               <activityRecord>
                  <id>12025714</id>
                  <activityDateTime>2015-09-04T16:07:16-05:00</activityDateTime>
                  <activityType>New Lead</activityType>
                  <mktgAssetName/>
                  <activityAttributes>
                     <attribute>
                        <attrName>Source Type</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>Web service API</attrValue>
                     </attribute>
                     <attribute>
                        <attrName>Source Info</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>Web service API</attrValue>
                     </attribute>
                     <attribute>
                        <attrName>Created Date</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>2015-09-04</attrValue>
                     </attribute>
                     <attribute>
                        <attrName>Lead ID</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>318815</attrValue>
                     </attribute>
                  </activityAttributes>
                  <campaign/>
                  <personName xsi:nil="true"/>
                  <mktPersonId>318815</mktPersonId>
                  <foreignSysId xsi:nil="true"/>
                  <orgName xsi:nil="true"/>
                  <foreignSysOrgId xsi:nil="true"/>
               </activityRecord>
            </activityRecordList>
         </leadActivityList>
      </ns1:successGetLeadActivity>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope
```

## XML de Resposta - Falha (Credenciais Inválidas)

```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
     <SOAP-ENV:Body>
       <SOAP-ENV:Fault>
         <faultcode>SOAP-ENV:Client</faultcode>
         <faultstring>20014 - Authentication failed</faultstring>
         <detail>
           <ns1:serviceException xmlns:ns1="http://www.marketo.com/mktows/">
             <name>mktServiceException</name>
             <message>Authentication failed (20014)</message>
             <code>20014</code>
           </ns1:serviceException>
         </detail>
       </SOAP-ENV:Fault>
     </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
```

## Código de exemplo PHP

```php
<?php
 
  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME 
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";
 
  // Create Signature
  $dtzObj = new DateTimeZone("America/Los_Angeles");
  $dtObj  = new DateTime('now', $dtzObj);
  $timeStamp = $dtObj-&gt;format(DATE_W3C);
  $encryptString = $timeStamp . $marketoUserId;
  $signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);
 
  // Create SOAP Header
  $attrs = new stdClass();
  $attrs-&gt;mktowsUserId = $marketoUserId;
  $attrs-&gt;requestSignature = $signature;
  $attrs-&gt;requestTimestamp = $timeStamp;
  $authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
 
  print_r($authHdr)
 
?>
```

## Código de amostra Java

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
import javax.xml.bind.Marshaller;
 
public class AuthenticationHeader {
 
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
 
            JAXBContext context = JAXBContext.newInstance(AuthenticationHeader.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(header, System.out);
 
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Código de exemplo Ruby

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
    'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,                     
    "requestTimestamp"  => requestTimestamp 
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')
```
