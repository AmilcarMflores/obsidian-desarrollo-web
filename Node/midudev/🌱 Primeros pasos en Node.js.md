==Node.js es un entorno que permite ejecutar código JavaScript fuera del navegador==. Puedes ejecutar en una computadora, servidor, incluso en una Raspberry Pi.

Está construido sobre V8, el mismo motor que usa Chrome, así que es rápido, eficiente y muy potente.
# Variables globales: ¿dónde vive mi código?

Cuando escribes código en el navegador, todo vive dentro de un objeto llamado `window`, pero en Node.js no existe `window` en su lugar existe un objeto llamado `global`.

En JavaScript moderno se introdujo `globalThis`, que es un "nombre universal" que puedes usarlo en el navegador y en Node.js.

Cosas como `console.log()`, `setTimeout()` o `Math.random()` en realidad son propiedades de `globalThis`, por eso funcionan en cualquier lado, sin necesidad de importarlas.
## Modularidad

Node.js te permite dividir tu código en módulos (por ejemplo, uno para sumar números, otro para leer archivos, otro para conectarse a una base de datos).

Hay dos formas principales de hacerlo:
### 1. CommonJS (la forma clásica)

- Exportas con `module.exports`
- Importas con `require()`
- Archivos terminan en `.js` o `.cjs`.

```javascript
// utils.js
function saludar(nombre) {
	return `¡Hola, ${nombre}!`;
}
module.exports = { saludar };
```
```javascript
// app.js
const { saludar } = require('./utils');
console.log(saludar('Ana')); // ¡Hola, Ana!
```
### 2. ES Modules (la forma moderna)

- Exportas con `export`
- Importas con `import`
- Archivos usan `.mjs` o debes poner `"type":"module"` en tu `package.json`.

```javascript
// utils.mjs
export function saludar(nombre) {
	return `¡Hola, ${nombre}!`;
}

// app.mjs
import { saludar } from './utils.mjs';
console.log(saludar('Luis')); // ¡Hola, Luis!
```
En ES Modules, debes escribir la extensión del archivo (`.mjs` o `.js` ), mientras en CommonJS, no.
### Módulos nativos

Node.js viene módulos incluidos como `fs` (para archivos), `os` (para información del sistema), `path` (para rutas), `http` (para servidores), y muchos más.

Y aunque puedes escribir `require('fs')`, la buena práctica hoy es usar el prefijo `node:`:
```javascript
const os = require('node:os');

console.log('Nombre del sistema operativo:', os.platform()); // Ej: 'darwin', 'win32', 'linux'
console.log('Arquitectura del sistema operativo:', os.arch()); // Ej: 'x64'
console.log('Memoria total:', os.totalmem() / 1024 / 1024 / 1024, 'GB'); // Convertido a GB
console.log('Tiempo de actividad del sistema:', os.uptime() / 3600, 'horas'); // Convertido a Horas (60 * 60)```
```
## Sistema de archivos y la magia de lo asíncrono

El módulo `fs` (File System) te permite leer, escribir, mover o borrar archivos.

En Node.js todo es asíncrono por defecto, pero vamos por pasos.
### Síncrono = bloqueante (como esperar en una fila)
```javascript
const fs = require('node:fs');
const contenido = fs.readFileSync('notas.txt', 'utf-8');
console.log(contenido); // Solo se imprime después de que termine la lectura
console.log('Fin'); // Esto va después
```
El programa se detiene hasta que el archivo se lea. Útil solo en scripts simples.
### Asíncrono = no bloqueante (como pedir un café y seguir trabajando)
```javascript
const fs = require('node:fs');

fs.readFile('notas.txt', 'utf-8', (error, contenido) => {
	if (error) throw error;
	console.log('Contenido:', contenido); // Esto llega "cuando esté listo"
});

console.log('Fin'); // ¡Esto se imprime PRIMERO!
```
El programa sigue corriendo mientras el archivo se lee en segundo plano. Ideal para servidores que atienden miles de usuarios.

> En proyectos, casi siempre usarás la versión asíncrona con promesas (o `async/await`), que es más legible:

```javascript
import { readFile } from 'node:fs/promises';

