---
title: Modo de Acesso de Segurança Avançada
feature: Mobile Marketing
description: Saiba mais sobre o Modo de acesso de segurança avançado para Marketo Mobile SDK, com geração de assinatura HMAC, configuração de ponto de extremidade de servidor, uso de ID de dispositivo e exemplos de iOS e Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
TQID: https://experienceleague.adobe.com/F6lH1aGbCakK-E6IU4wLwYw58BG2-CRE-Ras2bMHeO8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 1%

---

# Modo de Acesso de Segurança Avançada

O Modo de acesso de segurança avançado exige que o Marketo SDK recupere e defina uma assinatura de segurança. O SDK fornece métodos para definir e remover a assinatura, além de um método de utilitário para recuperar a ID do dispositivo.

Ao fazer logon, envie a ID do dispositivo e o endereço de email para o servidor do cliente para calcular a assinatura de segurança. Em seguida, o SDK chama o endpoint do cliente para recuperar os campos necessários para instanciar o objeto de assinatura. Se o Modo de acesso de segurança estiver ativado no Marketo Mobile Admin, você deve definir essa assinatura no SDK.

## Configuração do Modo de Acesso Seguro

Implemente esta configuração antes de ativar o modo de Acesso seguro na página Administrador do Marketo > Aplicativos e dispositivos móveis.

O modo de acesso seguro requer um algoritmo de assinatura do lado do servidor e um terminal de cliente. O endpoint retorna os seguintes valores:

- Chave de acesso
- Assinatura calculada
- Carimbo de data/hora de expiração
- Endereço de e-mail

O algoritmo usa a chave de acesso do usuário, o segredo de acesso, o endereço de email, o carimbo de data e hora e a ID do dispositivo. O cliente deve configurar o endpoint, implementar o cálculo de assinatura e manter atualizado o carimbo de data e hora de expiração.

```python
import argparse
import datetime
import hashlib
import hmac


ACCESS_KEY = 'Your Access Key'
ACCESS_SECRET = 'Your access secret'

# Key should not be unicode
def get_signing_key(timestamp):
    return 'MKTO' + ACCESS_SECRET + str(timestamp)

def get_string_to_sign(email, uuid):
    return email + uuid

def get_hmac(key, string_to_sign):
    return hmac.new(key, string_to_sign.encode('utf-8'), hashlib.sha256).hexdigest()

def get_epoch_plus_day():
    epoch = datetime.datetime.utcfromtimestamp(0)
    valid_until_dt = datetime.datetime.utcnow() + datetime.timedelta(days=1)
    return long((valid_until_dt - epoch).total_seconds())

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--email", required=True, help="email address")
    parser.add_argument("-u", "--uuid", required=True, help="Device install id")
    parser.add_argument("-t", "--timestamp", type=int, help="Valid until timestamp")
    args = parser.parse_args()
    string_to_sign = get_string_to_sign(args.email, args.uuid)
    if not args.timestamp:
        valid_until = get_epoch_plus_day()
    else:
        valid_until = args.timestamp
    signing_key = get_signing_key(valid_until)
    hmac_string = get_hmac(signing_key, string_to_sign)
    print 'HMAC is ', hmac_string
```

Use os métodos específicos da plataforma para definir ou remover a assinatura de segurança e recuperar a ID do dispositivo.

### iOS

```objectivec
Marketo * sharedInstance =[Marketo sharedInstance];

// set secure signature
MKTSecuritySignature *signature =
[[MKTSecuritySignature alloc] initWithAccessKey:<ACCESS_KEY> signature:<SIGNATURE_TOKEN> timestamp:<EXPIRY_TIMESTAMP> email:<EMAIL>];
[sharedInstance setSecureSignature:signature];

// remove signature
[sharedInstance removeSecureSignature];

// get device id
[sharedInstance getDeviceId];
```

```swift
let sharedInstance = Marketo.sharedInstance()

 // set secure signature
let signature = MKTSecuritySignature(accessKey: <ACCESS_KEY>, signature: <SIGNATURE_TOKEN> , timestamp: <EXPIRY_TIMESTAMP>, email: <EMAIL>)
sharedInstance.setSecureSignature(signature)

// remove signature
[sharedInstance removeSecureSignature];

// get device id
sharedInstance.getDeviceId()
```

### Android

```java
Marketo sdk = Marketo.getInstance(getApplicationContext());

// set signature
MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
secureMode.setAccessKey(<ACCESS_KEY>);
secureMode.setEmail(<EMAIL_ADDRESS>);
secureMode.setSignature(<SIGNATURE_TOKEN>);
secureMode.setTimestamp(<EXPIRY_DATE>);
if (secureMode.isValid()) {
  sdk.setSecureSignature(secureMode);
}

// remove signature
sdk.removeSecureSignature();

// get device id
sdk.getDeviceId();
```
