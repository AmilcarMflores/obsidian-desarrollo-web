## ¿Por qué aprender React?
- **Alta demanda laboral:** Es una de las tecnologías más solicitadas por las empresas actualmente.
- **Ecosistema robusto:** Es la biblioteca de JavaScript más popular para el desarrollo de interfaces de usuario.
- **Curva de aprendizaje:** Es el paso lógico perfecto para quien ya domina JavaScript, potenciando sus capacidades.
## Potencialidades clave de React
React fue creado por ingenieros de Facebook (Meta) para solucionar problemas de **rendimiento y escalabilidad** en interfaces complejas y dinámicas (como el _feed_ de noticias), optimizando la experiencia de usuario.
### 1. Virtual DOM
Es el motor de rendimiento de React. El "DOM Virtual" es una copia ligera del DOM real que se mantiene en la memoria.

**¿Cómo funciona?** Cuando el estado de la aplicación cambia, React actualiza primero este DOM virtual. Mediante un algoritmo de "diferencia" (_diffing_), compara la nueva versión con la anterior, identifica qué cambió exactamente y actualiza **solo esos elementos** en el DOM real, evitando recargar toda la página.
### 2. Componentes
Son los bloques de construcción de React. Dividen la interfaz de usuario en piezas independientes, reutilizables y aisladas.

**Analogía:** Piensa en piezas de LEGO. Cada pieza (botón, menú, formulario) tiene su propia lógica y diseño, y al unirlas forman la aplicación completa.
### 3. Estados (State)
Es la memoria del componente. El "Estado" es un conjunto de datos dinámicos que determina cómo se comporta y se visualiza el componente en un momento dado.

**Funcionamiento:** A diferencia de una variable normal, cuando el **Estado** cambia, React detecta la modificación y vuelve a renderizar (pintar) automáticamente el componente para reflejar los nuevos datos.
### 4. SPA (Single Page Application)
React permite crear "Aplicaciones de Página Única". Esto significa que la aplicación carga un solo archivo HTML al inicio. A medida que el usuario navega, el contenido se actualiza dinámicamente sin necesidad de recargar la página completa, ofreciendo una experiencia fluida similar a una aplicación de escritorio.
### 5. React Native (Multiplataforma)
El conocimiento de React permite dar el salto a **React Native**, una tecnología que permite crear aplicaciones nativas reales para iOS y Android utilizando la misma sintaxis y lógica que en la web.
## Archivos Importantes
- **src/App.jsx:** Es el Componente Principal (o raíz). Generalmente, actúa como el contenedor donde estructuramos nuestra aplicación e importamos otros componentes. Aquí es donde solemos empezar a trabajar.
- **src/main.jsx (o index.js):** Es el punto de entrada de la aplicación. Aquí es donde React se "conecta" con el HTML (el DOM real) para inyectar todo el contenido.
## Sintaxis JSX
JSX es una extensión de la sintaxis de JavaScript que nos permite escribir código similar a HTML dentro de JavaScript.

- **Clases:** No se usa `class` (ya que es una palabra reservada en JS), se debe usar `className`.
- **Etiquetas cerradas:** Todas las etiquetas deben cerrarse. Si no tienen contenido, se cierran a sí mismas (ej: `<br />`, `<img />`).
- **Regla del Padre Único:** En el `return` de la función, siempre debe haber un solo elemento padre que envuelva a todo el contenido. Si no quieres agregar un `div` extra que ensucie tu HTML, puedes usar "Fragmentos": `<> ...contenido... </>`.
## ¿Dónde va el CSS?
React es flexible con los estilos. Las formas más comunes son:

1. **Estilos Globales:** En `/src/index.css` (afecta a toda la app).
2. **Estilos de Componente:** En `/src/App.css` (o el nombre de tu componente).
3. **Importación:** Para que funcionen, siempre debes importar el archivo CSS en la parte superior de tu componente JSX: `import './App.css'`
## ¿Dónde escribimos la lógica JavaScript?
El código JavaScript (variables, funciones, cálculos) se escribe **dentro de la función, pero ANTES del `return`**.

Para mostrar el valor de esas variables dentro del HTML (en el return), usamos llaves `{ }`. Por ejemplo: `<h1>Hola {nombreUsuario}</h1>`
## Añadir Imágenes
Si la imagen está dentro de la carpeta `src` (por ejemplo, en una carpeta `src/assets`), debemos importarla como si fuera un módulo de código.

1. **Importar:** `import miImagen from './assets/foto.png';`
2. **Usar:** `<img src={miImagen} alt="Descripción de la imagen" />`
