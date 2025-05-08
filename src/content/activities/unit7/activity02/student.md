Fragmentos de código clave del sketch.js
-
La lógica del proceso se encuentra principalmente en el **draw()**, donde ocurre la actualización continua del estado del sistema (el pace y la velocidad) y su representación visual:

```js
function draw() {
  background(0);
  
  // Dibuja el cuadrado principal grande primero
  let squareSize = 300;
  let squareX = width / 2;
  let squareY = height / 2 - 50;
  
  // Obtener el color actual basado en el ritmo
  let currentColor = getColorForPace(currentPace);
  
  // Dibujar el cuadrado grande con el color actual
  noStroke();
  fill(currentColor);
  rectMode(CENTER);
  rect(squareX, squareY, squareSize, squareSize);
  
  // Dibujar las imágenes de animales con transición según el modo actual
  drawAnimalWithTransition(squareX, squareY, squareSize * 0.8, currentPace);
  
  // Dibujar el gradiente pequeño debajo del cuadrado
  let gradientHeight = 30;
  let gradientY = squareY + (squareSize / 2) + 30;
  drawGradientBackground(gradientY, gradientHeight);
  
  // Línea indicadora del pace actual en el gradiente
  let x = map(currentPace, 3, 10, 0, width);
  stroke(255);
  strokeWeight(5);
  line(x, gradientY - 10, x, gradientY + gradientHeight + 10);
  
  // Mostrar texto de velocidad con mejor visibilidad
  // Primero dibujamos un fondo para el texto
  fill(0, 0, 0, 200); // Fondo semi-transparente
  noStroke();
  rectMode(CENTER);
  rect(width/2, gradientY + gradientHeight + 40, 250, 40, 10); // Rectángulo redondeado
  
  // Luego dibujamos el texto con borde
  textSize(22);
  textAlign(CENTER, CENTER);
  textStyle(BOLD);
  fill(255); // Texto blanco
  text(`Velocidad: ${nf(currentVelocity, 1, 2)} m/s`, width/2, gradientY + gradientHeight + 40);

  // Mostrar indicador de modo actual (Medellín/Bogotá)
  fill(0, 0, 0, 200); // Fondo semi-transparente
  rect(width/2, gradientY + gradientHeight + 85, 150, 30, 8); // Rectángulo redondeado
  textSize(16);
  fill(255); // Texto blanco
  text(`Modo: ${currentMode === 'medellin' ? 'Medellín' : 'Bogotá'}`, width/2, gradientY + gradientHeight + 85);

  // Guardar histórico (para promedio futuro)
  paceHistory.push(currentPace);
  if (paceHistory.length > 1000) paceHistory.shift();

  // Transición suave
  currentPace = lerp(currentPace, targetPace, 0.05);
}

function drawGradientBackground(y, height) {
  for (let x = 0; x < width; x++) {
    let pace = map(x, 0, width, 3, 10);
    let c = getColorForPace(pace);
    stroke(c);
    line(x, y, x, y + height);
  }
}
```
La conexión se establece usando Socket.IO dentro del setup(), aquí se establece y se mantiene conexión con el proceso, recibir datos clave (pace, velocity, y mode) y responder a solicitudes del servidor (modo actual). Dentro del setup() incluí los console.log y verifique que funcionaran correctamente: 

```js
function setup() {
  createCanvas(700, 600);
  ...

  // Conexión al servidor vía socket
  socket = io();

  socket.on('connect', () => {
    connectionStatus = "Conectado";
    console.log("Conectado al servidor");
  });

  socket.on('disconnect', () => {
    connectionStatus = "Desconectado";
    console.log("Desconectado del servidor");
  });

  // Recibe actualizaciones de ritmo y velocidad desde el proceso
  socket.on('pace_update', (data) => {
    targetPace = data.pace;
    currentVelocity = data.velocity;
    console.log(`Nuevo pace: ${targetPace.toFixed(2)} min/km, Velocidad: ${currentVelocity.toFixed(2)} m/s`);
  });

  // Cambio de modo (ciudad) desde el proceso
  socket.on('mode_change', (mode) => {
    currentMode = mode;
    console.log(`Modo cambiado a: ${currentMode}`);
  });

  // Solicitud desde el servidor para conocer el modo actual
  socket.on('get_mode', () => {
    socket.emit('current_mode', currentMode);
  });
}
```
![image](https://github.com/user-attachments/assets/1ac41cfd-ec25-4647-8cd4-9caa71be8543)

Mi desafio en esta parte del desarrollo fue lograr una transición fluida entre las imágenes y la misma barra de colores (las imágenes si no eran del mismo tamaño y estaban centrados de la misma manera, la transición se veía muy mal) tanto así que modifiqué muchas veces mis imágenes con el fin de lograr una transición más linda. Le pedí a la IA que me ayudara con la transición de los colores ya que esto se hacía directamente con el código y yo modifique las imágenes para que quedaran centradas y del mismo tamaño, lo hice con un editor de fotos.
