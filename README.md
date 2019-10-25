> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)

> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc

# [WIP] Notas sobre NodeJS (Server-Side JavaScript)

## Instalación

- **Windows:** ir a la [página oficial](https://nodejs.org/en/), descargar e instalar el binario de la versión **LTS**
- **Linux / OS X:** descargar e instalar el script [nvm](https://github.com/nvm-sh/nvm/) y luego instalar la versión **LTS** con el comando `nvm install --lts`

Para chequear que se haya instalado correctamente, correr el comando `node -v` en la consola (debe retornar una versión >= 12)

> **¿Qué es _LTS_?** Leer [LTS vs Current version](https://stackoverflow.com/questions/33661274/what-are-the-differences-between-long-term-support-lts-and-stable-versions-of)

## ¿Qué es NodeJS?

_NodeJS_ ó _Node_ a secas, es principalmente un _entorno de ejecución_, algo que nos brinda el _contexto_ necesario para poder ejecutar código JavaScript por fuera de un browser. 

**Recordemos que JavaScript sólo funciona en el browser de forma _nativa_ y que se trata de un lenguaje de _alto nivel_**. Esto significa que realizar tareas como **_networking_**, acceder a hardware de red (ej: acceder a la tarjeta de red) para poder _escuchar requests_ y responder, **_acceder al file system_** del sistema operativo que estemos usando, para leer archivos (ej: un documento HTML) y enviarlos, poder levantar y correr un _server_, comunicarnos con una _base de datos_, desarrollar una _API_, etc, **no son capacidades propias de un lenguaje como JavaScript**.

Es por esto que necesitamos un **intermediario**, algo así como una _API_ que nos permita, escribiendo el código JavaScript que ya conocemos, acceder a este tipo de funcionalidades, necesarias para el _backend_ de nuestra aplicación.

Vamos a llamar **_entorno de ejecución_** a todo lo que necesitamos para poder ejecutar nuestro código e interactuar con estas funcionalidades. **Node** es quien nos va a dar este _entorno_ y nos va a permitir ejecutar código JavaScript prácticamente en todos lados.

### V8

Uno de los componentes de **Node** es **[V8](https://v8project.blogspot.com.ar)**, un _engine_ de JavaScript que se encarga de _parsear_, _compilar_, _optimizar_, _interpretar_ y ejecutar nuestro código

![](https://i.imgur.com/a0o7qLc.png)

Aparte de **V8**, tenemos el lenguaje **C++**, el cual, a traves de ciertas librerias que vienen con **Node** (como [libuv](https://github.com/libuv/libuv)), nos permiten acceder a _funcionalidad de más bajo nivel_ como las mencionadas antes e interactucar directamente con el sistema operativo. Para acceder a esa funcionalidad, vamos a utilizar la [**_API_**](https://nodejs.org/dist/latest-v12.x/docs/api/) que nos provee Node.

### Entorno + API

Por lo tanto, podríamos decir que **Node** termina siendo un _entorno de ejecución_ para poder correr código JavaScript y una _API_ (ó librería) que nos provee acceso a funcionalidades necesarias para el backend de nuestra aplicación.

## REPL

_RePL_ viene de **R**ead, **E**val, **P**rint, **L**oop. Nos permite ejecutar Node en la terminal para probar cosas, como si se tratase de la consola del browser.

- [https://flaviocopes.com/node-repl/](https://flaviocopes.com/node-repl/)

```js
// 1
console.log("Hello World");
```

```bash
// 2
node helloworld.js
```

## Server

```js
// 3
const http = require("http");

// `createServer()` retorna un objeto, que tiene el método `listen`
const server = http.createServer();
server.listen(8888);
```

```bash
// 4
node server.js
```

- Cuando el servidor recibe un _request_, se dispara el evento [`request`](https://nodejs.org/api/http.html#http_event_request) y se ejecuta el _callback_ que recibe `createServer`. Este callback tiene 2 parámetros, los objetos `request` y `response`

```js
// 5
// en un archivo server.js
const http = require("http");

http.createServer((request, response) => {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```

```bash
// 6
node server.js
```

- Instalar y usar el módulo [`nodemon`](https://www.npmjs.com/package/nodemon)

```js
// 7
// refactoring
const http = require("http");
const HOSTNAME = '127.0.0.1'
const PORT = 8888;

function onRequest(request, response) {
  console.log("Request received.");
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}

http.createServer(onRequest).listen(PORT, error => {
  if (error) {
    console.error('THIS IS FINE. 🔥🔥🔥🚒');
  } else {
    console.log(`Server listening on http://${HOSTNAME}:${PORT}`);
  }
});
```
