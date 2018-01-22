---
title: "Выбор между ASP.NET и ASP.NET Core"
author: rick-anderson
description: "Как выбрать между ASP.NET и ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: c909c9a852549577c4a9fbc461aaf3f710b301ef
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="d5152-103">Выбор между ASP.NET и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5152-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="d5152-104">Независимо от того, какое веб-приложение вы создаете, ASP.NET предлагает вам решение: от корпоративных веб-приложений, предназначенных для Windows Server, до небольших микрослужб, предназначенных для контейнеров Linux, и все остальное.</span><span class="sxs-lookup"><span data-stu-id="d5152-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="d5152-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5152-105">ASP.NET Core</span></span>

<span data-ttu-id="d5152-106">ASP.NET Core — это кроссплатформенная среда на основе исходного кода для создания современных облачных веб-приложений в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="d5152-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="d5152-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5152-107">ASP.NET</span></span>

<span data-ttu-id="d5152-108">ASP.NET — это развитая платформа, предоставляющая все необходимые службы для создания серверных веб-приложений корпоративного класса в Windows.</span><span class="sxs-lookup"><span data-stu-id="d5152-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="d5152-109">Какой вариант подойдет мне лучше всего?</span><span class="sxs-lookup"><span data-stu-id="d5152-109">Which one is right for me?</span></span>

| <span data-ttu-id="d5152-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5152-110">ASP.NET Core</span></span> | <span data-ttu-id="d5152-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5152-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="d5152-112">Предназначена для Windows, macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="d5152-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="d5152-113">Предназначена для Windows</span><span class="sxs-lookup"><span data-stu-id="d5152-113">Build for Windows</span></span>|
|<span data-ttu-id="d5152-114">[Razor Pages](xref:mvc/razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d5152-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="d5152-115">См. также [MVC](xref:mvc/overview) и [веб-API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="d5152-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="d5152-116">Использование [веб-форм](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [веб-API](https://docs.microsoft.com/aspnet/web-api/) или [веб-страниц](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="d5152-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="d5152-117">Несколько версий для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="d5152-117">Multiple versions per machine</span></span>|<span data-ttu-id="d5152-118">Одна версия для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="d5152-118">One version per machine</span></span>|
|<span data-ttu-id="d5152-119">Разработка в Visual Studio, [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) или [Visual Studio Code](https://code.visualstudio.com/) с использованием C# или F#</span><span class="sxs-lookup"><span data-stu-id="d5152-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="d5152-120">Разработка с Visual Studio с использованием C#, VB и F#</span><span class="sxs-lookup"><span data-stu-id="d5152-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="d5152-121">Более высокая производительность, чем в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5152-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="d5152-122">Хорошая производительность</span><span class="sxs-lookup"><span data-stu-id="d5152-122">Good performance</span></span>|
|[<span data-ttu-id="d5152-123">Выбор среды выполнения .NET Framework или .NET Core</span><span class="sxs-lookup"><span data-stu-id="d5152-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="d5152-124">Использование среды выполнения .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d5152-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="d5152-125">Сценарии ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5152-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="d5152-126">[Страницы Razor](xref:mvc/razor-pages/index) являются рекомендуемым методом для создания пользовательского веб-интерфейса в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d5152-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="d5152-127">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="d5152-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="d5152-128">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="d5152-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="d5152-129">Сценарии ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5152-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="d5152-130">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="d5152-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="d5152-131">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="d5152-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="d5152-132">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="d5152-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="d5152-133">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="d5152-133">Resources</span></span>

* [<span data-ttu-id="d5152-134">Введение в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5152-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="d5152-135">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5152-135">Introduction to ASP.NET Core</span></span>](xref:index)