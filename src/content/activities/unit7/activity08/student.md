Planificación para la documentación final
-

Del concepto que planteé inicialmente me gustaría conservar casi todo lo que tiene que ver con los animales, lo que si me gustaría adaptar es el control remoto de las ciudades y los respectivos elementos caracteristicos en las imágenes para que sean más agradables visualmente y mejorar las transiciones.

**Resumen del flujo IPO**

El sistema recibe datos de velocidad (velocity) simulados desde el servidor. Estos datos son procesados en tiempo real por el cliente para generar una visualización dinámica: cambia el color del fondo, la imagen del animal y muestra texto e indicadores que representan el estado del corredor.

| Etapa     | Descripción breve                                      | Ejemplo de código clave                         |
|-----------|--------------------------------------------------------|-------------------------------------------------|
| **Input** | Datos recibidos desde el servidor (`pace`, `velocity`) | `socket.on('pace_update', (data) => {...})`     |
| **Process** | Suavizado, mapeo de ritmo a color/imagen              | `currentPace = lerp(currentPace, targetPace, 0.05)`<br>`getColorForPace(currentPace)` |
| **Output** | Visualización en pantalla: color, imagen, texto        | `fill(currentColor); rect(...)`<br>`drawAnimalWithTransition(...)`<br>`text(...)`      |

¿Qué aspectos clave del prototipo necesitas mostrar en el video? ¿Cómo capturarás la interacción y la generatividad? ¿Qué software usarás para grabar y editar (si es necesario)? 

El video debe mostrar cómo se abre la aplicación en el navegador y se establece la conexión con el servidor, luego se debe explicar brevemente que los datos de velocidad (velocity) provienen del servidor y se actualizan automáticamente cada dos segundos y mostrar cómo se usan esos datos para controlar los elementos visuales.

También el video debe mostrar cómo el sistema cambia de modo y cómo cambian las imágenes de los animales según el modo. Por último, mostrar una vista general del sistema funcionando con todos los elementos actualizándose: color, imagen, gradiente, velocidad y modo.

Para grabar creo que usaré Capcup, clipChamp o After Effects ya que son los que más se usar y sé que tienen todas las herramientas para hacer una buena edición. 

