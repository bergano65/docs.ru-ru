---
title: Безопасное обновление интерфейсов с помощью методов интерфейса по умолчанию в C#
description: В этом подробном учебнике рассматривается, как можно добавить новые возможности существующих определений интерфейса без нарушения работы классов и структур, которые реализуют этот интерфейс.
ms.date: 05/06/2019
ms.custom: mvc
ms.openlocfilehash: 71fce2594dbf5ef3175a6b9bdf4e6edba754bb84
ms.sourcegitcommit: 992f80328b51b165051c42ff5330788627abe973
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275989"
---
# <a name="tutorial-update-interfaces-with-default-interface-methods-in-c-80"></a><span data-ttu-id="405f1-103">Учебник. Обновление интерфейсов с помощью методов интерфейса по умолчанию в C# 8.0</span><span class="sxs-lookup"><span data-stu-id="405f1-103">Tutorial: Update interfaces with default interface methods in C# 8.0</span></span>

<span data-ttu-id="405f1-104">Начиная с C# 8.0 в .NET Core 3.0 можно определить реализацию при объявлении члена интерфейса.</span><span class="sxs-lookup"><span data-stu-id="405f1-104">Beginning with C# 8.0 on .NET Core 3.0, you can define an implementation when you declare a member of an interface.</span></span> <span data-ttu-id="405f1-105">Наиболее распространенным сценарием является безопасное добавление членов в интерфейс, который уже выпущен и используется многочисленными клиентами.</span><span class="sxs-lookup"><span data-stu-id="405f1-105">The most common scenario is to safely add members to an interface already released and used by innumerable clients.</span></span>

<span data-ttu-id="405f1-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="405f1-106">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="405f1-107">Безопасно расширять интерфейсы, добавляя методы с реализациями.</span><span class="sxs-lookup"><span data-stu-id="405f1-107">Extend interfaces safely by adding methods with implementations.</span></span>
> * <span data-ttu-id="405f1-108">Создавать параметризованные реализации, которые обеспечивают большую гибкость.</span><span class="sxs-lookup"><span data-stu-id="405f1-108">Create parameterized implementations to provide greater flexibility.</span></span>
> * <span data-ttu-id="405f1-109">Позволить разработчику реализовать более конкретную реализацию в виде переопределения.</span><span class="sxs-lookup"><span data-stu-id="405f1-109">Enable implementers to provide a more specific implementation in the form of an override.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="405f1-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="405f1-110">Prerequisites</span></span>

<span data-ttu-id="405f1-111">Вам нужно настроить свой компьютер для выполнения .NET Core, включая компилятор C# 8.0.</span><span class="sxs-lookup"><span data-stu-id="405f1-111">You’ll need to set up your machine to run .NET Core, including the C# 8.0 compiler.</span></span> <span data-ttu-id="405f1-112">Компилятор C# 8.0 доступен, начиная с [версии 16.3 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019), или [в пакете SDK .NET Core 3.0](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="405f1-112">The C# 8.0 compiler is available starting with [Visual Studio 2019 version 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or the [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download).</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="405f1-113">Обзор сценария</span><span class="sxs-lookup"><span data-stu-id="405f1-113">Scenario overview</span></span>

<span data-ttu-id="405f1-114">Этот учебник начинается с первой версии библиотеки связи с клиентом.</span><span class="sxs-lookup"><span data-stu-id="405f1-114">This tutorial starts with version 1 of a customer relationship library.</span></span> <span data-ttu-id="405f1-115">Начальное приложение можно получить в нашем репозитории [примеров на сайте GitHub](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/starter/customer-relationship).</span><span class="sxs-lookup"><span data-stu-id="405f1-115">You can get the starter application on our [samples repo on GitHub](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/starter/customer-relationship).</span></span> <span data-ttu-id="405f1-116">Компания, которая создала эту библиотеку, рассчитывала, что клиенты с существующими приложениями будут ее внедрять.</span><span class="sxs-lookup"><span data-stu-id="405f1-116">The company that built this library intended customers with existing applications to adopt their library.</span></span> <span data-ttu-id="405f1-117">Она определила минимальный интерфейс для реализации библиотеки пользователями.</span><span class="sxs-lookup"><span data-stu-id="405f1-117">They provided minimal interface definitions for users of their library to implement.</span></span> <span data-ttu-id="405f1-118">Вот определение интерфейса для клиента:</span><span class="sxs-lookup"><span data-stu-id="405f1-118">Here's the interface definition for a customer:</span></span>

