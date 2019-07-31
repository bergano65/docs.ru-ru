---
title: Создание шаблона элемента для dotnet new — .NET Core CLI
description: Из этой статьи вы узнаете, как создать шаблон элемента для команды dotnet new. Шаблоны элементов могут содержать любое число файлов.
author: thraka
ms.date: 06/25/2019
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: c50aaf413f08c2e4cbe3f8ce8c057e5841067c92
ms.sourcegitcommit: 6472349821dbe202d01182bc2cfe9d7176eaaa6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67870613"
---
# <a name="tutorial-create-an-item-template"></a><span data-ttu-id="19080-104">Учебник. Создание шаблона элемента</span><span class="sxs-lookup"><span data-stu-id="19080-104">Tutorial: Create an item template</span></span>

<span data-ttu-id="19080-105">С помощью .NET Core вы можете создавать и развертывать шаблоны, которые генерируют проекты, файлы и даже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="19080-105">With .NET Core, you can create and deploy templates that generate projects, files, even resources.</span></span> <span data-ttu-id="19080-106">Это руководство представляет собой первую часть серии, в которой описано, как создавать, устанавливать и удалять шаблоны для использования с командой `dotnet new`.</span><span class="sxs-lookup"><span data-stu-id="19080-106">This tutorial is part one of a series that teaches you how to create, install, and uninstall, templates for use with the `dotnet new` command.</span></span>

<span data-ttu-id="19080-107">Из этой части вы узнаете, как выполнять такие задачи:</span><span class="sxs-lookup"><span data-stu-id="19080-107">In this part of the series, you'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="19080-108">создание класса для шаблона элемента;</span><span class="sxs-lookup"><span data-stu-id="19080-108">Create a class for an item template</span></span>
> * <span data-ttu-id="19080-109">создание папки и файла конфигурации шаблона;</span><span class="sxs-lookup"><span data-stu-id="19080-109">Create the template config folder and file</span></span>
> * <span data-ttu-id="19080-110">установка шаблона из файла;</span><span class="sxs-lookup"><span data-stu-id="19080-110">Install a template from a file path</span></span>
> * <span data-ttu-id="19080-111">тестирование шаблона элемента;</span><span class="sxs-lookup"><span data-stu-id="19080-111">Test an item template</span></span>
> * <span data-ttu-id="19080-112">удаление шаблона элемента.</span><span class="sxs-lookup"><span data-stu-id="19080-112">Uninstall an item template</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19080-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="19080-113">Prerequisites</span></span>

