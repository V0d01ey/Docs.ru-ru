---
title: "Реализация веб-сервера, HTTP.sys, в ASP.NET Core"
author: rick-anderson
description: "Вводит HTTP.sys, веб-сервер для ASP.NET Core в Windows. Основанное на драйвер режима ядра Http.Sys, HTTP.sys — это альтернатива Kestrel, который можно использовать для прямого подключения к Интернету, без IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="88eee-104">Реализация веб-сервера, HTTP.sys, в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88eee-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="88eee-105">По [Tom Dykstra](https://github.com/tdykstra) и [Ross Крис](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="88eee-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="88eee-106">Этот раздел относится только к ASP.NET Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="88eee-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="88eee-107">В более ранних версиях ASP.NET Core, называется HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="88eee-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="88eee-108">Компонент HTTP.sys — [веб-сервер для ASP.NET Core](index.md) , которое будет выполняться только в Windows.</span><span class="sxs-lookup"><span data-stu-id="88eee-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="88eee-109">Она была основана на [драйвер режима ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="88eee-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="88eee-110">Компонент HTTP.sys — это альтернатива [Kestrel](kestrel.md) , где есть некоторые функции, которые не Kestel.</span><span class="sxs-lookup"><span data-stu-id="88eee-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="88eee-111">**HTTP.sys не может использоваться с IIS или IIS Express, поскольку он не совместим с [модуль ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="88eee-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="88eee-112">HTTP.sys поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="88eee-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="88eee-113">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="88eee-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="88eee-114">Совместное использование порта</span><span class="sxs-lookup"><span data-stu-id="88eee-114">Port sharing</span></span>
- <span data-ttu-id="88eee-115">HTTPS с сетевого Адаптера</span><span class="sxs-lookup"><span data-stu-id="88eee-115">HTTPS with SNI</span></span>
- <span data-ttu-id="88eee-116">HTTP/2 через TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="88eee-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="88eee-117">Файл прямой передачи данных</span><span class="sxs-lookup"><span data-stu-id="88eee-117">Direct file transmission</span></span>
- <span data-ttu-id="88eee-118">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="88eee-118">Response caching</span></span>
- <span data-ttu-id="88eee-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="88eee-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="88eee-120">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="88eee-120">Supported Windows versions:</span></span>

- <span data-ttu-id="88eee-121">Windows 7 и Windows Server 2008 R2 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="88eee-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="88eee-122">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="88eee-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="88eee-123">Когда следует использовать HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="88eee-123">When to use HTTP.sys</span></span>

<span data-ttu-id="88eee-124">HTTP.sys полезен для развертываний, где нужно предоставить сервера к Интернету напрямую без использования служб IIS.</span><span class="sxs-lookup"><span data-stu-id="88eee-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="88eee-126">Так как она была основана на Http.Sys, HTTP.sys не требует обратного прокси-сервера для защиты от атак на систему.</span><span class="sxs-lookup"><span data-stu-id="88eee-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="88eee-127">Компонент Http.Sys — это надежная технология, которое защищает от атак многих типов, а также обеспечивает надежности, безопасности и масштабируемости полнофункциональное веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="88eee-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="88eee-128">Сами службы IIS выполняется как HTTP-прослушивателем на базе Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="88eee-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="88eee-129">Компонент HTTP.sys — хороший выбор для внутреннего развертывания при необходимости компонент, не поддерживаемых Kestrel, такие как проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="88eee-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="88eee-131">Как использовать HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="88eee-131">How to use HTTP.sys</span></span>

<span data-ttu-id="88eee-132">Ниже приведен обзор задач установки для ОС и приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88eee-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="88eee-133">Настройка Windows Server</span><span class="sxs-lookup"><span data-stu-id="88eee-133">Configure Windows Server</span></span>

