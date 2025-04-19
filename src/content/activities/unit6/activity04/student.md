SIMULACIONES
-
El input de mi proyecto que será simulado son las velocidades de los corredores.

Utilizaremos una herramienta de simulación GPS (como GPS Simulator), donde se marcará una ruta fija, por ejemplo, de 1 metro, para que sea manejable en demostraciones y pruebas.
El GPS Simulator generará un archivo con coordenadas que representarán el recorrido del corredor a un ritmo constante, simulado a 6:30 min/km (aproximadamente 2.57 m/s) y este archivo incluirá marcas de tiempo y coordenadas geográficas (latitud, longitud), necesarias para estimar velocidad.

Luego en un código específico hecho en p5.js se cargará el archivo con las coordenadas simuladas (en .json) y el sistema leerá las posiciones consecutivas (lat, lon) y sus marcas de tiempo. Con base en estos datos, se calculará la distancia entre puntos usando la fórmula de Haversine y finalmente se estimará la velocidad a partir de la fórmula: **velocidad (km/h) = Distancia/ Tiempo**.
Esta velocidad estimada se enviará luego a otro código p5.js, que la usará como input para transformar la visualización de fondo según el ritmo del corredor.

La simulación debe imitar de forma realista cómo variaría la velocidad de un corredor en movimiento (se puede definir que tipo de corredor quiero simular: un éliteo un promedio), permitiendo observar cómo reacciona la visualización a distintos ritmos. Este comportamiento es fundamental para validar que el sistema responda correctamente en transiciones suaves, momentos de aceleración y frenadas bruscas.