[!code-csharp[InitialCustomerInterface](~/samples/csharp/tutorials/default-interface-members-versions/starter/customer-relationship/ICustomer.cs?name=SnippetICustomerVersion1)]

<span data-ttu-id="405f1-119">Компания определила второй интерфейс, представляющий заказ:</span><span class="sxs-lookup"><span data-stu-id="405f1-119">They defined a second interface that represents an order:</span></span>

[!code-csharp[InitialOrderInterface](~/samples/csharp/tutorials/default-interface-members-versions/starter/customer-relationship/IOrder.cs?name=SnippetIorderVersion1)]

<span data-ttu-id="405f1-120">На основе этих интерфейсов команда может собрать библиотеку для удобства работы клиентов своих пользователей.</span><span class="sxs-lookup"><span data-stu-id="405f1-120">From those interfaces, the team could build a library for their users to create a better experience for their customers.</span></span> <span data-ttu-id="405f1-121">Их целью было более полно взаимодействовать с существующими клиентами и повысить уровень связи с новыми.</span><span class="sxs-lookup"><span data-stu-id="405f1-121">Their goal was to create a deeper relationship with existing customers and improve their relationships with new customers.</span></span>

<span data-ttu-id="405f1-122">Пришло время перейти к библиотеке для следующего выпуска.</span><span class="sxs-lookup"><span data-stu-id="405f1-122">Now, it's time to upgrade the library for the next release.</span></span> <span data-ttu-id="405f1-123">Одна из востребованных возможностей — добавление скидки за лояльность для клиентов, размещающих большое количество заказов.</span><span class="sxs-lookup"><span data-stu-id="405f1-123">One of the requested features enables a loyalty discount for customers that have lots of orders.</span></span> <span data-ttu-id="405f1-124">Это новая скидка лояльности применяется каждый раз, когда клиент делает заказ.</span><span class="sxs-lookup"><span data-stu-id="405f1-124">This new loyalty discount gets applied whenever a customer makes an order.</span></span> <span data-ttu-id="405f1-125">Специальная скидка является свойством каждого клиента.</span><span class="sxs-lookup"><span data-stu-id="405f1-125">The specific discount is a property of each individual customer.</span></span> <span data-ttu-id="405f1-126">Каждая реализация `ICustomer` может задавать разные правила для скидки лояльности.</span><span class="sxs-lookup"><span data-stu-id="405f1-126">Each implementation of `ICustomer` can set different rules for the loyalty discount.</span></span> 

<span data-ttu-id="405f1-127">Наиболее удобный способ добавления этой функции — расширить `ICustomer` методом для применения любых скидок лояльности.</span><span class="sxs-lookup"><span data-stu-id="405f1-127">The most natural way to add this functionality is to enhance the `ICustomer` interface with a method to apply any loyalty discount.</span></span> <span data-ttu-id="405f1-128">Это предложение по разработке вызвало проблемы среди опытных разработчиков. "Интерфейсы являются неизменяемыми после выпуска!</span><span class="sxs-lookup"><span data-stu-id="405f1-128">This design suggestion caused concern among experienced developers: "Interfaces are immutable once they've been released!</span></span> <span data-ttu-id="405f1-129">Это критическое изменение!"</span><span class="sxs-lookup"><span data-stu-id="405f1-129">This is a breaking change!"</span></span> <span data-ttu-id="405f1-130">В C# 8.0 добавлены *реализации интерфейсов по умолчанию* для обновления интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="405f1-130">C# 8.0 adds *default interface implementations* for upgrading interfaces.</span></span> <span data-ttu-id="405f1-131">Авторы библиотеки могут добавлять новые элементы интерфейса и реализации по умолчанию для этих элементов.</span><span class="sxs-lookup"><span data-stu-id="405f1-131">The library authors can add new members to the interface and provide a default implementation for those members.</span></span>

