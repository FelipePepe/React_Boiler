# React - TypeScript - Babel - Webpack

Pasos para realizar la intalación de React con Typescript y webpack.
Como comenzar.

Creamos le directorio del proyecto. El nombre del proyecto no puede empezar por mayúculas o tener espacios.

```
$ mkdir project
```

Entramos dentro del directorio e inicializamos el proyecto.

```
$ cd proyect-name
$ npm init -y
```

Una vez inicializado el proyecto, dentro del directorio se ha creado el fichero package.json, el cual contendrá la configuración del proyecto.

Ahora instalamos las dependencias.
Empezamos el instalando Webpack (empaquetador de módulo), como dependencias de desarrollo.

```
$ npm install webpack webpack-cli --save-dev
```

Una vez terminada la instalación de Webpack, procedemos a instalar React, también como dependencias de desarrollo ya que podemos tener diferentes proyectos con diferentes versiones de React.

```
$ npm install react react-dom --save-dev
$ npm install @types/react @types/react-dom --save-dev
```

Ha llegado el momento de instalar Typescript en el proyecto.

```
$ npm install typescript --save-dev
```

Una vez instalado typescript debemos de crear el fichero de configuración de typescript, tsconfig.json

```
{
  "compilerOptions": {
    "target": "es6",
    "module": "es6",
    "moduleResolution": "node",
    "declaration": false,
    "noImplicitAny": false,
    "allowSyntheticDefaultImports": true,
    "sourceMap": true,
    "jsx": "react",
    "noLib": false,
    "suppressImplicitAnyIndexErrors": true,
    "skipLibCheck": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

Pasamos a instalar babel.

```
$ npm install @babel/cli @babel/core babel-loader @babel/preset-react @babel/preset-typescript @babel/preset-env --save-dev
```

Una vez instalados los paquetes de babel, creamos el fichero de configuración _.babelrc_

```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-typescript",
    "@babel/preset-react"
  ]
}
```

Ahora vamos a crear el fichero de configuración de Webpack, _webpack.config.js_

```
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");
const basePath = __dirname;

module.exports = {
  context: path.join(basePath, "src"),
  resolve: {
    extensions: [".js", ".ts", ".tsx"],
  },
  entry: {
    app: ["./index.tsx"],
  },
  devtool: "eval-source-map",
  stats: "errors-only",
  output: {
    filename: "[name].[chunkhash].js",
  },
  module: {
    rules: [
      {
        test: /.tsx?$/,
        exclude: /node_modules/,
        loader: "babel-loader",
      },
      {
        test: /.(png|jpg)$/,
        exclude: /node_modules/,
        loader: "url-loader?limit=5000",
      },
      {
        test: /.html$/,
        loader: "html-loader",
      },
    ],
  },
  plugins: [
    //Generate index.html in /dist => https://github.com/ampedandwired/html-webpack-plugin
    new HtmlWebpackPlugin({
      filename: "index.html", //Name of file in ./dist/
      template: "index.html", //Name of template in ./src
    }),
  ],
};
```

Vamos a instalar las dependencias de desarrollo html-loader, url-loader.

```
$ npm install html-loader url-loader file-loader html-webpack-plugin --save-dev
```

Vamos a modificar el fichero package.json, el apartado script.

```
"start": "run-p -l type-check:watch start:dev",
"type-check": "tsc --noEmit",
"type-check:watch": "npm run type-check -- --watch",
"start:dev": "webpack-dev-server --mode development --open",
"build": "rimraf dist &amp;&amp; webpack --mode development"
```

Los comandos start o build contienen ordenes que aún no han sido instaladas, run-p y rimraf.

```
$ npm install rimraf npm-run-all --save-dev
```

Ya está toda la configuración inicial creada con todo necesario para comenzar. Procedemos a crear el directorio src.

```
$ mkdir src
```

Entramos dentro de la carpeta src y añadimos los ficheros principales de react.

```
$ cd src
```

Creamos el fichero src/index.html

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>My App Example</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

Creamos el fichero src/index.tsx

```
import React from "react";
import ReactDOM from "react-dom";
import { App } from "./app";

ReactDOM.render(
  <div>
    <App />
  </div>,
  document.getElementById("root")
);
```

Creamos el fichero src/app.tsx

```
import React from "react";

export const App = () => {
  return <h1>Hello React !!</h1>;
};
```

Lanzamos la orden para ejecutar el proyecto.

```
npm start
```

<figure class="wp-block-image size-large is-resized"><img src="https://undesastreinformatico.files.wordpress.com/2020/07/helloreact.png?w=766" alt="" class="wp-image-76" width="576" height="307"/></figure>
