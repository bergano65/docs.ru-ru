---
ms.openlocfilehash: de40d16dbb5e7a7a49ae0988342b3eb75bc078c5
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67804438"
---
### <a name="currentculture-and-currentuiculture-flow-across-tasks"></a>Поток CurrentCulture и CurrentUICulture между задачами

|   |   |
|---|---|
|Подробные сведения|Начиная с .NET Framework 4.6 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> и <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> хранятся в <xref:System.Threading.ExecutionContext?displayProperty=name> потока, который проходит через асинхронные операции. Это означает, что изменения <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> или <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> будут отражены в задачах, которые позже выполняются асинхронно. Это отличается от поведения предыдущих версий .NET Framework (которые во всех асинхронных задачах сбрасывали <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> и <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name>).|
|Предложение|Приложения, которых коснулось это изменение, могут обойти его, явно задав <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> или <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> в качестве первой операции в асинхронной задаче. Кроме того, можно выбрать прежнее поведение (без потока <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name>) путем установки следующего переключателя совместимости:<pre><code class="lang-csharp">AppContext.SetSwitch(&quot;Switch.System.Globalization.NoAsyncCurrentCulture&quot;, true);&#13;&#10;</code></pre>Эта проблема была устранена с помощью WPF в .NET Framework 4.6.2. Она также была устранена в .NET Framework версий 4.6 и 4.6.1 посредством [KB 3139549](https://support.microsoft.com/kb/3139549). Приложения, предназначенные для .NET Framework 4.6 или более поздней версии, автоматически получают правильное поведение в приложениях WPF — <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name>) будет сохранено в операциях диспетчера.|
|Область|Дополнительный номер|
|Версия|4.6|
|Тип|Изменение целевой платформы|
|Затронутые API|<ul><li><xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType></li><li><xref:System.Threading.Thread.CurrentCulture?displayProperty=nameWithType></li><li><xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=nameWithType></li><li><xref:System.Threading.Thread.CurrentUICulture?displayProperty=nameWithType></li></ul>|

