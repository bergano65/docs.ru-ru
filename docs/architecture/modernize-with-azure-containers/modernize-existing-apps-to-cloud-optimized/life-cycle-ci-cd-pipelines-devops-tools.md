---
title: Модернизация жизненного цикла приложений с помощью конвейеров непрерывной интеграции и непрерывного развертывания и средств DevOps в облаке
description: Модернизация существующих приложений .NET с помощью облака Azure и контейнеров Windows | Модернизация жизненного цикла приложений с помощью конвейеров непрерывной интеграции и непрерывного развертывания и средств DevOps в облаке
ms.date: 04/30/2018
ms.openlocfilehash: 17a78c108bfc61471128a34191ec7a5d7cc28289
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77503846"
---
# <a name="modernize-your-apps-lifecycle-with-cicd-pipelines-and-devops-tools-in-the-cloud"></a>Модернизация жизненного цикла приложений с помощью конвейеров непрерывной интеграции и непрерывного развертывания и средств DevOps в облаке

Современные компании должны быстро внедрять инновации, чтобы быть конкурентоспособными на рынке. Для предоставления высококачественных современных приложений требуются средства и процессы DevOps, которые важны для реализации этого постоянного цикла инноваций. С помощью правильных средств DevOps разработчики могут упростить непрерывное развертывание и быстро предоставлять инновационные приложения пользователям.

Несмотря на наличие хорошо устоявшихся методов непрерывной интеграции и развертывания внедрение контейнеров требует учета новых аспектов, особенно при работе с многоконтейнерными приложениями.

Azure DevOps Services поддерживает непрерывную интеграцию и развертывание многоконтейнерных приложений в различных средах с помощью официальных задач развертывания:

- [развертывание в решении "Веб-приложения Azure для контейнеров"](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-docker-webapp?tabs=dotnet-core);

- [развертывание в Службе Azure Kubernetes](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-aks?tabs=dotnet-core).

Но вы также можете выполнить развертывание в [Docker Swarm](https://blog.jcorioland.io/archives/2016/11/29/full-ci-cd-pipeline-to-deploy-multi-containers-application-on-azure-container-service-docker-swarm-using-visual-studio-team-services.html) или DC/OS с помощью задач Azure DevOps Services на основе скриптов.

Чтобы улучшить гибкость развертывания, эти средства предоставляют отличные возможности развертывания рабочих нагрузок контейнеров в средах разработки, тестирования и, наконец, в рабочей, а также различные решения для разработки, непрерывной интеграции и доставки.

На рис. 4-12 показан конвейер непрерывного развертывания, который выполняет развертывание в кластере Kubernetes в Службе контейнеров Azure.

![Снимок экрана, где показан конвейер Azure DevOps Services, осуществляющий развертывание в кластере Kubernetes.](./media/life-cycle-ci-cd-pipelines-devops-tools/deploy-mvc-app-container-kubernetes.png)

**Рис. 4-12**. Конвейер непрерывного развертывания Azure DevOps Services, осуществляющий развертывание в кластере Kubernetes

>[!div class="step-by-step"]
>[Назад](modernize-your-apps-with-monitoring-and-telemetry.md)
>[Вперед](migrate-to-hybrid-cloud-scenarios.md)
