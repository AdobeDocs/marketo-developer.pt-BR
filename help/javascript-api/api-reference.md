---
title: Referência da API do Munchkin
description: Use a API Javascript do Munchkin para personalizar seus dados do Munchkin.
feature: Javascript
source-git-commit: c6c0a492ede415471e10efb6213eb3f590e63ebe
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 9%

---


# Referência da API do Munchkin

O Munchkin fornece várias funções que podem ser chamadas manualmente através do Javascript. Isso pode permitir o rastreamento personalizado de eventos do navegador, como reproduções de vídeo ou cliques em não links.

## Funções

A API Munchkin é composta das seguintes funções: `init`, `createTrackingCookie`, `munchkinFunction`.

### Munchkin.init()

`Munchkin.init()` deve ser chamado antes de qualquer outra função. Ele configura o Munchkin na página atual para enviar atividades para uma instância específica e gera uma atividade &quot;Visitas da página da Web&quot; para a página atual.

| Nome do parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| ID do Munchkin | Obrigatório | String | ID da conta do Munchkin encontrada no menu Admin > Integração > Munchkin. Define a instância de destino para a qual enviar atividades. |
| [Configurações](configuration.md) | Opcional | Objeto | Habilita configurações de comportamento alternativas para o Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

Quando chamada, esta opção verifica se existe um cookie `_mkto_trk` no navegador e, caso contrário, cria um. Isso é útil para rastrear usuários durante ações específicas, como registro ou download de um ativo, se `cookieAnon` estiver definido como falso.

| Nome do parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| forceCreate | Obrigatório | Booleano | Criar cookie mesmo se `cookieAnon` estiver definido como falso. |


```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Usado para gerar comportamentos de rastreamento personalizados, como reproduzir e pausar o reprodutor de vídeo, ou visitas de página para navegação fora do padrão, como códigos hash.

| Nome do parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| Tipo de função | Obrigatório | String | Determina a atividade a ser registrada. Valores permitidos: `visitWebPage`, `clickLink`, `associateLead` |
| Dados | Obrigatório | Objeto | Contém dados para a atividade a ser registrada. |

#### visitWebPage

Chamar `munchkinFunction()` com `visitWebPage` envia uma atividade de &quot;visita&quot; do usuário atual para a Marketo. Você pode personalizar a URL e `querystring`, que são enviados com o objeto de dados no segundo argumento.

| Nome da propriedade de dados | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| url | Obrigatório | String | O caminho de arquivo do URL usado para registrar uma visita de página.  Esse valor é anexado ao nome de domínio atual para criar um nome de página completo. Por exemplo, se a url for `/index.html` e o nome de domínio for `www.example.com`, a página visitada será registrada como `www.example.com/index.html`. |
| params | Opcional | String | Uma sequência de consulta dos parâmetros desejados a serem registrados. |

Por exemplo, `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

Chamar `munchkinFunction()` com `clickLink` envia uma atividade de clique do usuário atual para a Marketo. Você pode personalizar a URL de clique com a propriedade `href` no objeto de dados.

| Nome da propriedade de dados | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| href | Obrigatório | String | O caminho de arquivo do URL usado para registrar um clique de link. Esse valor é anexado ao nome de domínio atual para criar o link completo. |

Por exemplo, se href for `/index.html` e o nome de domínio for `www.example.com`, o clique no link será registrado como `www.example.com/index.html`.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associateLead

Este método foi substituído e não está mais disponível para uso.
