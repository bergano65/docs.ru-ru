---
ms.openlocfilehash: d3c6818861f8b0261a9a71a4654029143d928d08
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67856953"
---
### <a name="allow-unicode-in-uris-that-resemble-unc-shares"></a>Разрешение Юникода в URI, которые напоминают общие папки UNC

|   |   |
|---|---|
|Подробные сведения|В <xref:System.Uri?displayProperty=fullName> формирование URI файла, содержащего UNC-имя общей папки и символы Юникода, больше не приводит к созданию URI с недопустимым внутренним состоянием. Это поведение изменится, только если выполняются все указанные ниже условия:<ul><li>URI имеет схему <code>file:</code> и за ним следуют четыре или более косых черт;</li><li>имя узла начинается с символа подчеркивания или другого незарезервированного символа;</li><li>URI содержит символы Юникода.</li></ul>|
|Предложение|Приложения, работающие с URI, в обязательном порядке содержащими символы Юникода, потенциально могли использовать это поведение для запрета ссылок на общие папки UNC. Эти приложения должны использовать <xref:System.Uri.IsUnc>.|
|Область|Пограничный случай|
|Версия|4.7.2|
|Тип|Среда выполнения|
|Затронутые API|<ul><li><xref:System.Uri?displayProperty=nameWithType></li></ul>|

