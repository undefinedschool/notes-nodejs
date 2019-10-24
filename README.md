# [WIP] Notas sobre NodeJS (Server-Side JavaScript)

## Instalación

- **Windows:** ir a la [página oficial](https://nodejs.org/en/), descargar e instalar el binario de la versión **LTS**
- **Linux / OS X:** descargar e instalar el script [nvm](https://github.com/nvm-sh/nvm/) y luego instalar la versión **LTS** con el comando `nvm install --lts`

Para chequear que se haya instalado correctamente, correr el comando `node -v` en la consola (debe retornar una versión >= 12)

> **¿Qué es _LTS_?** Leer [LTS vs Current version](https://stackoverflow.com/questions/33661274/what-are-the-differences-between-long-term-support-lts-and-stable-versions-of)

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

http.createServer(onRequest)
  .listen(PORT, error => console.error('THIS IS FINE. 🔥🔥🔥🚒'));

console.log(`Server listening on ${HOSTNAME}:${PORT}`);
```
