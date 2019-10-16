---
title: Пошаговое руководство. Создание пользовательских исключений с локализованными сообщениями об исключениях
description: Узнайте, как создавать пользовательские исключения с локализованными сообщениями.
author: Youssef1313
ms.author: ronpet
ms.date: 09/13/2019
ms.openlocfilehash: b4aa567fccda9354bc5959d6b9838d678d53abef
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2019
ms.locfileid: "71696706"
---
# <a name="how-to-create-user-defined-exceptions-with-localized-exception-messages"></a><span data-ttu-id="323ed-103">Пошаговое руководство. Создание пользовательских исключений с локализованными сообщениями об исключениях</span><span class="sxs-lookup"><span data-stu-id="323ed-103">How to: create user-defined exceptions with localized exception messages</span></span>

<span data-ttu-id="323ed-104">Из этой статьи вы узнаете, как создавать пользовательские исключения, унаследованные от базового класса <xref:System.Exception>, с локализованными сообщениями о них с использованием вспомогательных сборок.</span><span class="sxs-lookup"><span data-stu-id="323ed-104">In this article, you will learn how to create user-defined exceptions that are inherited from the base <xref:System.Exception> class with localized exception messages using satellite assemblies.</span></span>

## <a name="create-custom-exceptions"></a><span data-ttu-id="323ed-105">Создание пользовательских исключений</span><span class="sxs-lookup"><span data-stu-id="323ed-105">Create custom exceptions</span></span>

<span data-ttu-id="323ed-106">.NET содержит множество различных исключений, которые можно использовать.</span><span class="sxs-lookup"><span data-stu-id="323ed-106">.NET contains many different exceptions that you can use.</span></span> <span data-ttu-id="323ed-107">Но в некоторых случаях, когда ни одно из них не соответствует вашим потребностям, вы можете создавать собственные пользовательские исключения.</span><span class="sxs-lookup"><span data-stu-id="323ed-107">However, in some cases when none of them meets your needs, you can create your own custom exceptions.</span></span>

<span data-ttu-id="323ed-108">Предположим, что нужно создать `StudentNotFoundException`, содержащее свойство `StudentName`.</span><span class="sxs-lookup"><span data-stu-id="323ed-108">Let's assume you want to create a `StudentNotFoundException` that contains a `StudentName` property.</span></span>
<span data-ttu-id="323ed-109">Чтобы создать пользовательское исключение, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="323ed-109">To create a custom exception, follow these steps:</span></span>

1. <span data-ttu-id="323ed-110">Создайте сериализуемый класс, который наследует от <xref:System.Exception>.</span><span class="sxs-lookup"><span data-stu-id="323ed-110">Create a serializable class that inherits from <xref:System.Exception>.</span></span> <span data-ttu-id="323ed-111">Имя класса должно заканчиваться на Exception:</span><span class="sxs-lookup"><span data-stu-id="323ed-111">The class name should end in "Exception":</span></span>

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception { }
    ```

1. <span data-ttu-id="323ed-112">Добавьте конструкторы по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="323ed-112">Add the default constructors:</span></span>

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }
    }
    ```

