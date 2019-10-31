---
title: Привязка данных WPF с помощью LINQ to XML
ms.date: 10/22/2019
ms.topic: conceptual
ms.openlocfilehash: 53bc5e09d3c837b69c8f215b1b5c61d1b745f683
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73139802"
---
# <a name="overview-of-wpf-data-binding-with-linq-to-xml"></a><span data-ttu-id="04b6a-102">Общие сведения о привязке данных WPF к LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="04b6a-102">Overview of WPF data binding with LINQ to XML</span></span>

<span data-ttu-id="04b6a-103">В этой статье описаны функции динамического связывания данных в пространстве имен <xref:System.Xml.Linq>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-103">This article introduces the dynamic data binding features in the <xref:System.Xml.Linq> namespace.</span></span> <span data-ttu-id="04b6a-104">Эти средства можно применять в качестве источника данных для элементов пользовательского интерфейса в приложениях Windows Presentation Foundation (WPF).</span><span class="sxs-lookup"><span data-stu-id="04b6a-104">These features can be used as a data source for user interface (UI) elements in Windows Presentation Foundation (WPF) apps.</span></span> <span data-ttu-id="04b6a-105">Такая организация работы основана на использовании особых *динамических свойств* классов <xref:System.Xml.Linq.XAttribute?displayProperty=fullName> и <xref:System.Xml.Linq.XElement?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-105">This scenario relies upon special *dynamic properties* of <xref:System.Xml.Linq.XAttribute?displayProperty=fullName> and <xref:System.Xml.Linq.XElement?displayProperty=fullName>.</span></span>

## <a name="xaml-and-linq-to-xml"></a><span data-ttu-id="04b6a-106">XAML и LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="04b6a-106">XAML and LINQ to XML</span></span>

<span data-ttu-id="04b6a-107">Язык XAML является диалектом XML, созданным в корпорации Майкрософт для поддержки технологий .NET.</span><span class="sxs-lookup"><span data-stu-id="04b6a-107">The Extensible Application Markup Language (XAML) is an XML dialect created by Microsoft to support .NET technologies.</span></span> <span data-ttu-id="04b6a-108">Он используется в WPF для представления элементов пользовательского интерфейса и связанных с этим возможностей, таких как события и привязка данных.</span><span class="sxs-lookup"><span data-stu-id="04b6a-108">It is used in WPF to represent user interface elements and related features, such as events and data binding.</span></span> <span data-ttu-id="04b6a-109">В Windows Workflow Foundation язык XAML используется для представления структуры программ, например в виде средств управления программой (*потоков операций*).</span><span class="sxs-lookup"><span data-stu-id="04b6a-109">In Windows Workflow Foundation, XAML is used to represent program structure, such as program control (*workflows*).</span></span> <span data-ttu-id="04b6a-110">XAML позволяет отделить декларативные аспекты технологии от связанного с ними процедурного кода, определяющего более индивидуализированное поведение программы.</span><span class="sxs-lookup"><span data-stu-id="04b6a-110">XAML enables the declarative aspects of a technology to be separated from the related procedural code that defines the more individualized behavior of a program.</span></span>

<span data-ttu-id="04b6a-111">Существует два общих способа взаимодействия XAML и LINQ to XML.</span><span class="sxs-lookup"><span data-stu-id="04b6a-111">There are two broad ways that XAML and LINQ to XML can interact:</span></span>

- <span data-ttu-id="04b6a-112">Поскольку файлы XAML - это XML-документы правильного формата, к ним можно выполнять запросы и управлять ими с помощью таких XML-технологий, как LINQ to XML.</span><span class="sxs-lookup"><span data-stu-id="04b6a-112">Because XAML files are well-formed XML, they can be queried and manipulated through XML technologies such as LINQ to XML.</span></span>

- <span data-ttu-id="04b6a-113">Запросы LINQ to XML представляют источник данных, поэтому могут использоваться как источник данных для привязки данных в элементах пользовательского интерфейса WPF.</span><span class="sxs-lookup"><span data-stu-id="04b6a-113">Because LINQ to XML queries represent a source of data, these queries can be used as a data source for data binding for WPF UI elements.</span></span>

<span data-ttu-id="04b6a-114">В этой документации описан второй сценарий.</span><span class="sxs-lookup"><span data-stu-id="04b6a-114">This documentation describes the second scenario.</span></span>

