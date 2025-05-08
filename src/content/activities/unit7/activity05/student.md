Una de la situaciones que planteé en la unidad 6 era la posibilidad de que aconteciera un cambio constante del modo cultural desde el control remoto. Efectivamente se veía super mal y no era fluido el cambio de las imágenes (se me olvidó registrar cómo se veía el cambio) entonces para solucionarlo me dedique a editar las imágenes de los animales de la mejor manera posible y tratando de conservar las mismas posiciones y los mismos tamaños. Esto último con la intensión de que el cambio de imagen no se sintiera tan brusco, también me tocó añadir en el código un proceso de difuminación durante la transición de imagen a imagen y procurar que el usuario sienta satisfactorio el cambio de animal (Para ver esto puedes pasar al link del prototipo funcionando que está en la actividad anterior).

La segunda situación extrema que provoqué fue que el código no recibiera velocidades (eliminé los datos del archivo NMEA del server) 

![image](https://github.com/user-attachments/assets/b6dd343a-748a-4b8f-a824-198915020592)

Y me di cuenta que el código seguía funcionando normalmente (pero se movía de otra manera poco organica), esto no me pareció normal ya que yo esperaba que los visuales permanecieran quietos ya que no había movimiento. Lo que sucedió es que la IA, en un momento donde le pedí ayuda para la generación de los visuales me añadió unas coordenadas de respaldo en caso de que no existieran datos NMEA suficientes para generar los visuales (me pareció útil pero me impide ver si hay fallas en el programa). 

![image](https://github.com/user-attachments/assets/8a8098ab-b090-418d-9451-5a9afaa62b67)

Para lo que decidí cambiar esto y simplemente mostrar un mensaje de "No hay movimiento" o "Estás quiet@" para entender de que no se está moviendo y no hay velocidad por calcular. 

(pendiente por colocar el resultado del cambio)

![image](https://github.com/user-attachments/assets/b40dc10a-48ea-4c4e-8ef4-e092fda59c77)
