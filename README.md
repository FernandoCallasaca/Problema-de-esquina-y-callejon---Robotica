# Problema de Esquina y Callejón

### Universidad Nacinal San Antonio Abad del Cusco (UNSAAC)
### Departamento Académico de Informática
### Robótica y Procesamiento de Señales

- BOLAÑOS CCOPA, RONY				      -   154624
- CALLASACA ACUÑA, FERNANDO        -  140989
- LIMPE QUISPE, JERRY ANDERSON     -  140985
- MENDOZA CJUIRO, NILO FIDEL       -  144987
- SERRANO AMAU, FIORELA ELOISA     -  133968

#### Especificaciones Técnicas

El Proyecto debe contar con un robot móvil de cuatro ruedas que tiene un arduino con un sensor de proximidad.

#### Reglas del Proyecto

El móvil debe ingresar a una ruta que tiene un mínimo de dos esquinas y al finalizar un callejón.  El móvil debe obtener información del sensor de proximidad y el computador debe decidir qué acción debe de hacer para ir a través de la ruta sin chocar llegar hasta el final y regresar por la misma ruta si el móvil toca hace tiene que regresar hasta el inicio sólo tendrán tres opciones para realizar y completar la actividad.
 
## Código　

```javascript
//Inclusiòn de librerias 
//*******************libreria para manejar el sensor ultrasònico hc sr05*/
#include <NewPing.h>
/*libreria para el servomotor*/
#include <Servo.h>
/**definimos los piner del sensor ultrasonico*/
#define TRIG_PIN A4
#define ECHO_PIN A5
#define MAX_DISTANCIA 200
NewPing sonar(TRIG_PIN,ECHO_PIN,MAX_DISTANCIA); 
Servo servo;
//variables
boolean haciaAdelante=false;
int distancia = 100;
int ENA = 5; //velocidad motor A
int IN1 = 9; //dirección motor a borna 1
int IN2 = 10; //dirección motor a borna 2

int ENB = 6; //velocidad motor B
int IN3 = 11; //dirección motor b borna 1
int IN4 = 12; //dirección motor b borna 2

//Establecer el Pin del  Servomotor
const int servoPin = 2;
//Establecemos el angulo Inicial
const int angulo = 90;
//Establecemos la velocidad de giro de los motores
int velocidadGiro = 160;


void setup()
{
    Serial.begin(9600);
   //configuracion de los pines de salida
   pinMode(ENA, OUTPUT);//Motor A
   pinMode(ENB, OUTPUT);//Motor B
   pinMode(IN1, OUTPUT);//direccion motor A
   pinMode(IN2, OUTPUT);//direccion motor A
   pinMode(IN3, OUTPUT);//direccion motor B
   pinMode(IN4, OUTPUT);//direccion motor B
   //calibrarcion de servo perpendicular a la direccion
   servo.attach(servoPin);
   servo.write(angulo);
   avanzar();
} 
int ang = 0;
int sig = 1;
void loop()
{
   int cnt = 6;
   while(cnt--) {
     servo.write(ang);
     delay(80);
     distancia=DistanciaGeneral();
     if(distancia<35 && ang==90)
     {
       parar();
       
       int distanciaDerecha=DistanciaDAngulo(0);
       int distanciaIzquierda=DistanciaDAngulo(180);
       if(distanciaDerecha>distanciaIzquierda)
       {
         giro_Izquierda();
         //girarDerecha();
         delay(400);
       }
       else
       {
         giro_Derecha();
         //girarIzquierda();
         delay(400);
       }
       avanzar();
     }
     if(distancia<18 && ang!=90)
     {
       if(ang<90)
       {
         giro_Derecha();
         //girarIzquierda();
         delay(ang*400.0/90);
       }
       else
       {
         giro_Izquierda();
         //girarDerecha();
         delay((180-ang)*400.0/90);
       }
     }
     avanzar();
     ang += 30*sig;
    }
   sig = -sig;
} 


int DistanciaDAngulo(int angulo)
{
  int dis=0;
  servo.write(angulo);
  delay(500);
  dis=DistanciaGeneral();
  return dis ;
  
}
int DistanciaGeneral()
{
   int cm = sonar.ping_cm();
   if(cm==0)
   {
   cm = 250;
   }
   return cm;
}
void parar()
{
   analogWrite(ENA, 0); //Velo7cidad del motor A 0
   digitalWrite(IN1, LOW); //Desactivar Direccion de motor A
   digitalWrite(IN2, LOW);//Desactivar Direccion de motor A
   
   analogWrite(ENB, 0);//Velocidad del motor B 0
   digitalWrite(IN3, LOW); //Desactivar Direccion de motor B
   digitalWrite(IN4, LOW);//Desactivar Direccion de motor B
   
}
void avanzar()
{
   analogWrite(ENA, 70);
   digitalWrite(IN1, LOW); // Activar Direccion de motor A
   digitalWrite(IN2, HIGH);//Desactivar Direccion de motor A
   analogWrite(ENB, 70);
   digitalWrite(IN3, LOW); // Activar Direccion de motor B
   digitalWrite(IN4, HIGH);//Desactivar Direccion de motor B
   
}
void moverAtras()
{
   digitalWrite(IN1, HIGH); //Desactivar Direccion de motor A
   digitalWrite(IN2, LOW);// Activar Direccion de motor A
   analogWrite(ENA, 70);
   digitalWrite(IN3, HIGH); //Desactivar Direccion de motor B
   digitalWrite(IN4, LOW); // Activar Direccion de motor B
   analogWrite(ENB, 70);
}
void giro_Derecha()
{
   digitalWrite(IN1, LOW);// Activar Direccion de motor A
   digitalWrite(IN2, HIGH); //Desactivar Direccion de motor A
   analogWrite(ENA, velocidadGiro);
   digitalWrite(IN3, HIGH); //Desactivar Direccion de motor B
   digitalWrite(IN4, LOW);// Activar Direccion de motor B
   analogWrite(ENB, velocidadGiro);
} 
void giro_Izquierda()
{
   analogWrite(ENA, velocidadGiro);
   digitalWrite(IN1, HIGH); // Dessactivar Direccion de motor A
   digitalWrite(IN2, LOW); //Activar Direccion de motor A
   analogWrite(ENB, velocidadGiro);
   digitalWrite(IN3, LOW); // Activar Direccion de motor B
   digitalWrite(IN4, HIGH); //Desactivar Direccion de motor B
   
} 

```


## Imágenes del Proyecto

Circuito:

![](https://github.com/FernandoCallasaca/Problema-de-esquina-y-callejon---Robotica/blob/main/Images/circuit.jpeg)

> Construido por nuestra compañera Fiorela.

Carrito:

![](https://github.com/FernandoCallasaca/Problema-de-esquina-y-callejon---Robotica/blob/main/Images/car.jpeg)

> Construido por nuestra compañera Fiorela.

Carrito en nuestro circuito:

![](https://github.com/FernandoCallasaca/Problema-de-esquina-y-callejon---Robotica/blob/main/Images/car_circuit.jpeg)

> Construido por nuestra compañera Fiorela.

Diagrama de Flujo:

![](https://github.com/FernandoCallasaca/Problema-de-esquina-y-callejon---Robotica/blob/main/Images/diagram.jpeg)

> Construido por nuestra compañero Jerry Limpe.

