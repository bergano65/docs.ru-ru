---
title: Переход на гибридные облачные сценарии
description: Модернизация существующих приложений .NET с помощью облака Azure и контейнеров Windows | Переход на гибридные облачные сценарии
ms.date: 04/30/2018
ms.openlocfilehash: dcbb799a45609f8bb811866c4041951abf1fda8b
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937366"
---
# <a name="migrate-to-hybrid-cloud-scenarios"></a>Переход на гибридные облачные сценарии

Некоторые организации и предприятия не могут перенести некоторые из своих приложений в общедоступные облака, такие как Microsoft Azure или любое другое, из-за нормативных требований или собственных политик. Тем не менее, вполне вероятно, что любой организации может быть выгодно использовать некоторые приложения в общедоступном облаке, а некоторые — в локальной среде. Однако смешанная среда может привести к чрезмерной сложности из-за различных платформ и технологий, используемых в общедоступных облаках и локальных средах.

Корпорация Майкрософт предоставляет лучшее гибридное облачное решение, позволяющее оптимизировать существующие ресурсы локально и в общедоступном облаке, обеспечивая согласованность в гибридном облаке Azure. Вы можете расширить имеющиеся навыки и получить гибкий унифицированный подход к сборке приложений, которые могут работать в облаке или локальной среде, благодаря Azure Stack (в локальной среде) и службе Azure (в общедоступном облаке).

Когда речь заходит о безопасности, вы можете централизовать управление и обеспечение безопасности в гибридном облаке. Вы можете получить контроль над всеми ресурсами (от центра обработки данных до облака), реализовав единый вход в локальные и облачные приложения. Это достигается путем расширения Active Directory в гибридное облако и управления идентификаторами.

Наконец, вы можете легко распространять и анализировать данные, использовать одни и те же языки запросов для облачных и локальных ресурсов, а также применять аналитику и глубокое обучение в Azure для дополнения ваших данных независимо от их источника.

## <a name="azure-stack"></a>Azure Stack

Azure Stack — это гибридная облачная платформа, которая позволяет доставлять службы Azure из корпоративного центра обработки данных. Платформа Azure Stack предназначена для поддержки новых возможностей для современных приложений в ключевых сценариях, таких как пограничные и отключенные среды, или для соответствия определенным требованиям безопасности и законодательства.

На рис. 4-13 показан обзор реальной гибридной облачной платформы, предоставляемой корпорацией Майкрософт.

![Схема гибридной облачной платформы Майкрософт с Azure Stack и Azure.](./media/migrate-to-hybrid-cloud-scenarios/microsoft-hybrid-cloud-platform.png)

**Рис. 4-13.** Гибридная облачная платформа Майкрософт с Azure Stack и Azure

Для соответствия вашим потребностям Azure Stack предлагается в двух вариантах развертывания:

- интегрированные системы Azure Stack;

- Пакет средств разработки Azure Stack.

### <a name="azure-stack-integrated-systems"></a>Интегрированные системы Azure Stack

Интегрированные системы Azure Stack предлагаются благодаря партнерству корпорации Майкрософт с партнерами по оборудованию. Это партнерство позволило создать решение, которое предлагает облачные инновации в сочетании с простотой управления. Так как Azure Stack предлагается в форме интегрированной системы с программным и аппаратным обеспечением, вы получаете необходимый уровень гибкости и контроля, поддерживающий внедрение инноваций на базе облака. Размер интегрированных систем Azure Stack варьируется от 4 до 12 узлов. Эти системы поддерживаются партнером по оборудованию и корпорацией Майкрософт. Использование интегрированных систем Azure Stack позволяет реализовать новые сценарии рабочих нагрузок.

### <a name="azure-stack-development-kit"></a>Пакет средств разработки Azure Stack

Пакет средств разработки Microsoft Azure Stack — это развертывание Azure Stack с одним узлом, которое можно использовать для оценки и изучения Azure Stack. Кроме того, Пакет средств разработки Azure Stack можно использовать как среду разработки, в которой можно разрабатывать решения с помощью API-интерфейсов и инструментов, совместимых с Azure. Пакет средств разработки Azure Stack не предназначен для использования в качестве рабочей среды.

### <a name="additional-resources"></a>Дополнительные ресурсы

- **Гибридное облако Azure**

    <https://azure.microsoft.com/overview/hybrid-cloud/>

- **Azure Stack**

    <https://azure.microsoft.com/overview/azure-stack/>

- **Учетные записи службы Active Directory для контейнеров Windows**

    <https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts>

- **Создание контейнера с поддержкой Active Directory**

    <https://docs.microsoft.com/archive/blogs/containerstuff/create-a-container-with-active-directory-support>

- **Лицензия по программе "Преимущество гибридного использования Azure"**

    <https://azure.microsoft.com/pricing/hybrid-benefit/>

>[!div class="step-by-step"]
>[Назад](life-cycle-ci-cd-pipelines-devops-tools.md)
>[Вперед](../walkthroughs-technical-get-started-overview.md)