* <span data-ttu-id="88eee-134">Установите версию .NET с требованиями приложения, такие как [.NET Core](https://www.microsoft.com/net/download/core) или [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="88eee-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="88eee-135">Предварительной регистрации префиксы URL-адрес, привязку к HTTP.sys и настроить сертификаты SSL</span><span class="sxs-lookup"><span data-stu-id="88eee-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="88eee-136">Если не предварительной регистрации URL-префиксы в Windows, необходимо запустить приложение с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="88eee-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="88eee-137">Единственное исключение — если привязать к локальному компьютеру с помощью HTTP (не HTTPS) с номерами выше 1024; номер порта в этом случае правами администратора, не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="88eee-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="88eee-138">Дополнительные сведения см. в разделе [предварительной регистрации префиксов и настройке SSL](#preregister-url-prefixes-and-configure-ssl) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="88eee-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="88eee-139">Открыть порты брандмауэра, разрешающее трафик для достижения HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="88eee-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="88eee-140">Можно использовать *netsh.exe* или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="88eee-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="88eee-141">Существуют также [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="88eee-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="88eee-142">Настройка приложения ASP.NET Core для использования HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="88eee-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="88eee-143">Программа установки пакета не требуется, при использовании [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="88eee-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="88eee-144">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) пакет включен в metapackage.</span><span class="sxs-lookup"><span data-stu-id="88eee-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="88eee-145">Вызовите `UseHttpSys` метод расширения в `WebHostBuilder` в вашей `Main` метод, указывая любой [параметры HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) , требуется, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="88eee-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="88eee-146">Настройка параметров HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="88eee-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="88eee-147">Ниже приведены некоторые параметры HTTP.sys и ограничений, которые можно настроить.</span><span class="sxs-lookup"><span data-stu-id="88eee-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="88eee-148">**Максимальное клиентских подключений**</span><span class="sxs-lookup"><span data-stu-id="88eee-148">**Maximum client connections**</span></span>

<span data-ttu-id="88eee-149">Можно задать максимальное число открытых подключений TCP для всего приложения на следующий код в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="88eee-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="88eee-150">Максимальное число подключений — неограниченное (null) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="88eee-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="88eee-151">**Размер максимального запроса**</span><span class="sxs-lookup"><span data-stu-id="88eee-151">**Maximum request body size**</span></span>

<span data-ttu-id="88eee-152">Размер максимального запроса текст по умолчанию — 30,000,000 байт которого составляет приблизительно 28.6 МБ.</span><span class="sxs-lookup"><span data-stu-id="88eee-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="88eee-153">Чтобы переопределить ограничение в приложении ASP.NET Core MVC рекомендуется использовать [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) атрибута в методе действия:</span><span class="sxs-lookup"><span data-stu-id="88eee-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="88eee-154">Ниже приведен пример, в котором показано, как настроить ограничения для всего приложения, каждый запрос:</span><span class="sxs-lookup"><span data-stu-id="88eee-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="88eee-155">Можно переопределить параметр на конкретный запрос в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="88eee-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="88eee-156">Исключение при попытке настроить ограничение на запрос после начала чтение запроса приложением.</span><span class="sxs-lookup"><span data-stu-id="88eee-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="88eee-157">Отсутствует `IsReadOnly` свойство, определяющее, если `MaxRequestBodySize` свойства находится в состоянии только для чтения, то есть ограничение уже слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="88eee-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="88eee-158">Сведения о других параметрах HTTP.sys см. в разделе [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="88eee-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="88eee-159">Настройка URL-адреса и порты прослушивания</span><span class="sxs-lookup"><span data-stu-id="88eee-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="88eee-160">По умолчанию ASP.NET Core привязывается к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="88eee-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="88eee-161">Чтобы настроить префиксы URL-адреса и порты, можно использовать `UseUrls` метод расширения `urls` аргумент командной строки, переменной среды ASPNETCORE_URLS или `UrlPrefixes` свойство [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="88eee-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="88eee-162">Следующий пример кода использует `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="88eee-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="88eee-163">Преимущество `UrlPrefixes` , могут быть получены сообщение об ошибке сразу же при попытке добавить префикс, который неправильно отформатирован.</span><span class="sxs-lookup"><span data-stu-id="88eee-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="88eee-164">Преимущество `UseUrls` (совместно используется с `urls` и ASPNETCORE_URLS) — что можно легко переключаться между Kestrel и HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="88eee-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="88eee-165">Если используются и `UseUrls` (или `urls` или ASPNETCORE_URLS) и `UrlPrefixes`, параметры в `UrlPrefixes` переопределить значения в `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="88eee-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="88eee-166">Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="88eee-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="88eee-167">Использует HTTP.sys [HTTP Server API UrlPrefix строковые форматы](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="88eee-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="88eee-168">Убедитесь, что указан одинаковые строки префикса в `UseUrls` или `UrlPrefixes` , вы предварительной регистрации на сервере.</span><span class="sxs-lookup"><span data-stu-id="88eee-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="88eee-169">Не используйте IIS</span><span class="sxs-lookup"><span data-stu-id="88eee-169">Don't use IIS</span></span>

<span data-ttu-id="88eee-170">Убедитесь, что приложение не настроено для запуска служб IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="88eee-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="88eee-171">В Visual Studio для IIS Express указан профиль запуска по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="88eee-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="88eee-172">Чтобы запустить проект как консольное приложение, вручную измените выбранного профиля, как показано на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="88eee-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Выберите профиль приложения консоли](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="88eee-174">Предварительной регистрации префиксы URL-адреса и настроить протокол SSL</span><span class="sxs-lookup"><span data-stu-id="88eee-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="88eee-175">Службы IIS и HTTP.sys основаны на базовый драйвер режима ядра Http.Sys для прослушивания запросов и первоначального обработки.</span><span class="sxs-lookup"><span data-stu-id="88eee-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="88eee-176">В службах IIS пользовательский Интерфейс управления позволяет сравнительно легко настраивать все.</span><span class="sxs-lookup"><span data-stu-id="88eee-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="88eee-177">Тем не менее необходимо настроить Http.Sys самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="88eee-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="88eee-178">Встроенное средство для выполнения, то есть *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="88eee-178">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="88eee-179">С *netsh.exe* можно зарезервировать префиксы URL-адрес и назначьте SSL-сертификаты.</span><span class="sxs-lookup"><span data-stu-id="88eee-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="88eee-180">Средство требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="88eee-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="88eee-181">Ниже приведен пример необходимый минимум для резервирования URL-префиксы для портов 80 и 443.</span><span class="sxs-lookup"><span data-stu-id="88eee-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="88eee-182">Приведенный ниже показано, как назначить SSL-сертификата:</span><span class="sxs-lookup"><span data-stu-id="88eee-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="88eee-183">Ниже приведен в справочной документации для *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="88eee-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="88eee-184">Команды Netsh для гипертекста передачи протокол (HTTP)</span><span class="sxs-lookup"><span data-stu-id="88eee-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="88eee-185">UrlPrefix строк</span><span class="sxs-lookup"><span data-stu-id="88eee-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="88eee-186">Подробные инструкции для нескольких сценариев на следующих ресурсах.</span><span class="sxs-lookup"><span data-stu-id="88eee-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="88eee-187">Статьи, которые ссылаются на HttpListener применяются при HTTP.sys, как они строятся на Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="88eee-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="88eee-188">Практическое руководство. Настройка порта с использованием SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="88eee-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="88eee-189">[Подключения по протоколу HTTPS - HttpListener и на основе размещения сертификацию клиента](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) это является блог сторонних разработчиков и довольно старый, но по-прежнему содержит полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="88eee-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="88eee-190">[Практическое руководство: Пошаговое руководство с помощью HttpListener или HTTP-сервере неуправляемого типов кода (C++) как простой сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) это тоже старые блог полезные сведения.</span><span class="sxs-lookup"><span data-stu-id="88eee-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="88eee-191">Ниже приведены некоторые сторонних средств, которые могут быть удобнее, чем *netsh.exe* командной строки.</span><span class="sxs-lookup"><span data-stu-id="88eee-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="88eee-192">Они не предоставляется или не поддерживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="88eee-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="88eee-193">Средства Запуск от имени администратора по умолчанию, поскольку *netsh.exe* сам требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="88eee-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="88eee-194">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский Интерфейс для включения в список и Настройка SSL-сертификатов и параметров, префиксов резервирования и списки доверия сертификатов.</span><span class="sxs-lookup"><span data-stu-id="88eee-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="88eee-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить сертификаты SSL и префиксы URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="88eee-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="88eee-196">Пользовательский Интерфейс будет более http.sys Manager предоставляет несколько дополнительных параметров конфигурации, а в противном случае она предоставляет аналогичные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="88eee-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="88eee-197">Не удается создать новый список доверия сертификатов (CTL), но можно назначить уже существующую.</span><span class="sxs-lookup"><span data-stu-id="88eee-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="88eee-198">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="88eee-198">Next steps</span></span>

<span data-ttu-id="88eee-199">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="88eee-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="88eee-200">Образец приложения для этой статьи</span><span class="sxs-lookup"><span data-stu-id="88eee-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="88eee-201">HTTP.sys исходного кода</span><span class="sxs-lookup"><span data-stu-id="88eee-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="88eee-202">Размещение</span><span class="sxs-lookup"><span data-stu-id="88eee-202">Hosting</span></span>](xref:fundamentals/hosting)