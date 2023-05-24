
#  Proyecto
 
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
* Configuración inicial: En la sección de configuración del código, se definen las conexiones de los componentes, como los pines para los LED y los botones. Además, se inicializan los estados iniciales de variables necesarias para el funcionamiento .
<pre lang="cpp">
#define SEG_A 2
#define SEG_B 3
#define SEG_C 4
#define SEG_D 5
#define SEG_E 6
#define SEG_F 7
#define SEG_G 8
#define LED_ROJO 13
#define LED_VERDE 12
#define BOTON_SUBIR 11
#define BOTON_DETENER 10
#define BOTON_BAJAR 9
int pisoActual = 0;  // 0: Planta baja, 1: Primer piso, 2: Segundo piso
bool enMovimiento = false;
unsigned long tiempoInicio = 0;
unsigned long tiempoTranscurrido = 0;
</pre>
* Definimos el setup de los componentes 
<pre lang="cpp">
void setup()
{
 pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_VERDE, OUTPUT);
  pinMode(SEG_A, OUTPUT);
  pinMode(SEG_B, OUTPUT);
  pinMode(SEG_C, OUTPUT);
  pinMode(SEG_D, OUTPUT);
  pinMode(SEG_E, OUTPUT);
  pinMode(SEG_F, OUTPUT);
  pinMode(SEG_G, OUTPUT);
  pinMode(BOTON_SUBIR, INPUT_PULLUP);
  pinMode(BOTON_DETENER, INPUT_PULLUP);
  pinMode(BOTON_BAJAR, INPUT_PULLUP);
  digitalWrite(LED_ROJO, HIGH);
  Serial.begin(9600);
}
</pre>

* Bucle principal
El Programa Entra En Un Bucle Principal Llamado loop(), Que Se Ejecuta Continuamente Mientras El Arduino Está Encendido.
En Ella Llamamos a Las Funciones Que Utiliza El Proyecto.
<pre lang="cpp">
void loop()
{
  subir_piso();
  bajar_piso();
  detenerMontacargas();
  actualizarDisplay(pisoActual);
  
  if (millis() - tiempoInicio >= tiempoTranscurrido)
  {
    detenerMontacargas();
  }
  
}
</pre>
* Subir piso:En el bucle principal, se llama a la función subir_piso(). Esta función se encarga de verificar si se ha presionado el botón de subir piso y si el montacargas no está en movimiento y el piso actual es menor a 3. Si se cumplen estas condiciones, se realiza lo siguiente:

Se incrementa el número del piso actual en 1.
Se marca el montacargas como en movimiento.
Se guarda el tiempo de inicio del movimiento.
Se establece el tiempo transcurrido para el trayecto entre pisos (3000 ms).
Se enciende el LED verde y se apaga el LED rojo.
Se muestra en el monitor serial el mensaje "Subiendo al piso [piso]".
Se actualiza el último tiempo de lectura.
<pre lang="cpp">
void subir_piso()
{
  static unsigned long ultimoTiempo = 0;
  const unsigned long tiempo_anti_rebote = 50; 

  if (millis() - ultimoTiempo < tiempo_anti_rebote)
  {
    // Ignorar lecturas durante el tiempo de antirrebote
    return;
  }

  if (digitalRead(BOTON_SUBIR) == HIGH && !enMovimiento && pisoActual < 3)
  {
    pisoActual++;
    enMovimiento = true;
    tiempoInicio = millis();
    tiempoTranscurrido = 3000; // Tiempo de trayecto entre pisos (3000 ms)
    digitalWrite(LED_VERDE, HIGH); 
    digitalWrite(LED_ROJO, LOW); 
    Serial.print("Subiendo al piso ");
    Serial.println(pisoActual);

    ultimoTiempo = millis(); 
  }
}
</pre>

* Bajar piso: A continuación, se llama a la función bajar_piso(). Esta función verifica si se ha presionado el botón de bajar piso y si el montacargas no está en movimiento y el piso actual es mayor a 0. Si se cumplen estas condiciones, se realiza lo siguiente:

