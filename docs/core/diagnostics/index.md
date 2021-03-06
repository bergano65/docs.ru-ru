---
title: Общие сведения о средствах диагностики в .NET Core
description: Общие сведения о средствах и методах диагностики приложений .NET Core.
ms.date: 12/17/2019
ms.topic: overview
ms.openlocfilehash: 0a78ec6c88f5323104277cddea4480a5e13b4e41
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2020
ms.locfileid: "75715570"
---
# <a name="what-diagnostic-tools-are-available-in-net-core"></a>Общие сведения о средствах диагностики в .NET Core

Программное обеспечение не всегда работает должным образом, но в .NET Core есть средства и API, которые помогут вам быстро и эффективно диагностировать проблемы.

Эта статья поможет вам выбрать нужные средства.

## <a name="managed-debuggers"></a>Управляемые отладчики

[Управляемые отладчики](managed-debuggers.md) позволяют взаимодействовать с программой. Такие операции, как приостановка, инкрементное выполнение, анализ и возобновление, предоставляют сведения о поведении кода. Отладчик — это самое подходящее средство для диагностики функциональных проблем, которые можно легко воспроизвести.

## <a name="logging-and-tracing"></a>Ведение журнала и трассировка

[Ведение журнала и трассировка](logging-tracing.md) — это связанные методы, предназначенные для инструментирования кода и создания файлов журнала. В файлах записываются сведения о том, что делает программа. Эти сведения можно использовать для диагностики самых сложных проблем. В сочетании с метками времени эти методы также используются для выявления проблем с производительностью.

## <a name="unit-testing"></a>Модульное тестирование

[Модульное тестирование](../testing/index.md) — это ключевой компонент непрерывной интеграции и развертывания высококачественного программного обеспечения. Модульные тесты позволяют сразу же узнать о возникшей проблеме.

## <a name="net-core-dotnet-diagnostic-global-tools"></a>Глобальные средства диагностики dotnet в .NET Core

### <a name="dotnet-counters"></a>dotnet-counters

[dotnet-counters](dotnet-counters.md) — это средство мониторинга производительности для первого уровня мониторинга работоспособности и анализа производительности. Оно отслеживает значения счетчиков производительности, опубликованные с помощью API <xref:System.Diagnostics.Tracing.EventCounter>. Например, можно быстро отслеживать использование ЦП или частоту возникновения исключений в приложении .NET Core.

### <a name="dotnet-dump"></a>dotnet-dump

[dotnet-dump](dotnet-dump.md) — это средство сбора и анализа дампов ядра Windows и Linux без собственного отладчика.

### <a name="dotnet-trace"></a>dotnet-trace

.NET Core включает в себя `EventPipe`, с помощью которого предоставляются диагностические данные. Средство [dotnet-trace](dotnet-trace.md) позволяет использовать интересные данные профилирования из приложения, которые могут помочь в сценариях, когда вам нужно найти причину медленной работы приложений.

## <a name="net-core-diagnostics-tutorials"></a>Учебники по диагностике .NET Core

### <a name="debug-a-memory-leak"></a>Отладка утечек памяти

[Учебник. Отладка утечек памяти](debug-memory-leak.md) содержит пошаговые инструкции по поиску утечек памяти. Средство [dotnet-counters](dotnet-counters.md) позволяет подтвердить наличие утечки, а средство [dotnet-dump](dotnet-dump.md) используется для ее диагностики.
