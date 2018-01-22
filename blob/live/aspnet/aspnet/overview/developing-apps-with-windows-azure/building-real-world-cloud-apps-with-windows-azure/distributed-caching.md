---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: "Распределенное кэширование (Создание реальных облачных приложений в Azure) | Документы Microsoft"
author: MikeWasson
description: "Построение реального мира облачными приложениями с помощью Azure электронная книга основан на разработанный Скотт Гатри презентации. Объясняет, 13 шаблоны и рекомендации, которые он может..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 923a8257376e98e6cae10d905f1cb18f7fdb28e7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="6d902-104">Распределенного кэширования (Создание реальных облачных приложений в Azure)</span><span class="sxs-lookup"><span data-stu-id="6d902-104">Distributed Caching (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="6d902-105">по [Mike Wasson](https://github.com/MikeWasson), [Рик Андерсон](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6d902-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6d902-106">[Загрузка решение проект](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) или [Загрузите электронную](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="6d902-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="6d902-107">**Построение реального мира облачными приложениями с помощью Azure** электронная книга основана на представления, разработанный Скотт Гатри.</span><span class="sxs-lookup"><span data-stu-id="6d902-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="6d902-108">Объясняются 13 шаблоны и рекомендации, которые помогут вам быть успешной разработки веб-приложений для облака.</span><span class="sxs-lookup"><span data-stu-id="6d902-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="6d902-109">Сведения об электронных книг см. в разделе [первой главы](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6d902-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="6d902-110">Предыдущей главе рассмотрены обработки временных сбоев и упомянутых кэширования в рамках стратегии Размыкатель цепи.</span><span class="sxs-lookup"><span data-stu-id="6d902-110">The previous chapter looked at transient fault handling and mentioned caching as a circuit breaker strategy.</span></span> <span data-ttu-id="6d902-111">В этой главе содержится Дополнительные сведения о кэшировании, а также условия использовать его, Общие шаблоны для использования и как реализовать ее в Azure.</span><span class="sxs-lookup"><span data-stu-id="6d902-111">This chapter gives more background about caching, including when to use it, common patterns for using it, and how to implement it in Azure.</span></span>

## <a name="what-is-distributed-caching"></a><span data-ttu-id="6d902-112">Что является распределенное кэширование</span><span class="sxs-lookup"><span data-stu-id="6d902-112">What is distributed caching</span></span>

<span data-ttu-id="6d902-113">Кэш обеспечивает высокой пропускной способностью и малой задержкой доступа к данным приложения с наиболее часто используемыми, сохраняя данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="6d902-113">A cache provides high throughput, low-latency access to commonly accessed application data, by storing the data in memory.</span></span> <span data-ttu-id="6d902-114">Для облачного приложения чаще тип кэша — распределенного кэша, поэтому данные не хранятся в памяти для отдельных веб сервера, но в других ресурсов облака и кэшированных данных становится доступным для всех веб-серверов приложения (или других облачных виртуальных машин, ar e используется приложением).</span><span class="sxs-lookup"><span data-stu-id="6d902-114">For a cloud app the most useful type of cache is distributed cache, which means the data is not stored on the individual web server's memory but on other cloud resources, and the cached data is made available to all of an application's web servers (or other cloud VMs that are used by the application).</span></span>

![Схема, показывающая нескольких веб-серверов, доступ к серверам же кэша](distributed-caching/_static/image1.png)

<span data-ttu-id="6d902-116">При масштабировании приложения, добавив или удалив серверы или серверы заменяются из-за обновлений или сбоев, кэшированные данные остаются доступными для каждого сервера, на котором выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="6d902-116">When the application scales by adding or removing servers, or when servers are replaced due to upgrades or faults, the cached data remains accessible to every server that runs the application.</span></span>

<span data-ttu-id="6d902-117">Рекомендуется избегать использования большая задержка доступа к данным хранилища постоянных данных, кэширование может существенно повысить скорость реагирования приложения.</span><span class="sxs-lookup"><span data-stu-id="6d902-117">By avoiding the high latency data access of a persistent data store, caching can dramatically improve application responsiveness.</span></span> <span data-ttu-id="6d902-118">Например получение данных из кэша намного быстрее, чем их извлечения из реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="6d902-118">For example, retrieving data from cache is much faster than retrieving it from a relational database.</span></span>

<span data-ttu-id="6d902-119">Дополнительное преимущество от кэширования уменьшается трафик в постоянное хранилище данных, что может привести к снижению затрат при наличии исходящих данных расходов для хранения постоянных данных.</span><span class="sxs-lookup"><span data-stu-id="6d902-119">A side benefit of caching is reduced traffic to the persistent data store, which may result in lower costs when there are data egress charges for the persistent data store.</span></span>

## <a name="when-to-use-distributed-caching"></a><span data-ttu-id="6d902-120">Когда следует использовать распределенное кэширование</span><span class="sxs-lookup"><span data-stu-id="6d902-120">When to use distributed caching</span></span>

<span data-ttu-id="6d902-121">Кэширование будет работать наилучшим образом для рабочих нагрузок приложений которого осуществляется чтение более чем записывать данные и если модель данных поддерживает организации ключ значение, которая используется для хранения и извлечения данных в кэше.</span><span class="sxs-lookup"><span data-stu-id="6d902-121">Caching works best for application workloads that do more reading than writing of data, and when the data model supports the key/value organization that you use to store and retrieve data in cache.</span></span> <span data-ttu-id="6d902-122">Он также полезные сведения, если пользователи приложения имеют много общих данных; например кэш не предоставляет столько преимущества Если каждого пользователя, как правило, получает данные, которые уникальны для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="6d902-122">It's also more useful when application users share a lot of common data; for example, cache would not provide as many benefits if each user typically retrieves data unique to that user.</span></span> <span data-ttu-id="6d902-123">Например, кэширование может быть очень полезными — это каталог товаров, так как данные изменяются редко, и все клиенты хотели бы на тех же данных.</span><span class="sxs-lookup"><span data-stu-id="6d902-123">An example where caching could be very beneficial is a product catalog, because the data does not change frequently, and all customers are looking at the same data.</span></span>

<span data-ttu-id="6d902-124">Преимущества кэширования становится все более измеримое более масштабирует приложения, как ограничения пропускной способности и задержки задержка на постоянное хранилище данных становятся более ограничение на общую производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="6d902-124">The benefit of caching becomes increasingly measurable the more an application scales, as the throughput limits and latency delays of the persistent data store become more of a limit on overall application performance.</span></span> <span data-ttu-id="6d902-125">Тем не менее можно реализовать кэширование по другим причинам, чем также производительности.</span><span class="sxs-lookup"><span data-stu-id="6d902-125">However, you might implement caching for other reasons than performance as well.</span></span> <span data-ttu-id="6d902-126">Для данных, не должен быть полностью актуальные, когда пользователь видит доступа к кэшу могут служить для Размыкатель цепи при на постоянное хранилище данных не отвечает или недоступен.</span><span class="sxs-lookup"><span data-stu-id="6d902-126">For data that doesn't have to be perfectly up-to-date when shown to the user, cache access can serve as a circuit breaker for when the persistent data store is unresponsive or unavailable.</span></span>

## <a name="popular-cache-population-strategies"></a><span data-ttu-id="6d902-127">Заполнение стратегии популярных кэша</span><span class="sxs-lookup"><span data-stu-id="6d902-127">Popular cache population strategies</span></span>

<span data-ttu-id="6d902-128">Чтобы иметь возможность получения данных из кэша, необходимо сначала сохранить ее наличие.</span><span class="sxs-lookup"><span data-stu-id="6d902-128">In order to be able to retrieve data from cache, you have to store it there first.</span></span> <span data-ttu-id="6d902-129">Существует несколько стратегий для получения данных, необходимых в кэш:</span><span class="sxs-lookup"><span data-stu-id="6d902-129">There are several strategies for getting data that you need into a cache:</span></span>

- <span data-ttu-id="6d902-130">По требованию / отдельно от кэша</span><span class="sxs-lookup"><span data-stu-id="6d902-130">On Demand / Cache Aside</span></span>

    <span data-ttu-id="6d902-131">Приложение пытается получить данные из кэша, и если кэш не имеет данных («промах»), приложение сохраняет данные в кэше, чтобы она будет доступна при очередном.</span><span class="sxs-lookup"><span data-stu-id="6d902-131">The application tries to retrieve data from cache, and when the cache doesn't have the data (a "miss"), the application stores the data in the cache so that it will be available the next time.</span></span> <span data-ttu-id="6d902-132">При очередном приложение пытается получить те же данные, он находит что следует искать в кэше («hit»).</span><span class="sxs-lookup"><span data-stu-id="6d902-132">The next time the application tries to get the same data, it finds what it's looking for in the cache (a "hit").</span></span> <span data-ttu-id="6d902-133">Чтобы предотвратить получение кэшированных данных, которые изменились в базе данных, кэш недействительным при внесении изменений в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="6d902-133">To prevent fetching cached data that has changed on the database, you invalidate the cache when making changes to the data store.</span></span>
- <span data-ttu-id="6d902-134">Принудительная отправка данных в фоновом режиме</span><span class="sxs-lookup"><span data-stu-id="6d902-134">Background Data Push</span></span>

    <span data-ttu-id="6d902-135">Фоновые службы Принудительная отправка данных в кэше по регулярному расписанию и приложение всегда получает из кэша.</span><span class="sxs-lookup"><span data-stu-id="6d902-135">Background services push data into the cache on a regular schedule, and the app always pulls from the cache.</span></span> <span data-ttu-id="6d902-136">Этот подход работает отлично с высокой задержкой источников данных, не требующие всегда возвращают самые последние данные.</span><span class="sxs-lookup"><span data-stu-id="6d902-136">This approach works great with high latency data sources that don't require you always return the latest data.</span></span>
- <span data-ttu-id="6d902-137">Размыкатель цепи</span><span class="sxs-lookup"><span data-stu-id="6d902-137">Circuit Breaker</span></span>

    <span data-ttu-id="6d902-138">Приложение нормально взаимодействует непосредственно с на постоянное хранилище данных, но если проблем с доступностью на постоянное хранилище данных, приложение извлекает данные из кэша.</span><span class="sxs-lookup"><span data-stu-id="6d902-138">The application normally communicates directly with the persistent data store, but when the persistent data store has availability problems, the application retrieves data from cache.</span></span> <span data-ttu-id="6d902-139">Данные могли быть помещены в кэш с помощью принудительной стратегии фона данных или выделить кэша.</span><span class="sxs-lookup"><span data-stu-id="6d902-139">Data may have been put in cache using either the cache aside or background data push strategy.</span></span> <span data-ttu-id="6d902-140">Это является ошибкой обработки стратегии, а не стратегии повышению производительности.</span><span class="sxs-lookup"><span data-stu-id="6d902-140">This is a fault handling strategy rather than a performance enhancing strategy.</span></span>

<span data-ttu-id="6d902-141">Чтобы регулярное обновление данных в кэше, когда приложение создает обновлений, или удаляет данные связанных записей можно будет удалить.</span><span class="sxs-lookup"><span data-stu-id="6d902-141">In order to keep data in the cache current, you can delete related cache entries when your application creates, updates, or deletes data.</span></span> <span data-ttu-id="6d902-142">Если это нормально для вашего приложения для получения данных, немного устаревшая иногда можно положиться на можно настроить срок для установки ограничения на как старый кэш данных может быть.</span><span class="sxs-lookup"><span data-stu-id="6d902-142">If it's alright for your application to sometimes get data that is slightly out-of-date, you can rely on a configurable expiration time to set a limit on how old cache data can be.</span></span>

<span data-ttu-id="6d902-143">Можно настроить абсолютный срок действия (количество времени с момента создания элемента кэша) или скользящий срок действия (количество времени со времени последнего обращения к элемента кэша).</span><span class="sxs-lookup"><span data-stu-id="6d902-143">You can configure absolute expiration (amount of time since the cache item was created) or sliding expiration (amount of time since the last time a cache item was accessed).</span></span> <span data-ttu-id="6d902-144">Абсолютный срок действия используется в том случае, если находятся в зависимости от механизма истечение срока действия кэша для предотвращения возникновения слишком устаревшие данные.</span><span class="sxs-lookup"><span data-stu-id="6d902-144">Absolute expiration is used when you are depending on the cache expiration mechanism to prevent the data from becoming too stale.</span></span> <span data-ttu-id="6d902-145">В ее исправить приложение мы будем вручную удалить старый кэш элементы, и мы будем использовать скользящий срок действия для хранения текущих данных в кэше.</span><span class="sxs-lookup"><span data-stu-id="6d902-145">In the Fix It app, we'll manually evict stale cache items and we'll use sliding expiration to keep the most current data in cache.</span></span> <span data-ttu-id="6d902-146">Независимо от выбранного это политика срока кэша автоматически исключите самые старые элементы (как минимум недавно использованные или LRU), при достижении предела памяти кэша.</span><span class="sxs-lookup"><span data-stu-id="6d902-146">Regardless of the expiration policy you choose, the cache will automatically evict the oldest (Least Recently Used or LRU) items when the cache's memory limit is reached.</span></span>

## <a name="sample-cache-aside-code-for-fix-it-app"></a><span data-ttu-id="6d902-147">Пример кода отдельно от кэша для приложения ее исправить</span><span class="sxs-lookup"><span data-stu-id="6d902-147">Sample cache-aside code for Fix It app</span></span>

<span data-ttu-id="6d902-148">В следующем примере кода мы проверяем кэша сначала при извлечении задачи ее исправить.</span><span class="sxs-lookup"><span data-stu-id="6d902-148">In the following sample code, we check the cache first when retrieving a Fix It task.</span></span> <span data-ttu-id="6d902-149">Если задача находится в кэше, мы возвращаем Если объект не найден, мы получить его из базы данных и их сохранения в кэше.</span><span class="sxs-lookup"><span data-stu-id="6d902-149">If the task is found in cache, we return it; if not found, we get it from the database and store it in the cache.</span></span> <span data-ttu-id="6d902-150">Изменения сделают Добавление кэширования `FindTaskByIdAsync` метод выделяются.</span><span class="sxs-lookup"><span data-stu-id="6d902-150">The changes you'd make to add caching to the `FindTaskByIdAsync` method are highlighted.</span></span>

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

<span data-ttu-id="6d902-151">При обновлении или удалении задачи ее исправить необходимо сделать недействительным задача кэшированные (удаления).</span><span class="sxs-lookup"><span data-stu-id="6d902-151">When you update or delete a Fix It task, you have to invalidate (remove) the cached task.</span></span> <span data-ttu-id="6d902-152">В противном случае будущее пытается считать, что задача будет продолжать получать старые данные из кэша.</span><span class="sxs-lookup"><span data-stu-id="6d902-152">Otherwise, future attempts to read that task will continue to get the old data from the cache.</span></span>

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

<span data-ttu-id="6d902-153">Ниже приведены образцы, демонстрирующие простого кэширования кода; кэширование не был реализован в загружаемый проект ее исправить.</span><span class="sxs-lookup"><span data-stu-id="6d902-153">These are samples to illustrate simple caching code; caching has not been implemented in the downloadable Fix It project.</span></span>

## <a name="azure-caching-services"></a><span data-ttu-id="6d902-154">Службы кэширования Azure</span><span class="sxs-lookup"><span data-stu-id="6d902-154">Azure caching services</span></span>

<span data-ttu-id="6d902-155">Azure предлагает следующие службы кэширования: [кэш Azure Redis](https://msdn.microsoft.com/en-us/library/dn690523.aspx) и [управляемого кэша Azure](https://msdn.microsoft.com/en-us/library/dn386094.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-155">Azure offers the following caching services: [Azure Redis Cache](https://msdn.microsoft.com/en-us/library/dn690523.aspx) and [Azure Managed Cache](https://msdn.microsoft.com/en-us/library/dn386094.aspx).</span></span> <span data-ttu-id="6d902-156">Кэш Azure Redis, основано на популярном [с открытым кодом кэша Redis](http://redis.io/) и кэширование является предпочтительным вариантом для большинства сценариев.</span><span class="sxs-lookup"><span data-stu-id="6d902-156">Azure Redis cache is based on the popular [open source Redis Cache](http://redis.io/) and is the first choice for most caching scenarios.</span></span>

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a><span data-ttu-id="6d902-157">Состояние сеанса ASP.NET с помощью поставщика кэша</span><span class="sxs-lookup"><span data-stu-id="6d902-157">ASP.NET session state using a cache provider</span></span>

<span data-ttu-id="6d902-158">Как упоминалось в [web разработки лучшие рекомендации главе](web-development-best-practices.md), рекомендуется избегать использования состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="6d902-158">As mentioned in the [web development best practices chapter](web-development-best-practices.md), a best practice is to avoid using session state.</span></span> <span data-ttu-id="6d902-159">Если приложению требуется состояние сеанса, Далее рекомендуется избежать поставщика в памяти по умолчанию, поскольку не обеспечивают масштабирование (несколько экземпляров веб-сервера).</span><span class="sxs-lookup"><span data-stu-id="6d902-159">If your application requires session state, the next best practice is to avoid the default in-memory provider because that doesn't enable scale out (multiple instances of the web server).</span></span> <span data-ttu-id="6d902-160">Поставщик состояния сеанса ASP.NET SQL Server включает сайту, который выполняется на нескольких веб-серверов для использования состояния сеанса, но он приводит к затратам высокой задержкой, по сравнению с поставщиком данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="6d902-160">The ASP.NET SQL Server session state provider enables a site that runs on multiple web servers to use session state, but it incurs a high latency cost compared to an in-memory provider.</span></span> <span data-ttu-id="6d902-161">Если необходимо использовать состояние сеанса лучше использовать поставщик кэша, такие как [поставщика состояний сеансов для кэша Azure](https://msdn.microsoft.com/en-us/library/windowsazure/gg185668.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-161">The best solution if you have to use session state is to use a cache provider, such as the [Session State Provider for Azure Cache](https://msdn.microsoft.com/en-us/library/windowsazure/gg185668.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="6d902-162">Сводка</span><span class="sxs-lookup"><span data-stu-id="6d902-162">Summary</span></span>

<span data-ttu-id="6d902-163">Было показано, как приложение исправить может реализовать кэширование позволяет улучшить время отклика и масштабируемость и предоставить приложению возможность продолжать отреагирует для операций чтения, когда база данных недоступна.</span><span class="sxs-lookup"><span data-stu-id="6d902-163">You've seen how the Fix It app could implement caching in order to improve response time and scalability, and to enable the app to continue to be responsive for read operations when the database is unavailable.</span></span> <span data-ttu-id="6d902-164">В [следующей главе](queue-centric-work-pattern.md) мы покажем, как для дальнейшего повышения масштабируемости и сделать приложение продолжает реагировать для операций записи.</span><span class="sxs-lookup"><span data-stu-id="6d902-164">In the [next chapter](queue-centric-work-pattern.md) we'll show how to further improve scalability and make the app continue to be responsive for write operations.</span></span>

## <a name="resources"></a><span data-ttu-id="6d902-165">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="6d902-165">Resources</span></span>

<span data-ttu-id="6d902-166">Дополнительные сведения о кэшировании см. следующие ресурсы.</span><span class="sxs-lookup"><span data-stu-id="6d902-166">For more information about caching, see the following resources.</span></span>

<span data-ttu-id="6d902-167">Документация</span><span class="sxs-lookup"><span data-stu-id="6d902-167">Documentation</span></span>

- <span data-ttu-id="6d902-168">[Кэш Azure](https://msdn.microsoft.com/en-us/library/gg278356.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-168">[Azure Cache](https://msdn.microsoft.com/en-us/library/gg278356.aspx).</span></span> <span data-ttu-id="6d902-169">Официальной документации MSDN о кэшировании в Azure.</span><span class="sxs-lookup"><span data-stu-id="6d902-169">Official MSDN documentation on caching in Azure.</span></span>
- <span data-ttu-id="6d902-170">[Шаблоны и методики - руководство по Azure Майкрософт](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-170">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span></span> <span data-ttu-id="6d902-171">См. руководство кэширование и шаблон отдельно от кэша.</span><span class="sxs-lookup"><span data-stu-id="6d902-171">See Caching guidance and Cache-Aside pattern.</span></span>
- <span data-ttu-id="6d902-172">[Отказоустойчивость: Руководство по гибкой облачной архитектуре](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-172">[Failsafe: Guidance for Resilient Cloud Architectures](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx).</span></span> <span data-ttu-id="6d902-173">Технический документ по Marc Mercuri, Ульрих Хоманн и Эндрю Таунхилл.</span><span class="sxs-lookup"><span data-stu-id="6d902-173">White paper by Marc Mercuri, Ulrich Homann, and Andrew Townhill.</span></span> <span data-ttu-id="6d902-174">В разделе на кэширование.</span><span class="sxs-lookup"><span data-stu-id="6d902-174">See the section on Caching.</span></span>
- <span data-ttu-id="6d902-175">[Советы и рекомендации по проектированию крупномасштабных служб в облачных службах Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-175">[Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx).</span></span> <span data-ttu-id="6d902-176">ВТ.</span><span class="sxs-lookup"><span data-stu-id="6d902-176">W.</span></span> <span data-ttu-id="6d902-177">Технический документ, Марк Симмс и Майкл Томасси.</span><span class="sxs-lookup"><span data-stu-id="6d902-177">White paper by Mark Simms and Michael Thomassy.</span></span> <span data-ttu-id="6d902-178">В разделе на распределенное кэширование.</span><span class="sxs-lookup"><span data-stu-id="6d902-178">See the section on distributed caching.</span></span>
- <span data-ttu-id="6d902-179">[Распределенное кэширование на путь к масштабируемости](https://msdn.microsoft.com/en-us/magazine/dd942840.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-179">[Distributed Caching On The Path To Scalability](https://msdn.microsoft.com/en-us/magazine/dd942840.aspx).</span></span> <span data-ttu-id="6d902-180">Более старые статьи MSDN Magazine (2009 г.), но четко написанное введение в распределенное кэширование в целом; переходит в более подробно, чем кэширования разделы в безопасном и рекомендации для технических документах.</span><span class="sxs-lookup"><span data-stu-id="6d902-180">An older (2009) MSDN Magazine article, but a clearly written introduction to distributed caching in general; goes into more depth than the caching sections of the FailSafe and Best Practices white papers.</span></span>

<span data-ttu-id="6d902-181">Видеоролики</span><span class="sxs-lookup"><span data-stu-id="6d902-181">Videos</span></span>

- <span data-ttu-id="6d902-182">[Отказоустойчивость: Построение масштабируемых, надежных облачные службы](https://channel9.msdn.com/Series/FailSafe).</span><span class="sxs-lookup"><span data-stu-id="6d902-182">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="6d902-183">Девять выпусков, Ульрих Хоманн, Marc Mercuri и Марк Симмс.</span><span class="sxs-lookup"><span data-stu-id="6d902-183">Nine-part series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="6d902-184">Отображает представление уровня 400 процесс проектирования облачных приложений.</span><span class="sxs-lookup"><span data-stu-id="6d902-184">Presents a 400-level view of how to architect cloud apps.</span></span> <span data-ttu-id="6d902-185">Эта серия основное внимание уделяется теории и причин, почему; более подробные инструкции по см. в больших построение серии, Марк Симмс.</span><span class="sxs-lookup"><span data-stu-id="6d902-185">This series focuses on theory and reasons why; for more how-to details, see the Building Big series by Mark Simms.</span></span> <span data-ttu-id="6d902-186">В описании кэширования в серии 3, начиная с 1:24:14.</span><span class="sxs-lookup"><span data-stu-id="6d902-186">See the caching discussion in episode 3 starting at 1:24:14.</span></span>
- <span data-ttu-id="6d902-187">[Построение Big: Уроках, извлеченных из клиентам Azure — часть I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Дэвис рассматриваются распределенного кэширования начиная с 46:00.</span><span class="sxs-lookup"><span data-stu-id="6d902-187">[Building Big: Lessons learned from Azure customers - Part I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discusses distributed caching starting at 46:00.</span></span> <span data-ttu-id="6d902-188">Аналогично Безаварийность рядов, но переход в Дополнительные практические сведения.</span><span class="sxs-lookup"><span data-stu-id="6d902-188">Similar to the Failsafe series but goes into more how-to details.</span></span> <span data-ttu-id="6d902-189">Представление было передано 31 октября 2012 г., он не распространяется на службы кэширования веб-приложений в службе приложений Azure, которая была введена в 2013.</span><span class="sxs-lookup"><span data-stu-id="6d902-189">The presentation was given October 31, 2012, so it does not cover caching service of Web Apps in Azure App Service that was introduced in 2013.</span></span>

<span data-ttu-id="6d902-190">Образец кода</span><span class="sxs-lookup"><span data-stu-id="6d902-190">Code sample</span></span>

- <span data-ttu-id="6d902-191">[Основы облачных служб в Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="6d902-191">[Cloud Service Fundamentals in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="6d902-192">Образец приложения, который реализует распределенное кэширование.</span><span class="sxs-lookup"><span data-stu-id="6d902-192">Sample application that implements distributed caching.</span></span> <span data-ttu-id="6d902-193">См. сопутствующие блога [Основы облачных служб — основы кэширование](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d902-193">See the accompanying blog post [Cloud Service Fundamentals – Caching Basics](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6d902-194">[Назад](transient-fault-handling.md)
[Вперед](queue-centric-work-pattern.md)</span><span class="sxs-lookup"><span data-stu-id="6d902-194">[Previous](transient-fault-handling.md)
[Next](queue-centric-work-pattern.md)</span></span>