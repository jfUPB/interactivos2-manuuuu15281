DOCUMENTACIÓN FINAL
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

**TECNOLOGIAS**

Las tecnologias usadas para este primer prototipo fueron:
1. El simulador de datos de movimiento
2. WebSockets para establecer conexiones.
3. Códigos en p5.js para experimentar