## <a name="data-binding-in-the-windows-presentation-foundation"></a><span data-ttu-id="04b6a-115">Привязка данных в Windows Presentation Foundation</span><span class="sxs-lookup"><span data-stu-id="04b6a-115">Data binding in the Windows Presentation Foundation</span></span>

<span data-ttu-id="04b6a-116">Привязка данных в WPF позволяет связать элемент пользовательского интерфейса с одним из его свойств в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="04b6a-116">WPF data binding enables a UI element to associate one of its properties with a data source.</span></span> <span data-ttu-id="04b6a-117">Простым примером этого может служить метка <xref:System.Windows.Controls.Label>, текст которой представляет значение открытого свойства в пользовательском объекте.</span><span class="sxs-lookup"><span data-stu-id="04b6a-117">A simple example of this is a <xref:System.Windows.Controls.Label> whose text presents the value of a public property in a user-defined object.</span></span> <span data-ttu-id="04b6a-118">Привязка данных в WPF зависит от следующих компонентов.</span><span class="sxs-lookup"><span data-stu-id="04b6a-118">WPF data binding relies on the following components:</span></span>

|<span data-ttu-id="04b6a-119">Компонент</span><span class="sxs-lookup"><span data-stu-id="04b6a-119">Component</span></span>|<span data-ttu-id="04b6a-120">Описание</span><span class="sxs-lookup"><span data-stu-id="04b6a-120">Description</span></span>|
|---------------|-----------------|
|<span data-ttu-id="04b6a-121">Цель привязки</span><span class="sxs-lookup"><span data-stu-id="04b6a-121">Binding target</span></span>|<span data-ttu-id="04b6a-122">Элемент пользовательского интерфейса, который должен быть привязан к источнику данных.</span><span class="sxs-lookup"><span data-stu-id="04b6a-122">The UI element to be associated with the data source.</span></span> <span data-ttu-id="04b6a-123">Визуальные элементы WPF являются производными класса <xref:System.Windows.UIElement>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-123">Visual elements in WPF are derived from the <xref:System.Windows.UIElement> class.</span></span>|
|<span data-ttu-id="04b6a-124">Целевое свойство</span><span class="sxs-lookup"><span data-stu-id="04b6a-124">Target property</span></span>|<span data-ttu-id="04b6a-125">*Свойство зависимостей* целевого объекта привязки, отражающее значение источника привязки к данным.</span><span class="sxs-lookup"><span data-stu-id="04b6a-125">The *dependency property* of the binding target that reflects the value of the data-binding source.</span></span> <span data-ttu-id="04b6a-126">Свойства зависимости напрямую поддерживаются классом <xref:System.Windows.DependencyObject>, производным которого является <xref:System.Windows.UIElement>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-126">Dependency properties are directly supported by the <xref:System.Windows.DependencyObject> class, which <xref:System.Windows.UIElement> derives from.</span></span>|
|<span data-ttu-id="04b6a-127">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="04b6a-127">Binding source</span></span>|<span data-ttu-id="04b6a-128">Исходный объект для одного или нескольких значений, которые отправляются в элемент пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="04b6a-128">The source object for one or more values that are supplied to the UI element for presentation.</span></span> <span data-ttu-id="04b6a-129">WPF автоматически поддерживает следующие типы источников привязки: объекты среды CLR, объекты данных ADO.NET, XML-данные (полученные с помощью запросов XPath или LINQ to XML) и другие объекты <xref:System.Windows.DependencyObject>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-129">WPF automatically supports the following types as binding sources: CLR objects, ADO.NET data objects, XML data (from XPath or LINQ to XML queries), or another <xref:System.Windows.DependencyObject>.</span></span>|
|<span data-ttu-id="04b6a-130">Путь к источнику</span><span class="sxs-lookup"><span data-stu-id="04b6a-130">Source path</span></span>|<span data-ttu-id="04b6a-131">Свойство источника привязки, разрешение которого приводит к получению значения или набора значений, подлежащих привязке.</span><span class="sxs-lookup"><span data-stu-id="04b6a-131">The property of the binding source that resolves to the value or set of values that is to be bound.</span></span>|

