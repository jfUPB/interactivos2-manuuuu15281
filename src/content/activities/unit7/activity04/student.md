PROTOTIPO EN ACCIÓN
-
En esta parte del **sketch.js** es donde se muestra cómo los inputs (velocidades simuladas) modifican los visuales. Este bloque recibe los valores enviados por el servidor y actualiza dos variables clave: targetPace y currentVelocity

```js
socket.on('pace_update', (data) => {
  targetPace = data.pace;
  currentVelocity = data.velocity;
  console.log(`Nuevo pace: ${targetPace.toFixed(2)} min/km, Velocidad: ${currentVelocity.toFixed(2)} m/s`);
});
```

```js
let currentColor = getColorForPace(currentPace);
fill(currentColor);
rect(squareX, squareY, squareSize, squareSize);


drawAnimalWithTransition(squareX, squareY, squareSize * 0.8, currentPace);


text(`Velocidad: ${nf(currentVelocity, 1, 2)} m/s`, width/2, gradientY + gradientHeight + 40);


currentPace = lerp(currentPace, targetPace, 0.05);

```

[Acceso al video del prototipo funcionando](https://youtu.be/cIaujlVPEao)

**Método IPO en mi código**

Los datos de entrada provienen del servidor, que simula movimiento a partir de coordenadas GPS en formato NMEA. Luego, el sketch.js procesa las entradas para calcular y actualizar el estado visual y por último las salidas son visualizaciones dinámicas (cambio de color en el fondo y la imagen) en la pantalla. 
