Reescritura del código
-
Para esta fase de la unidad, decidí renombrar todas mis variables y funciones en español y hacerlas mucho más descrptivas para entender mejor que está haciendo el código en cada parte.

A mi personalmente me gusta tener los códigos bien comentados porque así los entiendo  mucho más, entonces decidí conservar la mayoría de comentarios que ya el código tenía. 
También eliminé el código innecesario de coordenadas de respaldo para cuando no hay datos NMEA disponibles y añadí un texto de "No hay movimiento" para indicar que no hay velocidades por calcular.

**ANTES**

![Captura de pantalla 2025-05-08 011038](https://github.com/user-attachments/assets/66a84c19-c165-4e6d-893e-39c16798ec02)

**AHORA**

![image](https://github.com/user-attachments/assets/9d34340f-40df-477b-8f40-a7246c037956)

CAMBIOS DE NOMBRES DE VARIABLES Y FUNCIONES
-

**ANTES**

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

**AHORA**

```js
function draw() {
  background(0);
  
  // Dibuja el cuadrado principal grande primero
  // Aumentamos el tamaño del cuadrado para dar espacio al nombre y al animal
  let tamanioCuadrado = 400; // Aumentado para dar más espacio a todo
  let posicionX = width / 2;
  let posicionY = height / 2 - 30; // Ajustado para centrar mejor con el nuevo canvas
  
  if (datosDisponibles) {
    // Obtener el color actual basado en el ritmo
    let colorActual = obtenerColorPorRitmo(ritmoActual);
    
    // Dibujar el cuadrado grande con el color actual
    noStroke();
    fill(colorActual);
    rectMode(CENTER);
    rect(posicionX, posicionY, tamanioCuadrado, tamanioCuadrado);
    
    // Dibujar las imágenes de animales con transición según el modo actual
    // La imagen se dibuja un poco más abajo para dejar espacio al nombre en la parte superior
    dibujarAnimalConTransicion(posicionX, posicionY + 20, tamanioCuadrado * 0.7, ritmoActual);
    

    // Usar la fuente cargada si está disponible, si no, usar la fuente por defecto
    textAlign(CENTER, TOP);
    textStyle(BOLD);
    textSize(65); // Texto más grande para mejor visibilidad
    
    // Primero dibujar una versión en negro detrás para crear el efecto de sombra/borde
    fill(0);
    for (let i = -3; i <= 3; i++) {
      for (let j = -3; j <= 3; j++) {
        if (i*i + j*j >= 9) continue; // Solo las posiciones en un círculo
        text(nombreCorredor, posicionX + i, posicionY - tamanioCuadrado/2 + 15 + j);
      }
    }
    
    // Luego dibujar el texto negro encima
    fill(0);
    text(nombreCorredor, posicionX, posicionY - tamanioCuadrado/2 + 15);
    
    // Dibujar el gradiente pequeño debajo del cuadrado
    let alturaGradiente = 30;
    let posicionYGradiente = posicionY + (tamanioCuadrado / 2) + 30;
    dibujarFondoGradiente(posicionYGradiente, alturaGradiente);
    
    // Línea indicadora del ritmo actual en el gradiente
    let x = map(ritmoActual, 3, 10, 0, width);
    stroke(255);
    strokeWeight(5);
    line(x, posicionYGradiente - 10, x, posicionYGradiente + alturaGradiente + 10);
    
    // Mostrar texto de velocidad con mejor visibilidad
    // Primero dibujamos un fondo para el texto
    fill(0, 0, 0, 200); // Fondo semi-transparente
    noStroke();
    rectMode(CENTER);
    rect(width/2, posicionYGradiente + alturaGradiente + 40, 250, 40, 10); // Rectángulo redondeado
    
    // Luego dibujamos el texto con borde
    textSize(22);
    textAlign(CENTER, CENTER);
    textStyle(BOLD);
    fill(255); // Texto blanco
    text(`Velocidad: ${nf(velocidadActual, 1, 2)} m/s`, width/2, posicionYGradiente + alturaGradiente + 40);

    // Mostrar indicador de modo actual (Medellín/Bogotá)
    fill(0, 0, 0, 200); // Fondo semi-transparente
    rect(width/2, posicionYGradiente + alturaGradiente + 85, 150, 30, 8); // Rectángulo redondeado
    textSize(16);
    fill(255); // Texto blanco
    text(`Modo: ${modoActual === 'medellin' ? 'Medellín' : 'Bogotá'}`, width/2, posicionYGradiente + alturaGradiente + 85);

    // Guardar histórico (para promedio futuro)
    historicoRitmos.push(ritmoActual);
    if (historicoRitmos.length > 1000) historicoRitmos.shift();

    // Transición suave
    ritmoActual = lerp(ritmoActual, ritmoObjetivo, 0.05);
  } else {
    // Mostrar mensaje de "No hay movimiento" cuando no hay datos disponibles
    fill(50); // Fondo gris oscuro para el cuadrado
    rectMode(CENTER);
    rect(posicionX, posicionY, tamanioCuadrado, tamanioCuadrado);
    
    textSize(32);
    textAlign(CENTER, CENTER);
    fill(255, 0, 0); // Color rojo para el texto
    text("No hay movimiento", posicionX, posicionY);
  }

  // Mostrar texto informativo en la parte inferior
  textSize(16);
  textAlign(CENTER, BOTTOM);
  fill(255);
  text("Visualización en tiempo real de datos GPS • Corredor", width/2, height - 10);
}

function dibujarFondoGradiente(y, altura) {
  for (let x = 0; x < width; x++) {
    let ritmo = map(x, 0, width, 3, 10);
    let c = obtenerColorPorRitmo(ritmo);
    stroke(c);
    line(x, y, x, y + altura);
  }
}
```
Un código limpio (con nombres claros, estructura coherente) facilita su lectura tanto para quien lo escribió como para otros desarrolladores que deban entenderlo más adelante.

Los comentarios precisos permiten comprender rápidamente el propósito de cada bloque, especialmente cuando se manejan procesos complejos o interacción entre múltiples componentes, como en este caso con entradas de GPS, procesamiento de datos y salidas visuales sincronizadas.
