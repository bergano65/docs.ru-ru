---
title: Сообщения protobuf — gRPC для разработчиков WCF
description: Узнайте, как сообщения protobuf определяются в IDL и создаются в C#.
ms.date: 09/09/2019
ms.openlocfilehash: c7375bafb7572b0eaa0458b0310a0114e3fd078c
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543044"
---
# <a name="protobuf-messages"></a>Сообщения Protobuf

В этом разделе описано, как объявлять сообщения в буфере протокола (protobuf) в файлах `.proto`. В нем объясняются фундаментальные понятия чисел и типов полей, а также рассматривается C# код, создаваемый компилятором `protoc`. 

Оставшаяся часть главы посвящена более подробному рассмотрению того, как различные типы данных представлены в protobuf.

## <a name="declaring-a-message"></a>Объявление сообщения

В Windows Communication Foundation (WCF) класс `Stock` для торгового рынка с торговым рынком может быть определен как в следующем примере:

```csharp
namespace TraderSys
{
    [DataContract]
    public class Stock
    {
        [DataMember]
        public int Id { get; set;}
        [DataMember]
        public string Symbol { get; set;}
        [DataMember]
        public string DisplayName { get; set;}
        [DataMember]
        public int MarketId { get; set; }
    }
}
```

Чтобы реализовать эквивалентный класс в protobuf, необходимо объявить его в файле `.proto`. Компилятор `protoc` создаст класс .NET как часть процесса сборки.

```protobuf
syntax "proto3";

option csharp_namespace = "TraderSys";

message Stock {

    int32 id = 1;
    string symbol = 2;
    string display_name = 3;
    int32 market_id = 4;

}  
```

В первой строке объявляется используемая версия синтаксиса. Версия 3 языка была выпущена в 2016. Это версия, которую мы рекомендуем использовать для gRPC Services.

В `option csharp_namespace` строке указывается пространство имен, используемое для создаваемых C# типов. Этот параметр будет проигнорирован при компиляции файла `.proto` для других языков. Файлы protobuf часто содержат параметры, зависящие от языка, для нескольких языков.

В `Stock` определении сообщения указано четыре поля. Каждый из них имеет тип, имя и номер поля.

## <a name="field-numbers"></a>Номера полей

Номера полей являются важной частью protobuf. Они используются для указания полей в зашифрованных двоичных данных, что означает, что они не могут быть изменены с версии до версии вашей службы. Преимущество в том, что возможна обратная совместимость и прямая совместимость. Клиенты и службы будут просто игнорировать номера полей, о которых они не известны, при условии, что вероятность отсутствующих значений будет обработана.

В двоичном формате номер поля объединяется с идентификатором типа. Номера полей от 1 до 15 могут быть закодированы с помощью одного байта. Числа от 16 до 2 047 принимают 2 байта. Если в сообщении по какой-либо причине требуется более 2 047 полей для сообщения, можно увеличить значение. Однобайтовые идентификаторы полей с номерами от 1 до 15 обеспечивают лучшую производительность, поэтому их следует использовать для самых базовых и часто используемых полей.

## <a name="types"></a>Типы

Объявления типов используют собственные скалярные типы данных protobuf, которые более подробно обсуждаются в [следующем разделе](protobuf-data-types.md). В оставшейся части этой главы будут рассмотрены встроенные типы protobuf и показано, как они связаны с распространенными типами .NET.

> [!NOTE]
> Protobuf изначально не поддерживает тип `decimal`, поэтому вместо него используется `double`. Для приложений, требующих полной десятичной точности, ознакомьтесь с [разделом десятичные знаки](protobuf-data-types.md#decimals) в следующей части этой главы.

## <a name="the-generated-code"></a>Сформированный код

При сборке приложения protobuf создает классы для каждого сообщения, сопоставляя собственные типы с C# типами. Созданный тип `Stock` будет иметь следующую сигнатуру:

```csharp
public class Stock
{
    public int Id { get; set; }
    public string Symbol { get; set; }
    public string DisplayName { get; set; }
    public int MarketId { get; set; }
}
```

Фактически созданный код гораздо более сложен. Причина заключается в том, что каждый класс содержит весь код, необходимый для сериализации и десериализации в двоичном формате сети.

### <a name="property-names"></a>Имена свойств

Обратите внимание, что компилятор protobuf применяется `PascalCase` к именам свойств, хотя они были `snake_case` в `.proto` файле. В описании [protobuf Style Guide](https://developers.google.com/protocol-buffers/docs/style) рекомендуется использовать `snake_case` в определениях сообщений, чтобы создание кода для других платформ выработало ожидаемый вариант для своих соглашений.

>[!div class="step-by-step"]
>[Назад](protocol-buffers.md)
>[Вперед](protobuf-data-types.md)
