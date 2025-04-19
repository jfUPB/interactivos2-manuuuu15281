INPUTS DETALLADOS
-

**1. Simulación del recorrido  del corredor (GPS Simulator)**

Para simular la ruta de un corredor en tiempo real, utilizamos un simulador de GPS que genera un archivo con coordenadas geográficas en formato NMEA. Este formato lo usaremos en un código en p5.js que sacará  las velocidades en determinado tramo **(para mi demo decidí que será de 1km)** de un corredor con ritmo promedio de 6:30 min/km que recorre 2.5 m/s (estas son las especificaciones con las que configuré el GPS simulator para verificar que si funcionara y efectivamente si sirvió. La ruta es generada artificialmente usando una herramienta de simulación GPS. Esta herramienta permite trazar un trayecto (por ejemplo, 1 metro de distancia) y descargar un archivo de datos simulados que emulan el movimiento de un corredor. 

El tipo de dato que se recibirá en el primer código serán coordenadas geográficas (latitud, longitud) y el formato esperado luego de que el código nos saque la velocidad será decimal. Finalmente las velocidades obtenidas serán enviadas al código de p5.js encargado de hacer las visualizaciones (que estará parametrizado el color del fondo para que se modifique con estos datos de velocidad) 

Para ver más o menos cómo hacerlo, tracé una ruta de 1km más o menos y configuré el programa como si fuera un corredor que hace 2,5 m/s: 

**Ruta: Desde el puente de la 4 sur hasta el parque de cristo rey**

![Cuandro Comparativo](../../../../assets/ejemplo35.png) 

Luego saqué el archivo con las coordenadas y otras informaciones y con ayuda del código de p5.js saqué las velocidades:

[CLIC AQUÍ PARA VER CÓMO OBTENDRIAMOS LAS VELOCIDADES CON EL CÓDIGO DE p5.js](https://editor.p5js.org/manuuuu15281/sketches/OfDxS9mYy)

Pdt: Es importante aclarar que aún no se si me sirva de la manera en que lo tengo en este código (ojalá que si). Aún debo probarlo y verificar si es posible hacer el envio de las velocidades en un array.

Por último, quise experimentar en cómo hacer los visuales con p5.js y funcionó pero en esta  parte simulé dentro del mismo código las velocidades (solo para ver como se movía el fondo y si era fluido, era más para ver la parte del diseño), logré que se viera fluido y dentro de los rangos de colores que establecí desde la unidad pasada para las distintas velocidades. 

![Cuandro Comparativo](../../../../assets/ejemplo36.png)

[CLIC AQUÍ PARA VER CÓMO SE VERÁN EL FONDO DE LOS VISUALES CON EL CÓDIGO DE p5.js](https://editor.p5js.org/manuuuu15281/sketches/7yMoI1-Tl)

Un tercer input proviene de un dispositivo móvil, que cumple la función de panel de control remoto para el administrador de la carrera. Desde allí, se puede seleccionar el modo de ambientación visual según la ciudad donde se realice la carrera. El tipo de dato será texto (etiqueta de ciudad) y su el formato será un String. Ejemplos: "cartagena", "bogota", "medellin", "pasto", "quindio".

Esto lo voy a simular a través de una interfaz con botones (radio buttons o menú desplegable) desde una aplicación en p5.js. Su importancia dentro de la experiencia es que define el estilo visual y decorativo de los elementos culturales. Por ejemplo, si se elige “Cartagena”, los animales que aparecen en pantalla pueden estar adornados con accesorios playeros, mientras que en “Bogotá” tendrían indumentaria andina o urbana. Este parámetro también actúa como un filtro que modifica el carácter de la visualización general.

PROCESSING DETALLADO
-
El procesamiento tendrá 3 fases:

**Fase 1: Entrada y procesamiento de coordenadas GPS**

El proceso comienza con la carga de un archivo simulado de coordenadas GPS, generado mediante una herramienta de simulación (como un simulador GPS). Este archivo contiene una secuencia de posiciones geográficas que representan el desplazamiento del corredor durante la experiencia. Luego, un script en p5.js se encarga de procesar estas coordenadas para **calcular la distancia entre puntos consecutivos y el tiempo que ha transcurrido entre cada registro. Con esta información, se estima la velocidad del corredor** en tiempo real utilizando la fórmula: velocidad = distancia / tiempo. Este valor se actualiza continuamente, permitiendo simular de manera fiel el avance de un corredor a un ritmo constante o variable.

**Fase 2: Mapeo de velocidad a visualización de colores**

Una vez obtenida la velocidad, esta estará condicionada en un rango definido —por ejemplo, entre 3 km/h y 18 km/h— y luego tendré que traducirlo para que se interprete como una posición dentro de un gradiente de colores (diseñado para ser fluido y vibrante). Esto lo hare usando funciones como lerpColor() en p5.js, se obtiene el color correspondiente a esa posición en el gradiente. El color resultante se aplica de dos formas fundamentales:

- Al fondo visual de la experiencia, el cual cambia dinámicamente en tiempo real según la velocidad del corredor.

- A un cuadro de referencia, ubicado en pantalla, que muestra directamente el color asociado a la velocidad actual.

(Por el momento lo planteé así para ver si está variando el color según la velocidad, luego cuando ya esté segura de que funciona dejaré solo el cuadro de referencia)

**Fase 3: Selección del contexto ambiental**

Paralelamente, la experiencia permite que un administrador, desde un dispositivo móvil, seleccione la ciudad o contexto geográfico en el cual se enmarca la simulación. Esta selección (por ejemplo, "Cartagena", "Bogotá" o "Quindío") se recibe como un parámetro denominado modo_cultural.

Este modo define una capa adicional de personalización visual, influyendo en la decoración de los elementos de la experiencia. Por ejemplo:

Los animales o elementos ilustrados pueden aparecer con accesorios propios de la región (como sombreros vueltiaos en la costa o ruanas en zonas andinas).

De este modo, la ambientación visual no solo responde al esfuerzo físico (velocidad del corredor), sino también al entorno simbólico donde ocurre la carrera, haciendo que la experiencia sea tanto física como culturalmente inmersiva.

(Debo seguir trabajando en esta parte y definir mucho mejor cómo será su realización)

OUTPUT DETALLADO
-

La experiencia está centrada en un sistema multisensorial, donde los outputs se manifiestan principalmente en el plano visual e interactivo:

**Visual:** Elemento dominante de la experiencia. Representa de forma abstracta la intensidad del movimiento del corredor y el contexto cultural.

**Interactivo:** Los cambios visuales responden en tiempo real a inputs tanto físicos (velocidad del corredor) como contextuales (modo cultural definido por el usuario administrador).

En el fondo de colores reactivo con su animal predeterminado que será de tipo visual, el comportamiento del fondo está dado cuando varía de manera continua y fluida a lo largo de un gradiente de colores, en función de la velocidad del corredor. Ejemplo: A velocidades más bajas, predominan tonos fríos (azules, verdes); a velocidades altas, se transiciona hacia colores cálidos e intensos (rojos, amarillos, naranjas).

Para la decoración según el contexto geográfico que será de tipo visual y escenográfico, tendrá propiedades dinámicas cómo atuendos, objetos, flora, arquitectura y demás elementos gráficos representativos de cada región y su comportamiento corresponde al momento en el que se activa y modifica según la ciudad seleccionada desde el control remoto (modo cultural).

Aquí está más especificado: 

**Cartagena:** Animales con gafas de sol, collares de flores, accesorios de playa.

**Bogotá:** Elementos con ruanas, frailejones, arquitectura colonial y andina.

**Medellín:** Fondos florales, ferias, tipografía paisa.

**Pasto:** Referencias al Carnaval de Negros y Blancos.

**Quindío:** Cafetales, sombreros aguadeños, vegetación frondosa.
