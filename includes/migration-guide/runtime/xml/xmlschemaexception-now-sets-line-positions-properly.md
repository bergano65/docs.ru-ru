---
ms.openlocfilehash: a5b3e325c13d2f56532ebc6ebb5c259d565a4952
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66379602"
---
### <a name="xmlschemaexception-now-sets-line-positions-properly"></a>Свойство XmlSchemaException теперь правильно задает позиции строк

|   |   |
|---|---|
|Подробные сведения|Если значение <xref:System.Xml.Linq.LoadOptions.SetLineInfo> передается методу Load и возникает ошибка проверки, свойства <xref:System.Xml.Schema.XmlSchemaException.LineNumber> и <xref:System.Xml.Schema.XmlSchemaException.LinePosition> теперь содержат сведения о строке.|
|Предложение|Код обработки исключений, который предполагает, что свойства <xref:System.Xml.Schema.XmlSchemaException.LineNumber> и <xref:System.Xml.Schema.XmlSchemaException.LinePosition> не будут задаваться, необходимо обновить, поскольку эти свойства теперь задаются правильно при использовании SetLineInfo во время загрузки XML.|
|Область|Пограничный случай|
|Версия|4.5|
|Тип|Среда выполнения|
|Затронутые API|<ul><li><xref:System.Xml.Linq.LoadOptions.SetLineInfo?displayProperty=nameWithType></li></ul>|
