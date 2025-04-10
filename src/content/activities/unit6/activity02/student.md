INPUTS DETALLADOS
-

**1. Simulación del recorrido  del corredor (GPS Simulator)**

Para simular la ruta de un corredor en tiempo real, utilizamos un simulador de GPS que genera un archivo con coordenadas geográficas en formato NMEA.

Tipo de dato: Coordenadas geográficas (latitud, longitud)

Formato esperado: Decimal o cadena en formato NMEA, por ejemplo: 6.25184,-75.56359

Rango esperado: Ruta corta de 1 metro (demo)

¿Se simula?: Sí.
La ruta es generada artificialmente usando una herramienta de simulación GPS. Esta herramienta permite trazar un trayecto (por ejemplo, 1 metro de distancia) y descargar un archivo de datos simulados que emulan el movimiento de un corredor.

Uso en la experiencia:
Estos datos se utilizan en un código de p5.js para calcular las velocidades instantáneas del corredor.
