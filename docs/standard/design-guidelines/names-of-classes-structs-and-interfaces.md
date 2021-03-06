---
title: Имена классов, структур и интерфейсов
ms.date: 10/22/2008
helpviewer_keywords:
- type names, guidelines
- classes [.NET Framework], names
- enumerations [.NET Framework], names
- names [.NET Framework], interfaces
- common type names
- names [.NET Framework], type names
- names [.NET Framework], classes
- interfaces [.NET Framework], names
- generic type parameters
ms.assetid: 87a4b0da-ed64-43b1-ac43-968576c444ce
ms.openlocfilehash: 2c528348c0e84037a80df9797c56f03b51c73adc
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727786"
---
# <a name="names-of-classes-structs-and-interfaces"></a>Имена классов, структур и интерфейсов
Приведенные ниже рекомендации по именованию применяются к общему именованию типов.

 ✔️ имена классов и структур с существительными или субстантивные словосочетаниями с помощью Паскалкасинг.

 Это отличает имена типов от методов, названных с помощью фраз глагола.

 ✔️ ИСПОЛЬЗОВАТЬ именованные интерфейсы с фразами прилагательных или иногда с существительными или фразами с существительными.

 Существительные и фразы существительное следует использовать редко, и они могут указывать, что тип должен быть абстрактным классом, а не интерфейсом.

 ❌ не присваивайте имена классов префиксом (например, "C").

 ✔️ Рекомендуется завершать имена производных классов именем базового класса.

 Это очень понятное и понятное отношение. Вот некоторые примеры кода: `ArgumentOutOfRangeException`, который является разновидностью `Exception`, и `SerializableAttribute`, который является разновидностью `Attribute`. Однако важно использовать разумные меры при применении этой рекомендации; Например, класс `Button` является типом события `Control`, хотя в его имени `Control` не отображается.

 ✔️ префиксы имен интерфейсов с буквой I, чтобы указать, что тип является интерфейсом.

 Например, `IComponent` (описательное существительное), `ICustomAttributeProvider` (субстантивные словосочетания) и `IPersistable` (прилагательные) — это соответствующие имена интерфейсов. Как и в случае с другими именами типов, следует избегать сокращений.

 ✔️ Убедитесь, что имена отличаются только префиксом "I" в имени интерфейса при определении пары "класс — интерфейс", в которой класс является стандартной реализацией интерфейса.

## <a name="names-of-generic-type-parameters"></a>Имена параметров универсального типа
 Универсальные шаблоны были добавлены в .NET Framework 2,0. В этой функции появился новый тип идентификатора, называемый *параметром типа*.

 ✔️ имена параметров универсального типа с описательными именами, если только имя в одном письме не является полностью понятным, а описательное имя не будет добавлять значение.

 ✔️ РЕКОМЕНДУЕТСЯ использовать `T` в качестве имени параметра типа для типов с одним однобуквенным параметром типа.

```csharp
public int IComparer<T> { ... }
public delegate bool Predicate<T>(T item);
public struct Nullable<T> where T:struct { ... }
```

 ✔️ добавлять в префикс имена параметров описательного типа с `T`.

```csharp
public interface ISessionChannel<TSession> where TSession : ISession {
    TSession Session { get; }
}
```

 ✔️ Рассмотрите возможность указания ограничений, накладываемых на параметр типа в имени параметра.

 Например, параметр, ограниченный `ISession`, может называться `TSession`.

## <a name="names-of-common-types"></a>Имена общих типов
 ✔️ следовать рекомендациям, описанным в следующей таблице, при именовании типов, производных от или реализующих определенные типы .NET Framework.

|Базовый тип|Руководство по производному или реализующему типу|
|---------------|------------------------------------------|
|`System.Attribute`|✔️ Добавить суффикс "Attribute" к именам классов настраиваемых атрибутов.|
|`System.Delegate`|✔️ Добавить суффикс EventHandler к именам делегатов, используемых в событиях.<br /><br /> ✔️ Добавить суффикс "Callback" к именам делегатов, отличных от используемых в качестве обработчиков событий.<br /><br /> ❌ не добавляйте суффикс "Delegate" в делегат.|
|`System.EventArgs`|✔️ Добавить суффикс EventArgs.|
|`System.Enum`|❌ не являются производными от этого класса; Вместо этого используйте ключевое слово, поддерживаемое языком. Например, в C#используйте ключевое слово `enum`.<br /><br /> ❌ не добавлять суффикс "enum" или "Flag".|
|`System.Exception`|✔️ Добавить суффикс "Exception".|
|`IDictionary` <br /> `IDictionary<TKey,TValue>`|✔️ Добавить суффикс "Dictionary". Обратите внимание, что `IDictionary` является коллекцией определенного типа, но это правило имеет приоритет над более общими коллекциями, приведенными ниже.|
|`IEnumerable` <br /> `ICollection` <br /> `IList` <br /> `IEnumerable<T>` <br /> `ICollection<T>` <br /> `IList<T>`|✔️ Добавить суффикс "Collection".|
|`System.IO.Stream`|✔️ Добавить суффикс "Stream".|
|`CodeAccessPermission IPermission`|✔️ Добавить суффикс "Permission".|

## <a name="naming-enumerations"></a>Перечисления именования
 Имена типов перечисления (также называемые перечислениями) в целом должны соответствовать стандартным правилам именования типов (Паскалкасинг и т. д.). Однако существуют дополнительные рекомендации, которые применяются специально для перечислений.

 ✔️ использовать единственное имя типа для перечисления, если его значения не являются битовыми полями.

 ✔️ использовать имя типа во множественном числе для перечисления с битовыми полями в качестве значений, также называемых flags Enum.

 ❌ не используйте суффикс Enum в именах типов Enum.

 ❌ не используйте суффиксы "Flag" или "Flags" в именах типов Enum.

 ❌ не использовать префикс в именах значений перечисления (например, "ad" для перечислений ADO, "RTF" для перечислений форматированного текста и т. д.).

 *Части © 2005, 2009 Корпорация Майкрософт. Все права защищены.*

 *Перепечатано с разрешения Pearson Education, Inc. из книги [Инфраструктура программных проектов. Соглашения, идиомы и шаблоны для многократно используемых библиотек .NET (2-е издание)](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619), авторы: Кржиштоф Цвалина (Krzysztof Cwalina) и Брэд Абрамс (Brad Abrams). Книга опубликована 22 октября 2008 г. издательством Addison-Wesley Professional в рамках серии, посвященной разработке для Microsoft Windows.*

## <a name="see-also"></a>См. также раздел

- [Рекомендации по проектированию на основе Framework](../../../docs/standard/design-guidelines/index.md)
- [Правила именования](../../../docs/standard/design-guidelines/naming-guidelines.md)
