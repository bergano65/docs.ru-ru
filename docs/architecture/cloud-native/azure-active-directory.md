---
title: Azure Active Directory
description: Создание архитектуры облачных приложений .NET для Azure | Azure Active Directory
ms.date: 06/30/2019
ms.openlocfilehash: 207043507a9052c47683383a98cef6417a1a2740
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840733"
---
# <a name="azure-active-directory"></a>Azure Active Directory

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Microsoft Azure Active Directory (Azure AD) предлагает управление удостоверениями и доступом в качестве службы. Клиенты используют его для настройки и обслуживания пользователей, сведения о том, какие данные следует хранить, кто имеет доступ к этой информации, кто может управлять ею и какие приложения могут получить к ним доступ. AAD может проверять подлинность пользователей для приложений, настроенных на его использование, обеспечивая единый вход (SSO). Его можно использовать самостоятельно или интегрировать с Windows AD на локальном компьютере.

Azure AD создается для облака. Это действительно решение для идентификации в облаке, которое использует API Graph и синтаксис OData на основе запросов, в отличие от Windows AD, где используется протокол LDAP. Локальные Active Directory могут синхронизировать атрибуты пользователей с облаком с помощью служб синхронизации удостоверений, что позволяет выполнять всю проверку подлинности в облаке с помощью Azure AD. Кроме того, можно настроить проверку подлинности с помощью Connect для передачи в локальную Active Directory через ADFS для локального выполнения в Windows AD.

Azure AD поддерживает экраны входа с фирменной символикой компании, многозаводскую проверку подлинности и облачные прокси приложения, которые используются для предоставления единого входа для приложений, размещенных локально. Она предлагает различные виды создания отчетов о безопасности и оповещений.

## <a name="references"></a>Ссылки

- [Платформа Microsoft Identity](https://docs.microsoft.com/azure/active-directory/develop/)

>[!div class="step-by-step"]
>[Назад](authentication-authorization.md)
>[Вперед](identity-server.md)