Se decrementa el número del piso actual en 1.
Se marca el montacargas como en movimiento.
Se guarda el tiempo de inicio del movimiento.
Se establece el tiempo transcurrido para el trayecto entre pisos (3000 ms).
Se enciende el LED verde y se apaga el LED rojo.
Se muestra en el monitor serial el mensaje "Bajando al piso [piso]".
Se actualiza el último tiempo de lectura.
tambien se establese un segundo if donde se cumple la condición if (enMovimiento && millis() - tiempoInicio >= tiempoTranscurrido), se ejecutará el bloque de código, lo cual apagará el LED verde, encenderá el LED rojo y mostrará el mensaje "Montacargas llegó al piso X"
<pre lang="cpp">
 void bajar_piso()
{ 
  static unsigned long ultimoTiempo = 0;
  const unsigned long tiempo_anti_rebote_bajada = 50; // Tiempo de antirrebote en milisegundos

  if (millis() - ultimoTiempo < tiempo_anti_rebote_bajada)
  {
    // Ignorar lecturas durante el tiempo de antirrebote
    return;
  }
  if (digitalRead(BOTON_BAJAR) == HIGH && !enMovimiento && pisoActual > 0)
  {
    pisoActual--;
    enMovimiento = true;
    tiempoInicio = millis();
    tiempoTranscurrido = 3000; // Tiempo de trayecto entre pisos (3000 ms)
    digitalWrite(LED_VERDE, HIGH); // Encender LED verde
    digitalWrite(LED_ROJO, LOW); // Apagar LED rojo
    Serial.print("Bajando al piso ");
    Serial.println(pisoActual);
    ultimoTiempo = millis();
  }
  if (enMovimiento && millis() - tiempoInicio >= tiempoTranscurrido)
{
  enMovimiento = false;
  digitalWrite(LED_VERDE, LOW);  // Apagar LED verde
  digitalWrite(LED_ROJO, HIGH);  // Encender LED rojo
  Serial.print("Montacargas llego al piso ");
  Serial.println(pisoActual);
  }
  
}
  </pre>
* Detener montacargas
Esta función verifica si se ha presionado el botón de detener montacargas. Si se cumple esta condición, se realiza lo siguiente:
Se marca el montacargas como no en movimiento.
Se apaga el LED verde y se enciende el LED rojo.
Se muestra en el monitor serial el mensaje "Montacargas detenido".
Se actualiza el último tiempo de lectura.
Actualizar display: Después de verificar las acciones de subir, bajar o detener el montacargas, se llama a la función actualizarDisplay(pisoActual). Esta función se encarga de mostrar en el display el número correspondiente al piso actual.

Control del tiempo de trayecto: Finalmente, se realiza una verificación para detener el montacargas si ha transcurrido el tiempo de trayecto establecido. Si el tiempo transcurrido desde el inicio del movimiento es mayor o igual al tiempo de trayecto, se ejecuta la función detenerMontacargas() para detener el montacargas.

El programa continúa en este bucle principal, repitiendo los pasos anteriores y verificando constantemente los botones y el tiempo transcurrido para controlar el movimiento del montacargas y mostrar la información adecuada en el display y el monitor serial.
<pre lang="cpp">
void detenerMontacargas()
{
 static unsigned long ultimoTiempo = 0;
  const unsigned long tiempo_anti_rebote_detener = 50; // Tiempo de antirrebote en milisegundos

  if (millis() - ultimoTiempo < tiempo_anti_rebote_detener)
  {
    // Ignorar lecturas durante el tiempo de antirrebote
    return;
  }

  if (digitalRead(BOTON_DETENER) == HIGH)
  {
    enMovimiento = false;
    digitalWrite(LED_VERDE, LOW);  // Apagar LED verde
    digitalWrite(LED_ROJO, HIGH);  // Encender LED rojo
    Serial.println("Montacargas detenido");
    ultimoTiempo = millis();  // Actualizar el último tiempo de lectura
  }
}	
</pre>

* actualizarDisplay:
Por último, se llama a la función actualizarDisplay() para mostrar en la pantalla de 7 segmentos el número del piso actual.
<pre lang="cpp">
void actualizarDisplay(int numero)
{
  // Definir patrones para cada dígito
  const byte patronesDigitos[4] = {
    B00111111, // 0
    B00000110, // 1
    B01011011, // 2
    B01001111  // 3
  };

  digitalWrite(SEG_A, bitRead(patronesDigitos[numero], 0));
  digitalWrite(SEG_B, bitRead(patronesDigitos[numero], 1));
  digitalWrite(SEG_C, bitRead(patronesDigitos[numero], 2));
  digitalWrite(SEG_D, bitRead(patronesDigitos[numero], 3));
  digitalWrite(SEG_E, bitRead(patronesDigitos[numero], 4));
  digitalWrite(SEG_F, bitRead(patronesDigitos[numero], 5));
  digitalWrite(SEG_G, bitRead(patronesDigitos[numero], 6));
}
</pre>
# Codigo Fuente
https://onlinegdb.com/fVfVxp6xg

# Link Tinkercad
https://www.tinkercad.com/things/5dPLfB4GVLU-parcial-1-sistema-de-procesamiento-de-datosmatias-cuervo-1b/editel?sharecode=ysDfynXlzeeFXNmR6bpO14WQ9hcSfNS7-Ayt-w6Zbe0