<span data-ttu-id="405f1-132">Реализации интерфейса по умолчанию позволяют разработчикам обновить интерфейс, по-прежнему позволяя другим разработчикам переопределять эту реализацию.</span><span class="sxs-lookup"><span data-stu-id="405f1-132">Default interface implementations enable developers to upgrade an interface while still enabling any implementors to override that implementation.</span></span> <span data-ttu-id="405f1-133">Пользователи библиотеки могут принимать реализацию по умолчанию в качестве некритического изменения.</span><span class="sxs-lookup"><span data-stu-id="405f1-133">Users of the library can accept the default implementation as a non-breaking change.</span></span> <span data-ttu-id="405f1-134">Если их бизнес-правила не совпадают, их можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="405f1-134">If their business rules are different, they can override.</span></span>

## <a name="upgrade-with-default-interface-methods"></a><span data-ttu-id="405f1-135">Обновление с методами интерфейса по умолчанию</span><span class="sxs-lookup"><span data-stu-id="405f1-135">Upgrade with default interface methods</span></span>

<span data-ttu-id="405f1-136">Команда пришла к выводу относительно реализации по умолчанию: скидки за лояльность для клиентов.</span><span class="sxs-lookup"><span data-stu-id="405f1-136">The team agreed on the most likely default implementation: a loyalty discount for customers.</span></span>

<span data-ttu-id="405f1-137">Обновление должно давать возможность задать два свойства: количество заказов, необходимое, чтобы иметь право на скидку, а также процент скидки.</span><span class="sxs-lookup"><span data-stu-id="405f1-137">The upgrade should provide the functionality to set two properties: the number of orders needed to be eligible for the discount, and the percentage of the discount.</span></span> <span data-ttu-id="405f1-138">В результате мы получаем идеальный сценарий для применения методов интерфейса по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="405f1-138">This makes it a perfect scenario for default interface methods.</span></span> <span data-ttu-id="405f1-139">Можно добавить метод в интерфейс `ICustomer` и предоставить наиболее вероятную реализацию.</span><span class="sxs-lookup"><span data-stu-id="405f1-139">You can add a method to the `ICustomer` interface, and provide the most likely implementation.</span></span> <span data-ttu-id="405f1-140">Все существующие и любые новые реализации могут использовать реализацию по умолчанию или предоставить свои собственные.</span><span class="sxs-lookup"><span data-stu-id="405f1-140">All existing, and any new implementations can use the default implementation, or provide their own.</span></span>

<span data-ttu-id="405f1-141">Во-первых, добавьте новый метод к реализации:</span><span class="sxs-lookup"><span data-stu-id="405f1-141">First, add the new method to the implementation:</span></span>

[!code-csharp[InitialOrderInterface](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetLoyaltyDiscountVersionOne)]

<span data-ttu-id="405f1-142">Автор библиотеки написал первый тест для проверки реализации:</span><span class="sxs-lookup"><span data-stu-id="405f1-142">The library author wrote a first test to check the implementation:</span></span>

[!code-csharp[TestDefaultImplementation](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetTestDefaultImplementation)]

<span data-ttu-id="405f1-143">Обратите внимание на следующую часть теста:</span><span class="sxs-lookup"><span data-stu-id="405f1-143">Notice the following portion of the test:</span></span>

[!code-csharp[TestDefaultImplementation](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetHighlightCast)]

<span data-ttu-id="405f1-144">Приведение `SampleCustomer` к `ICustomer` необходимо.</span><span class="sxs-lookup"><span data-stu-id="405f1-144">That cast from `SampleCustomer` to `ICustomer` is necessary.</span></span> <span data-ttu-id="405f1-145">Классу `SampleCustomer` не требуется предоставлять реализацию для `ComputeLoyaltyDiscount`; она предоставляется интерфейсом `ICustomer`.</span><span class="sxs-lookup"><span data-stu-id="405f1-145">The `SampleCustomer` class doesn't need to provide an implementation for `ComputeLoyaltyDiscount`; that's provided by the `ICustomer` interface.</span></span> <span data-ttu-id="405f1-146">Тем не менее класс `SampleCustomer` не наследует члены от своих интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="405f1-146">However, the `SampleCustomer` class doesn't inherit members from its interfaces.</span></span> <span data-ttu-id="405f1-147">Это правило не изменилось.</span><span class="sxs-lookup"><span data-stu-id="405f1-147">That rule hasn't changed.</span></span> <span data-ttu-id="405f1-148">Чтобы вызвать любой метод, который был объявлен и реализован в интерфейсе, переменная должна иметь тип интерфейса (`ICustomer` в этом примере).</span><span class="sxs-lookup"><span data-stu-id="405f1-148">In order to call any method declared and implemented in the interface, the variable must be the type of the interface, `ICustomer` in this example.</span></span>

