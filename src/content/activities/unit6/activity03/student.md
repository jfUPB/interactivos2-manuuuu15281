SITUACIONES EXTREMAS
-

**1. Velocidad mínima (casi 0 km/h)**

Lo que puede pasar es que el archivo GPS simulado muestra velocidades extremadamente bajas (por ejemplo, 0.2 km/h, como si el corredor estuviera detenido o caminando lentamente). El output esperado será el color de fondo se estanca en tonos fríos (azules oscuros, verdes suaves)y la animación visual se vuelve más lenta o casi estática (no va a cambiar entre ningún otro color).

Para que la experiencia no deje de ser significativa, podríamos tener un tipo de "feedback" que quiera decir que el corredor está tomando un descanso, pausa o reflexión. Puede ser usado narrativamente como un "respiro" entre tantos esfuerzos.


**2. Input: Velocidad máxima (más de 18 km/h)**

Se simula una velocidad fuera del rango típico (por ejemplo, 20 km/h). El output esperado será que las posición del gradiente se limita al máximo y el fondo muestra su color más cálido e intenso (rojo vibrante o naranja). Esto debe controlarse porque puede saturar visualmente o parecer bugueada la experiencia.

Para solucionarlo podría añadir un "modo turbo" o efecto visual extra (chispas, ráfagas) para reforzar la idea de velocidad extrema y que no parezca mala o tildada la experiencia.


**3. No hay movimiento (velocidad cero constante)**

El archivo GPS simulado no cambia de coordenadas. El output esperado será que el fondo permanece congelado en un solo color frío y la experiencia se siente inactiva o “vacía”. Esto presenta un problema ya que el público podría interpretar que hay un fallo técnico. Mi solución es añadir una cajita de texto que indique "Sin movimiento" y así dar a entender que el fondo está quieto por falta de interactividad.

**4. Cambio constante del modo cultural desde el control remoto**
   
Escenario: El administrador cambia rápidamente entre modos culturales (Cartagena → Bogotá → Medellín…). El output esperado será cambios bruscos de visuales y una posible sobrecarga de estímulos o pérdida de coherencia narrativa. Esto podría romper la inmersión y puede saturar al espectador. (No se muy bien como solucionarlo, estoy pensando)

**5. Falla del GPS o del envío de velocidades al codigo de los visuales**
   
Puede ocurrir una interrupción en la lectura del archivo simulado, error en la extracción de coordenadas o detención del flujo de datos.Lo que puede ocurrir es que el sistema deja de modificar el color del fondo y se congela en su último estado, con esto el público puede percibir que “algo se dañó” y para solucionarlo puedo mostrar un mensaje que indique “esperando señal de movimiento”.

**6.  Falla del dispositivo de control remoto (modo cultural)**

El móvil se desconecta, se pierde conexión con el socket o no se recibe ningún nuevo modo cultural y lo que genera es que see mantiene el último modo visual activo y no recibe datos, se conserva la ambientación anterior. Para podría integrarse un modo por defecto si no se recibe ningún input al principio, que la experiencia siempre inicie en “modo neutro” donde los animales no están ambientados de ninguna manera.
