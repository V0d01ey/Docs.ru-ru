---
uid: whitepapers/mvc4-release-notes
title: "ASP.NET MVC 4 | Документы Microsoft"
author: rick-anderson
description: "В этом документе описывается выпуск ASP.NET MVC 4."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: bea6f6112388290a2c6b5ed267626ba28fc36671
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> В этом документе описывается выпуск ASP.NET MVC 4.


- [Замечания по установке](#_Toc303253802)
- [Документация](#_Toc303253803)
- [Поддержка](#_Toc303253804)
- [Требования к программному обеспечению](#_Toc303253805)
- [Новые возможности в ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Улучшения в шаблоны проектов по умолчанию](#_Toc303253808)
    - [Шаблон проекта для мобильных устройств](#_Toc303253809)
    - [Режимы отображения](#_Toc303253810)
    - [jQuery Mobile, переключателя и переопределения браузера](#_Toc303253811)
    - [Поддержка задач для асинхронных контроллеров](#_Toc303253813)
    - [Пакет Azure SDK](#_Toc303253814)
    - [Миграция базы данных](#_Toc303253818)
    - [Пустой шаблон проекта](#_Toc303253819)
    - [Добавить контроллер в любой папке проекта](#_Toc303253820)
    - [Объединение и минификация](#_Toc303253821)
    - [Включить имена входа из Facebook, а также других узлов, с помощью OAuth и OpenID](#_Toc303253822)
- [Обновление проекта ASP.NET MVC 3 до ASP.NET MVC 4](#_Toc303253806)
- [Изменения с ASP.NET MVC 4, версия-кандидат](#_Toc303253817)
- [Известные проблемы и критические изменения](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Замечания по установке

ASP.NET MVC 4 для Visual Studio 2010 можно установить из [Домашняя страница ASP.NET MVC 4](../mvc/mvc4.md) с помощью установщика веб-платформы.

Рекомендуется удалить все ранее установленные предварительные версии ASP.NET MVC 4 до установки ASP.NET MVC 4. ASP.NET MVC 4, бета-версии и версии-кандидата можно обновить до ASP.NET MVC 4 без удаления.

Этот выпуск не совместим с любой предварительных выпусков платформы .NET Framework 4.5. Необходимо отдельно обновить все установленные предварительных выпусков платформы .NET Framework 4.5 до окончательной версии перед установкой ASP.NET MVC 4.

ASP.NET MVC 4 могут быть установлены и запущены side-by-side с ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Документация

Документация по ASP.NET MVC доступна на веб-сайте MSDN по АДРЕСУ:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Учебники и другие сведения о ASP.NET MVC доступны на странице веб-сайта ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Поддержка

ASP.NET MVC 4, полностью поддерживается. При наличии вопросов о работе в этом выпуске можно также разместить их на форуме ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), где члены сообщества ASP.NET друг другу, предоставляя полезные сведения.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

Компоненты ASP.NET MVC 4 для Visual Studio требуется PowerShell 2.0, а Visual Studio 2010 с пакетом обновления 1 или Visual Web Developer Express 2010 с пакетом обновления 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Новые возможности в ASP.NET MVC 4

В этом разделе описываются функции, которые были введены в версии ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 содержит веб-API ASP.NET, новую платформу для создания служб HTTP может попасть широкого диапазона клиентов, включая браузеры и мобильные устройства. Веб-API ASP.NET также является идеальной платформой для создания служб RESTful.

Веб-API ASP.NET включает поддержку следующих возможностей:

- **Современные модели программирования HTTP:** непосредственного доступа и управления HTTP-запросы и ответы в вашей веб-API, с помощью нового, строго типизированной объектной модели HTTP. Же программирования модели и HTTP-конвейера симметрично доступна на клиентском компьютере с помощью нового *HttpClient* типа.
- **Полная поддержка маршруты:** веб-API ASP.NET поддерживает полный набор возможностей маршрутизация ASP.NET, включая параметры маршрута и ограничения маршрута. Кроме того можно используйте простой соглашения для сопоставления действия с HTTP-методов.
- **Согласование содержимого:** клиентом и сервером можно совместно определяют неправильный формат для данных, возвращаемых из веб-API. Веб-API ASP.NET по умолчанию обеспечивает поддержку для XML, JSON и форматы форма URL-адреса и можно расширить эту функцию, добавив собственные модули форматирования, или даже заменить стратегии согласования содержимого по умолчанию.
- **Модель привязки и проверки:** связыватели моделей обеспечивают простой способ извлечения данных из различных частей HTTP-запроса и преобразования этих частей сообщения в объекты .NET, которые можно использовать при выполнении действий веб-API. Параметры действия, основанные на данных заметок также выполняется проверка.
- **Фильтры:** веб-API ASP.NET поддерживает фильтры, включая хорошо известных фильтры, такие как *[Authorize]* атрибута. Можно создать и подключить собственные фильтры для действия, авторизации и обработка исключений.
- **Построении запроса:** используйте *[Queryable]* атрибут фильтра на основе действия, возвращающего *IQueryable* для включения поддержки для выполнения запросов к веб-API через о запросов OData.
- **Улучшенные возможности тестирования:** вместо задания HTTP сведения в объектах статического контекста, веб-API рабочих действий с экземплярами *HttpRequestMessage* и *HttpResponseMessage*. Создание проекта модульного теста вместе с проекта веб-API, чтобы приступить к написанию модульных тестов для ваших функциональных возможностей веб-API быстро.
- **Конфигурация на основе кода:** конфигурации веб-API ASP.NET осуществляется только через код, если оставить файл конфигурации очистить файлы. Используйте шаблон локатора службы для настройки точки расширения.
- **Улучшенная поддержка инверсии управления (IoC) контейнеры:** веб-API ASP.NET предоставляет отличную поддержку для контейнеров IoC через абстракцию Сопоставитель улучшенные зависимостей
- **Резидентной:** веб-API может размещаться в процессе помимо служб IIS и продолжать использовать всю мощь маршруты и другие компоненты веб-API.
- **Создавать специальную справку страницы и тестировать:** теперь можно легко специальную справку построения и тестирования страниц для вашего веб-API с помощью нового *IApiExplorer* службы, чтобы получить описание полного времени выполнения веб-API-интерфейсы.
- **Мониторинг и диагностика:** веб-API ASP.NET теперь предоставляет легкий инфраструктура трассировки, позволяющие легко интегрировать с существующие решения ведения журнала, например System.Diagnostics, трассировки событий Windows и сторонних платформ ведения журналов. Можно включить трассировку, предоставляя *ITraceWriter* реализация и добавление его в конфигурацию веб-API.
- **Связать поколения:** используйте ASP.NET Web API *UrlHelper* для создания ссылки на связанные ресурсы в одном приложении.
- **Шаблон проекта веб-API:** выберите новую форму проекта веб-API мастер создания проекта MVC 4, чтобы быстро приступить к работе с веб-API ASP.NET.
- **Формирование шаблонов:** используйте **добавить контроллер** диалоговое окно, чтобы быстро формировании шаблона контроллера веб-API на основе Entity Framework на основе типа модели.

Для получения дополнительных сведений о веб-API ASP.NET обращайтесь [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Улучшения в шаблоны проектов по умолчанию

Шаблон, который используется для создания новых проектов ASP.NET MVC 4 был обновлен для создания более современные поиска веб-сайта:

![](mvc4-release-notes/_static/image1.png)

В дополнение к формальной усовершенствования Улучшена функциональность в новом шаблоне. Шаблон использует метод, называемый адаптивной отрисовки для браузеров настольных компьютеров и мобильных браузеров стандартной плохо.

![](mvc4-release-notes/_static/image2.png)

Для просмотра адаптивной отрисовки в действии, можно использовать мобильном эмуляторе или просто измените размеры окна браузера настольного компьютера меньше по размеру. Когда окно браузера получает достаточно мал, изменяется макет страницы.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Шаблон проекта для мобильных устройств

Если вы начинаете новый проект и создать сайт специально для мобильных устройств и браузеров планшета, можно использовать новый шаблон проекта приложения для мобильных устройств. Следующий пример основан на jQuery Mobile, библиотеку открытым исходным кодом, для создания пользовательского интерфейса, оптимизированных для сенсорных экранов:

![](mvc4-release-notes/_static/image3.png)

Этот шаблон содержит ту же структуру приложения, что шаблон веб-приложение (и код контроллера практически идентичен), но со стилем с помощью jQuery Mobile хорошо выглядят и работают на основе touch мобильных устройствах. Дополнительные сведения о структуре и мобильных пользовательского интерфейса в стиле см. в разделе [jQuery мобильном проекте веб-сайта](http://jquerymobile.com/).

При наличии сайта для рабочей среды, нужно добавить представления, оптимизированные для мобильных устройств, чтобы, или если нужно создать на одном сайте, служит другой по стилю представления для браузеров настольных компьютеров и мобильных устройств можно использовать новую функцию режимов отображения. (См. следующий раздел).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Режимы отображения

Новая функция режимов отображения для работы приложения выберите представления, в зависимости от браузера, который выполняет запрос. Например если браузер на компьютере запросы на домашней странице, в приложении можно использовать шаблон Views\Home\Index.cshtml. Если мобильный браузер запрашивает домашнюю страницу, приложение может возвратить Views\Home\Index.mobile.cshtml шаблона.

Макеты и частичными репликами также может быть переопределен для конкретной браузера типов. Пример:

- Если в одну папку содержит оба \_Layout.cshtml и \_Layout.mobile.cshtml шаблоны по умолчанию будет использоваться приложением \_Layout.mobile.cshtml во время запросов из браузеры для мобильных устройств и \_Layout.cshtml во время других запросов.
- Если папка содержит оба \_MyPartial.cshtml и \_MyPartial.mobile.cshtml, инструкция @Html.Partial(«\_MyPartial») будет отображаться \_MyPartial.mobile.cshtml во время запросы от мобильных устройств браузеры, и \_MyPartial.cshtml во время других запросов.

Если требуется создать более конкретным представлениям макетов и частичных представлений для других устройств, можно зарегистрировать новый *DefaultDisplayMode* экземпляра, чтобы указать, какое имя для поиска, если запрос удовлетворяет условиям определенного. Например, можно добавить следующий код в *приложения\_запустить* метод в файле Global.asax для регистрации строку «iPhone» как режим просмотра, который применяется, когда браузер Apple iPhone делает запрос:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

После выполнения этого кода, когда браузер Apple iPhone выполняет запрос, приложение будет использовать одну\\_Layout.iPhone.cshtml макета (если он существует). Дополнительные сведения о режиме отображения см. в разделе [функции мобильных устройств ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Следует установить приложения, использующие DisplayModeProvider [основных DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) пакет NuGet. [ASP.NET Осень 2012 обновление](https://go.microsoft.com/fwlink/?LinkID=271322) включает [основных DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) пакет NuGet в новые шаблоны проектов. В разделе [Fixedd ошибки кэширования в Mobile для ASP.NET MVC 4](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) подробные сведения для устранения этой проблемы.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile и возможности мобильных устройств

Сведения о разработке мобильных приложений с использованием ASP.NET MVC 4 с помощью jQuery Mobile, см. в учебнике [функции мобильных устройств ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Поддержка задач для асинхронных контроллеров

Теперь можно написать асинхронные методы действия как один методы, которые возвращают объект типа *задачи* или *задачи&lt;ActionResult&gt;*.

 Дополнительные сведения см. [использование асинхронных методов в ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Пакет Azure SDK

ASP.NET MVC 4 поддерживает 1.6 и более поздних версиях Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Миграция базы данных

Проекты ASP.NET MVC 4 теперь включают Entity Framework 5. Одна из замечательных функций в Entity Framework 5 — поддержка миграции базы данных. Эта функция позволяет легко развить схемы базы данных с помощью миграции, ориентированный на код, сохраняя данные в базе данных. Дополнительные сведения о миграции базы данных см. в разделе [Добавление нового поля в таблице и модели фильма](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) в [введение в ASP.NET MVC 4 учебника](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Пустой шаблон проекта

Так, чтобы можно было начать с полностью очищенном MVC пустой шаблон проекта не действительно пуст. Более ранней версии шаблона пустой проект был переименован в базовый.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Добавить контроллер в любой папке проекта

Теперь можно правой кнопкой мыши и выберите **добавить контроллер** из любой папки в проект MVC. Это дает большую гибкость для организации контроллеры угодно, включая сохранение контроллеров MVC и веб-API в отдельные папки.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Объединение и Минификация

Объединение и Минификация framework позволяет уменьшить количество HTTP-запросов, которые необходимо сделать, если объединение отдельных файлов в один объединенный файл для скриптов и CSS веб-страницы. Затем его можно уменьшить общий размер запросов, Минификация содержимое пакета. Минификация может включать действий, подобных устраняя пробел, чтобы сократить имена переменных даже свертывающейся селекторов CSS, в зависимости от их семантика. Пакеты объявляются и в код и являются легко ссылаться на них в представлениях посредством вспомогательные методы, которые можно создать либо отдельную ссылку в набор, или, при отладке нескольких ссылки на отдельные содержимое пакета. Дополнительные сведения см. [объединении и Минификация](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Включить имена входа из Facebook, а также других узлов, с помощью OAuth и OpenID

Шаблоны по умолчанию в шаблон проекта ASP.NET MVC 4 Интернет теперь включает поддержку OAuth и OpenID имени входа при использовании библиотека DotNetOpenAuth. Сведения о настройке поставщика OAuth или OpenID см. в разделе [поддержка протоколов OAuth или OpenID для веб-форм, MVC и веб-страниц](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) и [OAuth и OpenID компонентов документации веб-страницах ASP.NET](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Обновление проекта ASP.NET MVC 3 до ASP.NET MVC 4

ASP.NET MVC 4 можно установить параллельно с ASP.NET MVC 3 на том же компьютере, что обеспечивает гибкость в выборе при обновлении приложения ASP.NET MVC 3 до ASP.NET MVC 4.

Самый простой способ обновления является создание нового проекта ASP.NET MVC 4 и скопируйте все представления, контроллеры, кода и содержимое файлов из существующего проекта MVC 3 в новый проект и затем обновление ссылок на сборку в новом проекте для сопоставления любого шаблона не MVC в включенные assembiles, которую вы используете. При внесении изменений в файл Web.config в проект MVC 3 необходимо также осуществить слияние этих изменений в файл Web.config в проекте MVC 4.

Чтобы вручную обновить существующее приложение ASP.NET MVC 3 до версии 4, выполните следующие действия:

1. В файле Web.config все файлы в проекте (имеется в корневой папке проекта, в папке «представления», и в папке Views каждой области в проекте), замените каждый экземпляр следующий текст (Примечание: System.Web.WebPages, Version = 1.0.0.0 не найден в проекты, созданные с помощью Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    соответствующий текст:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. В корневом файле Web.config, обновите *webPages:Version* элемента на «2.0.0.0» и добавьте новую *PreserveLoginUrl* ключ, который имеет значение «true»: 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. В обозревателе решений щелкните правой кнопкой мыши в ссылках и выберите Управление пакетами NuGet. В левой области выберите **источник официального пакета Online\NuGet**, обновите следующее:

    - ASP.NET MVC 4
    - (Необязательно) jQuery, jQuery проверки и jQuery UI
    - (Необязательно) Платформа Entity Framework
    - (Optonal) Modernizr
4. В обозревателе решений щелкните правой кнопкой мыши имя проекта и выберите команду Выгрузить проект. Затем щелкните правой кнопкой мыши имя и выберите изменение, *ProjectName*CSPROJ-файл.
5. Найдите *ProjectTypeGuids* и замените {E53F8FEA-EAE0-44A6-8774-FFD645390401} с {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Сохранить изменения, закройте файл проекта (CSPROJ-файл), которую вы изменяете, щелкните правой кнопкой мыши проект и выберите команду перезагрузить проект.
7. Если проект ссылается на все библиотеки сторонних разработчиков, которые компилируются с помощью предыдущих версий ASP.NET MVC, откройте корневой файл Web.config и добавьте следующие три *bindingRedirect* элементы  *Конфигурация* раздела: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Изменения с ASP.NET MVC 4, версия-кандидат

Заметки о выпуске для ASP.NET MVC 4, версия-кандидат можно найти здесь:

Основные изменения из версии-кандидата ASP.NET MVC 4 в этом выпуске приведены ниже:

- **Каждой конфигурации контроллера:** контроллеров веб-API ASP.NET можно объяснить с пользовательским атрибутом, который реализует *IControllerConfiguration* для своих собственных модулей форматирования, выбора действий и связыватели параметра программы установки . *HttpControllerConfigurationAttribute* был удален.
- **На обработчики сообщений маршрута:** теперь можно указать обработчик сообщение последним в цепочке запроса для данного маршрута. Это обеспечивает поддержку платформы расстояния вдоль использует маршрутизацию для перенаправления с собственной (отличных*IHttpController*) конечные точки.
- **Уведомления о ходе выполнения:** *ProgressMessageHandler* приводит к возникновению ошибки уведомления о ходе выполнения для отправляемых объектов запроса и загружаемых объектов ответа. С помощью этого обработчика возможна для отслеживания насколько Отправка текст запроса или загрузка текст ответа.
- **Принудительно содержимое:** *PushStreamContent* класс позволяет использовать сценарии, в которых производителю данных требуется вести запись непосредственно в запрос или ответ (синхронное или асинхронное) с использованием потока. Когда *PushStreamContent* готов к приему данных, он вызывает делегат действия с выходной поток. Затем разработчик может записи в поток для завершения, пока необходимые и закрыть поток при записи. *PushStreamContent* обнаруживает закрытие потока и завершения базовой асинхронной *задачи* для вывода содержимого.
- **Создание сообщения об ошибках:** используйте *HttpError* тип для согласованного представления сведений об ошибках, такие как проверка ошибок и исключений по-прежнему соблюдением *IncludeErrorDetailPolicy* . Используйте новую *CreateErrorResponse* методы расширения для создания сообщения об ошибках с *HttpError* как содержимое. *HttpError* содержимое полностью содержимого согласование.
- **Удалить MediaRangeMapping:** диапазоны типов мультимедиа теперь обрабатываются согласователь содержимого по умолчанию.
- **Теперь является привязка параметров по умолчанию для простого типа параметров [FromUri]:** в предыдущих версиях ASP.NET Web API по умолчанию параметра привязки для привязки модели использовать параметры простого типа. Теперь является привязка параметров по умолчанию для простого типа параметров *[FromUri]*.
- **Выбор действия учитывает обязательные параметры:** Выбор действия в ASP.NET Web API будет теперь только выберите действие, если указаны все обязательные параметры, полученные из URI. Параметр можно указать как необязательные, указав значение по умолчанию для аргумента в сигнатуре метода действия.
- **Настройка привязки параметра HTTP:** использовать *ParameterBindingAttribute* для настройки привязки параметра для параметра определенное действие или использовать *ParameterBindingRules* на *HttpConfiguration* для настройки привязки параметров более широко.
- **Усовершенствования MediaTypeFormatter:** модули форматирования теперь имеют доступ к полной *HttpContent* экземпляра.
- **Буферизации Выбор политики узла:** реализовать и настроить *IHostBufferPolicySelector* службы в ASP.NET Web API для включения узлов определить политику для буферизации при для использования.
- **Доступ к сертификатам клиента в независимо от узла:** используйте *GetClientCertificate* метод расширения для получения предоставленного сертификата клиента из сообщения запроса.
- **Расширяемость согласования содержимого:** Настройка согласования содержимого путем наследования от *DefaultContentNegotiator* и переопределение любую часть согласования содержимого, который будет.
- **Поддержка для возвращения 406 неприемлемо ответов:** теперь может возвращать ответы 406 неприемлемо в веб-API ASP.NET, если не найден подходящий модуль форматирования, создав *DefaultContentNegotiator* с *excludeMatchOnTypeOnly* равным *true*.
- **Прочитать данные формы как NameValueCollection или JToken:** можно считывать данные формы, в строке запроса URI или в тексте запроса как *NameValueCollection* с помощью *ParseQueryString* и  *ReadAsFormDataAsync* методы расширения соответственно. Аналогичным образом можно считывать данные формы в строке запроса URI или в тексте запроса как *JToken* с помощью *TryReadQueryAsJson* и *ReadAsAsync*&lt;T&gt; методы расширения соответственно.
- **Составном усовершенствования:** теперь можно написать *MultipartStreamProvider* , полностью соответствующий тип MIME составных данных, может читать и представит результат оптимальным способом для пользователя. Можно также подключить шаг завершающей обработки на *MultipartStreamProvider* , позволяющий реализация, которую нужно сделать post, независимо от его обработки требуется для частей текста MIME. Например *MultipartFormDataStreamProvider* реализация считывает HTML-формы частей данных и добавляет их в *NameValueCollection* так, чтобы они легко получить в вызывающий объект.
- **Связать усовершенствования поколения:** *UrlHelper* больше не зависит от *HttpControllerContext*. Теперь вы можете просматривать *UrlHelper* в любом контексте, где *HttpRequestMessage* доступен.
- **Изменения порядка выполнения обработчика сообщений:** обработчики сообщений, теперь выполняются в порядке, в котором они настроены вместо в обратном порядке.
- **Вспомогательный класс для обработчиков сообщений подключение:** новый *HttpClientFactory* , можно подключить *DelegatingHandlers* и создать *HttpClient* с требуемый готова к работе конвейера. Он также предоставляет функциональные возможности для подключение с альтернативным внутренних обработчиков (значение по умолчанию — *HttpClientHandler*) также как и привязки при использовании *HttpMessageInvoker* или другой  *DelegatingHandler* вместо *HttpClient* как средство вызова top.
- **Поддержка CDN в ASP.NET веб-оптимизация:** ASP.NET веб-оптимизация теперь поддерживает CDN альтернативные пути, что позволяет задать для каждого объединить дополнительные URL-адрес, который указывает на этот же ресурс в сети доставки содержимого. Поддержка CDN позволяет получить ваш сценариев и стилей пакеты географически ближе к конечным пользователям веб-приложений.
- **Направляет веб-API ASP.NET и переместить конфигурации *WebApiConfig.Register* статический метод, который может быть resused в коде теста.** Веб-API ASP.NET маршруты ранее были добавлены в *RouteConfig.RegisterRoutes* вместе с MVC стандартных маршрутов. По умолчанию, направляет веб-API ASP.NET и конфигурации теперь обрабатываются в отдельном *WebApiConfig.Register* метод, чтобы упростить тестирование.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

- **Версия RC и RTM ASP.NET MVC 4 неправильно возвращается представлений в кэше рабочего стола, если мобильных представлений должны быть возвращены.**

    - В разделе [ASP.NET MVC 4 Mobile кэширование ошибки основных](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) подробные сведения для устранения этой проблемы. Исправления можно установить из [основных DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) пакет NuGet.
- **Критические изменения в представлений Razor**. Следующие типы были удалены из *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Также были удалены следующие методы: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Если WebMatrix.WebData.dll включен в каталоге/Bin приложения ASP.NET MVC 4, он берет на себя URL-адрес для проверки подлинности форм.** Добавление сборки WebMatrix.WebData.dll приложения (например, выбрав «ASP.NET Web Pages с синтаксисом Razor» при использовании диалогового окна Добавление развертываемых зависимостей) переопределяет перенаправления входа проверки подлинности для входа/account/вместо / учетной записи или имя входа, ожидаемого контроллера учетных записей ASP.NET MVC по умолчанию. Чтобы предотвратить такое поведение и использовать URL-адрес уже указан в разделе проверки подлинности файла Web.config, можно добавить appSetting называется PreserveLoginUrl и присвоено значение true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Диспетчер пакетов NuGet не устанавливается при установке ASP.NET MVC 4 для параллельная установка Visual Studio 2010 и Visual Web Developer 2010.** Для запуска Visual Studio 2010 и Visual Web Developer 2010 параллельно с ASP.NET MVC 4 ASP.NET MVC 4 необходимо установить обе версии Visual Studio уже после установки.
- **При удалении ASP.NET MVC 4 завершается ошибкой, если необходимые компоненты уже был удален.** Чтобы полностью удалить ASP.NET MVC 4you необходимо удалить перед удалением Visual Studio ASP.NET MVC 4.
- **Установка ASP.NET MVC 4 прерывается приложений ASP.NET MVC 3 RTM.** Выпуск приложений ASP.NET MVC 3, которые были созданы с RTM (не с [обновления средств ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) выпуска) рядом с ASP.NET MVC 4 работы требуются следующие изменения. Построение проекта без внесения результаты этих обновлений в ошибки компиляции. 

    **Необходимые обновления**

    1. В корневом файле Web.config, добавьте новый  *&lt;appSettings&gt;*  запись с ключом *webPages:Version* и значение *1.0.0.0*. 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. В обозревателе решений щелкните правой кнопкой мыши имя проекта и выберите команду Выгрузить проект. Затем щелкните правой кнопкой мыши имя и выберите изменение, *ProjectName*CSPROJ-файл.
    3. Найдите ссылки на следующие сборки: 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        Замените их следующим кодом:

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. Сохранить изменения, закройте файл проекта (CSPROJ-файл), изменения, а затем щелкните правой кнопкой мыши проект и выберите перезагрузить.
- **Изменение проекта ASP.NET MVC 4 целевой 4.0 из 4.5 не обновляет ссылку на сборку EntityFramework:** при переключении проекта ASP.NET MVC 4 целевой 4.0 после для различных версий 4.5 по-прежнему будет указывать ссылку на сборку EntityFramework версии 4.5. Для устранения этой проблемы удалите и переустановите пакет EntityFramework NuGet.
- **При запуске приложения ASP.NET MVC 4 в Azure после изменения целевой 4.0 из 4.5 403 Запрещено:** при изменении проекта ASP.NET MVC 4 целевой 4.0 после для различных версий 4.5 и затем развернуть в Azure, может появиться ошибка 403 запрещено во время выполнения. Для устранения этой проблемы добавьте следующий код в файл web.config:`<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 аварийно завершает работу при вводе "\' в строковый литерал в файле Razor.** Для временного решения этой проблемы закрывающая кавычка для строкового литерала сначала введите.
- **Просмотр &quot;учетной записи и управление&quot; в результатах шаблона Интернет ошибка среды выполнения для CHS, TRK и Китайский языки.** Для устранения проблемы измените страницу, чтобы отделить  *@User.Identity.Name*  , поместив его в качестве содержимого только в пределах  *&lt;строгого&gt;*  тег.
- **Google и LinkedIn поставщики не поддерживаются в пределах веб-сайтов Azure.** Используйте поставщики проверки подлинности альтернативный при развертывании на веб-сайтах Azure.
- **При использовании UriPathExtensionMapping 8 с IIS Express и IIS, при попытке использовать расширение получит ошибки 404 не найдено.** Обработчику файла статистики будет мешать запросы к веб-API, использовать *UriPathExtensionMappings*. Задать *runAllManagedModulesForAllRequests = true* в файле web.config, чтобы обойти эту проблему.
- **Метод Controller.Execute больше не вызывается.** Всех контроллеров MVC теперь всегда выполняются асинхронно.