<span data-ttu-id="04b6a-132">Свойство зависимости является понятием WPF, которое представляет динамически вычисляемое свойство элемента пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="04b6a-132">A dependency property is a concept specific to WPF that represent a dynamically computed property of a UI element.</span></span> <span data-ttu-id="04b6a-133">Например, свойства зависимостей часто имеют значение или значения по умолчанию, предоставляемые родительским элементом.</span><span class="sxs-lookup"><span data-stu-id="04b6a-133">For example, dependency properties often have default values or values that are provided by a parent element.</span></span> <span data-ttu-id="04b6a-134">Эти особые свойства поддерживаются экземплярами класса <xref:System.Windows.DependencyProperty> (но не полями, как в случае стандартных свойств).</span><span class="sxs-lookup"><span data-stu-id="04b6a-134">These special properties are backed by instances of the <xref:System.Windows.DependencyProperty> class (and not fields as with standard properties).</span></span> <span data-ttu-id="04b6a-135">Дополнительные сведения см. в [обзоре свойств зависимостей](/dotnet/framework/wpf/advanced/dependency-properties-overview).</span><span class="sxs-lookup"><span data-stu-id="04b6a-135">For more information, see [Dependency Properties Overview](/dotnet/framework/wpf/advanced/dependency-properties-overview).</span></span>

### <a name="dynamic-data-binding-in-wpf"></a><span data-ttu-id="04b6a-136">Динамическая привязка данных в WPF</span><span class="sxs-lookup"><span data-stu-id="04b6a-136">Dynamic data binding in WPF</span></span>

<span data-ttu-id="04b6a-137">По умолчанию привязка данных происходит, только если инициализирован целевой элемент пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="04b6a-137">By default, data binding occurs only when the target UI element is initialized.</span></span> <span data-ttu-id="04b6a-138">Это называется *однократной* привязкой.</span><span class="sxs-lookup"><span data-stu-id="04b6a-138">This is called *one-time* binding.</span></span> <span data-ttu-id="04b6a-139">Для большинства целей этого недостаточно, поскольку решение с привязкой к данным требует, чтобы изменения динамически передавались во время выполнения с помощью одного из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="04b6a-139">For most purposes, this is insufficient; typically, a data-binding solution requires that the changes be dynamically propagated at run time using one of the following:</span></span>

- <span data-ttu-id="04b6a-140">*Односторонняя* привязка автоматически передает изменения в одном направлении.</span><span class="sxs-lookup"><span data-stu-id="04b6a-140">*One-way* binding causes the changes to one side to be propagated automatically.</span></span> <span data-ttu-id="04b6a-141">В большинстве случаев изменения в источнике отражаются в назначении, но иногда полезно, чтобы происходило обратное.</span><span class="sxs-lookup"><span data-stu-id="04b6a-141">Most commonly, changes to the source are reflected in the target, but the reverse can sometimes be useful.</span></span>

- <span data-ttu-id="04b6a-142">При наличии *двусторонней* привязки изменения в источнике автоматически передаются в назначение, а изменения в назначении автоматически отражаются в источнике.</span><span class="sxs-lookup"><span data-stu-id="04b6a-142">In *two-way* binding, changes to the source are automatically propagated to the target, and changes to the target are automatically propagated to the source.</span></span>

<span data-ttu-id="04b6a-143">Чтобы односторонняя или двусторонняя привязка могла иметь место, источник должен применять механизм уведомлений об изменениях, например с помощью реализации интерфейса <xref:System.ComponentModel.INotifyPropertyChanged>, либо используя шаблон *PropertyNameChanged* для каждого поддерживаемого свойства.</span><span class="sxs-lookup"><span data-stu-id="04b6a-143">For one-way or two-way binding to occur, the source must implement a change notification mechanism, for example by implementing the <xref:System.ComponentModel.INotifyPropertyChanged> interface or by using a *PropertyNameChanged* pattern for each property supported.</span></span>

<span data-ttu-id="04b6a-144">Дополнительные сведения о привязке данных в WPF см. в статье [Привязка данных (WPF)](/dotnet/framework/wpf/data/data-binding-wpf).</span><span class="sxs-lookup"><span data-stu-id="04b6a-144">For more information about data binding in WPF, see [Data Binding (WPF)](/dotnet/framework/wpf/data/data-binding-wpf).</span></span>

