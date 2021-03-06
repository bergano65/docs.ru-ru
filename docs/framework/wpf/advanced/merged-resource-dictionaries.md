---
title: Объединенные словари ресурсов
ms.date: 03/30/2017
helpviewer_keywords:
- merged resource dictionaries [WPF]
- dictionaries [WPF], merged resources
ms.assetid: d159531f-05d4-49fd-b951-c332de51e5bc
ms.openlocfilehash: 899955a9f3302e67de4efa0ce0cb6baf6bf0ec5d
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559785"
---
# <a name="merged-resource-dictionaries"></a>Объединенные словари ресурсов
Ресурсы [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] поддерживают функцию объединенных словарей ресурсов. Эта функция обеспечивает способ определения части ресурсов приложения [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] за пределами скомпилированного приложения [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]. Затем ресурсы можно совместно использовать в приложениях; они также более удобно изолируются для локализации.  
  
## <a name="introducing-a-merged-resource-dictionary"></a>Общие сведения об объединенном словаре ресурсов  
 Для представления на странице объединенного словаря ресурсов используйте в разметке следующий синтаксис.  
  
 [!code-xaml[ResourceMergeDictionary#MergedXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourceMergeDictionary/CS/default.xaml#mergedxaml)]  
  
 Обратите внимание, что элемент <xref:System.Windows.ResourceDictionary> не имеет [директивы x:Key](../../../desktop-wpf/xaml-services/xkey-directive.md), которая обычно требуется для всех элементов в коллекции ресурсов. Но еще один <xref:System.Windows.ResourceDictionary> ссылка в коллекции <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> является особым случаем, зарезервированным для этого сценария объединенного словаря ресурсов. <xref:System.Windows.ResourceDictionary>, представляющая Объединенный словарь ресурсов, не может иметь [директиву x:Key](../../../desktop-wpf/xaml-services/xkey-directive.md). Как правило, каждый <xref:System.Windows.ResourceDictionary> в коллекции <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> указывает атрибут <xref:System.Windows.ResourceDictionary.Source%2A>. Значение <xref:System.Windows.ResourceDictionary.Source%2A> должно быть универсальным кодом ресурса (URI), который разрешается в расположение объединяемого файла ресурсов. Назначение этого URI должно быть другим [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] файлом с <xref:System.Windows.ResourceDictionary> в качестве корневого элемента.  
  
> [!NOTE]
> Можно определить ресурсы в <xref:System.Windows.ResourceDictionary>, который указан в виде объединенного словаря, либо в качестве альтернативы указанию <xref:System.Windows.ResourceDictionary.Source%2A>, либо в дополнение к любым ресурсам, включенным в указанный источник. Однако это не очень распространенный сценарий; основным сценарием для объединенных словарей является объединение ресурсов из внешних файлов. Если вы хотите указать ресурсы в разметке для страницы, обычно их следует определить в основном <xref:System.Windows.ResourceDictionary>, а не в Объединенных словарях.  
  
## <a name="merged-dictionary-behavior"></a>Поведение объединенного словаря  
 Ресурсы в объединенном словаре занимают место в области поиска ресурсов сразу после области главного словаря ресурсов, в который они будут включены. Хотя ключ ресурса должен быть уникальным в пределах каждого отдельного словаря, один и тот же ключ может встречаться несколько раз в наборе объединенных словарей. В этом случае возвращаемый ресурс будет поступать из последнего словаря, последовательно найденного в коллекции <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A>. Если коллекция <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> была определена в [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], то порядок объединенных словарей в коллекции является порядком элементов, указанных в разметке. Если ключ определен в главном словаре, а также в словаре, который был объединен, возвращаемый ресурс поступит из основного словаря. Эти правила поиска применяются одинаково для ссылок как на статические, так и на динамические ресурсы.  
  
### <a name="merged-dictionaries-and-code"></a>Объединенные словари и код  
 Объединенные словари могут быть добавлены в словарь `Resources` с помощью кода. По умолчанию изначально пустая <xref:System.Windows.ResourceDictionary>, существующая для любого свойства `Resources`, также имеет значение по умолчанию, изначально пустое свойство коллекции <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A>. Чтобы добавить Объединенный словарь с помощью кода, можно получить ссылку на требуемый первичный <xref:System.Windows.ResourceDictionary>, получить его значение свойства <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A> и вызвать `Add` для универсального `Collection`, содержащегося в <xref:System.Windows.ResourceDictionary.MergedDictionaries%2A>. Добавляемый объект должен быть новым <xref:System.Windows.ResourceDictionary>. В коде не задается свойство <xref:System.Windows.ResourceDictionary.Source%2A>. Вместо этого необходимо получить объект <xref:System.Windows.ResourceDictionary> путем его создания или загрузки. Один из способов загрузить существующий <xref:System.Windows.ResourceDictionary>, чтобы вызвать <xref:System.Windows.Markup.XamlReader.Load%2A?displayProperty=nameWithType> в существующем [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] файловом потоке, который имеет <xref:System.Windows.ResourceDictionary> корень, а затем присвоить <xref:System.Windows.Markup.XamlReader.Load%2A?displayProperty=nameWithType> возвращаемое значение <xref:System.Windows.ResourceDictionary>.  
  
### <a name="merged-resource-dictionary-uris"></a>URI объединенных словарей ресурсов  
 Существует несколько способов включения объединенного словаря ресурсов, который указывается в формате универсального кода ресурса (URI), который будет использоваться. Вообще говоря, эти методы можно разделить на две категории: ресурсы, которые компилируются как часть проекта, и ресурсы, которые не компилируются как часть проекта.  
  
 Для ресурсов, которые компилируются как часть проекта, можно использовать относительный путь, ссылающийся на расположение ресурса. Относительный путь вычисляется во время компиляции. Ресурс должен быть определен как часть проекта в качестве действия сборки ресурса. Если XAML-файл ресурса включен в проект в качестве ресурса, то не нужно копировать этот файл ресурса в выходной каталог, так как ресурс уже включен в скомпилированное приложение. Можно также использовать действие сборки содержимого, но затем будет необходимо скопировать файлы в выходной каталог и развернуть эти файлы ресурсов по тому же пути, связанному с исполняемым файлом.  
  
> [!NOTE]
> Не используйте действие сборки внедренного ресурса. Само действие сборки поддерживается для [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] приложений, но разрешение <xref:System.Windows.ResourceDictionary.Source%2A> не включает <xref:System.Resources.ResourceManager>, и, таким способом, не может разделить отдельные ресурсы из потока. Вы по-прежнему можете использовать внедренный ресурс для других целей, если вы также используете <xref:System.Resources.ResourceManager> для доступа к ресурсам.  
  
 Близким методом являются использование URI типа pack в файле [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] и ссылка на него в качестве источника. URI типа pack позволяет ссылки на компоненты связанных сборок и другие методы. Дополнительные сведения об URI типа pack см. в разделе [Ресурсы, контент и файлы данных приложения WPF](../app-development/wpf-application-resource-content-and-data-files.md).  
  
 Для ресурсов, которые не компилируются как часть проекта, URI вычисляется во время выполнения. Для ссылки на файл ресурсов можно использовать общий транспорт URI, такой как file: или http:. Недостаток подхода с использованием некомпилированного ресурса заключается в том, что для доступа file: требуются дополнительные действия по развертыванию, а доступ http: подразумевает зону безопасности Интернета.  
  
### <a name="reusing-merged-dictionaries"></a>Многократное использование объединенных словарей  
 Можно повторно использовать объединенные словари ресурсов или обмениваться ими между приложениями, так как к словарю ресурсов для слияния можно обращаться через любой допустимый универсальный код ресурса (URI). Точный способ сделать это будет зависеть от стратегии развертывания приложения и используемой модели приложения. Вышеупомянутая стратегия URI типа pack обеспечивает способ совместного создания объединенного ресурса между несколькими проектами в процессе разработки путем совместного использования ссылки на сборку. В этом сценарии ресурсы по-прежнему распространяются клиентом и по крайней мере одно из приложений должно разворачивать связанную сборку. Можно также ссылаться на объединенные ресурсы с помощью распределенного URI, который использует протокол http.  
  
 Запись объединенных словарей как локальных файлов приложения или в локальное общее хранилище является другим возможным сценарием объединенного словаря или развертывания приложения.  
  
### <a name="localization"></a>Локализация  
 Если ресурсы, которые необходимо локализовать, изолированы от словарей, объединенных в основные словари, и хранятся как свободные [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], эти файлы можно локализовать отдельно. Этот способ является упрощенной альтернативой для локализации вспомогательных сборок ресурсов. Дополнительные сведения см. в статье [Общие сведения о глобализации и локализации WPF](wpf-globalization-and-localization-overview.md).  
  
## <a name="see-also"></a>См. также:

- <xref:System.Windows.ResourceDictionary>
- [Ресурсы XAML](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [Ресурсы и код](resources-and-code.md)
- [Файлы ресурсов, содержимого и данных WPF-приложения](../app-development/wpf-application-resource-content-and-data-files.md)
