---
title: Istruzioni online
permalink: index.html
layout: home
ms.openlocfilehash: 1dde36744b9541205d719973757171e13ec37223
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2021
ms.locfileid: "132625521"
---
# <a name="content-directory"></a>Directory contenuto

I file necessari per il lab possono essere [SCARICATI QUI](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

In basso sono elencati i collegamenti ipertestuali a tutti gli esercizi lab.

## <a name="labs"></a>Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modulo | Lab |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}


