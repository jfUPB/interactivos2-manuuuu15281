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

Antes de que los navegadores se conecten, necesitan intercambiar la info de cómo encontrarse. Esto se hace con ayuda de un servidor de señalización, que permite a los usuarios intercambiar datos como una dirección IP, una codificación de audio o video e información de red. 

En mi código se inicializa p5LiveMedia para enviar el contenido del canvas como un stream de datos WebRTC. Donde:

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
**¿Qué es un peer connection?**

Peer Connection es un componente de WebRTC que permite la comunicación directa entre dos navegadores para intercambiar audio, video o datos en tiempo real.
En el código que hice en la actividad anterior, la conexión P2P entre los navegadores se maneja con p5LiveMedia, la cual usa RTCPeerConnection internamente, lo que nos permite compartir el canvas en tiempo real.

```
let p5l = new p5LiveMedia(this, "CANVAS", myCanvas, "party");
p5l.on('stream', gotStream);
```
En este código, p5LiveMedia crea una Peer Connection automáticamente, "CANVAS" indica que queremos compartir un lienzo en vez de video/audio y "party" es el nombre de la sala donde se conectan los usuarios.

**¿Qué es un data channel?**

Un Data Channel es un mecanismo que permite enviar y recibir datos como texto, JSON y coordenadas  en tiempo real entre pares de una conexión WebRTC.

Mientras que WebRTC es conocido por la transmisión de audio y video, el Data Channel permite el intercambio de otros tipos de información, como mensajes, posiciones de objetos en un juego, eventos de interacción, etc.

En la aplicación que hice, el Data Channel está gestionado por p5LiveMedia para enviar el dibujo a los demás usuarios en la sala en tiempo realy aunque no estamos accediendo directamente a un Data Channel, p5LiveMedia internamente lo usa para enviar información del canvas en tiempo real. Esto significa que cuando un usuario dibuja algo, los datos de las coordenadas del dibujo se envían a los otros clientes a través del Data Channel, lo que permite que todos vean los trazos en tiempo real.

**¿Qué es un media stream?**

Un Media Stream en WebRTC es un objeto que representa una secuencia de audio y/o video capturada con una cámara web, un micrófono o incluso una pantalla compartida. Este concepto se basa en la API MediaStream, que permite a los navegadores capturar video o audio desde dispositivos locales, enviar estas transmisiones a otros usuarios a través de WebRTC y mostrar la transmisión en una propia página web.

En la aplicación, el Media Stream se usa para capturar y transmitir el video de la cámara a través de WebRTC con p5LiveMedia. Cada usuario captura su propio video con createCapture(VIDEO), luego p5LiveMedia usa WebRTC para compartir este video con otros usuarios en la misma sala y cuando un usuario nuevo se conecta, recibe los Media Streams de los demás permitiendo a todos poder ver el video de los otros en tiempo real.

**¿Qué es un ICE server?**

Un ICE Server (Interactive Connectivity Establishment Server) es un servidor que ayuda a los dispositivos a descubrir cómo conectarse entre sí en una red. sus Funciones clave son: 
- Facilita la comunicación entre usuarios en redes diferentes.
- Encuentra la mejor ruta para la conexión WebRTC.
- Puede actuar como STUN (descubre direcciones IP públicas) o TURN (retransmite datos si no hay conexión directa).

En mi aplicación, los ICE Servers están implementados de manera interna en p5LiveMedia, que usa WebRTC para gestionar la conexión entre los participantes.
Cuando creamos una conexión WebRTC con p5LiveMedia, internamente se usa ICE para descubrir la dirección IP pública de cada usuario (usando un servidor STUN), establecer una conexión entre los pares si es posible  y reenviar datos a través de un servidor TURN si los firewalls o NAT bloquean la conexión directa.

**¿Qué es un STUN server?**

Un STUN (Session Traversal Utilities for NAT) Server es un servidor que ayuda a los dispositivos a descubrir su dirección IP pública y el tipo de NAT (Network Address Translation) bajo el que están operando.

**¿Qué es un TURN server?**

Un TURN (Traversal Using Relays around NAT) Server es un servidor que retransmite datos entre dos pares cuando no pueden establecer una conexión directa debido a firewalls o configuraciones de NAT restrictivas. Sus funciones principales son:

- Actúa como intermediario cuando no se puede hacer una conexión directa P2P.
- Reenvía video, audio o datos entre los usuarios.
- Se usa solo cuando STUN y las conexiones directas fallan, ya que aumenta la latencia.

**¿Qué es un signaling server?**

Un Signaling Server es un servidor que ayuda a los peers a encontrarse e intercambiar información inicial para establecer una conexión WebRTC. Sus funciones clave son facilitar el intercambio de información necesaria para iniciar la conexión WebRTC. Además, no transmite el video, audio ni datos, solo ayuda a que los pares se conecten y usa protocolos como WebSockets, HTTP, o WebRTC DataChannels para comunicarse.
En nuestro código, p5LiveMedia automáticamente se conecta a un Signaling Server en la nube, este servidor envía y recibe información de conexión entre los usuarios y una vez que la conexión WebRTC se establece, el Signaling Server ya no es necesario.