const contenido = await readFile('notas.txt', 'utf-8');
console.log(contenido);
```

### Callbacks, promesas y async/await: evolución de lo asíncrono

Node.js nació con callbacks (funciones que se ejecutan "cuando termine algo"). Pero con el tiempo, el código se volvía un "infierno de callbacks anidados".

Por eso hoy se prefiere:
- Promesas: encadenan operaciones asíncronas con `.then()`.
- async/await: hacen que el código asíncrono se lea como si fuera síncrono (mucho más limpio).

> - `await` solo funciona dentro de funciones marcadas con `async`.
> - Y solo está disponible en ES Modules (o en CommonJS  si  usas Babel o versiones muy recientes de Node.js con flags especiales).
> - En CommonJS puro, no puedes usar `await` en el nivel superior del archivo.
---
## Cuatro formas de manejas tareas en Node.js: sincrónica, callback, secuencial y paralela

### 1. Sincrónica - "Todo en orden, sin saltos"

- Bloquea el hilo hasta que termina.
- Simple, pero lento si hay operaciones pesadas (como leer archivos grandes).
- Útil solo en scripts de configuración o CLI simples.
```javascript
const fs = require('node:fs');

console.log('1. Leyendo archivo A...');
const a = fs.readFileSync('a.txt', 'utf-8');
console.log('Contenido A:', a);

console.log('2. Leyendo archivo B...');
const b = fs.readFileSync('b.txt', 'utf-8');
console.log('Contenido B:', b);
// → B no empieza hasta que A termine
```
### 2. Asíncrono con callback - "Avísame cuando esté listo"

- No bloquea el hilo.
- Pero si encadenas muchos, caes en el "callback hell" (pirámide del infierno).
```javascript
fs.readFile('a.txt', 'utf-8', (err, a) => {
  if (err) throw err;
  console.log('A:', a);
  
  fs.readFile('b.txt', 'utf-8', (err, b) => {
    if (err) throw err;
    console.log('B:', b);
    // ¿Y si ahora necesitas C? 😱
  });
});
```
Difícil de leer, depurar y mantener.
### 3. Asíncrono secuencial con `async/await` - "Haz esto, luego esto otro... pero sin bloquear"

- Usa promesas bajo el capó
- El código parece sincrónico, pero no lo es.
- Ideal cuando el segundo paso depende del primero.
```javascript
async function leerArchivosSecuencial() {
  try {
    const a = await fs.readFile('a.txt', 'utf-8');
    console.log('A:', a);
    
    const b = await fs.readFile('b.txt', 'utf-8');
    console.log('B:', b);
    // B espera a que A termine, pero el programa no se congela
  } catch (err) {
    console.error('Error:', err);
  }
}
```
Es legible, control de errores con  `try/catch` y sigue siendo no bloqueante.
### 4. Asíncrono en paralelo - "Haz todo al mismo tiempo"

- Cuando las tareas no dependen una de otra, hazlas juntas.
- Mucho más rápido.
```javascript
async function leerArchivosParalelo() {
  try {
    // Inician las dos lecturas AL MISMO TIEMPO
    const [a, b] = await Promise.all([
      fs.readFile('a.txt', 'utf-8'),
      fs.readFile('b.txt', 'utf-8')
    ]);
    
    console.log('A:', a);
    console.log('B:', b);
  } catch (err) {
    console.error('Error:', err);
  }
}
```
## Módulo nativo `path` 

Windows usa `\` y macOS/Linux `/` para separar carpetas, por ende si escribimos rutas como `"carpeta\\archivo.txt"` , el código romperá en otro sistema.

El `path` es un módulo nativo que adapta las rutas automáticamente al sistema operativo.
```javascript
const path = require('node:path');

// 1. Separador del sistema
console.log('Separador:', path.sep); // \ en Windows, / en macOS/Linux

// 2. Unir partes de una ruta (¡sin preocuparte por barras!)
const ruta = path.join('proyectos', 'node', 'app.js');
console.log('Ruta unida:', ruta); // proyectos/node/app.js (o con \ en Windows)

