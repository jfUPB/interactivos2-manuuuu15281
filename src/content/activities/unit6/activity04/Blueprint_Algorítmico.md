RESUMEN DEL PROYECTO
-

"LIGA SALVAJE" es una visualización generativa inspirada en el movimiento de un corredor, donde la velocidad transforma fondos cromáticos y los entornos culturales se adaptan según la ciudad. Un homenaje al ritmo y la diversidad colombiana, vista a través del movimiento humano en una gran carrera.

TABLITA DE INPUTS
-

| Input                   | Tipo de Dato         | Simulación                                   |
|------------------------|----------------------|----------------------------------------------|
| Coordenadas GPS        | Coordenadas (lat, lon) | Se usa el GPS Simulator     |
| Velocidad (calculada)  | Numérico (km/h)      | Código en p5.js que saca la velocidad según datos del GPS|
| Modo cultural (ciudad) | Texto (string)       | Selección desde un dispositivo móvil remoto  |

PROCESSING
-
Se inicia con la carga de un archivo que contiene coordenadas simuladas en formato NMEA. El sistema procesa este archivo calculando la distancia entre puntos consecutivos y el tiempo transcurrido entre cada punto. A partir de estos datos se estima la velocidad del corredor en kilómetros por hora usando la fórmula:
velocidad = distancia / tiempo.

Luego la velocidad calculada se transforma en un valor t dentro del rango [0.0, 1.0], representando su posición dentro del rango de colores. Este color dado por su velocidad se aplica en el codigo de los visuales, donde hay un cuadro (flyer) que será un indicador y actúa como referencia visual del estado actual.

El input adicional proviene de un dispositivo móvil que actúa como control remoto. Desde allí, el administrador selecciona una ciudad (como Cartagena, Bogotá, Medellín, etc.). Esta elección define un "modo cultural" que modifica la estética de la experiencia, influyendo en: La decoración visual de elementos (como animales que adoptan vestimenta o accesorios característicos del lugar).

TABLITA DE OUTPUTS
-

| Output                           | Tipo Principal | Elementos Generados                                      | Propiedades Dinámicas                                          | Relación con Inputs                                                       |
|----------------------------------|----------------|----------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------------------|
| Fondo visual reactivo            | Visual         | Gradiente de colores en movimiento                       | Color cambia en tiempo real según velocidad                    | La velocidad se mapea a un valor `t` (0.0–1.0) → color mediante `lerpColor()` |
| Animales y decoración cultural   | Visual         | Figuras adornadas con elementos locales (ej. ruana, gafas de sol) | Estilo gráfico y accesorios cambian según la ciudad seleccionada | Control remoto define el “modo cultural” que altera esta capa visual      |