1. <span data-ttu-id="323ed-113">Определите любые дополнительные свойства и конструкторы:</span><span class="sxs-lookup"><span data-stu-id="323ed-113">Define any additional properties and constructors:</span></span>

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public string StudentName { get; }

        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }

        public StudentNotFoundException(string message, string studentName)
            : this(message)
        {
            StudentName = studentName;
        }
    }
    ```

## <a name="create-localized-exception-messages"></a><span data-ttu-id="323ed-114">Создание локализованных сообщений об исключениях</span><span class="sxs-lookup"><span data-stu-id="323ed-114">Create localized exception messages</span></span>

<span data-ttu-id="323ed-115">Вы создали пользовательское исключение и можете вызывать его где угодно с помощью кода, подобного следующему:</span><span class="sxs-lookup"><span data-stu-id="323ed-115">You have created a custom exception, and you can throw it anywhere with code like the following:</span></span>

```csharp
throw new StudentNotFoundException("The student cannot be found.", "John");
```

<span data-ttu-id="323ed-116">Проблема с предыдущей строкой заключается в том, что `"The student cannot be found."` — это просто константная строка.</span><span class="sxs-lookup"><span data-stu-id="323ed-116">The problem with the previous line is that `"The student cannot be found."` is just a constant string.</span></span> <span data-ttu-id="323ed-117">В локализованном приложении вам необходимо иметь разные сообщения в зависимости от языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="323ed-117">In a localized application, you want to have different messages depending on user culture.</span></span>
<span data-ttu-id="323ed-118">Это можно сделать с помощью [вспомогательных сборок](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md).</span><span class="sxs-lookup"><span data-stu-id="323ed-118">[Satellite Assemblies](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md) are a good way to do that.</span></span> <span data-ttu-id="323ed-119">Вспомогательная сборка — это библиотека DLL, содержащая ресурсы для определенного языка.</span><span class="sxs-lookup"><span data-stu-id="323ed-119">A satellite assembly is a .dll that contains resources for a specific language.</span></span> <span data-ttu-id="323ed-120">При запросе конкретных ресурсов во время выполнения среда CLR находит этот ресурс в зависимости от языка и региональных параметров пользователя.</span><span class="sxs-lookup"><span data-stu-id="323ed-120">When you ask for a specific resources at run time, the CLR finds that resource depending on user culture.</span></span> <span data-ttu-id="323ed-121">Если вспомогательная сборка не найдена для этого языка и региональных параметров, используются ресурсы языка и региональных параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="323ed-121">If no satellite assembly is found for that culture, the resources of the default culture are used.</span></span>

<span data-ttu-id="323ed-122">Чтобы создать локализованные сообщения об исключениях:</span><span class="sxs-lookup"><span data-stu-id="323ed-122">To create the localized exception messages:</span></span>

1. <span data-ttu-id="323ed-123">Создайте папку с именем *Ресурсы* для хранения файлов ресурсов.</span><span class="sxs-lookup"><span data-stu-id="323ed-123">Create a new folder named *Resources* to hold the resource files.</span></span>
1. <span data-ttu-id="323ed-124">Добавьте в нее новый файл ресурсов.</span><span class="sxs-lookup"><span data-stu-id="323ed-124">Add a new resource file to it.</span></span> <span data-ttu-id="323ed-125">Для этого в Visual Studio щелкните правой кнопкой мыши папку в **обозревателе решений** и выберите **Добавить** > **Новый элемент** > **Файл ресурсов**.</span><span class="sxs-lookup"><span data-stu-id="323ed-125">To do that in Visual Studio, right-click the folder in **Solution Explorer**, and select **Add** > **New Item** > **Resources File**.</span></span> <span data-ttu-id="323ed-126">Присвойте файлу имя *ExceptionMessages.resx*.</span><span class="sxs-lookup"><span data-stu-id="323ed-126">Name the file *ExceptionMessages.resx*.</span></span> <span data-ttu-id="323ed-127">Это файл ресурсов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="323ed-127">This is the default resources file.</span></span>
1. <span data-ttu-id="323ed-128">Добавьте пару "имя/значение" для сообщения об исключении, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="323ed-128">Add a name/value pair for your exception message, like the following image shows:</span></span>

   ![Добавление ресурсов в язык и региональные параметры по умолчанию](media/add-resources-to-default-culture.jpg)

1. <span data-ttu-id="323ed-130">Добавьте новый файл ресурсов для французского языка.</span><span class="sxs-lookup"><span data-stu-id="323ed-130">Add a new resource file for French.</span></span> <span data-ttu-id="323ed-131">Присвойте ему имя *ExceptionMessages.fr-FR.resx*.</span><span class="sxs-lookup"><span data-stu-id="323ed-131">Name it *ExceptionMessages.fr-FR.resx*.</span></span>
1. <span data-ttu-id="323ed-132">Снова добавьте пару "имя/значение" для сообщения об исключении, но со значением французского языка:</span><span class="sxs-lookup"><span data-stu-id="323ed-132">Add a name/value pair for the exception message again, but with a French value:</span></span>

   ![Добавление ресурсов в язык и региональные параметры fr-FR](media/add-resources-to-fr-culture.jpg)

1. <span data-ttu-id="323ed-134">После сборки проекта папка выходных данных сборки должна содержать папку *fr-FR* с файлом *DLL*, который является вспомогательной сборкой.</span><span class="sxs-lookup"><span data-stu-id="323ed-134">After you build the project, the build output folder should contain the *fr-FR* folder with a *.dll* file, which is the satellite assembly.</span></span>
1. <span data-ttu-id="323ed-135">Исключение вызывается с помощью кода, подобного приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="323ed-135">You throw the exception with code like the following:</span></span>

    ```csharp
    var resourceManager = new ResourceManager("FULLY_QIALIFIED_NAME_OF_RESOURCE_FILE", Assembly.GetExecutingAssembly());
    throw new StudentNotFoundException(resourceManager.GetString("StudentNotFound"), "John");
    ```

  > [!NOTE]
  > <span data-ttu-id="323ed-136">Если имя проекта — `TestProject`, а файл ресурсов *ExceptionMessages.resx* находится в папке *Ресурсы* проекта, полное имя файла ресурсов — `TestProject.Resources.ExceptionMessages`.</span><span class="sxs-lookup"><span data-stu-id="323ed-136">If the project name is `TestProject` and the resource file *ExceptionMessages.resx* resides in the *Resources* folder of the project, the fully qualified name of the resource file is `TestProject.Resources.ExceptionMessages`.</span></span>

## <a name="see-also"></a><span data-ttu-id="323ed-137">См. также</span><span class="sxs-lookup"><span data-stu-id="323ed-137">See also</span></span>

- [<span data-ttu-id="323ed-138">Как создать пользовательские исключения</span><span class="sxs-lookup"><span data-stu-id="323ed-138">How to create user-defined exceptions</span></span>](how-to-create-user-defined-exceptions.md)
- <span data-ttu-id="323ed-139">[Creating Satellite Assemblies for Desktop Apps](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md) (Создание вспомогательных сборок для классических приложений)</span><span class="sxs-lookup"><span data-stu-id="323ed-139">[Creating Satellite Assemblies for Desktop Apps](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)</span></span>
- [<span data-ttu-id="323ed-140">base (справочник по C#)</span><span class="sxs-lookup"><span data-stu-id="323ed-140">base (C# Reference)</span></span>](../../csharp/language-reference/keywords/base.md)
- [<span data-ttu-id="323ed-141">this (справочник по C#)</span><span class="sxs-lookup"><span data-stu-id="323ed-141">this (C# Reference)</span></span>](../../csharp/language-reference/keywords/this.md)