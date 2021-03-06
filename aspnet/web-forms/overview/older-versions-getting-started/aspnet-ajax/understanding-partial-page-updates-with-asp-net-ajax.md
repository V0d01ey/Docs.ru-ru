---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: "Основные сведения о частичной обновлений с помощью ASP.NET AJAX | Документы Microsoft"
author: scottcate
description: "Возможно, самой заметной функцией расширения ASP.NET AJAX является возможность выполнять частичной или добавочной страницы обновления, не выполняя полную обратную передачу t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 1d8d3009df0a264e466d3f7decfb65978d8ae7a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Основные сведения о частичной обновлений с помощью ASP.NET AJAX
====================
по [Scott кате](https://github.com/scottcate)

[Скачать PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Возможно, самой заметной функцией расширения ASP.NET AJAX является возможность выполнять обновления на странице частичной или добавочной без выполнения полной обратной передачи на сервер, без изменения кода и разметки минимальные изменения. Преимущества заключаются в расширенную — состояние вашего мультимедиа (например, Adobe Flash или Windows Media) не изменяется, снижаются затраты на полосу пропускания и клиент не возникает мерцание, обычно связанные с помощью обратной передачи.


## <a name="introduction"></a>Вступление

Технология ASP.NET корпорации Майкрософт предоставляет модель программирования объектно ориентированного и событиями и объединяет его с преимуществами скомпилированного кода. Однако его серверную обработку модели имеет несколько недостатков, присущих этой технологии.

- Страница обновления требуют приема-передачи на сервер, который требует обновления страницы.
- Циклов приема-передачи не сохраняются все эффекты, созданных в Javascript или другие клиентские технологии (например, Adobe Flash)
- Во время обратной передачи браузеров, отличных от Microsoft Internet Explorer не поддерживают автоматическое восстановление позиции прокрутки. И даже в Internet Explorer по-прежнему мерцание при обновлении страницы.
- Обратные передачи может привести к высокой объем пропускной способности \_ \_поля формы VIEWSTATE может увеличиваться, особенно при работе с элементами управления, например элемент управления GridView или знаки повторения.
- Не предусмотрена унифицированную модель доступа к веб-служб через JavaScript или другие клиентские технологии.

Введите расширения ASP.NET AJAX корпорации Майкрософт. AJAX, предназначенную для **A** синхронной **J** avaScript **A** nd **X** ML, — это интегрированная платформа для предоставления добавочных страницы обновления через JavaScript кросс платформенных, состоящий из кода на стороне сервера, включающего платформу Microsoft AJAX и компонент скрипта скрипт библиотеки Microsoft AJAX. Расширения ASP.NET AJAX также поддерживает кросс платформенных для доступа к веб-служб ASP.NET с помощью JavaScript.

В этом техническом документе рассматриваются функциональные возможности частичного обновления расширения ASP.NET AJAX, включающий компонент ScriptManager управления UpdatePanel и управления UpdateProgress, и рассматривает ситуации, в которых они должны или не должно быть загруженные.

В этом техническом документе основан на бета-версии 2 выпуска Visual Studio 2008 и .NET Framework 3.5, которая интегрирует расширения ASP.NET AJAX в библиотеке базовых классов (где он был ранее в качестве дополнения для ASP.NET 2.0). В этом техническом документе также предполагается, что вы используете Visual Studio 2008 и не Visual Web Developer Express Edition; Некоторые шаблоны проекта, на которые имеются ссылки не могут быть доступны для пользователей Visual Web Developer, экспресс-выпуск.

## <a name="partial-page-updates"></a>Частичного обновления страниц

Возможно, самой заметной функцией расширения ASP.NET AJAX является возможность выполнять обновления на странице частичной или добавочной без выполнения полной обратной передачи на сервер, без изменения кода и разметки минимальные изменения. Преимущества заключаются в расширенную — состояние вашего мультимедиа (например, Adobe Flash или Windows Media) не изменяется, снижаются затраты на полосу пропускания и клиент не возникает мерцание, обычно связанные с помощью обратной передачи.

Возможность интеграции частичной отрисовки интегрирована в ASP.NET с минимальными изменениями в проект.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Пошаговое руководство: Интеграция частичную отрисовку в существующий проект


1. В Microsoft Visual Studio 2008, создайте новый проект веб-сайт ASP.NET, выбрав пункты *файл*  *- &gt; New*  *- &gt; веб-сайт* из диалогового окна выбора веб-сайт ASP.NET. Ее можно назвать угодно и может работать в файловой системе или в Internet Information Services (IIS).
2. Откроется с пустым по умолчанию страницей с basic разметки ASP.NET (серверной формы и `@Page` директива). Перетащите метку с именем `Label1` и кнопки вызывается `Button1` на страницу в элемент form. Любые действия можно присвоить их свойства текста.
3. В конструкторе, дважды щелкните `Button1` для создания кода обработчика событий. В этот обработчик событий задать `Label1.Text` для была нажата кнопка! .

**Список 1: Разметки для default.aspx, прежде чем частичная отрисовка разрешена**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Список 2: Фонового кода (усечение) в default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Нажмите клавишу F5 для запуска веб-сайта. Visual Studio предложит пользователю добавить файл web.config для включения отладки; Сделайте это. При нажатии кнопки, обратите внимание, обновления страницы для изменения текста в метке, что имеется краткое мерцание при перерисовке страницы.
2. После закрытия окна браузера, возвращают для Visual Studio и разметка страницы. Прокрутите вниз на панели элементов Visual Studio и найдите вкладку расширения AJAX. (Если у вас на этой вкладке так, как вы используете старую версию расширения AJAX или Atlas, см. пошаговое руководство по регистрации элементов панели элементов расширения AJAX далее в этом техническом документе, или установить текущую версию с помощью загружаемых установщика Windows на веб-сайте).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Просмотр полноразмерное изображение](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. *Известные проблемы:*при установке Visual Studio 2008 на компьютере с Visual Studio 2005 устанавливается вместе с ASP.NET 2.0 AJAX Extensions уже Visual Studio 2008 будет импортировать элементы области элементов расширения AJAX. Можно определить ли это так, изучив подсказка компонентов; они говорят, версии 3.5.0.0. Если говорят версии 2.0.0.0 импортированных старых элементов панели элементов затем будет необходимо вручную импортировать их с помощью диалогового окна Выбор элементов панели элементов в Visual Studio. Нельзя добавлять элементы управления версии 2 через конструктор.

1. Прежде чем `<asp:Label>` тег начинается, создать строку из пробелов и дважды щелкните на панели элементов управления UpdatePanel. Обратите внимание, что новый `@Register` директива включается в верхней части страницы, указывающее, что элементы управления в пространстве имен System.Web.UI должна импортироваться с помощью `asp:` префикс.
2. Перетащите закрывающий `</asp:UpdatePanel>` тег после конца элемент Button так, чтобы элемент правильного формата с элементами управления Label и кнопку в оболочку.
3. После открывающего `<asp:UpdatePanel>` тег, приступать к открытию нового тега. Обратите внимание, что IntelliSense предлагает два варианта. В этом случае создайте `<ContentTemplate>` тег. Убедитесь, что этот тег вокруг метка и кнопка программы-оболочки, чтобы разметку имеет правильный формат.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Просмотр полноразмерное изображение](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. В любом месте в пределах `<form>` элемент, включать элемент управления ScriptManager, дважды щелкнув `ScriptManager` элемент на панели элементов.
2. Изменить `<asp:ScriptManager>` тег, чтобы он включал атрибут `EnablePartialRendering= true`.

**Список 3: Разметка default.aspx с включенной частичной отрисовкой**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Откройте файл web.config. Обратите внимание, что Visual Studio автоматически добавляет ссылку на System.Web.Extensions.dll компиляции.

1. Новые возможности Visual Studio 2008: web.config, который поставляется с шаблонами проектов веб-сайт ASP.NET, автоматически включает все ссылки, необходимые для расширения ASP.NET AJAX и включает комментарии области сведений о конфигурации, который может быть Отмена комментария дополнительных функциональных возможностей. Visual Studio 2005 была похожих шаблонов, когда были установлены расширения ASP.NET 2.0 AJAX. Однако в Visual Studio 2008 расширения AJAX, отказ от использования по умолчанию (то есть они имеются по умолчанию, но можно удалить ссылки на).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Просмотр полноразмерное изображение](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Нажмите клавишу F5 для запуска веб-сайта. Обратите внимание на то, как исходный код изменения не требуются для поддержки частичную отрисовку — только разметки было изменено.

При запуске веб-сайта, вы увидите, что частичную отрисовку — теперь включен, так как после нажатия на кнопку существует будет не мерцание и не будет ли какие-либо изменения позиции прокрутки страницы (этот пример не демонстрирует,). Если для поиска в источнике, готовый для просмотра страницы после нажатия кнопки, будет проверка на самом деле не произошло обратная передача — исходный текст метки по-прежнему является частью исходной разметки, и изменении метки с помощью JavaScript.

По-видимому, Visual Studio 2008 не поставляются с предварительно определенный шаблон для веб-сайте служб поддержкой ASP.NET AJAX. Однако такой шаблон был доступен в Visual Studio 2005, если были установлены Visual Studio 2005 и ASP.NET 2.0 AJAX Extensions. Следовательно настройки веб-сайта и запуска с помощью шаблона веб-сайта AJAX-Enabled вероятно, будет еще проще, в шаблоне следует включить в файл web.config полностью настроен (поддерживающая все расширения ASP.NET AJAX, включая доступ к веб-служб и сериализация JSON - JavaScript Object Notation) и включает UpdatePanel и ContentTemplate в главную страницу веб-форм по умолчанию. Включение частичной визуализации с этой страницей по умолчанию является просто возвращаясь шаг 10 этого пошагового руководства и удаление элементов управления на страницу.

## <a name="the-scriptmanager-control"></a>Элемент управления ScriptManager

## <a name="scriptmanager-control-reference"></a>Ссылка на элемент управления ScriptManager

Свойства с поддержкой разметку:

| **Имя свойства** | **Тип** | **Описание** |
| --- | --- | --- |
| Перенаправления AllowCustomErrors | Bool | Указывает, использовать ли пользовательский раздел ошибок файла web.config для обработки ошибок. |
| AsyncPostBackError сообщения | Строка | Возвращает или задает сообщение об ошибке отправляется клиенту, если возникает ошибка. |
| Время ожидания AsyncPostBack | Int32 | Возвращает или задает период времени, в которых клиент должен ожидать асинхронного запроса для завершения по умолчанию. |
| Глобализация EnableScript | Bool | Получает или задает скрипт глобализации, включена ли. |
| Локализация EnableScript | Bool | Возвращает или задает включение локализации сценариев. |
| ScriptLoadTimeout | Int32 | Определяет количество секунд, разрешенное для загрузки скриптов в клиенте |
| ScriptMode | Enum (Auto, отладки, выпуска, наследуют) | Возвращает или задает, следует ли отображать версии скриптов |
| ScriptPath | Строка | Возвращает или задает корневой путь к расположению файлов скриптов, отправляемый клиенту. |

Свойства, доступные только для кода:

| **Имя свойства** | **Тип** | **Описание** |
| --- | --- | --- |
| AuthenticationService | Диспетчер AuthenticationService | Возвращает сведения о прокси-службы проверки подлинности ASP.NET, который будет отправлен клиенту. |
| IsDebuggingEnabled | Bool | Получает ли сценарий и код отладки. |
| IsInAsyncPostback | Bool | Возвращает ли в данный момент запроса асинхронной обратной передачи страницы. |
| ProfileService | Диспетчер ProfileService | Возвращает сведения о прокси-службы профилирования ASP.NET, который будет отправлен клиенту. |
| Сценарии | Коллекция&lt;ссылку скрипта&gt; | Возвращает коллекцию ссылок на скрипты, которые будут отправлены клиенту. |
| Службы | Коллекция&lt;ссылки на службу&gt; | Возвращает коллекцию ссылок на прокси-сервера веб-службы, которые будут отправляться на клиенте. |
| SupportsPartialRendering | Bool | Получает значение, указывающее текущий клиент поддерживает частичную отрисовку. Если это свойство возвращает **false**, то все запросы страниц будет стандартных обратных передач. |

Методы открытого кода:

| **Имя метода** | **Тип** | **Описание** |
| --- | --- | --- |
| SetFocus(string) | Void | Устанавливает фокус клиента на определенный элемент управления при завершении запроса. |

Потомки разметку:

| **Тег** | **Описание** |
| --- | --- |
| &lt;AuthenticationService&gt; | Предоставляет сведения о прокси-сервера службы проверки подлинности ASP.NET. |
| &lt;ProfileService&gt; | Предоставляет сведения о прокси-сервер для ASP.NET, профилирование службы. |
| &lt;Сценарии&gt; | Предоставляет ссылки на дополнительные сценарии. |
| &lt;ASP: ScriptReference&gt; | Указывает ссылку на определенный сценарий. |
| &lt;Service&gt; | Предоставляет дополнительные ссылки веб-службы, имеющие созданные классы-посредники. |
| &lt;ASP: ServiceReference&gt; | Обозначает справочную веб-службы. |

Элемент управления ScriptManager такое ядро необходимые для расширения AJAX ASP.NET. Предоставляет доступ к библиотеке сценариев (включая системы типов широко клиентского скрипта), поддерживает частичную отрисовку и обеспечивает расширенную поддержку дополнительных служб ASP.NET (например, проверку подлинности и профилирование, а также другие веб-службы). Элемент управления ScriptManager также обеспечивает поддержку локализации и глобализации для клиентских скриптов.

## <a name="providing-alterative-and-supplemental-scripts"></a>Предоставляет альтернативный и дополнительные скрипты

Хотя расширения Microsoft ASP.NET 2.0 AJAX включить в отладочную код весь скрипт и освободить выпуски как ресурсы, внедренные в ссылочных сборках, разработчики могут перенаправить ScriptManager файлы пользовательского сценария, а также для регистрации Дополнительные необходимые скрипты.

Чтобы переопределить привязку по умолчанию для сценариев обычно включаемые (таких, которые поддерживают Sys.WebForms пространства имен и пользовательского ввода системы), вы можете зарегистрироваться в `ResolveScriptReference` класса ScriptManager. При вызове этого метода обработчика событий имеет возможность изменить путь к файлу сценария в вопросе; Диспетчер скриптов затем отправляет клиенту различных или настроенные копия сценариев.

Кроме того, ссылки на скрипты (представленного `ScriptReference` класса) можно включить программно или с помощью разметки. Чтобы сделать это, либо программно изменить `ScriptManager.Scripts` коллекции, или включать `<asp:ScriptReference>` теги под `<Scripts>` тег, который является дочерним для элемента управления ScriptManager первого уровня.

## <a name="custom-error-handling-for-updatepanels"></a>Пользовательскую обработку ошибок для элементы UpdatePanel

Несмотря на то, что обновления обрабатываются триггеры, заданные элементы управления UpdatePanel, поддержка обработки ошибок и пользовательские сообщения об ошибках обрабатывается экземпляр элемента управления ScriptManager страницы. Это делается путем предоставления события, `AsyncPostBackError`, страницу, которая может затем предоставления пользовательской логики обработки исключений.

Путем использования события AsyncPostBackError, можно указать `AsyncPostBackErrorMessage` свойство, которое вызывает окно предупреждения, формируемые по завершении обратного вызова.

Настройка на стороне клиента можно также вместо использования по умолчанию окно предупреждения; для экземпляра, может потребоваться отобразить настраиваемый `<div>` элемент, а не модальное диалоговое окно браузера по умолчанию. В этом случае можно обрабатывать ошибки в клиентском сценарии.

**Список 5: Клиентский сценарий для отображения пользовательских ошибок**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Все очень просто приведенного выше скрипта регистрирует обратный вызов для клиентского AJAX среды выполнения при завершении асинхронного запроса. Затем проверяет, является ли произошла ошибка и если да, обрабатывает сведения о ее, и, наконец, определяющее в среду выполнения, ошибки обработано в пользовательских сценариях.

## <a name="globalization-and-localization-support"></a>Поддержка локализации и глобализации

Элемент управления ScriptManager предоставляет широкие возможности для локализации строк скрипта и компоненты пользовательского интерфейса; Однако этот раздел находится за пределами этого технического документа. Дополнительные сведения см. в документе, поддержка глобализации в расширения ASP.NET AJAX.

## <a name="the-updatepanel-control"></a>Элемент управления UpdatePanel

## <a name="updatepanel-control-reference"></a>Ссылка на элемент управления UpdatePanel

Свойства с поддержкой разметку:

| **Имя свойства** | **Тип** | **Описание** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Указывает, является ли дочерние элементы управления автоматически вызывать обновление при обратной передаче. |
| Отображается как | enum (блок, встроенные) | Указывает способ содержимое отображается визуально. |
| UpdateMode | enum (всегда, условное) | Указывает ли UpdatePanel всегда обновляются во время частичной отрисовки, или если они только обновляются при каждом попадании триггера. |

Свойства, доступные только для кода:

| **Имя свойства** | **Тип** | **Описание** |
| --- | --- | --- |
| IsInPartialRendering | bool | Получает значение, указывающее UpdatePanel поддерживает частичную отрисовку для текущего запроса. |
| ContentTemplate | ITemplate | Возвращает шаблон разметки для запроса на обновление. |
| ContentTemplateContainer | Control | Получает программный шаблона для запроса на обновление. |
| Триггеры | UpdatePanel - TriggerCollection | Получает список триггеров, связанных с текущей UpdatePanel. |

Методы открытого кода:

| **Имя метода** | **Тип** | **Описание** |
| --- | --- | --- |
| Update() | Void | Обновляет указанный UpdatePanel программными средствами. Позволяет активировать частичной отрисовки UpdatePanel, в противном случае запланированное запроса сервера. |

Потомки разметку:

| **Тег** | **Описание** |
| --- | --- |
| &lt;ContentTemplate&gt; | Задает разметку, используемый для визуализации результатов частичной отрисовки. Дочерний &lt;asp: UpdatePanel&gt;. |
| &lt;Триггеры&gt; | Задает коллекцию  *n*  элементы управления, связанные с обновлением этой UpdatePanel. Дочерний &lt;asp: UpdatePanel&gt;. |
| &lt;ASP: AsyncPostBackTrigger&gt; | Указывает триггер, который вызывается для заданного UpdatePanel частичной отрисовки. Это может или не может быть элементом управления потомок рассматриваемой UpdatePanel. Детализированные и имя события. Дочерний &lt;триггеры&gt;. |
| &lt;ASP: PostBackTrigger&gt; | Указывает элемент управления, который запускает обновления всей страницы. Это может или не может быть элементом управления потомок рассматриваемой UpdatePanel. Детальные объекту. Дочерний &lt;триггеры&gt;. |

`UpdatePanel` Элемент управления, разделяющий серверные содержимое, которое будет принять участие в частичную отрисовку функциональные возможности расширения AJAX. Неограниченное число элементов управления UpdatePanel, которые могут быть на странице, и они могут быть вложенными. Каждый UpdatePanel изолирована, чтобы каждый может работать независимо (могут иметь два элементы UpdatePanel выполняются одновременно, подготовки к просмотру различных частей страницы, независимо от обратной передачи страницы).

В основном сделок с триггерами управления — по умолчанию любой элемент управления внутри элемента управления UpdatePanel управления UpdatePanel `ContentTemplate` , создающего обратный зарегистрирован как триггер для UpdatePanel. Это означает, что UpdatePanel может работать с привязкой к данным элементы управления по умолчанию (такие как GridView), с пользовательскими элементами управления, что их можно запрограммировать в скрипте.

По умолчанию при запуске частичной отрисовки всех элементов управления UpdatePanel на странице будут обновляться, ли элементы управления UpdatePanel определенные триггеры для такого действия. Например если один элемент UpdatePanel определяет элемент управления Button и нажатии этой кнопки элемента управления, все элементы управления UpdatePanel на этой странице будут обновляться по умолчанию. Это происходит, поскольку по умолчанию `UpdateMode` панели UpdatePanel свойству `Always`. Кроме того, можно присвоить свойство UpdateMode `Conditional`, означающее, что UpdatePanel обновляются только при достижении определенного триггера.

## <a name="custom-control-notes"></a>Заметки о пользовательского элемента управления

UpdatePanel можно добавить любой пользовательский элемент управления или пользовательский элемент управления; Однако страницы, на котором включены эти элементы управления необходимо также включить элемент управления ScriptManager с свойство EnablePartialRendering **true**.

Одним из способов в котором вы может учетную запись для этого при использовании пользовательских веб-элементов управления, Переопределите защищенный `CreateChildControls()` метод `CompositeControl` класса. Таким образом, можно ввести UpdatePanel между дочернего элемента управления и внешним миром, если вы решили, что страница поддерживает частичную отрисовку; в противном случае можно просто уровня дочерних элементов управления в контейнер `Control` экземпляра.

## <a name="updatepanel-considerations"></a>Вопросы UpdatePanel

UpdatePanel означает то же время черного ящика упаковки обратных передач ASP.NET в контексте JavaScript XMLHttpRequest. Однако есть вопросы производительности необходимо понять, как с точки зрения поведения и скорости. Чтобы понять, как работает UpdatePanel, таким образом, чтобы лучше всего можно решить, когда будет его использования, следует изучить AJAX exchange. В следующем примере существующий сайт и, Mozilla Firefox с расширением Firebug (Firebug собираются данные о XMLHttpRequest).

Рассмотрим форму, которая, помимо прочего, имеется текстовое поле Почтовый индекс, который должен заполнить поля Город и область формы или элемента управления. В конечном счете Эта форма собирает сведения о членстве, включая имя пользователя, адрес и контактные данные. Существует много вопросы проектирования необходимо принимать во внимание, в зависимости от требований конкретного проекта.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Просмотр полноразмерное изображение](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Просмотр полноразмерное изображение](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


В исходной итерации этого приложения, который был создан элемент управления включена для полноты данные регистрации пользователя, включая почтовый индекс, Город и область. Весь элемент управления был це UpdatePanel и перетащить в веб-форму. Если почтовый индекс, введенные пользователем, UpdatePanel обнаруживает событие (соответствующее TextChanged событие в серверной части, можно указать триггеры, либо с помощью ChildrenAsTriggers свойству задано значение true). AJAX учитывает все поля в UpdatePanel, как охваченные FireBug (увидеть как расширяется схема справа).

Как показывает снимок экрана, предоставляются значения из каждого элемента управления в UpdatePanel (в этом случае они пусты), а также поля ViewState. Все told более 9 КБ данных отправляется, когда на самом деле только пять байт данных были необходимы для выполнения данного конкретного запроса. Ответ еще более перегруженными: всего 57 КБ отправляется клиенту, просто обновить текстовое поле и раскрывающийся список поля.

Он также могут представлять интерес, чтобы увидеть, как ASP.NET AJAX обновляет презентации. Часть ответа запрос на обновление UpdatePanel отображается в окне консоли Firebug слева; представляет собой специально создан с разделителями канала строку, нарушается клиентский сценарий и затем повторно собираются на странице. В частности, ASP.NET AJAX задает *innerHTML* свойство элемента HTML на клиенте, который представляет ваш UpdatePanel. Как браузер повторно формирует модель DOM, имеется небольшая задержка, в зависимости от объема сведений, которые необходимо обработать.

Повторное создание модели DOM триггеров ряд дополнительных вопросов:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Просмотр полноразмерное изображение](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Если HTML-элемента, получившего фокус находится внутри UpdatePanel, оно также теряет фокус. Таким образом для пользователей, при нажатии клавиши Tab, чтобы выйти из текстового поля индекс, их следующей точки назначения были бы текстового поля Город. Однако после обновления UpdatePanel отображение формы больше не позволило бы фокус и при нажатии клавиши Tab должен был запустить выделение элементов фокус (например, ссылки).
- Если любой тип пользовательского сценария на стороне клиента используется, доступ к DOM элементы, ссылки на сохранены функциями может стать нефункционирующими после частичного обратной передачи.

Элементы UpdatePanel не предназначены для решения универсальным. Вместо этого они обеспечивают быстрое решение для определенных ситуациях, включая создание прототипов обновлений небольшой элемент управления и предоставляют знакомый интерфейс для разработчиков ASP.NET, которые могут быть знакомы с моделью объектов .NET, но менее, с помощью модели DOM. Существует ряд альтернативных решений, которые могут привести к повышению производительности в зависимости от сценария приложения:

- Рассмотрите возможность использования PageMethods и JSON (JavaScript Object Notation) позволяет разработчику вызывать статические методы страницы, как если бы вызванного вызов веб-службы. Поскольку методы являются статическими, ни одно состояние не требуется; вызывающий скрипт предоставляет параметры, а результат возвращается в асинхронном режиме.
- Рассмотрите возможность использования веб-службы и JSON, если один элемент управления должен использоваться в нескольких местах внутри приложения. Это снова не требует больших усилий специальные и работает асинхронно.

Включение функции через веб-службы или методы страницы имеет также недостатки. Первое и самое главное ASP.NET разработчиков обычно стремятся создания небольших компонентов функциональные возможности в пользовательские элементы управления (ASCX). Методы страницы не может размещаться в этих файлах; они должны быть включены в класс страницы фактическое aspx. Аналогично, веб-службы, должна быть размещена внутри класса .asmx. В зависимости от приложения эта архитектура может нарушать персональной ответственности в том, что функциональные возможности для одного компонента теперь распределены два или более физических компонентов, которые, возможно, почти или совсем не целостного WITH ties.

Наконец Если приложение требует, что используются элементы UpdatePanel, приведенным ниже рекомендациям следует помогает устранять неполадки и обслуживания.

- **Вложенные элементы UpdatePanel как можно реже, не только в пределах единицы, а также между блоки кода.** Например наличие UpdatePanel на странице, которая создает оболочку для элемента управления, пока этот элемент управления также содержит UpdatePanel, который содержит другой элемент управления, содержащий UpdatePanel, является вложения между единицами. Это помогает защищать очистить какие элементы должны обновляться и предотвращает непредвиденное обновляет дочерние элементы UpdatePanel.
- **Сохранить *ChildrenAsTriggers* свойств значение false, а также явно задать запускающего события.** Использование `<Triggers>` коллекция гораздо более четкое способ обработки событий и может предотвратить непредвиденное поведение, помогая с задачами обслуживания и принудительное разработчику возможность подписаться на событие.
- **Наименьший можно используйте для реализации функциональности.** Как указано в описании службы почтовый индекс, упаковки только минимальный набор снижает время для сервера, общее обработки и места, занимаемого клиент сервер exchange, повышение производительности.

## <a name="the-updateprogress-control"></a>Элемент управления UpdateProgress

## <a name="updateprogress-control-reference"></a>Ссылка на элемент управления UpdateProgress

Свойства с поддержкой разметку:

| **Имя свойства** | **Тип** | **Описание** |
| --- | --- | --- |
| AssociatedUpdate PanelID | Строка | Указывает идентификатор UpdatePanel, которые этот UpdateProgress следует в отчет. |
| DisplayAfter | Int | Указывает время ожидания в миллисекундах перед отображением этого элемента управления, после начала асинхронного запроса. |
| DynamicLayout | bool | Определяет, отображаются ли ход выполнения динамически. |

Потомки разметку:

| **Тег** | **Описание** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Содержит шаблон элемента управления для содержимого, которое будет отображаться с этим элементом управления. |

Элемент управления UpdateProgress предоставляет измерение отзывов сохранение процент пользователей при выполнения необходимых действий для передачи на сервер. Это может помочь пользователей о том, что вы выполняете что-то даже если он могут остаться незамеченными, особенно в том случае, поскольку большинство пользователей используются для страницы, обновление и отображается в строке состояния выделения.

В качестве примечания элементов управления UpdateProgress может находиться в любом в иерархии страниц. Однако в случаях, в которых частичного обратную передачу инициируется из дочерних UpdatePanel, (где UpdatePanel вложен в другой UpdatePanel), обратные передачи этим триггером дочерние UpdatePanel вызовет UpdateProgress чтобы шаблоны отображались для дочернего элемента UpdatePanel, а также родительский UpdatePanel. Но, если триггер является прямым потомком родительского UpdatePanel, будет отображен только UpdateProgress шаблоны, связанные с родительским объектом.

## <a name="summary"></a>Сводка

Расширения Microsoft ASP.NET AJAX являются сложные продуктов, предназначенных для помощи в доступности веб-содержимого и предоставить пользователю более широкие возможности для веб-приложений. В рамках расширения ASP.NET AJAX, частичной отрисовки элементов управления, включая элементы управления ScriptManager, UpdatePanel и UpdateProgress приведены некоторые наиболее видимым компонентов набора средств.

Компонент ScriptManager интегрирует подготовки клиента JavaScript для расширений, а также включает различные компоненты стороне сервера и клиента для работы совместно с минимальными разработки инвестиций.

Управления UpdatePanel является очевидным magic поле - разметка в рамках UpdatePanel могут иметь Codebehind на стороне сервера, но не активирует обновления страницы. Элементы управления UpdatePanel могут быть вложенными и могут зависеть от элементов управления в другие UpdatePanel. По умолчанию элементы UpdatePanel обрабатывать любые обратных передач, вызываемых своих дочерних элементов управления, несмотря на то, что эта функция может быть точно настроенных, декларативно или программно.

При использовании элемента управления UpdatePanel, разработчикам следует учитывать влияние на производительность, потенциально может возникнуть. Потенциальные предусмотрены следующие ответные веб-служб и методы страницы на то, что следует учитывать разработки приложения.

UpdateProgress управления позволяет пользователю знать, что он не будет пропущен, фоновый запроса происходит во время выполнения страницы — в противном случае не выполняя никаких действий в ответ на ввод данных пользователем. Она также включает возможность прервать частичную отрисовку результатов.

Вместе эти средства помогают создает широкие возможности и удобный пользовательский интерфейс рабочего сервера менее очевидными для пользователя и менее прерывания рабочего процесса.

## <a name="bio"></a>Биография

Скотт кате работает с веб-технологиями Майкрософт с 1997 и – президент myKB.com ([www.myKB.com](http://www.myKB.com)) приложений, основное внимание уделено базы знаний программные решения, где он специализируется на написание ASP.NET на основе. Скотт можно обратиться по электронной почте [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) или его блог по [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Вперед](understanding-asp-net-ajax-updatepanel-triggers.md)
