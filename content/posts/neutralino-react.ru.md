---
title: "Настройка и использование React в NeutralinoJS"
date: 2022-05-18T22:29:57+07:00
tags: ["JS"]
author: "qBite"
---

## Введение
Официальный гайд по тому как настроить NeutralinoJS можно найти [здесь](https://neutralino.js.org/docs/how-to/use-a-frontend-library). Этот пост лишь моя вариация настройки.

Репозиторий в github: https://github.com/Bit-of-Meat/neutralino-react

## Создание проекта
Создадим create-react-app проект:
```
npx create-react-app neutralino-react
```
Теперь создадим ``neutralino.config.json`` файл и напишем в нём следующее:
```json
{
  "applicationId": "js.neutralino.react",
  "version": "0.0.1",
  "defaultMode": "window",
  "documentRoot": "/build/",
  "url": "/",
  "enableServer": true,
  "enableNativeAPI": true,
  "tokenSecurity": "one-time",
  "nativeAllowList": [
    "app.*",
    "os.*",
    "debug.log"
  ],
  "modes": {
    "window": {
      "title": "neutralino-react",
      "width": 800,
      "height": 500,
      "minWidth": 400,
      "minHeight": 200,
      "icon": "/public/logo192.png"
    }
  },
  "cli": {
    "binaryName": "neutralino-react",
    "resourcesPath": "/build/",
    "clientLibrary": "/public/neutralino.js",
    "binaryVersion": "4.5.0",
    "clientVersion": "3.4.0",
    "frontendLibrary": {
      "patchFile": "/public/index.html",
      "devUrl": "http://localhost:3000"
    }
  }
}
```
Затем зайдём в ``/public/index.html`` файл и добавим в тег ``<head>`` клиентский скрипт neutralino:
```html
<script src="%PUBLIC_URL%/neutralino.js"></script>
```

Также добавим в конец ``.gitignore`` строки для neutralino:
```bash
# Neutralinojs binaries and builds
/bin
/dist

# Neutralinojs client (minified)
neutralino.js

# Neutralinojs related files
.storage
*.log
```
В ``package.json`` изменим скрипт сборки добавив сборку neutralino:
```json
"scripts": {
  "build": "react-scripts build && neu build"
}
```
После этого обновим neutralino запустив команду:
```
neu update
```

## React

В конец ``src/index.js`` файла добавим:
```js
window.Neutralino.init();
```
И изменим ``App.js`` файл:
```js
import { useState, useEffect } from 'react';

import logo from './logo.svg';
import './App.css';

function App() {
  const [username, setUsername] = useState();

  useEffect(() => {
    async function getUsername() {
      const key = window.NL_OS === 'Windows' ? 'USERNAME' : 'USER';
      let value = '';
      try {
          value = await window.Neutralino.os.getEnv(key);
      }
      catch(err) {
          console.error(err);
      }
      setUsername(value);
    };
    getUsername();
  }, [setUsername]);

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Hello, {username}
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```

## Hot reloading
Для удобства разработки добавим [concurrently](https://www.npmjs.com/package/concurrently), который позволит на организовать одновременный запуск нескольких команд:
```
npm i concurrently
```
В ``package.json`` добавим следующий скрипт для разработки:
```json
"scripts": {
    "dev": "concurrently --kill-others \"BROWSER=false react-scripts start\" \"neu run --frontend-lib-dev\""
}
```
* ``react-scripts start`` - запускает dev сервер react'а.
* ``neu run --frontend-lib-dev`` - запускает neutralino в режиме frontend-lib. 

Флаг ``--kill-others`` означает что если одна из команд упадёт, то все остальные процессы, указанные в командах, будут закрыты.
