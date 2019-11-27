---
title: Безопасность Azure для облачных приложений
description: Создание архитектуры облачных приложений .NET для Azure | Безопасность Azure для собственных облачных приложений
ms.date: 06/30/2019
ms.openlocfilehash: 44e81bc91fa952448f501a29e9db8afb2dbda752
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "73841273"
---
# <a name="azure-security-for-cloud-native-apps"></a>Безопасность Azure для облачных приложений

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Облачные приложения могут быть как простыми, так и более сложными в защите, чем традиционные приложения. С недостатком, необходимо защитить более небольшие приложения и выделить больше энергии для создания инфраструктуры безопасности. Разнородная природа языков программирования и стилей в большинстве развертываний служб также означает, что необходимо уделять больше внимания бюллетеням безопасности от многих разных поставщиков.

На стороне зеркального отображения небольшие службы, каждый из которых имеет собственное хранилище данных, ограничивают область атаки. Если злоумышленник нарушает одну систему, скорее всего, злоумышленник сможет выполнить переход на другую систему, чем в монолитном приложении. Границы процессов являются строгой границей. Кроме того, если происходит утечка резервной копии базы данных, то повреждение более ограничено, так как эта база данных содержит только подмножество данных и вряд ли будет содержать персональные данные.

## <a name="threat-modeling"></a>Моделирование угроз

Не имеет значения, если преимущества перевешивают недостатки облачных приложений, необходимо соблюдать одинаковую целостность системы безопасности. Безопасность и безопасное обдумывание должны быть частью каждого этапа разработки и эксплуатации. При планировании приложения задайте такие вопросы:

- Каковы последствия потери данных?
- Как можно ограничить ущерб от недопустимых данных, внедренных в эту службу?
- Кто должен иметь доступ к этим данным?
- Существуют ли политики аудита на месте процесса разработки и выпуска?

Все эти вопросы являются частью процесса, называемого [моделированием угроз](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool). Этот процесс пытается ответить на вопрос о том, какие угрозы есть в системе, насколько вероятны угрозы, а также о потенциальном повреждении.