* <span data-ttu-id="19080-114">[Пакет SDK для .NET Core 2.2](https://www.microsoft.com/net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="19080-114">[.NET Core 2.2 SDK](https://www.microsoft.com/net/core) or later versions.</span></span>
* <span data-ttu-id="19080-115">Ознакомьтесь со статьей справки [Пользовательские шаблоны для команды dotnet new](../tools/custom-templates.md).</span><span class="sxs-lookup"><span data-stu-id="19080-115">Read the reference article [Custom templates for dotnet new](../tools/custom-templates.md).</span></span>

  <span data-ttu-id="19080-116">В ней приведены общие сведения о шаблонах и о способах их создания.</span><span class="sxs-lookup"><span data-stu-id="19080-116">The reference article explains the basics about templates and how they're put together.</span></span> <span data-ttu-id="19080-117">Некоторые сведения будут повторены и здесь.</span><span class="sxs-lookup"><span data-stu-id="19080-117">Some of this information will be reiterated here.</span></span>

* <span data-ttu-id="19080-118">Откройте терминал и перейдите к папке _working\templates\\_ .</span><span class="sxs-lookup"><span data-stu-id="19080-118">Open a terminal and navigate to the _working\templates\\_ folder.</span></span>

## <a name="create-the-required-folders"></a><span data-ttu-id="19080-119">Создание нужных папок</span><span class="sxs-lookup"><span data-stu-id="19080-119">Create the required folders</span></span>

<span data-ttu-id="19080-120">Для этой серии используется рабочая папка, в которой размещен источник шаблона, и тестовая папка для тестирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="19080-120">This series uses a "working folder" where your template source is contained and a "testing folder" used to test your templates.</span></span> <span data-ttu-id="19080-121">Рабочая папка и тестовая папка должны находиться в одной родительской папке.</span><span class="sxs-lookup"><span data-stu-id="19080-121">The working folder and testing folder should be under the same parent folder.</span></span>

<span data-ttu-id="19080-122">Сначала создайте родительскую папку (имя не имеет значения).</span><span class="sxs-lookup"><span data-stu-id="19080-122">First, create the parent folder, the name does not matter.</span></span> <span data-ttu-id="19080-123">Затем создайте вложенную папку с именем _working_.</span><span class="sxs-lookup"><span data-stu-id="19080-123">Then, create a subfolder named _working_.</span></span> <span data-ttu-id="19080-124">В папке _working_ создайте вложенную папку с именем _templates_.</span><span class="sxs-lookup"><span data-stu-id="19080-124">Inside of the _working_ folder, create a subfolder named _templates_.</span></span>

<span data-ttu-id="19080-125">Затем в родительской папке создайте папку с именем _test_.</span><span class="sxs-lookup"><span data-stu-id="19080-125">Next, create a folder under the parent folder named _test_.</span></span> <span data-ttu-id="19080-126">Структура папок должна выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="19080-126">The folder structure should look like the following:</span></span>

```console
parent_folder
├───test
└───working
    └───templates
```

## <a name="create-an-item-template"></a><span data-ttu-id="19080-127">Создание шаблона элемента</span><span class="sxs-lookup"><span data-stu-id="19080-127">Create an item template</span></span>

<span data-ttu-id="19080-128">Шаблон элемента — это шаблон определенного типа, который содержит один или несколько файлов.</span><span class="sxs-lookup"><span data-stu-id="19080-128">An item template is a specific type of template that contains one or more files.</span></span> <span data-ttu-id="19080-129">Шаблоны такого типа полезны, если вам нужно создать определенный объект, например файл конфигурации, кода или решения.</span><span class="sxs-lookup"><span data-stu-id="19080-129">These types of templates are useful when you want to generate something like a config, code, or solution file.</span></span> <span data-ttu-id="19080-130">В этом примере описано, как создать класс, который добавляет метод расширения в строковый тип.</span><span class="sxs-lookup"><span data-stu-id="19080-130">In this example, you'll create a class that adds an extension method to the string type.</span></span>

<span data-ttu-id="19080-131">В окне терминала перейдите к папке _working\templates\\_ и создайте вложенную папку с именем _extensions_.</span><span class="sxs-lookup"><span data-stu-id="19080-131">In your terminal, navigate to the _working\templates\\_ folder and create a new subfolder named _extensions_.</span></span> <span data-ttu-id="19080-132">Войдите в папку.</span><span class="sxs-lookup"><span data-stu-id="19080-132">Enter the folder.</span></span>

```console
working
└───templates
    └───extensions
```

<span data-ttu-id="19080-133">Создайте файл с именем _CommonExtensions.cs_ и откройте его в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="19080-133">Create a new file named _CommonExtensions.cs_ and open it with your favorite text editor.</span></span> <span data-ttu-id="19080-134">Этот класс предоставит метод расширения с именем `Reverse`, который изменяет порядок символов в строке на обратный.</span><span class="sxs-lookup"><span data-stu-id="19080-134">This class will provide an extension method named `Reverse` that reverses the contents of a string.</span></span> <span data-ttu-id="19080-135">Вставьте приведенный ниже код и сохраните файл:</span><span class="sxs-lookup"><span data-stu-id="19080-135">Paste in the following code and save the file:</span></span>

```csharp
using System;

namespace System
{
    public static class StringExtensions
    {
        public static string Reverse(this string value)
        {
            var tempArray = value.ToCharArray();
            Array.Reverse(tempArray);
            return new string(tempArray);
        }
    }
}
```

<span data-ttu-id="19080-136">Теперь, когда вы создали содержимое шаблона, необходимо создать файл конфигурации шаблона в его корневой папке.</span><span class="sxs-lookup"><span data-stu-id="19080-136">Now that you have the content of the template created, you need to create the template config at the root folder of the template.</span></span>

## <a name="create-the-template-config"></a><span data-ttu-id="19080-137">Создание конфигурации шаблона</span><span class="sxs-lookup"><span data-stu-id="19080-137">Create the template config</span></span>

<span data-ttu-id="19080-138">Шаблоны распознаются в .NET Core по специальной папке и файлу конфигурации, которые находятся в корневой папке шаблона.</span><span class="sxs-lookup"><span data-stu-id="19080-138">Templates are recognized in .NET Core by a special folder and config file that exist at the root of your template.</span></span> <span data-ttu-id="19080-139">Для этого руководства папка шаблона расположена по пути _working\templates\extensions\\_ .</span><span class="sxs-lookup"><span data-stu-id="19080-139">In this tutorial, your template folder is located at _working\templates\extensions\\_.</span></span>

<span data-ttu-id="19080-140">При создании шаблона все файлы и папки в этой папке включаются как его часть, кроме специальной папки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19080-140">When you create a template, all files and folders in the template folder are included as part of the template except for the special config folder.</span></span> <span data-ttu-id="19080-141">Эта папка конфигурации имеет имя _.template.config_.</span><span class="sxs-lookup"><span data-stu-id="19080-141">This config folder is named _.template.config_.</span></span>

<span data-ttu-id="19080-142">Сначала создайте вложенную папку с именем _.template.config_ и откройте ее.</span><span class="sxs-lookup"><span data-stu-id="19080-142">First, create a new subfolder named _.template.config_, enter it.</span></span> <span data-ttu-id="19080-143">Затем создайте файл с именем _template.json_.</span><span class="sxs-lookup"><span data-stu-id="19080-143">Then, create a new file named _template.json_.</span></span> <span data-ttu-id="19080-144">Структура папки должна быть такой:</span><span class="sxs-lookup"><span data-stu-id="19080-144">Your folder structure should look like this:</span></span>

```console
working
└───templates
    └───extensions
        └───.template.config
                template.json
```

<span data-ttu-id="19080-145">Откройте файл _template.json_ в текстовом редакторе, вставьте приведенный ниже код JSON и сохраните его:</span><span class="sxs-lookup"><span data-stu-id="19080-145">Open the _template.json_ with your favorite text editor and paste in the following JSON code and save it:</span></span>

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": [ "Common", "Code" ],
  "identity": "ExampleTemplate.StringExtensions",
  "name": "Example templates: string extensions",
  "shortName": "stringext",
  "tags": {
    "language": "C#",
    "type": "item"
  }
}
```

<span data-ttu-id="19080-146">Этот файл конфигурации содержит все параметры шаблона.</span><span class="sxs-lookup"><span data-stu-id="19080-146">This config file contains all the settings for your template.</span></span> <span data-ttu-id="19080-147">Вы можете увидеть основные параметры, например `name` и `shortName`, но здесь также присутствует параметр `tags/type` со значением `item`.</span><span class="sxs-lookup"><span data-stu-id="19080-147">You can see the basic settings, such as `name` and `shortName`, but there's also a `tags/type` value that is set to `item`.</span></span> <span data-ttu-id="19080-148">Он указывает, что ваш шаблон является шаблоном элемента.</span><span class="sxs-lookup"><span data-stu-id="19080-148">This categorizes your template as an item template.</span></span> <span data-ttu-id="19080-149">Вы можете создать шаблон любого типа.</span><span class="sxs-lookup"><span data-stu-id="19080-149">There's no restriction on the type of template you create.</span></span> <span data-ttu-id="19080-150">Значения `item` и `project` — это общие рекомендуемые имена .NET Core, которые позволяют без усилий фильтровать типы шаблонов при поиске.</span><span class="sxs-lookup"><span data-stu-id="19080-150">The `item` and `project` values are common names that .NET Core recommends so that users can easily filter the type of template they're searching for.</span></span>

<span data-ttu-id="19080-151">Элемент `classifications` представляет столбец **tags**, который отображается после запуска команды `dotnet new` и получения списка шаблонов.</span><span class="sxs-lookup"><span data-stu-id="19080-151">The `classifications` item represents the **tags** column you see when you run `dotnet new` and get a list of templates.</span></span> <span data-ttu-id="19080-152">Пользователи также могут выполнять поиск по тегам классификации.</span><span class="sxs-lookup"><span data-stu-id="19080-152">Users can also search based on classification tags.</span></span> <span data-ttu-id="19080-153">Не спутайте свойство `tags` в файле \*.json со списком тегов `classifications`.</span><span class="sxs-lookup"><span data-stu-id="19080-153">Don't confuse the `tags` property in the \*.json file with the `classifications` tags list.</span></span> <span data-ttu-id="19080-154">Это два разных элемента, которые, к сожалению, имеют одинаковые имена.</span><span class="sxs-lookup"><span data-stu-id="19080-154">They're two different things unfortunately named similarly.</span></span> <span data-ttu-id="19080-155">Полная схема файла *template.json* находится в [хранилище схем JSON](http://json.schemastore.org/template).</span><span class="sxs-lookup"><span data-stu-id="19080-155">The full schema for the *template.json* file is found at the [JSON Schema Store](http://json.schemastore.org/template).</span></span> <span data-ttu-id="19080-156">Дополнительные сведения о файле *template.json* см. на [вики-сайте о шаблонах dotnet](https://github.com/dotnet/templating/wiki).</span><span class="sxs-lookup"><span data-stu-id="19080-156">For more information about the *template.json* file, see the [dotnet templating wiki](https://github.com/dotnet/templating/wiki).</span></span>

<span data-ttu-id="19080-157">Теперь, когда у вас есть допустимый файл _.template.config/template.json_, вы можете установить шаблон.</span><span class="sxs-lookup"><span data-stu-id="19080-157">Now that you have a valid _.template.config/template.json_ file, your template is ready to be installed.</span></span> <span data-ttu-id="19080-158">В окне терминала перейдите к папке _extensions_ и выполните приведенную ниже команду, чтобы установить шаблон, расположенный в текущей папке:</span><span class="sxs-lookup"><span data-stu-id="19080-158">In your terminal, navigate to the  _extensions_ folder and run the following command to install the template located at the current folder:</span></span>

* <span data-ttu-id="19080-159">**В Windows**: `dotnet new -i .\`</span><span class="sxs-lookup"><span data-stu-id="19080-159">**On Windows**: `dotnet new -i .\`</span></span>
* <span data-ttu-id="19080-160">**В Linux или macOS**: `dotnet new -i ./`</span><span class="sxs-lookup"><span data-stu-id="19080-160">**On Linux or macOS**: `dotnet new -i ./`</span></span>

<span data-ttu-id="19080-161">Эта команда выводит список установленных шаблонов, в числе которых должен быть и ваш шаблон.</span><span class="sxs-lookup"><span data-stu-id="19080-161">This command outputs the list of templates installed, which should include yours.</span></span>

```console
C:\working\templates\extensions> dotnet new -i .\
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name            Language          Tags
-------------------------------------------------------------------------------------------------------------------------------
Example templates: string extensions              stringext             [C#]              Common/Code
Console Application                               console               [C#], F#, VB      Common/Console
Class library                                     classlib              [C#], F#, VB      Common/Library
WPF Application                                   wpf                   [C#], VB          Common/WPF
Windows Forms (WinForms) Application              winforms              [C#], VB          Common/WinForms
Worker Service                                    worker                [C#]              Common/Worker/Web
```

## <a name="test-the-item-template"></a><span data-ttu-id="19080-162">Тестирование шаблона элемента</span><span class="sxs-lookup"><span data-stu-id="19080-162">Test the item template</span></span>

<span data-ttu-id="19080-163">Теперь, когда вы установили шаблон элемента, протестируйте его.</span><span class="sxs-lookup"><span data-stu-id="19080-163">Now that you have an item template installed, test it.</span></span> <span data-ttu-id="19080-164">Перейдите к папке _test/_ и создайте консольное приложение с помощью команды `dotnet new console`.</span><span class="sxs-lookup"><span data-stu-id="19080-164">Navigate to the _test/_ folder and create a new console application with `dotnet new console`.</span></span> <span data-ttu-id="19080-165">При этом будет создан рабочий проект, который вы можете с легкостью протестировать, выполнив команду `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="19080-165">This generates a working project you can easily test with the `dotnet run` command.</span></span>

```console
C:\test> dotnet new console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\test\test.csproj...
  Restore completed in 54.82 ms for C:\test\test.csproj.

Restore succeeded.
```

```console
C:\test> dotnet run
Hello World!
```

<span data-ttu-id="19080-166">Затем выполните команду `dotnet new stringext`, чтобы создать файл _CommonExtensions.cs_ из шаблона.</span><span class="sxs-lookup"><span data-stu-id="19080-166">Next, run `dotnet new stringext` to generate the _CommonExtensions.cs_ from the template.</span></span>

```console
C:\test> dotnet new stringext
The template "Example templates: string extensions" was created successfully.
```

<span data-ttu-id="19080-167">Измените код в файле _Program.cs_, чтобы поменять порядок символов в строке `"Hello World"` на обратный с помощью метода расширения, предоставляемого шаблоном.</span><span class="sxs-lookup"><span data-stu-id="19080-167">Change the code in _Program.cs_ to reverse the `"Hello World"` string with the extension method provided by the template.</span></span>

```csharp
Console.WriteLine("Hello World!".Reverse());
```

<span data-ttu-id="19080-168">Запустите программу еще раз, и вы увидите, что порядок символов изменен на обратный.</span><span class="sxs-lookup"><span data-stu-id="19080-168">Run the program again and you'll see that the result is reversed.</span></span>

```console
C:\test> dotnet run
!dlroW olleH
```

<span data-ttu-id="19080-169">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="19080-169">Congratulations!</span></span> <span data-ttu-id="19080-170">Вы создали и развернули шаблон элемента с помощью .NET Core.</span><span class="sxs-lookup"><span data-stu-id="19080-170">You created and deployed an item template with .NET Core.</span></span> <span data-ttu-id="19080-171">Для подготовки к следующей части этой серии руководств вам необходимо удалить созданный шаблон.</span><span class="sxs-lookup"><span data-stu-id="19080-171">In preparation for the next part of this tutorial series, you must uninstall the template you created.</span></span> <span data-ttu-id="19080-172">Также обязательно удалите все файлы из папки _test_.</span><span class="sxs-lookup"><span data-stu-id="19080-172">Make sure to delete all files from the _test_ folder too.</span></span> <span data-ttu-id="19080-173">Так вы воссоздадите первоначальные условия для работы со следующим разделом этого руководства.</span><span class="sxs-lookup"><span data-stu-id="19080-173">This will get you back to a clean state ready for the next major section of this tutorial.</span></span>

## <a name="uninstall-the-template"></a><span data-ttu-id="19080-174">Удаление шаблона</span><span class="sxs-lookup"><span data-stu-id="19080-174">Uninstall the template</span></span>

<span data-ttu-id="19080-175">Так как вы установили шаблон с указанием пути к файлу, вам нужно удалить его, указав **абсолютный** путь к файлу.</span><span class="sxs-lookup"><span data-stu-id="19080-175">Because you installed the template by file path, you must uninstall it with the **absolute** file path.</span></span> <span data-ttu-id="19080-176">Вы можете просмотреть список всех установленных шаблонов, выполнив команду `dotnet new -u`.</span><span class="sxs-lookup"><span data-stu-id="19080-176">You can see a list of templates installed by running the `dotnet new -u` command.</span></span> <span data-ttu-id="19080-177">Ваш шаблон должен быть последним в списке.</span><span class="sxs-lookup"><span data-stu-id="19080-177">Your template should be listed last.</span></span> <span data-ttu-id="19080-178">Удалите шаблон с помощью команды `dotnet new -u <ABSOLUTE PATH TO TEMPLATE DIRECTORY>`, указав путь к нему.</span><span class="sxs-lookup"><span data-stu-id="19080-178">Use the path listed to uninstall your template with the `dotnet new -u <ABSOLUTE PATH TO TEMPLATE DIRECTORY>` command.</span></span>

```console
C:\working> dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:
  Microsoft.DotNet.Common.ItemTemplates
    Templates:
      dotnet gitignore file (gitignore)
      global.json file (globaljson)
      NuGet Config (nugetconfig)
      Solution File (sln)
      Dotnet local tool manifest file (tool-manifest)
      Web Config (webconfig)

... cut to save space ...

  NUnit3.DotNetNew.Template
    Templates:
      NUnit 3 Test Project (nunit) C#
      NUnit 3 Test Item (nunit-test) C#
      NUnit 3 Test Project (nunit) F#
      NUnit 3 Test Item (nunit-test) F#
      NUnit 3 Test Project (nunit) VB
      NUnit 3 Test Item (nunit-test) VB
  C:\working\templates\extensions
    Templates:
      Example templates: string extensions (stringext) C#
```

```console
C:\working> dotnet new -u C:\working\templates\extensions
```

## <a name="next-steps"></a><span data-ttu-id="19080-179">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="19080-179">Next steps</span></span>

<span data-ttu-id="19080-180">С помощью этого руководства вы создали шаблон элемента.</span><span class="sxs-lookup"><span data-stu-id="19080-180">In this tutorial, you created an item template.</span></span> <span data-ttu-id="19080-181">Чтобы узнать, как создать шаблон проекта, перейдите к следующей части этой серии.</span><span class="sxs-lookup"><span data-stu-id="19080-181">To learn how to create a project template, continue this tutorial series.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="19080-182">Создание шаблона проекта</span><span class="sxs-lookup"><span data-stu-id="19080-182">Create a project template</span></span>](cli-templates-create-project-template.md)