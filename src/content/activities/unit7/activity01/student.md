ETAPA FINAL DEL PROYECTO DE CURSO
-
Para esta fase, la estructura de carpetas que tomé fue la siguiente: 

![image](https://github.com/user-attachments/assets/41906b18-4bcd-4395-910a-1cdc65db2408)

Dentro de la carpeta del proyecto "visualizador_corredor" tenemos la caperta "node_modules" donde están dependencias y otras cosas que necesita Node.js para funcionar. Además, tengo los archivos index.html, sketch.js, remote.html y las imágenes que necesito para mi proyecto fuera de la carpeta public porque no me funcionó, también está el server.js.

**Código inicial del index.html**

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Visualizador de Pace Running</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.6.1/socket.io.min.js"></script>
  <style>
    body {
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #111;
      color: white;
      font-family: Arial, sans-serif;
    }
    canvas {
      border: 2px solid #333;
    }
    .info {
      position: absolute;
      bottom: 10px;
      text-align: center;
      width: 100%;
    }
  </style>
</head>
```

**Código inicial del remote.html (código  del control remoto)**

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Control Remoto - Visualizador</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.6.1/socket.io.min.js"></script>
  <style>
    body {
      margin: 0;
      padding: 20px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background-color: #111;
      color: white;
      font-family: Arial, sans-serif;
    }
    h1 {
      margin-bottom: 30px;
      text-align: center;
    }
    .buttons {
      display: flex;
      flex-direction: column;
      gap: 20px;
      width: 100%;
      max-width: 300px;
    }
    .btn {
      padding: 20px;
      font-size: 1.2rem;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      width: 100%;
      text-transform: uppercase;
      font-weight: bold;
      transition: all 0.3s ease;
    }
    .btn-medellin {
      background-color: #228B22;
      color: white;
    }
    .btn-bogota {
      background-color: #4B0082;
      color: white;
    }
    .btn:hover {
      opacity: 0.8;
      transform: scale(1.05);
    }
    .btn:active {
      transform: scale(0.95);
    }
    .active {
      box-shadow: 0 0 15px rgba(255, 255, 255, 0.7);
      position: relative;
    }
    .active::after {
      content: "✓";
      position: absolute;
      right: 20px;
    }
    .status {
      margin-top: 30px;
      padding: 10px;
      text-align: center;
      font-size: 0.9rem;
      color: #aaa;
    }
  </style>
</head>
```

**Código inicial del sketch.js**

```js
function setup() {
  createCanvas(700, 600);
  frameRate(30);
  noStroke();

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

  // Define los colores del gradiente (de izquierda a derecha) - INVERTIDO
  paceColors = [
    { pace: 3, color: color('#8B0000') },   // rojo oscuro (Halcón) - ahora para pace rápido
    { pace: 4, color: color('#FF4500') },   // naranja rojizo
    { pace: 5, color: color('#FFFF00') },   // amarillo (Cheeta)
    { pace: 6, color: color('#7CFC00') },   // verde brillante (Lobo)
    { pace: 7.5, color: color('#32CD32') }, // verde lima (Saltamontes)
    { pace: 9, color: color('#228B22') },   // verde bosque
    { pace: 10, color: color('#4B0082') }   // violeta oscuro (Tortuga) - ahora para pace lento
  ];
}

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

```
**Código inicial del package.json**

```json
{
  "name": "visualizador_corredor",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "express": "^5.1.0",
    "socket.io": "^4.8.1"
  }
}
```

**Código inicial del server.js**

```
// Ruta para el control remoto
app.get('/remote', (req, res) => {
  res.sendFile(path.join(__dirname, 'remote.html'));
});

// Estado global para el modo de visualización (Medellín/Bogotá)
let currentMode = 'medellin'; // Valor predeterminado: Medellín

// Función para extraer las coordenadas de las sentencias NMEA
function parseNMEACoordinates(nmeaData) {
  // Dividir el texto en líneas
  const lines = nmeaData.trim().split('\n');
  
  // Array para almacenar los datos GPS extraídos
  const gpsData = [];
  
  // Para cada línea, procesamos solo las sentencias GPGGA que contienen coordenadas
  for (const line of lines) {
    if (line.startsWith('$GPGGA')) {
      const parts = line.split(',');
      
      // Verificar que tenemos suficientes partes y datos válidos
      if (parts.length >= 6) {
        // Extraer latitud, dirección de latitud, longitud, dirección de longitud
        const lat = parts[2];
        const latDir = parts[3];
        const lon = parts[4];
        const lonDir = parts[5];
        
        // Asegurarse de que no estamos añadiendo coordenadas vacías
        if (lat && latDir && lon && lonDir) {
          gpsData.push({
            lat: lat,
            latDir: latDir,
            lon: lon,
            lonDir: lonDir
          });
        }
      }
    }
  }
  
  return gpsData;
}

// Función para convertir coordenadas GPS en formato "0612.240,N" a grados decimales
function convertToDecimal(degreeMinute, direction) {
  let degrees = parseInt(degreeMinute.slice(0, degreeMinute.indexOf('.') - 2));
  let minutes = parseFloat(degreeMinute.slice(degreeMinute.indexOf('.') - 2));
  let decimal = degrees + minutes / 60;
  if (direction === 'S' || direction === 'W') decimal *= -1;
  return decimal;
}

// Haversine para distancia en metros entre dos puntos geográficos
function haversineDistance(lat1, lon1, lat2, lon2) {
  const R = 6371000; // Radio de la Tierra en metros
  const toRad = angle => angle * Math.PI / 180;

  let dLat = toRad(lat2 - lat1);
  let dLon = toRad(lon2 - lon1);
  let a = Math.sin(dLat / 2) ** 2 +
          Math.cos(toRad(lat1)) * Math.cos(toRad(lat2)) *
          Math.sin(dLon / 2) ** 2;
  let c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
  return R * c;
}
```

El index.html si funciona y el servidor  arranca con normalidad.

![image](https://github.com/user-attachments/assets/1a18c2ab-f8f5-4275-bc58-a3218538853a)


