WebRTC
-
**¿Qué es webRTC?**

Es una tecnología que permite la comunicación en tiempo real de audio, video y datos directamente entre navegadores sin usar servidores de intermediarios. Se utiliza para videollamadas, transmisión de audio o video y transferencia de datos. 
En la actividad anterior, usé la librería p5LiveMedia, que facilita la implementación del WebRTC en p5.js. 

```
let p5l = new p5LiveMedia(this, "CANVAS", myCanvas, "party");
p5l.on('stream', gotStream);

```
Aquí se establece la conexión WebRTC para compartir el lienzo (canvas) en tiempo real con otras personas que abran la misma aplicación.

**¿Cómo funciona webRTC?**

Se basa en comunicación peer-to-peer(P2P) y utiliza varios protocolos para manejar conexiones seguras y de baja latencia. 
Para que WebRTC funcione debe seguir los siguientes tres pasos: 

**1. Signaling:** Antes de que los navegadores se conecten, necesitan intercambiar la info de cómo encontrarse. Esto se hace con ayuda de un servidor de señalización, que permite a los usuarios intercambiar datos como una dirección IP, una codificación de audio o video e información de red. 

Enmi código se inicializa p5LiveMedia para enviar el contenido del canvas como un stream de datos WebRTC. Donde:

- "CANVAS": Indica que queremos compartir el lienzo (canvas) en lugar de un video o audio.
- myCanvas: Es el elemento a compartir.
- "party": Es la "sala" de WebRTC, permitiendo que los usuarios que usen este código se conecten automáticamente entre sí.

Cada vez que un usuario se conecta y transmite su lienzo, el evento 'stream' se activa y ejecuta gotStream(stream).

```
p5l.on('stream', gotStream);
```
Y cuando se recibe un nuevo stream, cualquier usuario que tenga esta aplicación abierta recibirá el lienzo de otro usuario en la misma sala.

```
function gotStream(stream) {
    otherVideo = stream;
}
```
