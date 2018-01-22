---
title: "Ведение журналов в ASP.NET Core"
author: ardalis
description: "Сведения о платформе ведения журналов в ASP.NET Core. Ознакомьтесь со встроенными поставщиками ведения журналов и получите подробные сведения о распространенных сторонних поставщиках."
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 387d19af9165d4b54ce3cb1a9b04412271da6fb0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="b3bf5-104">Общие сведения о ведении журналов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3bf5-104">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="b3bf5-105">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="b3bf5-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b3bf5-106">ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="b3bf5-107">Встроенные поставщики позволяют отправлять журналы в одно или несколько мест назначений. Можно подключить стороннюю платформу ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="b3bf5-108">В этой статье содержатся сведения об использовании в коде встроенного API и поставщиков ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3bf5-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3bf5-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3bf5-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3bf5-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="b3bf5-113">Создание журналов</span><span class="sxs-lookup"><span data-stu-id="b3bf5-113">How to create logs</span></span>

<span data-ttu-id="b3bf5-114">Чтобы создать журналы, получите объект `ILogger` из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="b3bf5-114">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="b3bf5-115">Затем в этом объекте средства ведения журнала вызовите методы ведения журналов:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="b3bf5-116">В этом примере создаются журналы с классом `TodoController` в качестве *категории*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="b3bf5-117">Категории рассматриваются [далее в этой статье](#log-category).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="b3bf5-118">ASP.NET Core не предоставляет асинхронные методы ведения журналов, так как ведение журналов должно выполняться настолько быстро, что использовать асинхронный метод нецелесообразно.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-118">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="b3bf5-119">Если возникла обратная ситуация, рекомендуется рассмотреть возможность изменения способа ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="b3bf5-120">Если хранилище данных работает медленно, сначала записывайте сообщения журнала в быстродействующее хранилище, а затем перемещайте их в медленное хранилище.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="b3bf5-121">Например, выполняйте запись в очередь сообщений, которые считываются и сохраняются в медленном хранилище другим процессом.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-121">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="b3bf5-122">Добавление поставщиков</span><span class="sxs-lookup"><span data-stu-id="b3bf5-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3bf5-124">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="b3bf5-125">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="b3bf5-126">Для использования поставщика вызовите метод расширения `Add<ProviderName>` поставщика в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="b3bf5-127">Шаблон проекта по умолчанию включает ведение журнала с помощью метода[CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-127">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3bf5-129">Поставщик ведения журнала принимает сообщения, созданные с помощью объекта `ILogger`, и отображает или сохраняет их.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="b3bf5-130">Например, поставщик Console отображает сообщения на консоли, а поставщик службы приложений Azure может сохранить их в хранилище больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="b3bf5-131">Чтобы использовать поставщик, установите его пакет NuGet и вызовите метод расширения поставщика в экземпляре `ILoggerFactory`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="b3bf5-132">[Внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) ASP.NET Core предоставляет экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="b3bf5-133">Методы расширения `AddConsole` и `AddDebug` определены в пакетах [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) и [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="b3bf5-134">Каждый метод расширения вызывает метод `ILoggerFactory.AddProvider`, передавая экземпляр поставщика.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="b3bf5-135">В примере приложения в этой статье поставщики ведения журналов добавляются в метод `Configure` класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-135">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="b3bf5-136">Чтобы получить выходные данные журнала из выполняемого ранее кода, добавьте поставщики ведения журналов в конструктор класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-136">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="b3bf5-137">Сведения о каждом [встроенном поставщике ведения журнала](#built-in-logging-providers) и ссылки на [сторонние поставщики](#third-party-logging-providers) приведены далее в статье.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="b3bf5-138">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="b3bf5-138">Sample logging output</span></span>

<span data-ttu-id="b3bf5-139">Используя пример кода из предыдущего раздела, вы увидите журналы в консоли после запуска из командной строки.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="b3bf5-140">Ниже приведен пример выходных данных консоли:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-140">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
<span data-ttu-id="b3bf5-141">Эти журналы были созданы при переходе по адресу `http://localhost:5000/api/todo/0`, который запускает выполнение обоих вызовов `ILogger`, приведенных в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="b3bf5-142">Ниже приведен пример тех же журналов в том виде, как они отображаются в окне отладки при запуске примера приложения в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="b3bf5-143">Журналы, которые были созданы путем вызовов `ILogger`, приведенных в предыдущем разделе, начинаются с "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="b3bf5-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="b3bf5-144">Журналы, которые начинаются с категорий "Microsoft", входят в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="b3bf5-145">В ASP.NET Core и коде приложения используется один и тот же API ведения журнала и одни и те же поставщики.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="b3bf5-146">В оставшейся части этой статьи рассматриваются некоторые сведения и параметры для ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="b3bf5-147">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="b3bf5-147">NuGet packages</span></span>

<span data-ttu-id="b3bf5-148">Интерфейсы `ILogger` и `ILoggerFactory` находятся в [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), а их реализации по умолчанию — в [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="b3bf5-149">Категория журнала</span><span class="sxs-lookup"><span data-stu-id="b3bf5-149">Log category</span></span>

<span data-ttu-id="b3bf5-150">*Категория* входит в состав каждого создаваемого журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="b3bf5-151">Категория указывается при создании объекта `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="b3bf5-152">Категория может быть любой строкой, однако действует соглашение по использованию полного имени класса, из которого записываются журналы.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="b3bf5-153">Например: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="b3bf5-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="b3bf5-154">Можно указать категорию в виде строки или использовать метод расширения, который наследует категорию от типа.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="b3bf5-155">Чтобы указать категорию как строку, вызовите `CreateLogger` в экземпляре `ILoggerFactory`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="b3bf5-156">В большинстве случаев будет проще использовать `ILogger<T>`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="b3bf5-157">Это эквивалентно вызову `CreateLogger` с полным именем типа `T`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="b3bf5-158">Уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="b3bf5-158">Log level</span></span>

<span data-ttu-id="b3bf5-159">Каждый раз при создании журнала указывается его значение перечисления [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-159">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="b3bf5-160">Уровень ведения журнала означает степень серьезности или важности.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="b3bf5-161">Например, можно создать журнал `Information` при нормальном завершении метода, журнал `Warning`, если метод возвращает код 404, и журнал `Error` при перехвате непредвиденного исключения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="b3bf5-162">В следующем примере кода имена методов (например, `LogWarning`) указывают уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="b3bf5-163">Первый параметр — это [ идентификатор события журнала](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="b3bf5-164">Второй параметр — это [шаблон сообщения](#log-message-template) с заполнителями для значений аргументов, предоставляемых оставшимися параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="b3bf5-165">Параметры метода рассматриваются более подробно далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="b3bf5-166">Методы ведения журнала, которые содержат уровень в имени метода, являются [методами расширения для ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-166">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="b3bf5-167">На самом деле эти методы вызывают метод `Log`, принимающий параметр `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="b3bf5-168">Метод `Log`, в отличие от этих методов расширения, можно вызывать напрямую, однако его синтаксис довольно сложен.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="b3bf5-169">Дополнительные сведения см. в разделе [Интерфейс ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) и [Исходный код расширений средства ведения журналов](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-169">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="b3bf5-170">В ASP.NET Core определяются следующие [уровни ведения журнала](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), упорядоченные в этом документе от наименьшего до наибольшего уровня серьезности.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-170">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="b3bf5-171">Трассировка = 0</span><span class="sxs-lookup"><span data-stu-id="b3bf5-171">Trace = 0</span></span>

  <span data-ttu-id="b3bf5-172">Для сведений, которые полезны, только если разработчик выполняет отладку проблемы.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-172">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="b3bf5-173">Эти сообщения могут содержать конфиденциальные данные приложения, поэтому их не следует включать в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-173">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="b3bf5-174">*По умолчанию отключено.*</span><span class="sxs-lookup"><span data-stu-id="b3bf5-174">*Disabled by default.*</span></span> <span data-ttu-id="b3bf5-175">Пример: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="b3bf5-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="b3bf5-176">Отладка = 1</span><span class="sxs-lookup"><span data-stu-id="b3bf5-176">Debug = 1</span></span>

  <span data-ttu-id="b3bf5-177">Для сведений, которые полезны в краткосрочной перспективе во время разработки и отладки.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="b3bf5-178">Пример: `Entering method Configure with flag set to true.` как правило, журналы уровня `Debug` включаются в рабочей среде только при устранении неполадок.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-178">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="b3bf5-179">Информация = 2</span><span class="sxs-lookup"><span data-stu-id="b3bf5-179">Information = 2</span></span>

  <span data-ttu-id="b3bf5-180">Для отслеживания общего хода выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="b3bf5-181">Эти журналы обычно полезны в долгосрочной перспективе.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="b3bf5-182">Пример: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="b3bf5-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="b3bf5-183">Предупреждение = 3</span><span class="sxs-lookup"><span data-stu-id="b3bf5-183">Warning = 3</span></span>

  <span data-ttu-id="b3bf5-184">Для нестандартных или непредвиденных событий, возникающих при выполнении приложения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="b3bf5-185">В их число могут входить ошибки или другие условия, которые не приводят к остановке приложения, но которые может потребоваться изучить.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-185">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="b3bf5-186">Обработанные исключения являются распространенным условием использования уровня ведения журнала `Warning`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="b3bf5-187">Пример: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="b3bf5-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="b3bf5-188">Ошибка = 4</span><span class="sxs-lookup"><span data-stu-id="b3bf5-188">Error = 4</span></span>

  <span data-ttu-id="b3bf5-189">Для ошибок и исключений, которые не могут быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="b3bf5-190">Эти сообщения указывают на сбой текущего действия или операции (например, текущего HTTP-запроса), а не ошибку уровня приложения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="b3bf5-191">Пример сообщения журнала: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="b3bf5-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="b3bf5-192">Критический = 5</span><span class="sxs-lookup"><span data-stu-id="b3bf5-192">Critical = 5</span></span>

  <span data-ttu-id="b3bf5-193">Для сбоев, которые требуют немедленного внимания.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-193">For failures that require immediate attention.</span></span> <span data-ttu-id="b3bf5-194">Примеры: потеря данных, нехватка места на диске.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="b3bf5-195">Уровень ведения журнала можно использовать для управления объемом выходных данных, записываемых на определенный носитель или в окно отображения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="b3bf5-196">Например, в рабочей среде может потребоваться, чтобы все журналы уровня `Information` и ниже записывались в хранилище томов, а все журналы уровня `Warning` и выше записывались в хранилище значений.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="b3bf5-197">Во время разработки можно отправлять журналы `Warning` или более высоких уровней серьезности на консоль.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="b3bf5-198">Затем, если потребуется устранить неполадки, можно добавить уровень `Debug`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="b3bf5-199">В разделе [Фильтрация журналов](#log-filtering) далее в этой статье приводятся сведения о том, как контролировать уровни журнала, обрабатываемые поставщиком.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="b3bf5-200">Платформа ASP.NET Core создает журналы уровня `Debug` для событий платформы.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="b3bf5-201">В примерах журнала ранее в этой статье не указывались журналы с уровнем ниже `Information`, поэтому журналы уровня `Debug` не отображались.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="b3bf5-202">Ниже приведен пример журналов консоли при запуске примера приложения, настроенного для отображения журналов уровня `Debug` и выше для поставщика Console.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="b3bf5-203">Идентификатор события журнала</span><span class="sxs-lookup"><span data-stu-id="b3bf5-203">Log event ID</span></span>

<span data-ttu-id="b3bf5-204">При каждом создании журнала можно указывать *идентификатор события*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="b3bf5-205">В примере приложения для этого используется локально определенный класс `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="b3bf5-206">Идентификатор события — это целочисленное значение, используемое для связи набора зарегистрированных событий друг с другом.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="b3bf5-207">Например, журнал для добавления позиции в корзину покупок может иметь идентификатор события 1000, а журнал для завершения покупки может иметь идентификатор события 1001.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="b3bf5-208">В зависимости от поставщика идентификатор события может быть сохранен в поле или включен в текст сообщения в выходных данных журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="b3bf5-209">Поставщик Debug не отображает идентификаторы событий, но поставщик Console показывает их в квадратных скобках после категории:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="b3bf5-210">Шаблон сообщения журнала</span><span class="sxs-lookup"><span data-stu-id="b3bf5-210">Log message template</span></span>

<span data-ttu-id="b3bf5-211">Каждый раз при записи сообщения журнала предоставляется шаблон сообщения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="b3bf5-212">Шаблон сообщения может быть строкой либо содержать именованные заполнители, в которые помещаются значения аргументов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="b3bf5-213">Шаблон не является строкой формата, а заполнители должны быть именованными, а не нумерованными.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="b3bf5-214">Параметры, используемые для предоставления значений, определяются порядком заполнителей, а не их именами.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="b3bf5-215">Предположим, что имеется следующий код:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="b3bf5-216">Полученное в результате сообщение журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="b3bf5-217">Платформа ведения журнала поддерживает такое форматирование сообщений, чтобы поставщики ведения журналов могли реализовывать [семантическое ведение журналов, также известное как структурированное ведение журнала](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="b3bf5-218">Так как в систему ведения журналов передаются сами аргументы, а не только отформатированный шаблон сообщения, поставщики ведения журналов могут сохранять значения параметров как поля (наряду с шаблоном сообщения).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="b3bf5-219">Если выходные данных журналов направляются в хранилище таблиц Azure, вызов метода средства ведения журнала выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="b3bf5-220">Каждая сущность таблицы Azure может иметь свойства `ID` и `RequestTime`, что упрощает выполнение запросов к данным журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="b3bf5-221">Можно находить все журналы в пределах определенного диапазона `RequestTime`, не анализируя время ожидания текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="b3bf5-222">Ведение журнала исключений</span><span class="sxs-lookup"><span data-stu-id="b3bf5-222">Logging exceptions</span></span>

<span data-ttu-id="b3bf5-223">Методы средства ведения журнала имеют перегрузки, которые позволяют передавать исключение, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="b3bf5-224">Разные поставщики обрабатывают сведения об исключениях по-разному.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="b3bf5-225">Ниже приведен пример выходных данных поставщика Debug из приведенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="b3bf5-226">Фильтрация журналов</span><span class="sxs-lookup"><span data-stu-id="b3bf5-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3bf5-228">Можно указать минимальный уровень ведения журнала для определенного поставщика и категории или для всех поставщиков или всех категорий.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="b3bf5-229">Все журналы с уровнем ниже минимального не передаются этому поставщику, поэтому они не отображаются и не сохраняются.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="b3bf5-230">Чтобы запретить все журналы, укажите `LogLevel.None` в качестве минимального уровня ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="b3bf5-231">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="b3bf5-232">**Создание правил фильтрации в конфигурации**</span><span class="sxs-lookup"><span data-stu-id="b3bf5-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="b3bf5-233">Шаблоны проектов создают код, который вызывает `CreateDefaultBuilder` для настройки ведения журналов для поставщиков Console и Debug.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="b3bf5-234">Метод `CreateDefaultBuilder` также настраивает ведение журнала для просмотра конфигурации в разделе `Logging`, используя код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="b3bf5-235">Данные конфигурации указывают минимальные уровни ведения журнала для каждого поставщика и категории, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="b3bf5-236">Этот JSON создает шесть правил фильтрации: одно для поставщика Debug, четыре для поставщика Console и одно, которое применяется ко всем поставщикам.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="b3bf5-237">Далее вы увидите, как при создании объекта `ILogger` для каждого поставщика выбирается лишь одно из этих правил.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="b3bf5-238">**Правила фильтрации в коде**</span><span class="sxs-lookup"><span data-stu-id="b3bf5-238">**Filter rules in code**</span></span>

<span data-ttu-id="b3bf5-239">Можно зарегистрировать правила фильтрации в коде, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="b3bf5-240">Второй `AddFilter` указывает поставщика, Debug, используя имя его типа.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="b3bf5-241">Первый `AddFilter` применяется ко всем поставщикам, так как он не определяет тип поставщика.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="b3bf5-242">**Применение правил фильтрации**</span><span class="sxs-lookup"><span data-stu-id="b3bf5-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="b3bf5-243">Данные конфигурации и код `AddFilter`, приведенный в предыдущих примерах, создают правила, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="b3bf5-244">Первые шесть взяты из примера конфигурации, а последние два — из примера кода.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="b3bf5-245">Число</span><span class="sxs-lookup"><span data-stu-id="b3bf5-245">Number</span></span> | <span data-ttu-id="b3bf5-246">Поставщик</span><span class="sxs-lookup"><span data-stu-id="b3bf5-246">Provider</span></span>      | <span data-ttu-id="b3bf5-247">Категории, которые начинаются с...</span><span class="sxs-lookup"><span data-stu-id="b3bf5-247">Categories that begin with ...</span></span>          | <span data-ttu-id="b3bf5-248">Минимальный уровень ведения журнала</span><span class="sxs-lookup"><span data-stu-id="b3bf5-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="b3bf5-249">1</span><span class="sxs-lookup"><span data-stu-id="b3bf5-249">1</span></span>      | <span data-ttu-id="b3bf5-250">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-250">Debug</span></span>         | <span data-ttu-id="b3bf5-251">Все категории</span><span class="sxs-lookup"><span data-stu-id="b3bf5-251">All categories</span></span>                          | <span data-ttu-id="b3bf5-252">Сведения</span><span class="sxs-lookup"><span data-stu-id="b3bf5-252">Information</span></span>       |
| <span data-ttu-id="b3bf5-253">2</span><span class="sxs-lookup"><span data-stu-id="b3bf5-253">2</span></span>      | <span data-ttu-id="b3bf5-254">Консоль</span><span class="sxs-lookup"><span data-stu-id="b3bf5-254">Console</span></span>       | <span data-ttu-id="b3bf5-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="b3bf5-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="b3bf5-256">Предупреждение</span><span class="sxs-lookup"><span data-stu-id="b3bf5-256">Warning</span></span>           |
| <span data-ttu-id="b3bf5-257">3</span><span class="sxs-lookup"><span data-stu-id="b3bf5-257">3</span></span>      | <span data-ttu-id="b3bf5-258">Консоль</span><span class="sxs-lookup"><span data-stu-id="b3bf5-258">Console</span></span>       | <span data-ttu-id="b3bf5-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="b3bf5-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="b3bf5-260">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-260">Debug</span></span>             |
| <span data-ttu-id="b3bf5-261">4</span><span class="sxs-lookup"><span data-stu-id="b3bf5-261">4</span></span>      | <span data-ttu-id="b3bf5-262">Консоль</span><span class="sxs-lookup"><span data-stu-id="b3bf5-262">Console</span></span>       | <span data-ttu-id="b3bf5-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="b3bf5-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="b3bf5-264">Error</span><span class="sxs-lookup"><span data-stu-id="b3bf5-264">Error</span></span>             |
| <span data-ttu-id="b3bf5-265">5</span><span class="sxs-lookup"><span data-stu-id="b3bf5-265">5</span></span>      | <span data-ttu-id="b3bf5-266">Консоль</span><span class="sxs-lookup"><span data-stu-id="b3bf5-266">Console</span></span>       | <span data-ttu-id="b3bf5-267">Все категории</span><span class="sxs-lookup"><span data-stu-id="b3bf5-267">All categories</span></span>                          | <span data-ttu-id="b3bf5-268">Сведения</span><span class="sxs-lookup"><span data-stu-id="b3bf5-268">Information</span></span>       |
| <span data-ttu-id="b3bf5-269">6</span><span class="sxs-lookup"><span data-stu-id="b3bf5-269">6</span></span>      | <span data-ttu-id="b3bf5-270">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="b3bf5-270">All providers</span></span> | <span data-ttu-id="b3bf5-271">Все категории</span><span class="sxs-lookup"><span data-stu-id="b3bf5-271">All categories</span></span>                          | <span data-ttu-id="b3bf5-272">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-272">Debug</span></span>             |
| <span data-ttu-id="b3bf5-273">7</span><span class="sxs-lookup"><span data-stu-id="b3bf5-273">7</span></span>      | <span data-ttu-id="b3bf5-274">Все поставщики</span><span class="sxs-lookup"><span data-stu-id="b3bf5-274">All providers</span></span> | <span data-ttu-id="b3bf5-275">Система</span><span class="sxs-lookup"><span data-stu-id="b3bf5-275">System</span></span>                                  | <span data-ttu-id="b3bf5-276">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-276">Debug</span></span>             |
| <span data-ttu-id="b3bf5-277">8</span><span class="sxs-lookup"><span data-stu-id="b3bf5-277">8</span></span>      | <span data-ttu-id="b3bf5-278">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-278">Debug</span></span>         | <span data-ttu-id="b3bf5-279">Майкрософт</span><span class="sxs-lookup"><span data-stu-id="b3bf5-279">Microsoft</span></span>                               | <span data-ttu-id="b3bf5-280">Трассировка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-280">Trace</span></span>             |

<span data-ttu-id="b3bf5-281">При создании объекта `ILogger`, используемого для создания журналов, объект `ILoggerFactory` выбирает одно правило для каждого поставщика, которое будет применено к этому средству ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="b3bf5-282">Все сообщения, записываемые с помощью этого объекта `ILogger`, фильтруются на основе выбранных правил.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="b3bf5-283">Самое подходящее правило для каждой пары поставщика и категории выбирается из списка доступных правил.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="b3bf5-284">При создании `ILogger` для данной категории для каждого поставщика используется приведенный далее алгоритм:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="b3bf5-285">Выберите все правила, которые соответствуют поставщику или его псевдониму.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="b3bf5-286">Если ничего не найдено, выберите все правила с пустым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="b3bf5-287">В результатах предыдущего шага выберите правила с самым длинным соответствующим префиксом категории.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="b3bf5-288">Если ничего не найдено, выберите все правила, которые не указывают категорию.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="b3bf5-289">Если выбрано несколько правил, примите **последнее**.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="b3bf5-290">Если правила не выбраны, используйте `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="b3bf5-291">Предположим, что у вас есть указанный выше список правил и вы создаете объект `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="b3bf5-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="b3bf5-292">К поставщику Debug применяются правила 1, 6 и 8.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="b3bf5-293">Правило 8 является наиболее подходящим, поэтому выбрано оно.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="b3bf5-294">К поставщику отладки применяются правила 3, 4, 5 и 6.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="b3bf5-295">Правило 3 является наиболее подходящим.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="b3bf5-296">При создании журналов с помощью `ILogger` для категории "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" журналы уровня `Trace` и выше перейдут поставщику отладки, а журналы уровня `Debug` и выше перейдут поставщику консоли.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="b3bf5-297">**Псевдонимы поставщиков**</span><span class="sxs-lookup"><span data-stu-id="b3bf5-297">**Provider aliases**</span></span>

<span data-ttu-id="b3bf5-298">Имя типа можно использовать, чтобы указать поставщик в конфигурации, но каждый поставщик определяет более короткий и простой в использовании *псевдоним*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="b3bf5-299">Для встроенных поставщиков используйте следующие псевдонимы:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="b3bf5-300">Консоль</span><span class="sxs-lookup"><span data-stu-id="b3bf5-300">Console</span></span>
- <span data-ttu-id="b3bf5-301">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-301">Debug</span></span>
- <span data-ttu-id="b3bf5-302">EventLog</span><span class="sxs-lookup"><span data-stu-id="b3bf5-302">EventLog</span></span>
- <span data-ttu-id="b3bf5-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="b3bf5-303">AzureAppServices</span></span>
- <span data-ttu-id="b3bf5-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="b3bf5-304">TraceSource</span></span>
- <span data-ttu-id="b3bf5-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="b3bf5-305">EventSource</span></span>

<span data-ttu-id="b3bf5-306">**Минимальный уровень по умолчанию**</span><span class="sxs-lookup"><span data-stu-id="b3bf5-306">**Default minimum level**</span></span>

<span data-ttu-id="b3bf5-307">Существует параметр минимального уровня, который вступает в силу только в том случае, если к данному поставщику или категории не применяются никакие правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="b3bf5-308">В следующем примере показано, как задать минимальный уровень:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="b3bf5-309">Если минимальный уровень не задан явным образом, используется значение по умолчанию — `Information`, означающее, что журналы `Trace` и `Debug` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="b3bf5-310">**Функции фильтрации**</span><span class="sxs-lookup"><span data-stu-id="b3bf5-310">**Filter functions**</span></span>

<span data-ttu-id="b3bf5-311">В функции фильтрации можно написать код для применения правил фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="b3bf5-312">Функция фильтрации вызывается для всех поставщиков и категорий, которым не назначены правила из конфигурации или кода.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="b3bf5-313">Код в функции имеет доступ к типу поставщика, категории и уровню ведения журнала, чтобы определить необходимость регистрации сообщений.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="b3bf5-314">Пример:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3bf5-316">Некоторые поставщики ведения журналов позволяют указывать, когда журналы следует записывать на носитель, а когда — игнорировать. При этом учитывается уровень ведения журнала и категория.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="b3bf5-317">Методы расширения `AddConsole` и `AddDebug`предоставляют перегрузки для передачи условий фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="b3bf5-318">В следующем примере кода поставщик Console игнорирует журналы с уровнем ниже `Warning`, а поставщик Debug не учитывает журналы, создаваемые платформой.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="b3bf5-319">Метод `AddEventLog` имеет перегрузку, принимающую экземпляр `EventLogSettings`, который в своем свойстве `Filter` может содержать функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="b3bf5-320">Поставщик TraceSource не предоставляет ни одну из этих перегрузок, так как его уровень ведения журнала и другие параметры основаны на используемых им `SourceSwitch` и `TraceListener`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="b3bf5-321">Можно задать правила фильтрации для всех поставщиков, которые зарегистрированы в экземпляре `ILoggerFactory`, с помощью метода расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="b3bf5-322">В приведенном ниже примере журналы платформы (категория начинается с "Microsoft" или "System") ограничиваются предупреждениями, однако разрешаются журналы приложений на уровне отладки.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="b3bf5-323">Чтобы использовать фильтрацию для предотвращения создания всех журналов для определенной категории, укажите `LogLevel.None` в качестве минимального уровня ведения журнала для этой категории.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="b3bf5-324">`LogLevel.None` равно целочисленному значению 6, то есть больше, чем `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="b3bf5-325">Пакет NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) предоставляет метод расширения `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="b3bf5-326">Метод возвращает новый экземпляр `ILoggerFactory` для фильтрации журнала сообщений, переданного всем поставщикам средства ведения журналов, которые зарегистрированы в этом экземпляре.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="b3bf5-327">Он не влияет на другие экземпляры `ILoggerFactory`, включая исходный экземпляр `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="b3bf5-328">Области журналов</span><span class="sxs-lookup"><span data-stu-id="b3bf5-328">Log scopes</span></span>

<span data-ttu-id="b3bf5-329">Для присоединения одних и тех же данных к каждому журналу, созданному в рамках набора, можно сгруппировать набор логических операций в *область*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span> <span data-ttu-id="b3bf5-330">Например, может потребоваться, чтобы каждый журнал, созданный в ходе обработки транзакции, содержал идентификатор транзакции.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="b3bf5-331">Область — это тип `IDisposable`, возвращаемый методом `ILogger.BeginScope<TState>` и действующий до окончательного уничтожения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="b3bf5-332">Чтобы использовать область, следует заключить вызовы средства ведения журналов в блок `using`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="b3bf5-333">Следующий код предоставляет области для поставщика Console:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3bf5-335">В файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="b3bf5-336">Для включения ведения журнала на уровне области требуется параметр `IncludeScopes` средства ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="b3bf5-337">Настройка `IncludeScopes` с помощью файлов конфигурации *appsettings* будет доступна в выпуске ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3bf5-339">В файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="b3bf5-340">Каждое сообщение журнала содержит ограниченную информацию:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="b3bf5-341">Встроенные поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="b3bf5-341">Built-in logging providers</span></span>

<span data-ttu-id="b3bf5-342">В состав ASP.NET Core входят следующие поставщики:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="b3bf5-343">Консоль</span><span class="sxs-lookup"><span data-stu-id="b3bf5-343">Console</span></span>](#console)
* [<span data-ttu-id="b3bf5-344">Отладка</span><span class="sxs-lookup"><span data-stu-id="b3bf5-344">Debug</span></span>](#debug)
* [<span data-ttu-id="b3bf5-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="b3bf5-345">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="b3bf5-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="b3bf5-346">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="b3bf5-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="b3bf5-347">TraceSource</span></span>](#tracesource)
* <span data-ttu-id="b3bf5-348">[служба приложений Azure](#appservice);</span><span class="sxs-lookup"><span data-stu-id="b3bf5-348">[Azure App Service](#appservice)</span></span>

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="b3bf5-349">Поставщик Console</span><span class="sxs-lookup"><span data-stu-id="b3bf5-349">The console provider</span></span>

<span data-ttu-id="b3bf5-350">Пакет поставщика [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) отправляет выходные данные журнала на консоль.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="b3bf5-353">[Перегрузки AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) позволяют передавать минимальный уровень ведения журнала, функцию фильтрации и логическое значение, указывающее, поддерживаются ли области.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-353">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="b3bf5-354">Другим вариантом является передача объекта `IConfiguration`, который может указать поддержку областей и уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="b3bf5-355">Если вы собираетесь использовать поставщик Console в рабочей среде, имейте в виду, что он оказывает значительное влияние на производительность.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="b3bf5-356">При создании проекта в Visual Studio метод `AddConsole` выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="b3bf5-357">Этот код ссылается на раздел `Logging` файла *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="b3bf5-358">Приведенные параметры ограничивают журналы платформы предупреждениями, но разрешают регистрацию приложений на уровне отладки, как описано в разделе [Фильтрация журналов](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="b3bf5-359">Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="b3bf5-360">Поставщик Debug</span><span class="sxs-lookup"><span data-stu-id="b3bf5-360">The Debug provider</span></span>

<span data-ttu-id="b3bf5-361">Пакет поставщика [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) записывает выходные данные журналов с помощью класса [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (вызовов метода `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="b3bf5-362">В Linux этот поставщик записывает журналы в каталог */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="b3bf5-365">[Перегрузки AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) позволяют передавать минимальный уровень ведения журнала или функцию фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-365">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="b3bf5-366">Поставщик EventSource</span><span class="sxs-lookup"><span data-stu-id="b3bf5-366">The EventSource provider</span></span>

<span data-ttu-id="b3bf5-367">Для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздних версий, реализовывать события трассировки можно с помощью пакета поставщика [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="b3bf5-368">В Windows используется [трассировка событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="b3bf5-369">Поставщик является кроссплатформенным, но для Linux и macOS инструменты сбора событий и отображений пока отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="b3bf5-372">Для сбора и просмотра данных журналов рекомендуется использовать [программу PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="b3bf5-373">Существуют и другие средства для просмотра журналов трассировки событий Windows, но PerfView обеспечивает максимальное удобство работы с событиями трассировки событий Windows, создаваемыми ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="b3bf5-374">Чтобы настроить PerfView для сбора событий, регистрируемых этим поставщиком, добавьте строку `*Microsoft-Extensions-Logging` в список **Дополнительные поставщики**.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="b3bf5-375">(Не пропустите звездочку в начале строки.)</span><span class="sxs-lookup"><span data-stu-id="b3bf5-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Дополнительные поставщики Perfview](index/_static/perfview-additional-providers.png)

<span data-ttu-id="b3bf5-377">Для записи событий на сервере Nano Server следует выполнить дополнительную настройку:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-377">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="b3bf5-378">Подключите PowerShell для удаленной работы к Nano Server:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-378">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="b3bf5-379">Создайте сеанс трассировки событий Windows:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-379">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="b3bf5-380">Добавьте поставщики трассировки событий Windows для [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core и другие, если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-380">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="b3bf5-381">Поставщик ASP.NET Core имеет GUID `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-381">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="b3bf5-382">Запустите сайт и выполните нужные действия, для которых требуется получить сведения трассировки.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-382">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="b3bf5-383">Остановите сеанс трассировки после окончания сбора данных:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-383">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="b3bf5-384">Итоговый файл *C:\trace.etl* можно проанализировать в PerfView и других выпусках Windows.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-384">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="b3bf5-385">Поставщик Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="b3bf5-385">The Windows EventLog provider</span></span>

<span data-ttu-id="b3bf5-386">Пакет поставщика [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) отправляет выходные данные журнала в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-387">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-387">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-388">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-388">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="b3bf5-389">[Перегрузки AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) позволяют передавать `EventLogSettings` или минимальный уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-389">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="b3bf5-390">Поставщик TraceSource</span><span class="sxs-lookup"><span data-stu-id="b3bf5-390">The TraceSource provider</span></span>

<span data-ttu-id="b3bf5-391">Пакет поставщика [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) использует библиотеки и поставщики [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-391">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-392">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-392">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-393">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-393">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="b3bf5-394">[Перегрузки AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) позволяют передавать исходный параметр и прослушиватель трассировки.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-394">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="b3bf5-395">Чтобы использовать этот поставщик, приложение должно выполняться в .NET Framework (а не в .NET Core).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-395">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="b3bf5-396">Поставщик позволяет перенаправлять сообщения различным [прослушивателям](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), например [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr), который используется в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-396">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="b3bf5-397">В следующем примере показана настройка поставщика `TraceSource`, записывающего в окне консоли сообщения типа `Warning` и сообщения с более высоким уровнем серьезности.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-397">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="b3bf5-398">Поставщик службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="b3bf5-398">The Azure App Service provider</span></span>

<span data-ttu-id="b3bf5-399">Пакет поставщика [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) записывает журналы в текстовые файлы в файловой системе приложения службы приложений Azure и в [хранилище больших двоичных объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) в учетной записи хранения Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-399">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="b3bf5-400">Поставщик доступен только для приложений, предназначенных для ASP.NET Core 1.1.0 или более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-400">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3bf5-401">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-401">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3bf5-402">Если планируется использовать .NET Core, не нужно устанавливать пакет поставщика или явным образом вызвать `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-402">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="b3bf5-403">Поставщик становится автоматически доступным для приложения при развертывании приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-403">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="b3bf5-404">Если планируется использовать .NET Framework, добавьте пакет поставщика в проект и вызовите `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-404">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3bf5-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3bf5-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="b3bf5-406">Перегрузка `AddAzureWebAppDiagnostics` позволяет передавать [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) для переопределения настроек по умолчанию, таких как шаблон выходных данных ведения журнала, имя BLOB-объекта и максимально допустимый размер файла.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="b3bf5-407">(*Шаблон выходных данных* — это шаблон сообщений, который применяется ко всем журналам на основе того, который вы указали при вызове метода `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="b3bf5-407">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="b3bf5-408">При развертывании в приложение службы приложений ваше приложение учитывает параметры в разделе [Журналы диагностики](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) на странице **Служба приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="b3bf5-409">Изменения этих параметров вступают в силу сразу же после внесения и не требуют перезапуска приложения или повторного развертывания в нем кода.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Параметры ведения журнала Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="b3bf5-411">По умолчанию файлы журнала находятся в папке *D:\\home\\LogFiles\\Application*, а имя файла по умолчанию — *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="b3bf5-412">Максимальный размер файла по умолчанию составляет 10 МБ, а максимальное количество сохраняемых по умолчанию файлов равно 2.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="b3bf5-413">Имя BLOB-объекта по умолчанию — *{имя_приложения}{метка_времени}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="b3bf5-414">Дополнительные сведения о поведении по умолчанию см. в разделе [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="b3bf5-415">Поставщик работает только при выполнении проекта в среде Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-415">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="b3bf5-416">Он не функционирует при запуске в локальной среде &mdash;; запись в локальные файлы или в локальное хранилище развертывания для больших двоичных объектов не производится.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="b3bf5-417">Сторонние поставщики ведения журналов</span><span class="sxs-lookup"><span data-stu-id="b3bf5-417">Third-party logging providers</span></span>

<span data-ttu-id="b3bf5-418">Ниже приведены некоторые сторонние платформы ведения журналов, которые работают с ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="b3bf5-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) — поставщик для службы Elmah.Io.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="b3bf5-420">[JSNLog](http://jsnlog.com) — регистрирует в серверном журнале исключения JavaScript и другие клиентские события.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="b3bf5-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) — поставщик для службы Loggr.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="b3bf5-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) — поставщик для библиотеки NLog.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="b3bf5-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) — поставщик для библиотеки Serilog.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="b3bf5-424">Некоторые сторонние платформы поддерживают [семантическое ведение журналов, также известное как структурированное ведение журналов](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="b3bf5-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="b3bf5-425">Использование сторонней платформы аналогично использованию одного из встроенных поставщиков: добавьте пакет NuGet в проект и вызовите метод расширения в `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="b3bf5-426">Дополнительные сведения см. в документации по каждой платформе.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="b3bf5-427">Для поддержки других платформ ведения журналов или собственных требований к ведению журналов можно создать собственных пользовательских поставщиков.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="b3bf5-428">Потоковая передача журналов Azure</span><span class="sxs-lookup"><span data-stu-id="b3bf5-428">Azure log streaming</span></span>

<span data-ttu-id="b3bf5-429">Потоковая передача журналов Azure позволяет просматривать активность журнала в режиме реального времени из следующих источников:</span><span class="sxs-lookup"><span data-stu-id="b3bf5-429">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="b3bf5-430">сервер приложений;</span><span class="sxs-lookup"><span data-stu-id="b3bf5-430">The application server</span></span> 
* <span data-ttu-id="b3bf5-431">веб-сервер;</span><span class="sxs-lookup"><span data-stu-id="b3bf5-431">The web server</span></span>
* <span data-ttu-id="b3bf5-432">трассировка неудачно завершенных запросов.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-432">Failed request tracing</span></span> 

<span data-ttu-id="b3bf5-433">Настройка потоковой передачи журналов Azure</span><span class="sxs-lookup"><span data-stu-id="b3bf5-433">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="b3bf5-434">Со страницы портала приложения перейдите на страницу **Журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="b3bf5-435">Включите параметр **Ведение журнала приложения (файловая система)**.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-435">Set **Application Logging (Filesystem)** to on.</span></span> 

![Страница журналов диагностики на портале Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="b3bf5-437">Перейдите на страницу **Потоковая передача журналов**, чтобы просмотреть сообщения приложения.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="b3bf5-438">Они записываются в приложении с помощью интерфейса `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="b3bf5-438">They are logged by application through the `ILogger` interface.</span></span> 

![Потоковая передача журнала приложения на портале Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="b3bf5-440">См. также</span><span class="sxs-lookup"><span data-stu-id="b3bf5-440">See also</span></span>

[<span data-ttu-id="b3bf5-441">Высокопроизводительное ведение журналов с помощью LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="b3bf5-441">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)