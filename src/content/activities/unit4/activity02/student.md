p5LiveAction
-
**¿Qué es p5LiveMedia?**

Después de ver el video del repositorio, entendí que p5LiveMedia es una extensión de p5 que permite transmitir audio y video en tiempo real desde el computador y a la vez permite la sincronización de los medios (audio y video).

**¿Qué posibilidades ofrece p5LiveMedia?**

Esta biblioteca permite capturar video en tiempo real desde la camara del computador e incorporarlo en un canvas de p5.js, también permite capturar audio y manipular los datos del sonido para generar visualizaciones si así lo queremos. La sincronización de varios medios en tiempo real como audio y video es otra de las posibilidades que ofrece la libreria, finalmente sirve como soporte WebRTC y WebSocket permitiendo la conexión entre servidores y navegadores facilitando la transmición en vivo.

**Ejemplos analizados**

https://editor.p5js.org/shawn/sketches/jZQ64AMJc

https://editor.p5js.org/shawn/sketches/e4LTqKI8Q

**Análisis de los ejemplos**

Después de ver estos ejemplos pude entender que: 

1. En la función setup, se crea una nueva instancia de p5LiveMedia para manejar la conexión WebRTC. Se pasa el lienzo (myCanvas), el tipo de medio ("CANVAS") y un código único ("e4LTqKI8Q").

El evento on('stream', gotStream) se activa cuando llega una nueva interacción de otro usuario y esto llama a la función gotStream.

```
let otherCanvas;

function setup() {
  let myCanvas = createCanvas(400, 400);
  let p5l = new p5LiveMedia(this, "CANVAS", myCanvas, "e4LTqKI8Q");
  p5l.on('stream', gotStream);
}
```
2. La variable otherCanvas se asigna a la nueva interacción recibida, y se menciona en los comentarios que el otherCanvas.id contiene un código único para el usuario conectado. También que la interacción podría ocultarse con otherCanvas.hide().

```
function gotStream(stream) {
  // This is just like a video/stream from createCapture(VIDEO)
  otherCanvas = stream;
  //otherCanvas.id is the unique identifier for this peer
  //otherCanvas.hide();
}
```

Además, es importante la  configuración del archivo  HTML del proyecto que permite cargar las bibliotecas necesarias para que el código funcione correctamente con la librería de p5LiveMedia y WebRTC.

**1.** 

```
 <script type="text/javascript" src="https://p5livemedia.itp.io/simplepeer.min.js"></script>
```
Es una librería llamada SimplePeer, que facilita el uso de WebRTC (WebRTC permite la transmisión de audio, video o datos entre navegadores sin necesidad de un servidor intermedio).

**2.**

```
<script type="text/javascript" src="https://p5livemedia.itp.io/socket.io.js"></script>
```
Este archivo es la librería Socket.io, que se utiliza para la comunicación en tiempo real entre el servidor y el cliente usando WebSockets.

**3.**

```
<script type="text/javascript" src="https://p5livemedia.itp.io/p5livemedia.js"></script>
```
Este es el archivo principal de la librería p5LiveMedia, que está diseñada para integrar las funcionalidades de WebRTC con p5.js.
