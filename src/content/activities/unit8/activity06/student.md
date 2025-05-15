LIGA SALVAJE
-

**INTRODUCCIÓN**

Imagina correr y ver cómo tu energía se convierte en color, movimiento y significado.
**LIGA SALVAJE** es una experiencia donde tu ritmo no solo cuenta metros, sino que construye una historia visual que reacciona a cada paso que das.

Este proyecto nace de la idea de mezclar el mundo físico del running con lo digital, para crear una experiencia personalizada. Mientras corres, tu velocidad activa visuales generativos y animales simbólicos que hablan de quién eres en movimiento. ¿Rápido como un halcón o constante como un lobo? Aquí, el asfalto se convierte en escenario y tú, en protagonista.

Además, la experiencia se adapta a la ciudad donde se desarrolla: Bogotá o Medellín… cada entorno transforma la estética de la carrera y refuerza la conexión entre cuerpo, tecnología y cultura.

Este documento te lleva por dentro del proceso creativo y técnico de la Liga Interactiva: desde cómo se capturan los datos hasta cómo se visualizan en tiempo real. Si te interesa explorar la frontera entre arte, deporte y sistemas físicos interactivos, estás en el lugar correcto.

**NARRATIVA**

**LIGA SALVAJE** es una experiencia digital pensada para transformar una simple carrera en una narrativa visual y motivacional. Su idea central es convertir la velocidad a la que se desplaza de cada corredor en una historia única que se proyecta en tiempo real, utilizando colores y símbolos animales como forma de expresión.

A medida que una persona corre, su velocidad es medida y utilizada para modificar el fondo visual que lo acompaña: los colores cambian de forma fluida según su ritmo, creando un paisaje cromático dinámico. Además, dependiendo de su velocidad, se le asigna un animal simbólico como un halcón, una cheeta, un lobo, un saltamontes o una tortuga ninja, cada uno con una personalidad distinta. Estos animales no representan quién corre más rápido, sino cómo corre cada persona, resaltando cualidades como la constancia, la estrategia o la energía.

A esto se suma una capa cultural: la experiencia se adapta visualmente al lugar donde ocurre la carrera. Si es en Medellín, los animales aparecerán con elementos florales y muy paisas; si es en Bogotá, llevarán accesorios característicos de la ciudad andina. Esta ambientación refuerza la conexión entre el cuerpo en movimiento y el territorio que habita.

En resumen, Liga Interactiva convierte los datos físicos de una carrera (como la velocidad) en una experiencia significativa, que celebra la diversidad de estilos al correr. No se trata de competir, sino de reconocerse en el movimiento.

**MÉTODO IPO**

Para estructurar el diseño de **LIGA SALVAJE**, se utilizó el método IPO (Input – Process – Output), una metodología sencilla pero poderosa que permite visualizar cómo fluye la información dentro de un sistema interactivo.

**Input (Entrada):** Datos que el sistema recibe, como la velocidad del corredor, su ubicación o la ciudad seleccionada desde un dispositivo remoto.

En esta sesión tenemos como input principal la velocidad a la que se está desplazando el corredor (esta fue simulada con un GPS Simulator) y tenemos un segundo input que viene desde el control remoto, donde se define en qué lugar se está corriendo. 

**Process (Proceso):** Algoritmos que transforman esos datos en decisiones visuales y narrativas: cálculo de velocidad, asignación de colores y selección de animales simbólicos.

El sistema comienza leyendo un archivo que contiene una serie de ubicaciones simuladas, como si fueran puntos por donde ha pasado una persona corriendo. A partir de estos puntos, el sistema calcula qué tan lejos se movió entre cada uno y cuánto tiempo pasó, lo que permite saber a qué velocidad estaba corriendo.

Una vez calculada la velocidad, esta se convierte en un valor que se usa para elegir un color dentro de una barra de colores. Ese color representa cómo está corriendo la persona en ese momento. Finalmente, ese color aparece en una parte visual del proyecto: un cuadro o “flyer” que cambia en tiempo real y sirve como indicador visual del ritmo actual del corredor.


**Output (Salida):** El resultado visible o sensorial que se genera: visuales en tiempo real y una ambientación adaptada al contexto cultural.

En esta parte se generan los visuales con ayuda de las velocidades que se calcularon, los colores y el animal cambian en función de ellas. Además, los objetos o frases que lleven las imagenes de los animales cambian en función del control remoto. 



**Para generar los datos NMEA con el simulador (son los datos que te van a dar las velocidades) mira el DEMO, ahí explico cómo usar la página del simulador**

**TECNOLOGIAS**

Las tecnologias usadas para este primer prototipo fueron:
1. El simulador de datos de movimiento
2. WebSockets para establecer conexiones.
3. Códigos en p5.js para experimentar









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




