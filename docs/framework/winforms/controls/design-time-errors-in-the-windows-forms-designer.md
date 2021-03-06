---
title: Ошибки времени разработки в конструктор Windows Forms
titleSuffix: ''
ms.date: 09/09/2019
f1_keywords:
- DTELErrorList
- WhyDTELPage
helpviewer_keywords:
- errors [Windows Forms Designer]
- design-time errors [Windows Forms Designer]
ms.assetid: ad408380-825a-46d8-9a4a-531b130b88ce
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 2a39c76d011c6d129f91647fabe3f129245b9466
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746097"
---
# <a name="windows-forms-designer-error-page"></a>Страница ошибки конструктор Windows Forms

Если конструктор Windows Forms не удается загрузить из-за ошибки в коде, стороннего компонента или в других местах, вместо конструктора отображается страница ошибки. Эта страница ошибки не обязательно означает ошибку в конструкторе. Ошибка может находиться где-нибудь на странице кода программной части с именем \<> имя-формы. Designer.cs. Ошибки отображаются в свертываемых, желтых полосах со ссылкой для перехода к расположению ошибки на кодовой странице.

![Страница ошибки конструктор Windows Forms](media/windows-forms-designer-error-page-collapsed.png)

Можно игнорировать ошибки и продолжить загрузку конструктора, нажав кнопку **Пропустить и продолжить**. Это действие может привести к непредвиденному поведению, например, элементы управления могут не отображаться в области конструктора.

## <a name="instances-of-this-error"></a>Экземпляры этой ошибки

Когда желтая полоса погрешностей разворачивается, отображается каждый экземпляр ошибки. Многие типы ошибок содержат точное расположение в следующем формате: *[имя проекта]* *[имя формы]* строка: *[номер строки]* столбец: *[номер столбца]* . Если стек вызовов связан с ошибкой, можно щелкнуть ссылку **Показать стек вызовов** , чтобы увидеть ее. Изучение стека вызовов может помочь в устранении ошибки.

![конструктор Windows Forms развернутая ошибка](media/windows-forms-designer-error-page-expanded.png)

> [!NOTE]
>
> - Для Visual Basic приложений страница ошибок времени разработки не отображает более одной ошибки, но может отображать несколько экземпляров одной и той же ошибки.
> - Для C++ приложений ошибки не содержат ссылок на расположение кода.

## <a name="help-with-this-error"></a>Справка по этой ошибке

Если раздел справки для ошибки доступен, щелкните ссылку **MSDN Help** , чтобы перейти непосредственно на страницу справки по адресу docs.Microsoft.com.

## <a name="forum-posts-about-this-error"></a>Сообщения об этой ошибке в форуме

