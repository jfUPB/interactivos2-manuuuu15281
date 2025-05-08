Fragmentos de la comunicación entre los códigos: 
-

**server.js**

```js
// Conexión de socket
io.on('connection', (socket) => {
  console.log('Cliente conectado');
  
  // Enviar el modo actual al cliente que se acaba de conectar
  socket.emit('mode_change', currentMode);
  
  // El cliente solicita el modo actual
  socket.on('get_mode', () => {
    socket.emit('current_mode', currentMode);
    console.log('Cliente solicitó el modo actual:', currentMode);
  });
  
  // Cambiar el modo (Medellín/Bogotá)
  socket.on('change_mode', (mode) => {
    if (mode === 'medellin' || mode === 'bogota') {
      currentMode = mode;
      console.log(`Modo cambiado a: ${mode}`);
      
      // Notificar a todos los clientes sobre el cambio de modo
      io.emit('mode_change', currentMode);
    }
  });
  
  let index = 0;
  
  // Enviar un dato GPS cada 2 segundos
  const interval = setInterval(() => {
    if (index < paces.length) {
      socket.emit('pace_update', { 
        pace: paces[index],
        velocity: velocidades[index]
      });
      index++;
    } else {
      // Reiniciar cuando acabe todos los datos
      index = 0;
      console.log("Reiniciando secuencia de datos GPS");
    }
  }, 2000);
  
  socket.on('disconnect', () => {
    console.log('Cliente desconectado');
    clearInterval(interval);
  });
});

// Iniciar el servidor
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Servidor escuchando en http://localhost:${PORT}`);
  console.log(`Control remoto disponible en http://localhost:${PORT}/remote`);
}
```

sketch.js

```js
function setup() {
  // Conectar al socket.io server
  socket = io();
  
  socket.on('connect', () => {
    connectionStatus = "Conectado";
    console.log("Conectado al servidor");
  });
  
  socket.on('disconnect', () => {
    connectionStatus = "Desconectado";
    console.log("Desconectado del servidor");
  });
  
  socket.on('pace_update', (data) => {
    targetPace = data.pace;
    currentVelocity = data.velocity;
    console.log(`Nuevo pace: ${targetPace.toFixed(2)} min/km, Velocidad: ${currentVelocity.toFixed(2)} m/s`);
  });
  
  // Nuevo: recibir cambios de modo (Medellín/Bogotá)
  socket.on('mode_change', (mode) => {
    currentMode = mode;
    console.log(`Modo cambiado a: ${currentMode}`);
  });
  
  // Nuevo: responder a solicitudes de modo actual
  socket.on('get_mode', () => {
    socket.emit('current_mode', currentMode);
  });

function draw() {
  // Mostrar indicador de modo actual (Medellín/Bogotá)
  fill(0, 0, 0, 200); // Fondo semi-transparente
  rect(width/2, gradientY + gradientHeight + 85, 150, 30, 8); // Rectángulo redondeado
  textSize(16);
  fill(255); // Texto blanco
  text(`Modo: ${currentMode === 'medellin' ? 'Medellín' : 'Bogotá'}`, width/2, gradientY + gradientHeight + 85);
}
```
![image](https://github.com/user-attachments/assets/205a7f02-319c-49ea-b0f5-3c2d05073dac)


Cuando se inicia la aplicación, el archivo sketch.js (cliente) establece una conexión con el servidor utilizando Socket.IO mediante la instrucción socket = io();. Una vez conectado, el cliente escucha varios eventos que el servidor puede emitir. Al conectarse, el servidor (server.js) imprime un mensaje de confirmación y le envía al cliente el modo actual de visualización (medellin o bogota) mediante el evento mode_change.

Luego, el servidor comienza a emitir cada 2 segundos datos de ritmo (pace) y velocidad (velocity) extraídos de sentencias NMEA simuladas. Estos datos se envían mediante el evento pace_update, que el cliente recibe y utiliza para actualizar variables internas (targetPace, currentVelocity) que afectan la visualización gráfica en pantalla. El cliente representa visualmente el ritmo con un color de fondo y una imagen de animal, ambos determinados por el valor del pace y del modo actual.

Además, cada vez que el servidor cambia el modo de visualización (por ejemplo, de Medellín a Bogotá), emite un evento mode_change, que el cliente escucha para ajustar las imágenes de animales correspondientes a ese modo
