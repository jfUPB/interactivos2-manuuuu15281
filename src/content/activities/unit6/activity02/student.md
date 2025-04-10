INPUTS DETALLADOS
-

**1. Simulación del recorrido  del corredor (GPS Simulator)**

Para simular la ruta de un corredor en tiempo real, utilizamos un simulador de GPS que genera un archivo con coordenadas geográficas en formato NMEA. Este formato lo usaremos en un código en p5.js que sacará  las velocidades en determinado tramo **(para mi demo decidí que será de 1km)** de un corredor con ritmo promedio de 6:30 min/km que recorre 2.5 m/s (estas son las especificaciones con las que configuré el GPS simulator para verificar que si funcionara y efectivamente si sirvió. La ruta es generada artificialmente usando una herramienta de simulación GPS. Esta herramienta permite trazar un trayecto (por ejemplo, 1 metro de distancia) y descargar un archivo de datos simulados que emulan el movimiento de un corredor. 
Para ver más o menos cómo hacerlo, tracé una ruta de 1km más o menos y configuré el programa como si fuera un corredor que hace 2,5 m/s: 



[CLIC AQUÍ PARA VER CÓMO OBTENDRIAMOS LAS VELOCIDADES CON EL CÓDIGO DE p5.js](https://editor.p5js.org/manuuuu15281/sketches/OfDxS9mYy)

Pdt: Es importante aclarar que aún no se si me sirva de la manera en que lo tengo en este código (ojalá que si). Aún debo probarlo y verificar si es posible hacer el envio de las velocidades en un array.

El tipo de dato que se recibirá en el primer código serán coordenadas geográficas (latitud, longitud) y el formato esperado luego de que el código nos saque la velocidad será decimal. Finalmente las velocidades obtenidas serán enviadas al código de p5.js encargado de hacer las visualizaciones (que estará parametrizado el color del fondo para que se modifique con estos datos de velocidad) 