## <a name="dynamic-properties-in-linq-to-xml-classes"></a><span data-ttu-id="04b6a-145">Динамические свойства в классах LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="04b6a-145">Dynamic properties in LINQ to XML classes</span></span>

<span data-ttu-id="04b6a-146">Большинство классов LINQ to XML не могут считаться настоящими динамическими источниками данных WPF:</span><span class="sxs-lookup"><span data-stu-id="04b6a-146">Most LINQ to XML classes do not qualify as proper WPF dynamic data sources.</span></span> <span data-ttu-id="04b6a-147">некоторые самые полезные данные доступны только через методы, но не через свойства, а свойства не реализуют уведомление об изменениях.</span><span class="sxs-lookup"><span data-stu-id="04b6a-147">Some of the most useful information is available only through methods, not properties, and properties in these classes do not implement change notifications.</span></span> <span data-ttu-id="04b6a-148">Для поддержки привязки данных WPF в LINQ to XML предоставляется набор *динамических свойств*.</span><span class="sxs-lookup"><span data-stu-id="04b6a-148">To support WPF data binding, LINQ to XML exposes a set of *dynamic properties*.</span></span>

<span data-ttu-id="04b6a-149">Эти динамические свойства являются особыми свойствами времени выполнения, дублирующими функциональность существующих методов и свойств в классах <xref:System.Xml.Linq.XAttribute> и <xref:System.Xml.Linq.XElement>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-149">These dynamic properties are special run-time properties that duplicate the functionality of existing methods and properties in the <xref:System.Xml.Linq.XAttribute> and <xref:System.Xml.Linq.XElement> classes.</span></span> <span data-ttu-id="04b6a-150">Динамические свойства были добавлены к этим классам только для того, чтобы позволить им выступать в роли динамических источников данных для WPF.</span><span class="sxs-lookup"><span data-stu-id="04b6a-150">They were added to these classes solely to enable them to act as dynamic data sources for WPF.</span></span> <span data-ttu-id="04b6a-151">С этой целью все динамические свойства реализуют уведомления об изменениях.</span><span class="sxs-lookup"><span data-stu-id="04b6a-151">To meet this need, all these dynamic properties implement change notifications.</span></span> <span data-ttu-id="04b6a-152">Подробные сведения о динамических свойствах представлены в следующем разделе, [Динамические свойства LINQ to XML](linq-to-xml-dynamic-properties.md).</span><span class="sxs-lookup"><span data-stu-id="04b6a-152">A detailed reference for these dynamic properties is provided in the next section, [LINQ to XML Dynamic Properties](linq-to-xml-dynamic-properties.md).</span></span>

> [!NOTE]
> <span data-ttu-id="04b6a-153">Многие стандартные общие свойства различных классов в пространстве имен <xref:System.Xml.Linq> могут использоваться в качестве однократной привязки данных.</span><span class="sxs-lookup"><span data-stu-id="04b6a-153">Many of the standard public properties, found in the various classes in the <xref:System.Xml.Linq> namespace, can be used for one-time data binding.</span></span> <span data-ttu-id="04b6a-154">Однако следует помнить, что в этом случае ни источник, ни цель не будут обновляться динамически.</span><span class="sxs-lookup"><span data-stu-id="04b6a-154">However, remember that neither the source nor the target will be dynamically updated under this scheme.</span></span>

### <a name="access-dynamic-properties"></a><span data-ttu-id="04b6a-155">Доступ к динамическим свойствам</span><span class="sxs-lookup"><span data-stu-id="04b6a-155">Access dynamic properties</span></span>

<span data-ttu-id="04b6a-156">Динамические свойства в классах <xref:System.Xml.Linq.XAttribute> и <xref:System.Xml.Linq.XElement> не могут оцениваться как стандартные свойства.</span><span class="sxs-lookup"><span data-stu-id="04b6a-156">The dynamic properties in the <xref:System.Xml.Linq.XAttribute> and <xref:System.Xml.Linq.XElement> classes cannot be accessed like standard properties.</span></span> <span data-ttu-id="04b6a-157">Например, в языках, совместимых со средой CLR, например в C#, с ними нельзя выполнять следующие действия.</span><span class="sxs-lookup"><span data-stu-id="04b6a-157">For example, in CLR-compliant languages such as C#, they cannot be:</span></span>