// 3. Nombre del archivo (con y sin extensión)
const archivo = '/documentos/contrato.pdf';
console.log('Base:', path.basename(archivo));        // contrato.pdf
console.log('Sin ext:', path.basename(archivo, '.pdf')); // contrato

// 4. Extensión
console.log('Extensión:', path.extname('foto.jpg')); // .jpg
```
Una buena práctica es que nunca escribas rutas a mano. Usa `path.join()` o `path.resolve()`.
### Ejemplo práctico: `ls` mejorado (listar archivos)

```javascript
// versión básica (06.ls.js)
const fs = require('node:fs/promises');

fs.readdir('.') // Lee el directorio actual
	.then(files => files.forEach(f => console.log(f)))
	.catch(err => console.error('Error:', err));
```

```javascript
// versión avanzada (08.ls-advanced.js)
const fs = require('node:fs/promises');

// Toma el primer argumento después del nombre del script
// Ej: node 08.ls-advanced.js documentos
const folder = process.argv[2] ?? '.'; // Si no hay argumento, usa '.'

fs.readdir(folder)
	.then(files => files.forEach(f => console.log(f)))
	. catch(err => console.error('No se pudo leer:', err.message));
```
> `process.argv` contiene:
> 	- `[0]`: ruta de Node.js
> 	- `[1]`: ruta del script
> 	- `[2]`, `[3]`, ... argumentos que tú pasas.

Ejemplo en terminal:
```bash
node 08.ls-advanced.js /home/usuario
```

### El objeto `process`: el puente entre tu código y el sistema

`process` es un objeto global que te da información y control sobre el entorno de ejecución.
```javascript
// 1. Argumentos de la línea de comandos
console.log('Argumentos:', process.argv.slice(2)); // Solo los tuyos

// 2. Directorio actual de trabajo
console.log('Trabajando en:', process.cwd());

// 3. Variables de entorno
console.log('Modo:', process.env.NODE_ENV); // 'development', 'production', etc.

// 4. Salir del programa (con código de error)
// process.exit(0); // éxito
// process.exit(1); // error

// 5. Escuchar cuando el proceso va a terminar
process.on('exit', () => {
	console.log('Limpiando recursos antes de salir...');
	// Cierra conexiones, guarda logs, etc.
});
```
Consejo: usa `process.env.NOCE_ENV` para cambiar comportamientos:
```javascript
if (process.env.NODE_ENV === 'production') {
	console.log = () => {}; // desactiva logs en producción
}
```

## NPM

NPM tiene dos caras:

1. **npm (el registro):** una gigantesca biblioteca pública en la nube con millones de paquetes (librerías) listas para usar.
2. **npm (la CLI):** una herramienta de línea de comandos que viene con Node.js y te permite instalar, gestionar y compartir esas librerías en tus proyectos.
### Empezar un proyecto: `npm init`

Cuando creas una app en Node.js, lo primero que haces es:
```bash
npm init
```
Este comando te hace preguntas (nombre, versión, descripción, autor, licencia..) y genera un archivo clave: `package.json`.

Este archivo es el "pasaporte" de tu proyecto. Contiene:
- Metadatos (nombre, versión, descripción)
- Dependencias: qué librerías necesita tu app
- Scripts personalizados (como `npm start`)
- Configuración de entorno

Si quieres saltarte, usa:
```bash
npm init -y
```
Crea un `package.json` con valores predeterminados.

### Instalar dependencias

Imagina que quieres usar una librería como `picocolors` para dar color a tus mensajes en la terminal:
```bash
npm install picocolors
```
Esto hace tres cosas:
1. Descarga `picocolors` (y sus propias dependencias) en una carpeta llamada `node_modules`.
2. Añade `picocolors` a tu `package.json` bajo `"dependencies"`.
3. Crea (o actualiza) un archivo `package-lock.json` que bloquea las versiones exactas para que todos los desarrolladores usen lo mismo.
#### Dos tipos de dependencias:

| Tipo       | Comando                 | ¿Cuándo usarlo?                                                        |
| ---------- | ----------------------- | ---------------------------------------------------------------------- |
| Producción | `npm install nombre`    | Tu app necesita esto para funcionar (ej: un framework, base de datos). |
| Desarrollo | `npm install nombre -D` | Solo lo usas mientras programas (ej: linter, test runner, bundler).    |
Ejemplo:
```bash
npm install express         # para el servidor -> producción
npm install eslint -D       # para revisar código -> desarrollo
```

## Creando tu primer servidor HTTP

Con el módulo `http`, puedes crear un servidor web real que responda a navegadores, Postman o apps móviles.

**Cómo funciona:**
```javascript
// 09.http.js
const http = require('node:http');
const { findAvailablePort } = require('./10.free-port.js');

