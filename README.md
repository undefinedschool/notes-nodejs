> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)

> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc

# [WIP] Notas sobre NodeJS (Server-Side JavaScript)

## Instalación

- **Windows:** ir a la [página oficial](https://nodejs.org/en/), descargar e instalar el binario de la versión **LTS**
- **Linux / OS X:** descargar e instalar el script [nvm](https://github.com/nvm-sh/nvm/) y luego instalar la versión **LTS** con el comando `nvm install --lts`

Para chequear que se haya instalado correctamente, correr el comando `node -v` en la consola (debe retornar una versión >= 12)

> **¿Qué es _LTS_?** Leer [LTS vs Current version](https://stackoverflow.com/questions/33661274/what-are-the-differences-between-long-term-support-lts-and-stable-versions-of)

## ¿Qué es NodeJS?

_NodeJS_ ó _Node_ a secas, es principalmente un _entorno de ejecución_, es decir, nos brinda el _contexto_ necesario para poder ejecutar código JavaScript por fuera de un browser. 

**Recordemos que JavaScript sólo funciona en el browser de forma _nativa_ y que se trata de un lenguaje de _alto nivel_**. Esto significa que realizar tareas como **_networking_**, acceder a hardware de red (ej: acceder a la tarjeta de red) para poder _escuchar requests_ y responder, **_acceder al file system_** del sistema operativo que estemos usando, para leer archivos (ej: un documento HTML) y enviarlos, poder levantar y correr un _server_, comunicarnos con una _base de datos_, desarrollar una _API_, etc, **no son capacidades propias de un lenguaje como JavaScript**.

Es por esto que necesitamos un **intermediario**, algo así como una _API_ que nos permita, escribiendo el código JavaScript que ya conocemos, acceder a este tipo de funcionalidades, necesarias para el _backend_ de nuestra aplicación.

Vamos a llamar **_entorno de ejecución_** a todo lo que necesitamos para poder ejecutar nuestro código e interactuar con estas funcionalidades. **Node** es quien nos va a dar este _entorno_ y nos va a permitir ejecutar código JavaScript prácticamente en todos lados.

### V8

Uno de los componentes de **Node** es **[V8](https://v8project.blogspot.com.ar)**, un _engine_ de JavaScript que se encarga de _parsear_, _compilar_, _optimizar_, _interpretar_ y ejecutar nuestro código

![](https://i.imgur.com/a0o7qLc.png)

Aparte de **V8**, tenemos el lenguaje **C++**, el cual, a traves de ciertas librerias que vienen con **Node** (como [libuv](https://github.com/libuv/libuv)), nos permiten acceder a _funcionalidad de más bajo nivel_ como las mencionadas antes e interactucar directamente con el sistema operativo. Para acceder a esa funcionalidad, vamos a utilizar la [**_API_**](https://nodejs.org/dist/latest-v12.x/docs/api/) que nos provee Node.

### Entorno + API

Por lo tanto, podríamos decir que **Node** termina siendo un _entorno de ejecución_ para poder correr código JavaScript y una _API_ (ó librería) que nos provee acceso a funcionalidades necesarias para el backend de nuestra aplicación.

Gracias a esta API, Node nos permite construir desde pequeñas aplicaciones de línea de comandos (_CLI_), hasta _servidores HTTP_ para crear sitios dinámicos y aplicaciones web.

## REPL

_REPL_ es una sigla que viene de **R**ead, **E**val, **P**rint, **L**oop. Nos permite ejecutar Node en la terminal para probar cosas, como si se tratase de la consola del browser.

- [https://flaviocopes.com/node-repl/](https://flaviocopes.com/node-repl/)

```js
// 1
console.log("Hello World");
```

```bash
// 2
node helloworld.js
```

## Node nos permite escribir código _asincrónico_

```js
// sync version
const result = database.query("SELECT * FROM veryHugeTable");
console.log("Hello World");
```

```js
// async version
database.query("SELECT * FROM hugetable", function(rows) {
  const result = rows;
});
console.log("Hello World");
```

## Leyendo argumentos

Para leer argumentos a través de la terminal/CLI, podemos utilizar [`process.argv`](https://nodejs.org/docs/latest-v12.x/api/process.html#process_process_argv), que nos da acceso a una _especie de Array_ de strings. Notar que este pseudo-array también incluye como argumentos el comando que usamos para correr nuestro script y el nombre del archivo, por lo que los argumentos que nos interesan comienzan recién a partir de la posición/índice 2.

## File System

Podemos usar Node para leer y escribir archivos, a través del módulo `fs`

Ver [the File System module](https://eloquentjavascript.net/20_node.html#h_o2abiQU0TD)

- Crear un archivo `readMe.txt` (en el mismo directorio donde tengamos nuestro `index.js`) con el contenido de [este txt](https://gist.githubusercontent.com/nhsz/8442053a20604ede482a2f4c506f83f9/raw/c6b51faf561a04211dcdfafc562f4f423dd4062b/payload.txt)

### Sync

- [`fs.readFileSync()`](https://nodejs.org/dist/latest-v12.x/docs/api/fs.html#fs_fs_readfilesync_path_options)

```js
const fs = require('fs');

const { readFileSync } = fs;
const txt = readFileSync('readMe.txt', 'utf-8');

console.log(txt);
```

#### Escribir un archivo

- [`fs.writeFileSync`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_writefilesync_file_data_options)

```js
const fs = require('fs');

const { readFileSync } = fs;
const readMe = readFileSync('readMe.txt', 'utf-8');
writeFileSync('writeMe.txt', readMe);

console.log(txt);
```

### Async

- [`fs.readFile()`](https://nodejs.org/dist/latest-v12.x/docs/api/fs.html#fs_fs_readfile_path_options_callback)

```js
const fs = require('fs');

const { readFile } = fs;

readFile('readMe.txt', 'utf8', (err, data) => {
  if (err) {
    throw err
  };
  
  console.log(data)
});

// const readFile = util.promisify(fs.readFile)
// readFile("path/to/myfile").then(file => console.log(file))
```

- [`fs.writeFile`](https://nodejs.org/dist/latest-v12.x/docs/api/fs.html#fs_fs_writefile_file_data_options_callback)

```js
const fs = require('fs');

const { readFile: read, writeFile: write } = fs;

read('readMe.txt', 'utf-8', (err, data) => {
  if (err) {
    console.error(err);
  }

  write('writeMe.txt', data, err => console.error(err));
})

console.log('HELLO!');
```

#### Borrar archivos

:warning: **Sólo podemos eliminar archivos que existan, caso contrario `unlink` va a retornar un error**

- [`fs.unlink`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_unlink_path_callback)

```js
const fs = require('fs');

const { unlink: del } = fs;
const filePath = 'writeMe.txt';

del(filePath, err => {
  if (err) {
    throw err;
  }

  console.log(`${filePath} was deleted succesfully.`)
});
```

#### Copiar archivos

Usar [`fs.copyFile`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_copyfile_src_dest_flags_callback) para copiar `'readMe.txt'` a `'readMeCopy.txt'`

#### Renombrar/mover archivos

Usar [`fs.rename`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_rename_oldpath_newpath_callback) para renombrar `'readMeCopy.txt'` a `'readMe_copy.txt'`

#### Crear directorios

- [`fs.mkdir`](https://nodejs.org/api/fs.html#fs_fs_mkdir_path_options_callback)
- [`fs.rmdir`](https://nodejs.org/api/fs.html#fs_fs_rmdir_path_options_callback)

```js
const fs = require('fs');

const { mkdir, rmdir, readFile: read, writeFile: write } = fs;

mkdir('node-fs', err => {
  if (err)
  read('readMe.txt', 'utf-8', (err, data) => {
    write('./node-fs/writeMe.txt', data)
  })
})
```

#### Crear directorio y mover un archivo

```js
const fs = require('fs');

const { mkdir, rename } = fs;

function move(src, dst) {
  rename(src, dst, err => {
    if (err) {
      throw err;
    }
  });
}

mkdir('node-fs', err => {
  if (err) {
    throw err;
  }

  move('readMe_copy.txt', 'node-fs/readMe_copy.txt');
});
```

#### Borrar directorios

:warning: Si intentamos borrar un directorio que no está vacío, se va a generar un error. Ver [Remove a directory that is not empty in NodeJS
](https://geedew.com/remove-a-directory-that-is-not-empty-in-nodejs/)

```js
const fs = require('fs');

const { rmdir } = fs;

rmdir('node-fs');
```

```js
const fs = require('fs');

const { rmdir, unlink: delete } = fs;

delete('./node-fs/writeMe.txt', () => {
  rmdir('node-fs');
})
```

#### Ejercicios

1. Usar [`prepend-file`](https://www.npmjs.com/package/prepend-file) para agregar el texto 

```js
`Con 15 peso', con 15 peso' me hago 

`
```

al principio del texto del archivo y mostrar el resultado en la consola

2. Escribir en el archivo `ticket.txt` el texto `Gastaste ${importe} en ${producto}!`, donde `importe` y `producto` son parámetros que se reciben por consola

3. Escribir la función `ls` que tome como parámetro un string que represente la ruta de un directorio local y loguee en consola los archivos del directorio. Investigar para esto el método [`fs.readdir](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_readdir_path_options_callback)

## Server

El módulo [`http`](https://nodejs.org/api/http.html) nos provee de la funcionalidad necesaria para crear servidores HTTP y realizar requests.

```js
// 3. server v1
const http = require("http");

// `createServer()` retorna un objeto, que tiene el método `listen`
const server = http.createServer();
server.listen(8888);
```

- Abrir `http://localhost:8888/` en el browser

```bash
// 4
node server.js
```

- Cuando el servidor recibe un _request_, se dispara el evento [`request`](https://nodejs.org/api/http.html#http_event_request) y se ejecuta el _callback_ que recibe `createServer`. Este callback tiene 2 parámetros, los objetos `request` y `response`

```js
// 5. server v2
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

#### Requests

Ver [Making HTTP requests with Node
](https://flaviocopes.com/node-make-http-requests/)

### Módulos 

Los [_módulos_](http://thenodeway.io/introduction/#build-small-single-purpose-modules) forman parte de los bloques fundamentales que utilizamos en Node para construir aplicaciones. Como buena práctica, vamos a tratar siempre en lo posible de construir **módulos pequeños, con un propósito único y claro**.

Los módulos en Node se importan utilizando la función `require()`. **Para ser cargado como módulo, un paquete debe contener un archivo `index.js` ó el campo `main` definido en el `package.json`, para indicar un _entry point_ específico**.

⚠️ La carga de los módulos se realiza de forma _sincrónica_ usando `require`, por lo que **siempre debemos cargarlos al inicio del archivo** si no queremos bloquear la aplicación

- Ver [Node JS Tutorial for Beginners #7 - Module Patterns](https://www.youtube.com/watch?v=9UaZtgB5tQI)

```js
// 7. server v3
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

#### Módulos vs. paquetes

Un archivo js importado con `require()` es un _módulo_ pero no un _paquete_, porque no tiene un archivo `package.json`.

Un _paquete_ es una carpeta/directorio que contiene lo siguiente:

- Un archivo [`package.json`](https://github.com/bpesquet/thejsway/blob/master/manuscript/chapter24.md#the-packagejson-file), que describe la aplicación y sus dependencias
- Un _entry point_ definido, que por default es el archivo `index.js`
- Un subdirectorio `node_modules`, que es la ubicación default donde Node va a buscar las dependencias del proyecto
- El resto de los archivos que forman parte del código fuente de la aplicación

#### Importar y exportar 

```js
// In `greetings.js`, create three functions
const sayHello = name => `Hello, ${name}`;
const flatter = () => `Look how gorgeous you are today!`;
const sayGoodbye = name => `Goodbye, ${name}`;

// Export two of them
module.exports = {
  sayHello,
  flatter
};

// Load the module "greetings.js"
const greetings = require("./greetings.js");

// Use exported functions
console.log(greetings.sayHello("Baptiste")); // "Hello, Baptiste"
console.log(greetings.flatter()); // "Look how gorgeous you are today!"
```

- Ver más ejemplos en [_The HTTP Module_ - Eloquent JavaScript](https://eloquentjavascript.net/20_node.html#h_3O5dGIJE9F)

### NPM

- Crear `package.json` usando el comando `npm init`
- ⚠️ Agregar `node_modules` al `.gitignore`
- ⚠️ Los archivos `package.json` y [`package-lock.json`](https://dev.to/saurabhdaware/but-what-the-hell-is-package-lock-json-b04) **deben comitearse SIEMPRE!**

#### NPM Cheatsheet

Instalar módulo como **dependencia** de nuestro proyecto

```bash
# son equivalentes
npm install <MODULE_NAME>
npm i <MODULE_NAME>
```

Instalar módulo como **dependencia de desarrollo** de nuestro proyecto

```bash
# son equivalentes
npm install --save-dev <MODULE_NAME>
npm i --save-dev <MODULE_NAME>
```

Instalar módulo de forma **global**

```bash
# son equivalentes
npm install --global <MODULE_NAME>
npm i -g <MODULE_NAME>
```

Actualizar módulo a la siguiente versión disponible, [según cómo tengamos declarada la dependencia en el `package.json`](https://docs.npmjs.com/about-semantic-versioning) (ver [_SEMVER_](https://semver.org))

```bash
npm update <MODULE_NAME>
```

Desinstalar una dependencia del proyecto

```bash
npm uninstall <MODULE_NAME>
```

Desinstalar un módulo global

```bash
npm uninstall -g <MODULE_NAME>
```

### NPX

[_NPX_](https://nodejs.dev/the-npx-nodejs-package-runner) sirve para ejecutar ciertos comandos (fundamentalmente relacionados a CLIs) sin tener la necesidad de descargar e instalar un módulo en nuestro proyecto

- [Using npx and npm scripts to Reduce the Burden of Developer Tools](https://dev.to/azure/using-npx-and-npm-scripts-to-reduce-the-burden-of-developer-tools-57f9)
- [npx vs npm - THE npx ADVANTAGE](https://dev.to/sarscode/npx-vs-npm-the-npx-advantage-1h0o)
