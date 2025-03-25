DISEÑO PRELIMINAR
-

La misión principal de mi experiencia es motivar a los corredores y agrandar la comunidad del running por medio de la interactividad que propone esta experiencia y la narrativa que la envuelve. 

Antes de la carrera quiero que los usuarios tengan contexto de cada animal y lo que representa, además es importante que los corredores antes de salir a correr ya estén vinculados con sus celulares o relojes inteligentes para poder proyectar sus flyers de manera aleatoria en las pantallas. Durante la experiencia el fondo del los flyers como el animal estará cambiando según el pacer que vaya creando el corredor, y estarán todo conectados a un gran sistema que permitirá visibilizar flyers aleatorios, además se irá promediando la info que se reciba de los dispositivos (el pace) para el flyer final. Al cruzar la meta, los corredores reciben su personalidad dominante con un resumen visual de su carrera, incentivando el sentido de logro y la conexión con su “instinto animal”.

La relación entre la narrativa de los animales y el desempeño de los corredores en **"LIGA SALVAJE"** se basa en una metáfora motivacional donde cada corredor, según su ritmo, encarna un arquetipo animal que representa su estilo de carrera y cada uno de estos simboliza una combinación de velocidad, resistencia y estrategia, reforzando la idea de que no importa el ritmo, todos tienen una esencia poderosa en la carrera. En lugar de enfocarse únicamente en ser el más rápido, la experiencia celebra la diversidad de ritmos y motiva a cada corredor con mensajes personalizados.

La información que se mostrará en las pantallas ubicadas en las rutas de la carrera será muy poco invasiva, aparecerá el nombre, su animal predominante hasta el momento y la frase corta motivacional, algo que los corredores no tengan que hacer mucho esfuerzo para leer, que el mensaje les sea claro y cumpla su objetivo.De manera aleatoria,la visualización será generada en p5.js, con gráficos dinámicos y transiciones llamativas.

La integración de los datos de cada corredor con su visualización en tiempo real se logrará mediante un sistema de captación, procesamiento y representación gráfica, usando tecnologías web como WebSockets, p5.js y servidores en Node.js. Cada corredor utilizará una app o dispositivo de seguimiento (ej. reloj GPS, smartphone con una app específica) que enviará datos clave como ritmo (pace en min/km), ubicación GPS, distancia recorrida y tiempo total de carrera y estos datos serán enviados mediante WebSockets a un servidor central, que los procesará en tiempo real, este analizará el pace actual de cada corredor y asignará su animal correspondiente, según la tabla de referencia (mostrada la actividad anterior). 

Para evitar que los corredores se ditraigan mientras ven la pantalla de su celular, la manera en que se puede mostrar la imagen es solo el fondo dinámico y la insignia del animal (sin letras que interpretar, solo info visual). También implementar notificaciones de voz cada que  el corredor cambie de animal, la idea es que la experiencia sea segura y no intrusiva. 

Otros datos que podrían enriquecer la experiencia podría ser detectando la aceleración o desaceleración del corredor y que este aspecto modifique algun parámetro del fondo (luminosidad, combine colores o que tenga algún efecto distintivo), también podriamos tener en cuenta la altitud o la candecia. 

El reporte final (flyer) se le entregará a los corredores via correo electronico o via mensaje de texto. Los elementos visuales que harán cada flyer único y personalizado será el fondo dinámico y sus datos del recorrido. 






