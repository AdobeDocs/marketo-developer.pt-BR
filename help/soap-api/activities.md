---
title: Atividades
feature: SOAP
description: Saiba como interagir com Atividades usando o SOAP, recuperar atividades de cliente potencial e rastrear alterações de cliente potencial com getLeadActivities e getLeadChanges
exl-id: fd695ab6-e7be-4ced-89c9-c4cd2d4c2ab0
TQID: https://experienceleague.adobe.com/6zUkvoDCqlRmblFDPWzLjdwITsyWxcXrJBKbLux76WI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: 79
ht-degree: 2%

---

# Atividades

As chamadas de SOAP a seguir podem ser usadas para interagir com Atividades.

- [getLeadActivities](getleadactivity.md)
- [getLeadChanges](getleadchanges.md)

>[!CAUTION]
>
>A partir de 30/12/2026, as chamadas para os pontos de extremidade `Get Lead Activities` e `Get Lead Changes` que incluem o parâmetro `listId` falharão (código de erro 1003) se as listas de destino contiverem 10.000 ou mais clientes potenciais. Para evitar interrupções do serviço, verifique se as chamadas têm o escopo correto para evitar esse limite. Consulte o [Guia de migração](migration.md).
