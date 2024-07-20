---
title: Modo de Acesso de Segurança Avançada
feature: Mobile Marketing
description: Detalhes sobre o Modo de Acesso de Segurança Avançada
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Modo de Acesso de Segurança Avançada

O SDK do Marketo expõe os métodos para definir e remover a assinatura de segurança. Também há um método de utilitário para recuperar a ID do dispositivo. A ID do dispositivo deve ser passada juntamente com o email, após o logon, para o servidor do cliente para uso no cálculo da assinatura de segurança. O SDK deve atualizar o novo endpoint de ocorrência, apontando para o algoritmo listado acima, para recuperar os campos necessários para instanciar o objeto de assinatura. Definir essa assinatura no SDK é uma etapa necessária se o Modo de acesso de segurança tiver sido habilitado no Marketo Mobile Admin.

## Configuração do Modo de Acesso Seguro

Esta configuração deve ser implementada antes que o modo de Acesso seguro seja ativado por meio da página Admin. do Marketo > Aplicativos e dispositivos móveis. As etapas a seguir descrevem o processo necessário para concluir o processo de validação de segurança:

O modo de acesso seguro exige a implementação do algoritmo de assinatura no lado do servidor do cliente, que fornecerá um terminal para recuperar a chave de acesso, a assinatura calculada, o carimbo de data e hora de expiração e o email. Esse algoritmo requer a chave de acesso do usuário, o segredo de acesso, o email, o carimbo de data e hora e a id do dispositivo para executar o cálculo. O cliente é responsável pela configuração do endpoint, implementação do algoritmo para executar cálculos de assinatura e também manter atualizado o carimbo de data e hora de expiração.

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

O SDK do Marketo expõe novos métodos para definir e remover a assinatura de segurança. Também há um método de utilitário para recuperar a ID do dispositivo. A ID do dispositivo deve ser passada juntamente com o email, após o logon, para o servidor do cliente para uso no cálculo da assinatura de segurança. O SDK deve atualizar o novo endpoint de ocorrência, apontando para o algoritmo listado acima, para recuperar os campos necessários para instanciar o objeto de assinatura. Definir essa assinatura no SDK é uma etapa necessária se o Modo de acesso de segurança tiver sido habilitado no Marketo Mobile Admin.

### iOS

```
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

```
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

```
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
