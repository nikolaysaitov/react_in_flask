Инструкция по интеграции React приложения в Flask

1. Создание HTML страницы. Создайте HTML страницу, в которую будет вставляться ваше React-приложение. Например, test.html. В этой странице должен быть блок с id="root_test_react", внутри которого будет размещено React-приложение.

```bash
# base.html
{% extends 'base.html' %}

{% block content %}
<div id="root_test_react"></div>
{% endblock %}
```

2. Добавление роутов в Flask. Добавьте маршрут, который будет отображать вашу HTML страницу.

```bash
# segmentation.py
@segmentation_module.route("/test", methods=["GET", "POST"])
def test():
    return render_template("./segmentation/test.html")
```

Разместите ссылку на страницу в нужном месте:

```bash
# html
<a href="/segmentation/test">Test</a>
```

3. Настройка React-приложения. Создайте папку для React-приложения внутри папки static. Например, reactApp.

Выполните инициализацию npm проекта:

```bash
# reactApp
npm init -y
```

Обновите скрипты в package.json:

```bash
# package.json
"scripts": {
  "dev": "webpack --watch --mode development ./src/index.tsx --output-path ./",
  "build": "webpack --mode production ./src/index.tsx --output-path ./"
}
```

Установите необходимые зависимости:

```bash
# reactApp
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader babel-plugin-transform-class-properties ts-loader webpack webpack-cli
npm install react react-dom @types/react @types/react-dom
npm i style-loader css-loader file-loader react-svg-loader

```

Создайте файл .babelrc:

```bash
# .babelrc
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
  "plugins": [
    "transform-class-properties"
  ]
}

```

Создайте файл webpack.config.js:

```bash
# webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-react", "@babel/preset-env"],
          },
        },
      },
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.tsx?$/,
        use: {
          loader: 'ts-loader',
          options: {
            logLevel: 'info',
          },
        },
        exclude: /node_modules/,
      },
      {
        test: /\.(png|jpe?g|gif)$/i,
        use: [
          {
            loader: "file-loader",
          },
        ],
      },
      {
        test: /\.svg$/,
        use: [
          {
            loader: "babel-loader",
          },
          {
            loader: "react-svg-loader",
            options: {
              jsx: true, // true outputs JSX tags
            },
          },
        ],
      },
      {
        test: /\.html$/i,
        loader: "html-loader",
      },
    ],
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  output: {
    filename: "script.js",
  },
};

```

Создайте файл tsconfig.json:

```bash
# tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": false,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ]
}
```

4. Создание React-компонента. Создайте папку src и файл index.tsx в ней:

```bash
#  index.tsx
import React from "react";
import ReactDOM from "react-dom/client";

const root = ReactDOM.createRoot(document.getElementById("root_test_react") as HTMLElement);
root.render(
  <div>
    <h1>Hello, world!</h1>
  </div>
);

import React from "react";
import ReactDOM from "react-dom/client";

const root = ReactDOM.createRoot(document.getElementById("root_test_react") as HTMLElement);
root.render(
  <div>
    <h1>Hello, world!</h1>
  </div>
);

```

5. Добавление скрипта в HTML страницу. Добавьте ссылку на выходной скрипт, который будет создан Webpack, в ваш test.html:

```bash
# test.html;
{% extends 'base.html' %}

{% block content %}

{% set static = 'reactApp/script.js' %}
<div id="root_test_react"></div>
<script src="{{ url_for('static', filename=static) }}"></script>

{% endblock %}

```

6. Запуск
   Выполните команду для запуска :

```bash
# reactApp;
npm run dev

```

ГОТОВО! Ваше React-приложение теперь интегрировано с Flask.