После того как список угроз будет установлен, необходимо решить, стоит ли отказаться от их устранения. Иногда угроза может быть настолько маловероятной и дорогостоящей для планирования того, что она не стоит тратить на энергию. Например, некоторые субъекты уровня состояния могут внедрять изменения в структуру процесса, который используется миллионами устройств. Теперь вместо выполнения определенного фрагмента кода в [кольце 3](https://en.wikipedia.org/wiki/Protection_ring)этот код выполняется в кольцевой 0. Это позволяет использовать оболочку, которая может обходить гипервизор и запускать код атаки на компьютерах без операционной системы, что позволяет выполнять атаки на всех виртуальных машинах, работающих на этом оборудовании.

Измененные процессоры трудно обнаружить без микрообласти и получить расширенные знания о архитектуре этого процессора. Этот сценарий вряд ли будет выполняться и дешевле, поэтому, вероятно, ни одна из моделей угроз не рекомендует создавать для нее защиту от эксплойтов.

Более вероятные угрозы, такие как неработающие элементы управления доступом, позволяющие `Id` увеличением числа атак (замена `Id=2` с `Id=3` в URL-адресе) или внедрением кода SQL, являются более привлекательными для создания защиты. Устранение этих угроз вполне оправдано для создания и предотвращения брешей в безопасности, мониторе репутацию компании.

## <a name="principle-of-least-privilege"></a>Принцип наименьших привилегий

Одной из найденных идей в области безопасности компьютера является принцип наименьших привилегий (ПОЛП). На самом деле, это основано на том, что в большинстве случаев безопасность является цифровой или физической. Вкратце, принцип состоит в том, что любой пользователь или процесс должен иметь наименьшее количество прав для выполнения задачи.

Например, представьте, что говорят в банке. доступ к сейфу является нераспространенным действием. Таким образом, среднее значение не может открыть само себя. Чтобы получить доступ, им нужно эскалировать свой запрос через Банк Manager, который выполняет дополнительные проверки безопасности.

В компьютерной системе, отличный пример, — права пользователя, подключающегося к базе данных. Во многих случаях используется одна учетная запись пользователя для создания структуры базы данных и запуска приложения. За исключением крайних случаев, учетной записи, в которой выполняется приложение, не требуется возможность обновлять сведения о схеме. Должно быть несколько учетных записей, которые предоставляют различные уровни привилегий. Приложение должно использовать только уровень разрешений, предоставляющий доступ на чтение и запись к данным в таблицах. Такой тип защиты позволит избежать атак, направленных на удаление таблиц базы данных или ввод вредоносных триггеров.

Почти во всех частях создания облачных приложений может быть полезно помнить принцип минимальных привилегий. Его можно найти при настройке брандмауэров, групп безопасности сети, ролей и областей в контроле доступа на основе ролей (RBAC).

## <a name="penetration-testing"></a>тестирование уязвимости;

По мере того, как приложения становятся более сложными, число векторов атак увеличивается по скорости сигнализации. Нарушение моделирования угроз в том, что она обычно выполняется теми же людьми, которые создают систему. Точно так же, как многие разработчики испытывают проблемы с представлением взаимодействия с пользователем, а затем создают непригодные для использования пользовательские интерфейсы, большинство разработчиков испытывают трудности при просмотре каждого вектора атак. Также возможно, что разработчики, создающие систему, не очень попадают в методологии атак и не имеют какого-либо важного.

Тестирование на проникновение или "тестирование пера" предполагает, что внешние субъекты попытаются атаковать систему. Злоумышленники могут быть внешней консалтинговой компанией или другими разработчиками, имеющими хорошие знания о безопасности из другой части бизнеса. Они получают полномочияу корзины, чтобы попытаться обходят систему. Часто они находят обширные бреши в системе безопасности, которые необходимо исправить. Иногда вектор атаки будет полностью непредвиденным образом, например с помощью атаки фишинга от Генерального директора.

Azure постоянно подключается к атакам от [группы хакеров внутри корпорации Майкрософт](https://azure.microsoft.com/resources/videos/red-vs-blue-internal-security-penetration-testing-of-microsoft-azure/). В течение многих лет они первыми нашли десятки потенциально разрушительных направлений атак, закрыв их, прежде чем они смогут использовать извне. Чем больше используется цель, тем более вероятно, что нескончаемые Actors попытается использовать его, и несколько целевых объектов в мире будут более заманчивыми, чем Azure.

## <a name="monitoring"></a>Мониторинг

Если злоумышленник пытается проникнуть приложение, это должно быть предупреждение. Часто атаки можно проанализировать, изучив журналы в службах. Атаки оставляют знаки признаков, которые могут быть обнаружены до того, как они будут выполнены. Например, злоумышленник, пытающийся угадать пароль, будет выполнять много запросов к системе входа. Мониторинг системы входа в систему может обнаружить странные шаблоны, которые не являются стандартными, с обычным шаблоном доступа. Этот мониторинг можно включить в оповещение, которое может, в свою очередь, оповещать пользователя об активации некоторого рода контрмер. Высокозрелая система мониторинга может даже предпринять действия на основе этих отклонений, заранее добавляя правила для блокировки запросов или регулирования ответов.

## <a name="securing-the-build"></a>Обеспечение безопасности сборки

В одном месте, где часто изменяются проблемы безопасности, является процесс сборки. Не только проверки безопасности запуска сборки, например сканирование небезопасного кода или учетных данных для возврата, но сама сборка должна быть безопасной. Если сервер сборки скомпрометирован, он предоставляет отличный вектор для введения в продукт произвольного кода.

Представьте себе, что злоумышленнику нужно украсть пароли людей, которые подписывается в веб-приложение. Они могут ввести этап сборки, который изменяет возвращенный код для отражения любого запроса на вход на другой сервер. Следующий код проходит через сборку, он обновляется автоматически. Сканирование уязвимостей исходного кода не перехватывает это, как оно выполняется перед сборкой. В этом случае никто не будет перехватывать его при проверке кода, так как на сервере сборки выполняется процедура сборки. Код злоумышленника перейдет в рабочую среду, где он сможет собирать пароли. Возможно, нет журнала аудита для процесса сборки или хотя бы никто не отслеживает аудит.

Это идеальный пример целевого объекта с низкой ценностью, который можно использовать для разбиения системы на части. После того как злоумышленник нарушает периметр системы, он может начать работу над поиском способов повышения их разрешений до момента, когда они могут вызвать реальные угрозы в любом месте.

## <a name="building-secure-code"></a>Создание безопасного кода

.NET Framework уже является весьма безопасной платформой. Он позволяет избежать некоторых ловушек неуправляемого кода, таких как проход по концам массивов. Работа активно выполняется для устранения брешей в системе безопасности по мере их обнаружения. Существует даже [Программа поощренияа ошибок](https://www.microsoft.com/msrc/bounty) , которая выплачивает исследователи, чтобы найти проблемы в платформе и сообщить о них вместо использования их.

Существует множество способов повышения безопасности кода .NET. Следующие рекомендации, такие как [рекомендации по безопасному кодированию для статьи .NET](https://docs.microsoft.com/dotnet/standard/security/secure-coding-guidelines) , являются разумным шагом для обеспечения безопасности кода с самого начала. [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_2017_Project) является еще одним ценным руководством по созданию безопасного кода.

Процесс сборки является хорошим местом для размещения средств сканирования для обнаружения проблем в исходном коде до того, как они будут внесены в рабочую среду. Большинство проектов имеют зависимости от других пакетов. Средство, которое может выполнять поиск устаревших пакетов, будет перехватывать проблемы в ночных сборках. Даже при создании образов DOCKER полезно проверить и убедиться, что в базовом образе отсутствуют известные уязвимости. Еще одно, что нужно проверить: никто случайно не вернул учетные данные.

## <a name="built-in-security"></a>Встроенная безопасность

Azure предназначен для балансировки использования и безопасности большинства пользователей. Разные пользователи будут иметь разные требования к безопасности, поэтому им нужно настроить подход к безопасности в облаке. Корпорация Майкрософт публикует значительную часть сведений о безопасности в [центре управления безопасностью](https://azure.microsoft.com/support/trust-center/). Этот ресурс должен быть первым остановкой для специалистов, желающих понять, как работают встроенные технологии защиты от атак.

В портал Azure [помощник Azure](https://azure.microsoft.com/services/advisor/) — это система, которая постоянно сканирует среду и делает рекомендации. Некоторые из этих рекомендаций предназначены для экономии денег пользователей, но другие предназначены для обнаружения потенциально небезопасных конфигураций, например для того, чтобы контейнер хранилища был открыт в мире и не защищен виртуальной сетью.

## <a name="azure-network-infrastructure"></a>Инфраструктура сети Azure

В локальной среде развертывания большой объем энергии выделен для настройки сети. Настройка маршрутизаторов, коммутаторов и, например, усложняет работу. Сети позволяют некоторым ресурсам взаимодействовать с другими ресурсами и запрещать доступ в некоторых случаях. Частое правило сети заключается в ограничении доступа к рабочей среде из среды разработки с тем, что разрабатывается вкось часть кода и удаляется сообщить данных.

В большинстве случаев большинство ресурсов Azure PaaS имеют только самую базовую и разрешающую сетевую установку. Например, все, кто находится в Интернете, может получить доступ к службе приложений. Новые экземпляры SQL Server обычно бывают ограниченными, чтобы внешние стороны не могли получить к ним доступ, но диапазоны IP-адресов, используемые Azure, разрешены через. Таким образом, несмотря на то, что сервер SQL Server защищен от внешних угроз, злоумышленнику нужно только настроить Azure-плацдарм, откуда они смогут запускать атаки для всех экземпляров SQL в Azure.

К счастью, большинство ресурсов Azure можно разместить в виртуальной сети Azure, что позволяет более детально управлять доступом. Подобно тому, как локальные сети устанавливают частные сети, защищенные из широкого мира, виртуальные сети — это острова частных IP-адресов, расположенных в сети Azure.

![рис. 10-1. Виртуальная сеть в Azure](./media/virtual-network.png)
**рис. 10-1**. Виртуальная сеть в Azure.

Так же, как локальные сети имеют брандмауэр, управляющий доступом к сети, вы можете установить аналогичный брандмауэр на границе виртуальной сети. По умолчанию все ресурсы в виртуальной сети по-прежнему могут взаимодействовать с Интернетом. Это только входящие подключения, требующие явной формы исключения брандмауэра.

После установки сети внутренние ресурсы, такие как учетные записи хранения, можно настроить для доступа только к ресурсам, которые также находятся в виртуальной сети. Этот брандмауэр обеспечивает дополнительный уровень безопасности, в случае утечки ключей для этой учетной записи хранения злоумышленники не смогут подключиться к ней для использования потерянных ключей. Это еще один пример принципа наименьших привилегий.

Узлы в кластере Azure Kubernetes могут участвовать в виртуальной сети так же, как другие ресурсы, которые являются более машинными для Azure. Эта функция называется [сетевым интерфейсом контейнера Azure](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md). Фактически она выделяет подсеть в виртуальной сети, в которой выделяются виртуальные машины и образы контейнеров.

Продолжая путь, иллюстрирующий принцип наименьших привилегий, не каждый ресурс в виртуальной сети должен взаимодействовать с каждым другим ресурсом. Например, в приложении, которое предоставляет веб-API для учетной записи хранения и базы данных SQL, маловероятно, что базе данных и учетной записи хранения необходимо взаимодействовать друг с другом. Любой обмен данными между ними будет проходить через веб-приложение. Таким образом, для запрета трафика между двумя службами можно использовать [группу безопасности сети (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) .

Реализация политики запрета взаимодействия между ресурсами может быть неудобной для реализации, особенно поступающих из использования Azure без ограничений трафика. В некоторых других облаках концепция групп безопасности сети гораздо более распространена. Например, политика по умолчанию в AWS заключается в том, что ресурсы не могут взаимодействовать между собой, пока они не будут включены правилами в NSG. Хотя это и более медленный процесс разработки, более ограниченная среда обеспечивает более безопасную работу по умолчанию. Использование правильных методик DevOps, особенно с помощью [Azure Resource Manager или terraform](infrastructure-as-code.md) для управления разрешениями, упрощает управление правилами.

Виртуальные сети также могут быть полезны при настройке обмена данными между локальными и облачными ресурсами. Виртуальную частную сеть можно использовать для беспроблемного подключения двух сетей друг к другу. Это позволяет запускать виртуальную сеть без какого-либо шлюза для сценариев, в которых все пользователи находятся на месте. Существует ряд технологий, которые можно использовать для установки этой сети. Проще всего использовать [VPN типа "сеть — сеть](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) ", которую можно установить между многими маршрутизаторами и Azure. Трафик шифруется и подключается через Интернет по таким же затратам на каждый байт, как и любой другой трафик. Для сценариев, в которых предпочтительнее пропускная способность или более безопасность, Azure предлагает службу с именем [Express Route](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute) , которая использует частный канал между локальной сетью и Azure. Он является более дорогостоящим и сложным в установке, но также более безопасным.

## <a name="role-based-access-control-for-restricting-access-to-azure-resources"></a>Управление доступом на основе ролей для ограниченного доступа к ресурсам Azure

RBAC — это система, которая предоставляет удостоверение для приложений, выполняющихся в Azure. Приложения могут обращаться к ресурсам с помощью этого удостоверения, а не в дополнение к использованию ключей или паролей.

## <a name="security-principals"></a>Субъекты безопасности

Первый компонент RBAC — это субъект безопасности. Субъект безопасности может быть пользователем, группой, субъектом-службой или управляемым удостоверением.

![рис. 10-2. различные типы субъектов безопасности](./media/rbac-security-principal.png)
**рис. 10-2**. Различные типы субъектов безопасности.

- Пользователь — любой пользователь с учетной записью в Azure Active Directory является пользователем.
- Группа — коллекция пользователей из Azure Active Directory. Как член группы, пользователь принимает роли этой группы в дополнение к собственным.
- Субъект-служба — удостоверение безопасности, под которым запускаются службы или приложения.
- Управляемое удостоверение — удостоверение Azure Active Directory, управляемое Azure. Управляемые удостоверения обычно используются при разработке облачных приложений, которые управляют учетными данными для проверки подлинности в службах Azure.

Субъект безопасности может применяться к большинству ресурсов. Это означает, что можно назначить субъект безопасности контейнеру, работающему в Azure Kubernetes, что позволит ему получить доступ к секретам, хранящимся в Key Vault. Функция Azure может принимать разрешение на обмен данными с экземпляром Active Directory для проверки JWT для вызывающего пользователя. После включения служб с помощью субъекта-службы их разрешения могут управляться детально с помощью ролей и областей.

## <a name="roles"></a>Роли

Субъект безопасности может принимать множество ролей или, используя более сарториал аналог, выполнять многие шляпы. Каждая роль определяет ряд разрешений, например "чтение сообщений из конечной точки служебной шины Azure". Эффективный набор разрешений субъекта безопасности — это сочетание всех разрешений, назначенных всем ролям, которые имеют субъект безопасности. В Azure имеется большое количество встроенных ролей, и пользователи могут определять собственные роли.

![рис. 10-3 определения ролей RBAC](./media/rbac-role-definition.png)
**рис. 10-3**. Определения ролей RBAC.

Встроенные в Azure, также представляют собой ряд высокоуровневых ролей, таких как владелец, участник, читатель и администратор учетных записей пользователей. С ролью владельца субъект безопасности может получить доступ ко всем ресурсам и назначить разрешения другим пользователям. Участник имеет тот же уровень доступа ко всем ресурсам, но не может назначать разрешения. Читатель может только просматривать существующие ресурсы Azure, а администратор учетных записей пользователей может управлять доступом к ресурсам Azure.

Более детализированные встроенные роли, такие как [участник зоны DNS](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#dns-zone-contributor) , имеют права, ограниченные одной службой. Субъекты безопасности могут принимать любое количество ролей.

## <a name="scopes"></a>Области

Роли можно применять к ограниченному набору ресурсов в Azure. Например, применение области к предыдущему примеру чтения из очереди служебной шины позволяет ограничить разрешение до одной очереди: "чтение сообщений из конечной точки служебной шины Azure `blah.servicebus.windows.net/queue1`"

Область может быть такой же, как и один ресурс, или же ее можно применить ко всей группе ресурсов, подписке или даже к группе управления.

При проверке того, имеет ли субъект безопасности определенные разрешения, учитывается сочетание роли и области. Это сочетание обеспечивает мощный механизм авторизации.

## <a name="deny"></a>Запрещено

Ранее для RBAC разрешены только правила "разрешить". Это поведение сделало некоторые области сложными для сборки. Например, разрешая субъекту безопасности доступ ко всем учетным записям хранения, за исключением одного, необходимо предоставить явное разрешение на потенциально неограниченный список учетных записей хранения. Каждый раз, когда новая учетная запись хранения была создана, ее необходимо добавить в этот список учетных записей. Это дополнительные затраты на управление, которые наверняка не были желательны.

Правила запрета имеют приоритет над разрешающими правилами. Теперь, представляя ту же область "разрешить все, кроме одной", можно представить в виде двух правил "разрешить все" и "отклонить только один из них". Запрещает использовать правила не только для упрощения управления, но и для обеспечения дополнительной безопасности ресурсов, запретив доступ всем.

## <a name="checking-access"></a>Проверка доступа

Как можно себе представить, наличие большого количества ролей и областей может значительно усложнить эффективное разрешение субъекта-службы. Накапливаться запрещает правила поверх этого, только увеличивает сложность. К счастью, есть Калькулятор разрешений, который может показывать действующие разрешения для любого субъекта-службы. Обычно он находится на вкладке IAM на портале, как показано на рисунке 10-3.

![рис. 10-4. Калькулятор разрешений для службы приложений](./media/check-rbac.png)
**рис. 10-4**. Калькулятор разрешений для службы приложений.

## <a name="securing-secrets"></a>Защита секретов

Пароли и сертификаты представляют собой распространенный вектор атаки для злоумышленников. Устройство взлома паролей может выполнять атаки методом подбора и пытаться угадать миллиарды паролей в секунду. Поэтому важно, чтобы пароли, используемые для доступа к ресурсам, были надежными, с большим количеством символов. Эти пароли — именно те пароли, которые практически невозможно запомнить. К счастью, пароли в Azure не должны быть известны ни одним человеком.

Многие эксперты по безопасности [предполагают](https://www.troyhunt.com/password-managers-dont-have-to-be-perfect-they-just-have-to-be-better-than-not-having-one/) , что использование диспетчера паролей для сохранения собственных паролей является лучшим способом. Хотя он централизует пароли в одном месте, он также позволяет использовать сложные пароли и обеспечивать их уникальность для каждой учетной записи. Эта же система существует в Azure: центральное хранилище для секретов.

## <a name="azure-key-vault"></a>Хранилище ключей Azure;

Azure Key Vault предоставляет централизованное расположение для хранения паролей для таких объектов, как базы данных, ключи API и сертификаты. После введения секрета в хранилище он никогда не отображается снова, и команды для их извлечения и просмотра становятся намеренно сложными. Информация в безопасности защищена с помощью шифрования программного обеспечения или FIPS 140-2 уровня 2 проверенных аппаратных модулей безопасности.

Доступ к хранилищу ключей осуществляется через RBAC, что означает, что не только пользователь может получить доступ к информации в хранилище. Предположим, что веб-приложению нужно получить доступ к строке подключения к базе данных, хранящейся в Azure Key Vault. Чтобы получить доступ, приложения должны работать с помощью субъекта-службы. В рамках этой предпринятой роли они могут считывать секреты из безопасного. Существует ряд различных параметров безопасности, которые могут дополнительно ограничить доступ приложения к хранилищу, чтобы не обновлять секреты, но только считывать их.

Доступ к хранилищу ключей можно отслеживать, чтобы обеспечить доступ к хранилищу только ожидаемым приложениям. Журналы можно интегрировать обратно в Azure Monitor, разблокируя возможность настройки оповещений при обнаружении непредвиденных условий.

## <a name="kubernetes"></a>Kubernetes

В Kubernetes существует аналогичная служба для обслуживания небольших фрагментов секретной информации. Секреты Kubernetes можно задать с помощью обычного исполняемого файла `kubectl`.

Создание секрета так же просто, как поиск версии Base64 сохраняемых значений:

```console
echo -n 'admin' | base64
YWRtaW4=
echo -n '1f2d1e2e67df' | base64
MWYyZDFlMmU2N2Rm
```

Затем добавьте его в секретный файл с именем `secret.yml` например, как показано в следующем примере:

```yml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

Наконец, этот файл можно загрузить в Kubernetes, выполнив следующую команду:

```console
kubectl apply -f ./secret.yaml
```

Эти секреты затем можно подключить к томам или предоставить процессам контейнеров через переменные среды. Подход к созданию приложений из [двенадцати факторов](https://12factor.net/) предполагает использование наименьшего общего знаменателя для передачи параметров в приложение. Переменные среды — это самый низкий общий знаменатель, так как они поддерживаются независимо от операционной системы или приложения.

Альтернативой использованию встроенных секретов Kubernetes является доступ к секретам в Azure Key Vault из Kubernetes. Самый простой способ сделать это — присвоить роль RBAC контейнеру для загрузки секретов. Затем приложение может использовать Azure Key Vault интерфейсы API для доступа к секретам. Однако этот подход требует внесения изменений в код и не соответствует шаблону использования переменных среды. Вместо этого можно внедрять значения в контейнер с помощью [Azure Key Vault инжектор](https://mrdevops.io/introducing-azure-key-vault-to-kubernetes-931f82364354). Этот подход на самом деле более безопасен, чем использование секретов Kubernetes напрямую, так как к ним могут обращаться пользователи в кластере.

## <a name="encryption-in-transit-and-at-rest"></a>Шифрование при передаче и при хранении

Обеспечение безопасности данных имеет большое значение, так как оно находится на диске или перемещается между различными службами. Самый эффективный способ избежать утечки данных — это шифровать их в формате, который не может быть легко прочитан другими пользователями. Azure поддерживает широкий спектр вариантов шифрования.

### <a name="in-transit"></a>При передаче

Существует несколько способов шифрования трафика в сети в Azure. Доступ к службам Azure обычно осуществляется через соединения, использующие протокол TLS. Например, для всех подключений к API Azure требуются TLS-подключения. Аналогично, подключения к конечным точкам в службе хранилища Azure могут быть ограничены для работы только через TLS зашифрованные соединения.

TLS — это сложный протокол, который просто гарантирует, что соединение использует TLS, не достаточным для обеспечения безопасности. Например, TLS 1,0 стабильно небезопасно, а TLS 1,1 не намного лучше. Даже в версиях TLS существуют различные параметры, которые могут упростить расшифровку подключений. Лучше всего проверить, использует ли подключение к серверу обновленные и правильно настроенные протоколы.

Эту проверку можно выполнить с помощью внешней службы, такой как проверка сервера SSL в лаборатории SSL. Тестовый запуск для обычной конечной точки Azure, в данном случае конечной точки служебной шины, дает близкий к идеальному показателю.

Даже службы, такие как базы данных SQL Azure, используют шифрование TLS для сохранения скрытых данных. Интересная часть, посвященная шифрованию данных при передаче с помощью TLS, заключается в том, что невозможно даже в корпорации Майкрософт ожидать подключения между компьютерами, на которых выполняется TLS. Это должно быть удобным для компаний, которые связаны с тем, что их данные могут быть подвержены риску от Microsoft правильного или даже субъекта штата с большим количеством ресурсов, чем у стандартного злоумышленника.

![рис. 10-5. отчет по лабораториям SSL, в котором показана оценка для конечной точки служебной шины.](./media/ssl-report.png)
**рис. 10-5**. Отчет о лабораториях SSL, в котором показана оценка для конечной точки служебной шины.

Хотя этот уровень шифрования не будет достаточным для всего времени, он должен вдохновить уверенность в том, что подключения Azure TLS являются весьма безопасными. Azure продолжит развивать свои стандарты безопасности по мере улучшения шифрования. Хорошо, что кто – то уже смотрит стандарты безопасности и обновляет Azure по мере их улучшения.

### <a name="at-rest"></a>Неактивных

В любом приложении существует ряд мест, где данные хранятся на диске. Сам код приложения загружается из механизма хранения. В большинстве приложений также используется некоторый тип базы данных, например SQL Server, Cosmos DB или даже эффективное экономичное хранилище таблиц. Все эти базы данных используют сильно зашифрованное хранилище, чтобы гарантировать, что никто, кроме приложений с соответствующими разрешениями, сможет считывать данные. Даже системные операторы не могут считывать данные, которые были зашифрованы. Поэтому клиенты могут оставаться уверенными в том, что секретные данные остаются секретными.

### <a name="storage"></a>Хранилище

Лежащая в Azure является подсистемой хранилища Azure. Диски виртуальной машины подключены поверх хранилища Azure. Службы Azure Kubernetes Services выполняются на виртуальных машинах, которые сами располагаются в службе хранилища Azure. Даже бессерверные технологии, такие как приложения функций Azure и экземпляры контейнеров Azure, запустили диск, который входит в состав службы хранилища Azure.

Если служба хранилища Azure хорошо зашифрована, она предоставляет основу для большинства других элементов, которые также должны быть зашифрованы. Служба хранилища Azure [шифруется](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) с помощью стандарта [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140) , совместимого с [256-разрядным AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). Это хорошо организованная технология шифрования, которая была предметом обширной ознакомительной версии за последние 20 лет. В настоящее время нет известной практичной атаки, которая позволит пользователям без знания ключа читать данные, зашифрованные с помощью AES.

По умолчанию ключи, используемые для шифрования хранилища Azure, управляются корпорацией Майкрософт. Для предотвращения вредоносного доступа к этим ключам существуют широкие возможности защиты. Однако пользователи с определенными требованиями к шифрованию также могут [предоставлять собственные ключи хранения](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-powershell) , управляемые в Azure Key Vault. Эти ключи могут быть отозваны в любое время, что эффективно выводит содержимое учетной записи хранения, используя недоступные данные.

Виртуальные машины используют зашифрованное хранилище, но можно предоставить другой уровень шифрования с помощью таких технологий, как BitLocker в Windows или DM-Encryption в Linux. Эти технологии означают, что даже если образ диска был утечкой из хранилища, он останется практически недоступен для чтения.

### <a name="azure-sql"></a>Azure SQL

Базы данных, размещенные в Azure SQL, используют технологию, именуемую [прозрачное шифрование данных (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption) , чтобы гарантировать, что данные останутся зашифрованными. Он включен по умолчанию во всех вновь создаваемых базах данных SQL, но должен быть включен вручную для устаревших баз данных. TDE выполняет шифрование и расшифровку в режиме реального времени не только для базы данных, но также резервных копий и журналов транзакций.

Параметры шифрования хранятся в базе данных `master` и во время запуска считываются в память для выполнения оставшихся операций. Это означает, что база данных `master` должна остаться незашифрованной. Фактический ключ управляется корпорацией Майкрософт. Тем не менее пользователи, имеющие точные требования к безопасности, могут предоставить собственный ключ в Key Vault практически так же, как и для службы хранилища Azure. Key Vault предоставляет такие службы, как смена ключа и отзыв.

"Прозрачная" часть TDS связана с тем, что нет изменений клиента, необходимых для использования зашифрованной базы данных. Хотя такой подход обеспечивает хорошую безопасность, для расшифровки данных пользователям достаточно утечки пароля базы данных. Существует другой подход, который шифрует отдельные столбцы или таблицы в базе данных. [Always encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) гарантирует, что зашифрованные данные не отображаются в виде обычного текста внутри базы данных.

Настройка этого уровня шифрования требует запуска мастера в SQL Server Management Studio для выбора типа шифрования и места в Key Vault для хранения связанных ключей.

![рис. 10-6. Выбор столбцов в таблице для шифрования с помощью Always Encrypted](./media/always-encrypted.png)
**рис. 10-6**. Выбор столбцов в таблице для шифрования с помощью Always Encrypted.

Клиентским приложениям, считывающим информацию из этих зашифрованных столбцов, необходимо предоставить специальные квоты для чтения зашифрованных данных. Строки подключения необходимо обновить с `Column Encryption Setting=Enabled` а учетные данные клиента необходимо получить из Key Vault. После этого клиент SQL Server должен быть зашифрован с помощью ключей шифрования столбцов. После этого оставшиеся действия используют стандартные интерфейсы для клиента SQL. Таким образом, такие средства, как Dapper и Entity Framework, построенные на основе клиента SQL, продолжат работать без изменений. Always Encrypted может быть еще не доступна для каждого драйвера SQL Server на каждом языке.

Сочетание TDE и Always Encrypted, которые можно использовать с клиентскими ключами, гарантирует, что поддерживаются даже наиболее точные требования к шифрованию.

### <a name="cosmos-db"></a>Cosmos DB

Cosmos DB — самая новая база данных, предоставляемая корпорацией Майкрософт в Azure. Он был создан с нуля с учетом безопасности и шифрования. Шифрование AES-256bit является стандартным для всех баз данных Cosmos DB и не может быть отключено. В сочетании с требованием TLS 1,2 для обмена данными все решение хранилища шифруется.

![рис. 10-7. поток шифрования данных в Cosmos DB](./media/cosmos-encryption.png)
**рис. 10-7**. Поток шифрования данных в Cosmos DB.

Несмотря на то, что Cosmos DB не предоставляет ключи шифрования клиентов, группа выполнила значительную работу, чтобы обеспечить соответствие требованиям PCI-DSS без этого. Cosmos DB также не поддерживает любое шифрование одного столбца, аналогичное Always Encrypted SQL Azure.

## <a name="keeping-secure"></a>Обеспечение безопасности

В Azure есть все необходимые средства для выпуска высокобезопасного продукта. Однако цепочка строго надежна, чем ее слабая ссылка. Если приложения, развернутые на основе Azure, не разрабатывались с надлежащей защитой и хорошим аудитом безопасности, то они становятся слабой связью в цепочке. Существует множество замечательных средств статического анализа, библиотек шифрования и методов обеспечения безопасности, которые можно использовать, чтобы убедиться, что программное обеспечение, установленное в Azure, защищено как само Azure. Примеры включают в себя [статические средства анализа](https://www.whitesourcesoftware.com/), [библиотеки шифрования](https://www.libressl.org/)и [методики безопасности](https://azure.microsoft.com/resources/videos/red-vs-blue-internal-security-penetration-testing-of-microsoft-azure/).

>[!div class="step-by-step"]
>[Назад](security.md)
>[Вперед](devops.md)