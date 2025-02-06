#### EJEMPLO 1 - P_2_1_2_01 de generative-design

![Cuandro Comparativo](../../../../assets/ejemplo11.png)

**Descripción**

Este código crea una cuadrícula de círculos, cuya posición, tamaño y apariencia cambian en función de la posición del mouse y la interacción del usuario. Los parámetros utilizados son:
- **tileCount (20):** Define el número de filas y columnas de la cuadrícula.
- **circleAlpha (130):** Define la transparencia del color del trazo de los círculos.
- **circleColor (negro con alpha 130):** Color del trazo de los círculos.
- **mouseX:** Controla el desplazamiento aleatorio de los círculos en las direcciones X e Y (shiftX y shiftY).Cuanto más a la derecha esté el mouse, mayor será el rango de desplazamiento.
- **mouseY:** Controla el tamaño de los círculos (mouseY / 15) y el grosor del trazo (mouseY / 60).Cuanto más abajo esté el mouse, más grandes y gruesos serán los círculos.

**Variaciones**
Las variaciones que hice fueron en el color de los circulos, de negro a rosado y en el grosor de los circulos donde se lo aumenté y ahora son más gorditos. Esto lo hice modificando ambos parametros, **circleColor** para obtener el rosado y **strokeWeight** para modificar el tamaño de los circulos. 
En la imagen se pueden observar ambos parámetros modificados:

![Cuandro Comparativo](../../../../assets/ejemplo12.png)

#### EJEMPLO 2 - P_2_1_4_01 de generative-design

![Cuandro Comparativo](../../../../assets/ejemplo13.png)

**Descripción**

Este código en p5.js crea una representación gráfica de una imagen usando una cuadrícula de casillas de verificación (checkboxes) en lugar de píxeles. Dependiendo del brillo de cada píxel en la imagen y del valor del deslizador, las casillas de verificación se activan o desactivan para generar una representación visual del contenido de la imagen. Los parámetros importantes son: 

- **Imágenes:** Se cargan tres imágenes (shapes.png, draw.png y toner.png), que el usuario puede alternar usando las teclas 1, 2 y 3.

- **Grilla de casillas:** La grilla tiene un tamaño fijo de 40x40. Cada casilla representa un píxel de la imagen redimensionada. box.style('display', 'inline') asegura que las casillas se alineen horizontalmente para formar una cuadrícula.

- **Umbral de brillo (threshold):** Ajustado por el deslizador (slider), con valores entre 0 y 255. Determina si una casilla debe estar marcada (checked) o no, en función del brillo del píxel.

- **Brillo de los píxeles:** Calculado como el promedio de los valores de rojo, verde y azul ((red + green + blue) / 3). Comparado con el valor del deslizador para decidir si el checkbox se activa.
