---
title: Использование ML.NET для сценариев многоклассовой классификации задач GitHub
description: Узнайте, как использовать ML.NET для сценариев многоклассовой классификации, чтобы назначать задачи GitHub определенным областям.
ms.date: 01/24/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 6f01357906fd4398f68dadfb35dbce816f4302c0
ms.sourcegitcommit: d9a0071d0fd490ae006c816f78a563b9946e269a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2019
ms.locfileid: "55066211"
---
# <a name="tutorial-use-mlnet-in-a-multiclass-classification-scenario-to-classify-github-issues"></a><span data-ttu-id="019e1-103">Учебник. Использование ML.NET для сценариев многоклассовой классификации задач GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-103">Tutorial: Use ML.NET in a multiclass classification scenario to classify GitHub issues.</span></span>

<span data-ttu-id="019e1-104">В этом практическом руководстве демонстрируется создание классификатора задач GitHub с помощью ML.NET в консольном приложении .NET Core на языке C# в Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="019e1-104">This sample tutorial illustrates using ML.NET to create a GitHub issue classifier via a .NET Core console application using C# in Visual Studio 2017.</span></span>

<span data-ttu-id="019e1-105">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="019e1-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="019e1-106">Определение проблемы</span><span class="sxs-lookup"><span data-stu-id="019e1-106">Understand the problem</span></span>
> * <span data-ttu-id="019e1-107">Выбор подходящей задачи машинного обучения</span><span class="sxs-lookup"><span data-stu-id="019e1-107">Select the appropriate machine learning task</span></span>
> * <span data-ttu-id="019e1-108">подготавливать данные;</span><span class="sxs-lookup"><span data-stu-id="019e1-108">Prepare your data</span></span>
> * <span data-ttu-id="019e1-109">создавать конвейеры обучения;</span><span class="sxs-lookup"><span data-stu-id="019e1-109">Create the learning pipeline</span></span>
> * <span data-ttu-id="019e1-110">загружать классификатор;</span><span class="sxs-lookup"><span data-stu-id="019e1-110">Load a classifier</span></span>
> * <span data-ttu-id="019e1-111">обучить модель;</span><span class="sxs-lookup"><span data-stu-id="019e1-111">Train the model</span></span>
> * <span data-ttu-id="019e1-112">проводить оценку модели по другому набору данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-112">Evaluate the model with a different dataset</span></span>
> * <span data-ttu-id="019e1-113">прогнозировать единый экземпляр результатов тестовых данных с помощью модели;</span><span class="sxs-lookup"><span data-stu-id="019e1-113">Predict a single instance of test data outcome with the model</span></span>
> * <span data-ttu-id="019e1-114">прогнозировать результаты для тестовых данных с помощью загруженной модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-114">Predict the test data outcomes with a loaded model</span></span>

