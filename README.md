> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)
> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc. 

> 👉 Si te resultó útil, **se agradece que lo compartas para que le llegue a más gente!**

# ![Notas sobre NodeJS (Server-Side JavaScript)](https://i.imgur.com/c6g1d9A.png)

## Notas relacionadas

### `backend`

- [**Express**](https://github.com/undefinedschool/notes-expressjs)
- [**APIs**](https://github.com/undefinedschool/notes-apis)
- [**Bases de Datos**](https://github.com/undefinedschool/notes-dbs)

### `async`

- [**Event Loop**](https://github.com/undefinedschool/notes-event-loop)
- [**Callbacks**](https://github.com/undefinedschool/notes-callbacks)

## Contenido

- [Instalación](https://github.com/undefinedschool/notes-nodejs#instalaci%C3%B3n)
- [¿Qué es NodeJS?](https://github.com/undefinedschool/notes-nodejs#qu%C3%A9-es-nodejs)
  - [V8](https://github.com/undefinedschool/notes-nodejs#v8)
  - [Operaciones I/O](https://github.com/undefinedschool/notes-nodejs#operaciones-io)
  - [_Single-thread_](https://github.com/undefinedschool/notes-nodejs#single-thread)
  - [_Non-blocking I/O_](https://github.com/undefinedschool/notes-nodejs#non-blocking-io)
  - [Entorno + API](https://github.com/undefinedschool/notes-nodejs#entorno--api)
- [REPL](https://github.com/undefinedschool/notes-nodejs#repl)
- [Node nos permite escribir código _asincrónico_](https://github.com/undefinedschool/notes-nodejs#node-nos-permite-escribir-c%C3%B3digo-asincr%C3%B3nico)
- [Leyendo argumentos](https://github.com/undefinedschool/notes-nodejs#leyendo-argumentos)
  - [Iterar argumentos](https://github.com/undefinedschool/notes-nodejs#iterar-argumentos)
- [File System](https://github.com/undefinedschool/notes-nodejs#file-system)
  - [Sync](https://github.com/undefinedschool/notes-nodejs#sync)
    - [Escribir un archivo](https://github.com/undefinedschool/notes-nodejs#escribir-un-archivo)
  - [Async](https://github.com/undefinedschool/notes-nodejs#async)
    - [Borrar archivos](https://github.com/undefinedschool/notes-nodejs#borrar-archivos)
    - [Copiar archivos](https://github.com/undefinedschool/notes-nodejs#copiar-archivos)
    - [Renombrar/mover archivos](https://github.com/undefinedschool/notes-nodejs#renombrarmover-archivos)
    - [Crear directorios](https://github.com/undefinedschool/notes-nodejs#crear-directorios)
    - [Crear directorio y mover un archivo](https://github.com/undefinedschool/notes-nodejs#crear-directorio-y-mover-un-archivo)
    - [Borrar directorios](https://github.com/undefinedschool/notes-nodejs#borrar-directorios)
    - [Ejercicios](https://github.com/undefinedschool/notes-nodejs#ejercicios)
- [Path](https://github.com/undefinedschool/notes-nodejs#path)
- [HTTP (Server y requests)](https://github.com/undefinedschool/notes-nodejs#http-server-y-requests)
  - [Haciendo requests con Node](https://github.com/undefinedschool/notes-nodejs#haciendo-requests-con-node)
  - [Haciendo requests con `node-fetch`](https://github.com/undefinedschool/notes-nodejs#haciendo-requests-con-node-fetch)
  - [Ejercicios](https://github.com/undefinedschool/notes-nodejs#ejercicios-1)
- [Módulos](https://github.com/undefinedschool/notes-nodejs#m%C3%B3dulos)
  - [Módulos vs. paquetes](https://github.com/undefinedschool/notes-nodejs#m%C3%B3dulos-vs-paquetes)
  - [Importar y exportar](https://github.com/undefinedschool/notes-nodejs#importar-y-exportar)
  - [NPM](https://github.com/undefinedschool/notes-nodejs#npm)
    - [NPM Cheatsheet](https://github.com/undefinedschool/notes-nodejs#npm-cheatsheet)
  - [NPX](https://github.com/undefinedschool/notes-nodejs#npx)


---

## Instalación

- **Windows:** ir a la [página oficial](https://nodejs.org/en/), descargar e instalar el binario de la versión **LTS**
- **Linux / OS X:** descargar e instalar el script [nvm](https://github.com/nvm-sh/nvm/) y luego instalar la versión **LTS** con el comando `nvm install --lts`

Para chequear que se haya instalado correctamente, correr el comando `node -v` en la consola (debe retornar una versión >= 12)

> **¿Qué es _LTS_?** Leer [LTS vs Current version](https://stackoverflow.com/questions/33661274/what-are-the-differences-between-long-term-support-lts-and-stable-versions-of)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

## ¿Qué es NodeJS?

_NodeJS_ ó _Node_ a secas, es principalmente un _entorno de ejecución_, es decir, nos brinda el _contexto_ necesario para poder ejecutar código JavaScript por fuera de un browser. 

Es _Open-Source_ y tiene soporte _multi-plataforma_.

**Recordemos que JavaScript sólo funciona en el browser de forma _nativa_ y que se trata de un lenguaje de _alto nivel_**. Esto significa que realizar tareas como **_networking_**, acceder a hardware de red (ej: acceder a la tarjeta de red) para poder _escuchar requests_ y responder, **_acceder al file system_** del sistema operativo que estemos usando, para leer archivos (ej: un documento HTML) y enviarlos, poder levantar y correr un _server_, comunicarnos con una _base de datos_, desarrollar una _API_, etc, **no son capacidades propias de un lenguaje como JavaScript**.

Es por esto que necesitamos un **intermediario**, algo así como una _API_ que nos permita, escribiendo el código JavaScript que ya conocemos, acceder a este tipo de funcionalidades, necesarias para el _backend_ de nuestra aplicación.

Vamos a llamar **_entorno de ejecución_** a todo lo que necesitamos para poder ejecutar nuestro código e interactuar con estas funcionalidades. **Node** es quien nos va a dar este _entorno_ y nos va a permitir ejecutar código JavaScript prácticamente en todos lados.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### V8

Uno de los componentes de **Node** es **[V8](https://v8project.blogspot.com.ar)**, un _engine_ de JavaScript que se encarga de _parsear_, _compilar_, _optimizar_, _interpretar_ y ejecutar nuestro código.

La computadora no entiende (y por lo tanto no puede ejecutar) JavaScript directamente. Un _engine_, como lo es V8, toma nuestro código JavaScript y lo convierte a algo que si entiende, lo que se conoce como _código máquina_ o _binario_.

![](https://i.imgur.com/a0o7qLc.png)

Aparte de **V8**, tenemos el lenguaje **C++**, el cual, a traves de ciertas librerias que vienen con **Node** (como [libuv](https://github.com/libuv/libuv)), nos permiten acceder a _funcionalidad de más bajo nivel_ como las mencionadas antes e interactucar directamente con el sistema operativo. Para acceder a esa funcionalidad, vamos a utilizar la [**_API_**](https://nodejs.org/dist/latest-v12.x/docs/api/) que nos provee Node.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Operaciones I/O

Las operaciones de I/O son aquellas que se producen cuando hay una comunicación entre una computadora y ciertos _periféricos_, como pueden ser una tarjeta de red ó un disco.

Estas operaciones pueden consistir en, por ejemplo, intercambiar información a través de una red (networking) realizando requests, escuchar requests en cierto puerto y generar una respuesta, acceder a una base de datos ó realizar diversas acciones con el _filesystem_, como crear, leer y escribir archivos, copiar y pegar archivos, creary eliminar directorios, etc.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### _Single-thread_

**Las aplicaciones desarrolladas en Node corren en un único proceso (aka _thread_)**. Es decir, no crean un proceso nuevo por cada _request_. Node nos provee de muchos métodos de I/O asincrónicos a través de su _API_ que previenen que el código se _bloquee_. La mayoría de las librerías, módulos y frameworks de Node estan escritas utilizando el paradigma asincrónico, por lo que encontrar _código bloqueante o sincrónico_ es más bien la excepción y no la norma.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### _Non-blocking I/O_

Cuando Node realiza alguna [operación de I/O](), en lugar de bloquear el único thread del que disponemos y desperdiciar _ciclos de CPU_ esperando, Node retomará las operaciones pendientes cuando la respuesta se encuentre disponible.

![](https://i.imgflip.com/3hdgov.jpg)

Esto le permite a Node por ejemplo,poder manejar tranquilamente miles de conexiones concurrentes con un único servidor levantado (recordemos que operamos con un único proceso), lo cual nos proporciona un gran ahorro de recursos, hardware, etc y evitamos tener que lidiar con el manejo de _threads_ para la concurrencia, simplificando en gran medida y agilizando el desarrollo.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Entorno + API

Por lo tanto, podríamos decir que **Node** termina siendo un _entorno de ejecución_ para poder correr código JavaScript y una _API_ (ó librería) que nos provee acceso a funcionalidades necesarias para el backend de nuestra aplicación.

Gracias a esta API, Node nos permite construir desde pequeñas aplicaciones de línea de comandos (_CLI_), hasta _servidores HTTP_ para crear sitios dinámicos y aplicaciones web.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

## Leyendo argumentos

Para leer argumentos a través de la terminal/CLI, podemos utilizar [`process.argv`](https://nodejs.org/docs/latest-v12.x/api/process.html#process_process_argv), que nos da acceso a un _Array_. Notar que este array también incluye como argumentos el comando que usamos para correr nuestro script y la ruta del archivo, por lo que los argumentos que nos interesan comienzan recién a partir del índice 2 del mismo, los cuales podemos obtener haciendo

```js
const args = process.argv.slice(2);
```

Para más info, ver [Node.js, accept arguments from the command line
](https://nodejs.dev/nodejs-accept-arguments-from-the-command-line)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Iterar argumentos

```js
process.argv.forEach((val, index) => {
  console.log(`${index}: ${val}`);
})
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

## File System

Podemos usar Node para leer y escribir archivos, a través del módulo `fs`

Ver [the File System module](https://eloquentjavascript.net/20_node.html#h_o2abiQU0TD)

- Crear un archivo `readMe.txt` (en el mismo directorio donde tengamos nuestro `index.js`) con el contenido de [este txt](https://gist.githubusercontent.com/nhsz/8442053a20604ede482a2f4c506f83f9/raw/c6b51faf561a04211dcdfafc562f4f423dd4062b/payload.txt)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Sync

- [`fs.readFileSync()`](https://nodejs.org/dist/latest-v12.x/docs/api/fs.html#fs_fs_readfilesync_path_options)

```js
const fs = require('fs');

const { readFileSync } = fs;
const txt = readFileSync('readMe.txt', 'utf-8');

console.log(txt);
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

#### Escribir un archivo

- [`fs.writeFileSync`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_writefilesync_file_data_options)

```js
const fs = require('fs');

const { readFileSync } = fs;
const readMe = readFileSync('readMe.txt', 'utf-8');
writeFileSync('writeMe.txt', readMe);

console.log(txt);
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

#### Copiar archivos

Usar [`fs.copyFile`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_copyfile_src_dest_flags_callback) para copiar `'readMe.txt'` a `'readMeCopy.txt'`

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

#### Renombrar/mover archivos

Usar [`fs.rename`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_rename_oldpath_newpath_callback) para renombrar `'readMeCopy.txt'` a `'readMe_copy.txt'`

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

const { rmdir, unlink: del } = fs;

del('./node-fs/writeMe.txt', err => {
  if (err) {
    throw err;
  }
 
  rmdir('node-fs');
})
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

#### Ejercicios

1. Usar [`prepend-file`](https://www.npmjs.com/package/prepend-file) para agregar el texto 

```js
`Con 15 peso', con 15 peso' me hago 

`
```

al principio del texto del archivo y mostrar el resultado en la consola

2. Escribir en el archivo `ticket.txt` el texto `Gastaste ${importe} en ${producto}!`, donde `importe` y `producto` son [parámetros que se reciben por consola](https://github.com/undefinedschool/notes-nodejs/blob/master/README.md#leyendo-argumentos)

3. Escribir la función `ls` que tome como parámetro por consola un string que represente la ruta de un directorio local y loguee en consola los archivos del directorio. Investigar para esto el método [`fs.readdir`](https://nodejs.org/docs/latest-v12.x/api/fs.html#fs_fs_readdir_path_options_callback)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

## Path

El módulo `path`de Node es muy útil para trabajar con y manipular rutas (_paths_) de diferentes maneras.

```js
const path = require("path");

// Normalizar una ruta
console.log(path.normalize("/test/test1//2slashes/1slash/tab/..")); // /test/test1/2slashes/1slash

// Unir múltiples paths
console.log(path.join("/first", "second", "something", "then", "..")); // /first/second/something

// Resolver un path (obtener la ruta absoluta de un archivo)
console.log(path.resolve("first.js"));

// Obtener la extensión de un archivo
console.log(path.extname("main.js")); // .js
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

## HTTP (Server y requests)

El módulo [`http`](https://nodejs.org/api/http.html) nos provee de la funcionalidad necesaria para crear servidores HTTP y realizar requests.

```js
// server v1
const http = require("http");

// `createServer()` crea un nuevo servidor HTTP y retorna un objeto, que tiene el método `listen`
const server = http.createServer();
server.listen(8888);
```

- Abrir `http://localhost:8888/` en el browser

```bash
node server.js
```

- Cuando el servidor recibe un _request_, se dispara el evento [`request`](https://nodejs.org/api/http.html#http_event_request) y se ejecuta el _callback_ que recibe `createServer`. Este callback tiene 2 parámetros, los objetos `request` y `response`

```js
// server v2
// en un archivo server.js
const http = require("http");

http.createServer((request, response) => {
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.write('Hello World');
  response.end();
}).listen(8888);
```

```bash
node server.js
```

```js
// 7. server v3 (refactoring)
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

```js
// server v4 (ejemplo de la documentación de Node)
const http = require('http');

const HOSTNAME = '127.0.0.1';
const PORT = process.env.PORT;

const server = http.createServer((request, response) => {
  response.statusCode = 200;
  response.setHeader('Content-Type', 'text/plain');
  response.end('Hello World!');
})

server.listen(port, hostname, () => {
  console.log(`Server running at http://${HOSTNAME}:${PORT}/`);
})
```

```bash
PORT=8888 node app.js
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Haciendo requests con Node

Ver [Making HTTP requests with Node
](https://flaviocopes.com/node-make-http-requests/)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Haciendo requests con `node-fetch`

Ver [`node-fetch`](https://www.npmjs.com/package/node-fetch)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Ejercicios

**Tip:** Usar [`nodemon`](https://www.npmjs.com/package/nodemon) y crear el script `dev: nodemon index.js` en el `package.json` para correrlos

1. Crear un servidor en Node, que escuche en el puerto `8001` (el puerto debe pasarse como parámetro a través de las variables de entorno) y responda con el siguiente HTML:

```html
<h1>Hey!</h1>
<p>Soy un servidor <code>Node</code> y vos estás haciendo un <code>{{METHOD}}</code> a la <em>url</em> <code>{{URL}}</code> 🎉</p>
```

donde `{{URL}}` es la _url_ a la cual el cliente hizo el request, ej: `localhost:8001/node` y `{{METHOD}}` es el _verbo HTTP_ utilizado, ej: `GET`. Para visualizar correctamente el HTML, tendremos que agregar el `charset` al `Content-Type` en los _headers_:

```js
'Content-Type': 'text/html; charset=utf-8'
```

2. Modificar el código del ejercicio anterior, para que si se hace un request a `/hello`, el servidor responda con el siguiente HTML:

```html
<h1>Hola! 😃</h1>
<p>Soy un servidor <code>Node</code> y vos estás haciendo un <code>{{METHOD}}</code> a la <em>url</em> <code>{{URL}}</code> 🎉</p>
```

y si el request se hace a `/bye`, la respuesta sea

```html
<h1>Bueno. 😢</h1>
<p>Soy un servidor <code>Node</code> y vos estás haciendo un <code>{{METHOD}}</code> a la <em>url</em> <code>{{URL}}</code> 🎉</p>
```

**Nota:** en ambos casos, lo único que cambia en la respuesta es el contenido del `h1` y si el request se hace a `/`, la respuesta debe ser la misma del primer ejercicio.

3. Modificar el código del ejercicio anterior, para que, si se realiza un request a la url `/christmas`, el servidor devuelva, en formato `JSON`, la cantidad de minutos que faltan entre la fecha actual y el 25 de Diciembre, 0hs. Usar [date-fns](https://date-fns.org/v1.29.0/docs/differenceInMinutes) para realizar este cálculo. (**Nota:** para importar el módulo correspondiente, usar `const differenceInMinutes = require('date-fns/differenceInMinutes')`, en la documentación está mal)

4. Modificar el código del ejercicio 1, para tener el servidor en un archivo `server.js`, que exporte la función `up`, la cual sirve para iniciar el servidor en el puerto `8888`. Esta función debe loguear mensajes por consola indicando cuando el servidor está levantado y cuando recibe un nuevo request. El _callback_ que recibe `createServer` debe modularizarse y moverse a la función `onRequest`. Por último, Crear el archivo `index.js`, en el cual vamos a importar el server y utilizar la función `up` para correrlo.

5. Crear un archivo `index.html` con el siguiente contenido

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Server</title>
    <link href="https://fonts.googleapis.com/css?family=Molle:400i&display=swap" rel="stylesheet" />
    <style>
      body {
        font-family: 'Molle', cursive;
        color: #026e00;
        padding: 24px;
      }
      h1 {
        font-size: 5em;
      }
      p {
        font-size: 2em;
      }
      h1,
      p {
        text-align: center;
      }
    </style>
  </head>
  <body>
    <h1>Hey!</h1>
    <p>Soy un servidor <code>Node</code> y estás viendo el <code>index.html</code> que leí y te estoy mandando 🎉</p>
  </body>
</html>
```

Luego, modificar el código del ítem anterior, para que como respuesta envíe el resultado de leer el contenido del archivo HTML creado.

6. Modificar el `html` del ejercicio anterior para incluir [esta imagen](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Node.js_logo.svg/1180px-Node.js_logo.svg.png) con el nombre `node.png` en el mismo. La imagen debe estar en el proyecto, dentro de una carpeta `assets`. En el caso de que el servidor reciba un request a una ruta no definida, debe responder con un status `404` y el html `<h2>404 - Page not found :(</h2>`

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

## Módulos 

Los [_módulos_](http://thenodeway.io/introduction/#build-small-single-purpose-modules) forman parte de los bloques fundamentales que utilizamos en Node para construir aplicaciones. Como buena práctica, vamos a tratar siempre en lo posible de construir **módulos pequeños, con un propósito único y claro**.

Los módulos en Node se importan utilizando la función `require()`. **Para ser cargado como módulo, un paquete debe contener un archivo `index.js` ó el campo `main` definido en el `package.json`, para indicar un _entry point_ específico**.

⚠️ La carga de los módulos se realiza de forma _sincrónica_ usando `require`, por lo que **siempre debemos cargarlos al inicio del archivo** si no queremos bloquear la aplicación

- Ver [Node JS Tutorial for Beginners #7 - Module Patterns](https://www.youtube.com/watch?v=9UaZtgB5tQI)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Módulos vs. paquetes

Un archivo js importado con `require()` es un _módulo_ pero no un _paquete_, porque no tiene un archivo `package.json`.

Un _paquete_ es una carpeta/directorio que contiene lo siguiente:

- Un archivo [`package.json`](https://github.com/bpesquet/thejsway/blob/master/manuscript/chapter24.md#the-packagejson-file), que describe la aplicación y sus dependencias
- Un _entry point_ definido, que por default es el archivo `index.js`
- Un subdirectorio `node_modules`, que es la ubicación default donde Node va a buscar las dependencias del proyecto
- El resto de los archivos que forman parte del código fuente de la aplicación

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### Importar y exportar 

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### NPM

- Crear `package.json` usando el comando `npm init`
- ⚠️ Agregar `node_modules` al `.gitignore`
- ⚠️ Los archivos `package.json` y [`package-lock.json`](https://dev.to/saurabhdaware/but-what-the-hell-is-package-lock-json-b04) **deben comitearse SIEMPRE!**

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)

### NPX

[_NPX_](https://nodejs.dev/the-npx-nodejs-package-runner) sirve para ejecutar ciertos comandos (fundamentalmente relacionados a CLIs) sin tener la necesidad de descargar e instalar un módulo en nuestro proyecto

- [Using npx and npm scripts to Reduce the Burden of Developer Tools](https://dev.to/azure/using-npx-and-npm-scripts-to-reduce-the-burden-of-developer-tools-57f9)
- [npx vs npm - THE npx ADVANTAGE](https://dev.to/sarscode/npx-vs-npm-the-npx-advantage-1h0o)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-nodejs#contenido)
