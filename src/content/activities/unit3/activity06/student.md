muestra el código de la aplicación: servidor y clientes. Explica claramente:

**Mi código de Sketch.js (Cliente):**

```
let capture;
let socket;
let photoGallery = [];
let captureButton;
let uploadButton;
let fileInput;

function setup() {
  createCanvas(640, 480);
  background(240);
  
  // Inicializar la conexión WebSocket
  socket = new WebSocket(`ws://${window.location.host}`);
  
  socket.onopen = () => {
    console.log('Conectado al servidor WebSocket');
  };
  
  socket.onmessage = (event) => {
    // Recibir imagen de otros usuarios
    let reader = new FileReader();
    reader.onload = function() {
      // Pre-cargar la imagen antes de añadirla a la galería
      loadImage(reader.result, img => {
        let imgObj = {
          img: img,
          timestamp: new Date().toLocaleTimeString()
        };
        photoGallery.push(imgObj);
        // Mantener solo las últimas 10 imágenes
        if (photoGallery.length > 10) {
          photoGallery.shift();
        }
      });
    };
    reader.readAsDataURL(event.data);
  };
  
  socket.onerror = (error) => {
    console.error('Error de WebSocket:', error);
  };
  
  // Inicializar cámara
  capture = createCapture(VIDEO);
  capture.size(320, 240);
  capture.hide();
  
  // Botón para capturar foto
  captureButton = createButton('Tomar Foto');
  captureButton.position(20, 520);
  captureButton.mousePressed(takePhoto);
  
  // Crear input para archivos primero
  fileInput = createFileInput(handleFile);
  fileInput.position(-200, -200); // Posicionarlo fuera de la vista
  
  // Botón para subir foto
  uploadButton = createButton('Seleccionar Foto');
  uploadButton.position(150, 520);
  uploadButton.mousePressed(() => {
    fileInput.elt.click();
  });
}

function draw() {
  background(240);
  
  // Mostrar vista previa de la cámara
  image(capture, 20, 20, 320, 240);
  
  // Dibujar un marco alrededor de la cámara
  noFill();
  stroke(0);
  rect(20, 20, 320, 240);
  
  // Mostrar imágenes recibidas en la galería
  displayGallery();
  
  // Mostrar instrucciones
  fill(0);
  noStroke();
  textSize(16);
  text('Vista previa de cámara', 20, 280);
  text('Galería de fotos recibidas:', 380, 30);
}

function takePhoto() {
  // Capturar imagen de la cámara
  let photo = capture.get();
  
  // Guardar la imagen en un canvas y convertirla a blob
  let canvas = document.createElement('canvas');
  canvas.width = photo.width;
  canvas.height = photo.height;
  
  // Dibujar la imagen en el canvas
  let ctx = canvas.getContext('2d');
  ctx.drawImage(photo.canvas, 0, 0);
  
  canvas.toBlob((blob) => {
    // Enviar la imagen por WebSocket
    if (socket && socket.readyState === WebSocket.OPEN) {
      socket.send(blob);
      console.log('Foto enviada');
    }
  }, 'image/jpeg', 0.85);
}

function handleFile(file) {
  console.log('Archivo seleccionado:', file.name, file.type, file.size);
  
  if (file.type.startsWith('image')) {
    // Procesar solo si es una imagen
    // Usando FileReader para leer el archivo como ArrayBuffer
    let reader = new FileReader();
    reader.onload = function() {
      let blob = new Blob([reader.result], {type: file.type});
      
      // Enviar blob directamente por WebSocket
      if (socket && socket.readyState === WebSocket.OPEN) {
        socket.send(blob);
        console.log('Imagen subida enviada:', file.name);
      }
    };
    reader.readAsArrayBuffer(file.file);
  } else {
    console.log('El archivo seleccionado no es una imagen');
  }
}

function displayGallery() {
  // Mostrar las últimas 3 imágenes recibidas
  let startIdx = Math.max(0, photoGallery.length - 3);
  
  for (let i = startIdx; i < photoGallery.length; i++) {
    let idx = i - startIdx;
    let item = photoGallery[i];
    
    // La imagen ya está cargada, simplemente mostrarla
    image(item.img, 360, 40 + idx * 140, 240, 120);
    
    // Añadir información de tiempo
    fill(0);
    noStroke();
    textSize(12);
    text(item.timestamp, 360, 40 + idx * 140 + 135);
  }
}
```
**Mi código de Server.js (Servidor):**

```
const express = require('express');
const http = require('http');
const WebSocket = require('ws');
const path = require('path');

// Crear servidor Express
const app = express();
const server = http.createServer(app);

// Configurar servidor WebSocket con opciones para manejar imágenes grandes
const wss = new WebSocket.Server({ 
  server,
  maxPayload: 10 * 1024 * 1024 // 10MB máximo tamaño de mensaje
});

// Almacenar conexiones activas
const clients = new Set();

// Manejar conexiones WebSocket
wss.on('connection', (ws) => {
  console.log('Cliente conectado');
  clients.add(ws);

  // Manejar mensajes recibidos
  ws.on('message', (message) => {
    console.log(`Imagen recibida (${message.length} bytes), reenviando a otros clientes`);
    
    // Reenviar el mensaje a todos los clientes excepto al que lo envió
    clients.forEach((client) => {
      if (client !== ws && client.readyState === WebSocket.OPEN) {
        try {
          client.send(message);
        } catch (e) {
          console.error('Error al enviar mensaje:', e);
        }
      }
    });
  });

  // Manejar errores
  ws.on('error', (error) => {
    console.error('Error de WebSocket:', error);
  });

  // Manejar desconexiones
  ws.on('close', () => {
    console.log('Cliente desconectado');
    clients.delete(ws);
  });
});

// Servir archivos estáticos desde la carpeta 'public'
app.use(express.static(path.join(__dirname, 'public')));

// Iniciar servidor
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Servidor ejecutándose en http://localhost:${PORT}`);
});
```

**¿Cómo se comunican los clientes con el servidor?**

La comunicación entre los clientes y el servidor se realiza mediante WebSockets.
El cliente establece una conexión WebSocket con el servidor al cargar la página web (socket = new WebSocket(ws://${window.location.host});)
Una vez establecida la conexión, tanto el cliente como el servidor pueden enviar mensajes en cualquier momento sin necesidad de iniciar nuevas solicitudes HTTP.


¿Cómo se comunican los clientes entre sí?
¿Qué tipo de mensajes se envían?
¿Qué tipo de datos se envían?
¿Qué tipo de eventos se generan?
¿Cómo es el flujo de datos entre los clientes y el servidor?
¿Cómo es el flujo de datos entre los clientes?