// 1. Define el puerto (con variable de entorno o por defecto)
const desiredPort = process.env.PORT ?? 3000;

// 2. Crea el servidor
const server = http.createServer((req, res) => {
  console.log('¡Alguien visitó el servidor!');
  res.end('Hola mundo desde Node.js 🚀');
});

// 3. Encuentra un puerto libre y escucha
findAvailablePort(desiredPort).then(port => {
  server.listen(port, () => {
    console.log(`Servidor corriendo en: http://localhost:${port}`);
  });
});
```
El `console.log()` solo se ve en la terminal, nunca en el navegador. El navegador solo recibe lo que envíes con `res.end()`  o `res.write()`.
### ¿Por qué buscar un puerto libre?

A veces el puerto 3000 ya está en uso. En vez de que tu servidor falle, este código inteligente:
1. Intenta usar el puerto deseando (ej: 3000).
2. Si está ocupado (`EADDRINUSE`), pide al sistema un puerto libre (usando `port=0`).
3. Te dice en qué puerto terminó escuchando.

El truco está en `10.free-port.js`:
```javascript
// 10.free-port.js
const net = require('node:net');

function findAvailablePort(desiredPort) {
  return new Promise((resolve, reject) => {
    const server = net.createServer();
    
    server.listen(desiredPort, () => {
      const { port } = server.address();
      server.close(() => resolve(port)); // cierra y devuelve el puerto
    });

    server.on('error', (err) => {
      if (err.code === 'EADDRINUSE') {
        // Si el puerto está ocupado, pide uno libre (0 = "elige tú")
        findAvailablePort(0).then(resolve);
      } else {
        reject(err);
      }
    });
  });
}

module.exports = { findAvailablePort };
```
En sistemas Unix, pedir el puerto `0` le dice al sistema: "dame cualquier puerto libre". 
## Variables de entorno en Node.js

Las variables de entorno son valores que puedes inyectar en tu programa desde fuera, sin cambiar el código.

**¿Por qué usarlas?**
- No quemar datos sensibles en el código (contraseñas, claves API).
- Cambiar comportamiento según el entorno: desarrollo, pruebas, producción.
- Personalizar ejecución sin tocar el archivo fuente.

**Cómo usarlas en Node.js**
Todas las variables de entorno están en el objeto global `process.env`.

Ejemplo:
```javascript
const port = process.env.PORT ?? 3000;
const modo = process.env.NODE_ENV ?? 'development';
```

**Cómo definirlas:**
```bash
PORT=4000 NODE_ENV=production node 09.http.js
```

```powershell
$env:PORT=4000; $env:NODE_ENV="production"; node 09.http.js
```

```cmd
set PORT=4000 && set NODE_ENV=production && node 09.http.js
```
### **Archivos `.env` (opcional, pero muy común)**

Para no escribir variables cada vez, puedes usar un archivo `.env` (con ayuda de librerías como `dotenv`):
```env
# .env
PORT=5000
NODE_ENV=development
DB_PASSWORD=secreto123
```

Luego el código:
```javascript
require('dotenv').config(); // carga el .env en process.env
console.log(process.env.PORT); // 5000
```
Nunca subas `.env` a GitHub. Agrégalo a tu `.gitignore`.