## <a name="provide-parameterization"></a><span data-ttu-id="405f1-149">Задайте параметризацию</span><span class="sxs-lookup"><span data-stu-id="405f1-149">Provide parameterization</span></span>

<span data-ttu-id="405f1-150">Это хорошее начало.</span><span class="sxs-lookup"><span data-stu-id="405f1-150">That's a good start.</span></span> <span data-ttu-id="405f1-151">Однако реализация по умолчанию слишком строга.</span><span class="sxs-lookup"><span data-stu-id="405f1-151">But, the default implementation is too restrictive.</span></span> <span data-ttu-id="405f1-152">Другие пользователи этой системы могут выбрать различные пороговые значения для числа покупок, разную длительность членства или другой процент скидки.</span><span class="sxs-lookup"><span data-stu-id="405f1-152">Many consumers of this system may choose different thresholds for number of purchases, a different length of membership, or a different percentage discount.</span></span> <span data-ttu-id="405f1-153">Удобство работы обновления можно повысить для нескольких клиентов сразу, предоставляя возможность установить эти параметры.</span><span class="sxs-lookup"><span data-stu-id="405f1-153">You can provide a better upgrade experience for more customers by providing a way to set those parameters.</span></span> <span data-ttu-id="405f1-154">Давайте добавим статический метод, который задает эти три параметра, контролируя реализацию по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="405f1-154">Let's add a static method that sets those three parameters controlling the default implementation:</span></span>

[!code-csharp[VersionTwoImplementation](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetLoyaltyDiscountVersionTwo)]

<span data-ttu-id="405f1-155">В этом небольшом фрагменте кода показано много новых языковых возможностей.</span><span class="sxs-lookup"><span data-stu-id="405f1-155">There are many new language capabilities shown in that small code fragment.</span></span> <span data-ttu-id="405f1-156">Интерфейсы теперь могут содержать статические члены, включая поля и методы.</span><span class="sxs-lookup"><span data-stu-id="405f1-156">Interfaces can now include static members, including fields and methods.</span></span> <span data-ttu-id="405f1-157">Также включены разные модификаторы доступа.</span><span class="sxs-lookup"><span data-stu-id="405f1-157">Different access modifiers are also enabled.</span></span> <span data-ttu-id="405f1-158">Дополнительные поля являются закрытыми, новый метод является открытым.</span><span class="sxs-lookup"><span data-stu-id="405f1-158">The additional fields are private, the new method is public.</span></span> <span data-ttu-id="405f1-159">Все эти модификаторы разрешены для членов интерфейса.</span><span class="sxs-lookup"><span data-stu-id="405f1-159">Any of the modifiers are allowed on interface members.</span></span>

<span data-ttu-id="405f1-160">Приложениям, использующим общую формулу для вычисления скидок лояльности с разными параметрами, не нужно создавать собственные реализации; достаточно задать аргументы с помощью статического метода.</span><span class="sxs-lookup"><span data-stu-id="405f1-160">Applications that use the general formula for computing the loyalty discount, but different parameters, don't need to provide a custom implementation; they can set the arguments through a static method.</span></span> <span data-ttu-id="405f1-161">Например, следующий код задает "повышение уровня клиента", в рамках которого награждается любой клиент более чем с месячным сроком членства:</span><span class="sxs-lookup"><span data-stu-id="405f1-161">For example, the following code sets a "customer appreciation" that rewards any customer with more than one month's membership:</span></span>

[!code-csharp[SetLoyaltyThresholds](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetSetLoyaltyThresholds)]

## <a name="extend-the-default-implementation"></a><span data-ttu-id="405f1-162">Расширение реализации по умолчанию</span><span class="sxs-lookup"><span data-stu-id="405f1-162">Extend the default implementation</span></span>