> [!NOTE]
> <span data-ttu-id="019e1-115">В этом разделе описано, как использовать платформу ML.NET, которая сейчас доступна в режиме предварительной версии. Этот материал может быть изменен.</span><span class="sxs-lookup"><span data-stu-id="019e1-115">This topic refers to ML.NET, which is currently in Preview, and material may be subject to change.</span></span> <span data-ttu-id="019e1-116">Дополнительные сведения см. в [обзоре ML.NET](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span><span class="sxs-lookup"><span data-stu-id="019e1-116">For more information, visit [the ML.NET introduction](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span></span>

## <a name="github-issue-sample-overview"></a><span data-ttu-id="019e1-117">Обзор примера задачи GitHub</span><span class="sxs-lookup"><span data-stu-id="019e1-117">GitHub issue sample overview</span></span>

<span data-ttu-id="019e1-118">Пример представляет собой консольное приложение, использующее ML.NET для обучения модели, которая классифицирует и прогнозирует метку области для задачи GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-118">The sample is a console app that uses ML.NET to train a model that classifies and predicts the Area label for a GitHub issue.</span></span> <span data-ttu-id="019e1-119">Также оно оценивает качество модели по второму набору данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-119">It also evaluates the model with a second dataset for quality analysis.</span></span> <span data-ttu-id="019e1-120">Наборы данных для задачи взяты из GitHub-репозитория dotnet/corefx.</span><span class="sxs-lookup"><span data-stu-id="019e1-120">The issue datasets are from the dotnet/corefx GitHub repo.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="019e1-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="019e1-121">Prerequisites</span></span>

* <span data-ttu-id="019e1-122">[Visual Studio 2017 15.6 или более поздней версии](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) с установленной рабочей нагрузкой "Кроссплатформенная разработка .NET Core".</span><span class="sxs-lookup"><span data-stu-id="019e1-122">[Visual Studio 2017 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the ".NET Core cross-platform development" workload installed.</span></span>

* <span data-ttu-id="019e1-123">[Файл задач GitHub с разделением знаками табуляции (issues_train.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv).</span><span class="sxs-lookup"><span data-stu-id="019e1-123">The [Github issues tab separated file (issues_train.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv).</span></span>
* <span data-ttu-id="019e1-124">[Файл с тестовыми данными задач GitHub с разделением знаками табуляции (issues_test.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv).</span><span class="sxs-lookup"><span data-stu-id="019e1-124">The [Github issues test tab separated file (issues_test.tsv)](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv).</span></span>

## <a name="machine-learning-workflow"></a><span data-ttu-id="019e1-125">Рабочий процесс машинного обучения</span><span class="sxs-lookup"><span data-stu-id="019e1-125">Machine learning workflow</span></span>

<span data-ttu-id="019e1-126">В этом руководстве используется рабочий процесс машинного обучения, который определяет порядок выполнения действий в процессе.</span><span class="sxs-lookup"><span data-stu-id="019e1-126">This tutorial follows a machine learning workflow that enables the process to move in an orderly fashion.</span></span>

<span data-ttu-id="019e1-127">Вот основные этапы этого рабочего процесса:</span><span class="sxs-lookup"><span data-stu-id="019e1-127">The workflow phases are as follows:</span></span>

1. <span data-ttu-id="019e1-128">**определение проблемы**;</span><span class="sxs-lookup"><span data-stu-id="019e1-128">**Understand the problem**</span></span>
2. <span data-ttu-id="019e1-129">**Подготовка данных**.</span><span class="sxs-lookup"><span data-stu-id="019e1-129">**Prepare your data**</span></span>
   * <span data-ttu-id="019e1-130">**Загрузка данных**.</span><span class="sxs-lookup"><span data-stu-id="019e1-130">**Load the data**</span></span>
   * <span data-ttu-id="019e1-131">**Извлечение компонентов (преобразование данных)**.</span><span class="sxs-lookup"><span data-stu-id="019e1-131">**Extract features (Transform your data)**</span></span>
3. <span data-ttu-id="019e1-132">**Сборка и обучение**.</span><span class="sxs-lookup"><span data-stu-id="019e1-132">**Build and train**</span></span> 
   * <span data-ttu-id="019e1-133">**Обучение модели**.</span><span class="sxs-lookup"><span data-stu-id="019e1-133">**Train the model**</span></span>
   * <span data-ttu-id="019e1-134">**оценка модели**;</span><span class="sxs-lookup"><span data-stu-id="019e1-134">**Evaluate the model**</span></span>
4. <span data-ttu-id="019e1-135">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="019e1-135">**Run**</span></span>
   * <span data-ttu-id="019e1-136">**Потребление модели**.</span><span class="sxs-lookup"><span data-stu-id="019e1-136">**Model consumption**</span></span>

### <a name="understand-the-problem"></a><span data-ttu-id="019e1-137">Определение проблемы</span><span class="sxs-lookup"><span data-stu-id="019e1-137">Understand the problem</span></span>

<span data-ttu-id="019e1-138">Прежде всего необходимо понять суть проблемы, что позволит разбить ее на отдельные фрагменты для построения и обучения модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-138">You first need to understand the problem, so you can break it down to parts that can support building and training the model.</span></span> <span data-ttu-id="019e1-139">Разделение проблемы на составляющие позволяет прогнозировать и оценивать результаты.</span><span class="sxs-lookup"><span data-stu-id="019e1-139">Breaking  down the problem allows you to predict and evaluate the results.</span></span>

<span data-ttu-id="019e1-140">Рассматриваемая в этом руководстве проблема: определить, к какой области относятся входящие задачи GitHub, чтобы корректно помечать их для дальнейшей приоритизации и планирования.</span><span class="sxs-lookup"><span data-stu-id="019e1-140">The problem for this tutorial is to understand what area incoming GitHub issues belong to in order to label them correctly for prioritization and scheduling.</span></span>

<span data-ttu-id="019e1-141">Проблему можно разделить на следующие части:</span><span class="sxs-lookup"><span data-stu-id="019e1-141">You can break down the problem to the following:</span></span>

* <span data-ttu-id="019e1-142">текст названия задачи;</span><span class="sxs-lookup"><span data-stu-id="019e1-142">the issue title text</span></span>
* <span data-ttu-id="019e1-143">текст описания задачи;</span><span class="sxs-lookup"><span data-stu-id="019e1-143">the issue description text</span></span>
* <span data-ttu-id="019e1-144">значение области для обучающих данных модели;</span><span class="sxs-lookup"><span data-stu-id="019e1-144">an area value for the model training data</span></span>
* <span data-ttu-id="019e1-145">прогнозируемое значение области, которое можно оценить и затем использовать на практике.</span><span class="sxs-lookup"><span data-stu-id="019e1-145">a predicted area value that you can evaluate and then use operationally</span></span>

<span data-ttu-id="019e1-146">После этого нужно **определить** область, что поможет выбрать задачу машинного обучения.</span><span class="sxs-lookup"><span data-stu-id="019e1-146">You then need to **determine** the area, which helps you with the machine learning task selection.</span></span>

## <a name="select-the-appropriate-machine-learning-task"></a><span data-ttu-id="019e1-147">Выбор подходящей задачи машинного обучения</span><span class="sxs-lookup"><span data-stu-id="019e1-147">Select the appropriate machine learning task</span></span>

<span data-ttu-id="019e1-148">Для этой проблемы вам известны следующие факты.</span><span class="sxs-lookup"><span data-stu-id="019e1-148">With this problem, you know the following facts:</span></span>

<span data-ttu-id="019e1-149">Обучающие данные</span><span class="sxs-lookup"><span data-stu-id="019e1-149">Training data:</span></span>

<span data-ttu-id="019e1-150">Задачам GitHub можно присваивать метки в нескольких областях (**Area**), как в следующих примерах:</span><span class="sxs-lookup"><span data-stu-id="019e1-150">GitHub issues can be labeled in several areas (**Area**) as in the following examples:</span></span>

* <span data-ttu-id="019e1-151">area-System.Numerics</span><span class="sxs-lookup"><span data-stu-id="019e1-151">area-System.Numerics</span></span>
* <span data-ttu-id="019e1-152">area-System.Xml</span><span class="sxs-lookup"><span data-stu-id="019e1-152">area-System.Xml</span></span>
* <span data-ttu-id="019e1-153">area-Infrastructure</span><span class="sxs-lookup"><span data-stu-id="019e1-153">area-Infrastructure</span></span>
* <span data-ttu-id="019e1-154">area-System.Linq</span><span class="sxs-lookup"><span data-stu-id="019e1-154">area-System.Linq</span></span>
* <span data-ttu-id="019e1-155">area-System.IO</span><span class="sxs-lookup"><span data-stu-id="019e1-155">area-System.IO</span></span>

<span data-ttu-id="019e1-156">Вы можете спрогнозировать **область** новой задачи GitHub, как в следующих примерах:</span><span class="sxs-lookup"><span data-stu-id="019e1-156">Predict the **Area** of a new GitHub Issue such as in the following examples:</span></span>

* <span data-ttu-id="019e1-157">Contract.Assert и Debug.Assert</span><span class="sxs-lookup"><span data-stu-id="019e1-157">Contract.Assert vs Debug.Assert</span></span>
* <span data-ttu-id="019e1-158">Защита полей от записи в System.Xml</span><span class="sxs-lookup"><span data-stu-id="019e1-158">Make fields readonly in System.Xml</span></span>

<span data-ttu-id="019e1-159">Для этого сценария лучше всего подходит задача классификации в машинном обучении.</span><span class="sxs-lookup"><span data-stu-id="019e1-159">The classification machine learning task is best suited for this scenario.</span></span>

### <a name="about-the-classification-task"></a><span data-ttu-id="019e1-160">Сведения о задаче классификации</span><span class="sxs-lookup"><span data-stu-id="019e1-160">About the classification task</span></span>

<span data-ttu-id="019e1-161">Классификацией называют задачу машинного обучения, которая использует данные для **определения** категории, типа или класса для некоторого элемента или ряда данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-161">Classification is a machine learning task that uses data to **determine** the category, type, or class of an item or row of data.</span></span> <span data-ttu-id="019e1-162">Например, классификацию можно использовать для следующих действий:</span><span class="sxs-lookup"><span data-stu-id="019e1-162">For example, you can use classification to:</span></span>

* <span data-ttu-id="019e1-163">определение положительной или отрицательной тональности;</span><span class="sxs-lookup"><span data-stu-id="019e1-163">Identify sentiment as positive or negative.</span></span>
* <span data-ttu-id="019e1-164">сортировка сообщений электронной почты на спам, нежелательную почту и нужные сообщения;</span><span class="sxs-lookup"><span data-stu-id="019e1-164">Classify email as spam, junk, or good.</span></span>
* <span data-ttu-id="019e1-165">диагностика раковых заболеваний по лабораторным пробам, взятых у пациентов;</span><span class="sxs-lookup"><span data-stu-id="019e1-165">Determine whether a patient's lab sample is cancerous.</span></span>
* <span data-ttu-id="019e1-166">распределение клиентов по категориям с разной готовностью реагировать на рекламную акцию.</span><span class="sxs-lookup"><span data-stu-id="019e1-166">Categorize customers by their propensity to respond to a sales campaign.</span></span>

<span data-ttu-id="019e1-167">Задачи классификации обычно относятся к одному из следующих типов.</span><span class="sxs-lookup"><span data-stu-id="019e1-167">Classification tasks are frequently one of the following types:</span></span>

* <span data-ttu-id="019e1-168">Двоичная: A или B.</span><span class="sxs-lookup"><span data-stu-id="019e1-168">Binary: either A or B.</span></span>
* <span data-ttu-id="019e1-169">Многоклассовая: несколько категорий, которые прогнозируются по одной модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-169">Multiclass: multiple categories that can be predicted by using a single model.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="019e1-170">Создание консольного приложения</span><span class="sxs-lookup"><span data-stu-id="019e1-170">Create a console application</span></span>

### <a name="create-a-project"></a><span data-ttu-id="019e1-171">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="019e1-171">Create a project</span></span>

1. <span data-ttu-id="019e1-172">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="019e1-172">Open Visual Studio 2017.</span></span> <span data-ttu-id="019e1-173">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="019e1-173">Select **File** > **New** > **Project** from the menu bar.</span></span> <span data-ttu-id="019e1-174">В диалоговом окне **Новый проект** выберите узел **Visual C#**, а затем — узел **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="019e1-174">In the **New Project** dialog, select the **Visual C#** node followed by the **.NET Core** node.</span></span> <span data-ttu-id="019e1-175">Выберите шаблон проекта **Консольное приложение (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="019e1-175">Then select the **Console App (.NET Core)** project template.</span></span> <span data-ttu-id="019e1-176">В текстовом поле **Имя** введите "Анализ тональности" и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="019e1-176">In the **Name** text box, type "SentimentAnalysis" and then select the **OK** button.</span></span>

2. <span data-ttu-id="019e1-177">Создайте каталог с именем *Data* в папке проекта, чтобы сохранить в нем файлы с наборами данных:</span><span class="sxs-lookup"><span data-stu-id="019e1-177">Create a directory named *Data* in your project to save your data set files:</span></span>

    <span data-ttu-id="019e1-178">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="019e1-178">In **Solution Explorer**, right-click on your project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="019e1-179">Введите имя папки Data и нажмите клавишу "ВВОД".</span><span class="sxs-lookup"><span data-stu-id="019e1-179">Type "Data" and hit Enter.</span></span>

3. <span data-ttu-id="019e1-180">Установите **пакет NuGet для Microsoft.ML**:</span><span class="sxs-lookup"><span data-stu-id="019e1-180">Install the **Microsoft.ML NuGet Package**:</span></span>

    <span data-ttu-id="019e1-181">В обозревателе решений щелкните проект правой кнопкой мыши и выберите **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="019e1-181">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="019e1-182">Выберите nuget.org в качестве источника пакетов, перейдите на вкладку "Обзор", выполните поиск по фразе **Microsoft.ML**, выберите из списка этот пакет и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="019e1-182">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML**, select that package in the list, and select the **Install** button.</span></span> <span data-ttu-id="019e1-183">Нажмите кнопку **ОК** в диалоговом окне **Предварительный просмотр изменений**, а затем нажмите кнопку **Принимаю** в диалоговом окне **Принятие условий лицензионного соглашения**, если вы согласны с указанными условиями лицензионного соглашения для выбранных пакетов.</span><span class="sxs-lookup"><span data-stu-id="019e1-183">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

### <a name="prepare-your-data"></a><span data-ttu-id="019e1-184">подготавливать данные;</span><span class="sxs-lookup"><span data-stu-id="019e1-184">Prepare your data</span></span>

1. <span data-ttu-id="019e1-185">Скачайте наборы данных [issues_train.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) и [issues_test.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) и сохраните их в ранее созданную папку *Data*.</span><span class="sxs-lookup"><span data-stu-id="019e1-185">Download the [issues_train.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) and the [issues_test.tsv](https://github.com/dotnet/samples/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) data sets and save them to the *Data* folder previously created.</span></span> <span data-ttu-id="019e1-186">Первый из этих наборов данных обучает модель машинного обучения, а второй позволяет оценить точность полученной модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-186">The first dataset trains the machine learning model and the second can be used to evaluate how accurate your model is.</span></span>

2. <span data-ttu-id="019e1-187">В обозревателе решений щелкните правой кнопкой мыши каждый из файлов \*.tsv и выберите **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="019e1-187">In Solution Explorer, right-click each of the \*.tsv files and select **Properties**.</span></span> <span data-ttu-id="019e1-188">В разделе **Дополнительно** для параметра **Копировать в выходной каталог** установите значение **Копировать более позднюю версию**.</span><span class="sxs-lookup"><span data-stu-id="019e1-188">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="019e1-189">Создание классов и определение путей</span><span class="sxs-lookup"><span data-stu-id="019e1-189">Create classes and define paths</span></span>

<span data-ttu-id="019e1-190">Добавьте следующие новые операторы `using` в начало файла *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="019e1-190">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

[!code-csharp[AddUsings](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddUsings)]

<span data-ttu-id="019e1-191">Нужно создать три глобальных поля для хранения путей к недавно скачанным файлам и глобальную переменную для `TextLoader`:</span><span class="sxs-lookup"><span data-stu-id="019e1-191">You need to create three global fields to hold the paths to the recently downloaded files, and a global variable for the `TextLoader`:</span></span>

* <span data-ttu-id="019e1-192">`_trainDataPath` содержит путь к набору данных, используемому для обучения модели;</span><span class="sxs-lookup"><span data-stu-id="019e1-192">`_trainDataPath` has the path to the dataset used to train the model.</span></span>
* <span data-ttu-id="019e1-193">`_testDataPath` содержит путь к набору данных, используемому для оценки модели;</span><span class="sxs-lookup"><span data-stu-id="019e1-193">`_testDataPath` has the path to the dataset used to evaluate the model.</span></span>
* <span data-ttu-id="019e1-194">`_modelPath` содержит путь, по которому сохраняется обученная модель;</span><span class="sxs-lookup"><span data-stu-id="019e1-194">`_modelPath` has the path where the trained model is saved.</span></span>
* <span data-ttu-id="019e1-195">`_mlContext` соответствует классу <xref:Microsoft.ML.MLContext>, предоставляющему контекст для обработки;</span><span class="sxs-lookup"><span data-stu-id="019e1-195">`_mlContext` is the <xref:Microsoft.ML.MLContext> that provides processing context.</span></span>
* <span data-ttu-id="019e1-196">`_trainingDataView` представляет собой интерфейс <xref:Microsoft.ML.Data.IDataView>, используемый для обработки обучающего набора данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-196">`_trainingDataView` is the <xref:Microsoft.ML.Data.IDataView> used to process the training dataset.</span></span>
* <span data-ttu-id="019e1-197">`_predEngine` соответствует классу <xref:Microsoft.ML.PredictionEngine%602>, используемому для одиночных прогнозов;</span><span class="sxs-lookup"><span data-stu-id="019e1-197">`_predEngine` is the <xref:Microsoft.ML.PredictionEngine%602> used for single predictions.</span></span>
* <span data-ttu-id="019e1-198">`_reader` соответствует классу <xref:Microsoft.ML.Data.TextLoader>, используемому для загрузки и преобразования наборов данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-198">`_reader` is the <xref:Microsoft.ML.Data.TextLoader> used to load and transform the datasets.</span></span>

<span data-ttu-id="019e1-199">Добавьте следующий код в строку непосредственно над методом `Main`, чтобы указать эти пути и другие переменные:</span><span class="sxs-lookup"><span data-stu-id="019e1-199">Add the following code to the line right above the `Main` method to specify those paths and the other variables:</span></span>

[!code-csharp[DeclareGlobalVariables](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DeclareGlobalVariables)]

<span data-ttu-id="019e1-200">Вам потребуется создать несколько классов для входных данных и прогнозов.</span><span class="sxs-lookup"><span data-stu-id="019e1-200">You need to create some classes for your input data and predictions.</span></span> <span data-ttu-id="019e1-201">Добавьте в проект новый класс:</span><span class="sxs-lookup"><span data-stu-id="019e1-201">Add a new class to your project:</span></span>

1. <span data-ttu-id="019e1-202">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункты **Добавить** > **Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="019e1-202">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="019e1-203">В диалоговом окне **Добавление нового элемента** выберите **Класс** и в поле **Имя** укажите *GitHubIssueData.cs*.</span><span class="sxs-lookup"><span data-stu-id="019e1-203">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *GitHubIssueData.cs*.</span></span> <span data-ttu-id="019e1-204">Теперь нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="019e1-204">Then, select the **Add** button.</span></span>

    <span data-ttu-id="019e1-205">Файл *GitHubIssueData.cs* откроется в редакторе кода.</span><span class="sxs-lookup"><span data-stu-id="019e1-205">The *GitHubIssueData.cs* file opens in the code editor.</span></span> <span data-ttu-id="019e1-206">Добавьте следующий оператор `using` в начало файла *GitHubIssueData.cs*:</span><span class="sxs-lookup"><span data-stu-id="019e1-206">Add the following `using` statement to the top of *GitHubIssueData.cs*:</span></span>

[!code-csharp[AddUsings](../../../samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#AddUsings)]

<span data-ttu-id="019e1-207">Удалите из файла *GitHubIssueData.cs* существующее определение класса и добавьте следующий код с двумя классами `GitHubIssue` и `IssuePrediction`:</span><span class="sxs-lookup"><span data-stu-id="019e1-207">Remove the existing class definition and add the following code, which has two classes `GitHubIssue` and `IssuePrediction`, to the *GitHubIssueData.cs* file:</span></span>

[!code-csharp[DeclareTypes](../../../samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#DeclareTypes)]

<span data-ttu-id="019e1-208">`GitHubIssue` является классом входного набора данных и имеет следующие поля <xref:System.String>:</span><span class="sxs-lookup"><span data-stu-id="019e1-208">`GitHubIssue` is the input dataset class and has the following <xref:System.String> fields:</span></span>

* <span data-ttu-id="019e1-209">`ID` содержит значение идентификатора задачи GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-209">`ID` contains a value for the GitHub issue ID</span></span>
* <span data-ttu-id="019e1-210">`Area` содержит значение метки `Area`.</span><span class="sxs-lookup"><span data-stu-id="019e1-210">`Area` contains a value for the `Area` label</span></span>
* <span data-ttu-id="019e1-211">`Title` содержит название задачи GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-211">`Title` contains the GitHub issue title</span></span>
* <span data-ttu-id="019e1-212">`Description` содержит описание задачи GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-212">`Description` contains the GitHub issue description</span></span>

<span data-ttu-id="019e1-213">Класс `IssuePrediction` используется для прогнозирования после обучения модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-213">`IssuePrediction` is the class used for prediction after the model has been trained.</span></span> <span data-ttu-id="019e1-214">Он имеет один параметр типа `string` (`Area`) и атрибут `PredictedLabel` `ColumnName`.</span><span class="sxs-lookup"><span data-stu-id="019e1-214">It has a single `string` (`Area`) and a `PredictedLabel` `ColumnName` attribute.</span></span> <span data-ttu-id="019e1-215">`Label` используется для создания и обучения модели, а также для оценки модели по второму набору данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-215">The `Label` is used to create and train the model, and it's also used with a second dataset to evaluate the model.</span></span> <span data-ttu-id="019e1-216">`PredictedLabel` используется для прогнозирования и оценки.</span><span class="sxs-lookup"><span data-stu-id="019e1-216">The `PredictedLabel` is used during prediction and evaluation.</span></span> <span data-ttu-id="019e1-217">Для оценки применяются входные обучающие данные, прогнозируемые значения и модель.</span><span class="sxs-lookup"><span data-stu-id="019e1-217">For evaluation, an input with training data, the predicted values, and the model are used.</span></span>

<span data-ttu-id="019e1-218">При создании модели с использованием ML.NET сначала необходимо создать <xref:Microsoft.ML.MLContext>.</span><span class="sxs-lookup"><span data-stu-id="019e1-218">When building a model with ML.NET, you start by creating an <xref:Microsoft.ML.MLContext>.</span></span> <span data-ttu-id="019e1-219">Концептуально это сопоставимо с использованием `DbContext` в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="019e1-219">This is comparable conceptually to using `DbContext` in Entity Framework.</span></span> <span data-ttu-id="019e1-220">Среда предоставляет контекст для вашего задания Машинного обучения, который можно использовать для отслеживания исключений и ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="019e1-220">The environment provides a context for your ML job that can be used for exception tracking and logging.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="019e1-221">Инициализация переменных в методе Main</span><span class="sxs-lookup"><span data-stu-id="019e1-221">Initialize variables in Main</span></span>

<span data-ttu-id="019e1-222">Инициализируйте глобальную переменную `_mlContext` в новом экземпляре `MLContext` со случайным начальным значением (`seed: 0`) для получения воспроизводимых и детерминированных результатов при множестве циклов обучения.</span><span class="sxs-lookup"><span data-stu-id="019e1-222">Initialize the `_mlContext` global variable  with a new instance of `MLContext` with a random seed (`seed: 0`) for repeatable/deterministic results across multiple trainings.</span></span>  <span data-ttu-id="019e1-223">Замените строку `Console.WriteLine("Hello World!")` в методе `Main` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="019e1-223">Replace the `Console.WriteLine("Hello World!")` line with the following code in the `Main` method:</span></span>

[!code-csharp[CreateMLContext](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a><span data-ttu-id="019e1-224">Загрузка данных</span><span class="sxs-lookup"><span data-stu-id="019e1-224">Load the data</span></span>

<span data-ttu-id="019e1-225">Затем инициализируйте глобальную переменную `_trainingDataView` <xref:Microsoft.ML.Data.IDataView> и загрузите данные с параметром `_trainDataPath`.</span><span class="sxs-lookup"><span data-stu-id="019e1-225">Next, initialize the `_trainingDataView` <xref:Microsoft.ML.Data.IDataView> global variable and load the data with the `_trainDataPath` parameter.</span></span>

 <span data-ttu-id="019e1-226">Точно так же как входные и выходные данные `Transforms`, `DataView` является основным типом конвейера данных, сравнимым с `IEnumerable` для `LINQ`.</span><span class="sxs-lookup"><span data-stu-id="019e1-226">As the input and output of `Transforms`, a `DataView` is the fundamental data pipeline type, comparable to `IEnumerable` for `LINQ`.</span></span>

<span data-ttu-id="019e1-227">В ML.NET данные аналогичны `SQL view`.</span><span class="sxs-lookup"><span data-stu-id="019e1-227">In ML.NET, data is similar to a `SQL view`.</span></span> <span data-ttu-id="019e1-228">Они схематизированы, неоднородны, и к ним применено отложенное вычисление.</span><span class="sxs-lookup"><span data-stu-id="019e1-228">It is lazily evaluated, schematized, and heterogenous.</span></span> <span data-ttu-id="019e1-229">Объект является первой частью конвейера. Он загружает данные.</span><span class="sxs-lookup"><span data-stu-id="019e1-229">The object is the first part of the pipeline, and loads the data.</span></span> <span data-ttu-id="019e1-230">В этом руководстве он загружает набор данных с названиями и описаниями задач, а также с соответствующей меткой области GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-230">For this tutorial, it loads a dataset with issue titles, descriptions, and corresponding area GitHub label.</span></span> <span data-ttu-id="019e1-231">`DataView` используется для создания и обучения модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-231">The `DataView` is used to create and train the model.</span></span>

<span data-ttu-id="019e1-232">Так как созданный ранее тип модели данных `GitHubIssue` соответствует схеме набора данных, вы можете объединить инициализацию, сопоставление и загрузку набора данных в одной строке кода.</span><span class="sxs-lookup"><span data-stu-id="019e1-232">Since your previously created `GitHubIssue` data model type matches the dataset schema, you can combine the initialization, mapping, and dataset loading into one line of code.</span></span>

<span data-ttu-id="019e1-233">Первая часть строки (`CreateTextReader<GitHubIssue>(hasHeader: true)`) создает <xref:Microsoft.ML.Data.TextLoader>, выводя схему набора данных из типа модели данных `GitHubIssue` и используя заголовок набора данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-233">The first part of the line (`CreateTextReader<GitHubIssue>(hasHeader: true)`) creates a <xref:Microsoft.ML.Data.TextLoader> by inferencing the dataset schema from the `GitHubIssue` data model type and using the dataset header.</span></span>

<span data-ttu-id="019e1-234">Вы определили схему данных ранее при создании класса `GitHubIssue`.</span><span class="sxs-lookup"><span data-stu-id="019e1-234">You defined the data schema previously when you created the `GitHubIssue` class.</span></span> <span data-ttu-id="019e1-235">Для схемы:</span><span class="sxs-lookup"><span data-stu-id="019e1-235">For your schema:</span></span>

* <span data-ttu-id="019e1-236">Первый столбец `ID` (идентификатор задачи GitHub).</span><span class="sxs-lookup"><span data-stu-id="019e1-236">the first column `ID` (GitHub Issue ID)</span></span>
* <span data-ttu-id="019e1-237">Второй столбец `Area` (прогноз для обучения).</span><span class="sxs-lookup"><span data-stu-id="019e1-237">the second column `Area` (the prediction for training)</span></span>
* <span data-ttu-id="019e1-238">Третий столбец `Title` (название задачи GitHub) является первым [признаком](../resources/glossary.md##feature), используемым для прогнозирования `Area`.</span><span class="sxs-lookup"><span data-stu-id="019e1-238">the third column `Title` (GitHub issue title) is the first [feature](../resources/glossary.md##feature)  used for predicting the `Area`</span></span>
* <span data-ttu-id="019e1-239">Четвертый столбец `Description` является вторым признаком, используемым для прогнозирования `Area`.</span><span class="sxs-lookup"><span data-stu-id="019e1-239">the fourth column  `Description` is the second feature used for predicting the `Area`</span></span>

<span data-ttu-id="019e1-240">Вторая часть строки (`.Read(_trainDataPath)`) использует метод <xref:Microsoft.ML.Data.TextLoader.Read%2A> для загрузки обучающего текстового файла с помощью `_trainDataPath` в глобальную переменную `IDataView` (`_trainingDataView`).</span><span class="sxs-lookup"><span data-stu-id="019e1-240">The second part of the line  (`.Read(_trainDataPath)`) uses <xref:Microsoft.ML.Data.TextLoader.Read%2A> method to load the training text file using `_trainDataPath` into the `IDataView` (`_trainingDataView`) global variable.</span></span>  

<span data-ttu-id="019e1-241">Чтобы инициализировать и загрузить глобальную переменную `_trainingDataView` для ее использования в конвейере, добавьте после инициализации `mlContext` следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-241">To initialize and load the `_trainingDataView` global variable in order to use it for the pipeline, add the following code after the  `mlContext` initialization:</span></span>

[!code-csharp[LoadTrainData](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTrainData)]


<span data-ttu-id="019e1-242">Добавьте в следующую строку метода `Main` приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="019e1-242">Add the following as the next line of code in the `Main` method:</span></span>

[!code-csharp[CallProcessData](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallProcessData)]

<span data-ttu-id="019e1-243">Метод `ProcessData` выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="019e1-243">The `ProcessData` method executes the following tasks:</span></span>

* <span data-ttu-id="019e1-244">извлечение и преобразование данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-244">Extracts and transforms the data.</span></span>
* <span data-ttu-id="019e1-245">возвращение конвейера обработки.</span><span class="sxs-lookup"><span data-stu-id="019e1-245">Returns the processing pipeline.</span></span>

<span data-ttu-id="019e1-246">Создайте метод `ProcessData` сразу после метода `Main`, вставив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-246">Create the `ProcessData` method, just after the `Main` method, using the following code:</span></span>

```csharp
public static EstimatorChain<ITransformer> ProcessData()
{

}
```

## <a name="extract-and-transform-the-data"></a><span data-ttu-id="019e1-247">Извлечение и преобразование данных</span><span class="sxs-lookup"><span data-stu-id="019e1-247">Extract and transform the data</span></span>

<span data-ttu-id="019e1-248">Предварительная обработка и очистка данных — это очень важные задачи, которые нужно выполнить перед использованием набора данных для машинного обучения.</span><span class="sxs-lookup"><span data-stu-id="019e1-248">Pre-processing and cleaning data are important tasks that occur before a dataset is used effectively for machine learning.</span></span> <span data-ttu-id="019e1-249">Необработанные данные часто содержат много шума, недостаточно надежны и в них могут быть пропуски.</span><span class="sxs-lookup"><span data-stu-id="019e1-249">Raw data is often noisy and unreliable, and may be missing values.</span></span> <span data-ttu-id="019e1-250">Использование данных напрямую без этих задач моделирования может дать неправильные результаты.</span><span class="sxs-lookup"><span data-stu-id="019e1-250">Using data without these modeling tasks can produce misleading results.</span></span>

<span data-ttu-id="019e1-251">Конвейеры преобразования ML.NET создают пользовательский набор преобразований, которые применяются к данным перед началом обучения или проверки.</span><span class="sxs-lookup"><span data-stu-id="019e1-251">ML.NET's transform pipelines compose a custom set of transforms that are applied to your data before training or testing.</span></span> <span data-ttu-id="019e1-252">Основной целью преобразования является [присвоение признаков](../resources/glossary.md#feature-engineering).</span><span class="sxs-lookup"><span data-stu-id="019e1-252">The transforms' primary purpose is data [featurization](../resources/glossary.md#feature-engineering).</span></span> <span data-ttu-id="019e1-253">Алгоритмы Машинного обучения определяют данные с [присвоенными признаками](../resources/glossary.md#feature), поэтому следующим шагом является преобразование текстовых данных в формат, который распознают наши алгоритмы Машинного обучения.</span><span class="sxs-lookup"><span data-stu-id="019e1-253">Machine learning algorithms understand [featurized](../resources/glossary.md#feature) data, so the next step is to transform our textual data into a format that our ML algorithms recognize.</span></span> <span data-ttu-id="019e1-254">Этот формат — [числовой вектор](../resources/glossary.md#numerical-feature-vector).</span><span class="sxs-lookup"><span data-stu-id="019e1-254">That format is a [numeric vector](../resources/glossary.md#numerical-feature-vector).</span></span>

<span data-ttu-id="019e1-255">При выполнении следующих шагов мы будем называть столбцы по именам, определенным в классе `GitHubIssue`.</span><span class="sxs-lookup"><span data-stu-id="019e1-255">In the next steps, we refer to the columns by the names defined in the `GitHubIssue` class.</span></span>

<span data-ttu-id="019e1-256">При обучении и оценке модели значения в столбце **Label** по умолчанию рассматриваются как правильные значения для прогноза.</span><span class="sxs-lookup"><span data-stu-id="019e1-256">When the model is trained and evaluated, by default, the values in the **Label** column are considered as correct values to be predicted.</span></span> <span data-ttu-id="019e1-257">Поскольку нам необходимо спрогнозировать метку области GitHub для `GitHubIssue`, скопируйте столбец `Area` в столбец **Label**.</span><span class="sxs-lookup"><span data-stu-id="019e1-257">As we want to predict the Area GitHub label for a `GitHubIssue`, copy the `Area` column into the **Label** column.</span></span> <span data-ttu-id="019e1-258">Для этого используйте `MLContext.Transforms.Conversion.MapValueToKey`, что является оболочкой для класса преобразования <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A>.</span><span class="sxs-lookup"><span data-stu-id="019e1-258">To do that, use the `MLContext.Transforms.Conversion.MapValueToKey`, which is a wrapper for the <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A> transformation class.</span></span>  <span data-ttu-id="019e1-259">`MapValueToKey` возвращает <xref:Microsoft.ML.Data.EstimatorChain%601>, что фактически станет конвейером.</span><span class="sxs-lookup"><span data-stu-id="019e1-259">The `MapValueToKey` returns an <xref:Microsoft.ML.Data.EstimatorChain%601> that will effectively be a pipeline.</span></span> <span data-ttu-id="019e1-260">Присвойте ему имя `pipeline`, так как затем вы добавите обучающую систему в `EstimatorChain`.</span><span class="sxs-lookup"><span data-stu-id="019e1-260">Name this `pipeline` as you will then append the trainer to the `EstimatorChain`.</span></span> <span data-ttu-id="019e1-261">Добавьте следующую строку кода:</span><span class="sxs-lookup"><span data-stu-id="019e1-261">Add this as the next line of code:</span></span>

[!code-csharp[MapValueToKey](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#MapValueToKey)]

<span data-ttu-id="019e1-262">Алгоритму обучения модели требуются **числовые** признаки. Вызовите `mlContext.Transforms.Text.FeaturizeText`, чтобы присвоить текстовым столбцам (`Title` и `Description`) признаки в виде числового вектора для всех вызываемых `TitleFeaturized` и `DescriptionFeaturized`.</span><span class="sxs-lookup"><span data-stu-id="019e1-262">The algorithm that trains the model requires **numeric** features, so you have Next, call `mlContext.Transforms.Text.FeaturizeText` which featurizes the text (`Title` and `Description`) columns into a numeric vector for each called `TitleFeaturized` and `DescriptionFeaturized`.</span></span> <span data-ttu-id="019e1-263">Такое присвоение задает значения ключа различным значениям в каждом из столбцов, и оно используется в алгоритме машинного обучения.</span><span class="sxs-lookup"><span data-stu-id="019e1-263">Featurizing assigns different numeric key values to the different values in each of the columns and is used by the machine learning algorithm.</span></span>
<span data-ttu-id="019e1-264">Добавьте присвоение признаков для обоих столбцов в конвейере, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-264">Append the featurization for both columns to the pipeline with the following code:</span></span>

[!code-csharp[FeaturizeText](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#FeaturizeText)]

<span data-ttu-id="019e1-265">Последний шаг на этапе подготовки данных заключается в объединении всех столбцов признаков в столбце **Features** с помощью класса преобразования `Concatenate`.</span><span class="sxs-lookup"><span data-stu-id="019e1-265">The last step in data preparation combines all of the feature columns into the **Features** column using the `Concatenate` transformation class.</span></span> <span data-ttu-id="019e1-266">По умолчанию алгоритм обучения обрабатывает только признаки, представленные в столбце **Features**.</span><span class="sxs-lookup"><span data-stu-id="019e1-266">By default, a learning algorithm processes only features from the **Features** column.</span></span> <span data-ttu-id="019e1-267">Добавьте это преобразование в конвейер, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-267">Append this transformation to the pipeline with the following code:</span></span>

[!code-csharp[Concatenate](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Concatenate)]

 <span data-ttu-id="019e1-268">Затем добавьте <xref:Microsoft.ML.Data.EstimatorChain`1.AppendCacheCheckpoint%2A> для кэширования DataView, которое может ускорить обучение при многократных итерациях по данным, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-268">Next, append a <xref:Microsoft.ML.Data.EstimatorChain`1.AppendCacheCheckpoint%2A> to cache the DataView so when you iterate over the data multiple times using the cache might get better performance, as with the following code</span></span>

[!code-csharp[AppendCache](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AppendCache)]

<span data-ttu-id="019e1-269">Выполните возврат конвейера в конце метода `ProcessData`.</span><span class="sxs-lookup"><span data-stu-id="019e1-269">Return the pipeline at the end of the `ProcessData` method.</span></span>

[!code-csharp[ReturnPipeline](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnPipeline)]

<span data-ttu-id="019e1-270">На этом этапе выполняется предварительная обработка и присвоение признаков.</span><span class="sxs-lookup"><span data-stu-id="019e1-270">This step handles preprocessing/featurization.</span></span> <span data-ttu-id="019e1-271">Дополнительные компоненты из ML.NET могут дать более точные результаты для вашей модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-271">Using additional components available in ML.NET can enable better results with your model.</span></span>

## <a name="build-and-train-the-model"></a><span data-ttu-id="019e1-272">Сборка и обучение модели</span><span class="sxs-lookup"><span data-stu-id="019e1-272">Build and train the model</span></span>

<span data-ttu-id="019e1-273">В следующей строке кода в методе `Main` добавьте приведенный ниже вызов метода `BuildAndTrainModel`:</span><span class="sxs-lookup"><span data-stu-id="019e1-273">Add the following call to the `BuildAndTrainModel`method as the next line of code in the `Main` method:</span></span>

[!code-csharp[CallBuildAndTrainModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallBuildAndTrainModel)]

<span data-ttu-id="019e1-274">Метод `BuildAndTrainModel` выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="019e1-274">The `BuildAndTrainModel` method executes the following tasks:</span></span>

* <span data-ttu-id="019e1-275">создание класса алгоритма обучения;</span><span class="sxs-lookup"><span data-stu-id="019e1-275">Creates the training algorithm class.</span></span>
* <span data-ttu-id="019e1-276">обучение модели;</span><span class="sxs-lookup"><span data-stu-id="019e1-276">Trains the model.</span></span>
* <span data-ttu-id="019e1-277">прогнозирование области на основе обучающих данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-277">Predicts area based on training data.</span></span>
* <span data-ttu-id="019e1-278">сохранение модели в файл `.zip`;</span><span class="sxs-lookup"><span data-stu-id="019e1-278">Saves the model to a `.zip` file.</span></span>
* <span data-ttu-id="019e1-279">возвращение модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-279">Returns the model.</span></span>

<span data-ttu-id="019e1-280">Создайте метод `BuildAndTrainModel` сразу после метода `Main`, вставив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-280">Create the `BuildAndTrainModel` method, just after the `Main` method, using the following code:</span></span>

```csharp
public static void BuildAndTrainModel()
{

}
```

<span data-ttu-id="019e1-281">Обратите внимание, что в метод BuildAndTrainModel передаются два параметра — `IDataView` для обучающего набора данных (`trainingDataView`) и <xref:Microsoft.ML.Data.EstimatorChain%601> для конвейера обработки, созданного в ProcessData (`pipeline`).</span><span class="sxs-lookup"><span data-stu-id="019e1-281">Notice that two parameters are passed into the BuildAndTrainModel method; an `IDataView` for the training dataset (`trainingDataView`), and a <xref:Microsoft.ML.Data.EstimatorChain%601> for the processing pipeline created in ProcessData (`pipeline`).</span></span>

 <span data-ttu-id="019e1-282">Добавьте следующий код в первую строку метода `BuildAndTrainModel`:</span><span class="sxs-lookup"><span data-stu-id="019e1-282">Add the following code as the first line of the `BuildAndTrainModel` method:</span></span>

### <a name="choose-a-trainer-algorithm"></a><span data-ttu-id="019e1-283">Выбор обучающего алгоритма</span><span class="sxs-lookup"><span data-stu-id="019e1-283">Choose a trainer algorithm</span></span>

<span data-ttu-id="019e1-284">Чтобы добавить обучающий алгоритм, вызовите метод оболочки `mlContext.Transforms.Text.FeaturizeText`, возвращающий объект <xref:Microsoft.ML.Trainers.SdcaMultiClassTrainer>.</span><span class="sxs-lookup"><span data-stu-id="019e1-284">To add the trainer algorithm, call the `mlContext.Transforms.Text.FeaturizeText` wrapper method which returns a <xref:Microsoft.ML.Trainers.SdcaMultiClassTrainer> object.</span></span> <span data-ttu-id="019e1-285">Это средство обучения по дереву принятия решений, которое мы применим для нашего конвейера.</span><span class="sxs-lookup"><span data-stu-id="019e1-285">This is a decision tree learner you'll use in this pipeline.</span></span> <span data-ttu-id="019e1-286">`SdcaMultiClassTrainer` добавляется в `pipeline` и принимает в качестве входных параметров `Title` и `Description` (`Features`) с присвоенными признаками, а также `Label` для обучения по историческим данным.</span><span class="sxs-lookup"><span data-stu-id="019e1-286">The `SdcaMultiClassTrainer` is appended to the `pipeline` and accepts the featurized `Title` and `Description` (`Features`) and the `Label` input parameters to learn from the historic data.</span></span>

<span data-ttu-id="019e1-287">Добавьте следующий код в метод `BuildAndTrainModel`:</span><span class="sxs-lookup"><span data-stu-id="019e1-287">Add the following code to the `BuildAndTrainModel` method:</span></span>

[!code-csharp[SdcaMultiClassTrainer](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SdcaMultiClassTrainer)]

<span data-ttu-id="019e1-288">Теперь, когда вы создали обучающий алгоритм, добавьте его в `pipeline`.</span><span class="sxs-lookup"><span data-stu-id="019e1-288">Now that you've created a trainer algorithm, append it to the `pipeline`.</span></span> <span data-ttu-id="019e1-289">Для возврата в исходное доступное для чтения состояние необходимо также сопоставить метку со значением.</span><span class="sxs-lookup"><span data-stu-id="019e1-289">You also need to map the label to the value to return to its original readable state.</span></span> <span data-ttu-id="019e1-290">Выполните оба эти действия, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-290">Do both of those actions with the following code:</span></span>

[!code-csharp[AddTrainer](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTrainer)]

### <a name="train-the-model"></a><span data-ttu-id="019e1-291">Обучение модели</span><span class="sxs-lookup"><span data-stu-id="019e1-291">Train the model</span></span>

<span data-ttu-id="019e1-292">Обучения модели <xref:Microsoft.ML.Data.TransformerChain%601> выполняется по набору данных, который вы ранее скачали и преобразовали.</span><span class="sxs-lookup"><span data-stu-id="019e1-292">You train the model, <xref:Microsoft.ML.Data.TransformerChain%601>, based on the dataset that has been loaded and transformed.</span></span> <span data-ttu-id="019e1-293">После определения средства оценки вы обучите модель с помощью <xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A>, предоставляя уже загруженные данные для обучения.</span><span class="sxs-lookup"><span data-stu-id="019e1-293">Once the estimator has been defined, you train your model using the <xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A> while providing the already loaded training data.</span></span> <span data-ttu-id="019e1-294">В результате возвращается модель, необходимая для выполнения прогнозов.</span><span class="sxs-lookup"><span data-stu-id="019e1-294">This returns a model to use for predictions.</span></span> <span data-ttu-id="019e1-295">`trainingPipeline.Fit()` обучает конвейер и возвращает `Transformer` на основе переданного типа `DataView`.</span><span class="sxs-lookup"><span data-stu-id="019e1-295">`trainingPipeline.Fit()` trains the pipeline and returns a `Transformer` based on the `DataView` passed in.</span></span> <span data-ttu-id="019e1-296">Эксперимент не выполняется, пока этот процесс не закончится.</span><span class="sxs-lookup"><span data-stu-id="019e1-296">The experiment is not executed until this happens.</span></span>

<span data-ttu-id="019e1-297">Добавьте следующий код в метод `BuildAndTrainModel`:</span><span class="sxs-lookup"><span data-stu-id="019e1-297">Add the following code to the `BuildAndTrainModel` method:</span></span>

[!code-csharp[TrainModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#TrainModel)]

<span data-ttu-id="019e1-298">Несмотря на то что `model` представляет собой `transformer`, который обрабатывает многочисленные строки данных, достаточно распространенным сценарием рабочей среды является прогнозирование на основе отдельных примеров.</span><span class="sxs-lookup"><span data-stu-id="019e1-298">While the `model` is a `transformer` that operates on many rows of data, a very common production scenario is a need for predictions on individual examples.</span></span> <span data-ttu-id="019e1-299"><xref:Microsoft.ML.PredictionEngine%602> — это программа-оболочка, возвращаемая из метода `CreatePredictionEngine`.</span><span class="sxs-lookup"><span data-stu-id="019e1-299">The <xref:Microsoft.ML.PredictionEngine%602> is a wrapper that is returned from the `CreatePredictionEngine` method.</span></span> <span data-ttu-id="019e1-300">Давайте добавим в следующей строке метода `BuildAndTrainModel` указанный ниже код для создания `PredictionEngine`:</span><span class="sxs-lookup"><span data-stu-id="019e1-300">Let's add the following code to create the `PredictionEngine` as the next line in the `BuildAndTrainModel` Method:</span></span>

[!code-csharp[CreatePredictionEngine](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]

<span data-ttu-id="019e1-301">Добавьте задачу GitHub для тестирования прогноза обученной модели в методе `Predict`, создав экземпляр `GitHubIssue`:</span><span class="sxs-lookup"><span data-stu-id="019e1-301">Add a GitHub issue to test the trained model's prediction in the `Predict` method by creating an instance of `GitHubIssue`:</span></span>

[!code-csharp[CreateTestIssue1](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateTestIssue1)]

<span data-ttu-id="019e1-302">Его можно использовать для прогнозирования метки `Area` в одном экземпляре данных задачи.</span><span class="sxs-lookup"><span data-stu-id="019e1-302">You can use that to predict the `Area` label of a single instance of the issue data.</span></span> <span data-ttu-id="019e1-303">Чтобы получить прогноз, примените <xref:Microsoft.ML.PredictionEngine%602.Predict%2A> для данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-303">To get a prediction, use <xref:Microsoft.ML.PredictionEngine%602.Predict%2A> on the data.</span></span> <span data-ttu-id="019e1-304">Обратите внимание, что входные данные имеют строковый формат, а модель выполняет присвоение признаков.</span><span class="sxs-lookup"><span data-stu-id="019e1-304">Note that the input data is a string and the model includes the featurization.</span></span> <span data-ttu-id="019e1-305">Конвейер синхронизируется во время обучения и прогнозирования.</span><span class="sxs-lookup"><span data-stu-id="019e1-305">Your pipeline is in sync during training and prediction.</span></span> <span data-ttu-id="019e1-306">Вам не нужно писать для прогнозирования отдельный код предварительной обработки и присвоения признаков. Кроме того, этот API выполняет как пакетное, так и разовое прогнозирование.</span><span class="sxs-lookup"><span data-stu-id="019e1-306">You didn’t have to write preprocessing/featurization code specifically for predictions, and the same API takes care of both batch and one-time predictions.</span></span>

[!code-csharp[Predict](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Predict)]

### <a name="model-operationalization-prediction"></a><span data-ttu-id="019e1-307">Ввод модели в эксплуатацию: прогнозирование</span><span class="sxs-lookup"><span data-stu-id="019e1-307">Model operationalization: prediction</span></span>

<span data-ttu-id="019e1-308">Отобразите `GitHubIssue` и соответствующий прогноз метки `Area`, чтобы поделиться результатами и обработать их должным образом.</span><span class="sxs-lookup"><span data-stu-id="019e1-308">Display `GitHubIssue` and corresponding `Area` label prediction in order to share the results and act on them accordingly.</span></span> <span data-ttu-id="019e1-309">Этот этап называется вводом в эксплуатацию, после которого возвращаемые данные можно включить в операционные политики.</span><span class="sxs-lookup"><span data-stu-id="019e1-309">This is called operationalization, using the returned data as part of the operational policies.</span></span> <span data-ttu-id="019e1-310">Создайте отображение для результатов с помощью следующего кода <xref:System.Console.WriteLine?displayProperty=nameWithType>:</span><span class="sxs-lookup"><span data-stu-id="019e1-310">Create a display for the results using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[OutputPrediction](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#OutputPrediction)]

### <a name="save-and-return-the-model-trained-to-use-for-evaluation"></a><span data-ttu-id="019e1-311">Сохранение и возврат обученной модели для оценки</span><span class="sxs-lookup"><span data-stu-id="019e1-311">Save and return the model trained to use for evaluation</span></span>

<span data-ttu-id="019e1-312">На этом этапе у вас есть модель типа <xref:Microsoft.ML.Data.TransformerChain%601>, которую можно интегрировать с любыми имеющимися или новыми приложениями .NET.</span><span class="sxs-lookup"><span data-stu-id="019e1-312">At this point, you have a model of type <xref:Microsoft.ML.Data.TransformerChain%601> that can be integrated into any of your existing or new .NET applications.</span></span> <span data-ttu-id="019e1-313">Чтобы сохранить обученную модель в ZIP-файл, добавьте приведенный ниже код вызова метода `SaveModelAsFile` в качестве следующей строки в `BuildAndTrainModel`:</span><span class="sxs-lookup"><span data-stu-id="019e1-313">To save your trained model to a .zip file, add the following code to call the `SaveModelAsFile` method as the next line in `BuildAndTrainModel`:</span></span>

[!code-csharp[CallSaveModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallSaveModel)]

<span data-ttu-id="019e1-314">Выполните возврат модели в конце метода `BuildAndTrainModel`.</span><span class="sxs-lookup"><span data-stu-id="019e1-314">Return the model at the end of the `BuildAndTrainModel` method.</span></span>

[!code-csharp[ReturnModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnModel)]

## <a name="save-the-model-as-azip-file"></a><span data-ttu-id="019e1-315">Сохранение модели в качестве ZIP-файла</span><span class="sxs-lookup"><span data-stu-id="019e1-315">Save the model as a.zip file</span></span>

<span data-ttu-id="019e1-316">Создайте метод `SaveModelAsFile` сразу после метода `BuildAndTrainModel`, вставив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-316">Create the `SaveModelAsFile` method, just after the `BuildAndTrainModel` method, using the following code:</span></span>

```csharp
private static void SaveModelAsFile(MLContext mlContext, ITransformer model)
{

}
```

<span data-ttu-id="019e1-317">Метод `SaveModelAsFile` выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="019e1-317">The `SaveModelAsFile` method executes the following tasks:</span></span>

* <span data-ttu-id="019e1-318">сохранение модели в качестве ZIP-файла.</span><span class="sxs-lookup"><span data-stu-id="019e1-318">Saves the model as a .zip file.</span></span>

<span data-ttu-id="019e1-319">Далее создайте метод для сохранения модели, чтобы ее можно было использовать повторно, а также применять в других приложениях.</span><span class="sxs-lookup"><span data-stu-id="019e1-319">Next, create a method to save the model so that it can be reused and consumed in other applications.</span></span> <span data-ttu-id="019e1-320">`ITransformer` содержит метод <xref:Microsoft.ML.Data.TransformerChain%601.SaveTo(Microsoft.ML.IHostEnvironment,System.IO.Stream)>, который принимает глобальное поле `_modelPath`, и <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="019e1-320">The `ITransformer` has a <xref:Microsoft.ML.Data.TransformerChain%601.SaveTo(Microsoft.ML.IHostEnvironment,System.IO.Stream)> method that takes in the `_modelPath` global field, and a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="019e1-321">Чтобы сохранить модель в качестве ZIP-файла, мы создадим `FileStream` непосредственно перед вызовом метода `SaveTo`.</span><span class="sxs-lookup"><span data-stu-id="019e1-321">To save this as a zip file, you'll create the `FileStream` immediately before calling the `SaveTo` method.</span></span> <span data-ttu-id="019e1-322">В качестве следующей строки в методе `SaveModelAsFile` добавьте приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="019e1-322">Add the following code to the `SaveModelAsFile` method as the next line:</span></span>

[!code-csharp[SaveModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SaveModel)]

<span data-ttu-id="019e1-323">Вы также можете просмотреть, куда был записан файл, написав консольное сообщение с `_modelPath`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-323">You could also display where the file was written by writing a console message with the `_modelPath`, using the following code:</span></span>

```csharp
Console.WriteLine("The model is saved to {0}", _modelPath);
```

## <a name="evaluate-the-model"></a><span data-ttu-id="019e1-324">Оценка модели</span><span class="sxs-lookup"><span data-stu-id="019e1-324">Evaluate the model</span></span>

<span data-ttu-id="019e1-325">Итак, вы завершили создание и обучение модели, и теперь ее можно оценить по другому набору данных, чтобы проверить ее точность и качество прогнозирования.</span><span class="sxs-lookup"><span data-stu-id="019e1-325">Now that you've created and trained the model, you need to evaluate it with a different dataset for quality assurance and validation.</span></span> <span data-ttu-id="019e1-326">В методе `Evaluate` передается для оценки модель, созданная в `BuildAndTrainModel`.</span><span class="sxs-lookup"><span data-stu-id="019e1-326">In the `Evaluate` method, the model created in `BuildAndTrainModel` is passed in to be evaluated.</span></span> <span data-ttu-id="019e1-327">Создайте метод `Evaluate` сразу после метода `BuildAndTrainModel` как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="019e1-327">Create the `Evaluate` method, just after `BuildAndTrainModel`, as in the following code:</span></span>

```csharp
public static void Evaluate()
{

}
```

<span data-ttu-id="019e1-328">Метод `Evaluate` выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="019e1-328">The `Evaluate` method executes the following tasks:</span></span>

* <span data-ttu-id="019e1-329">загрузка тестового набора данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-329">Loads the test dataset.</span></span>
* <span data-ttu-id="019e1-330">создание средства оценки многоклассовой классификации;</span><span class="sxs-lookup"><span data-stu-id="019e1-330">Creates the multiclass evaluator.</span></span>
* <span data-ttu-id="019e1-331">оценка модели и создание метрик;</span><span class="sxs-lookup"><span data-stu-id="019e1-331">Evaluates the model and create metrics.</span></span>
* <span data-ttu-id="019e1-332">отображение метрик.</span><span class="sxs-lookup"><span data-stu-id="019e1-332">Displays the metrics.</span></span>

<span data-ttu-id="019e1-333">Добавьте вызов нового метода из метода `Main`, сразу после вызова метода `BuildAndTrainModel`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-333">Add a call to the new method from the `Main` method, right under the `BuildAndTrainModel` method call, using the following code:</span></span>

[!code-csharp[CallEvaluate](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallEvaluate)]

<span data-ttu-id="019e1-334">Как и для обучающего набора данных ранее, вы можете объединить инициализацию, сопоставление и загрузку тестового набора данных в одной строке кода.</span><span class="sxs-lookup"><span data-stu-id="019e1-334">As you did previously with the training dataset, you can combine the initialization, mapping, and test dataset loading into one line of code.</span></span> <span data-ttu-id="019e1-335">С помощью этого набора данных вы можете оценить модели для проверки ее качества.</span><span class="sxs-lookup"><span data-stu-id="019e1-335">You can evaluate the model using this dataset as a quality check.</span></span> <span data-ttu-id="019e1-336">Добавьте следующий код в метод `Evaluate`:</span><span class="sxs-lookup"><span data-stu-id="019e1-336">Add the following code to the `Evaluate` method:</span></span>

[!code-csharp[LoadTestDataset](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTestDataset)]

<span data-ttu-id="019e1-337">`MulticlassClassificationContext.Evaluate` является оболочкой для метода <xref:Microsoft.ML.MulticlassClassificationContext.Evaluate%2A>, который вычисляет метрики качества для модели по указанному набору данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-337">The `MulticlassClassificationContext.Evaluate` is a wrapper for the <xref:Microsoft.ML.MulticlassClassificationContext.Evaluate%2A> method that computes the quality metrics for the model using the specified dataset.</span></span> <span data-ttu-id="019e1-338">Он возвращает объект <xref:Microsoft.ML.Data.MultiClassClassifierMetrics>, содержащий общие метрики, вычисляемые средствами оценки многоклассовой классификации.</span><span class="sxs-lookup"><span data-stu-id="019e1-338">It returns a <xref:Microsoft.ML.Data.MultiClassClassifierMetrics> object that contains the overall metrics computed by multiclass classification evaluators.</span></span>
<span data-ttu-id="019e1-339">Чтобы отобразить их для оценки качества модели, сначала метрики необходимо получить.</span><span class="sxs-lookup"><span data-stu-id="019e1-339">To display these to determine the quality of the model, you need to get the metrics first.</span></span>
<span data-ttu-id="019e1-340">Используйте метод `Transform` (преобразователь) для глобальной переменной `_trainedModel` машинного обучения для ввода признаков и возврата прогнозов.</span><span class="sxs-lookup"><span data-stu-id="019e1-340">Notice the use of the `Transform` method of the machine learning `_trainedModel` global variable (a transformer) to input the features and return predictions.</span></span> <span data-ttu-id="019e1-341">В качестве следующей строки в методе `Evaluate` добавьте приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="019e1-341">Add the following code to the `Evaluate` method as the next line:</span></span>

[!code-csharp[Evaluate](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Evaluate)]

<span data-ttu-id="019e1-342">Для многоклассовой классификации выполняется оценка следующих метрик:</span><span class="sxs-lookup"><span data-stu-id="019e1-342">The following metrics are evaluated for multiclass classification:</span></span>

* <span data-ttu-id="019e1-343">Микроточность — на метрику точности в равной степени влияет каждая пара "пример-класс".</span><span class="sxs-lookup"><span data-stu-id="019e1-343">Micro Accuracy - Every sample-class pair contributes equally to the accuracy metric.</span></span>  <span data-ttu-id="019e1-344">Значение микроточности должно быть максимально близко к 1.</span><span class="sxs-lookup"><span data-stu-id="019e1-344">You want Micro Accuracy to be as close to 1 as possible.</span></span>

* <span data-ttu-id="019e1-345">Макроточность — на метрику точности в равной степени влияет каждый класс.</span><span class="sxs-lookup"><span data-stu-id="019e1-345">Macro Accuracy - Every class contributes equally to the accuracy metric.</span></span> <span data-ttu-id="019e1-346">Миноритарным классам назначается тот же вес, что и более крупным.</span><span class="sxs-lookup"><span data-stu-id="019e1-346">Minority classes are given equal weight as the larger classes.</span></span> <span data-ttu-id="019e1-347">Значение макроточности должно быть максимально близко к 1.</span><span class="sxs-lookup"><span data-stu-id="019e1-347">You want Macro Accuracy to be as close to 1 as possible.</span></span>

* <span data-ttu-id="019e1-348">Логарифмические потери — см. термин [логарифмические потери](../resources/glossary.md#log-loss).</span><span class="sxs-lookup"><span data-stu-id="019e1-348">Log-loss - see [Log Loss](../resources/glossary.md#log-loss).</span></span> <span data-ttu-id="019e1-349">Значение логарифмических потерь должно быть максимально близко к нулю.</span><span class="sxs-lookup"><span data-stu-id="019e1-349">You want Log-loss to be as close to zero as possible.</span></span>

* <span data-ttu-id="019e1-350">Минимизация логарифмических потерь — находится в диапазоне [-inf, 100], где 100 — идеальный прогноз, а 0 — среднее прогнозное значение.</span><span class="sxs-lookup"><span data-stu-id="019e1-350">Log-loss reduction - Ranges from [-inf, 100], where 100 is perfect predictions and 0 indicates mean predictions.</span></span> <span data-ttu-id="019e1-351">Значение минимизации логарифмических потерь должно быть максимально близко к нулю.</span><span class="sxs-lookup"><span data-stu-id="019e1-351">You want Log-loss reduction to be as close to zero as possible.</span></span>

### <a name="displaying-the-metrics-for-model-validation"></a><span data-ttu-id="019e1-352">Отображение метрик для проверки модели</span><span class="sxs-lookup"><span data-stu-id="019e1-352">Displaying the metrics for model validation</span></span>

<span data-ttu-id="019e1-353">Используйте следующий код для отображения метрик, передачи результатов и выполнения действий по ним:</span><span class="sxs-lookup"><span data-stu-id="019e1-353">Use the following code to display the metrics, share the results, and then act on them:</span></span>

[!code-csharp[DisplayMetrics](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayMetrics)]

## <a name="predict-the-test-data-outcome-with-the-saved-model"></a><span data-ttu-id="019e1-354">Прогнозирование результатов для тестовых данных с помощью сохраненной модели</span><span class="sxs-lookup"><span data-stu-id="019e1-354">Predict the test data outcome with the saved model</span></span>

<span data-ttu-id="019e1-355">Добавьте вызов нового метода из метода `Main`, сразу после вызова метода `Evaluate`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-355">Add a call to the new method from the `Main` method, right under the `Evaluate` method call, using the following code:</span></span>

[!code-csharp[CallPredictIssue](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallPredictIssue)]

<span data-ttu-id="019e1-356">Создайте метод `PredictIssue` сразу после метода `Evaluate`, вставив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-356">Create the `PredictIssue` method, just after the `Evaluate` method, using the following code:</span></span>

```csharp
private static void PredictIssue()
{

}
```

<span data-ttu-id="019e1-357">Метод `PredictIssue` выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="019e1-357">The `PredictIssue` method executes the following tasks:</span></span>

* <span data-ttu-id="019e1-358">создание одной задачи с тестовыми данными;</span><span class="sxs-lookup"><span data-stu-id="019e1-358">Creates a single issue of test data.</span></span>
* <span data-ttu-id="019e1-359">прогнозирование области на основе тестовых данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-359">Predicts Area based on test data.</span></span>
* <span data-ttu-id="019e1-360">объединение тестовых данных и прогнозов для создания отчетов;</span><span class="sxs-lookup"><span data-stu-id="019e1-360">Combines test data and predictions for reporting.</span></span>
* <span data-ttu-id="019e1-361">отображение результатов прогнозирования.</span><span class="sxs-lookup"><span data-stu-id="019e1-361">Displays the predicted results.</span></span>

<span data-ttu-id="019e1-362">Сначала загрузите сохраненную ранее модель, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="019e1-362">First, load the model that you saved previously with the following code:</span></span>

[!code-csharp[LoadModel](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadModel)]

<span data-ttu-id="019e1-363">Добавьте задачу GitHub для тестирования прогноза обученной модели в методе `Predict`, создав экземпляр `GitHubIssue`:</span><span class="sxs-lookup"><span data-stu-id="019e1-363">Add a GitHub issue to test the trained model's prediction in the `Predict` method by creating an instance of `GitHubIssue`:</span></span>

[!code-csharp[AddTestIssue](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTestIssue)]

[!code-csharp[CreatePredictionEngine](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]
  
<span data-ttu-id="019e1-364">Теперь, когда у вас есть модель, ее можно использовать для прогнозирования метки области GitHub у одного экземпляра данных задачи GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-364">Now that you have a model, you can use that to predict the Area GitHub label of a single instance of the GitHub issue data.</span></span> <span data-ttu-id="019e1-365">Чтобы получить прогноз, примените <xref:Microsoft.ML.PredictionEngine%602.Predict%2A> для данных.</span><span class="sxs-lookup"><span data-stu-id="019e1-365">To get a prediction, use <xref:Microsoft.ML.PredictionEngine%602.Predict%2A> on the data.</span></span> <span data-ttu-id="019e1-366">Обратите внимание, что входные данные имеют строковый формат, а модель выполняет присвоение признаков.</span><span class="sxs-lookup"><span data-stu-id="019e1-366">Note that the input data is a string and the model includes the featurization.</span></span> <span data-ttu-id="019e1-367">Конвейер синхронизируется во время обучения и прогнозирования.</span><span class="sxs-lookup"><span data-stu-id="019e1-367">Your pipeline is in sync during training and prediction.</span></span> <span data-ttu-id="019e1-368">Вам не нужно писать для прогнозирования отдельный код предварительной обработки и присвоения признаков. Кроме того, этот API выполняет как пакетное, так и разовое прогнозирование.</span><span class="sxs-lookup"><span data-stu-id="019e1-368">You didn’t have to write preprocessing/featurization code specifically for predictions, and the same API takes care of both batch and one-time predictions.</span></span> <span data-ttu-id="019e1-369">Добавьте в метод `PredictIssue` приведенный ниже код для прогнозирования:</span><span class="sxs-lookup"><span data-stu-id="019e1-369">Add the following code to the `PredictIssue` method for the predictions:</span></span>

[!code-csharp[PredictIssue](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]

### <a name="model-operationalization-prediction"></a><span data-ttu-id="019e1-370">Ввод модели в эксплуатацию: прогнозирование</span><span class="sxs-lookup"><span data-stu-id="019e1-370">Model operationalization: prediction</span></span>

<span data-ttu-id="019e1-371">Отобразите `Area`, чтобы категоризировать задачу и обработать ее должным образом.</span><span class="sxs-lookup"><span data-stu-id="019e1-371">Display `Area` in order to categorize the issue and act on it accordingly.</span></span> <span data-ttu-id="019e1-372">Этот этап называется вводом в эксплуатацию, после которого возвращаемые данные можно включить в операционные политики.</span><span class="sxs-lookup"><span data-stu-id="019e1-372">This is called operationalization, using the returned data as part of the operational policies.</span></span> <span data-ttu-id="019e1-373">Создайте отображение для результатов с помощью следующего кода <xref:System.Console.WriteLine?displayProperty=nameWithType>:</span><span class="sxs-lookup"><span data-stu-id="019e1-373">Create a display for the results using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[DisplayResults](../../../samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayResults)]

## <a name="results"></a><span data-ttu-id="019e1-374">Результаты</span><span class="sxs-lookup"><span data-stu-id="019e1-374">Results</span></span>

<span data-ttu-id="019e1-375">Результаты выполнения должны выглядеть примерно так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="019e1-375">Your results should be similar to the following.</span></span> <span data-ttu-id="019e1-376">В процессе работы конвейер выводит сообщения.</span><span class="sxs-lookup"><span data-stu-id="019e1-376">As the pipeline processes, it displays messages.</span></span> <span data-ttu-id="019e1-377">Вы можете видеть предупреждения или обработанные сообщения.</span><span class="sxs-lookup"><span data-stu-id="019e1-377">You may see warnings, or processing messages.</span></span> <span data-ttu-id="019e1-378">Для удобства они удалены из следующих результатов.</span><span class="sxs-lookup"><span data-stu-id="019e1-378">These have been removed from the following results for clarity.</span></span>

```console
=============== Single Prediction just-trained-model - Result: area-System.Net ===============
The model is saved to C:\Users\johalex\dotnet-samples\samples\machine-learning\tutorials\GitHubIssueClassification\bin\Debug\netcoreapp2.0\..\..\..\Models\model.zip
*************************************************************************************************************
*       Metrics for Multi-class Classification model - Test Data
*------------------------------------------------------------------------------------------------------------
*       MicroAccuracy:    0.74
*       MacroAccuracy:    0.687
*       LogLoss:          .932
*       LogLossReduction: 63.852
*************************************************************************************************************
=============== Single Prediction - Result: area-System.Data ===============
```

<span data-ttu-id="019e1-379">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="019e1-379">Congratulations!</span></span> <span data-ttu-id="019e1-380">Вы успешно создали модель машинного обучения для классификации и прогнозирования метки области у задачи GitHub.</span><span class="sxs-lookup"><span data-stu-id="019e1-380">You've now successfully built a machine learning model for classifying and predicting an Area label for a GitHub issue.</span></span> <span data-ttu-id="019e1-381">Исходный код для этого руководства можно найти в репозитории [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification).</span><span class="sxs-lookup"><span data-stu-id="019e1-381">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="019e1-382">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="019e1-382">Next steps</span></span>

<span data-ttu-id="019e1-383">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="019e1-383">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="019e1-384">Определение проблемы</span><span class="sxs-lookup"><span data-stu-id="019e1-384">Understand the problem</span></span>
> * <span data-ttu-id="019e1-385">Выбор подходящей задачи машинного обучения</span><span class="sxs-lookup"><span data-stu-id="019e1-385">Select the appropriate machine learning task</span></span>
> * <span data-ttu-id="019e1-386">подготавливать данные;</span><span class="sxs-lookup"><span data-stu-id="019e1-386">Prepare your data</span></span>
> * <span data-ttu-id="019e1-387">создавать конвейеры обучения;</span><span class="sxs-lookup"><span data-stu-id="019e1-387">Create the learning pipeline</span></span>
> * <span data-ttu-id="019e1-388">загружать классификатор;</span><span class="sxs-lookup"><span data-stu-id="019e1-388">Load a classifier</span></span>
> * <span data-ttu-id="019e1-389">обучить модель;</span><span class="sxs-lookup"><span data-stu-id="019e1-389">Train the model</span></span>
> * <span data-ttu-id="019e1-390">проводить оценку модели по другому набору данных;</span><span class="sxs-lookup"><span data-stu-id="019e1-390">Evaluate the model with a different dataset</span></span>
> * <span data-ttu-id="019e1-391">прогнозировать результаты для тестовых данных с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="019e1-391">Predict the test data outcomes with the model</span></span>

<span data-ttu-id="019e1-392">Переходите к следующему руководству:</span><span class="sxs-lookup"><span data-stu-id="019e1-392">Advance to the next tutorial to learn more</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="019e1-393">Прогнозирование платы за такси</span><span class="sxs-lookup"><span data-stu-id="019e1-393">Taxi Fare Predictor</span></span>](taxi-fare.md)