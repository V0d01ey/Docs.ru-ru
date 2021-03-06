---
title: "Справочник по конфигурации ASP.NET модуль Core"
author: guardrex
description: "Подробные сведения о настройке модуля ASP.NET Core для размещения приложений ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Справочник по конфигурации ASP.NET модуль Core

По [Latham Люк](https://github.com/guardrex), [Рик Андерсон](https://twitter.com/RickAndMSFT), и [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Этот документ содержит инструкции о том, как настроить модуль ASP.NET Core для размещения приложений ASP.NET Core. Введение модуль ASP.NET Core и инструкции по установке см. в разделе [Общие сведения о модуле ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Конфигурация с web.config

Модуль Core ASP.NET настроена с `aspNetCore` раздел `system.webServer` узел на сайте *web.config* файла.

Следующие *web.config* файл публикуется для [развертывания зависит от framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) и настраивает модуль Core ASP.NET для обработки запросов для сайта:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Следующие *web.config* опубликована для [автономное развертывание](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Когда приложение развертывается в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` задан путь к `\\?\%home%\LogFiles\stdout`. Путь сохраняет журналы stdout *LogFiles* папку, в которой находится в расположении, автоматически созданный с помощью службы.

В разделе [конфигурации подзапроса приложений](xref:host-and-deploy/iis/index#sub-application-configuration) для важное примечание, относящиеся к конфигурации *web.config* файлы в приложениях для вложенных.

### <a name="attributes-of-the-aspnetcore-element"></a>Атрибуты элемента aspNetCore

| Атрибут | Описание | По умолчанию |
| --------- | ----------- | :-----: |
| `arguments` | <p>Необязательный строковый атрибут.</p><p>Аргументы для исполняемого файла, указанного в **processPath**.</p>| |
| `disableStartUpErrorPage` | true или false.</p><p>Если значение равно true, **502.5 - сбой процесса** подавляется страницы и кодовую страницу 502 состояния на *web.config* имеет более высокий приоритет.</p> | `false` |
| `forwardWindowsAuthToken` | true или false.</p><p>Значение true, если маркер отправляется дочернего процесса, прослушивающего порт % ASPNETCORE_PORT % как заголовок «MS-ASPNETCORE-WINAUTHTOKEN» каждого запроса. Он отвечает этот процесс для вызова CloseHandle по этому токену каждого запроса.</p> | `true` |
| `processPath` | <p>Обязательный строковый атрибут.</p><p>Путь к исполняемому файлу, который запускает процесс, который прослушивает HTTP-запросов. Поддерживаются относительные пути. Если путь начинается с `.`, путь считается относительно корневого каталога сайта.</p> | |
| `rapidFailsPerMinute` | <p>Необязательный целочисленный атрибут.</p><p>Указывает число раз в процесс **processPath** может завершиться сбоем в минуту. Если этот предел превышен, модуль останавливает запуск процесса в течение оставшейся части минуты.</p> | `10` |
| `requestTimeout` | <p>Timespan необязательный атрибут.</p><p>Указывает продолжительность, для которого модуль ASP.NET Core ожидает ответа от процесса, прослушивающего порт % ASPNETCORE_PORT %.</p><p>`requestTimeout` Должно быть указано в целых минутах, в противном случае по умолчанию на 2 минуты.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Необязательный целочисленный атрибут.</p><p>Длительность в секундах, для которых модуль ожидает исполняемый файл для корректного завершения работы при *app_offline.htm* обнаружен файл.</p> | `10` |
| `startupTimeLimit` | <p>Необязательный целочисленный атрибут.</p><p>Длительность в секундах модуль для исполняемого файла для запуска процесса, прослушивающего порт. При превышении этого ограничения модуль процесс. Модуль попытается перезапустить процесс при получении нового запроса и попытаться перезапустить процесс на последующие входящие запросы, если не удается запустить приложение по-прежнему **rapidFailsPerMinute** раз за последние последовательное минуты.</p> | `120` |
| `stdoutLogEnabled` | <p>Дополнительный логический атрибут.</p><p>Если значение равно true, **stdout** и **stderr** для процесса, указанного в **processPath** перенаправляются к файлу, заданному в **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Необязательный строковый атрибут.</p><p>Указывает относительный или абсолютный путь, для которого **stdout** и **stderr** из процесса, указанного в **processPath** регистрируются. Относительные пути задаются относительно корневого каталога сайта. Любой путь, начинающийся с `.` являются относительно узла корневого и другими путями, считаются абсолютные пути. Все папки, путь должен существовать для модуля, который нужно создать файл журнала. С помощью разделителей подчеркивания, timestamp, идентификатор процесса и расширение файла (*.log*) добавляются к последнему сегмент **stdoutLogFile** пути. Если `.\logs\stdout` передается как значение, создается журнал в stdout примере сохраняется как *stdout_20180205194132_1934.log* в *журналы* папки при сохранении на 2, 5/2018 в 19:41:32 с Идентификатором процесса 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Задание переменных среды

Переменные среды можно указать в процесс `processPath` атрибута. Укажите переменную среды с `environmentVariable` дочерний элемент элемента `environmentVariables` элемент коллекции. Переменные среды, заданные в этом разделе имеют приоритет над системных переменных среды.

В следующем примере две переменные среды. `ASPNETCORE_ENVIRONMENT` Настраивает среду приложения для `Development`. Разработчик может временно задать это значение *web.config* файл, чтобы принудительно реализовать [страницы разработчик исключений](xref:fundamentals/error-handling) для загрузки при отладке приложения исключения. `CONFIG_DIR` Примером может служить переменной пользовательской среды, где разработчик написал код, который считывает значение во время запуска, чтобы сформировать путь для загрузки файла конфигурации приложения.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> Задать только `ASPNETCORE_ENVIRONMENT` envirnonment переменной `Development` на промежуточных и тестовых серверах, которые не доступны к ненадежным сетям, например в Интернете.

## <a name="appofflinehtm"></a>app_offline.htm

Если файл с именем *app_offline.htm* обнаруживается в корневом каталоге приложения, модуль ASP.NET Core пытается корректного завершения работы приложения и остановка обработки входящих запросов. Если приложение по-прежнему выполняется через количество секунд, определенные в `shutdownTimeLimit`, модуль ASP.NET Core разрывает выполняющийся процесс.

Хотя *app_offline.htm* файл присутствует, модуль ASP.NET Core отвечает на запросы, отправляет содержимое *app_offline.htm* файла. Когда *app_offline.htm* файл удаляется, следующий запрос запускает приложение.

## <a name="start-up-error-page"></a>При запуске страницы ошибок

Если модуль Core ASP.NET не удалось запустить фоновой обработки или начинается процесс серверной части, но не удается прослушать настроенный порт *502.5 сбой процесса* появится страница кода состояния. Отмените запуск этой страницы и вернуться к кодовой странице состояние IIS 502 по умолчанию, используйте `disableStartUpErrorPage` атрибута. Дополнительные сведения о настройке пользовательские сообщения об ошибках см. в разделе [ошибки HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 страница кода состояния сбоя процесса](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Создание журнала и перенаправления

Модуль Core ASP.NET перенаправляет `stdout` и `stderr` журналы на диск, если `stdoutLogEnabled` и `stdoutLogFile` атрибуты `aspNetCore` задается. Все папки в `stdoutLogFile` путь должен существовать в порядке для модуля, который нужно создать файл журнала. Отметка времени и расширением файла добавляются автоматически при создании файла журнала. Журналы не заменяется, пока не произойдет перезапуск или перезапуска процесса. Он отвечает поставщика услуг размещения, чтобы ограничить дисковое пространство, которое использовать журналы. С помощью `stdout` журнала рекомендуется только для устранения неполадок при запуске приложения. Не используйте stdout журнала для ведения журнала Общие приложения. Для процедуры входа в приложение ASP.NET Core, используйте библиотеку ведения журнала, которая ограничивает размер файла журнала и поворачивает журналы. Дополнительные сведения см. в разделе [сторонних регистраторов](xref:fundamentals/logging/index#third-party-logging-providers).

Имя файла журнала составляется путем добавления timestamp, идентификатор процесса и расширение файла (*.log*) для последнего сегмента `stdoutLogFile` пути (обычно *stdout*) с разделителями, символами подчеркивания. Если `stdoutLogFile` путь заканчивается *stdout*, имя файла имеет журналов для приложений с PID 1934, созданные на 2, 5/2018 в 19:42:32 *stdout_20180205194132_1934.log*.

Следующий пример `aspNetCore` элемент настраивает `stdout` входа для приложения, размещенного в службе приложений Azure. Локальный путь или путь к сетевой папке приемлемо для ведения локального журнала. Убедитесь, что удостоверение AppPool пользователя имеется разрешение на запись в указанный путь.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

В разделе [конфигурации с web.config](#configuration-with-webconfig) пример `aspNetCore` элемент в *web.config* файла.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Модуль ASP.NET Core с IIS общей конфигурации

Модуль ASP.NET Core программа установки запускается с правами **системы** учетной записи. Так как учетная запись локальной системы не имеют права на изменение для путь к общей папке, используемой общей конфигурации IIS, установщик достигает отказ в доступе при попытке настроить параметры для модуля *applicationHost.config* в общей папке. При использовании общей конфигурации IIS, выполните следующие действия.

1. Отключение общей конфигурации IIS.
1. Запустите установщик.
1. Экспорт обновленного *applicationHost.config* файл в общую папку.
1. Повторное включение общей конфигурации IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Версия модуля и размещения журналы установщика пакета

Чтобы определить версию установленного модуля ASP.NET Core:

1. На принимающей системе, перейдите к *%windir%\System32\inetsrv*.
1. Найдите *aspnetcore.dll* файла.
1. Щелкните правой кнопкой мыши файл и выберите **свойства** из контекстного меню.
1. Выберите **сведения** вкладки. **Версия файла** и **версия продукта** представляют установленную версию модуля.

Журналы Windows Server, где размещены пакет установщика для модуля находятся в *C:\\пользователей\\% UserName %\\AppData\\локального\\Temp*. Этот файл имеет имя *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Модуль, схемы и конфигурации расположения файлов

### <a name="module"></a>выхода

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Схема

**Службы IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Конфигурация

**Службы IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Файлы можно найти путем поиска *aspnetcore.dll* в *applicationHost.config* файла. Для IIS Express *applicationHost.config* файл не должен существовать по умолчанию. Файл будет создан в  *\<application_root >\\удаляйте\\config* при запуске приложения любой веб-проект в решении Visual Studio.