<span data-ttu-id="405f1-163">Код, который вы добавили на данный момент, предоставляет удобную реализацию для сценариев, где нужны что-то наподобие реализации по умолчанию или несвязанный набор правил.</span><span class="sxs-lookup"><span data-stu-id="405f1-163">The code you've added so far has provided a convenient implementation for those scenarios where users want something like the default implementation, or to provide an unrelated set of rules.</span></span> <span data-ttu-id="405f1-164">Чтобы дополнить код, давайте проведем рефакторинг, чтобы учесть сценарии, где пользователям нужно расширить реализацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="405f1-164">For a final feature, let's refactor the code a bit to enable scenarios where users may want to build on the default implementation.</span></span> 

<span data-ttu-id="405f1-165">Предположим, стартап хочет привлечь новых клиентов.</span><span class="sxs-lookup"><span data-stu-id="405f1-165">Consider a startup that wants to attract new customers.</span></span> <span data-ttu-id="405f1-166">Они предоставляют скидку в 50 % на первый заказ нового клиента.</span><span class="sxs-lookup"><span data-stu-id="405f1-166">They offer a 50% discount off a new customer's first order.</span></span> <span data-ttu-id="405f1-167">В противном случае существующие клиенты получают стандартные скидки.</span><span class="sxs-lookup"><span data-stu-id="405f1-167">Otherwise, existing customers get the standard discount.</span></span> <span data-ttu-id="405f1-168">Автору библиотеки необходимо перенести реализацию по умолчанию в метод `protected static`, чтобы любой класс, реализующий этот интерфейс, мог повторно использовать код в своей реализации.</span><span class="sxs-lookup"><span data-stu-id="405f1-168">The library author needs to move the default implementation into a `protected static` method so that any class implementing this interface can reuse the code in their implementation.</span></span> <span data-ttu-id="405f1-169">По умолчанию реализация члена интерфейса также вызывает этот общий метод:</span><span class="sxs-lookup"><span data-stu-id="405f1-169">The default implementation of the interface member calls this shared method as well:</span></span>

[!code-csharp[VersionTwoImplementation](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetFinalVersion)]

<span data-ttu-id="405f1-170">В реализации класса, реализующего этот интерфейс, переопределение может вызывать статический вспомогательный метод и расширить эту логику для предоставления скидки новым клиентам:</span><span class="sxs-lookup"><span data-stu-id="405f1-170">In an implementation of a class that implements this interface, the override can call the static helper method, and extend that logic to provide the "new customer" discount:</span></span>

[!code-csharp[VersionTwoImplementation](~/samples/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/SampleCustomer.cs?name=SnippetOverrideAndExtend)]

<span data-ttu-id="405f1-171">Весь готовый код доступен в [репозитории примеров на GitHub](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/finished/customer-relationship).</span><span class="sxs-lookup"><span data-stu-id="405f1-171">You can see the entire finished code in our [samples repo on GitHub](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/finished/customer-relationship).</span></span> <span data-ttu-id="405f1-172">Начальное приложение можно получить в нашем репозитории [примеров на сайте GitHub](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/starter/customer-relationship).</span><span class="sxs-lookup"><span data-stu-id="405f1-172">You can get the starter application on our [samples repo on GitHub](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/starter/customer-relationship).</span></span>

<span data-ttu-id="405f1-173">Эти новые функции означают, что интерфейсы могут обновляться безопасно при наличии разумной реализации по умолчанию для этих новых элементов.</span><span class="sxs-lookup"><span data-stu-id="405f1-173">These new features mean that interfaces can be updated safely when there's a reasonable default implementation for those new members.</span></span> <span data-ttu-id="405f1-174">Тщательно проектируйте интерфейсы для выражения единой функциональной идеи, которую могут реализовывать несколько классов.</span><span class="sxs-lookup"><span data-stu-id="405f1-174">Carefully design interfaces to express single functional ideas that can be implemented by multiple classes.</span></span> <span data-ttu-id="405f1-175">Это упрощает обновление этих определений интерфейса при обнаружении новых требований для этой же функциональности.</span><span class="sxs-lookup"><span data-stu-id="405f1-175">That makes it easier to upgrade those interface definitions when new requirements are discovered for that same functional idea.</span></span>