Щелкните **Поиск сообщений, связанных с этой ошибкой** , на ФОРУМах MSDN, чтобы перейти к форумам сети разработчиков Microsoft. Вам может потребоваться выполнить Специальный Поиск на форумах [конструктор Windows Forms](https://social.msdn.microsoft.com/Forums/windows/home?forum=winformsdesigner) или [Windows Forms](https://social.msdn.microsoft.com/Forums/windows/home?category=windowsforms) .

## <a name="design-time-errors"></a>Ошибки времени разработки

В этом разделе перечислены некоторые ошибки, которые могут возникнуть.

### <a name="identifier-name-is-not-a-valid-identifier"></a>"\<имя идентификатора >" не является допустимым идентификатором

Эта ошибка указывает на неправильное именование поля, метода, события или объекта.

### <a name="name-already-exists-in-project-name"></a>"\<имя >" уже существует в "\<имя проекта >"

Сообщение об ошибке: ""\<имя > "уже существует в"\<имя проекта > ". Введите уникальное имя ".

Вы указали имя для наследуемой формы, которая уже существует в проекте. Чтобы исправить эту ошибку, присвойте производной форме уникальное имя.

### <a name="toolbox-tab-name-is-not-a-toolbox-category"></a>"\<имя вкладки панели элементов >" не является категорией панели элементов

Сторонний конструктор пытался получить доступ к вкладке на панели элементов, которая не существует. Обратитесь к поставщику компонента.

### <a name="a-requested-language-parser-is-not-installed"></a>Синтаксический анализатор запрошенного языка не установлен

Сообщение об ошибке: "запрошенный анализатор языка не установлен. Имя анализатора языка — "{0}".

Visual Studio попытался загрузить конструктор, зарегистрированный для типа файла, но это не удалось. Скорее всего, это вызвано ошибкой, возникшей во время установки. Обратитесь к поставщику языка, который вы используете для исправления.

### <a name="a-service-required-for-generating-and-parsing-source-code-is-missing"></a>Отсутствует служба, которая требуется для генерирования и анализа исходного кода

Это проблема с компонентом стороннего производителя. Обратитесь к поставщику компонента.

### <a name="an-exception-occurred-while-trying-to-create-an-instance-of-object-name"></a>Произошло исключение при попытке создать экземпляр "\<имя объекта >"

Сообщение об ошибке: "произошло исключение при попытке создать экземпляр"\<имя объекта > ". Исключением было "\<строкового исключения\>".

Сторонний конструктор запросил, чтобы Visual Studio создал объект, но объект вызвал ошибку. Обратитесь к поставщику компонента.

### <a name="another-editor-has-document-name-open-in-an-incompatible-mode"></a>Другой редактор "\<имя документа >" открыт в несовместимом режиме

Сообщение об ошибке: "другой редактор содержит\<имя документа >" открыть в несовместимом режиме. Закройте редактор и повторите операцию ".

Эта ошибка возникает при попытке открыть файл, который уже открыт в другом редакторе. Отображается редактор, в котором уже открыт файл. Чтобы исправить эту ошибку, закройте редактор, в котором открыт файл, и повторите попытку.

### <a name="another-editor-has-made-changes-to-document-name"></a>Другой редактор внес изменения в "\<имя документа >"

Закройте и снова откройте конструктор, чтобы изменения вступили в силу. Как правило, Visual Studio автоматически перезагружает конструктор после внесения изменений. Однако другие конструкторы, такие как сторонние конструкторы компонентов, могут не поддерживать поведение перезагрузки. В этом случае Visual Studio предложит закрыть и снова открыть конструктор вручную.

### <a name="another-editor-has-the-file-open-in-an-incompatible-mode"></a>Файл открыт в несовместимом режиме в другом редакторе

Сообщение об ошибке: "в другом редакторе файл открыт в несовместимом режиме. Закройте редактор и повторите операцию ".

Это сообщение аналогично "другому редактору\<имя документа >" открыть в несовместимом режиме ", но Visual Studio не удается определить имя файла. Чтобы исправить эту ошибку, закройте редактор, в котором открыт файл, и повторите попытку.

### <a name="array-rank-rank-in-array-is-too-high"></a>Слишком большой ранг массива "\<в массиве >"

Visual Studio поддерживает только одномерные массивы в блоке кода, который анализируется конструктором. Многомерные массивы допустимы за пределами этой области.

### <a name="assembly-assembly-name-could-not-be-opened"></a>Не удалось открыть сборку "\<имя сборки >"

Сообщение об ошибке: "сборка"\<имя сборки > "не может быть открыта. Убедитесь, что файл по-прежнему существует. "

Это сообщение об ошибке возникает при попытке открыть файл, который не удалось открыть. Убедитесь, что файл существует и является допустимой сборкой.

### <a name="bad-element-type-this-serializer-expects-an-element-of-type-type-name"></a>Недопустимый тип элемента. Этот сериализатор принимает элемент типа "\<имя типа >"

Это проблема с компонентом стороннего производителя. Обратитесь к поставщику компонента.

### <a name="cannot-access-the-visual-studio-toolbox-at-this-time"></a>Не удается получить доступ к панели элементов Visual Studio

Visual Studio выполнил вызов к панели элементов, который был недоступен. Если эта ошибка возникает, то при возникновении этой ошибки Зарегистрируйте проблему, используя [сообщение о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="cannot-bind-an-event-handler-to-the-event-name-event-because-it-is-read-only"></a>Не удается привязать обработчик событий к событию "\<имя события >", так как оно доступно только для чтения

Эта ошибка часто возникает при попытке подключения события к элементу управления, унаследованному от базового класса. Если переменная-член элемента управления является закрытой, Visual Studio не сможет подключить событие к методу. К закрытым унаследованным элементам управления нельзя привязывать дополнительные события.

### <a name="cannot-create-a-method-name-for-the-requested-component-because-it-is-not-a-member-of-the-design-container"></a>Не удается создать имя метода для указанного компонента, так как он не является членом контейнера конструктора

Visual Studio попыталась добавить обработчик событий в компонент, который не имеет переменной-члена в конструкторе. Обратитесь к поставщику компонента.

### <a name="cannot-name-the-object-name-because-it-is-already-named-name"></a>Не удается присвоить имя объекту "\<имя >", так как он уже называется "\<имя >"

Это внутренняя ошибка в сериализаторе Visual Studio. Он указывает, что сериализатор пытается присвоить имя объекту дважды, что не поддерживается. Если вы видите эту ошибку, зарегистрируйте проблему, выполнив инструкцию [сообщить о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="cannot-remove-or-destroy-inherited-component-component-name"></a>Невозможно удалить или уничтожить унаследованный компонент "\<имя компонента >"

Унаследованные элементы управления являются собственностью наследуемого класса. Изменения наследуемого элемента управления должны быть сделаны в классе, из которого создается элемент управления. Поэтому вы не можете переименовать или удалить его.

### <a name="category-toolbox-tab-name-does-not-have-a-tool-for-class-class-name"></a>Категория "\<имя вкладки панели элементов >" не имеет средства для класса "\<имя класса >"

Конструктор попытался сослаться на класс на определенной вкладке панели элементов, но класс не существует. Обратитесь к поставщику компонента.

### <a name="class-class-name-has-no-matching-constructor"></a>Класс "\<имя класса >" не имеет соответствующего конструктора

Сторонний конструктор запросил Visual Studio создать объект с определенными параметрами в конструкторе, который не существует. Обратитесь к поставщику компонента.

### <a name="code-generation-for-property-property-name-failed"></a>Не удалось создать код для свойства "\<имя свойства >"

Это универсальная оболочка для ошибки. Строка ошибки, сопровождающая это сообщение, предоставит дополнительные сведения о сообщении об ошибке и ссылку на более конкретный раздел справки. Чтобы исправить эту ошибку, устраните ошибку, указанную в сообщении об ошибке, добавленном к этой ошибке.

### <a name="component-component-name-did-not-call-containeradd-in-its-constructor"></a>Компоненту "\<имени компонента >" не удалось вызвать контейнер. Add () в его конструкторе

Это ошибка в компоненте, который вы только что загрузили или поместили в форму. Он указывает, что компонент не был добавлен в контейнерный элемент управления (будь то другой элемент управления или форма). Конструктор продолжит работать, но могут возникнуть проблемы с компонентом во время выполнения.

Чтобы исправить эту ошибку, обратитесь к поставщику компонента. Или, если это созданный компонент, вызовите метод `IContainer.Add` в конструкторе компонента.

### <a name="component-name-cannot-be-empty"></a>Имя компонента не может быть пустым

Эта ошибка возникает при попытке переименовать компонент в пустое значение.

### <a name="could-not-access-the-variable-variable-name-because-it-has-not-been-initialized-yet"></a>Не удалось получить доступ к переменной "\<имя переменной >", так как она еще не инициализирована

Эта ошибка может возникать из-за двух сценариев. Поставщик стороннего компонента имеет проблему с элементом управления или компонентом, который они были распределены, или написанный вами код имеет рекурсивные зависимости между компонентами.

Чтобы исправить эту ошибку, убедитесь, что в коде отсутствует рекурсивная зависимость. Если такие проблемы не возникают, запишите точный текст сообщения об ошибке и обратитесь к поставщику компонента.

### <a name="could-not-find-type-type-name"></a>Не удалось найти тип "\<имя типа >"

Сообщение об ошибке: "не удалось найти тип\<имя типа >". Убедитесь, что ссылка на сборку, содержащую этот тип, указана. Если этот тип является частью проекта разработки, убедитесь, что проект успешно построен. "

Эта ошибка возникла из-за того, что ссылка не найдена. Убедитесь, что указан тип, указанный в сообщении об ошибке, и что на все сборки, необходимые для этого типа, также ссылаются. Часто проблема заключается в том, что элемент управления в решении не был построен. Чтобы выполнить сборку, в меню **Сборка** выберите пункт **собрать решение** . В противном случае, если элемент управления уже создан, добавьте ссылку вручную из контекстного меню папки **References** или **Dependencies** в Обозреватель решений.

### <a name="could-not-load-type-type-name"></a>Не удалось загрузить тип "\<имя типа >"

Сообщение об ошибке: "не удалось загрузить тип\<имя типа >". Убедитесь, что сборка, содержащая этот тип, добавлена в ссылки проекта ".

Visual Studio попытался подключить метод обработки событий и не смог найти один или несколько типов параметров для метода. Обычно это вызвано отсутствием ссылки. Чтобы исправить эту ошибку, добавьте в проект ссылку, содержащую тип, и повторите попытку.

### <a name="could-not-locate-the-project-item-templates-for-inherited-components"></a>Не удалось найти шаблоны элементов проекта для унаследованных компонентов

Шаблоны для наследуемых форм в Visual Studio недоступны. Если вы видите эту ошибку, зарегистрируйте проблему, выполнив инструкцию [сообщить о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="delegate-class-class-name-has-no-invoke-method-is-this-class-a-delegate"></a>Класс делегата "\<имя класса >" не имеет метода Invoke. Является ли этот класс делегатом?

Visual Studio попытался создать обработчик событий, но возникли проблемы с типом события. Это может произойти, если событие было создано с помощью несовместимого с CLS языка. Обратитесь к поставщику компонента.

### <a name="duplicate-declaration-of-member-member-name"></a>Повторяющееся объявление члена "\<имя члена >"

Эта ошибка возникает из-за того, что переменная-член была объявлена дважды (например, в коде объявляются два элемента управления с именем `Button1`). Имена должны быть уникальными в наследуемых формах. Кроме того, имена не могут отличаться только регистром.

### <a name="error-reading-resources-from-the-resource-file-for-the-culture-culture-name"></a>Ошибка при чтении ресурсов из файла ресурсов для языка и региональных параметров "\<имя языка и региональных параметров >"

Эта ошибка может возникать, если в проекте имеется поврежденный RESX-файл.

Чтобы исправить эту ошибку, сделайте следующее:

1. Нажмите кнопку **Показать все файлы** в Обозреватель решений, чтобы просмотреть RESX файлы, связанные с решением.
2. Загрузите RESX-файл в редактор XML, щелкнув правой кнопкой мыши RESX-файл и выбрав команду **Открыть**.
3. Измените файл. resx вручную, чтобы устранить ошибки.

### <a name="error-reading-resources-from-the-resource-file-for-the-default-culture-culture-name"></a>Ошибка при чтении ресурсов из файла ресурсов для языка и региональных параметров по умолчанию "\<имя языка и региональных параметров >"

Эта ошибка может возникать, если в проекте имеется поврежденный RESX-файл для языка и региональных параметров по умолчанию.

Чтобы исправить эту ошибку, сделайте следующее:

1. Нажмите кнопку **Показать все файлы** в Обозреватель решений, чтобы просмотреть RESX файлы, связанные с решением.
2. Загрузите RESX-файл в редактор XML, щелкнув правой кнопкой мыши RESX-файл и выбрав команду **Открыть**.
3. Измените файл. resx вручную, чтобы устранить ошибки.

### <a name="failed-to-parse-method-method-name"></a>Не удалось проанализировать метод "\<имя метода >"

Сообщение об ошибке: "не удалось выполнить синтаксический анализ метода"\<имя метода > ". Средство синтаксического анализа сообщило о следующей ошибке: "\<строка ошибки >". Возможные ошибки можно найти в список задач ".

Это общее сообщение об ошибке, возникающее во время синтаксического анализа. Эти ошибки часто возникают из-за синтаксических ошибок. Конкретные сообщения об этой ошибке см. в список задач.

### <a name="invalid-component-name-component-name"></a>Недопустимое имя компонента: "\<имя компонента >"

Вы попытались переименовать компонент в недопустимое значение для этого языка. Чтобы исправить эту ошибку, присвойте компоненту имя, которое соответствует правилам именования для этого языка.

### <a name="the-type-class-name-is-made-of-several-partial-classes-in-the-same-file"></a>Тип "\<имя класса >" состоит из нескольких разделяемых классов в одном файле

При определении класса в нескольких файлах с помощью ключевого слова [partial](../../../csharp/language-reference/keywords/partial-type.md) можно иметь только одно частичное определение в каждом файле.

Чтобы исправить эту ошибку, удалите из файла все определения класса, кроме одного из частичных определений.

### <a name="the-assembly-assembly-name-could-not-be-found"></a>Не удалось найти сборку "\<имя сборки >"

Сообщение об ошибке: "не удалось найти сборку"\<имя сборки > ". Убедитесь, что ссылка на сборку указана. Если сборка является частью текущего проекта разработки, убедитесь, что проект построен. "

Эта ошибка похожа на "тип\<имя типа >" не удалось найти ", но эта ошибка обычно возникает из-за атрибута метаданных. Чтобы исправить эту ошибку, убедитесь, что имеются ссылки на все сборки, используемые атрибутами.

### <a name="the-assembly-name-assembly-name-is-invalid"></a>Недопустимое имя сборки "\<имя сборки >"

Компонент запросил определенную сборку, но имя, предоставленное компонентом, не является допустимым именем сборки. Обратитесь к поставщику компонента.

### <a name="the-base-class-class-name-cannot-be-designed"></a>Не удается разработать базовый класс "\<имя класса >"

Visual Studio загрузил класс, но класс не может быть спроектирован, так как разработчик класса не предоставил конструктор. Если класс поддерживает конструктор, убедитесь в отсутствии проблем, которые могут вызвать проблемы с отображением в конструкторе, например ошибками компилятора. Кроме того, убедитесь, что все ссылки на класс верны, и все имена классов написаны правильно. В противном случае, если класс недоступен для разработки, измените его в представлении кода.

### <a name="the-base-class-class-name-could-not-be-loaded"></a>Не удалось загрузить базовый класс "\<имя класса >"

В проекте нет ссылки на класс, поэтому Visual Studio не может загрузить его. Чтобы исправить эту ошибку, добавьте ссылку на класс в проекте, а затем закройте и снова откройте окно конструктор Windows Forms.

### <a name="the-class-class-name-cannot-be-designed-in-this-version-of-visual-studio"></a>Класс "\<имя класса >" не может быть спроектирован в этой версии Visual Studio

Конструктор для этого элемента управления или компонента не поддерживает те же типы, что и Visual Studio. Обратитесь к поставщику компонента.

### <a name="the-class-name-is-not-a-valid-identifier-for-this-language"></a>Имя класса в этом языке является недопустимым идентификатором

Исходный код, создаваемый пользователем, имеет имя класса, которое недопустимо для используемого языка. Чтобы исправить эту ошибку, присвойте классу имя, соответствующее требованиям к языку.

### <a name="the-component-cannot-be-added-because-it-contains-a-circular-reference-to-reference-name"></a>Невозможно добавить компонент, так как он содержит циклическую ссылку на "\<ссылочное имя >"

Невозможно добавить элемент управления или компонент к самому себе. Другая ситуация, в которой это может произойти, заключается в том, что в методе InitializeComponent формы (например, Form1) есть код, создающий другой экземпляр Form1.

### <a name="the-designer-cannot-be-modified-at-this-time"></a>Невозможно изменить конструктор

Эта ошибка возникает, когда файл в редакторе помечен как "только для чтения". Убедитесь, что файл не помечен как "только для чтения", а приложение не работает.

### <a name="the-designer-could-not-be-shown-for-this-file-because-none-of-the-classes-within-it-can-be-designed"></a>Для данного файла не удалось отобразить конструктор, так как в нем отсутствуют классы для разработки

Эта ошибка возникает, когда Visual Studio не удается найти базовый класс, соответствующий требованиям конструктора. Формы и элементы управления должны быть производными от базового класса, который поддерживает конструкторы. Если вы наследуете от наследуемой формы или элемента управления, убедитесь, что проект построен.

### <a name="the-designer-for-base-class-class-name-is-not-installed"></a>Конструктор для базового класса "\<имя класса >" не установлен

Visual Studio не удалось загрузить конструктор для класса. Если вы видите эту ошибку, зарегистрируйте проблему, выполнив инструкцию [сообщить о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="the-designer-must-create-an-instance-of-type-type-name-but-it-cant-because-the-type-is-declared-as-abstract"></a>Конструктор должен создать экземпляр типа "\<имя типа >", но это не может быть вызвано тем, что тип объявлен как абстрактный

Эта ошибка возникла из-за того, что базовый класс объекта, переданного в конструктор, является [абстрактным](../../../csharp/language-reference/keywords/abstract.md), что недопустимо.

### <a name="the-file-could-not-be-loaded-in-the-designer"></a>Не удалось загрузить файл в конструктор

Базовый класс этого файла не поддерживает конструкторы. Чтобы обойти это решение, используйте представление кода для работы с файлом. Щелкните правой кнопкой мыши файл в обозреватель решений и выберите команду **Просмотреть код**.

### <a name="the-language-for-this-file-does-not-support-the-necessary-code-parsing-and-generation-services"></a>Язык этого файла не поддерживает службы, необходимые для создания и разбора кода

Сообщение об ошибке: "язык этого файла не поддерживает необходимые службы синтаксического анализа и формирования кода. Убедитесь, что открываемый файл является членом проекта, и попытайтесь открыть его еще раз. "

Эта ошибка, скорее всего, привела к открытию файла в проекте, который не поддерживает конструкторы.

### <a name="the-language-parser-class-class-name-is-not-implemented-properly"></a>Класс синтаксического анализатора языка "\<имя класса >" не реализован должным образом

Сообщение об ошибке: "класс анализатора языка"\<имя класса > "не реализован должным образом. Обратитесь к поставщику за обновленным модулем синтаксического анализатора.

Используемый язык зарегистрировал класс конструктора, который не является производным от правильного базового класса. Обратитесь к поставщику языка, который вы используете.

### <a name="the-name-name-is-already-used-by-another-object"></a>Имя "\<имя >" уже используется другим объектом

Это внутренняя ошибка в сериализаторе Visual Studio. Если вы видите эту ошибку, зарегистрируйте проблему, выполнив инструкцию [сообщить о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="the-object-object-name-does-not-implement-the-icomponent-interface"></a>Объект "\<имя объекта >" не реализует интерфейс IComponent

Visual Studio попытался создать компонент, но созданный объект не реализует интерфейс <xref:System.ComponentModel.IComponent>. Обратитесь к поставщику компонента за исправлением.

### <a name="the-object-object-name-returned-null-for-the-property-property-name-but-this-is-not-allowed"></a>Объект "\<имя объекта >" вернул значение NULL для свойства "\<имя свойства >", но это запрещено

Существуют некоторые свойства .NET, которые всегда должны возвращать объект. Например, коллекция **Controls** формы должна всегда возвращать объект, даже если в нем нет элементов управления.

Чтобы исправить эту ошибку, убедитесь, что свойство, указанное в ошибке, не имеет значение null.

### <a name="the-serialization-data-object-is-not-of-the-proper-type"></a>Объект данных сериализации имеет неверный тип

Объект данных, предлагаемый сериализатором, не является экземпляром типа, который соответствует используемому текущему сериализатору. Обратитесь к поставщику компонента.

### <a name="the-service-service-name-is-required-but-could-not-be-located"></a>Служба "\<имя службы >" является обязательной, но ее не удалось найти

Сообщение об ошибке: "служба"\<имя службы > "является обязательной, но ее не удалось найти. Возможно, возникла проблема с установкой Visual Studio ".

Служба, необходимая для Visual Studio, недоступна. Если вы пытались загрузить проект, который не поддерживает этот конструктор, используйте редактор кода, чтобы внести необходимые изменения. В противном случае, если вы видите эту ошибку, зарегистрируйте проблему, используя [сообщение о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="the-service-instance-must-derive-from-or-implement-interface-name"></a>Экземпляр службы должен быть производным от или реализовывать "\<имя интерфейса >"

Эта ошибка означает, что конструктор компонента или компонента вызвал метод **AddService** , для которого требуется интерфейс и объект, но указанный объект не реализует указанный интерфейс. Обратитесь к поставщику компонента.

### <a name="the-text-in-the-code-window-could-not-be-modified"></a>Не удалось изменить текст в окне кода

Сообщение об ошибке: "текст в окне кода не может быть изменен. Убедитесь, что файл не доступен только для чтения и на диске достаточно места. "

Эта ошибка возникает, когда Visual Studio не удается изменить файл из-за проблем с дисковым пространством или памятью либо файл помечен как "только для чтения".

### <a name="the-toolbox-enumerator-object-only-supports-retrieving-one-item-at-a-time"></a>Объект перечислителя панели элементов поддерживает одновременное извлечение только одного элемента

Если эта ошибка возникает, то при возникновении этой ошибки Зарегистрируйте проблему, используя [сообщение о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="the-toolbox-item-for-component-name-could-not-be-retrieved-from-the-toolbox"></a>Не удалось получить элемент панели элементов для "\<имя компонента >" из панели элементов

Сообщение об ошибке: "не удалось извлечь элемент панели элементов для"\<имя компонента > "из панели элементов. Убедитесь, что сборка, содержащая элемент панели элементов, установлена правильно. Элемент панели элементов вызвал следующую ошибку: \<строка ошибки > ".

Рассматриваемый компонент вызвал исключение при обращении Visual Studio к нему. Обратитесь к поставщику компонента.

### <a name="the-toolbox-item-for-toolbox-item-name-could-not-be-retrieved-from-the-toolbox"></a>Не удалось получить элемент панели элементов для "\<имя элемента панели элементов >" из панели элементов

Сообщение об ошибке: "не удалось получить элемент панели элементов для"\<имя элемента панели элементов > "из панели элементов. Попробуйте удалить элемент из панели элементов и добавить его обратно. "

Эта ошибка возникает, если данные в элементе панели элементов повреждены или изменилась версия компонента. Попробуйте удалить элемент из панели элементов и снова добавить его.

### <a name="the-type-type-name-could-not-be-found"></a>Не удалось найти тип "\<имя типа >"

Сообщение об ошибке: "не удалось найти тип\<имя типа >". Убедитесь, что ссылка на сборку, содержащую тип, указана. Если сборка является частью текущего проекта разработки, убедитесь, что проект построен. "

При загрузке конструктора Visual Studio не удалось найти тип. Убедитесь, что ссылка на сборку, содержащую тип, указана. Если сборка является частью текущего проекта разработки, убедитесь, что проект построен.

### <a name="the-type-resolution-service-may-only-be-called-from-the-main-application-thread"></a>Служба разрешения типов может быть вызвана только из потока основного приложения

Visual Studio попытался получить доступ к необходимым ресурсам из неверного потока. Эта ошибка отображается, когда код, используемый для создания конструктора, вызвал службу разрешения типов из потока, отличного от основного потока приложения. Чтобы исправить эту ошибку, вызовите службу из нужного потока или обратитесь к поставщику компонента.

### <a name="the-variable-variable-name-is-either-undeclared-or-was-never-assigned"></a>Переменная "\<имя переменной >" либо не объявлена, либо не была назначена

Исходный код содержит ссылку на переменную, например **Button1**, которая не объявлена или назначена. Если переменная не назначена, это сообщение отображается как предупреждение, а не как ошибка.

### <a name="there-is-already-a-command-handler-for-the-menu-command-menu-command-name"></a>Уже существует обработчик команд для команды меню "\<меню" имя команды ">"

Эта ошибка возникает, если сторонний конструктор добавляет команду, уже имеющую обработчик в командную таблицу. Обратитесь к поставщику компонента.

### <a name="there-is-already-a-component-named-component-name"></a>Уже существует компонент с именем "\<имя компонента >"

Сообщение об ошибке: "уже существует компонент с именем"\<имя компонента > ". Компоненты должны иметь уникальные имена, а имена не должны быть чувствительны к регистру. Имя также не может конфликтовать с именем любого компонента в унаследованном классе. "

Это сообщение об ошибке возникает при изменении имени компонента в окно свойств. Чтобы исправить эту ошибку, убедитесь, что все имена компонентов уникальны, не учитывают регистр и не конфликтуют с именами каких-либо компонентов в унаследованных классах.

### <a name="there-is-already-a-toolbox-item-creator-registered-for-the-format-format-name"></a>Уже существует создатель элемента панели элементов, зарегистрированный для формата "\<имя формата >"

Сторонний компонент выполнил обратный вызов элемента на вкладке панели элементов, но элемент уже содержал обратный вызов. Обратитесь к поставщику компонента.

### <a name="this-language-engine-does-not-support-a-codemodel-with-which-to-load-a-designer"></a>Языковые средства для данного языка не поддерживаю CodeModel, с помощью которой загружается конструктор

Это сообщение похоже на «язык этого файла не поддерживает необходимые службы синтаксического анализа и формирования кода», но это сообщение включает в себя внутреннюю проблему регистрации. Если эта ошибка возникает, то при возникновении этой ошибки Зарегистрируйте проблему, используя [сообщение о проблеме](/visualstudio/ide/how-to-report-a-problem-with-visual-studio).

### <a name="type-type-name-does-not-have-a-constructor-with-parameters-of-types-parameter-type-names"></a>Тип "\<имя типа\>" не имеет конструктора с параметрами типов "\<имена типов параметров >"

Visual Studio не удалось найти [конструктор](../../../csharp/programming-guide/classes-and-structs/constructors.md) с совпадающими параметрами. Это может быть результатом предоставления конструктора с типами, отличными от требуемых. Например, конструктор **Point** может принимать два целых числа. Если вы указали число с плавающей запятой, возникает эта ошибка.

Чтобы исправить эту ошибку, используйте другой конструктор или явно приведите типы параметров таким образом, чтобы они совпадали с параметрами, предоставленными конструктором.

### <a name="unable-to-add-reference-reference-name-to-the-current-application"></a>Не удалось добавить ссылку "\<ссылочное имя >" в текущее приложение

Сообщение об ошибке: "не удается добавить ссылку"\<ссылочное имя > "в текущее приложение. Убедитесь, что другая версия "\<Reference Name >" еще не упоминается ".

Visual Studio не удалось добавить ссылку. Чтобы исправить эту ошибку, убедитесь, что на другую версию ссылки еще нет ссылок.

### <a name="unable-to-check-out-the-current-file"></a>Не удалось извлечь текущий файл

Сообщение об ошибке: "не удается извлечь текущий файл. Файл может быть заблокирован или может потребоваться извлечь его вручную. "

Эта ошибка возникает при изменении файла, который в настоящий момент возвращен в систему управления версиями. Как правило, Visual Studio отображает диалоговое окно извлечения файла, чтобы пользователь мог извлечь его. На этот раз файл не был извлечен, возможно, из-за конфликта слияния во время извлечения. Чтобы исправить эту ошибку, убедитесь, что файл не заблокирован, и попытайтесь извлечь его вручную.

### <a name="unable-to-find-page-named-options-dialog-box-tab-name"></a>Не удалось найти страницу с именем "диалоговое окно параметров\<" имя вкладки > "

Эта ошибка возникает, когда конструктор компонентов запрашивает доступ к странице из диалогового окна "Параметры", используя несуществующее имя. Обратитесь к поставщику компонента.

### <a name="unable-to-find-property-property-name-on-page-options-dialog-box-tab-name"></a>Не удается найти свойство "\<имя свойства >" на странице "имя вкладки диалогового окна" Параметры\<">"

Эта ошибка возникает, когда конструктор компонентов запрашивает доступ к определенному значению на странице в диалоговом окне "Параметры", но это значение не существует. Обратитесь к поставщику компонента.

### <a name="visual-studio-cannot-open-a-designer-for-the-file-because-the-class-within-it-does-not-inherit-from-a-class-that-can-be-visually-designed"></a>В Visual Studio не удается открыть конструктор файла, так как класс этого файла не унаследован от класса, поддерживающего визуальную разработку

В Visual Studio загружен класс, но загрузить конструктор для этого класса невозможно. Visual Studio требует, чтобы конструкторы использовали первый класс в файле. Чтобы исправить эту ошибку, переместите код класса так, чтобы он был первым классом в файле, а затем снова загрузите конструктор.

### <a name="visual-studio-cannot-save-or-load-instances-of-the-type-type-name"></a>Visual Studio не может сохранить или загрузить экземпляры типа "\<имя типа >"

Это проблема с компонентом стороннего производителя. Обратитесь к поставщику компонента.

### <a name="visual-studio-is-unable-to-open-document-name-in-design-view"></a>Visual Studio не удается открыть "\<имя документа >" в представление конструирования

Сообщение об ошибке: "Visual Studio не удается открыть"\<имя документа > "в представление конструирования. Средство синтаксического анализа не установлено для типа файлов. "

Эта ошибка означает, что язык проекта не поддерживает конструктор и возникает при попытке открыть файл в диалоговом окне Открыть файл или обозреватель решений. Вместо этого измените файл в представлении кода.

### <a name="visual-studio-was-unable-to-find-a-designer-for-classes-of-type-type-name"></a>Visual Studio не удалось найти конструктор для классов типа "\<имя типа >"

Visual Studio загрузил класс, но класс не может быть спроектирован. Вместо этого измените класс в представлении кода, щелкнув правой кнопкой мыши класс и выбрав пункт **Просмотреть код**.

## <a name="see-also"></a>См. также раздел

- [Разработка Windows Forms элементов управления с помощью конструктора](developing-windows-forms-controls-at-design-time.md)
- [Форум конструктор Windows Forms](https://social.msdn.microsoft.com/Forums/windows/home?forum=winformsdesigner)
