---
title: Referência da API do Munchkin
description: Use a API JavaScript do Munchkin para rastrear visitas de página, cliques em links e eventos personalizados com os métodos init, createTrackingCookie e munchkinFunction.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
TQID: https://experienceleague.adobe.com/s97x6wVZijnnxZwS7HMIkQAKlxXkcfPXuSZG4KjXGoc
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 414
ht-degree: 9%

---

# Referência da API do Munchkin

O Munchkin fornece funções do JavaScript para o rastreamento personalizado de eventos do navegador. Por exemplo, é possível rastrear reproduções de vídeos ou cliques em elementos que não são links.

## Funções

A API do Munchkin inclui as seguintes funções:

- `init`
- `createTrackingCookie`
- `munchkinFunction`

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` deve ser chamado antes de qualquer outra função. Ele configura o Munchkin na página atual para enviar atividades para uma instância específica e gera uma atividade &quot;Visitas da página da Web&quot; para a página atual.

| Nome do parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| ID do Munchkin | Obrigatório | String | ID de conta do Munchkin encontrada no menu Admin > Integração > Munchkin. Define a instância de destino para a qual enviar atividades. |
| [Configurações](configuration.md) | Opcional | Objeto | Ativa configurações de comportamento alternativas para o Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

`Munchkin.createTrackingCookie()` verifica se há um cookie `_mkto_trk` no navegador. Se o cookie não existir, a função criará um.

Quando `cookieAnon` é definido como falso, use essa função para rastrear usuários durante ações específicas, como registrar ou baixar um ativo.

| Nome do parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| forceCreate | Obrigatório | Booleano | Criar cookie mesmo se `cookieAnon` estiver definido como falso. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Use `Munchkin.munchkinFunction()` para criar comportamentos de rastreamento personalizados. Por exemplo, rastreie a atividade do reprodutor de vídeo ou as visitas da página de navegação fora do padrão, como alterações de hash.

| Nome do parâmetro | Opcional/Obrigatório | Tipo | Descrição |
| --- | --- | --- | --- |
| Tipo de função | Obrigatório | String | Determina a atividade a ser registrada. Valores permitidos: `visitWebPage`, `clickLink`, `associateLead` |
| Dados | Obrigatório | Objeto | Contém dados para a atividade a ser registrada. |

#### visitWebPage

Chamar `munchkinFunction()` com `visitWebPage` envia uma atividade de &quot;visita&quot; do usuário atual para a Marketo. Use o objeto de dados no segundo argumento para personalizar a URL e `querystring`.

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

Chamar `munchkinFunction()` com `clickLink` envia uma atividade de clique do usuário atual para a Marketo. Use a propriedade `href` no objeto de dados para personalizar a URL de clique.

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