- <span data-ttu-id="04b6a-158">Оценивать непосредственно во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="04b6a-158">Accessed directly at compile time.</span></span> <span data-ttu-id="04b6a-159">Динамические свойства невидимы для компилятора и технологии IntelliSense среды Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04b6a-159">Dynamic properties are invisible to the compiler and to Visual Studio IntelliSense.</span></span>

- <span data-ttu-id="04b6a-160">Обнаруживать или оценивать во время выполнения с помощью отражения .NET.</span><span class="sxs-lookup"><span data-stu-id="04b6a-160">Discovered or accessed at run time using .NET reflection.</span></span> <span data-ttu-id="04b6a-161">Даже во время выполнения они не рассматриваются как свойства с точки зрения среды CLR.</span><span class="sxs-lookup"><span data-stu-id="04b6a-161">Even at run time, they are not properties in the basic CLR sense.</span></span>

<span data-ttu-id="04b6a-162">В C# динамические свойства можно оценивать только во время выполнения через ряд возможностей пространства имен <xref:System.ComponentModel>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-162">In C#, dynamic properties can only be accessed at run time through facilities provided by the <xref:System.ComponentModel> namespace.</span></span>

<span data-ttu-id="04b6a-163">В отличие от этого, в XML-источнике динамические свойства можно оценивать, используя простую нотацию в следующей форме:</span><span class="sxs-lookup"><span data-stu-id="04b6a-163">In contrast, however, in an XML source dynamic properties can be accessed through a straightforward notation in the following form:</span></span>

```xml
<object>.<dynamic-property>
```

<span data-ttu-id="04b6a-164">Динамические свойства для этих двух классов разрешены с получением значения, которое можно использовать напрямую, или индексатора, который необходимо представить вместе с индексом, чтобы получить значение или коллекцию значений.</span><span class="sxs-lookup"><span data-stu-id="04b6a-164">The dynamic properties for these two classes either resolve to a value that can be used directly, or to an indexer that must be supplied with an index to obtain the resulting value or collection of values.</span></span> <span data-ttu-id="04b6a-165">В последнем случае синтаксис принимает следующий вид:</span><span class="sxs-lookup"><span data-stu-id="04b6a-165">The latter syntax takes the form:</span></span>

```xml
<object>.<dynamic-property>[<index-value>]
```

<span data-ttu-id="04b6a-166">Дополнительные сведения см. в разделе [Динамические свойства LINQ to XML](linq-to-xml-dynamic-properties.md).</span><span class="sxs-lookup"><span data-stu-id="04b6a-166">For more information, see [LINQ to XML Dynamic Properties](linq-to-xml-dynamic-properties.md).</span></span>

<span data-ttu-id="04b6a-167">Для реализации динамической привязки WPF динамические свойства используются со средствами пространства имен <xref:System.Windows.Data>, особенно с классом <xref:System.Windows.Data.Binding>.</span><span class="sxs-lookup"><span data-stu-id="04b6a-167">To implement WPF dynamic binding, dynamic properties will be used with facilities provided by the <xref:System.Windows.Data> namespace, most notably the <xref:System.Windows.Data.Binding> class.</span></span>

## <a name="see-also"></a><span data-ttu-id="04b6a-168">См. также</span><span class="sxs-lookup"><span data-stu-id="04b6a-168">See also</span></span>

- [<span data-ttu-id="04b6a-169">Привязка данных WPF с помощью LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="04b6a-169">WPF Data Binding with LINQ to XML</span></span>](wpf-data-binding-with-linq-to-xml-overview.md)
- [<span data-ttu-id="04b6a-170">Динамические свойства LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="04b6a-170">LINQ to XML Dynamic Properties</span></span>](linq-to-xml-dynamic-properties.md)
- [<span data-ttu-id="04b6a-171">XAML в WPF</span><span class="sxs-lookup"><span data-stu-id="04b6a-171">XAML in WPF</span></span>](/dotnet/framework/wpf/advanced/xaml-in-wpf)
- [<span data-ttu-id="04b6a-172">Привязка данных (WPF)</span><span class="sxs-lookup"><span data-stu-id="04b6a-172">Data Binding (WPF)</span></span>](/dotnet/framework/wpf/data/data-binding-wpf)
- [<span data-ttu-id="04b6a-173">Использование разметки рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="04b6a-173">Using Workflow Markup</span></span>](https://go.microsoft.com/fwlink/?LinkId=98685)