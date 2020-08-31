# Circuito-Proximidad v1
El circuito sensor ultrasonido para proximidad de objetos

Modificacion v1
/*
   
 	El circuito trabajado consiste en presentar y dar a conocer el manejo de un sensor ultrasónico
    y cómo a este se le puede dar la funcionalidad de sensor de proximidad de objetos. 
    Cabe mencionar que este circuito fue modificado
    y versionado para la realización particular de este informe de laboratorio. 
  
   Este codigo tiene como objetivo dar muestra de un uso practico de:
   Sensor ultrasonico ping (parallax)
   Según la hoja de datos de Parallax para el PING))), 
   hay 73,746 microsegundos por pulgada o 29,034 microsegundos por centimetro 
   (es decir, el sonido viaja a 1130 pies (o 34442.4cm) por segundo). 
   Este da la distancia recorrida por el ping, ida y vuelta, 
   por lo que dividimos por 2 para obtener la distancia del obstáculo.
   ver: 
   https://www.parallax.com/sites/default/files/downloads/28015-PING-Sensor-Product-Guide-v2.0.pdf
        [En el PDF: TO_IN = 73_746' Inches ; TO_CM = 29_034' Centimeters ]
   El circuito:
     * +V conectado a sensor PING))) en +5V
     * GND conectado a sensor PING))) en GND (ground)
     * SIG conectado a sensor PING))) en pin digital 7
     * LED conectado a pin 9 (PWM)
   Funcion
   readUltrasonicDistance(int triggerPin, int echoPin): Referencia obtenida de sensor ultrasonico tinkercad.com
*/
int inches = 0;// Se define la variable inches de forma entero, en pulgadas(Inches)
int cm = 0;// Se define la variable cm de forma entero, en centimetros.

/*Se define una función readUltrasonicDistance al inicio 
para poder llamarla en el loop es del tipo long para 
aumentar la presición y tener mayor cantidad de decimsles 
Triger emite frecuencia y eco recibe mismo terminal SIG*/

long readUltrasonicDistance(int triggerPin, int echoPin)
{
  pinMode(triggerPin, OUTPUT);  // Inicializar LOW para limpiar trigger por 2 microsegundos
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  // Inicializar trigger en HIGH por 8 microsegundos para comenzar
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  // lectura de pin echo con el retorno de la señal
  return pulseIn(echoPin, HIGH);
}


/*FUNCIÓN SETUP se llama solo una vez y es la encargada de definir los pines como entradas o salidas */

void setup()
{
  Serial.begin(9600);// comunicación serial 9600 caracteres por segundo
  pinMode(8, OUTPUT); //salida LED azul
  pinMode(7, OUTPUT); //salida LED AMARILLO
  pinMode(4, OUTPUT); //salida LED ROJO
  pinMode(10, OUTPUT); //Salida LED blanco
}

/*FUNCIÓN LOOP se forma de manera iterativa ciclica */
void loop()
{ 
 //FLOAT VARIABLE NÚMERO ENTERO CON 2 DECIMALES 
  float distancia = 0.01723 * readUltrasonicDistance(2, 2);
  /*CONDICIONES PARA DIFERENTES DISTANCIAS*/
  if((distancia < 336) && (distancia >= 200)) //DISTANCIA MENOR A 336 O MATOR A 200 E IGUAL A 200 CM
  {
    digitalWrite(4, HIGH); //ENCIENDE LED ROJO
    digitalWrite(10, LOW); //LED BLANCO APAGADO
    digitalWrite(7, LOW); // LED AMARILLO APAGADO
    digitalWrite(8, LOW); // LED azul APAGADO
  } 
  else {
    digitalWrite(4, LOW); //LED ROJO APAGADO
    digitalWrite(10, LOW); //LED BLANCO APAGADO
  }
  /*CONDICION PARA CIERTA DISTANCIA */
  if((distancia < 200) && (distancia >= 50)) { //SI LA DISTANCIA ES MENOR A 200 O MAYOR E IGUAL A 50
    digitalWrite(7, HIGH);//LED AMARILLO ENCENDIDO
  } 
  else {
    digitalWrite(7, LOW); //LED AMARILLO APAGADO
  }
  /*CONDICIÓN PARA CIERTA DISTANCIA*/
  if(distancia < 50) { //DISTANCIA MENOR A 50
    digitalWrite(8, HIGH);//LED AZUL ENCIENDE A UNA DISTANCIA MENOR A 50 cm
    digitalWrite(10, HIGH); // LED BLANCO ENCENDIDO
  } 
  else {
    digitalWrite(8, LOW); //LED AZUL APAGADO
    digitalWrite(10, LOW); //LED BLANCO APAGADO
  }
  /*SE COMPARA MONITOR SERIE CON VALOR VISUAL DEL SENSOR SUS RANGO*/
  Serial.print("distancia: "); //IMPRIMIR EN MONITOR SERIE
  Serial.println(distancia+1.8); // IMPRIME VALOR DE DISTANCIA 
  delay(10); // Delay a little bit to improve simulation performance
}

