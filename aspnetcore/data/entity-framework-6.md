---
title: "Начало работы с ASP.NET Core и Entity Framework 6"
author: tdykstra
description: "В этой статье показано, как использовать платформу Entity Framework 6 в приложении ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 02/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: data/entity-framework-6
ms.openlocfilehash: 7407fe8a976978d7d5077d5e5ac6cc264565621d
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Начало работы с ASP.NET Core и Entity Framework 6

Авторы: [Павел Грудзень](https://github.com/pgrudzien12) (Paweł Grudzień), [Дэмиен Понтифекс](https://github.com/DamienPontifex) (Damien Pontifex) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)

В этой статье показано, как использовать платформу Entity Framework 6 в приложении ASP.NET Core.

## <a name="overview"></a>Обзор

Чтобы использовать платформу Entity Framework 6, проект необходимо компилировать на основе .NET Framework, поскольку Entity Framework 6 не поддерживает .NET Core. Если вам требуются какие-либо кроссплатформенные функции, необходимо выполнить обновление до [Entity Framework Core](https://docs.microsoft.com/ef/).

Рекомендуемый способ использования платформы Entity Framework 6 в приложении ASP.NET Core заключается в помещении контекста и классов модели EF6 в проект библиотеки классов, который предназначен для полной платформы. Добавьте ссылку на библиотеку классов из проекта ASP.NET Core. См. пример [решения Visual Studio с проектами EF6 и ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Контекст EF6 нельзя поместить в проект ASP.NET Core, поскольку проекты .NET Core поддерживают не все функции, используемые командами EF6, например *Enable-Migrations*.

Независимо от типа проекта, в котором размещается контекст EF6, в этом контексте работают только средства командной строки EF6. Например, команда `Scaffold-DbContext` доступна только в Entity Framework Core. Дополнительные сведения о реконструировании базы данных в модель EF6 см. в разделе [Code First для существующей базы данных](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Использование ссылок на полную платформу и EF6 в проекте ASP.NET Core

В проекте ASP.NET Core необходимо использовать ссылки на платформу .NET Framework и EF6. Например, файл *CSPROJ* вашего проекта ASP.NET Core будет иметь вид, схожий с показанным в следующем примере (показаны только соответствующие части файла).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

При создании нового проекта используйте шаблон **Веб-приложение ASP.NET Core (.NET Framework)**.

## <a name="handle-connection-strings"></a>Обработка строк подключения

Средства командной строки EF6, которые используются в проекте библиотеки классов EF6, требуют наличия конструктора по умолчанию, с помощью которого они могут создавать экземпляр контекста. Тем не менее, возможно, вам потребуется задать строку подключения для использования в проекте ASP.NET Core. В этом случае конструктор контекста должен иметь параметр, в котором передается строка подключения. Ниже приведен пример.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Поскольку в контексте EF6 отсутствует конструктор без параметров, в проекте EF6 необходимо предоставить реализацию [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Средства командной строки EF6 будут находить и использовать эту реализацию для создания экземпляра контекста. Ниже приведен пример.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

В этом примере кода реализация `IDbContextFactory` передает жестко запрограммированную строку подключения. Эта строка подключения будет использоваться средствами командной строки. В некоторых случаях требуется реализовать стратегию, в которой библиотека классов использует ту же строку подключения, что и вызывающее приложение. Например, вы можете получать значение из переменной среды в обоих проектах.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Настройка внедрения зависимостей в проекте ASP.NET Core

В файле *Startup.cs* проекта Core настройте контекст EF6 для внедрения зависимостей в `ConfigureServices`. Объекты контекста EF должны иметь время существования, соответствующее запросу.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

После этого можно получить экземпляр контекста в контроллерах посредством внедрения зависимостей. Этот код похож на тот, который вы написали для контекста EF Core:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Пример приложения

Рабочий пример приложения см. в разделе [пример решения Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/), который прилагается к этой статье.

Этот пример можно создать с нуля, выполнив следующие действия в Visual Studio:

* Создайте решение.

* **Добавить новый проект > Интернет > Веб-приложение ASP.NET Core (.NET Framework)**

* **Добавить новый проект > Классический рабочий стол Windows > Библиотека классов (.NET Framework)**

* В **консоли диспетчера пакетов** для обоих проектов выполните команду `Install-Package Entityframework`.

* В проекте библиотеки классов создайте классы модели данных и класс контекста, а также реализацию `IDbContextFactory`.

* В консоли диспетчера пакетов для проекта библиотеки классов выполните команды `Enable-Migrations` и `Add-Migration Initial`. Если проект ASP.NET Core настроен как запускаемый, добавьте `-StartupProjectName EF6` в эти команды.

* В проекте Core добавьте ссылку на проект библиотеки классов.

* В проекте Core в файле *Startup.cs* зарегистрируйте контекст для внедрения зависимостей.

* В проекте Core в файле *appsettings.json* добавьте строку подключения.

* В проекте Core добавьте контроллер и представление (или представления), чтобы проверить возможность чтения и записи данных. (Обратите внимание, что функция формирования шаблонов ASP.NET Core MVC не будет работать с контекстом EF6, на который указывает ссылка из библиотеки классов.)

## <a name="summary"></a>Сводка

В этой статье приводятся общие рекомендации по использованию платформы Entity Framework 6 в приложении ASP.NET Core.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Entity Framework — конфигурация на основе кода](https://msdn.microsoft.com/data/jj680699.aspx)
