---
title: "Создание мода для Terraria 1.4 Journey's End"
date: 2021-05-06T01:04:57+07:00
tags: ["Моддинг"]
summary: "Создание аддона который включает в себя модель."
author: "qBite"
draft: true
---

## Подготовка
### Visual Studio Installer
Для начала скачиваем [Visual Studio 2022 [Скачать]](https://visualstudio.microsoft.com/ru/downloads/).

> Примечание: выбрать нужно имеено последнюю Visual Studio 2022 т.к tModLoader для Terraria v1.4 работает с пакетом .NET Framework 4.8.

Открываем установщик и выбираем пункт ``Разработка классических приложений .NET``:
![Visual Studio Installer](visual-studio-installer.png)

### tModLoader
Также зайдём в Steam и добавим себе tModLoader:

{{< steam id="1281930" width="700" >}}

Зайдём в свойства tModLoader'а и выберем ``1.4-stable - ALPHA. Testing monthly-updated & tested stable branch of tModLoader for 1.4 Terraria``.

![Смена версии](mod/change-version.png)

После этого зайдём в tModLoader, включим режим разработчика нажав на ``Enable developer mode``

![Enable developer mode](mod/enable-developer-mode.png)

После включения перейдём в Mod sources, создадим мод нажав на ``Create mod`` и введя данные:

![Создание мода](mod/mod-settings.png)

## Разработка

Чтобы изменить код, мода нажмём на open .csproj, после измененией нужно нажать на ``Build + Reload``:

![Сборка мода](mod/build-mod.png)


## Тестирование

Для тестирования мода лучше скачать себе мод ``Cheet Sheet``, с помощью которого можно выдавать себе предметы:

![CheatSheet](cheatsheet.png)