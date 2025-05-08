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

La generación de los datos simulados la hice directamente desde el server.js: 

```js
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

// Texto NMEA proporcionado - normalmente leerías esto de un archivo
const nmeaText = `$GPRMC,132850.277,A,0612.408,N,07535.159,W,008.9,020.1,020525,000.0,W*65
$GPGGA,132851.277,0612.410,N,07535.158,W,1,12,1.0,0.0,M,0.0,M,,*7A
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30`;

// También podríamos leer este archivo desde disco:
// const nmeaText = fs.readFileSync(path.join(__dirname, 'gps_data.nmea'), 'utf8');

// Extraer los datos GPS del texto NMEA
const extractedGpsData = parseNMEACoordinates(nmeaText);

// Calcular velocidades en m/s
let velocidades = [];
for (let i = 0; i < extractedGpsData.length - 1; i++) {
  let p1 = extractedGpsData[i];
  let p2 = extractedGpsData[i + 1];

  let lat1 = convertToDecimal(p1.lat, p1.latDir);
  let lon1 = convertToDecimal(p1.lon, p1.lonDir);
  let lat2 = convertToDecimal(p2.lat, p2.latDir);
  let lon2 = convertToDecimal(p2.lon, p2.lonDir);

  let distancia = haversineDistance(lat1, lon1, lat2, lon2); // en metros
  let tiempo = 1; // segundos entre puntos (asumimos 1 segundo entre puntos NMEA)
  let velocidad = distancia / tiempo; // m/s

  velocidades.push(parseFloat(velocidad.toFixed(2)));
}

// Convertir velocidades de m/s a min/km (pace)
let paces = velocidades.map(v => {
  // v es velocidad en m/s
  // convertir a min/km: (1000/v)/60 = 16.67/v
  const minPerKm = v > 0 ? 16.67 / v : 10; // máximo 10 min/km si la velocidad es muy baja
  return Math.min(Math.max(minPerKm, 3), 10); // limitar entre 3 y 10 min/km
});

console.log("Servidor inicializado con", paces.length, "puntos GPS procesados de NMEA");

// En caso de que no haya suficientes datos NMEA, usamos los datos predeterminados
if (paces.length < 5) {
  console.warn("Pocos datos GPS extraídos, usando conjunto de datos predeterminado");
  
  // Lista de coordenadas simuladas como respaldo
  const defaultGpsData = [
    { lat: "0612.166", latDir: "N", lon: "07534.597", lonDir: "W" },
    { lat: "0612.167", latDir: "N", lon: "07534.598", lonDir: "W" },
    { lat: "0612.167", latDir: "N", lon: "07534.600", lonDir: "W" },
    // (mantenemos solo algunos datos para brevedad)
    { lat: "0612.179", latDir: "N", lon: "07534.626", lonDir: "W" },
    { lat: "0612.179", latDir: "N", lon: "07534.628", lonDir: "W" },
    { lat: "0612.180", latDir: "N", lon: "07534.629", lonDir: "W" }
  ];
  
  // Recalcular velocidades con los datos predeterminados
  velocidades = [];
  for (let i = 0; i < defaultGpsData.length - 1; i++) {
    let p1 = defaultGpsData[i];
    let p2 = defaultGpsData[i + 1];

    let lat1 = convertToDecimal(p1.lat, p1.latDir);
    let lon1 = convertToDecimal(p1.lon, p1.lonDir);
    let lat2 = convertToDecimal(p2.lat, p2.latDir);
    let lon2 = convertToDecimal(p2.lon, p2.lonDir);

    let distancia = haversineDistance(lat1, lon1, lat2, lon2);
    let tiempo = 1;
    let velocidad = distancia / tiempo;

    velocidades.push(parseFloat(velocidad.toFixed(2)));
  }
  
  // Recalcular paces
  paces = velocidades.map(v => {
    const minPerKm = v > 0 ? 16.67 / v : 10;
    return Math.min(Math.max(minPerKm, 3), 10);
  });
}

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
```

Mi desafio en esta parte del desarrollo fue lograr una transición fluida entre las imágenes y la misma barra de colores (las imágenes si no eran del mismo tamaño y estaban centrados de la misma manera, la transición se veía muy mal) tanto así que modifiqué muchas veces mis imágenes con el fin de lograr una transición más linda. Le pedí a la IA que me ayudara con la transición de los colores ya que esto se hacía directamente con el código y yo modifique las imágenes para que quedaran centradas y del mismo tamaño, lo hice con un editor de fotos.

Además, en la parte de la simulación de los datos el mayor problema que encontré era que los archivos .NMEA traían una gran cantidad de datos 
 y eran archivos muy pesados para que una IA los procesara y me diera un arreglo listo para colocarlo en el primer código que había planteado que calculaba las velocidades a partir de una arreglo ya procesado por la IA. Mi solución para esto fue crear un código que arreglara directamente los datos por si mismo y luego, a partir de estos calculara la velocidad en m/s, esto me funcionó muy bien y ahora puedo trazar rutas de largas distancias sin pensar que no podrán ser procesadas por contener muchos datos. 

Por último, mi mayor problema que quise solucionar era que la simulación del GPS no es organica ni representa a un corredor de manera natural, esto lo descubrí cuando vi que las velocidades que me generaba el simulador eran muy repetitivas y variaban entre un mismo rango de manera ciclica, la única solucíon que vi ante esto es que: Para lograr la naturalidad que yo quería la mejor manera de hacerlo es creando una app de GPS real que me genere los datos de verdad y poder ver el comportamiento de los visuales de una carrera no simulada. Pero también llegué a la conclusión de que dentro de una carrera, la mayoría de personas mantienen su velocidad constante y si llega a variar es en un rango no muy lejano a esa velocidad, lo que no me genera mucha variedad en los visuales. Esto decidí dejarlo así ya que es normal que estos datos no alcancen en ningún momento la naturalidad que quiero y es precisamente porque son simulaciones.
