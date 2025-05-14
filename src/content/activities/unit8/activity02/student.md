ESQUEMA (ÍNDICE) 
-
- Titulo de mi proyecto
- ¿Cómo llegué a querer hacer esto?¿De qué se trata?
- El concepto
- Método IPO aplicado a mi proyecto.
- Video del prototipo funcionando
- Programas. tecnologías y métodos utilizados.
- Aprendizajes del proyecto.

TEXTOS CLAVE
-
1. CONCEPTO: Para mi proyecto de curso elegí una carrera de running interactiva, donde la velocidad, la constancia y el esfuerzo se convierten en colores, personajes y formas dinámicas. A través de sensores, pantallas interactivas y tecnología web en tiempo real, los corredores podrán ver reflejada su energía a lo largo del recorrido y en sus propios dispositivos. Cada zancada no solo los llevara a la meta, sino que irán pintando su esfuerzo en un lienzo digital que captura su esencia en cada momento de la carrera.
   
2. CONCEPTO: Mi idea es que cada corredor con ayuda de su celular o reloj inteligente pueda llevar el registro de que distancia lleva, a que velocidad va y de cuanto es su pace. Como su pace durante toda la carrera puede variar, la idea es que se genere un flyer que se modifique en tiempo real según el pace del corredor y por medio de una pantalla mostrar aleatoriamente los flyer de algunos corredores que estén pasando cerca de las pantallas.
   
3. NARRATIVA (ADAPTAR): Desde tiempos ancestrales, en los vastos territorios de la resistencia y la velocidad, existe una élite secreta de corredores conocida como "LA LIGA SALVAJE". Cada miembro de esta "orden" representa una forma única de correr, con habilidades y fortalezas propias. Se dice que los corredores que encuentran su espíritu velocista desbloquean su verdadero potencial y pueden alcanzar nuevas dimensiones en su desempeño.
En cada carrera, los velocistas de la orden observan y guían a los corredores, revelando su espíritu animal según su ritmo. Al final del recorrido, cada corredor descubre cuál de los cinco guardianes ha sido su guía principal, recibiendo su insignia.

![image](https://github.com/user-attachments/assets/053bf8dc-f252-4a67-990a-0ec8f3d16e7c)

![image](https://github.com/user-attachments/assets/cd05e4ef-27f3-4e2e-b8e0-d8bf59a88368)

4. PROTOTIPADO: Para simular la ruta de un corredor en tiempo real, utilizamos un simulador de GPS que genera un archivo con coordenadas geográficas en formato NMEA. Este formato lo usaremos en un código en p5.js que sacará las velocidades en determinado tramo (para mi demo decidí que será de 1km) de un corredor con ritmo promedio de 6:30 min/km que recorre 2.5 m/s (estas son las especificaciones con las que configuré el GPS simulator para verificar que si funcionara y efectivamente si sirvió. La ruta es generada artificialmente usando una herramienta de simulación GPS. Esta herramienta permite trazar un trayecto (por ejemplo, 1 metro de distancia) y descargar un archivo de datos simulados que emulan el movimiento de un corredor.

El tipo de dato que se recibirá en el primer código serán coordenadas geográficas (latitud, longitud) y el formato esperado luego de que el código nos saque la velocidad será decimal. Finalmente las velocidades obtenidas serán enviadas al código de p5.js encargado de hacer las visualizaciones (que estará parametrizado el color del fondo para que se modifique con estos datos de velocidad).

[CLIC AQUÍ PARA VER CÓMO OBTENDRIAMOS LAS VELOCIDADES CON EL CÓDIGO DE p5.js](https://editor.p5js.org/manuuuu15281/sketches/OfDxS9mYy)

5. PROTOTIPADO: Un tercer input proviene de un dispositivo móvil, que cumple la función de panel de control remoto para el administrador de la carrera. Desde allí, se puede seleccionar el modo de ambientación visual según la ciudad donde se realice la carrera. El tipo de dato será texto (etiqueta de ciudad) y su el formato será un String. Ejemplos: "cartagena", "bogota", "medellin", "pasto", "quindio".

Esto lo voy a simular a través de una interfaz con botones (radio buttons o menú desplegable) desde una aplicación en p5.js. Su importancia dentro de la experiencia es que define el estilo visual y decorativo de los elementos culturales.


Mi código final está organizado, solo me queda montarlo al repo :)
