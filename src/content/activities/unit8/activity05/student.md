TUTORIAL PARA EJECUTAR EL PROTOTIPO
-
Para empezar vamos a listar los requisitos previos que debes tener para poder ejecutar el prototipo:
- Node.js (versión 12 o superior)
- npm (incluido con Node.js)
- Un navegador web moderno

# Paso 1: Clonar el repositorio 
Para este paso debes reemplazar [URL-DEL-REPOSITORIO] con la URL de GitHub del proyecto como te muestro en los siguientes comandos:

```bash
git clone [URL-DEL-REPOSITORIO]
cd [NOMBRE-DEL-DIRECTORIO]
```
# Paso 2: Instalar dependencias
Una vez estes en el directorio del proyecto ejecuta este comando: 

```bash
npm install
```
Este comando instalará todas las dependencias necesarias definidas en el archivo package.json, incluyendo Express y Socket.io.

# Paso 3: Verificar la Estructura de Archivos
Asegúrate de que tienes todos estos archivos en tu directorio:
- index.html - Página principal del visualizador
- remote.html - Página de control remoto
- server.js - Servidor Express y lógica de datos
- sketch.js - Código de visualización (p5.js)
- package.json - Definición de dependencias
- Imágenes: tortuga.png, saltamontes.png, lobo.png, cheeta.png, halcon.png
tortuga_bogota.png, saltamontes_bogota.png, lobo_bogota.png, cheeta_bogota.png, halcon_bogota.png

# Paso 4: Arranca el servidor
Ejecuta el siguiente comando para iniciar la aplicación
```bash
npm start
```
# Paso 5: Accede a la aplicación
Una vez que el servidor esté en funcionamiento, verás mensajes como:
```bash
Servidor escuchando en http://localhost:3000
Control remoto disponible en http://localhost:3000/remote
```
Abre tu navegador y abre en dos pestañas:
- Para ver el visualizador principal: http://localhost:3000
- Para acceder al control remoto: http://localhost:3000/remote

# Paso 6: Uso del control remoto
En la página de control remoto (http://localhost:3000/remote), puedes:

1. Presionar el botón "Medellín" para usar el conjunto de imágenes de animales con flores
2. Presionar el botón "Bogotá" para usar el conjunto alternativo de imágenes de animales

El cambio se reflejará automáticamente en todas las instancias del visualizador principal.

# Paso 7: Interpretación del visualizador
En la pantalla principal (http://localhost:3000):

El color del cuadrado representa la velocidad actual
El animal mostrado cambia según la velocidad (de más rápido a más lento):
- Halcón (rojo) - Mayores velocidades 
- Cheeta (naranja)
- Lobo (amarillo)
- Saltamontes (verde)
- Tortuga (morado) - menores velocidades 

La línea vertical en la barra inferior indica la velocidad actual en la escala de colores y la velocidad se muestra en metros por segundo.

**Para generar los datos NMEA con el simulador (son los datos que te van a dar las velocidades) mira el DEMO, ahí explico cómo usar la página del simulador**



