
#  Proyect

Parcial 1 Sistema de Procesamiento de Datos/MATIAS CUERVO -1B
[![spd-foto-parcial.png](https://i.postimg.cc/76z2qJjb/spd-foto-parcial.png)](https://postimg.cc/CBF1N5hV)
[![spd-foto-parcial-2.png](https://i.postimg.cc/MH8jHK85/spd-foto-parcial-2.png)](https://postimg.cc/G8qpSRtT)

# Diagrama esquemático del circuito
[![Parcial-1-Sistema-de-Procesamiento-de-Datos-MATIAS-CUERVO-1-B.png](https://i.postimg.cc/yWCq2tck/Parcial-1-Sistema-de-Procesamiento-de-Datos-MATIAS-CUERVO-1-B.png)](https://postimg.cc/T50HDkk6)

# Funcionamnmiento de los componentes 
*   Pantalla de 7 segmentos: El display de 7 segmentos se utiliza para mostrar el número del piso actual. Se conectan los pines del Arduino (SEG_A a SEG_G) a los segmentos correspondientes del display para encender los segmentos y formar el número deseado.
* LED rojo (LED_ROJO): Este LED se utiliza como indicador de detención del montacargas. Se enciende cuando el montacargas está detenido.
* LED verde (LED_VERDE): Este LED se utiliza como indicador de movimiento del montacargas. Se enciende cuando el montacargas está en movimiento.
* Botón de subir piso (BOTON_SUBIR): Este botón se utiliza para solicitar al montacargas que suba un piso. Cuando se presiona el botón, se verifica que el montacargas no esté en movimiento y que el piso actual sea menor a 3. Si se cumplen estas condiciones, el montacargas comienza a subir al siguiente piso.
* Botón de bajar piso (BOTON_BAJAR): Este botón se utiliza para solicitar al montacargas que baje un piso. Cuando se presiona el botón, se verifica que el montacargas no esté en movimiento y que el piso actual sea mayor a 0. Si se cumplen estas condiciones, el montacargas comienza a bajar al piso anterior
* Botón de detener (BOTON_DETENER): Este botón se utiliza para detener el montacargas en cualquier momento. Cuando se presiona el botón, se detiene el montacargas y se enciende el LED rojo

# Funcionamiento integral del proyecto  montacargas
El proyecto del  montacargas consiste en un sistema que permite Subir ,Bajar y o Pausar el montacargas entre diferentes pisos utilizando botones de control. A continuación, se explica el funcionamiento paso a paso:
* 1.Configuración inicial: En la sección de configuración del código, se definen las conexiones de los componentes, como los pines para los LED y los botones. Además, se inicializan los estados iniciales de variables necesarias para el funcionamiento .

# Codigo Fuente
https://onlinegdb.com/fVfVxp6xg

# Link Tinkercad
https://www.tinkercad.com/things/5dPLfB4GVLU-parcial-1-sistema-de-procesamiento-de-datosmatias-cuervo-1b/editel?sharecode=ysDfynXlzeeFXNmR6bpO14WQ9hcSfNS7-Ayt-w6Zbe0
